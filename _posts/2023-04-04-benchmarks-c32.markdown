---
layout: post
title:  "Benchmarks (c32)"
date:   2023-04-03 00:00:11 +0100
permalink: /bench-c32/
---

The benchmarks were run on an `11th Gen Intel(R) Core(TM) i5-11400 @ 2.60GHz` with 12 threads.  
- `nalgebra` is used with the `matrixmultiply` backend
- `ndarray` is used with the `openblas` backend
- `eigen` is compiled with `-march=native -O3 -fopenmp`

All computations are done on column major matrices containing `c32` values.

## Matrix multiplication

Multiplication of two square matrices of dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.1µs      2.1µs      3.1µs     17.9µs      2.4µs
   64     16.3µs     16.3µs       16µs    123.8µs      8.3µs
   96     53.8µs     17.6µs     16.5µs    455.3µs     15.7µs
  128    126.9µs     34.7µs     51.6µs      1.1ms     61.9µs
  192    423.7µs       94µs    111.1µs      3.5ms     91.5µs
  256        1ms      210µs      316µs      8.1ms    261.8µs
  384      3.4ms    637.9µs    778.7µs     26.8ms    648.7µs
  512      7.9ms      1.5ms      2.2ms     62.9ms      2.1ms
  640     15.6ms        3ms      4.2ms    122.2ms      3.3ms
  768     27.1ms      5.3ms      6.3ms    210.6ms      5.1ms
  896     43.3ms      8.4ms     10.9ms    337.8ms     10.8ms
 1024       65ms     12.2ms     16.6ms    528.6ms     15.9ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.5µs      3.5µs     10.2µs     15.7µs     10.9µs
   64     16.9µs     17.1µs     32.3µs     83.9µs     48.4µs
   96     49.4µs       35µs     67.7µs    260.9µs    120.7µs
  128    104.2µs     62.8µs    168.6µs    611.9µs    237.7µs
  192    303.4µs    138.9µs    331.3µs        2ms    634.7µs
  256    667.7µs    267.1µs    768.5µs      4.6ms      1.3ms
  384      2.1ms    525.3µs      1.7ms     15.3ms      3.8ms
  512      4.8ms      1.1ms      4.3ms     35.3ms      7.5ms
  640      8.9ms      2.1ms        7ms     68.2ms     13.6ms
  768       15ms      3.8ms     11.7ms    116.4ms     22.9ms
  896     23.5ms      6.2ms     16.8ms    183.9ms     35.5ms
 1024     35.3ms        9ms     24.2ms      273ms     49.1ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      5.9µs     13.5µs      9.8µs     15.6µs     10.9µs
   64     17.9µs     29.7µs     32.4µs     84.2µs     48.5µs
   96     41.5µs     49.3µs     68.1µs    263.3µs    120.8µs
  128     65.5µs     68.9µs      169µs    611.4µs    237.8µs
  192    168.7µs    128.2µs    331.7µs        2ms    634.6µs
  256    318.1µs    210.5µs    778.3µs      4.6ms      1.3ms
  384    903.4µs    436.2µs      1.7ms     15.3ms      3.8ms
  512      1.9ms    748.5µs      4.3ms     35.3ms      7.5ms
  640      3.5ms      1.1ms        7ms     68.2ms     13.6ms
  768      5.8ms      1.5ms     11.6ms    116.4ms     22.9ms
  896      8.9ms      2.3ms     16.7ms    183.8ms     35.5ms
 1024       13ms      3.3ms     24.2ms    273.3ms     49.1ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.1µs      3.1µs      4.4µs      5.5µs     10.9µs
   64     11.6µs     11.8µs     45.8µs     29.1µs     43.8µs
   96       33µs     33.2µs     84.2µs     88.1µs    100.2µs
  128     51.9µs     52.3µs      136µs    204.1µs      172µs
  192    160.1µs    159.3µs      303µs    661.4µs    414.9µs
  256    298.6µs      238µs        1ms      1.5ms    765.7µs
  384    875.4µs    658.3µs      1.3ms      5.1ms      1.9ms
  512      2.1ms      1.1ms        4ms     12.1ms      3.9ms
  640      3.6ms      1.9ms      3.9ms     23.3ms      6.4ms
  768      6.4ms      2.8ms      6.3ms     39.7ms     10.2ms
  896      9.7ms      4.2ms      8.2ms     62.8ms     14.8ms
 1024     15.2ms      5.3ms     16.5ms     92.5ms     21.3ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32        6µs      6.1µs      6.8µs      9.7µs     21.3µs
   64     24.5µs     24.8µs     27.4µs     53.7µs     81.3µs
   96     60.2µs     60.3µs     66.9µs    171.8µs    185.2µs
  128    114.9µs    116.2µs    243.2µs    408.1µs    390.8µs
  192    314.5µs    319.3µs    431.4µs      1.3ms      1.1ms
  256    631.6µs    592.4µs    653.5µs      3.1ms        2ms
  384      1.8ms      1.3ms      1.4ms     10.1ms      4.4ms
  512      3.9ms      2.3ms      2.6ms       24ms      8.5ms
  640      7.1ms      3.6ms      3.9ms     45.9ms     12.8ms
  768     11.7ms      5.4ms      5.7ms     79.1ms       19ms
  896     18.1ms      7.7ms      7.9ms    124.6ms     25.9ms
 1024     26.5ms     10.6ms       11ms    189.1ms     35.5ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      8.2µs      8.4µs          -     18.8µs    202.2µs
   64     35.1µs     34.9µs          -    136.6µs      1.5ms
   96     92.5µs     92.7µs          -    449.6µs      4.8ms
  128      198µs    198.3µs          -      1.1ms     11.3ms
  192    574.6µs    576.9µs          -      3.5ms     37.9ms
  256      1.5ms      1.5ms          -      8.3ms     89.5ms
  384      4.6ms      4.4ms          -     27.5ms    297.6ms
  512       12ms      8.6ms          -     65.6ms      701ms
  640     21.3ms     12.5ms          -    126.6ms      1.36s
  768       37ms     18.5ms          -    218.5ms      2.34s
  896     56.9ms       26ms          -    347.5ms      3.71s
 1024     88.8ms     37.3ms          -    528.5ms      5.54s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     16.9µs     16.9µs     32.7µs     22.3µs     19.3µs
   64     54.6µs     54.6µs    256.7µs    135.7µs    102.8µs
   96    118.4µs    118.6µs    609.7µs    436.5µs    177.2µs
  128    218.3µs    218.5µs      1.1ms        1ms    320.2µs
  192    578.4µs      578µs      2.6ms      3.3ms    727.9µs
  256      1.2ms      1.2ms      4.8ms      7.7ms      1.4ms
  384      3.5ms      2.5ms     13.3ms     25.6ms      3.7ms
  512      7.6ms      4.4ms     24.8ms     60.5ms      7.8ms
  640     14.6ms      6.9ms     37.2ms    117.2ms     14.3ms
  768     24.4ms     10.5ms       58ms      202ms     23.8ms
  896     37.7ms       15ms     73.5ms    320.5ms     37.1ms
 1024     55.7ms       21ms    106.4ms    480.6ms     55.3ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     26.2µs     40.8µs          -     31.1µs     23.1µs
   64    110.9µs    127.7µs          -    219.5µs       92µs
   96    256.4µs    281.9µs          -    715.3µs    219.9µs
  128    500.9µs    554.3µs          -      1.7ms      423µs
  192      1.3ms      1.4ms          -      5.5ms      1.1ms
  256      2.6ms      2.4ms          -     12.9ms      2.3ms
  384      7.5ms      4.6ms          -       43ms      7.2ms
  512     16.8ms        8ms          -    101.7ms     18.1ms
  640     30.5ms     12.1ms          -    197.7ms     30.8ms
  768     50.6ms     17.1ms          -    340.8ms     52.5ms
  896     78.9ms     23.2ms          -    542.6ms     79.9ms
 1024    119.1ms     32.4ms          -    817.7ms    128.2ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     23.1µs     41.7µs     15.8µs       46µs     45.4µs
   64     78.8µs     94.4µs     71.4µs    243.8µs    186.1µs
   96    193.9µs      175µs    243.4µs    758.1µs    445.3µs
  128    332.9µs    266.4µs      536µs      1.7ms    955.9µs
  192    905.4µs    640.6µs        1ms      5.4ms      2.4ms
  256      1.8ms      1.1ms      1.8ms     12.8ms      4.9ms
  384      5.1ms      2.4ms      3.2ms     41.3ms     12.7ms
  512     11.1ms      4.3ms      6.1ms     96.6ms     25.2ms
  640     20.8ms      7.4ms      9.9ms    184.6ms     43.5ms
  768     34.7ms     11.5ms     14.8ms    315.6ms     69.6ms
  896     53.7ms     17.3ms     22.2ms    495.8ms    102.4ms
 1024     78.7ms     29.3ms     31.8ms    751.2ms    140.9ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    126.9µs    157.7µs    118.2µs    149.5µs    199.9µs
   64      446µs    442.8µs    969.6µs    889.7µs    838.5µs
   96    991.4µs        1ms        2ms      2.8ms      2.3ms
  128      1.8ms      1.8ms      3.3ms      6.9ms      3.6ms
  192      4.5ms      4.5ms      7.2ms     22.5ms      8.4ms
  256      8.8ms      7.5ms     12.8ms     60.4ms     15.4ms
  384     24.5ms     15.3ms     28.1ms    187.2ms     38.1ms
  512     53.4ms     28.3ms       56ms    530.8ms     75.8ms
  640     99.1ms     45.5ms     87.6ms    947.2ms    124.7ms
  768    161.5ms     71.7ms    132.8ms      1.76s    198.7ms
  896    248.3ms      120ms    188.6ms      2.77s    291.2ms
 1024    371.6ms      164ms    269.4ms      4.75s    447.7ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      1.8ms      1.8ms      8.2ms     10.2ms      3.5ms
   64      5.5ms      5.1ms     20.2ms     40.1ms     10.3ms
   96     11.6ms      7.7ms       36ms     89.7ms     23.1ms
  128     19.3ms     11.6ms     56.4ms    159.3ms     37.7ms
  192     41.1ms     21.7ms     85.6ms    359.2ms     71.1ms
  256     71.3ms     33.5ms    128.5ms      661ms    118.7ms
  384    153.1ms     65.1ms    202.3ms      1.57s    260.1ms
  512    274.5ms    105.4ms    467.6ms      3.07s    479.1ms
  640    430.8ms      154ms    463.3ms      4.94s    771.4ms
  768    628.9ms    255.5ms    617.6ms       7.5s      1.14s
  896    869.7ms    343.2ms    760.3ms      10.4s      1.56s
 1024      1.18s    439.5ms      1.17s     14.23s      2.07s
```

## Hermitian matrix eigenvalue decomposition

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     52.6µs     56.3µs    159.8µs     77.7µs     83.5µs
   64    219.1µs    222.8µs      1.7ms    502.8µs    353.3µs
   96    499.1µs    546.1µs      5.2ms      1.6ms    846.7µs
  128      902µs    890.5µs     11.7ms      3.5ms      1.7ms
  192      2.2ms      2.3ms     36.2ms       11ms      4.4ms
  256      4.3ms      3.8ms     83.4ms     25.6ms        9ms
  384     11.6ms      9.1ms    199.5ms     83.1ms     26.6ms
  512     24.7ms     15.9ms    400.3ms    195.2ms     61.1ms
  640     45.3ms     26.4ms    765.2ms    375.1ms    109.7ms
  768     74.3ms     39.2ms      1.28s    643.8ms    183.3ms
  896    113.3ms     55.9ms         2s      1.01s    284.9ms
 1024      166ms     87.6ms      2.83s      1.52s      430ms
```

## Non Hermitian matrix eigenvalue decomposition

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    194.7µs    190.5µs    437.8µs          -      1.3ms
   64        1ms        1ms      4.5ms          -      8.5ms
   96      3.7ms      3.8ms     10.6ms          -     30.2ms
  128      7.3ms      7.7ms     21.9ms          -     65.7ms
  192     17.7ms     20.6ms       54ms          -    205.2ms
  256     33.4ms     34.2ms     71.8ms          -    478.7ms
  384       77ms     79.3ms    154.1ms          -      1.59s
  512    179.6ms    167.7ms    422.8ms          -      4.01s
  640    351.6ms    304.3ms    630.5ms          -      7.41s
  768      555ms    478.4ms    836.7ms          -     13.57s
  896    785.6ms    654.5ms      1.05s          -     20.93s
 1024      1.21s    985.3ms      1.21s          -     32.49s
```
