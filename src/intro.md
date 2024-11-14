# introduction

_`faer`_ is a linear algebra library developed in the rust programming language.
it exposes a high level abstraction over a lower level api, similar to the one offered by blas/lapack libraries, modified to better match the capabilities of modern computers, and allows tweaking the inner parameters of the various algorithms.

# community

most of the discussion around the library happens on the [discord](https://discord.gg/Ak5jDsAFVZ) server. users who have questions about using the library, performance issues, bug reports, etc, as well as people looking to contribute, are welcome to join the server!

bug reports and pull requests can also be posted on the [github](https://www.github.com/sarah-ek/faer-rs) repository.

# available features

  - matrix creation and basic operation (addition, subtraction, multiplication, etc.),
  - high performance matrix multiplication,
  - triangular solvers,
  - cholesky decomposition, llt, ldlt, and bunch-kaufman lblt,
  - lu decomposition, with partial or full pivoting,
  - qr decomposition, with or without column pivoting.
  - svd decomposition.
  - eigenvalue decomposition for hermitian and non hermitian (real or complex) matrices.
  - sparse algorithms.

# future plans

  - n-dimensional tensor structures and operations.
  - gpu acceleration.
