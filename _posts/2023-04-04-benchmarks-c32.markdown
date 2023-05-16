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
    4       84ns       84ns      143ns       92ns       26ns
    8      108ns      114ns      206ns      298ns      112ns
   16      320ns      327ns      655ns        2µs      387ns
   32      2.1µs      2.1µs      3.1µs     15.6µs      2.4µs
   64     16.4µs     16.4µs       16µs    124.1µs      8.5µs
   96     53.6µs     17.5µs     17.5µs    454.3µs     15.8µs
  128      127µs     34.2µs     50.2µs      1.1ms     61.5µs
  192    423.8µs     94.7µs     91.8µs      3.5ms     91.8µs
  256        1ms    209.1µs    314.7µs      8.1ms    261.5µs
  384      3.4ms    635.6µs    778.3µs     26.8ms      649µs
  512      7.9ms      1.5ms      2.1ms     62.8ms      2.1ms
  640     15.6ms        3ms      4.1ms    122.1ms      3.3ms
  768     27.1ms      5.2ms      6.3ms    210.3ms      5.1ms
  896     43.3ms      8.4ms     11.1ms      337ms       10ms
 1024     65.1ms     12.2ms     16.4ms    527.4ms     15.1ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       31ns       30ns      780ns      102ns      112ns
    8      271ns      271ns      1.5µs      587ns      614ns
   16      1.1µs      1.2µs      4.2µs      2.8µs      2.6µs
   32      3.5µs      3.5µs     10.6µs       14µs     10.9µs
   64     16.7µs     16.8µs     32.6µs     83.9µs     48.4µs
   96     48.5µs     34.6µs     68.4µs    259.1µs    120.7µs
  128    101.7µs     60.6µs    168.5µs    606.4µs    237.3µs
  192    303.5µs      141µs    331.4µs        2ms    633.5µs
  256    667.7µs    268.5µs    748.7µs      4.6ms      1.3ms
  384      2.1ms      525µs      1.8ms     15.3ms      3.8ms
  512      4.7ms      1.1ms      4.3ms     36.2ms      7.5ms
  640      8.9ms      2.1ms        7ms     68.4ms     13.6ms
  768     15.1ms      3.3ms     11.7ms    116.9ms     22.8ms
  896     23.5ms      5.9ms     16.8ms    183.9ms     35.4ms
 1024     35.3ms        9ms     24.4ms      274ms       49ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      170ns      5.3µs      808ns      102ns      112ns
    8      601ns      6.3µs      1.4µs      587ns      614ns
   16      2.1µs      8.6µs      4.1µs      2.8µs      2.6µs
   32        6µs     14.1µs     10.6µs       14µs     10.9µs
   64     18.4µs     30.4µs     32.5µs     83.9µs     48.5µs
   96     44.7µs     50.4µs     68.2µs    259.1µs    120.7µs
  128     66.5µs       69µs    168.1µs    604.4µs    237.2µs
  192    175.3µs    127.7µs    330.5µs        2ms    633.3µs
  256      320µs    209.2µs    755.2µs      4.6ms      1.3ms
  384    919.8µs    433.9µs      1.8ms     15.3ms      3.8ms
  512      1.9ms      739µs      4.3ms     36.2ms      7.5ms
  640      3.5ms      1.1ms        7ms     68.4ms     13.6ms
  768      5.8ms      1.5ms     11.6ms      117ms     22.8ms
  896      8.9ms      2.5ms     16.7ms    184.1ms     35.4ms
 1024     13.1ms        4ms     24.3ms    274.2ms       49ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       52ns       52ns      164ns      123ns      134ns
    8      142ns      141ns      373ns      283ns      495ns
   16      629ns      627ns      1.2µs        1µs      1.6µs
   32      2.7µs      2.8µs      4.4µs      4.9µs     10.9µs
   64       11µs     11.1µs     45.6µs     29.1µs     43.7µs
   96     31.5µs     31.7µs     84.4µs     88.4µs    100.1µs
  128     50.6µs       51µs    135.6µs    204.5µs    171.9µs
  192    157.8µs      160µs    257.9µs    662.4µs    414.9µs
  256    294.7µs      237µs    975.4µs      1.5ms    764.3µs
  384    871.5µs    637.6µs      1.3ms      5.1ms      1.9ms
  512      2.1ms      1.1ms        4ms     12.5ms      3.9ms
  640      3.6ms      1.9ms      3.9ms     23.6ms      6.4ms
  768      6.3ms      2.8ms      6.2ms     40.5ms     10.2ms
  896      9.7ms      4.2ms      8.1ms     63.4ms     14.8ms
 1024     15.1ms      5.2ms     16.3ms       94ms     21.3ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      138ns      137ns      209ns      108ns      353ns
    8      299ns      300ns      512ns      343ns      1.6µs
   16      900ns      874ns      1.9µs      1.6µs      5.9µs
   32      3.9µs      3.9µs      6.8µs      8.4µs     21.5µs
   64     17.7µs     17.7µs     27.3µs     53.4µs     81.9µs
   96     43.5µs     43.3µs     66.9µs    173.8µs    185.9µs
  128     89.8µs       90µs    235.9µs    408.9µs    389.1µs
  192    254.8µs    254.4µs    415.7µs      1.3ms      1.1ms
  256    550.7µs    474.6µs    631.1µs      3.1ms        2ms
  384      1.6ms      1.1ms      1.5ms       10ms      4.4ms
  512      3.6ms        2ms      3.5ms     24.4ms      8.4ms
  640      6.6ms        3ms      3.9ms     45.7ms     12.9ms
  768       11ms      4.5ms      5.7ms     78.9ms     18.9ms
  896       17ms      6.6ms      7.9ms    124.1ms     25.6ms
 1024     25.4ms      9.2ms     10.9ms    189.2ms     35.2ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      274ns      267ns          -      135ns      922ns
    8      667ns      655ns          -      546ns      5.1µs
   16      2.5µs      2.6µs          -        3µs     30.3µs
   32      8.2µs      8.2µs          -     18.9µs    206.9µs
   64     35.3µs     35.2µs          -      137µs      1.5ms
   96     92.2µs     92.4µs          -    455.1µs      4.8ms
  128    197.7µs    197.3µs          -      1.1ms     11.3ms
  192    580.4µs    584.1µs          -      3.5ms     37.9ms
  256      1.4ms      1.4ms          -      8.3ms     89.5ms
  384      4.4ms      4.4ms          -     27.5ms    297.6ms
  512     11.4ms      8.6ms          -     66.2ms    700.8ms
  640     20.1ms     12.5ms          -    126.6ms      1.36s
  768     35.2ms     18.3ms          -    218.7ms      2.34s
  896     53.8ms     25.9ms          -      346ms      3.71s
 1024     87.1ms     36.5ms          -    533.9ms      5.54s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      189ns      190ns      1.1µs      213ns      455ns
    8      597ns      598ns      2.6µs      642ns      1.7µs
   16      2.4µs      2.4µs      7.8µs      3.3µs      5.6µs
   32     10.1µs     10.1µs     32.7µs     19.9µs     19.4µs
   64     48.6µs     48.6µs    254.9µs    135.8µs    103.1µs
   96    108.7µs    108.7µs    611.2µs    439.4µs    177.6µs
  128    203.6µs    203.6µs      1.1ms        1ms    320.2µs
  192    554.3µs    554.3µs      2.6ms      3.3ms    728.2µs
  256      1.2ms      1.1ms      4.5ms      7.7ms      1.4ms
  384      3.4ms      2.4ms     16.3ms     25.6ms      3.7ms
  512      7.5ms      4.3ms     25.2ms     60.7ms      7.9ms
  640     14.5ms      6.8ms     36.2ms    117.5ms     14.3ms
  768     24.2ms     10.2ms     55.3ms    202.3ms     23.7ms
  896     37.5ms     14.6ms     73.6ms      321ms     36.8ms
 1024     55.2ms     20.7ms    106.7ms    482.4ms     54.7ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      265ns      257ns          -      242ns      548ns
    8      969ns      976ns          -      873ns        2µs
   16      3.5µs      3.5µs          -      4.9µs      6.6µs
   32     13.5µs     13.6µs          -     30.7µs     23.2µs
   64       71µs     84.2µs          -    219.6µs     92.1µs
   96    175.4µs    194.3µs          -    716.7µs    220.9µs
  128    352.5µs    401.4µs          -      1.7ms    424.5µs
  192    948.5µs        1ms          -      5.5ms      1.1ms
  256        2ms      1.9ms          -     12.9ms      2.4ms
  384      5.9ms        4ms          -       43ms        7ms
  512     13.2ms      7.2ms          -    101.8ms     17.3ms
  640       25ms     11.3ms          -    197.8ms     29.4ms
  768     42.5ms       16ms          -      341ms       50ms
  896     69.8ms     22.1ms          -    541.4ms     76.3ms
 1024    107.8ms     52.8ms          -      816ms    121.2ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      926ns      7.3µs      610ns      250ns      821ns
    8      2.5µs      9.2µs      1.2µs      1.6µs      3.3µs
   16      6.4µs     13.3µs      3.9µs        8µs     12.3µs
   32     20.8µs     35.8µs     15.5µs     40.8µs     45.4µs
   64     72.6µs     85.6µs     71.6µs    247.4µs    186.1µs
   96    187.4µs    157.3µs      219µs    756.2µs      446µs
  128    305.7µs      244µs    465.5µs      1.7ms      955µs
  192      868µs    576.9µs      970µs      5.5ms      2.5ms
  256      1.7ms    968.7µs      1.8ms     12.9ms      4.9ms
  384        5ms      2.1ms      3.3ms     42.2ms     12.8ms
  512     10.8ms        4ms      6.2ms     99.1ms     25.5ms
  640     20.3ms        8ms       10ms    186.6ms     43.6ms
  768     34.1ms     13.1ms       15ms    318.5ms     69.7ms
  896     52.8ms     19.6ms     22.1ms    499.4ms    102.8ms
 1024     77.5ms     27.5ms       32ms    758.5ms    141.9ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      2.9µs      2.8µs      3.8µs      1.6µs      7.2µs
    8     10.7µs     24.8µs      9.2µs      4.5µs       56µs
   16     35.4µs     62.9µs     30.4µs       23µs     55.3µs
   32    126.5µs      157µs    118.9µs    130.1µs    209.3µs
   64    438.8µs    437.4µs      812µs    907.3µs    842.8µs
   96    977.7µs        1ms      1.9ms      2.9ms      2.3ms
  128      1.8ms      1.8ms      3.3ms      6.9ms      3.7ms
  192      4.4ms      4.5ms      7.1ms     22.3ms      8.5ms
  256      8.8ms      7.4ms     12.8ms     60.4ms     15.7ms
  384     24.6ms     15.5ms     28.3ms    190.4ms     39.3ms
  512     54.1ms     28.4ms     57.3ms    542.8ms       81ms
  640     98.9ms     45.4ms     87.6ms    953.4ms      132ms
  768    164.6ms     79.9ms      135ms      1.79s    210.9ms
  896    250.2ms    122.8ms    187.2ms      2.83s    304.1ms
 1024    378.1ms    164.5ms    276.4ms      4.71s    458.6ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     91.2µs     91.5µs    454.2µs    199.2µs      1.1ms
    8    239.1µs    258.9µs      1.2ms    698.4µs      6.4ms
   16    635.6µs    719.2µs        3ms      2.7ms        1ms
   32      1.8ms      1.8ms      8.3ms     10.2ms      3.6ms
   64      5.4ms      4.9ms     20.6ms     40.2ms     10.7ms
   96     11.4ms      7.6ms       36ms     90.1ms     23.7ms
  128     19.2ms     11.3ms     56.6ms    159.1ms     38.6ms
  192     40.8ms     21.2ms     84.1ms    358.5ms     76.6ms
  256     70.9ms       33ms    125.8ms    667.2ms    126.4ms
  384    152.8ms     64.3ms    200.5ms      1.57s    276.1ms
  512    274.9ms    104.5ms    470.9ms      3.03s    500.5ms
  640      431ms    154.2ms    467.6ms      4.85s    791.9ms
  768    629.7ms    257.1ms    622.1ms      7.34s      1.17s
  896    871.6ms    339.5ms    768.3ms     10.22s      1.59s
 1024      1.19s    436.7ms      1.19s     14.31s       2.1s
```

## Hermitian matrix eigenvalue decomposition

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.3µs      1.3µs      2.1µs      835ns      1.4µs
    8      3.8µs      4.1µs      5.9µs      3.1µs      5.4µs
   16     13.5µs     15.5µs     29.1µs     14.2µs     20.1µs
   32     53.5µs     54.8µs    155.6µs     79.9µs       83µs
   64    211.8µs      214µs      1.4ms    504.7µs    347.7µs
   96    489.3µs    541.1µs      5.2ms      1.6ms    843.2µs
  128      881µs    904.9µs     11.2ms      3.5ms      1.7ms
  192      2.2ms      2.2ms     36.3ms     11.1ms      4.4ms
  256      4.3ms      3.9ms       84ms     25.4ms        9ms
  384     11.6ms        9ms    199.4ms     83.2ms     26.2ms
  512     24.7ms     15.6ms    396.1ms    196.5ms     59.3ms
  640     45.4ms     26.7ms    729.2ms      376ms      106ms
  768     74.2ms     39.3ms      1.22s    643.5ms    178.9ms
  896    113.1ms       55ms      1.91s      1.01s    270.8ms
 1024    165.3ms     79.8ms      2.82s      1.53s    418.8ms
```

## Non Hermitian matrix eigenvalue decomposition

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      5.3µs      5.3µs      7.1µs          -      5.6µs
    8     16.9µs     16.4µs     19.8µs          -     29.8µs
   16       57µs     54.3µs     86.2µs          -      167µs
   32    251.3µs    262.5µs    440.2µs          -      1.1ms
   64        1ms        1ms      4.2ms          -      8.5ms
   96      3.5ms      3.8ms     10.8ms          -       31ms
  128      6.6ms      7.1ms     20.9ms          -     64.4ms
  192     18.5ms     18.9ms     34.8ms          -    201.5ms
  256     32.3ms     33.6ms    105.7ms          -    470.6ms
  384     77.5ms     77.6ms    154.6ms          -      1.58s
  512    175.9ms    159.8ms    400.6ms          -      3.99s
  640    335.3ms    317.3ms    594.7ms          -      7.47s
  768    537.4ms    470.1ms    837.2ms          -     13.47s
  896    751.1ms    641.1ms      1.07s          -     21.18s
 1024      1.16s    938.5ms      1.26s          -      32.3s
```
