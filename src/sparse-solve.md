# solving a linear system

just like for dense matrices, _`faer`_ provides several sparse matrix decompositions for solving linear systems \\(Ax = b\\), where \\(A\\) is sparse and \\(b\\) and \\(x\\) are dense.
these typically come in two variants, supernodal and simplicial.
the variant is selected automatically depending on the sparsity structure of the matrix.
and although the lower level api provides a way to tune the selection options, it is currently not fully documented.

## \\(A\\) is triangular
[`faer::sparse::FaerSparseMat::sp_solve_lower_triangular_in_place`](https://docs.rs/faer/latest/faer/sparse/struct.SparseColMat.html#method.sp_solve_lower_triangular_in_place) can be used, or similar methods for when the diagonal is unit and/or the matrix is upper triangular.

## \\(A\\) is real-symmetric/complex-Hermitian and positive definite
If \\(A\\) is Hermitian and positive definite, users can use the [cholesky llt decomposition](https://docs.rs/faer/latest/faer/sparse/struct.SparseColMat.html#method.sp_cholesky).

```rust
use faer::prelude::*;
use faer::sparse::FaerSparseMat;
use faer::Side;

let a = SparseColMat::<usize, f64>::try_new_from_triplets(
    2,
    2,
    &[
        (0, 0, 10.0),
        (1, 0, 2.0),
        (0, 1, 2.0),
        (1, 1, 10.0),
    ],
).unwrap();
let b = mat![[15.0], [-3.0f64]];

let llt = a.sp_cholesky(Side::Lower).unwrap();
let x = llt.solve(&b);
```

## \\(A\\) is square
for a square matrix \\(A\\), we can use the [lu decomposition with partial pivoting](https://docs.rs/faer/latest/faer/sparse/struct.SparseColMat.html#method.sp_lu).

```rust
use faer::prelude::*;
use faer::sparse::FaerSparseMat;

let a = SparseColMat::<usize, f64>::try_new_from_triplets(
    2,
    2,
    &[
        (0, 0, 10.0),
        (1, 0, 2.0),
        (0, 1, 4.0),
        (1, 1, 20.0),
    ],
).unwrap();
let b = mat![[15.0], [-3.0f64]];

let lu = a.sp_lu().unwrap();
let x = lu.solve(&b);
```

## \\(A\\) is a tall matrix (least squares solution)
when the linear system is over-determined, an exact solution may not
necessarily exist, in which case we can get a best-effort result by computing
the least squares solution.
that is, the solution that minimizes \\(||A x - b||\\).

this can be done using the [qr decomposition](https://docs.rs/faer/latest/faer/sparse/struct.SparseColMat.html#method.sp_qr).
```rust
use faer::prelude::*;
use faer::sparse::FaerSparseMat;

let a = SparseColMat::<usize, f64>::try_new_from_triplets(
    3,
    2,
    &[
        (0, 0, 10.0),
        (1, 0, 2.0),
        (2, 0, 3.0),
        (0, 1, 4.0),
        (1, 1, 20.0),
        (2, 1, -45.0),
    ],
).unwrap();
let b = mat![[15.0], [-3.0], [33.0f64]];

let qr = a.sp_qr().unwrap();
let x = qr.solve_lstsq(&b);
```
