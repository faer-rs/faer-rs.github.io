# lifetime branding

the typical way rust code deals with avoiding runtime bound checks is by using iterators.

this works less well in the case of linear algebra code because the 2d structure sometimes gives us
extra information to work with. and in the case of sparse algorithms, we have to deal with unstructured indices that we
don't necessarily want to repeatedly check.

to work around these limitations, faer primarily uses a technique called lifetime branding.
it revolves around defining unique types distinguished by their lifetimes.

types that differ only in their lifetimes are merged before code generation, so they help
keep the binary size small and allow some unique patterns.

the main example is the `Dim<'N>` type. it represents a matrix dimension that's fixed at runtime.
the invariant it carries is that for a given lifetime `'N`, two instances of this type
always have the same value. you can think of it as being similar to const generics (e.g. `Dim<const N: usize>`),
with the difference being that the size is fixed at runtime instead of comptime.
since `Dim<'N>` is defined to be invariant in the lifetime `'N`, this guarantees that
the borrow checker will never coerce an instance of `Dim<'a>` to `Dim<'b>`, unless `'a` and `'b`
match exactly.

instances of `Dim` along with their lifetimes can be created with the `with_dim!` macro.

other types are built around `Dim<'N>`'s invariant. for example `Idx<'N>` guarantees
that instances of it will always hold a valid index for the corresponding `Dim<'N>`,
which in turn implies that bound checks can be soundly elided.

`MaybeIdx<'N>` carries either a valid index or a sentinel value, and `IdxInc<'N>` carries
an inclusive index, i.e. `idx <= n`, and is often used for slicing matrices rather than indexing directly.

```rust
with_dim!(N, 4);

let i = N.idx(3); // checks the index at creation

// ...

// compiles to a no-op, since the check is enforced at compile time thanks to the lifetime brand.
assert!(i < N);
```

# experimental: covariant/contravariant lifetime brands
_`faer`_ also has a mostly internal api that makes use of lifetime coercion to unlock new patterns.

the main idea is to generate lifetimes ordered by the `outlives` relationship that carry new invariants with them.

the main example is `Segment<'scope, 'dim, 'range>`, which is invariant over all three lifetimes and similarly enforces that instances of the same type hold the same value.
it also guarantees that for a fixed `'scope`, instances of the type hold a valid range that can be used to slice dimensions of `Dim<'n>`.

on top of that, given two lifetimes `'long_range` and `'short_range` such that `'long_range: 'short_range` (long outlives short),
`Segment` guarantees that instances of `Segment<'scope, 'dim, 'short_range>` carry a segment that's included in that of instances of `Segment<'scope, 'dim, 'long_range>`.

this fact is used by `SegmentIdx<'scope, 'dim, 'range>` and `SegmentIdxInc<'scope, 'dim, 'range>`, which are contravariant over the lifetime `'range`, and invariant over  the others.
this allows instances of `SegmentIdx<'scope, 'dim, 'short_range>` to be coerced to instances of `SegmentIdx<'scope, 'dim, 'long_range>`.

instances of `Segment` along with their lifetimes can be created with the `ghost_tree!` macro, which is more complex than `Dim`, since it needs to express the subset relationships between the different lifetimes.

