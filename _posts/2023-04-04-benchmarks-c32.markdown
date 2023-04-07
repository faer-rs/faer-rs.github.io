---
layout: post
title:  "Benchmarks (c32)"
date:   2023-04-03 00:00:03 +0100
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
   64     16.4µs     16.4µs     19.8µs    126.1µs      8.6µs
   96     53.7µs       18µs     21.4µs    419.8µs     15.8µs
  128    127.1µs       34µs     65.1µs      1.1ms     61.4µs
  192    424.2µs     90.1µs    111.3µs      3.5ms     91.5µs
  256        1ms    210.8µs    318.8µs      8.2ms    262.3µs
  384      3.4ms    661.2µs    779.9µs     26.8ms    648.8µs
  512        8ms      1.6ms      2.2ms     62.9ms      2.1ms
  640     15.6ms        3ms      4.1ms    122.1ms      3.3ms
  768     27.2ms      5.4ms      6.4ms    210.3ms      5.1ms
  896     43.4ms      8.6ms       11ms      339ms     11.2ms
 1024     66.3ms       14ms     16.4ms      516ms     16.1ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.8µs      3.7µs     10.4µs     15.5µs     10.9µs
   64     17.2µs     17.2µs     31.2µs     83.7µs     48.4µs
   96     50.2µs     36.9µs     67.6µs      270µs    120.6µs
  128    103.9µs     67.2µs    167.5µs    612.9µs    237.5µs
  192    310.6µs    141.3µs    330.3µs        2ms    633.9µs
  256    675.7µs    263.2µs      742µs      4.6ms      1.3ms
  384      2.1ms      528µs      1.8ms     15.2ms      3.8ms
  512      4.8ms      1.1ms      4.3ms     35.2ms      7.5ms
  640        9ms      2.1ms      6.9ms     68.2ms     13.6ms
  768     15.2ms        4ms     11.6ms    116.7ms     22.7ms
  896     23.7ms      6.2ms     16.7ms    183.6ms     35.2ms
 1024     35.6ms      9.2ms     24.1ms    273.2ms     48.8ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      4.7µs     12.6µs     10.5µs     15.5µs     10.9µs
   64     15.8µs     28.1µs     31.3µs     83.8µs     48.5µs
   96     38.5µs       48µs     68.3µs    256.7µs    120.7µs
  128     62.3µs     69.7µs    169.2µs    575.4µs    237.6µs
  192    163.8µs      130µs    330.7µs        2ms    634.3µs
  256    311.4µs    212.5µs    750.6µs      4.6ms      1.3ms
  384      894µs    419.9µs      1.8ms     15.2ms      3.8ms
  512      1.9ms    727.2µs      4.3ms     35.3ms      7.5ms
  640      3.5ms      1.1ms        7ms     68.4ms     13.6ms
  768      5.8ms      1.5ms     11.6ms    116.8ms     22.8ms
  896      8.8ms      2.3ms     16.8ms    183.6ms     35.4ms
 1024     13.1ms      3.4ms     24.2ms    273.5ms       49ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      3.4µs      3.4µs      4.4µs      6.7µs     10.8µs
   64     12.2µs     12.3µs       44µs     33.9µs     43.4µs
   96     35.5µs     35.5µs      102µs    101.7µs     99.8µs
  128     52.7µs     52.7µs    236.4µs    234.5µs    170.1µs
  192      166µs    163.5µs    336.8µs    726.7µs    410.4µs
  256    301.4µs    241.6µs    747.4µs      1.7ms    754.9µs
  384    887.3µs    687.1µs      1.3ms      5.4ms      1.9ms
  512      2.1ms      1.1ms      4.1ms     12.9ms      3.9ms
  640      3.7ms      1.9ms      3.7ms     24.3ms      6.3ms
  768      6.4ms      2.8ms        6ms     41.5ms     10.1ms
  896      9.8ms      4.1ms      7.9ms     64.6ms     14.8ms
 1024     15.2ms      5.2ms     16.1ms     95.7ms     21.2ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      8.1µs      8.2µs      6.9µs      9.8µs     21.1µs
   64     36.8µs     36.7µs     27.7µs     53.9µs       81µs
   96     88.9µs     88.8µs     67.5µs    177.8µs    184.6µs
  128    172.6µs    172.9µs    247.1µs    416.7µs    387.9µs
  192    453.4µs    464.1µs    433.5µs      1.3ms      1.1ms
  256    902.9µs    895.2µs      656µs      3.1ms        2ms
  384      2.4ms      2.2ms      1.3ms     10.1ms      4.5ms
  512        5ms      3.9ms      3.5ms     24.2ms      8.5ms
  640      8.7ms      6.4ms      3.9ms     45.7ms     12.8ms
  768       14ms      9.7ms      5.7ms     78.9ms     19.2ms
  896     21.2ms     13.9ms      7.9ms    123.8ms     25.7ms
 1024     31.3ms     18.5ms       11ms    187.9ms     35.2ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     11.1µs     11.1µs          -     18.7µs    201.5µs
   64     45.9µs     46.3µs          -    137.7µs      1.5ms
   96    121.8µs    121.9µs          -    454.5µs      4.8ms
  128    257.8µs    258.3µs          -      1.1ms     11.3ms
  192      713µs    713.1µs          -      3.5ms     37.9ms
  256      1.7ms      1.7ms          -      8.3ms     89.4ms
  384      5.1ms        5ms          -     27.5ms    297.5ms
  512     12.2ms        9ms          -     65.4ms    700.2ms
  640     22.1ms     13.4ms          -    126.2ms      1.36s
  768     37.3ms       19ms          -    217.9ms      2.34s
  896     57.6ms     27.1ms          -    346.8ms      3.71s
 1024     87.8ms     43.2ms          -      522ms      5.53s
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     18.8µs     18.8µs     32.8µs     22.4µs     19.2µs
   64     56.9µs     57.1µs    254.5µs    135.8µs     99.7µs
   96    121.6µs    121.5µs      606µs    437.2µs    173.4µs
  128    221.8µs    221.8µs      1.1ms        1ms    312.9µs
  192    578.2µs    578.4µs      2.6ms      3.3ms    716.8µs
  256      1.2ms      1.2ms      5.2ms      7.7ms      1.4ms
  384      3.5ms      2.5ms     17.4ms     25.6ms      3.7ms
  512      7.6ms      4.5ms     25.1ms     60.4ms      7.8ms
  640     14.6ms        7ms     35.1ms    117.2ms     14.2ms
  768     24.4ms     10.5ms     55.1ms    201.9ms     23.8ms
  896     37.7ms       15ms     73.3ms    320.3ms     36.9ms
 1024     55.3ms     21.1ms    106.1ms    479.4ms     55.8ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     36.1µs     51.2µs          -     31.1µs     23.1µs
   64    143.9µs    171.2µs          -    219.7µs     91.6µs
   96    343.1µs    393.1µs          -    718.4µs    220.4µs
  128    660.2µs    723.5µs          -      1.7ms    429.5µs
  192      1.7ms      1.7ms          -      5.5ms      1.1ms
  256      3.3ms      2.9ms          -     12.9ms      2.3ms
  384        9ms      5.3ms          -       43ms      7.3ms
  512     19.2ms      8.7ms          -    101.6ms     18.9ms
  640     34.5ms     13.4ms          -    197.7ms     30.5ms
  768     56.6ms     18.8ms          -    340.8ms     52.3ms
  896     86.6ms     26.5ms          -    543.5ms     78.9ms
 1024    127.9ms     59.5ms          -    813.5ms    126.4ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32     21.4µs       41µs     15.6µs     46.2µs     45.3µs
   64     84.2µs    104.3µs     71.9µs    245.5µs    186.2µs
   96    216.3µs    199.3µs      219µs    761.9µs      445µs
  128    373.2µs    326.5µs    434.4µs      1.7ms      955µs
  192        1ms    768.8µs        1ms      5.5ms      2.5ms
  256        2ms      1.3ms      1.8ms     12.9ms      4.8ms
  384      5.6ms        3ms      3.3ms     41.7ms     12.7ms
  512       12ms      5.9ms      6.2ms     97.4ms     25.2ms
  640     22.5ms      9.9ms      9.9ms    184.9ms     43.6ms
  768     37.4ms     16.2ms     14.8ms    316.1ms     69.7ms
  896     57.3ms     23.1ms     22.2ms    496.2ms    102.8ms
 1024     83.5ms     32.9ms     31.8ms      753ms    141.3ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32    157.5µs    198.3µs    118.4µs    151.7µs    209.1µs
   64    555.7µs    567.1µs    984.8µs    915.3µs      825µs
   96      1.2ms      1.3ms      1.9ms      2.8ms      2.2ms
  128      2.2ms      2.2ms      3.3ms      6.9ms      3.5ms
  192      5.4ms      5.2ms      7.1ms     22.4ms      8.3ms
  256     10.4ms      8.4ms     12.7ms     58.6ms     15.2ms
  384     28.1ms       17ms     28.3ms    188.1ms     37.7ms
  512     59.8ms     31.2ms     56.4ms      527ms       76ms
  640    106.9ms     49.5ms     86.2ms    946.6ms    123.7ms
  768      175ms     82.3ms    132.7ms      1.76s    198.4ms
  896    266.7ms    129.5ms    185.9ms      2.77s    287.6ms
 1024    395.8ms    175.4ms    272.9ms      4.55s    443.8ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
   32      1.8ms      1.9ms      8.3ms     10.1ms      3.6ms
   64      5.7ms      5.2ms     20.6ms     39.6ms     10.6ms
   96     11.9ms      8.6ms       36ms     89.9ms     23.9ms
  128     19.9ms     12.6ms     56.4ms      159ms     40.8ms
  192     42.3ms     23.3ms     84.8ms    359.1ms     71.1ms
  256     73.5ms     35.7ms    124.9ms    654.3ms    118.6ms
  384    156.7ms       67ms    200.3ms      1.58s    263.5ms
  512    279.9ms    106.6ms    467.3ms      3.05s    486.2ms
  640    439.7ms    156.4ms    459.1ms      4.92s    777.1ms
  768    642.6ms    255.4ms    618.6ms       7.4s      1.15s
  896    887.2ms    344.6ms    762.2ms      10.3s      1.58s
 1024       1.2s    440.4ms      1.16s     14.26s      2.07s
```
