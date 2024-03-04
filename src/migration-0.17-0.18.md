# Migration from v0.17 to v0.18
`faer v0.18` comes with a refactor of the code base into a more scalable form and to better improve the user experience with the [docs.rs](https://docs.rs/faer/latest/faer/) documentation.
The changes affect mostly the low level APIs. These include:
- Merging `faer-core`, `faer-cholesky`, `faer-lu`, `faer-qr`, `faer-svd`, `faer-evd`, `faer-sparse` into `faer`.
Those crates are now deprecated and will no longer be updated, so switching to depending on `faer` directly is recommended.
- Low level matrix multiplication moved from `faer_core::mul` to `faer::linalg::matmul`.
- Low level triangular matrix solve moved from `faer_core::solve` to `faer::linalg::triangular_solve`.
- Low level triangular matrix inverse moved from `faer_core::inverse` to `faer::linalg::triangular_inverse`.
- Low level Cholesky implementation moved from `faer_cholesky` to `faer::linalg::cholesky`.
- Sparse API moved from `faer_core::sparse` to `faer::sparse`.
- Low level LU implementation moved from `faer_lu` to `faer::linalg::lu`.
- Low level QR implementation moved from `faer_qr` to `faer::linalg::qr`.
- Low level SVD implementation moved from `faer_svd` to `faer::linalg::svd`.
- Low level EVD implementation moved from `faer_evd` to `faer::linalg::evd`.
- Low level sparse decompositions moved from `faer_sparse` to `faer::sparse::linalg`.
- High level dense solvers moved from `faer::solvers` to `faer::linalg::solvers`.
- High level sparse solvers moved from `faer::sparse::solvers` to `faer::sparse::linalg::solvers`.

For an easier transition, deprecated aliases were placed in `faer::modules::{core,cholesky,lu,qr,svd,evd,sparse}` that emulate the corresponding old versions of the low level crates to some extent, but they will be removed in a future version.
