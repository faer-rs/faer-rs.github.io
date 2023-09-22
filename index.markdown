---
title: faer
layout: home
---

# About faer

`faer` is a linear algebra library developed in the Rust programming
language. It exposes a high level abstraction over a lower level API, similar
to the one offered by `BLAS/Lapack` libraries, modified to better match the
capabilities of modern computers, and allows tweaking the inner parameters of
the various algorithms.

A high level interface is planned in the future, once the basic building blocks
are ready and a suitable API design is stabilized.

# Community

Most of the discussion around the library happens on the [Discord][discord] server.
Users who have questions about using the library, performance issues, bug reports, etc,
as well as people looking to contribute, are welcome to join the server!

Bug reports and pull requests can also be posted on the [GitHub][github] repository.

# Available features

- Matrix creation and basic operation (addition, subtraction, multiplication, etc.),
- High performance matrix multiplication,
- Triangular solvers,
- Cholesky decomposition, LLT and LDLT,
- LU decomposition, with partial or full pivoting,
- QR decomposition, with or without column pivoting.
- SVD decomposition.
- Eigenvalue decomposition for hermitian and non hermitian (real or complex)
  matrices.

# Future plans

Soon:
- Bunch-Kaufman LDLT decomposition,

Later:
- Sparse algorithms.

[discord]: https://discord.gg/Ak5jDsAFVZ
[github]: https://github.com/sarah-ek/faer-rs
