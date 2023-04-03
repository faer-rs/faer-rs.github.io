---
layout: post
title:  "Benchmarks (f32)"
date:   2022-12-10 00:00:59 +0100
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
   32      655ns      611ns      595ns      1.3µs      649ns
   64      4.1µs      4.1µs        4µs      7.6µs      3.7µs
   96     13.9µs      8.6µs     13.1µs     22.6µs      6.8µs
  128     32.3µs     11.4µs     28.9µs     52.1µs     17.6µs
  192    107.4µs     31.4µs     32.3µs      168µs     26.7µs
  256    255.3µs     67.4µs    113.2µs    389.9µs     72.1µs
  384    853.9µs    184.9µs    217.1µs      1.3ms    180.8µs
  512        2ms    430.8µs    608.7µs        3ms      582µs
  640      3.9ms    816.6µs      1.1ms      5.9ms      994µs
  768      6.8ms      1.3ms      1.6ms     10.2ms      1.5ms
  896     10.9ms      2.3ms      2.8ms     16.2ms      2.8ms
 1024     16.5ms      4.1ms      4.2ms     24.4ms        4ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.5µs      2.4µs      8.5µs      6.2µs      2.8µs
   64      9.4µs      9.3µs     27.1µs     27.1µs     12.3µs
   96     24.7µs     26.3µs     54.8µs     70.7µs     30.4µs
  128     45.3µs     36.5µs    115.9µs    148.4µs     64.5µs
  192    126.1µs     78.3µs    216.7µs    471.1µs    154.6µs
  256      252µs    123.8µs    578.3µs      1.1ms    350.9µs
  384    720.8µs    224.2µs      1.1ms      3.5ms    873.1µs
  512      1.6ms    443.3µs        3ms      8.4ms        2ms
  640      2.9ms        1ms      3.5ms     17.1ms      3.2ms
  768      4.6ms      1.6ms      7.5ms     28.9ms      5.5ms
  896        7ms      2.2ms      8.6ms     45.4ms      8.2ms
 1024     10.7ms      3.2ms       18ms     67.8ms     13.3ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.6µs     10.7µs      8.5µs      6.2µs      2.8µs
   64     10.9µs     17.5µs       27µs     27.2µs     12.3µs
   96     25.6µs       36µs     54.7µs     70.7µs     30.4µs
  128     35.6µs     47.5µs    116.6µs    148.9µs     64.5µs
  192     86.9µs     76.9µs      217µs      471µs    154.8µs
  256      141µs    112.6µs    580.4µs      1.1ms    350.8µs
  384    369.1µs    208.7µs      1.1ms      3.5ms    872.9µs
  512      710µs    315.7µs        3ms      8.4ms        2ms
  640      1.3ms    544.3µs      3.6ms     17.1ms      3.2ms
  768        2ms      1.1ms      7.6ms     28.9ms      5.5ms
  896      2.9ms      1.1ms      8.5ms     45.5ms      8.2ms
 1024      4.3ms      1.5ms       18ms     67.5ms     13.3ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.7µs      4.7µs      2.7µs      2.5µs      2.2µs
   64     11.6µs     11.7µs     12.5µs     10.4µs      7.3µs
   96     25.8µs     25.8µs     27.2µs       28µs     16.6µs
  128     32.3µs     32.5µs    127.9µs     56.3µs     27.9µs
  192     82.3µs     92.1µs    245.5µs    171.6µs     66.5µs
  256    115.8µs    132.4µs    431.1µs    395.9µs    136.6µs
  384      288µs    349.1µs    933.9µs      1.2ms    394.2µs
  512    625.4µs      460µs      3.2ms      3.3ms    783.5µs
  640      1.1ms    975.1µs      2.9ms      5.9ms      1.3ms
  768      1.9ms      1.1ms      4.3ms     11.1ms      2.1ms
  896      2.8ms      1.6ms      5.4ms     17.6ms      3.1ms
 1024      4.3ms      1.8ms     13.7ms     26.7ms      4.8ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      5.9µs      5.9µs      4.3µs      4.6µs      3.8µs
   64     26.9µs     26.8µs     15.6µs     17.6µs     13.8µs
   96     61.8µs     73.4µs       34µs     46.5µs     31.5µs
  128    114.9µs    141.2µs     64.1µs    104.1µs     91.1µs
  192    324.7µs    313.5µs    151.5µs    304.6µs    329.2µs
  256    611.4µs    668.1µs      361µs    719.3µs    656.8µs
  384      1.4ms      1.6ms    669.3µs      2.4ms      1.4ms
  512      2.9ms      2.5ms        1ms      6.2ms      3.4ms
  640      4.2ms      4.5ms      1.5ms     11.1ms      4.4ms
  768      7.2ms      6.9ms      2.6ms     19.4ms      6.7ms
  896      9.1ms      9.6ms      5.2ms     30.1ms        8ms
 1024     13.1ms     11.6ms      5.1ms     45.9ms     14.4ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     16.7µs     16.9µs          -     14.1µs     11.1µs
   64       67µs     67.1µs          -    100.6µs     64.4µs
   96    153.8µs    153.8µs          -    322.5µs    184.3µs
  128    297.3µs    299.1µs          -    767.2µs      413µs
  192    694.7µs      697µs          -      2.5ms      1.2ms
  256      1.3ms      1.3ms          -      5.9ms      2.8ms
  384      3.5ms      3.7ms          -     19.8ms        9ms
  512      8.2ms      7.1ms          -     47.4ms     22.1ms
  640     13.8ms     10.5ms          -     91.6ms     41.5ms
  768       23ms       16ms          -    158.7ms     72.2ms
  896     33.5ms     21.6ms          -    251.3ms    113.1ms
 1024     51.9ms     28.4ms          -    377.2ms    172.6ms
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     13.8µs     13.8µs     16.5µs      6.7µs      7.4µs
   64     38.3µs     38.1µs     63.9µs     34.3µs       40µs
   96       73µs       73µs    267.8µs    101.6µs     63.1µs
  128    120.1µs    120.1µs    666.6µs    222.9µs    110.2µs
  192    273.5µs    273.5µs      1.5ms    706.1µs    234.8µs
  256    511.3µs    618.2µs      2.8ms      1.7ms    461.9µs
  384      1.3ms      1.5ms      9.3ms      5.4ms      1.2ms
  512      2.8ms      2.7ms     12.4ms     12.3ms      2.4ms
  640      4.9ms      3.7ms     16.7ms     23.5ms      4.2ms
  768      7.8ms      5.4ms     28.8ms       40ms      6.8ms
  896     11.5ms      7.3ms     38.1ms     62.8ms     10.3ms
 1024     16.7ms       10ms     55.1ms       93ms       15ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32       34µs     45.7µs          -     16.5µs      8.6µs
   64    134.8µs    160.9µs          -    118.9µs     35.1µs
   96    313.2µs    357.5µs          -    383.6µs     75.3µs
  128    574.8µs    624.6µs          -    885.7µs    145.2µs
  192      1.3ms      1.4ms          -      2.9ms    390.2µs
  256      2.4ms      2.2ms          -      6.9ms    883.9µs
  384      6.1ms        4ms          -     22.8ms      2.7ms
  512     12.1ms      6.4ms          -     53.4ms      7.5ms
  640     20.9ms      9.5ms          -    103.9ms     14.6ms
  768       33ms     13.4ms          -    178.6ms     26.2ms
  896     49.2ms     18.1ms          -    283.1ms     41.3ms
 1024     68.9ms       22ms          -    421.8ms     65.8ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     15.3µs     34.5µs      8.8µs     19.2µs     10.9µs
   64     61.5µs     82.1µs     34.9µs     77.9µs     43.7µs
   96    148.2µs    160.3µs    177.9µs    202.1µs    101.3µs
  128    215.2µs    216.8µs    267.6µs    435.7µs    253.2µs
  192    584.7µs    547.4µs      594µs      1.3ms    725.3µs
  256        1ms    784.6µs      1.4ms      3.1ms      1.5ms
  384      2.5ms        2ms      1.9ms     10.1ms      3.5ms
  512      4.8ms      3.5ms      3.5ms     27.5ms      8.3ms
  640      9.2ms      5.4ms      5.2ms     51.8ms     13.1ms
  768     14.4ms      7.9ms      7.3ms     88.6ms     20.8ms
  896     21.2ms     11.3ms     10.9ms    135.8ms     28.3ms
 1024     30.6ms     18.1ms       15ms    206.8ms       45ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    148.6µs    181.7µs     76.5µs     79.9µs    139.1µs
   64    483.9µs    450.6µs      601µs    355.9µs    582.7µs
   96        1ms        1ms      1.4ms      1.1ms      1.5ms
  128      1.7ms      1.7ms      2.3ms      2.9ms      2.4ms
  192      3.9ms      3.9ms        5ms      8.2ms      5.5ms
  256      7.1ms      6.4ms      8.5ms       23ms      9.4ms
  384     17.2ms     13.1ms       18ms     75.8ms     22.2ms
  512     34.3ms     21.9ms     34.8ms    279.4ms       43ms
  640     57.3ms     34.3ms       51ms    352.1ms     68.4ms
  768     90.9ms     59.9ms     80.6ms    844.9ms    104.8ms
  896    132.9ms     84.6ms    108.8ms      1.01s    147.9ms
 1024    192.9ms    108.8ms    171.8ms       2.4s    223.5ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      1.1ms      1.3ms      4.5ms      2.6ms      1.2ms
   64      2.9ms        3ms     13.8ms      9.7ms      3.8ms
   96      5.4ms      5.3ms     26.9ms     20.7ms      8.5ms
  128      8.4ms      7.8ms     41.3ms     37.3ms     13.8ms
  192     16.3ms     14.1ms     58.2ms       86ms     27.4ms
  256     26.7ms     21.1ms     73.5ms      155ms     44.6ms
  384     57.2ms     37.1ms    104.5ms    364.4ms     92.7ms
  512     99.6ms     57.4ms    148.9ms    811.2ms    159.2ms
  640    154.5ms     81.2ms    191.5ms      1.19s    244.6ms
  768    225.2ms      130ms      253ms      2.16s    363.2ms
  896    308.9ms    170.3ms    324.6ms      2.97s      513ms
 1024    416.6ms    218.6ms    547.9ms      5.14s    715.4ms
```
