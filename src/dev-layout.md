# matrix layout

_`faer`_ matrices are composed of a pointer to the data, the shape of the matrix, and its strides. its layout can be described as:

```rust
struct MatRef<'a, T> {
  ptr: *const T,
  nrows: usize,
  ncols: usize,
  row_stride: isize,
  col_stride: isize,
  __marker: PhantomData<&'a T>,
}
```

the row and column count must be non-negative, while strides can be arbitrary.

the matrix is column major when `row_stride == 1`, or row major when `col_stride == 1`.

most algorithms in _`faer`_ are currently optimized for column major matrices, with the 
main exceptions being matrix multiplication and triangular matrix solve.

`MatMut` is essentially the same as `MatRef`, except mutable, and has the additional constraint that no two elements within its bounds may alias each other.
so for example, if `ncols > 0` and `nrows >= 2`, then we can't have `row_stride == 0`, as that would imply that `mat[(0, 0)]` and `mat[(1, 0)]` point to the same memory address,
which can create a soundness hole in the general case.

`Mat` is an owned matrix type, similar to `Vec`, and is always column major.

# move semantics

just like `&[T]` and `&mut [T]`, `MatRef<'_, T>` and `MatMut<'a, T>` are respectively `Copy` and `!Copy`.
this is in order to follow rust's sharing xor mutability rule which lets us avoid a entire class of memory safety issues.

there is, however, a big ergonomics gap between `&mut [T]` and `MatMut<'a, T>`. this is due to the fact
that native rust references have built-in language support for reborrowing, which describes the action
of borrowing a parent reference to create a child reference with a shorter lifetime. it can be thought of as a function that takes
`&'short mut &'long mut T`, and returns `&'short mut T`.

while the reborrow is active, the original value can't be used. and once the reborrow is no longer being used, the original borrow becomes accessible again.

the compiler automatically determines whether a reference is reborrowed or moved, depending on the context in which it's used.

matrix views try to emulate this behavior using the `reborrow::Reborrow[Mut]` traits.

built-in reborrowing example:
```rust

fn takes_ref(_: &i32) {}
fn takes_mut(_: &mut i32) {}
fn takes<T>(_: T) {}

let mut i = 0i32;
let ptr = &mut i;

// compiler transforms it to `takes_ref(&*ptr)`
takes_ref(ptr);
// compiler transforms it to `takes_ref(&mut *ptr)`
takes_mut(ptr);
// even though `&mut i32` is not `Copy`,
// we can still use the reference because it was only reborrowed so far
takes_mut(ptr);

// no reborrowing happens here because the corresponding
// `takes` parameter doesn't bind to a reference
takes(ptr);

// doesn't compile. ptr is no longer usable since it was moved
// in the previous call to `takes`.
// takes_mut(ptr);
```

emulated reborrowing example:
```rust
use reborrow::*;

fn takes_ref(_: MatRef<'_, i32>) {}
fn takes_mut(_: MatMut<'_, i32>) {}

let mut ptr = MatMut::<'_, i32>::from_column_major_slice(&mut [], 0, 0);

// immutable reborrow
takes_ref(ptr.rb());
// mutable reborrow
takes_mut(ptr.rb_mut());
// we can still use the reference because it was only reborrowed so far
takes_mut(ptr);

// doesn't compile. ptr is no longer usable since it was moved
// in the previous call to `takes_mut`, since we didn't reborrow
// it manually.
// takes_mut(ptr);
```
