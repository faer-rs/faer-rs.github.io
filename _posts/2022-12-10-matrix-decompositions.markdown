---
layout: post
title:  "Matrix decompositions"
date:   2022-12-10 00:00:02 +0100
permalink: /matrix-decompositions/
---

Matrix decompositions are algorithms that split a matrix into a product of
simpler (e.g., triangular) matrices.
This allows us to more easily solve linear systems, compute the determinant of
the matrix, solve least squares problems, etc.

`faer` provides a variety of matrix decompositions, with more to come in the
near future. Each of these has its own properties that makes it useful for a
certain class of problems.

# Cholesky decompositions

Given a Hermitian matrix $A$ (a matrix whose conjugate transpose is equal to
itself), one can often compute two matrices $L$ an $D$, such that $L$ is lower
triangular with a unit diagonal, $D$ is diagonal, and

$$A = LDL^H.$$

If the matrix is also positive-definite, (i.e., all of its eigenvalues are
positive), then it can be decomposed into

$$A = LL^H,$$

where $L$ is lower triangular.

The Cholesky decomposition has several desirable properties:
- It can be computed in a very efficient manner,
- It can be updated, either by adding a low rank term to $A$, or by
  adding/deleting rows and columns from $A$.

# LU decompositions

The LU decomposition with partial pivoting decomposes a matrix $A$ into
matrices $L$, $U$, and $P$, where $L$ is lower triangular with unit
diagonal, $U$ is upper triangular, and $P$ is a permutation matrix,
such that

$$PA = LU.$$

It is useful for solving general linear systems, though it is only considered
stable for square linear systems.

The LU decomposition with full pivoting is similar, but has an added
permutation term $Q$, such that

$$PAQ^\top = LU.$$

It is more numerically stable than the LU decomposition with partial
pivoting, and can be safely used with any linear system, but takes longer to
compute.

# QR decompositions

The QR decomposition decomposes a matrix $A$ into
matrices $Q$, and $R$ where $Q$ is unitary (its inverse is equal to its
conjugate transpose), and $R$ is an upper trapezoidal matrix

$$A = QR.$$

It is useful for solving least squares problems. An example of how this can be
done is provided in the documentation of `faer-qr`.

The QR decomposition with column pivoting is similar, but has an added
permutation term $P$, such that

$$AP^\top = QR.$$

It is more numerically stable than the QR decomposition, when the matrix is
rank-deficient or almost rank deficient but takes longer to compute.

It has the property that the diagonal elements of $R$ are non-increasing (if
we don't take into account the effects of floating point arithmetic).

# Singular value decomposition

The singular value decomposition decomposes a matrix $A$ into
matrices $U$, $S$, and $V$, where $U$ and $V$ are unitary matrices, and $S$
is zero except on the main diagonal, such that

$$A = U S V^H.$$

The diagonal elements of $S$ are stored in non-increasing order.

It has a variety of applications, such as computing a pseudoinverse of the
matrix $A$, computing the rank, finding a basis for the null space, etc.

# Eigenvalue decomposition

The eigenvalue decomposition decomposes a square diagonalizable matrix $A$ into
matrices $U$ and $S$, where $U$ is an invertible matrix (unitary if $A$ is
hermitian), and $S$ is diagonal (+real-valued if $A$ is hermitian), such that

$$A = U S U^{-1}.$$

# Example

```rust
use faer_core::{Conj, Parallelism};
use reborrow::*;
use dyn_stack::{DynStack, GlobalMemBuffer, StackReq};
use faer_cholesky::ldlt_diagonal as ldl;

// as the matrix `a` is going to be treated as symmetric,
// we don't need to explicitly store the strictly upper triangular part
// as it is going to be implicitly deduced.
let a = mat![
    [1.15, 0.00, 0.00],
    [6.34, 8.10, 0.00],
    [8.71, 5.33, 3.07_f64],
];

// allocate a workspace with the size and alignment needed for the operations
let mut mem = GlobalMemBuffer::new(
    StackReq::any_of([
        ldl::compute::raw_cholesky_in_place_req::<f64>(
            a.nrows(),
            Parallelism::None,
            Default::default(), // use default parameters
        ).unwrap(),
        ldl::update::insert_rows_and_cols_clobber_req::<f64>(
            1, // we're inserting one column
            Parallelism::None,
        ).unwrap(),
    ]),
);
let mut stack = DynStack::new(&mut mem);

let mut ldl_factors = a.clone();
ldl::compute::raw_cholesky_in_place(
    ldl_factors.as_mut(),
    Default::default(), // no regularization
    Parallelism::None,
    stack.rb_mut(),
    Default::default(), // use default parameters
);

// L is now stored in the strictly lower part of `ldl_factors`, with an
// implicit unit diagonal. D is stored on the diagonal of `ldl_factors`
// we can use it to solve linear systems, or compute the inverse of `a`

// we can also update the LD factors, for example by adding columns and rows to
// `a`

// allocate enough space for the new row and column
ldl_factors.resize_with(|i, j| 0.0, 4, 4);

let mut inserted_matrix = mat![
    [1.0],
    [2.0],
    [3.0],
    [4.0],
];

ldl::update::insert_rows_and_cols_clobber(
    ldl_factors.as_mut(),
    2, // insert at index 2
    inserted_matrix.as_mut(),
    Parallelism::None,
    stack.rb_mut(),
);

// we now have the LDLT factorization of the matrix
// [
//     [1.15, 6.34, 1.00, 8.71],
//     [6.34, 8.10, 2.00, 5.33],
//     [1.00, 2.00, 3.00, 4.00]
//     [8.71, 5.33, 4.00, 3.07],
// ]
```
