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
   32      4.1µs      4.1µs      5.4µs     35.6µs      4.8µs
   64     32.6µs     32.6µs     17.6µs    262.3µs     12.9µs
   96    108.1µs     31.1µs     28.9µs    861.3µs     28.4µs
  128    254.5µs     59.4µs     88.3µs        2ms      120µs
  192    849.8µs    178.4µs    213.6µs      6.7ms    178.5µs
  256        2ms    409.8µs    543.4µs     15.7ms    523.6µs
  384      6.7ms      1.3ms      1.6ms     52.5ms      1.3ms
  512       16ms        3ms      4.2ms      124ms      4.4ms
  640     31.9ms      6.3ms        8ms    244.4ms      7.1ms
  768     55.4ms     10.5ms     13.3ms    461.1ms       11ms
  896     88.1ms     16.5ms     21.6ms    818.9ms     20.9ms
 1024    136.4ms     25.1ms     31.5ms      1.29s       33ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32        4µs      3.9µs     10.8µs     22.3µs      7.8µs
   64     23.7µs     23.7µs       46µs    144.8µs     43.8µs
   96     72.7µs     50.5µs     85.4µs    473.2µs    122.8µs
  128    162.1µs     94.5µs    195.6µs      1.1ms    277.3µs
  192    514.9µs    207.8µs    464.8µs      3.6ms    835.6µs
  256      1.2ms    430.2µs        1ms      8.5ms        2ms
  384      3.8ms    887.8µs      2.9ms     28.2ms      5.2ms
  512      8.7ms        2ms      6.5ms     65.8ms     12.2ms
  640     16.8ms      3.7ms     11.2ms    127.5ms     23.7ms
  768     29.2ms      6.5ms     18.3ms    219.9ms     38.3ms
  896     46.3ms     11.7ms     25.9ms    357.5ms     59.7ms
 1024       71ms     17.2ms     37.6ms    583.7ms     90.4ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32        6µs     13.3µs      8.8µs     22.2µs      7.8µs
   64     20.2µs     32.2µs       63µs    140.1µs     43.8µs
   96     51.3µs     57.5µs     85.7µs    473.1µs    122.9µs
  128     88.8µs     89.6µs    195.3µs      1.1ms    277.4µs
  192    250.1µs    188.2µs    464.1µs      3.6ms    835.7µs
  256    509.1µs    328.9µs        1ms      8.5ms        2ms
  384      1.5ms    684.8µs      2.9ms     28.2ms      5.2ms
  512      3.3ms      1.2ms      6.4ms     65.8ms     12.2ms
  640      6.3ms      1.8ms     11.2ms    127.6ms     23.7ms
  768     10.7ms        3ms     18.3ms    220.2ms     38.3ms
  896     16.8ms      4.8ms     25.9ms    361.2ms     59.8ms
 1024     24.7ms      6.7ms     37.3ms    587.3ms     90.1ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.9µs      3.9µs      4.8µs      7.9µs     10.6µs
   64     17.6µs     17.7µs     47.6µs     49.7µs     44.2µs
   96     43.3µs     43.5µs     93.4µs    159.8µs      105µs
  128     85.8µs     86.2µs    178.3µs    371.1µs    184.2µs
  192    241.3µs    225.9µs    535.1µs      1.2ms    470.7µs
  256    527.8µs    403.6µs      1.2ms        3ms    898.7µs
  384      1.6ms        1ms        2ms      9.6ms      2.4ms
  512      3.5ms      1.9ms      4.7ms     22.5ms        5ms
  640      6.7ms      3.2ms      5.9ms     43.4ms      8.9ms
  768     11.7ms      4.9ms     11.4ms     74.2ms     14.7ms
  896     17.9ms        8ms     13.4ms    116.8ms     22.3ms
 1024     26.7ms     10.4ms     21.4ms    173.9ms     32.5ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32        8µs        8µs      7.7µs     15.1µs     14.7µs
   64     37.1µs     37.4µs     33.7µs     97.9µs     81.5µs
   96     93.3µs     93.6µs     86.4µs    313.9µs      242µs
  128    187.8µs    188.6µs    277.3µs    733.3µs    519.7µs
  192    522.3µs    513.4µs    507.3µs      2.4ms      1.4ms
  256      1.1ms      958µs      820µs        6ms      2.7ms
  384      3.2ms      2.1ms      1.9ms       19ms      6.2ms
  512      7.1ms        4ms      4.9ms       45ms     11.6ms
  640     13.3ms      6.3ms      5.7ms     86.7ms     18.6ms
  768     22.5ms      9.8ms      8.9ms    155.1ms     28.2ms
  896     35.2ms     14.8ms     12.9ms      264ms     39.5ms
 1024     52.4ms     20.8ms     18.8ms    434.5ms     53.9ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      9.7µs      9.6µs          -     24.5µs    132.5µs
   64     51.8µs     52.1µs          -    186.4µs      1.2ms
   96    147.2µs    147.8µs          -      606µs      3.8ms
  128    334.6µs    334.1µs          -      1.4ms      8.8ms
  192      1.1ms      1.1ms          -      4.6ms     29.2ms
  256      2.8ms      2.8ms          -     11.3ms     68.9ms
  384      8.7ms      8.3ms          -     36.9ms      229ms
  512     19.7ms     12.6ms          -     87.2ms      537ms
  640     38.3ms     19.2ms          -      170ms      1.04s
  768     72.1ms     29.8ms          -    309.3ms       1.8s
  896    139.9ms     61.9ms          -    544.2ms      2.87s
 1024    248.8ms      126ms          -    862.8ms      4.31s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     20.7µs     20.7µs     31.4µs     27.9µs     16.3µs
   64     71.8µs     71.8µs    158.7µs    181.2µs    123.6µs
   96    168.9µs    168.9µs    978.2µs      586µs    234.8µs
  128    330.5µs    330.2µs      1.8ms      1.3ms    471.4µs
  192    914.1µs    914.8µs      3.9ms      4.4ms      1.2ms
  256      1.9ms      1.7ms      7.2ms     10.4ms      2.4ms
  384      5.9ms      3.6ms     20.9ms     34.7ms      6.9ms
  512     13.1ms      6.5ms     37.7ms     81.9ms     14.8ms
  640       25ms     10.8ms     56.8ms    159.6ms     28.7ms
  768     42.5ms     17.1ms     84.9ms    280.9ms     49.4ms
  896     66.6ms     25.2ms    116.6ms    467.4ms     79.2ms
 1024     99.8ms     37.2ms    191.3ms    731.1ms    117.1ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     25.9µs     35.8µs          -     36.4µs     22.5µs
   64    114.3µs    132.4µs          -    265.5µs      101µs
   96    296.5µs    338.1µs          -    874.6µs    270.2µs
  128      598µs    672.7µs          -        2ms    551.6µs
  192      1.7ms      1.8ms          -      6.6ms      1.7ms
  256      3.8ms      3.3ms          -     15.7ms      4.5ms
  384     11.5ms      6.5ms          -     52.3ms     12.8ms
  512     26.1ms     10.8ms          -    123.4ms       30ms
  640     52.8ms     17.3ms          -    243.5ms     54.7ms
  768       95ms     26.4ms          -    443.8ms    102.4ms
  896    167.1ms     50.3ms          -    744.3ms    181.4ms
 1024    279.6ms    109.5ms          -      1.16s    327.4ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32       27µs     46.8µs     17.7µs     66.1µs     32.7µs
   64    105.1µs    114.8µs     93.7µs    405.9µs    180.4µs
   96    267.1µs      222µs    286.5µs      1.3ms    515.5µs
  128    510.3µs    388.3µs      645µs        3ms      1.2ms
  192      1.4ms    949.5µs      1.3ms      9.8ms      3.2ms
  256        3ms      1.7ms      2.3ms     23.8ms        7ms
  384        9ms      3.9ms      4.7ms       77ms     17.7ms
  512     20.6ms      8.1ms      9.7ms    180.6ms     38.9ms
  640     39.6ms     14.2ms     16.2ms    347.9ms     71.2ms
  768     66.4ms     24.5ms     26.3ms    608.8ms    112.8ms
  896    103.6ms     48.1ms     39.9ms    999.2ms    168.8ms
 1024    152.1ms     55.7ms     58.5ms      1.66s    247.7ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    144.9µs    169.1µs    133.3µs      246µs    276.2µs
   64    565.1µs    570.3µs      1.1ms      1.6ms      1.4ms
   96      1.4ms      1.4ms      2.9ms        5ms      3.6ms
  128      2.6ms      2.6ms      4.8ms     12.6ms        6ms
  192      6.9ms      6.5ms     10.5ms     39.6ms     14.8ms
  256     14.6ms     10.8ms     19.3ms      118ms     28.8ms
  384     42.7ms     24.4ms     43.1ms    376.4ms     70.7ms
  512     94.8ms     44.9ms     82.8ms    922.8ms      141ms
  640    176.7ms     75.9ms    133.9ms      1.84s    242.7ms
  768    308.9ms    121.9ms    210.9ms      3.62s    406.1ms
  896      511ms    209.3ms    339.4ms       5.6s    618.8ms
 1024    805.5ms    337.3ms    514.3ms     10.23s    949.4ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.6ms      2.5ms     10.8ms     18.3ms      8.2ms
   64        9ms      7.7ms       27ms     72.5ms     25.2ms
   96     19.4ms     11.7ms     49.6ms    163.2ms     51.6ms
  128     33.5ms     18.1ms     81.1ms    303.6ms     83.9ms
  192       73ms     35.3ms    127.3ms    718.8ms    171.1ms
  256    128.1ms     56.8ms    310.9ms      1.39s    298.4ms
  384    284.8ms    107.6ms    366.1ms      3.28s    649.7ms
  512    511.7ms    236.3ms    714.3ms      5.97s      1.16s
  640    811.3ms    309.9ms    749.8ms      9.58s      1.81s
  768      1.23s    440.6ms      1.19s     14.42s      2.58s
  896      1.74s    608.2ms      1.23s     19.98s      3.51s
 1024      2.38s    823.2ms      1.85s     27.78s      4.58s
```

## Hermitian matrix eigenvalue decomposition

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     70.3µs     74.5µs    179.7µs    122.5µs     85.7µs
   64    308.1µs      298µs      1.5ms      889µs    431.9µs
   96    765.4µs    730.6µs      6.7ms      2.7ms      1.2ms
  128      1.4ms      1.3ms     15.4ms      6.2ms      2.4ms
  192      3.6ms      3.5ms       46ms     19.9ms      6.7ms
  256      7.2ms        6ms    105.7ms     46.5ms     15.6ms
  384     20.3ms     13.9ms      239ms    151.4ms     46.9ms
  512     43.8ms       26ms    475.2ms    354.2ms    107.1ms
  640     80.4ms     42.5ms    870.7ms    682.7ms    201.6ms
  768    134.5ms     64.6ms      1.55s       1.2s    365.8ms
  896    208.3ms     91.2ms      2.31s      1.95s    664.8ms
 1024    309.3ms    129.5ms      3.74s      3.07s      1.21s
```

## Non Hermitian matrix eigenvalue decomposition

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    254.3µs    254.4µs    568.7µs          -      1.7ms
   64      1.5ms      1.5ms      5.8ms          -     11.2ms
   96      5.5ms        6ms     19.7ms          -       38ms
  128     10.9ms     11.3ms       38ms          -     83.3ms
  192     31.4ms     31.3ms     62.2ms          -    271.3ms
  256     58.8ms       58ms    114.7ms          -    672.9ms
  384    147.4ms    139.3ms      242ms          -      2.25s
  512    308.2ms    250.2ms    593.1ms          -      5.62s
  640      580ms    457.5ms    916.1ms          -     10.74s
  768    954.2ms    738.4ms      1.28s          -     19.61s
  896      1.42s      1.04s       1.7s          -     29.91s
 1024      2.16s      1.57s      2.19s          -     48.13s
```
