conversions from/to external library types is provided separately from _`faer`_ itself, in the _`faer-ext`_ crate.

# note
only matrix view types can be converted. owning matrices can't be converted due to _`faer`_ using a different allocation scheme from __`nalgebra`__ and _`ndarray`_.

# converting to/from _`nalgebra`_ matrices
conversion from _`nalgebra`_ types is done by enabling the `nalgebra` feature and using the [`IntoFaer`](https://docs.rs/faer/latest/faer/trait.IntoFaer.html) and [`IntoFaerComplex`](https://docs.rs/faer-ext/latest/faer_ext/trait.IntoFaerComplex.html) traits.  
conversion to _`nalgebra`_ types is enabled by the same feature and uses the [`IntoNalgebra`](https://docs.rs/faer/latest/faer/trait.IntoNalgebra.html) and [`IntoNalgebraComplex`](https://docs.rs/faer-ext/latest/faer_ext/trait.IntoNalgebraComplex.html) traits.

```rust
use faer::Mat;
use faer_ext::*;

let mut I_faer = Mat::<f32>::identity(8, 7);
let mut I_nalgebra = nalgebra::DMatrix::<f32>::identity(8, 7);

assert!(I_nalgebra.view_range(.., ..).into_faer() == I_faer);
assert!(I_faer.as_ref().into_nalgebra() == I_nalgebra);

assert!(I_nalgebra.view_range_mut(.., ..).into_faer() == I_faer);
assert!(I_faer.as_mut().into_nalgebra() == I_nalgebra);
```

# converting to/from _`ndarray`_ matrices
conversion from _`ndarray`_ types is done by enabling the `ndarray` feature and using the [`IntoFaer`](https://docs.rs/faer/latest/faer/trait.IntoFaer.html) and [`IntoFaerComplex`](https://docs.rs/faer-ext/latest/faer_ext/trait.IntoFaerComplex.html) traits.  
conversion to _`ndarray`_ types is enabled by the same feature and uses the [`IntoNdarray`](https://docs.rs/faer/latest/faer/trait.IntoNdarray.html) and [`IntoNdarrayComplex`](https://docs.rs/faer-ext/latest/faer_ext/trait.IntoNdarrayComplex.html) traits.

```rust
use faer::Mat;
use faer_ext::*;

let mut I_faer = Mat::<f32>::identity(8, 7);
let mut I_ndarray = ndarray::Array2::<f32>::zeros([8, 7]);
I_ndarray.diag_mut().fill(1.0);

assert_matrix_eq!(I_ndarray.view().into_faer(), I_faer, comp = exact);
assert!(I_faer.as_ref().into_ndarray() == I_ndarray);

assert!(I_ndarray.view_mut().into_faer() == I_faer);
assert!(I_faer.as_mut().into_ndarray() == I_ndarray);
```
