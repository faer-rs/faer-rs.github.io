# simd

_`faer`_ provides a common interface for generic and composable simd, using the
_`pulp`_ crate as a backend. _`pulp`_'s high level api abstracts away the differences
between various instruction sets and provides a common api that's generic over
them (but not the scalar type). this allows users to write a generic implementation
that gets turned into several functions, one for each possible instruction set
among a predetermined subset. finally, the generic implementation can be used along
with an `Arch` structure that determines the best implementation at runtime.

Here's an example of how _`pulp`_ could be used to compute the expression \\(x^2 +
2y - |z|\\), and store it into an output vector.

```rust
use core::iter::zip;

fn compute_expr(out: &mut[f64], x: &[f64], y: &[f64], z: &[f64]) {
    struct Impl<'a> {
        out: &'a mut [f64],
        x: &'a [f64],
        y: &'a [f64],
        z: &'a [f64],
    }

    impl pulp::WithSimd for Impl<'_> {
        type Output = ();

        #[inline(always)]
        fn with_simd<S: pulp::Simd>(self, simd: S) {
            let Self { out, x, y, z } = self;

            let (out_head, out_tail) = S::as_mut_simd_f64s(out);
            let (x_head, x_tail) = S::as_simd_f64s(x);
            let (y_head, y_tail) = S::as_simd_f64s(y);
            let (z_head, z_tail) = S::as_simd_f64s(z);

            let two = simd.splat_f64s(2.0);
            for (out, (&x, (&y, &z))) in zip(
                out_head,
                zip(x_head, zip(y_head, z_head)),
            ) {
                *out = simd.add_f64s(
                    x,
                    simd.sub_f64s(simd.mul_f64s(two, y), simd.abs_f64s(z)),
                );
            }

            for (out, (&x, (&y, &z))) in zip(
                out_tail,
                zip(x_tail, zip(y_tail, z_tail)),
            ) {
                *out = x - 2.0 * y - z.abs();
            }
        }
    }

    pulp::Arch::new().dispatch(Impl { out, x, y, z });
}
```

there's a lot of things going on at the same time in this code example. let us
go over them step by step.

_`pulp`_'s generic simd implementation happens through the `WithSimd` trait,
which takes `self` by value to pass in the function parameters. it additionally
provides another parameter to `with_simd` describing the instruction set being
used. `WithSimd::with_simd` *must* be marked with the `#[inline(always)]` attribute.
forgetting to do so could lead to a significant performance drop.

inside the body of the function, we split up each of `out`, `x`, `y` and
`z` into two parts using `S::as_f64s[_mut]_simd`. the first part (`head`) is a
slice of `S::f64s`, representing the vectorizable part of the original slice.
The second part (`tail`) contains the remainder that doesn't fit into a vector
register.

handling the head section is done using vectorized operation. currently these
need to take `simd` as a parameter, in order to guarantee its availability in a
sound way. this is what allows the api to be safe. the tail section is handled
using scalar operations.

the final step is actually calling into our simd implementation. this is done
by creating an instance of `pulp::Arch` that performs the runtime detection
(and caches the result, so that future invocations are as fast as possible),
then calling `Arch::dispatch` which takes a type that implements `WithSimd`,
and chooses the best simd implementation for it.

# memory alignment

instead of splitting the input and output slices into two sections
(vectorizable head + non-vectorizable tail), an alternative approach would be
to split them up into three sections instead (vectorizable head + vectorizable
body + vectorizable tail). this can be accomplished using masked loads and
stores, which can speed things up if the slices are _similarly aligned_.

similarly aligned slices are slices which have the same base address modulo
the byte size of the cpu's vector registers. the simplest way to guarantee this
is to allocate the slices in aligned memory (such that the base address is a
multiple of the register size in bytes), in which case the slices are similarly
aligned, and any subslices of them (with a shared offset and size) will also be
similarly aligned. aligned allocation is done automatically for matrices in _`faer`_,
which helps uphold these guarantees for maximum performance.

_`pulp`_ itself doesn't currently provide safe memory alignment api for now. but provides an unsafe
abstraction that can be used to build. the _`faer`_ implementation is built on top of
[`Simd::mask_between_xxx`], [`Simd::mask_load_ptr_xxx`] and [`Simd::mask_store_ptr_xxx`], as well as the lifetime branded indexing api.

example using _`faer`_ simd:

```rust
use faer::ComplexField;
use faer::ColRef;
use faer::ContiguousFwd;
use faer::utils::simd::SimdCtx;
use faer::utils::bound::Dim;
use pulp::Simd;

#[inline(always)]
pub fn dot_product_f32<'N, S: Simd>(
    simd: S,
    lhs: ColRef<'_, f32, Dim<'N>, ContiguousFwd>,
    rhs: ColRef<'_, f32, Dim<'N>, ContiguousFwd>,
) -> f32 {
    let simd = f32::simd_ctx(simd);
    let N = lhs.nrows();
    let align = size_of::<S::f32s>();

    let simd = SimdCtx::<'N, f32, S>::new_align(
        simd,
        N,
        lhs.as_ptr().align_offset(align),
    );

    let mut acc0 = simd.zero();
    let mut acc1 = simd.zero();
    let mut acc2 = simd.zero();
    let mut acc3 = simd.zero();

    let (head, idx4, idx1, tail) = simd.batch_indices::<4>();

    if let Some(i0) = head {
        let l0 = simd.read(lhs, i0);
        let r0 = simd.read(rhs, i0);

        acc0 = simd.mul_add(l0, r0, acc0);
    }
    for [i0, i1, i2, i3] in idx4 {
        let l0 = simd.read(lhs, i0);
        let l1 = simd.read(lhs, i1);
        let l2 = simd.read(lhs, i2);
        let l3 = simd.read(lhs, i3);

        let r0 = simd.read(rhs, i0);
        let r1 = simd.read(rhs, i1);
        let r2 = simd.read(rhs, i2);
        let r3 = simd.read(rhs, i3);

        acc0 = simd.mul_add(l0, r0, acc0);
        acc1 = simd.mul_add(l1, r1, acc1);
        acc2 = simd.mul_add(l2, r2, acc2);
        acc3 = simd.mul_add(l3, r3, acc3);
    }

    for i0 in idx1 {
        let l0 = simd.read(lhs, i0);
        let r0 = simd.read(rhs, i0);

        acc0 = simd.mul_add(l0, r0, acc0);
    }
    if let Some(i0) = tail {
        let l0 = simd.read(lhs, i0);
        let r0 = simd.read(rhs, i0);

        acc0 = simd.mul_add(l0, r0, acc0);
    }
    acc0 = simd.add(acc0, acc1);
    acc2 = simd.add(acc2, acc3);
    acc0 = simd.add(acc0, acc2);

    simd.reduce_sum(acc0)
}
```

assuming the alignment offset manages to align both `lhs` and `rhs`, this version can be much more efficient than the unaligned one.

however, it has a subtle bug that we will explain in the next section.

# floating point determinism

when performing reductions on floating point values, it's important to keep in mind that these operations
are typically only approximately associative and commutative. so the rounding error arising from the limited
float precision depends on the order in which the operations are performed.

there is however a useful loophole we can exploit, assume we have a register size of 4, and we want to sum up 23 elements, with a batch size of two registers.

```rust
[
    x0, x1, x2, x3,
    x4, x5, x6, x7,
    x8, x9, x10, x11,
    x12, x13, x14, x15,
    x16, x17, x18, x19,
    x20, x21, x22,
]
```

if we use something similar to the previous algorithm, we get the following intermediate results.

in the case where the alignment offset is `0`.
```rust
// after the batch 2 loop
acc0 = f32x4(
    x0 + x8,
    x1 + x9,
    x2 + x10,
    x3 + x11,
);
acc1 = f32x4(
    x4 + x12,
    x5 + x13,
    x6 + x14,
    x7 + x15,
);

// after the batch 1 loop
acc0 = f32x4(
    (x0 + x8) + x16,
    (x1 + x9) + x17,
    (x2 + x10) + x18,
    (x3 + x11) + x19,
);

// after the tail
acc0 = f32x4(
    ((x0 + x8) + x16) + x20,
    ((x1 + x9) + x17) + x21,
    ((x2 + x10) + x18) + x22,
    ((x3 + x11) + x19) + 0.0,
);

// we add up `acc0` and `acc1`
acc = f32x4(
    (((x0 + x8) + x16) + x20) + (x4 + x12),
    (((x1 + x9) + x17) + x21) + (x5 + x13),
    (((x2 + x10) + x18) + x22) + (x6 + x14),
    (((x3 + x11) + x19) + 0.0) + (x7 + x15),
);

// reduce sum: x => (x.0 + x.2) + (x.1 + x.3)
acc = (((((x0 + x8) + x16) + x20) + (x4 + x12)) + ((((x1 + x9) + x17) + x21) + (x5 + x13)))
    + ((((x2 + x10) + x18) + x22) + (x6 + x14)) + ((((x3 + x11) + x19) + 0.0) + (x7 + x15))
```

now let us take the case where the alignment offset is `3` instead. in this case we pad the array by one element on the left before proceeding.

so it's as if we were working with

```rust
[
    0.0, x0, x1, x2,
    x3, x4, x5, x6,
    x7, x8, x9, x10,
    x11, x12, x13, x14,
    x15, x16, x17, x18,
    x19, x20, x21, x22,
]
```

skipping the intermediate computations, we get the final result
```rust
acc = (((((0.0 + x7) + x15) + x19) + (x3 + x11)) + ((((x0 + x8) + x16) + x20) + (x4 + x12)))
    + ((((x1 + x9) + x17) + x21) + (x5 + x13)) + ((((x2 + x10) + x18) + x19) + (x6 + x14))
```

note that this version of the result contains the term `((x2 + x10) + x18) + x19`, which doesn't appear
as a term in the original sum unless we assume exact associativity. (commutativity is assumed to be exact for the operations we care about.)

this results in us getting a slightly different result than before, which could lead to hard-to-reproduce bugs if the user expects consistent behavior
between different runs on the same machine. and the alignment happens to be different due to a difference in the memory allocator or the os behavior.

however, if we sum up our elements like this

```rust
// after the batch 2 loop
acc0 = f32x4(
    x0 + x8,
    x1 + x9,
    x2 + x10,
    x3 + x11,
);
acc1 = f32x4(
    x4 + x12,
    x5 + x13,
    x6 + x14,
    x7 + x15,
);

// after the batch 1 loop
acc0 = f32x4(
    (x0 + x8) + x16,
    (x1 + x9) + x17,
    (x2 + x10) + x18,
    (x3 + x11) + x19,
);

// after the tail
acc1 = f32x4(
    (x4 + x12) + x20,
    (x5 + x13) + x21,
    (x6 + x14) + x22,
    (x7 + x15) + 0.0,
);

// we add up `acc0` and `acc1`
acc = f32x4(
    ((x0 + x8) + x16) + ((x4 + x12) + x20),
    ((x1 + x9) + x17) + ((x5 + x13) + x21),
    ((x2 + x10) + x18) + ((x6 + x14) + x22),
    ((x3 + x11) + x19) + ((x7 + x15) + 0.0),
);

// reduce sum: x => (x.0 + x.2) + (x.1 + x.3)
acc = (((x0 + x8) + x16) + ((x4 + x12) + x20) + ((x1 + x9) + x17) + ((x5 + x13) + x21))
    + (((x2 + x10) + x18) + ((x6 + x14) + x22) + (((x3 + x11) + x19) + ((x7 + x15) + 0.0)))
```

then the offset version outputs:
```rust
acc = (((0.0 + x7) + x15) + ((x3 + x11) + x19) + ((x0 + x8) + x16) + ((x4 + x12) + x20))
    + (((x1 + x9) + x17) + ((x5 + x13) + x21) + (((x2 + x10) + x18) + ((x6 + x14) + x22)))
```

close inspection will reveal that this is exactly equal to the previous sum,
assuming that `x + 0.0 == x` (which is true modulo signed zero), and `x + y == y + x`.

to generalize this strategy, it can be shown that reducing the outputs by rotating the accumulators

```
acc0 -> acc1 -> acc2 -> acc3 -> acc0 -> acc1 -> acc2 -> acc3 -> acc0 -> ...
```

and performing a tree reduction at the end

```rust
acc = (acc0 + acc2) + (acc1 + acc3);

// similarly reduce `acc`
acc = (acc.0 + acc.2) + (acc.1 + acc.3);
```

 will always produce the same results.

_`faer`_ aims to guarantee this behavior, and not doing so is considered a bug.

# universal floating point determinism

the determinism we mentioned above still only guarantees the same results between different runs on the same machine.

if the register size is changed for example, then the logic falls apart. if we want to guarantee that the results
will be the same on any machine, other conditions must be met.

the first one is that the register size must be the same everywhere, this means that we must fix a register size
that's available with one instruction set, and emulate it on ones where it's not available. the other condition
is that the number of threads executing the code must be the same. this is due to the fact that reductions parameters depend on 
the thread count, if we use multithreadede reductions.

currently, _`faer`_ doesn't provide universal floating point determinism as a feature. since simd emulation
comes with overhead, this must be an opt-in feature that we want to provide in a future release. the matrix multiplication
backend used by _`faer`_ must also satisfy the same criteria in that case, which is also still a work in progress.

[`Simd::mask_between_xxx`]: https://docs.rs/pulp/0.19.4/pulp/trait.Simd.html#method.mask_between_m64s
[`Simd::mask_load_ptr_xxx`]: https://docs.rs/pulp/0.19.4/pulp/trait.Simd.html#method.mask_load_ptr_f64s
[`Simd::mask_store_ptr_xxx`]: https://docs.rs/pulp/0.19.4/pulp/trait.Simd.html#method.mask_store_ptr_f64s
