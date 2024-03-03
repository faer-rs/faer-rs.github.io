# Matrix arithmetic operations
`faer` matrices implement most of the arithmetic operators, so two matrices
can be added simply by writing `&a + &b`, the result of the expression is a
`faer::SparseColMat` or `faer::SparseRowMat`, which allows simple chaining of operations (e.g. `(&a - &b) * &c`), although
at the cost of allocating temporary matrices.

For more complicated use cases, users are encouraged to preallocate the storage for the temporaries with the corresponding sparsity structure and use [`faer::sparse::ops::binary_op_assign_into`](https://docs.rs/faer/latest/faer/sparse/ops/fn.binary_op_assign_into.html) or [`faer::modules::core::ternary_op_assign_into`](https://docs.rs/faer-core/latest/faer_core/sparse/ops/fn.ternary_op_assign_into.html).
