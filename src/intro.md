# Introduction

`faer` is a linear algebra library developed in the Rust programming language.
It exposes a high level abstraction over a lower level API, similar to the one offered by BLAS/Lapack libraries, modified to better match the capabilities of modern computers, and allows tweaking the inner parameters of the various algorithms.

# Community

Most of the discussion around the library happens on the [Discord](https://discord.gg/Ak5jDsAFVZ) server. Users who have questions about using the library, performance issues, bug reports, etc, as well as people looking to contribute, are welcome to join the server!

Bug reports and pull requests can also be posted on the [GitHub](https://www.github.com/sarah-ek/faer-rs) repository.

# Available features

  - Matrix creation and basic operation (addition, subtraction, multiplication, etc.),
  - High performance matrix multiplication,
  - Triangular solvers,
  - Cholesky decomposition, LLT, LDLT, and Bunch-Kaufman LBLT,
  - LU decomposition, with partial or full pivoting,
  - QR decomposition, with or without column pivoting.
  - SVD decomposition.
  - Eigenvalue decomposition for hermitian and non hermitian (real or complex) matrices.
  - Sparse algorithms.

# Future plans

  - n-dimensional tensor structures and operations.
  - GPU acceleration.
