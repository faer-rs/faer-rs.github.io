---
layout: post
title:  "Benchmarks (c64)"
date:   2023-04-03 00:00:04 +0100
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
   32      4.1µs      4.1µs      5.4µs     35.7µs      4.8µs
   64     32.8µs     32.8µs     20.7µs    264.9µs     12.9µs
   96    108.1µs     31.5µs       36µs    870.8µs     28.3µs
  128      255µs     59.7µs     92.1µs        2ms    119.2µs
  192    851.1µs    178.5µs    214.5µs      6.7ms    177.4µs
  256        2ms      408µs    529.7µs     15.7ms    521.9µs
  384      6.8ms      1.3ms      1.6ms     52.5ms      1.3ms
  512     16.2ms      3.2ms      4.2ms      124ms      4.4ms
  640     32.1ms      6.3ms      7.9ms    243.2ms      7.1ms
  768     56.5ms     10.8ms     13.3ms    461.3ms       11ms
  896       88ms     16.9ms     21.5ms    819.8ms     21.1ms
 1024    135.5ms     27.1ms     31.2ms      1.29s     33.1ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.3µs      4.1µs     11.7µs     22.3µs      7.7µs
   64     23.7µs     23.8µs     44.7µs    148.2µs     43.6µs
   96     73.7µs     54.2µs       83µs    478.4µs    122.6µs
  128    162.7µs     98.3µs    194.8µs      1.1ms    279.8µs
  192    517.5µs    208.2µs    466.8µs      3.6ms    836.6µs
  256      1.2ms    433.2µs      1.1ms      8.5ms        2ms
  384      3.8ms    884.4µs      2.9ms     28.3ms      5.2ms
  512      8.7ms        2ms      6.5ms     66.2ms     12.4ms
  640     16.8ms      3.7ms     11.3ms      128ms       24ms
  768     29.2ms      6.7ms     18.5ms    220.4ms     39.3ms
  896     46.2ms     11.7ms       26ms    361.9ms     60.7ms
 1024     70.3ms     17.2ms     37.5ms    602.7ms     91.7ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.8µs       13µs      9.4µs     22.3µs      7.7µs
   64     17.9µs     31.1µs     40.4µs    149.1µs     43.7µs
   96     46.8µs     55.7µs    110.8µs    478.3µs    122.6µs
  128     83.8µs     88.2µs    192.1µs      1.1ms    279.6µs
  192    239.7µs    187.5µs    463.6µs      3.6ms    835.1µs
  256    501.6µs    324.5µs      1.1ms      8.5ms        2ms
  384      1.5ms    662.4µs      2.9ms     28.2ms      5.2ms
  512      3.3ms      1.2ms      6.4ms     65.9ms     12.3ms
  640      6.3ms      1.8ms     11.1ms    127.4ms     23.7ms
  768     10.8ms        3ms     18.3ms    220.6ms     38.7ms
  896     16.9ms      4.7ms     25.7ms    357.7ms     59.8ms
 1024     24.8ms      6.8ms     37.2ms    597.4ms     90.2ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.7µs      3.7µs        5µs      8.9µs     10.5µs
   64       17µs       17µs     47.1µs     53.9µs     43.7µs
   96     42.1µs     42.1µs       92µs    169.7µs    103.8µs
  128       85µs     85.1µs    180.6µs    388.7µs    182.7µs
  192      241µs    224.9µs    588.2µs      1.2ms    469.6µs
  256    526.1µs    389.4µs      1.2ms        3ms    891.9µs
  384      1.6ms        1ms      1.9ms      9.9ms      2.4ms
  512      3.5ms      1.9ms      4.7ms     22.9ms        5ms
  640      6.7ms      3.2ms        6ms     43.8ms      8.9ms
  768     11.8ms      4.9ms     11.3ms     74.8ms     14.7ms
  896       18ms      7.5ms     13.3ms    117.5ms     22.2ms
 1024     26.8ms      9.9ms     21.3ms    174.6ms     32.4ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      8.8µs      8.7µs      7.9µs     15.2µs     14.6µs
   64     44.1µs     42.5µs     33.5µs     97.9µs     80.9µs
   96    107.4µs    110.4µs     86.2µs      315µs    239.3µs
  128      225µs    226.2µs      267µs    733.4µs    520.4µs
  192    594.2µs    623.7µs    507.9µs      2.4ms      1.4ms
  256      1.3ms      1.2ms    803.5µs      5.9ms      2.7ms
  384      3.6ms      2.7ms      1.5ms     19.2ms      6.2ms
  512      7.8ms      5.4ms        5ms     45.3ms     11.7ms
  640     14.3ms      9.1ms      5.8ms       87ms     18.7ms
  768     24.1ms     14.3ms      9.1ms    154.4ms     28.2ms
  896     37.4ms     20.7ms     13.1ms    263.4ms     39.6ms
 1024     55.4ms     27.5ms     18.9ms    440.6ms     54.3ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     12.1µs       12µs          -     23.9µs    132.9µs
   64     64.7µs     64.7µs          -    187.7µs      1.2ms
   96    177.2µs    176.5µs          -      608µs      3.8ms
  128    395.2µs    394.9µs          -      1.4ms      8.9ms
  192      1.2ms      1.2ms          -      4.7ms     29.3ms
  256        3ms        3ms          -     11.2ms     69.3ms
  384      9.6ms      8.9ms          -       37ms    230.8ms
  512     21.7ms     13.5ms          -     87.3ms    543.4ms
  640     41.4ms     19.7ms          -      169ms      1.05s
  768     76.2ms     29.9ms          -    317.1ms      1.81s
  896    145.1ms     58.7ms          -    550.6ms       2.9s
 1024    264.5ms    127.1ms          -    880.5ms      4.36s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     21.1µs     21.2µs     31.9µs       28µs     16.2µs
   64     71.4µs     71.6µs    157.6µs    181.7µs    125.6µs
   96    166.8µs    166.5µs    985.2µs    586.4µs    235.6µs
  128    325.3µs    325.3µs      1.8ms      1.3ms      471µs
  192    899.1µs    899.9µs      3.9ms      4.4ms      1.2ms
  256      1.9ms      1.7ms      9.2ms     10.4ms      2.4ms
  384      5.8ms      3.6ms     20.4ms     34.7ms      6.8ms
  512     13.1ms      6.5ms     37.2ms     81.8ms     14.9ms
  640     24.9ms     10.7ms       57ms    159.5ms     28.6ms
  768     42.4ms     17.2ms     85.3ms    281.1ms     49.9ms
  896     66.6ms     25.3ms    117.5ms    467.3ms     79.4ms
 1024    100.1ms     36.6ms    193.1ms    737.2ms    118.2ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     35.6µs     51.9µs          -     36.3µs     21.8µs
   64    157.6µs    181.6µs          -    267.1µs     99.9µs
   96    390.9µs    449.6µs          -    873.4µs    272.7µs
  128    762.8µs    832.6µs          -        2ms    553.2µs
  192      2.1ms      2.1ms          -      6.6ms      1.6ms
  256      4.5ms        4ms          -     15.7ms      4.4ms
  384     13.6ms      7.3ms          -     52.4ms     14.3ms
  512     30.4ms     12.1ms          -    123.6ms     33.9ms
  640     56.9ms     18.6ms          -      241ms       61ms
  768    102.2ms       28ms          -    444.9ms    115.4ms
  896      177ms     51.9ms          -    747.6ms    196.8ms
 1024    301.1ms    115.2ms          -      1.16s    355.1ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     23.8µs     44.8µs     18.7µs     66.7µs     32.5µs
   64    102.7µs      117µs     89.3µs    420.6µs    180.3µs
   96    270.2µs    239.1µs    286.4µs      1.3ms    513.9µs
  128    527.7µs    429.7µs    637.3µs      3.1ms      1.2ms
  192      1.5ms        1ms      1.3ms      9.8ms      3.2ms
  256      3.2ms        2ms      2.3ms       24ms        7ms
  384      9.3ms      4.5ms      4.7ms     77.8ms     17.7ms
  512     21.2ms      9.9ms      9.9ms    181.5ms     39.1ms
  640     40.7ms     16.4ms     16.3ms    349.7ms     71.7ms
  768     68.4ms     26.4ms     26.3ms    610.6ms    113.8ms
  896      107ms     40.3ms     43.7ms         1s    170.4ms
 1024    155.9ms     74.6ms     58.5ms      1.67s    250.7ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    174.8µs    210.3µs    132.9µs    258.1µs    273.7µs
   64    674.2µs    677.6µs      1.1ms      1.6ms      1.4ms
   96      1.6ms      1.6ms      2.9ms        5ms      3.7ms
  128        3ms      2.9ms      4.8ms       12ms      6.1ms
  192      7.7ms        7ms     10.4ms     39.9ms     14.6ms
  256     16.1ms     11.6ms     19.1ms    116.3ms     28.5ms
  384     45.8ms     25.9ms     42.9ms    374.7ms     69.6ms
  512    100.6ms     47.4ms     83.2ms    922.2ms    142.3ms
  640    185.2ms     79.4ms    133.6ms       1.8s    242.1ms
  768    324.1ms    124.7ms    210.4ms      3.53s    406.4ms
  896    534.9ms    227.2ms    341.2ms       5.5s    620.6ms
 1024    837.1ms    342.1ms    514.2ms      9.78s    964.7ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.6ms      2.5ms     10.8ms     18.1ms      7.9ms
   64        9ms      7.9ms     27.2ms     72.8ms     24.1ms
   96     19.6ms     12.5ms       50ms    162.2ms     48.2ms
  128     33.8ms     19.2ms     83.4ms    297.8ms     79.5ms
  192     73.3ms     36.1ms    128.6ms    724.8ms    167.4ms
  256    128.5ms       57ms    311.2ms      1.39s    294.6ms
  384    288.5ms      108ms      368ms      3.28s    641.5ms
  512    517.9ms    176.2ms    709.7ms      5.95s      1.15s
  640    820.9ms    294.3ms    749.6ms      9.52s      1.79s
  768      1.24s      445ms      1.19s     14.28s      2.57s
  896      1.76s    610.9ms      1.23s     19.95s      3.49s
 1024       2.4s    825.9ms      1.85s      27.5s      4.56s
```
