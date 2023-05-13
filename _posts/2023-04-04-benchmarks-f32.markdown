---
layout: post
title:  "Benchmarks (f32)"
date:   2023-04-03 00:00:01 +0100
permalink: /bench-f32/
---

The benchmarks were run on an `11th Gen Intel(R) Core(TM) i5-11400 @ 2.60GHz` with 12 threads.  
- `nalgebra` is used with the `matrixmultiply` backend
- `ndarray` is used with the `openblas` backend
- `eigen` is compiled with `-march=native -O3 -fopenmp`

All computations are done on column major matrices containing `f32` values.

## Matrix multiplication

Multiplication of two square matrices of dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      612ns      566ns      564ns      1.2µs      649ns
   64      4.1µs      4.1µs        4µs        7µs      3.7µs
   96     13.9µs      8.4µs     13.1µs     21.8µs      6.8µs
  128     32.3µs     11.4µs     28.8µs     50.6µs     17.5µs
  192    107.5µs     31.2µs     38.6µs    164.6µs     26.7µs
  256    255.5µs     64.1µs    113.7µs    387.4µs     71.9µs
  384    849.1µs    183.1µs    216.8µs      1.3ms    179.5µs
  512        2ms    402.5µs    604.5µs        3ms    553.6µs
  640      3.9ms    907.5µs      1.1ms      5.9ms    982.6µs
  768      6.8ms      1.6ms      1.5ms     10.1ms      1.5ms
  896     10.8ms      2.6ms      2.8ms     16.2ms      2.8ms
 1024     16.3ms      3.9ms      4.1ms     24.4ms      3.9ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.3µs      2.3µs      9.7µs      6.1µs      2.8µs
   64      9.1µs      9.2µs       27µs       27µs     12.2µs
   96     23.4µs     23.9µs     54.3µs     70.6µs     30.3µs
  128     44.5µs     34.3µs    118.3µs    150.9µs     64.2µs
  192    121.3µs     78.4µs    218.3µs    474.9µs    153.8µs
  256    247.1µs    125.3µs    583.8µs        1ms    345.1µs
  384    702.6µs    222.9µs      1.1ms      3.6ms    886.1µs
  512      1.5ms    695.4µs        3ms      8.9ms        2ms
  640      2.8ms      1.1ms      3.7ms     18.4ms      3.3ms
  768      4.5ms      1.6ms      7.6ms     31.4ms      5.5ms
  896      6.8ms      2.2ms      8.6ms     48.7ms      8.2ms
 1024     10.5ms      3.1ms     18.1ms     70.8ms     13.3ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      5.2µs     11.7µs      8.5µs      6.2µs      2.8µs
   64       14µs     18.4µs     27.1µs     27.1µs     12.2µs
   96     31.6µs     38.4µs     54.3µs     70.6µs     30.3µs
  128     41.9µs     48.2µs      118µs    150.9µs     64.2µs
  192     98.8µs     80.7µs    217.9µs    474.8µs    153.8µs
  256    153.3µs    114.9µs    583.4µs      1.1ms      345µs
  384    392.1µs    218.7µs      1.1ms      3.6ms    885.6µs
  512    731.2µs    323.7µs        3ms      8.9ms        2ms
  640      1.3ms    459.9µs      3.5ms     18.3ms      3.3ms
  768        2ms      680µs      7.6ms       31ms      5.5ms
  896      2.9ms      1.5ms      8.6ms     48.2ms      8.2ms
 1024      4.3ms      1.5ms     18.2ms     70.2ms     13.3ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.2µs      2.2µs      2.7µs      1.9µs      2.1µs
   64      6.6µs      6.7µs     12.4µs      8.1µs      7.1µs
   96     17.5µs     17.7µs     27.2µs     20.8µs     16.4µs
  128     22.4µs     22.7µs    122.2µs     45.2µs     27.1µs
  192     65.7µs     72.9µs    255.9µs    131.5µs     64.7µs
  256     94.1µs    101.8µs    422.8µs    324.1µs    131.3µs
  384    253.8µs      307µs      863µs      1.1ms    357.5µs
  512    577.9µs    416.2µs        3ms        3ms    755.9µs
  640        1ms    943.3µs      2.8ms      5.5ms      1.3ms
  768      1.8ms      1.1ms      4.2ms     10.4ms      2.1ms
  896      2.7ms      1.5ms      5.4ms     16.9ms        3ms
 1024      4.2ms      1.8ms     13.6ms     24.6ms      4.6ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.1µs      4.1µs      4.3µs      4.6µs      3.9µs
   64     15.4µs     15.2µs     15.5µs     18.6µs     14.2µs
   96     33.6µs     33.6µs     33.8µs       47µs     31.8µs
  128     62.2µs     62.3µs     64.5µs    105.4µs     91.8µs
  192    153.2µs    192.5µs    151.3µs    314.2µs      330µs
  256    290.8µs    326.3µs    368.4µs    762.6µs    655.9µs
  384    737.1µs    702.8µs    675.4µs      2.5ms      1.4ms
  512      1.5ms      1.2ms      1.1ms      6.4ms      3.4ms
  640      2.6ms      1.8ms      1.7ms     11.5ms      4.4ms
  768      4.1ms      2.5ms      3.9ms     20.2ms      6.7ms
  896        6ms      3.5ms      3.9ms     31.2ms        8ms
 1024      8.7ms      4.8ms      5.1ms     47.6ms     13.8ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      6.2µs      6.3µs          -     14.4µs     10.6µs
   64     23.7µs     23.6µs          -    100.8µs     62.4µs
   96     55.1µs     55.3µs          -    322.5µs    180.3µs
  128    116.4µs    115.7µs          -    761.7µs    408.6µs
  192    302.6µs    303.6µs          -      2.5ms      1.2ms
  256    657.9µs    653.8µs          -      5.9ms      2.8ms
  384        2ms      2.7ms          -     19.8ms      8.9ms
  512      5.6ms      5.3ms          -     47.3ms     21.9ms
  640      9.6ms      8.2ms          -     91.3ms     41.1ms
  768     16.5ms     12.8ms          -    157.7ms     71.5ms
  896     25.7ms       18ms          -    250.2ms      112ms
 1024     40.7ms     25.2ms          -    375.1ms    169.6ms
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     12.9µs     12.9µs     16.6µs      6.5µs      7.1µs
   64     37.2µs     37.1µs     62.4µs     34.4µs     39.9µs
   96     71.1µs     71.1µs    266.4µs    100.9µs     64.6µs
  128    117.3µs    117.4µs    661.8µs      223µs    111.6µs
  192    269.4µs    269.4µs      1.5ms    719.2µs    235.9µs
  256    503.1µs    616.8µs      2.8ms      1.7ms    465.2µs
  384      1.3ms      1.5ms      9.1ms      5.3ms      1.1ms
  512      2.7ms      2.6ms     12.3ms     12.4ms      2.4ms
  640      4.7ms      3.6ms     16.8ms     23.3ms      4.2ms
  768      7.5ms      5.2ms     28.8ms     39.8ms      6.7ms
  896     11.2ms        7ms     38.2ms     62.6ms     10.3ms
 1024     16.2ms      9.5ms     55.1ms     93.1ms       15ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     20.1µs     28.3µs          -     16.6µs      8.9µs
   64     76.3µs     97.8µs          -      118µs     32.2µs
   96    177.2µs    199.1µs          -    379.9µs     73.7µs
  128    326.8µs    378.1µs          -    879.8µs    147.9µs
  192      811µs    866.2µs          -      2.9ms    392.6µs
  256      1.6ms      1.6ms          -      6.9ms    857.3µs
  384      4.1ms      3.1ms          -     22.7ms      2.8ms
  512      8.8ms      5.4ms          -     53.4ms      7.5ms
  640     15.9ms      8.1ms          -    103.3ms     13.9ms
  768     25.7ms     11.6ms          -    177.9ms     24.1ms
  896     38.7ms     15.8ms          -      282ms     38.3ms
 1024     55.8ms     20.7ms          -    420.7ms     60.9ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32       17µs     33.8µs      8.9µs     18.9µs       11µs
   64     52.6µs     72.4µs     34.9µs     78.2µs     43.4µs
   96    126.1µs      122µs    197.4µs    202.9µs    101.3µs
  128    184.1µs    163.3µs    334.2µs    440.1µs    253.7µs
  192    461.9µs    379.3µs    662.4µs      1.3ms    723.4µs
  256    768.7µs    561.3µs        1ms      3.1ms      1.5ms
  384        2ms      1.2ms        2ms       10ms      3.6ms
  512      3.9ms      1.9ms      3.5ms     25.4ms      8.2ms
  640      7.1ms      3.2ms      5.1ms     47.1ms     13.1ms
  768     11.1ms      4.5ms      7.3ms     80.3ms     20.6ms
  896     16.5ms        7ms     10.7ms    123.5ms     28.5ms
 1024     24.4ms       11ms     14.8ms    185.4ms     44.4ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     99.6µs    130.3µs     80.5µs       74µs    141.7µs
   64    317.5µs    297.3µs    590.2µs      373µs    590.1µs
   96    675.6µs    712.3µs      1.4ms      1.1ms      1.6ms
  128      1.2ms      1.1ms      2.3ms        3ms      2.5ms
  192      2.7ms      2.9ms        5ms      8.2ms      5.6ms
  256        5ms        5ms      8.5ms     23.7ms      9.4ms
  384     12.9ms     10.9ms     18.1ms     75.1ms     22.3ms
  512     26.3ms     18.4ms     35.3ms    283.9ms     43.4ms
  640       46ms     28.6ms     50.8ms    350.3ms     69.4ms
  768     74.2ms     49.7ms     80.7ms      855ms    106.8ms
  896    110.2ms     68.2ms      110ms      1.03s      151ms
 1024    165.2ms     93.3ms    169.7ms      2.41s    226.8ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    964.4µs      1.1ms      4.5ms      2.6ms      1.2ms
   64      2.5ms      2.6ms     13.8ms      9.4ms      3.9ms
   96      4.8ms      4.5ms     26.6ms     20.6ms      8.3ms
  128      7.4ms      6.6ms     41.4ms     36.9ms     13.5ms
  192     14.5ms     12.1ms     57.2ms     85.3ms     27.4ms
  256     23.9ms     18.3ms     72.8ms    154.6ms     44.6ms
  384     51.5ms       33ms    104.7ms    359.6ms     92.3ms
  512     90.3ms     51.3ms    147.7ms    799.1ms    159.4ms
  640    139.6ms     72.1ms    190.7ms      1.18s      251ms
  768    204.6ms    114.9ms    251.9ms      2.18s    365.4ms
  896    282.5ms    151.6ms    323.6ms      2.89s    510.8ms
 1024    383.3ms    190.5ms    540.4ms      5.02s    710.1ms
```

## Hermitian matrix eigenvalue decomposition

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32       44µs     48.3µs    162.5µs     35.9µs     37.2µs
   64    175.4µs    181.8µs    822.4µs      182µs    151.2µs
   96    387.1µs      453µs      2.2ms    499.2µs    356.8µs
  128    668.7µs    671.3µs      4.3ms      1.1ms    694.4µs
  192      1.5ms      1.6ms     12.9ms      3.2ms      1.8ms
  256      2.7ms      2.7ms       28ms      7.3ms      3.8ms
  384      6.7ms      6.3ms     85.6ms     22.4ms     10.9ms
  512     12.9ms     11.1ms    152.4ms     51.8ms     25.6ms
  640     21.9ms     18.4ms    232.7ms     97.4ms     48.1ms
  768     34.2ms     26.5ms    354.6ms    166.4ms     83.6ms
  896     51.4ms     39.5ms    504.3ms      260ms    134.4ms
 1024     73.4ms     52.3ms    789.9ms    386.3ms    196.6ms
```

## Non Hermitian matrix eigenvalue decomposition

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      160µs    165.6µs    146.6µs          -      181µs
   64    902.1µs    874.1µs    813.8µs          -      703µs
   96      1.9ms      2.2ms      4.2ms          -        2ms
  128      3.4ms        4ms      8.1ms          -      5.2ms
  192      9.3ms     10.7ms     12.5ms          -     12.8ms
  256     15.2ms     16.6ms     40.5ms          -     44.7ms
  384     34.1ms     37.4ms     64.3ms          -    131.8ms
  512     71.6ms     78.3ms    174.9ms          -    444.2ms
  640    131.5ms      127ms    246.4ms          -      546ms
  768    190.2ms    186.2ms    325.8ms          -      1.27s
  896    248.1ms    240.8ms    412.7ms          -      1.61s
 1024    421.8ms    382.7ms    648.9ms          -       4.5s
```
