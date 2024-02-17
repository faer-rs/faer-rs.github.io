# Matrix management

## Creating a matrix

`faer` provides several ways to create dense matrices and matrix views.

The main matrix types are [`Mat`], [`MatRef`] and [`MatMut`],
which can be thought of as being analogous to `Vec`, `&[_]` and `&mut [_]`.
- [`Mat`] owns its data, provides read-write access to it, and can be resized after creation.
- [`MatRef`] provides read-only access to the underlying data.
- [`MatMut`] provides read-write access to the underlying data.

The most flexible way to initialize a matrix is to initialize a zero matrix,
then fill out the values by hand.

```rust
use faer::Mat;

let mut a = Mat::<f64>::zeros(4, 3);

for j in 0..a.ncols() {
    for i in 0..a.nrows() {
        a[(i, j)] = 9.0;
    }
}
```

Given a callable object that outputs the matrix elements, [`Mat::from_fn`], can also be used.

```rust
use faer::Mat;

let a = Mat::from_fn(3, 4, |i, j| (i + j) as f64);
```

For common matrices such as the zero matrix and the identity matrix, shorthands
are provided.

```rust
use faer::Mat;

// creates a 10×4 matrix whose values are all `0.0`.
let a = Mat::<f64>::zeros(10, 4);

// creates a 5×4 matrix containing `0.0` except on the main diagonal,
// which contains `1.0` instead.
let a = Mat::<f64>::identity(5, 4);
```

In some cases, users may wish to avoid the cost of initializing the matrix to zero,
in which case, unsafe code may be used to allocate an uninitialized matrix, which
can then be filled out before it's used.
```rust
// `a` is initially a 0×0 matrix.
let mut a = Mat::<f64>::with_capacity(4, 3);

// `a` is now a 4×3 matrix, whose values are uninitialized.
unsafe { a.set_dims(4, 3) };

for j in 0..a.ncols() {
    for i in 0..a.nrows() {
        // we cannot write `a[(i, j)] = 9.0`, as that would
        // create a reference to uninitialized data,
        // which is currently disallowed by Rust.
        a.write(i, j, 9.0);

        // we can also skip the bound checks using
        // read_unchecked and write_unchecked
        unsafe { a.write_unchecked(i, j, 9.0) };
    }
}
```

## Creating a matrix view
In some situations, it may be desirable to create a matrix view over existing
data.
In that case, we can use [`MatRef`] (or [`MatMut`] for
mutable views).

They can be created in a safe way using:

- [`mat::from_column_major_slice`],
- [`mat::from_row_major_slice`],
- [`mat::from_column_major_slice_mut`],
- [`mat::from_row_major_slice_mut`],

for contiguous matrix storage, or:

- [`mat::from_column_major_slice_with_stride`],
- [`mat::from_row_major_slice_with_stride`],
- [`mat::from_column_major_slice_with_stride_mut`],
- [`mat::from_row_major_slice_with_stride_mut`],

for strided matrix storage.

An unsafe lower level pointer API is also provided for handling uninitialized data
or arbitrary strides using [`mat::from_raw_parts`] and [`mat::from_raw_parts_mut`].

## Converting to a view

A [`Mat`] instance `m` can be converted to [`MatRef`] or [`MatMut`] by writing [`m.as_ref()`](https://docs.rs/faer/latest/faer/type.Mat.html#method.as_ref)
or [`m.as_mut()`](https://docs.rs/faer/latest/faer/type.Mat.html#method.as_mut).

## Reborrowing a mutable view

Immutable matrix views can be freely copied around, since they are non-owning
wrappers around a pointer and the matrix dimensions/strides.

Mutable matrices however are limited by Rust's borrow checker. Copying them
would be unsound since only a single active mutable view is allowed at a time.

This means the following code does not compile.

```rust
use faer::{Mat, MatMut};

fn takes_view_mut(m: MatMut<f64>) {}

let mut a = Mat::<f64>::new();
let view = a.as_mut();

takes_view_mut(view);

// This would have failed to compile since `MatMut` is never `Copy`
// takes_view_mut(view);
```

The alternative is to temporarily give up ownership over the data, by creating
a view with a shorter lifetime, then recovering the ownership when the view is
no longer being used.

This is also called reborrowing.

```rust
use faer::{Mat, MatMut, MatRef};
use reborrow::*;

fn takes_view(m: MatRef<f64>) {}
fn takes_view_mut(m: MatMut<f64>) {}

let mut a = Mat::<f64>::new();
let mut view = a.as_mut();

takes_view_mut(view.rb_mut());
takes_view_mut(view.rb_mut());
takes_view(view.rb()); // We can also reborrow immutably

{
    let short_view = view.rb_mut();

    // This would have failed to compile since we can't use the original view
    // while the reborrowed view is still being actively used
    // takes_view_mut(view);

    takes_view_mut(short_view);
}

// We can once again use the original view
takes_view_mut(view.rb_mut());

// Or consume it to convert it to an immutable view
takes_view(view.into_const());
```

## Splitting a matrix view, slicing a submatrix
A matrix view can be split up along its row axis, column axis or both.
This is done using [`MatRef::split_at_row`], [`MatRef::split_at_col`] or
[`MatRef::split_at`] (or [`MatMut::split_at_row_mut`], [`MatMut::split_at_col_mut`] or
[`MatMut::split_at_mut`]).

These functions take the middle index at which the split is performed, and return
the two sides (in top/bottom or left/right order) or the four corners (top
left, top right, bottom left, bottom right)

We can also take a submatrix using [`MatRef::subrows`], [`MatRef::subcols`] or
[`MatRef::submatrix`] (or [`MatMut::subrows_mut`], [`MatMut::subcols_mut`] or
[`MatMut::submatrix_mut`]).

Alternatively, we can also use [`MatRef::get`] or [`MatMut::get_mut`], which take
as parameters the row and column ranges.

### ⚠️Warning⚠️
Note that [`MatRef::submatrix`] (and [`MatRef::subrows`], [`MatRef::subcols`]) takes
as a parameter, the first row and column of the submatrix, then the number
of rows and columns of the submatrix.

On the other hand, [`MatRef::get`] takes a range from the first row and column
to the last row and column.

[`Mat`]: https://docs.rs/faer/latest/faer/type.Mat.html
[`MatRef`]: https://docs.rs/faer/latest/faer/type.MatRef.html
[`MatMut`]: https://docs.rs/faer/latest/faer/type.MatMut.html

[`Mat::from_fn`]: https://docs.rs/faer/latest/faer/type.Mat.html#method.from_fn

[`mat::from_column_major_slice`]: https://docs.rs/faer/latest/faer/mat/fn.from_column_major_slice.html
[`mat::from_row_major_slice`]: https://docs.rs/faer/latest/faer/mat/fn.from_row_major_slice.html
[`mat::from_column_major_slice_mut`]: https://docs.rs/faer/latest/faer/mat/fn.from_column_major_slice_mut.html
[`mat::from_row_major_slice_mut`]: https://docs.rs/faer/latest/faer/mat/fn.from_row_major_slice_mut.html

[`mat::from_column_major_slice_with_stride`]: https://docs.rs/faer/latest/faer/mat/fn.from_column_major_slice_with_stride.html
[`mat::from_row_major_slice_with_stride`]: https://docs.rs/faer/latest/faer/mat/fn.from_row_major_slice_with_stride.html
[`mat::from_column_major_slice_with_stride_mut`]: https://docs.rs/faer/latest/faer/mat/fn.from_column_major_slice_with_stride_mut.html
[`mat::from_row_major_slice_with_stride_mut`]: https://docs.rs/faer/latest/faer/mat/fn.from_row_major_slice_with_stride_mut.html

[`MatRef::split_at`]: https://docs.rs/faer/latest/faer/type.MatRef.html#method.split_at
[`MatMut::split_at_mut`]: https://docs.rs/faer/latest/faer/type.MatMut.html#method.split_at_mut
[`MatRef::split_at_col`]: https://docs.rs/faer/latest/faer/type.MatRef.html#method.split_at_col
[`MatMut::split_at_col_mut`]: https://docs.rs/faer/latest/faer/type.MatMut.html#method.split_at_col_mut
[`MatRef::split_at_row`]: https://docs.rs/faer/latest/faer/type.MatRef.html#method.split_at_row
[`MatMut::split_at_row_mut`]: https://docs.rs/faer/latest/faer/type.MatMut.html#method.split_at_row_mut

[`MatRef::submatrix`]: https://docs.rs/faer/latest/faer/type.MatRef.html#method.submatrix
[`MatMut::submatrix_mut`]: https://docs.rs/faer/latest/faer/type.MatMut.html#method.submatrix_mut
[`MatRef::subrows`]: https://docs.rs/faer/latest/faer/type.MatRef.html#method.subrows
[`MatMut::subrows_mut`]: https://docs.rs/faer/latest/faer/type.MatMut.html#method.subrows_mut
[`MatRef::subcols`]: https://docs.rs/faer/latest/faer/type.MatRef.html#method.subcols
[`MatMut::subcols_mut`]: https://docs.rs/faer/latest/faer/type.MatMut.html#method.subcols_mut
