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
   32      658ns      610ns      591ns      1.3µs      647ns
   64      4.1µs      4.1µs        4µs      7.4µs      3.7µs
   96     13.9µs      8.6µs     13.1µs     22.8µs      6.8µs
  128     32.4µs     11.4µs     29.3µs     51.6µs     17.5µs
  192      108µs     31.5µs     32.3µs    168.4µs     26.9µs
  256    255.4µs     65.8µs    112.9µs    389.5µs       72µs
  384    854.9µs    188.4µs    216.4µs      1.3ms    179.9µs
  512        2ms    422.3µs    604.8µs        3ms    553.4µs
  640      3.9ms      817µs      1.1ms      5.9ms      984µs
  768      6.8ms      1.3ms      1.6ms     10.2ms      1.5ms
  896     10.8ms      2.4ms      2.9ms     16.3ms      2.8ms
 1024     16.5ms      4.1ms      4.1ms     24.4ms      3.9ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.5µs      2.4µs      8.8µs      6.2µs      2.8µs
   64      9.2µs      9.2µs     28.2µs     27.3µs     12.2µs
   96     24.9µs     25.5µs     54.3µs     70.7µs     30.3µs
  128     45.4µs     33.8µs    116.4µs      149µs     65.9µs
  192    126.4µs     78.2µs    215.8µs    471.8µs    154.3µs
  256    251.8µs    124.2µs    564.9µs      1.1ms    346.7µs
  384    728.3µs    228.6µs      1.1ms      3.5ms    873.3µs
  512      1.6ms    746.8µs      2.9ms      8.6ms        2ms
  640      2.9ms      1.1ms      3.5ms     16.1ms      3.3ms
  768      4.6ms      1.6ms      7.4ms     28.2ms      5.4ms
  896        7ms      2.2ms      8.6ms     45.3ms      8.1ms
 1024     10.7ms      3.1ms     17.8ms     65.9ms     13.4ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.8µs     10.7µs      9.1µs      6.2µs      2.8µs
   64     11.3µs     17.4µs     28.5µs     27.4µs     12.2µs
   96     26.6µs       35µs     54.3µs     70.6µs     30.5µs
  128     36.5µs     46.6µs    116.2µs    147.8µs     65.9µs
  192       89µs     76.8µs    215.1µs    472.1µs    154.3µs
  256    142.8µs      112µs    565.1µs      1.1ms    346.5µs
  384    374.8µs    208.1µs      1.1ms      3.5ms    873.1µs
  512    717.9µs    311.8µs      2.9ms      8.6ms        2ms
  640      1.3ms    448.9µs      3.6ms     16.1ms      3.3ms
  768        2ms        1ms      7.4ms     28.2ms      5.4ms
  896      2.9ms      1.1ms      8.7ms     45.3ms      8.1ms
 1024      4.3ms      1.5ms     17.9ms     65.8ms     13.5ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      1.9µs      1.9µs      2.6µs      2.3µs      2.2µs
   64      6.2µs      6.2µs     12.6µs       10µs      7.3µs
   96     15.9µs       16µs     27.4µs     27.9µs     16.6µs
  128     21.7µs     21.8µs    122.1µs     55.4µs       28µs
  192     62.8µs     71.1µs    249.1µs    171.4µs     66.3µs
  256     94.7µs    107.6µs    411.9µs    397.4µs    136.9µs
  384    256.3µs    297.6µs      1.1ms      1.2ms    391.8µs
  512    584.3µs      404µs      3.9ms      3.2ms    776.4µs
  640        1ms    897.2µs      2.8ms      5.8ms      1.3ms
  768      1.8ms        1ms      4.1ms     10.2ms      2.1ms
  896      2.8ms      1.5ms      5.2ms       16ms      3.1ms
 1024      4.2ms      1.7ms     13.8ms     24.1ms      4.7ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.5µs      3.5µs      4.3µs      4.5µs      3.8µs
   64     16.1µs     17.2µs     15.6µs     17.9µs     14.3µs
   96     40.1µs     40.3µs     33.9µs       46µs     31.5µs
  128     73.6µs     77.6µs     64.1µs    102.1µs     91.3µs
  192    188.9µs    214.6µs    151.5µs      301µs      331µs
  256    350.5µs    424.7µs    364.9µs    674.8µs    643.9µs
  384    957.3µs      990µs    679.1µs      2.4ms      1.4ms
  512      1.8ms      1.7ms      1.1ms      6.1ms      3.4ms
  640      3.1ms      2.7ms      1.6ms     11.2ms      4.4ms
  768      5.1ms      4.2ms      3.6ms     19.3ms      6.8ms
  896      7.3ms      5.5ms      3.9ms     29.8ms      8.1ms
 1024     10.7ms      8.1ms      5.1ms       46ms     14.6ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      8.1µs        8µs          -     14.1µs       12µs
   64       31µs     30.9µs          -    100.5µs     65.5µs
   96     73.6µs       74µs          -    321.7µs    183.2µs
  128      155µs    154.7µs          -    766.3µs    413.5µs
  192    383.3µs    383.2µs          -      2.5ms      1.2ms
  256    822.9µs    820.4µs          -      5.9ms      2.8ms
  384      2.4ms      2.6ms          -     19.8ms      8.9ms
  512      6.2ms      5.7ms          -     47.4ms       22ms
  640     10.9ms      8.6ms          -     91.8ms     41.5ms
  768     19.2ms     13.6ms          -    158.8ms     72.3ms
  896     28.7ms     18.5ms          -    251.5ms      113ms
 1024     44.3ms     24.5ms          -    377.1ms    172.2ms
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     11.3µs     11.4µs     16.7µs      6.5µs        7µs
   64       33µs       33µs     63.6µs     34.2µs     40.3µs
   96     64.2µs     64.1µs    271.7µs      101µs     63.4µs
  128    106.7µs    106.6µs      660µs    223.1µs    111.5µs
  192    248.5µs    248.5µs      1.4ms    720.3µs    236.9µs
  256    473.2µs    576.1µs      2.6ms      1.7ms      464µs
  384      1.3ms      1.5ms      9.2ms      5.3ms      1.2ms
  512      2.6ms      2.6ms     12.3ms     12.2ms      2.4ms
  640      4.6ms      3.5ms     17.1ms     23.4ms      4.2ms
  768      7.4ms      5.1ms     28.3ms       40ms      6.8ms
  896     11.1ms      6.9ms     37.5ms     62.8ms     10.3ms
 1024     16.1ms      9.4ms     55.6ms     93.9ms     15.1ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     25.7µs     39.2µs          -     16.6µs      8.6µs
   64    101.2µs    127.1µs          -    117.1µs     32.9µs
   96    232.9µs      261µs          -    380.2µs     71.8µs
  128    426.2µs    470.6µs          -    881.1µs    146.9µs
  192        1ms      1.1ms          -      2.9ms      396µs
  256        2ms      1.9ms          -      6.9ms    885.4µs
  384      5.2ms      3.6ms          -     22.7ms      2.7ms
  512     10.7ms      6.1ms          -     53.3ms      7.4ms
  640     18.7ms      9.1ms          -    103.6ms     14.6ms
  768       30ms       13ms          -    178.5ms     26.1ms
  896     45.2ms     17.5ms          -    282.8ms     41.2ms
 1024     65.9ms     22.5ms          -    422.4ms     67.9ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     13.4µs     29.2µs      8.7µs     19.3µs       11µs
   64     47.7µs     67.8µs     34.8µs     77.2µs     42.8µs
   96    117.4µs    120.1µs    172.7µs    202.1µs    100.9µs
  128    181.4µs    169.6µs    330.7µs    433.5µs    254.4µs
  192      455µs    380.1µs    625.9µs      1.3ms      724µs
  256    784.5µs    639.7µs        1ms      3.1ms      1.5ms
  384      2.2ms      1.4ms      1.9ms      9.9ms      3.5ms
  512      4.2ms      2.3ms      3.5ms     24.9ms      8.2ms
  640      7.7ms      4.2ms      5.1ms     46.3ms       13ms
  768     11.9ms      6.3ms      6.8ms     78.9ms     20.6ms
  896       18ms      8.6ms     10.6ms    121.1ms     28.2ms
 1024     26.7ms     12.7ms     14.7ms    184.5ms     44.5ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    130.4µs    168.1µs     79.5µs     75.9µs    138.7µs
   64    433.8µs    414.9µs    587.4µs    373.3µs    580.7µs
   96    935.5µs    974.3µs      1.3ms      1.1ms      1.5ms
  128      1.6ms      1.6ms      2.4ms      2.9ms      2.4ms
  192      3.6ms      3.6ms        5ms      8.2ms      5.5ms
  256      6.5ms        6ms      8.3ms     23.1ms      9.4ms
  384       16ms     12.2ms     18.1ms     75.3ms     22.2ms
  512     31.5ms     20.4ms     35.5ms    274.5ms     43.3ms
  640     53.3ms     35.8ms     50.3ms    356.1ms     69.1ms
  768     85.5ms     55.3ms       80ms    860.9ms    106.6ms
  896    125.9ms       77ms    108.2ms      1.01s    150.8ms
 1024    183.6ms    100.5ms      168ms      2.37s    228.7ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    959.1µs      1.1ms      4.5ms      2.6ms      1.2ms
   64      2.5ms      2.6ms     13.7ms      9.4ms        4ms
   96      4.9ms      4.8ms     26.4ms     20.8ms      8.6ms
  128      7.7ms      7.1ms     41.2ms     37.6ms     13.9ms
  192     15.1ms       13ms     55.3ms     84.6ms     28.7ms
  256       25ms     19.6ms     72.1ms    153.7ms     46.4ms
  384     54.4ms     34.8ms      104ms      364ms     95.6ms
  512     95.1ms     53.4ms    145.4ms    796.2ms    164.3ms
  640    148.6ms     82.2ms    190.7ms      1.19s    252.4ms
  768    216.5ms    124.9ms      251ms      2.17s      374ms
  896    299.1ms    164.1ms    323.7ms      2.94s    526.1ms
 1024    404.9ms    204.1ms    546.2ms      5.08s    723.1ms
```
