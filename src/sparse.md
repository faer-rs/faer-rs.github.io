# Sparse Linear Algebra
A sparse matrix is a matrix for which the majority of entries are zeros.
By skipping those entries and only processing the non-zero elements, along with their positions in the matrix, we can obtain a reduction in memory usage and amount of computations.

Since the sparse data structures and algorithms can often deal with very large matrices and use an unpredictable amount of memory, `faer` tries to be more robust with respect to handling out of memory conditions.
Due to this, most of the sparse `faer` routines that require memory allocations will return a result that could indicate whether the matrix was too large to be stored.

Most of the errors can be propagated to the caller and handled at the top level, or handled at some point by switching to a method that requires less memory usage.

## Creating a sparse matrix
Just like how dense matrices can be column-major or row-major, sparse matrices also come in two layouts.
The first one is \[Compressed\] Sparse Column layout (CSC) and the second is \[Compressed\] Sparse Row layout (CSR).
Most of the `faer` sparse algorithms operate on CSC matrices, with CSR matrices requiring a preliminary conversion step.

The CSC (resp. CSR) layout of a matrix \\(A\\) is composed of three components:
- the column pointers (resp. row pointers),
- (Optional) the number of non-zero elements per column (resp. row), in which case we say the matrix is in an uncompressed mode,
- the row indices (resp. column indices),
- the numerical values

The column pointers are in an array of size `A.ncols() + 1`.  
The row indices are in an array of size `col_ptr[A.ncols()]`, such that when the matrix is compressed, `row_indices[col_ptr[j]..col_ptr[j + 1]]` contains the indices of the non-zero rows in column `j`.
When the matrix is uncompressed, the row indices are instead contained in `row_indices[col_ptr[j]..col_ptr[j] + nnz_per_col[j]]`.  
The numerical values are in an array of the same size as the row indices, and indicate the numerical value stored at the corresponding index.

Let us take the following matrix as an example:
```notcode
[[0.0, 2.0, 4.0, 7.0]
 [1.0, 0.0, 5.0, 0.0]
 [0.0, 3.0, 6.0, 0.0]]
```

In CSC layout, the matrix would be stored as follows:
```notcode
column pointers = [0, 1,    3,       6, 7]
row indices     = [1, 0, 2, 0, 1, 2, 0]
values          = [1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0]
```

In CSR layout, it would be stored as follows:
```notcode
row pointers    = [0,       3,    5,    7]
column indices  = [1, 2, 3, 0, 2, 1, 2]
values          = [2.0, 4.0, 7.0, 1.0, 5.0, 3.0, 6.0]
```

CSC matrices require the row indices in each column to be sorted and contain no duplicates, and matrices created by `faer` will respect that unless otherwise specified.
The same is true for CSR matrices.  
Some algorithms specifically don't strictly require sorted indices, such as the matrix decomposition algorithms. In which case `faer` provides an escape hatch for users that wish to avoid the overhead of sorting their input data.

The simplest way to create sparse matrices is to use the [`SparseColMat::try_new_from_triplets`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseColMat.html#method.try_new_from_triplets) (or [`SparseRowMat::try_new_from_triplets`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseRowMat.html#method.try_new_from_triplets)) constructor, which takes as input a potentially unsorted list of tuples containing the row index, column index and numerical value of each entry.
Entries with the same row and column indices are added together. [`SparseColMat::try_new_from_nonnegative_triplets`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseColMat.html#method.try_new_from_nonnegative_triplets) can also be used, which skips entries containins a negative row or column index.

For problems where the sparsity pattern doesn't change as often as the numerical values, the symbolic and numeric parts can be decoupled using [`SymbolicSparseColMat::try_new_from_indices`](https://docs.rs/faer-core/latest/faer_core/sparse/struct.SymbolicSparseColMat.html#method.try_new_from_indices) (or [`SymbolicSparseColMat::try_new_from_nonnegative_indices`](https://docs.rs/faer-core/latest/faer_core/sparse/struct.SymbolicSparseColMat.html#method.try_new_from_nonnegative_indices)) along with [`SparseColMat::new_from_order_and_values`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseColMat.html#method.new_from_order_and_values).

```rust
use faer::sparse::*;
use faer::modules::core::sparse::CreationError;

let a = SparseColMat::<usize, f64>::try_new_from_triplets(
    3,
    4,
    &[
        (1, 0, 1.0),
        (0, 1, 2.0),
        (2, 1, 3.0),
        (0, 2, 4.0),
        (1, 2, 5.0),
        (2, 2, 6.0),
        (0, 3, 7.0),
    ],
);

let a = match a {
    Ok(a) => a,
    Err(err) => match err {
        CreationError::Generic(err) => return Err(err),
        OutOfBounds { row, col } => panic!("some of the provided indices exceed the size of the matrix."),
    },
};
```

In the case where users want to create their matrix directly from the components, [`SymbolicSparseColMat::new_checked`](https://docs.rs/faer-core/latest/faer_core/sparse/struct.SymbolicSparseColMat.html#method.new_checked), [`SymbolicSparseColMat::new_unsorted_checked`](https://docs.rs/faer-core/latest/faer_core/sparse/struct.SymbolicSparseColMat.html#method.new_unsorted_checked) or [`SymbolicSparseColMat::new_unchecked`](https://docs.rs/faer-core/latest/faer_core/sparse/struct.SymbolicSparseColMat.html#method.new_unchecked) can be used to respectively create a matrix after checking that it satisfies the validity requirements, creating a matrix after skipping the sorted indices requirement, or skipping all checks altogether.

Just like dense matrices, sparse matrices also come in three variants. An owning kind ([`SparseColMat`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseColMat.html)), a reference kind ([`SparseColMatRef`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseColMatRef.html)) and a mutable reference kind ([`SparseColMatMut`](https://docs.rs/faer-core/latest/faer_core/sparse/type.SparseColMatMut.html)).

Note that mutable sparse matrix views do not allow modifying the sparsity structure. Only the numerical values are modifiable through mutable views.
