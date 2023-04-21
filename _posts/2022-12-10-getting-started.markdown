---
layout: post
title:  "Getting started"
date:   2022-12-10 00:00:01 +0100
permalink: /getting-started/
---

# Setup

To use `faer` in your project, users can add the required dependencies to their
`Cargo.toml` file.

`faer` is split into multiple crates. Once the high level API is implemented,
it will be possible to get all the features by depending on a single crate,
`faer`.

```toml
[dependencies]
faer-core     = "0.8"
faer-lu       = "0.8"
faer-qr       = "0.8"
faer-cholesky = "0.8"
faer-svd      = "0.8"
```

# Matrix types

`faer-core` implements three fundamental matrix types:
- `Mat<T>`: an owning matrix type,
- `MatRef<'_, T>`: a reference to a matrix,
- `MatMut<'_, T>`: a mutable reference to a matrix.

These can be thought of as being analogous to `Vec<T>`, `&'_ T`, `&'_ mut T`.

Matrices can be created in several ways:

```rust
use faer_core::{mat, Mat};

// empty 0x0 matrix
let m0: Mat<f64> = Mat::new();

// zeroed 4x3 matrix
let m1: Mat<f64> = Mat::zeros(4, 3);

// 3x3 identity matrix
let m2 = Mat::with_dims(3, 3, |i, j| if i == j { 1.0 } else { 0.0 });

// 4x2 matrix with custom data
let m3 = mat![
    [4.93, 2.41],
    [5.43, 4.33],
    [9.83, 1.59],
    [7.13, 5.02_f64],
];
```

Mathematical operators can be used with matrix types:
- `+` performs elementwise addition,
- `-` performs elementwise subtraction,
- `*` performs the scalar product if one of the operands is a scalar and the
  other is a matrix. Otherwise, it performs matrix multiplication

For more details on creating and manipulating matrix types, users can checkout
the documentation on [docs.rs][docs].

[docs]: https://docs.rs/faer-core/latest/faer_core/
