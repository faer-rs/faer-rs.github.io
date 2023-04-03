---
layout: post
title:  "Benchmarks (c32)"
date:   2022-12-10 00:00:59 +0100
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
   32      2.2µs      2.2µs      3.1µs     17.9µs      2.4µs
   64     16.4µs     16.4µs     19.2µs    141.1µs      8.3µs
   96     53.6µs       18µs     20.9µs    446.4µs     15.6µs
  128    126.7µs       34µs     65.1µs      1.1ms     61.1µs
  192    424.3µs     90.6µs    110.3µs      3.5ms     91.6µs
  256        1ms    207.8µs    317.1µs      8.1ms    262.2µs
  384      3.4ms    651.6µs    779.3µs     26.8ms    648.6µs
  512        8ms      1.6ms      2.1ms     62.9ms      2.1ms
  640     15.6ms        3ms      4.1ms    122.2ms      3.3ms
  768     27.2ms      5.4ms      6.4ms    210.7ms      5.1ms
  896     43.5ms      8.6ms       11ms    335.7ms     10.8ms
 1024     66.2ms       15ms     16.4ms    524.4ms     16.1ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.8µs      3.8µs     10.7µs     15.5µs     10.9µs
   64     17.2µs     17.2µs     31.2µs       84µs     48.5µs
   96     50.7µs     38.1µs       68µs    256.8µs    120.7µs
  128    104.9µs     64.5µs    168.8µs    577.4µs    238.5µs
  192    310.8µs    139.3µs    329.4µs        2ms    633.6µs
  256    676.5µs      262µs    753.7µs      4.6ms      1.3ms
  384      2.1ms    526.9µs      1.7ms     15.2ms      3.8ms
  512      4.8ms      1.1ms      4.2ms       35ms      7.5ms
  640        9ms      2.1ms      6.9ms     67.9ms     13.6ms
  768     15.2ms        4ms     11.5ms    116.2ms     22.7ms
  896     23.7ms      6.2ms     16.6ms    182.8ms     35.1ms
 1024     35.5ms      9.1ms     24.1ms    272.8ms     48.7ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.6µs     12.3µs     10.6µs     15.5µs     10.9µs
   64     15.6µs     27.8µs     31.1µs     84.1µs     48.5µs
   96     37.5µs     47.7µs     68.3µs    256.7µs    120.7µs
  128     61.3µs     68.7µs    169.3µs    598.1µs    238.6µs
  192    162.2µs    124.7µs    330.3µs        2ms    633.6µs
  256    310.7µs    206.5µs    768.2µs      4.6ms      1.3ms
  384    890.2µs    416.4µs      1.7ms     15.3ms      3.8ms
  512      1.9ms      744µs      4.3ms     35.1ms      7.5ms
  640      3.5ms      1.1ms      6.9ms     68.1ms     13.6ms
  768      5.8ms      1.5ms     11.6ms    116.5ms     22.8ms
  896      8.8ms      2.3ms     16.7ms    183.2ms     35.2ms
 1024       13ms      3.3ms     24.2ms    273.2ms     48.8ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      9.7µs      9.7µs      4.5µs      6.9µs     10.9µs
   64       25µs       25µs     43.8µs     34.2µs     43.4µs
   96     58.3µs     58.3µs     83.7µs    100.3µs     99.6µs
  128     79.3µs     79.3µs    237.6µs      229µs    170.1µs
  192    212.8µs    213.6µs    343.1µs      722µs    410.9µs
  256    351.3µs    292.4µs    767.2µs      1.7ms    757.2µs
  384    955.8µs    778.2µs      1.3ms      5.3ms      1.9ms
  512      2.1ms      1.2ms      4.1ms     12.9ms      3.9ms
  640      3.8ms      2.1ms      3.9ms     24.3ms      6.3ms
  768      6.5ms        3ms      6.2ms     41.5ms     10.1ms
  896      9.9ms      4.3ms      8.1ms     64.6ms     14.7ms
 1024     15.3ms      5.4ms     16.5ms     95.8ms     21.2ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      8.3µs      8.3µs      6.9µs      9.7µs     21.2µs
   64     43.2µs     43.1µs     27.4µs     53.6µs       81µs
   96      102µs     90.5µs       67µs    172.4µs    184.3µs
  128    177.3µs    182.1µs    234.7µs    409.6µs    389.1µs
  192    491.1µs    478.3µs    416.9µs      1.3ms      1.1ms
  256      920µs    997.5µs    631.8µs      3.1ms        2ms
  384      2.6ms      2.1ms      1.3ms     10.1ms      4.5ms
  512      5.1ms      4.1ms      2.2ms     24.2ms      8.5ms
  640      9.5ms      7.3ms      5.3ms     45.8ms     12.8ms
  768     15.3ms      9.9ms      5.7ms     79.2ms     18.9ms
  896     21.6ms     15.6ms      7.9ms    124.2ms     25.6ms
 1024     33.3ms     20.5ms       11ms    189.5ms     35.2ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     19.5µs     19.5µs          -     18.5µs    201.5µs
   64     82.9µs       83µs          -    136.5µs      1.5ms
   96    202.1µs    202.4µs          -    452.5µs      4.8ms
  128    413.4µs    415.5µs          -      1.1ms     11.3ms
  192      1.1ms      1.1ms          -      3.5ms     37.9ms
  256      2.3ms      2.3ms          -      8.3ms     89.5ms
  384      6.2ms        6ms          -     27.6ms    297.6ms
  512       14ms     10.5ms          -     65.6ms    700.5ms
  640     24.4ms     15.1ms          -    126.6ms      1.36s
  768     40.1ms     21.3ms          -    218.7ms      2.34s
  896     60.8ms     30.8ms          -      346ms      3.71s
 1024     93.2ms     41.7ms          -    528.6ms      5.54s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     20.7µs     20.7µs     32.8µs     22.3µs     19.2µs
   64       60µs     59.7µs    256.6µs      136µs     99.5µs
   96    125.3µs      125µs    607.1µs    438.1µs    173.2µs
  128    227.5µs    227.4µs      1.1ms        1ms      313µs
  192    602.4µs    601.4µs      2.6ms      3.3ms    715.3µs
  256      1.2ms      1.2ms      5.6ms      7.7ms      1.4ms
  384      3.6ms      2.6ms     18.1ms     25.6ms      3.7ms
  512      7.8ms      4.8ms     24.5ms     60.6ms      7.8ms
  640       15ms      7.4ms     35.9ms    117.5ms     14.2ms
  768       25ms     11.2ms       55ms    202.5ms     23.9ms
  896     38.4ms     15.9ms     73.7ms    321.1ms     37.2ms
 1024     56.4ms     22.4ms    105.6ms    481.6ms     55.5ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     42.7µs     56.9µs          -     30.7µs       23µs
   64    174.4µs    198.5µs          -    219.7µs     91.2µs
   96    389.8µs    439.8µs          -    717.7µs    219.8µs
  128    735.3µs    794.4µs          -      1.7ms    423.1µs
  192      1.8ms      1.9ms          -      5.5ms      1.1ms
  256      3.5ms      3.1ms          -     12.9ms      2.3ms
  384      9.5ms      5.4ms          -     43.1ms      7.4ms
  512     20.2ms      8.9ms          -    101.9ms     18.6ms
  640     36.1ms     13.6ms          -    198.1ms     33.9ms
  768     58.5ms     18.9ms          -    341.6ms     57.8ms
  896     89.4ms     25.7ms          -      542ms     87.7ms
 1024    132.9ms     37.3ms          -    818.7ms    136.1ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     21.6µs     43.8µs     15.6µs     46.1µs     45.2µs
   64     84.9µs    105.4µs     72.1µs    245.2µs    185.9µs
   96    212.1µs    212.5µs    212.9µs      757µs    445.8µs
  128    403.1µs    332.6µs    399.7µs      1.7ms    957.6µs
  192      1.1ms      796µs    955.2µs      5.5ms      2.5ms
  256        2ms      1.5ms      1.8ms     12.9ms      4.8ms
  384      5.9ms      3.1ms      3.2ms     41.6ms     12.7ms
  512     12.4ms      6.1ms      6.1ms     97.9ms     25.3ms
  640     23.2ms     10.8ms     10.1ms    185.7ms     43.5ms
  768     37.6ms     16.6ms     14.9ms    317.8ms     69.7ms
  896     57.8ms     25.1ms     22.2ms    498.9ms    102.5ms
 1024     83.8ms       33ms     32.2ms    753.7ms    141.5ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    172.6µs    207.3µs    115.9µs    148.6µs    208.7µs
   64    598.1µs    583.9µs        1ms    906.7µs    824.3µs
   96      1.3ms      1.3ms      1.9ms      2.8ms      2.2ms
  128      2.4ms      2.3ms      3.3ms      6.9ms      3.5ms
  192      5.7ms      5.4ms      7.1ms     22.6ms      8.3ms
  256     10.9ms      8.6ms     12.8ms     60.2ms     15.2ms
  384     28.8ms     17.4ms     28.3ms    187.6ms     38.2ms
  512     61.3ms       32ms       56ms    528.5ms     78.2ms
  640    109.6ms     50.4ms     85.8ms    937.7ms    126.7ms
  768    181.1ms     87.4ms    131.7ms      1.75s    203.5ms
  896    270.1ms    130.8ms    184.8ms      2.78s    295.1ms
 1024    403.5ms    180.1ms    272.1ms      4.62s    453.9ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.1ms      2.2ms      8.3ms     10.2ms      3.5ms
   64      6.2ms      5.9ms     20.5ms     40.3ms     10.6ms
   96     12.7ms      9.6ms     36.1ms     89.9ms     23.8ms
  128       21ms       14ms     56.7ms    159.7ms     39.5ms
  192     43.9ms     25.4ms     84.7ms    372.6ms     71.3ms
  256     75.8ms     38.6ms    126.3ms    666.8ms    119.8ms
  384    160.7ms     71.3ms    201.9ms      1.58s    263.6ms
  512    284.6ms    112.8ms    470.1ms      3.06s    484.1ms
  640    447.8ms    164.9ms    462.8ms      4.95s    777.3ms
  768    650.7ms    254.6ms    625.5ms      7.43s      1.14s
  896    898.2ms    356.9ms    768.3ms     10.33s      1.58s
 1024      1.22s    454.1ms      1.18s      14.2s      2.07s
```
