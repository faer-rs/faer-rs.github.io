Several applications require solving a linear system of the form \\(A x = b\\).
The recommended method can vary depending on the properties of \\(A\\), and the
desired numerical accuracy.

## \\(A\\) is triangular
In this case, one can use \\(A\\) and \\(b\\) directly to find \\(x\\), using the functions
provided in [`faer::modules::core::solve`](https://docs.rs/faer-core/latest/faer_core/solve/index.html).

```rust
use faer::{Mat, Parallelism};
use faer::modules::core::solve::solve_lower_triangular_in_place;

let a = Mat::<f64>::from_fn(4, 4, |i, j| if i >= j { 1.0 } else { 0.0 });
let b = Mat::<f64>::from_fn(4, 2, |i, j| (i - j) as f64);

let mut x = Mat::<f64>::zeros(4, 2);
x.copy_from(&b);
solve_lower_triangular_in_place(a.as_ref(), x.as_mut(), Parallelism::None);

// x now contains the approximate solution
```

In the case where \\(A\\) has a unit diagonal, one can use
[`solve_unit_lower_triangular_in_place`](https://docs.rs/faer-core/latest/faer_core/solve/fn.solve_unit_lower_triangular_in_place.html), which avoids reading the diagonal, and
instead implicitly uses the value `1.0` as a replacement.

## \\(A\\) is real-symmetric/complex-Hermitian
If \\(A\\) is Hermitian and positive definite, users can use the Cholesky LLT
decomposition.

```rust
use faer::{mat, Side};
use faer::prelude::*;

let a = mat![
    [10.0, 2.0],
    [2.0, 10.0f64],
];
let b = mat![[15.0], [-3.0f64]];

// Compute the Cholesky decomposition,
// reading only the lower triangular half of the matrix.
let llt = a.cholesky(Side::Lower).unwrap();

let x = llt.solve(&b);
```

### Low level API

Alternatively, a lower-level API could be used to avoid temporary allocations.
The corresponding code for other decompositions follows the same pattern, so we
will skip similar examples for the remainder of this page.

```rust
use faer::{mat, Parallelism, Conj};
use faer::modules::cholesky::llt::compute::cholesky_in_place_req;
use faer::modules::cholesky::llt::compute::{cholesky_in_place, LltRegularization, LltParams};
use faer::modules::cholesky::llt::solve::solve_in_place_req;
use faer::modules::cholesky::llt::solve::solve_in_place_with_conj;
use dyn_stack::{PodStack, GlobalPodBuffer};

let a = mat![
    [10.0, 2.0],
    [2.0, 10.0f64],
];
let mut b = mat![[15.0], [-3.0f64]];

let mut llt = Mat::<f64>::zeros(2, 2);
let no_par = Parallelism::None;

// Compute the size and alignment of the required scratch space
let cholesky_memory = cholesky_in_place_req::<f64>(
    a.nrows(),
    Parallelism::None,
    LltParams::default(),
).unwrap();
let solve_memory = solve_in_place_req::<f64>(
    a.nrows(),
    b.ncols(),
    Parallelism::None,
).unwrap();

// Allocate the scratch space
let mut memory = GlobalPodBuffer::new(cholesky_memory.or(solve_memory));
let mut stack = PodStack::new(&mut mem);

// Compute the decomposition
llt.copy_from(&a);
cholesky_in_place(
    llt.as_mut(),
    LltRegularization::default(), // no regularization
    no_par,
    stack.rb_mut(),               // scratch space
    LltParams::default(),         // default settings
);
// Solve the linear system
solve_in_place_with_conj(llt.as_ref(), Conj::No, b.as_mut(), no_par, stack);
```

If \\(A\\) is not positive definite, the Bunch-Kaufman LBLT decomposition is recommended instead.
```rust
use faer::{mat, Side};
use faer::prelude::*;

let a = mat![
    [10.0, 2.0],
    [2.0, -10.0f64],
];
let b = mat![[15.0], [-3.0f64]];

// Compute the Bunch-Kaufman LBLT decomposition,
// reading only the lower triangular half of the matrix.
let lblt = a.lblt(Side::Lower);

let x = lblt.solve(&b);
```

## \\(A\\) is square
For a square matrix \\(A\\), we can use the LU decomposition with partial pivoting,
or the full pivoting variant which is slower but can be more accurate when the
matrix is nearly singular.

```rust
use faer::mat;
use faer::prelude::*;

let a = mat![
    [10.0, 3.0],
    [2.0, -10.0f64],
];
let b = mat![[15.0], [-3.0f64]];

// Compute the LU decomposition with partial pivoting,
let plu = a.partial_piv_lu();
let x1 = plu.solve(&b);

// or the LU decomposition with full pivoting.
let flu = a.full_piv_lu();
let x2 = flu.solve(&b);
```

## \\(A\\) is a tall matrix (least squares solution)
When the linear system is over-determined, an exact solution may not
necessarily exist, in which case we can get a best-effort result by computing
the least squares solution.
That is, the solution that minimizes \\(||A x - b||\\).

This can be done using the QR decomposition.

```rust
use faer::mat;
use faer::prelude::*;

let a = mat![
    [10.0, 3.0],
    [2.0, -10.0],
    [3.0, -45.0f64],
];
let b = mat![[15.0], [-3.0], [13.1f64]];

// Compute the QR decomposition.
let qr = a.qr();
let x = qr.solve_lstsq(&b);
```

## Computing the singular value decomposition
```rust
use faer::mat;
use faer::prelude::*;

let a = mat![
    [10.0, 3.0],
    [2.0, -10.0],
    [3.0, -45.0f64],
];

// Compute the SVD decomposition.
let svd = a.svd();
// Compute the thin SVD decomposition.
let svd = a.thin_svd();
// Compute the singular values.
let svd = a.singular_values();
```

## Computing the eigenvalue decomposition
```rust
use faer::mat;
use faer::prelude::*;
use faer::complex_native::c64;

let a = mat![
    [10.0, 3.0],
    [2.0, -10.0f64],
];

// Compute the eigendecomposition.
let evd = a.eigendecomposition::<c64>();

// Compute the eigenvalues.
let evd = a.eigenvalues::<c64>();

// Compute the eigendecomposition assuming `a` is Hermitian.
let evd = a.selfadjoint_eigendecomposition();

// Compute the eigenvalues assuming `a` is Hermitian.
let evd = a.selfadjoint_eigenvalues();
```
