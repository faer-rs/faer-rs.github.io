---
layout: post
title:  "Getting started"
date:   2022-12-10 00:00:01 +0100
permalink: /getting-started/
---

# Setup

To use `faer` in your project, users can add the required dependency to their
`Cargo.toml` file.

```toml
[dependencies]
faer = "0.11"
```

# Matrix types

`faer` implements three fundamental matrix types:
- `Mat<T>`: an owning matrix type,
- `MatRef<'_, T>`: a reference to a matrix,
- `MatMut<'_, T>`: a mutable reference to a matrix.

These can be thought of as being analogous to `Vec<T>`, `&'_ T`, `&'_ mut T`.

Matrices can be created in several ways:

```rust
use faer::{mat, Mat, prelude::*};

// empty 0x0 matrix
let m0: Mat<f64> = Mat::new();

// zeroed 4x3 matrix
let m1: Mat<f64> = Mat::zeros(4, 3);

// 3x3 identity matrix
let m2 = Mat::from_fn(3, 3, |i, j| if i == j { 1.0 } else { 0.0 });

// 4x2 matrix with custom data
let m3 = mat![
    [4.93, 2.41],
    [5.43, 4.33],
    [9.83, 1.59],
    [7.13, 5.02_f64],
];

// compute the qr decomposition of a matrix
let qr_decomposition = m3.qr();
```

Mathematical operators can be used with matrix types:
- `+` performs elementwise addition,
- `-` performs elementwise negation/subtraction,
- `*` performs the scalar product if one of the operands is a scalar and the
  other is a matrix. Otherwise, it performs matrix multiplication

For more details on creating and manipulating matrix types, users can checkout
the documentation on [docs.rs][docs].

[docs]: https://docs.rs/faer/latest/faer/
