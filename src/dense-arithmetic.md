# Matrix arithmetic operations
`faer` matrices implement most of the arithmetic operators, so two matrices
can be added simply by writing `&a + &b`, the result of the expression is a
`faer::Mat`, which allows simple chaining of operations (e.g. `(&a + &b) * &c`), although
at the cost of allocating temporary matrices.

Temporary allocations can be avoided by using the [`zipped!`](https://docs.rs/faer/latest/faer/macro.zipped.html) api:
```rust
use faer::{Mat, zipped, unzipped};

let a = Mat::<f64>::zeros(4, 3);
let b = Mat::<f64>::zeros(4, 3);
let mut c = Mat::<f64>::zeros(4, 3);

// Sums `a` and `b` and stores the result in `c`.
zipped!(&mut c, &a, &b).for_each(|unzipped!(c, a, b)| *c = *a + *b);

// Sums `a`, `b` and `c` into a new matrix `d`.
let d = zipped!(&mut c, &a, &b).map(|unzipped!(c, a, b)| *a + *b + *c);
```
For matrix multiplication, the non-allocating api is provided in the
[`faer::modules::core::mul`](https://docs.rs/faer-core/latest/faer_core/mul/index.html) module.

```rust
use faer::{Mat, Parallelism};
use faer::modules::core::mul::matmul;

let a = Mat::<f64>::zeros(4, 3);
let b = Mat::<f64>::zeros(3, 5);

let mut c = Mat::<f64>::zeros(4, 5);

// Computes `faer::scale(3.0) * &a * &b` and stores the result in `c`.
matmul(c.as_mut(), a.as_ref(), b.as_ref(), None, 3.0, Parallelism::None);

// Computes `faer::scale(3.0) * &a * &b + 5.0 * &c` and stores the result in `c`.
matmul(c.as_mut(), a.as_ref(), b.as_ref(), Some(5.0), 3.0, Parallelism::None);
```
