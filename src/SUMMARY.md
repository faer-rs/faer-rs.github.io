# Summary

[introduction](./intro.md)
[getting started](./getting-started.md)
[benchmarks (single-threaded)](./bench-st.md)
[benchmarks (multi-threaded)](./bench-mt.md)

---
# user guide
- [dense linear algebra](./dense.md)
  - [matrix arithmetic](./dense-arithmetic.md)
  - [solving a linear system](./solve.md)
  - [interoperabilitiy with `nalgebra`/`ndarray`](./dense-cvt.md)
- [sparse linear algebra](./sparse.md)
  - [matrix arithmetic](./sparse-arithmetic.md)
  - [solving a linear system](./sparse-solve.md)
- [matrix-free solvers]()
---
# dev guide
- [matrix types](./dev-layout.md)
- [lifetime-branded shapes](./dev-branding.md)
- [simd](./dev-simd.md)
- [dense algorithms]()
  - [matmul]()
  - [cholesky]()
  - [qr]()
  - [lu]()
  - [svd]()
  - [evd]()
    - [self-adjoint evd]()
    - [general evd]()
- [sparse algorithms]()
  - [matmul]()
  - [cholesky]()
  - [qr]()
  - [lu]()
