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
   32      4.1µs      4.1µs      5.5µs     35.5µs      4.8µs
   64     32.7µs     32.7µs     20.4µs    265.8µs     13.1µs
   96    107.8µs     31.3µs     36.9µs      860µs     28.4µs
  128    254.5µs     59.2µs     93.1µs        2ms    120.2µs
  192      850µs    176.1µs    213.6µs      6.7ms    177.9µs
  256        2ms    400.9µs    540.3µs     15.7ms    516.3µs
  384      6.8ms      1.3ms      1.6ms     52.5ms      1.3ms
  512     16.2ms      3.2ms      4.2ms      124ms      4.4ms
  640     32.2ms      6.3ms        8ms    244.6ms      7.2ms
  768     56.5ms     10.9ms     13.2ms    454.7ms       11ms
  896     88.1ms     16.9ms     21.5ms    817.1ms       21ms
 1024    135.6ms     26.8ms     31.4ms      1.29s     33.2ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.2µs      4.1µs     11.2µs     22.3µs      7.7µs
   64     23.9µs     23.9µs     45.3µs    149.2µs     43.8µs
   96     73.8µs     49.8µs       84µs      475µs    122.9µs
  128    162.8µs     98.5µs    192.6µs      1.1ms    279.7µs
  192    518.1µs    208.2µs    467.8µs      3.6ms    835.3µs
  256      1.2ms    428.8µs        1ms      8.5ms        2ms
  384      3.8ms    883.5µs      2.9ms     28.2ms      5.2ms
  512      8.8ms        2ms      6.5ms     65.9ms     12.3ms
  640     16.9ms      3.7ms     11.2ms    127.5ms     23.7ms
  768     29.3ms      6.7ms     18.4ms    220.6ms     38.6ms
  896     46.2ms     11.7ms     25.8ms    362.6ms     59.8ms
 1024     70.4ms     17.2ms     37.1ms      597ms     90.5ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.9µs     12.9µs      8.9µs     22.2µs      7.7µs
   64     18.2µs     31.1µs     63.3µs      149µs     43.8µs
   96     46.1µs     55.6µs     84.3µs      475µs    122.9µs
  128     84.3µs     88.6µs    193.4µs      1.1ms    279.8µs
  192    238.1µs    183.7µs    465.3µs      3.6ms    835.8µs
  256    499.1µs    324.3µs        1ms      8.5ms        2ms
  384      1.5ms    663.2µs      2.9ms     28.3ms      5.2ms
  512      3.3ms      1.2ms      6.5ms     66.1ms     12.3ms
  640      6.3ms      1.8ms     11.2ms    127.7ms     23.8ms
  768     10.8ms        3ms     18.3ms    221.5ms     38.8ms
  896     16.8ms      4.8ms     25.7ms    361.4ms       60ms
 1024     24.7ms      6.7ms     37.2ms    598.3ms     90.5ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     10.4µs     10.4µs        5µs        9µs     10.4µs
   64     30.3µs     30.3µs     46.3µs     54.3µs     43.6µs
   96       66µs       66µs     93.5µs    170.9µs      104µs
  128    111.9µs    112.2µs    173.6µs    391.2µs    182.6µs
  192    288.6µs    278.5µs    527.7µs      1.3ms    469.4µs
  256    582.1µs    458.3µs      1.2ms        3ms    889.7µs
  384      1.7ms      1.1ms        2ms      9.9ms      2.4ms
  512      3.7ms        2ms      4.6ms     22.8ms        5ms
  640      6.8ms      3.4ms      5.7ms     43.7ms      8.9ms
  768       12ms        5ms       11ms     74.7ms     14.6ms
  896     18.2ms      7.7ms     13.1ms    117.3ms     22.2ms
 1024     27.1ms     10.2ms     20.8ms    174.5ms     32.4ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     10.1µs     10.1µs      7.8µs     15.3µs     14.6µs
   64     54.4µs     54.5µs     33.2µs       98µs       81µs
   96    123.4µs    134.9µs     86.1µs    314.7µs    241.3µs
  128    252.2µs      252µs      262µs    735.4µs    519.5µs
  192    712.2µs    739.3µs    504.4µs      2.4ms      1.4ms
  256      1.5ms      1.4ms    850.3µs      5.9ms      2.7ms
  384      3.8ms      3.3ms      2.8ms     19.1ms      6.2ms
  512      8.6ms      6.1ms      3.6ms     45.2ms     11.7ms
  640     15.7ms      9.8ms      5.9ms     86.6ms     18.8ms
  768     25.4ms       16ms      9.1ms    153.8ms     28.3ms
  896     39.2ms     22.2ms     13.2ms    263.3ms     39.8ms
 1024     58.1ms     31.1ms     18.9ms    441.2ms     54.4ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     16.2µs     16.2µs          -     24.6µs    132.6µs
   64     82.2µs     82.4µs          -    187.2µs      1.2ms
   96    217.2µs    217.4µs          -    606.4µs      3.8ms
  128    464.8µs    463.8µs          -      1.4ms      8.9ms
  192      1.3ms      1.3ms          -      4.7ms     29.5ms
  256      3.2ms      3.2ms          -     11.3ms     69.7ms
  384      9.5ms      8.9ms          -     37.2ms    230.8ms
  512     21.2ms     13.9ms          -     87.8ms    544.3ms
  640     39.9ms     20.5ms          -    169.4ms      1.06s
  768     74.2ms     31.7ms          -    316.1ms      1.82s
  896    144.3ms     62.5ms          -    545.2ms       2.9s
 1024    266.3ms    130.3ms          -    883.4ms      4.37s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     23.6µs     23.6µs     31.8µs       28µs     16.2µs
   64     75.8µs     75.9µs      159µs    181.7µs      125µs
   96    175.2µs      175µs    995.8µs      585µs    235.1µs
  128    338.1µs    338.2µs      1.8ms      1.3ms    471.4µs
  192    936.3µs      937µs      3.8ms      4.4ms      1.2ms
  256        2ms      1.8ms      8.3ms     10.4ms      2.4ms
  384        6ms      3.8ms     27.2ms     34.7ms      6.8ms
  512     13.4ms      6.8ms     36.4ms     81.9ms     14.9ms
  640     25.5ms     11.3ms     56.7ms    159.9ms     28.5ms
  768     43.2ms     17.9ms       84ms    281.8ms     49.7ms
  896     67.8ms     26.3ms    115.4ms    469.5ms     78.7ms
 1024    101.4ms     38.3ms    189.6ms    737.9ms    117.3ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     41.8µs     56.9µs          -     36.4µs     21.8µs
   64    172.1µs    194.9µs          -    267.6µs     99.6µs
   96    418.8µs    471.9µs          -    877.6µs      268µs
  128    813.6µs    889.8µs          -        2ms    550.2µs
  192      2.1ms      2.2ms          -      6.6ms      1.6ms
  256      4.7ms        4ms          -     15.7ms      4.1ms
  384     13.7ms      7.4ms          -     52.3ms     13.2ms
  512     30.5ms     12.1ms          -    123.4ms     29.4ms
  640     57.6ms     18.8ms          -    242.8ms     56.6ms
  768    102.3ms     29.4ms          -    444.1ms    101.9ms
  896    178.6ms     53.3ms          -    743.4ms    186.5ms
 1024    300.8ms    116.7ms          -      1.16s      334ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     25.4µs     46.9µs     17.9µs     66.6µs     32.8µs
   64    110.2µs    129.4µs       89µs    413.5µs    182.2µs
   96    293.5µs    251.3µs    290.1µs      1.3ms    515.5µs
  128    575.7µs    472.1µs    633.9µs      3.1ms      1.2ms
  192      1.6ms      1.1ms      1.3ms      9.8ms      3.2ms
  256      3.3ms      2.1ms      2.3ms     23.8ms        7ms
  384      9.7ms      4.9ms      4.8ms     77.2ms     17.7ms
  512       22ms     10.4ms      9.8ms    181.1ms     38.9ms
  640     41.9ms     17.7ms     16.2ms    348.6ms     71.4ms
  768     69.4ms     28.9ms     26.2ms    612.6ms    112.8ms
  896    107.9ms     59.7ms     39.7ms      1.01s    168.9ms
 1024    159.2ms     64.9ms     58.9ms      1.66s    249.5ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    189.9µs    219.7µs    133.5µs    247.3µs    274.3µs
   64    713.9µs    698.5µs      1.1ms      1.6ms      1.4ms
   96      1.6ms      1.7ms        3ms        5ms      3.7ms
  128        3ms      2.9ms      4.8ms       12ms      6.1ms
  192      7.8ms      7.2ms     10.5ms     40.2ms     14.6ms
  256     16.3ms     11.9ms     19.2ms    117.7ms     28.5ms
  384       46ms     26.3ms     43.1ms    374.4ms     69.8ms
  512    100.8ms     48.1ms       83ms    929.3ms    142.8ms
  640    184.2ms     79.7ms      136ms      1.81s    242.2ms
  768    324.3ms    128.5ms    212.6ms      3.55s    406.5ms
  896    536.4ms    231.5ms    337.4ms      5.55s    617.1ms
 1024    838.3ms    347.7ms    512.8ms      9.92s    958.9ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      2.9ms      2.8ms     10.5ms     17.9ms      7.9ms
   64      9.7ms      8.6ms     26.8ms     72.5ms     23.9ms
   96     20.5ms     13.6ms     48.8ms    163.3ms     48.4ms
  128       35ms     20.5ms     81.5ms      300ms     80.4ms
  192     74.9ms     38.1ms    128.7ms    727.9ms    168.1ms
  256    130.8ms     59.5ms    315.3ms      1.39s    293.8ms
  384      292ms    111.6ms    372.2ms      3.28s    645.9ms
  512    522.5ms    181.4ms    711.1ms      5.95s      1.15s
  640    826.5ms    358.9ms    750.8ms      9.56s      1.79s
  768      1.25s    453.3ms      1.18s     14.31s      2.57s
  896      1.76s    621.3ms      1.24s     19.93s      3.49s
 1024      2.41s    838.6ms      1.85s     27.44s      4.56s
```
