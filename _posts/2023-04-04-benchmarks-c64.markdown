---
layout: post
title:  "Benchmarks (c64)"
date:   2023-04-03 00:00:12 +0100
permalink: /bench-c64/
---

The benchmarks were run on an `11th Gen Intel(R) Core(TM) i5-11400 @ 2.60GHz` with 12 threads.  
- `nalgebra` is used with the `matrixmultiply` backend
- `ndarray` is used with the `openblas` backend
- `eigen` is compiled with `-march=native -O3 -fopenmp`

All computations are done on column major matrices containing `c64` values.

## Matrix multiplication

Multiplication of two square matrices of dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       79ns       79ns      155ns       94ns       25ns
    8      129ns      133ns      244ns      514ns      147ns
   16      592ns      599ns      929ns      3.8µs      679ns
   32      4.1µs      4.1µs      5.4µs     30.8µs      4.8µs
   64     32.6µs     32.7µs     16.7µs    261.9µs     13.7µs
   96      108µs     31.1µs     28.5µs    859.6µs     28.4µs
  128      255µs     59.3µs     72.2µs        2ms    119.1µs
  192    850.2µs    177.6µs    207.2µs      6.7ms    178.7µs
  256        2ms    411.7µs    542.1µs     15.7ms    523.3µs
  384      6.7ms      1.3ms      1.6ms     52.5ms      1.3ms
  512       16ms        3ms      4.2ms      124ms      4.4ms
  640       32ms      6.3ms        8ms    246.8ms      7.1ms
  768     55.4ms     10.5ms     13.2ms    459.2ms       11ms
  896       88ms     16.4ms     21.5ms    819.4ms     20.9ms
 1024    135.9ms     25.2ms     31.2ms      1.29s     29.3ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       31ns       29ns      761ns      100ns      106ns
    8      264ns      264ns      1.6µs      590ns      390ns
   16      1.2µs      1.2µs      4.8µs      3.3µs      1.6µs
   32      3.9µs      3.9µs     11.9µs       20µs      7.7µs
   64     23.7µs     23.7µs     46.1µs    143.2µs     43.6µs
   96     72.7µs       51µs     83.9µs    466.6µs    122.6µs
  128    163.2µs       94µs    192.1µs      1.1ms    277.4µs
  192    516.1µs    208.4µs    460.7µs      3.6ms    835.3µs
  256      1.2ms    425.6µs      1.1ms      8.5ms        2ms
  384      3.8ms      888µs      2.9ms     28.2ms      5.2ms
  512      8.8ms        2ms      6.4ms       66ms     12.2ms
  640     16.8ms      3.7ms     11.1ms    127.5ms     23.7ms
  768     29.2ms      6.2ms     18.4ms    220.2ms     38.4ms
  896     46.3ms     10.2ms     25.8ms    358.4ms     59.6ms
 1024     70.5ms     17.1ms     37.1ms    570.4ms     90.6ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      172ns      5.1µs      777ns      100ns      107ns
    8      609ns      6.2µs      1.9µs      590ns      393ns
   16      2.1µs      8.4µs      4.8µs      3.3µs      1.6µs
   32      6.4µs     13.6µs     11.7µs       20µs      7.7µs
   64       21µs       33µs     46.7µs    142.5µs     43.6µs
   96     53.2µs     58.7µs     84.8µs    465.8µs    122.7µs
  128     90.4µs     89.1µs      194µs      1.1ms    277.2µs
  192    254.4µs    185.5µs    463.5µs      3.6ms      835µs
  256    512.2µs    327.5µs      1.1ms      8.6ms        2ms
  384      1.5ms    679.5µs      2.9ms     28.4ms      5.2ms
  512      3.4ms      1.2ms      6.5ms     66.2ms     12.3ms
  640      6.3ms      1.8ms     11.2ms    127.8ms     23.8ms
  768     10.7ms        3ms     18.3ms    221.6ms     38.5ms
  896     16.9ms      4.9ms     25.8ms    360.2ms       60ms
 1024     24.6ms      7.7ms     37.2ms    570.6ms     90.3ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       56ns       56ns      163ns      121ns      142ns
    8      151ns      151ns      373ns      282ns      466ns
   16      513ns      513ns      1.2µs      1.2µs      1.6µs
   32      3.3µs      3.3µs      4.9µs        7µs     10.6µs
   64     15.9µs       16µs     48.6µs     49.6µs     44.1µs
   96     38.9µs       39µs     95.9µs    159.4µs    104.9µs
  128     83.9µs     84.2µs    181.1µs    370.3µs    183.6µs
  192    232.9µs    221.7µs    483.8µs      1.2ms    471.2µs
  256      524µs    383.9µs      1.6ms        3ms    899.6µs
  384      1.6ms    963.2µs      2.1ms      9.7ms      2.4ms
  512      3.5ms      1.9ms      4.5ms     22.7ms        5ms
  640      6.7ms      3.3ms      5.6ms     43.6ms      8.9ms
  768     11.8ms      4.8ms     10.8ms     74.5ms     14.7ms
  896       18ms      7.9ms     12.6ms    117.2ms     22.2ms
 1024     26.7ms     10.3ms     20.6ms    173.9ms     32.3ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      118ns      119ns      219ns      127ns      355ns
    8      304ns      286ns      542ns      376ns      1.1µs
   16      940ns      927ns      2.1µs        2µs      3.9µs
   32      4.7µs      4.6µs      7.8µs     13.1µs     14.8µs
   64     23.3µs     23.2µs     33.7µs     97.5µs     81.4µs
   96     64.6µs     64.3µs     86.5µs    314.1µs    241.8µs
  128    139.5µs    139.1µs    271.1µs    736.3µs    520.5µs
  192      414µs    393.7µs    510.9µs      2.4ms      1.4ms
  256    936.9µs    753.2µs    815.5µs        6ms      2.7ms
  384      2.8ms      1.6ms      1.8ms     19.3ms      6.2ms
  512      6.4ms      3.2ms      3.7ms     45.5ms     11.6ms
  640     12.2ms      5.1ms      5.8ms     87.1ms     18.7ms
  768     21.1ms        8ms        9ms    155.2ms     28.2ms
  896     33.4ms     12.1ms     13.1ms    262.8ms     39.6ms
 1024       50ms     17.6ms     18.9ms    434.6ms     54.1ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      236ns      256ns          -      138ns      532ns
    8      834ns      818ns          -      573ns      2.2µs
   16      2.4µs      2.5µs          -      3.4µs     11.2µs
   32      9.7µs      9.8µs          -     24.6µs    135.7µs
   64     51.5µs     51.8µs          -    187.2µs      1.2ms
   96    148.5µs    148.2µs          -    607.5µs      3.8ms
  128    334.1µs    336.5µs          -      1.4ms      8.8ms
  192      1.1ms      1.1ms          -      4.6ms     29.2ms
  256      2.9ms      2.9ms          -     11.3ms     68.9ms
  384      9.3ms      8.5ms          -       37ms    228.5ms
  512     21.7ms       13ms          -     87.5ms    537.2ms
  640     41.1ms     19.5ms          -    171.2ms      1.04s
  768     76.7ms     30.7ms          -    315.2ms       1.8s
  896    141.9ms     60.4ms          -    543.3ms      2.88s
 1024    256.9ms    128.2ms          -    869.9ms      4.32s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      192ns      191ns      1.2µs      295ns      484ns
    8      541ns      541ns      2.7µs      855ns      1.4µs
   16      1.9µs      1.9µs      8.2µs      4.2µs      4.3µs
   32      9.2µs      9.2µs     31.7µs     25.6µs     16.2µs
   64     61.3µs     61.2µs    159.7µs    183.3µs    124.7µs
   96    152.3µs    152.2µs        1ms    591.1µs    235.5µs
  128    304.9µs    304.8µs      1.8ms      1.4ms    470.9µs
  192    871.3µs    870.9µs      3.8ms      4.5ms      1.2ms
  256      1.9ms      1.7ms      7.2ms     10.5ms      2.4ms
  384      5.7ms      3.5ms     20.4ms       35ms      6.8ms
  512       13ms      6.3ms     38.4ms     82.7ms     14.9ms
  640     24.7ms     10.5ms     57.4ms      161ms     28.1ms
  768     42.1ms     16.7ms     86.1ms    282.9ms     48.6ms
  896     66.2ms     24.8ms    118.3ms    470.7ms     77.3ms
 1024     99.2ms     36.6ms      196ms    737.6ms    115.6ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      249ns      249ns          -      316ns      608ns
    8      671ns      684ns          -      1.1µs      1.8µs
   16      2.4µs      2.4µs          -      5.6µs        6µs
   32     11.1µs     11.1µs          -     36.4µs     22.5µs
   64     81.4µs     94.6µs          -    266.2µs     99.8µs
   96    223.1µs    237.1µs          -    878.9µs    269.8µs
  128    456.2µs    515.3µs          -        2ms    578.2µs
  192      1.4ms      1.5ms          -      6.7ms      1.7ms
  256      3.2ms      2.9ms          -     15.7ms      4.3ms
  384     10.2ms      6.1ms          -     52.4ms     12.7ms
  512     24.3ms     10.2ms          -    123.8ms     29.7ms
  640     48.9ms     16.5ms          -    242.7ms     54.5ms
  768     89.7ms     25.2ms          -      443ms    100.3ms
  896    159.1ms     50.6ms          -      742ms    181.8ms
 1024    269.7ms    111.1ms          -      1.15s    322.3ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      997ns      7.3µs      624ns      255ns      790ns
    8      2.6µs      9.4µs      1.3µs      1.7µs      2.4µs
   16      6.9µs     14.1µs      4.4µs      9.7µs        8µs
   32     23.9µs     40.1µs     17.7µs     58.3µs     32.8µs
   64     92.4µs     98.7µs     93.7µs    406.9µs    180.3µs
   96    241.8µs    195.2µs    265.4µs      1.3ms    513.4µs
  128      467µs    348.8µs    565.7µs        3ms      1.2ms
  192      1.3ms    832.5µs      1.3ms      9.8ms      3.2ms
  256      2.9ms      1.5ms      2.4ms     23.8ms      6.9ms
  384      8.7ms      3.5ms      4.8ms       77ms     17.6ms
  512     19.9ms      7.3ms      9.9ms    180.6ms     38.9ms
  640     38.7ms     14.7ms     16.2ms    348.3ms     70.9ms
  768     65.2ms     24.8ms     26.3ms    608.1ms    112.2ms
  896      102ms     37.7ms     39.3ms      1.01s    168.6ms
 1024    149.6ms     53.2ms     58.2ms      1.64s    247.2ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      3.2µs      3.4µs        4µs      2.1µs      7.6µs
    8     11.1µs     24.8µs       10µs      7.2µs     63.3µs
   16     36.4µs     64.3µs     35.1µs     35.1µs     64.6µs
   32    135.3µs    158.9µs    131.1µs    219.2µs    281.6µs
   64    553.5µs    559.3µs    954.4µs      1.7ms      1.4ms
   96      1.3ms      1.4ms      2.7ms      5.1ms      3.6ms
  128      2.6ms      2.5ms      4.8ms     12.6ms      6.2ms
  192      6.9ms      6.5ms     10.5ms     40.7ms     14.8ms
  256     14.5ms     10.9ms     19.4ms    119.8ms     29.5ms
  384     42.7ms     24.7ms       43ms    380.6ms     72.3ms
  512     95.1ms     45.7ms     83.3ms    939.8ms    144.1ms
  640    175.2ms     75.9ms    133.7ms      1.85s    248.1ms
  768    308.1ms      123ms    210.5ms      3.54s    418.3ms
  896    514.6ms    221.6ms    342.9ms      5.58s      631ms
 1024    809.7ms    337.7ms    512.3ms      9.93s      953ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       83µs     82.8µs    849.9µs      371µs      1.6ms
    8    277.9µs    286.4µs        2ms      1.3ms      7.5ms
   16    827.1µs    863.9µs      5.1ms      4.6ms      2.6ms
   32      2.5ms      2.4ms       11ms     18.4ms      8.3ms
   64      8.8ms      7.5ms     26.8ms     73.5ms     25.3ms
   96     19.2ms     11.3ms     49.5ms      168ms     51.1ms
  128     33.2ms     17.7ms     82.4ms    300.5ms     85.7ms
  192     72.6ms     34.8ms    130.4ms    733.4ms    171.9ms
  256    127.9ms       56ms    313.3ms      1.41s      299ms
  384    284.5ms    106.4ms    370.6ms      3.31s    650.7ms
  512    511.6ms    174.1ms    708.4ms      6.13s      1.16s
  640    811.5ms    296.2ms    749.1ms      9.82s      1.81s
  768      1.23s    439.5ms      1.19s     14.77s      2.58s
  896      1.74s    606.7ms      1.25s     20.51s       3.5s
 1024      2.39s    817.4ms      1.85s     28.37s      4.57s
```

## Hermitian matrix eigenvalue decomposition

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.5µs      1.5µs      2.3µs      918ns      1.4µs
    8      4.3µs      4.4µs      6.6µs      4.4µs      5.1µs
   16     16.4µs     15.5µs     33.9µs     20.9µs     20.1µs
   32     62.7µs     61.5µs    183.1µs    130.5µs     88.2µs
   64    296.5µs    285.8µs      1.7ms    861.2µs    437.1µs
   96    719.7µs      720µs      5.9ms      2.7ms      1.1ms
  128      1.4ms      1.4ms     14.9ms      6.2ms      2.4ms
  192      3.5ms      3.5ms     45.6ms     19.7ms      6.6ms
  256      7.2ms        6ms    104.8ms     46.3ms     15.2ms
  384     20.1ms       14ms      248ms    150.7ms       47ms
  512     43.8ms       26ms    498.2ms    355.4ms    106.4ms
  640     80.2ms     42.4ms    862.6ms      681ms    200.1ms
  768    134.4ms     64.6ms      1.47s       1.2s    365.9ms
  896    208.8ms       92ms      2.31s      1.95s    676.8ms
 1024    310.7ms    124.7ms      3.73s      3.05s      1.21s
```

## Non Hermitian matrix eigenvalue decomposition

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      6.2µs      6.2µs      6.6µs          -      6.8µs
    8       20µs     20.7µs     25.6µs          -     35.8µs
   16     69.1µs     71.8µs    109.6µs          -    237.4µs
   32    337.3µs    322.4µs      580µs          -      1.5ms
   64      1.5ms      1.5ms      5.8ms          -     11.4ms
   96      5.3ms      5.7ms     15.5ms          -     38.6ms
  128     10.7ms     11.9ms     31.1ms          -     83.7ms
  192     32.7ms     30.8ms     80.8ms          -    266.7ms
  256     58.6ms       59ms    112.2ms          -    684.5ms
  384    146.2ms    133.9ms    253.2ms          -      2.26s
  512    304.8ms    251.1ms    569.7ms          -      5.59s
  640    572.1ms    453.7ms    937.1ms          -     10.69s
  768    948.5ms      717ms      1.29s          -     19.61s
  896      1.39s      1.02s      1.64s          -     29.92s
 1024      2.13s      1.56s      2.16s          -     48.49s
```
