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
    4       45ns       40ns      146ns       42ns       16ns
    8      108ns      111ns       52ns      129ns       89ns
   16      132ns      136ns      104ns      292ns      137ns
   32      580ns      557ns      565ns      1.1µs      650ns
   64      4.1µs      4.1µs        4µs        7µs      3.8µs
   96     13.9µs      8.6µs     13.1µs     22.1µs      6.7µs
  128     32.4µs     11.4µs     28.9µs     50.7µs     17.3µs
  192    107.6µs     31.4µs     34.1µs    164.6µs     26.6µs
  256      255µs       64µs    113.1µs    387.2µs     71.3µs
  384    848.2µs    179.7µs    215.4µs      1.3ms    179.3µs
  512        2ms    404.1µs    609.6µs        3ms    518.4µs
  640      3.9ms    816.3µs      1.1ms      5.9ms    885.5µs
  768      6.8ms      1.3ms      1.6ms     10.1ms      1.5ms
  896     10.8ms      2.6ms      2.8ms     16.1ms      2.9ms
 1024     16.2ms      3.9ms      4.2ms     24.3ms        4ms
```

## Triangular solve

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       22ns       20ns      786ns       38ns       63ns
    8      124ns      122ns      1.5µs      195ns      182ns
   16      574ns      584ns      3.1µs      1.3µs      843ns
   32      2.3µs      2.3µs      8.7µs      5.8µs      2.8µs
   64      8.9µs      9.1µs     28.3µs     27.5µs     14.3µs
   96     23.7µs     24.5µs     55.8µs     71.3µs     30.4µs
  128     44.4µs     33.8µs    118.7µs    152.6µs     65.1µs
  192    122.7µs     77.7µs    217.3µs    476.9µs    156.3µs
  256      247µs    124.4µs    570.5µs      1.1ms      352µs
  384    704.5µs      223µs      1.1ms      3.6ms    888.2µs
  512      1.5ms      434µs        3ms      9.1ms        2ms
  640      2.8ms    856.3µs      3.6ms     18.5ms      3.3ms
  768      4.5ms      2.1ms      7.6ms     31.7ms      5.5ms
  896      6.8ms      2.2ms      8.6ms     48.6ms      8.2ms
 1024     10.4ms      3.1ms     18.1ms     71.9ms     13.4ms
```

## Triangular inverse

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      198ns      5.2µs      767ns       38ns       63ns
    8      549ns        6µs      1.5µs      196ns      178ns
   16      1.6µs      7.8µs      3.1µs      1.3µs      851ns
   32      4.3µs     18.6µs      8.4µs      5.8µs      2.8µs
   64     12.3µs     23.5µs     27.2µs     27.5µs     12.2µs
   96     28.9µs     45.8µs     55.4µs     71.6µs     30.3µs
  128     39.8µs     56.5µs    119.3µs    151.6µs     65.2µs
  192     94.1µs     90.6µs    222.9µs    476.8µs    156.2µs
  256    149.5µs    133.3µs    569.6µs      1.1ms    351.7µs
  384    382.9µs    262.6µs      1.1ms      3.6ms    884.4µs
  512    726.8µs    388.8µs      2.9ms      8.6ms        2ms
  640      1.3ms    605.2µs      3.6ms     16.6ms      3.3ms
  768        2ms    849.8µs      7.4ms     28.3ms      5.4ms
  896      2.9ms      1.1ms      8.4ms     44.4ms      8.2ms
 1024      4.3ms      1.5ms       18ms     65.9ms     13.3ms
```

## Cholesky decomposition

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       54ns       54ns      149ns       41ns       47ns
    8      130ns      130ns      313ns       94ns      143ns
   16      491ns      490ns      756ns      359ns      416ns
   32      1.8µs      1.8µs      2.6µs      1.7µs      2.1µs
   64      5.8µs      5.9µs     12.4µs      8.3µs      7.2µs
   96     15.4µs     15.6µs     27.3µs     21.1µs     16.3µs
  128     20.3µs     20.6µs    124.8µs     44.3µs       27µs
  192     61.1µs     71.8µs    263.8µs    135.3µs     64.8µs
  256     90.5µs    103.7µs    421.5µs    324.2µs    131.1µs
  384    246.9µs      298µs    876.6µs      1.1ms    355.7µs
  512    569.1µs    398.2µs      3.1ms      2.9ms    752.3µs
  640    993.5µs    927.4µs      2.8ms      5.4ms      1.3ms
  768      1.8ms        1ms      4.2ms      9.7ms      2.1ms
  896      2.7ms      1.5ms      5.3ms     15.3ms        3ms
 1024      4.2ms      1.7ms     13.6ms     22.6ms      4.6ms
```

## LU decomposition with partial pivoting

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       98ns      104ns      174ns       87ns       93ns
    8      209ns      212ns      390ns      169ns      282ns
   16      534ns      521ns      1.2µs      955ns      929ns
   32      2.5µs      2.5µs      4.2µs        4µs        4µs
   64     10.9µs     10.8µs     15.7µs     18.2µs     14.1µs
   96       25µs     26.3µs     33.6µs     46.4µs     31.7µs
  128     49.6µs     49.9µs       64µs    105.1µs     93.4µs
  192    127.4µs    156.8µs    150.5µs    303.8µs    326.1µs
  256    247.6µs      278µs    360.2µs    725.2µs    651.8µs
  384    651.2µs    608.4µs    667.2µs      2.4ms      1.4ms
  512      1.3ms        1ms        1ms      6.4ms      3.5ms
  640      2.3ms      1.6ms      2.2ms     11.6ms      4.5ms
  768      3.8ms      2.2ms      3.7ms     20.4ms      6.7ms
  896      5.4ms      3.1ms      3.9ms     31.1ms      8.1ms
 1024      8.2ms      4.2ms      5.1ms     48.1ms     14.2ms
```

## LU decomposition with full pivoting

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      189ns      188ns          -      105ns      155ns
    8      406ns      423ns          -      408ns      455ns
   16      1.3µs      1.3µs          -      2.3µs      1.8µs
   32      6.2µs      6.3µs          -     14.2µs     10.7µs
   64     23.8µs     23.5µs          -    100.3µs     63.4µs
   96     55.6µs     55.3µs          -      322µs    181.6µs
  128    115.9µs    116.2µs          -    756.2µs    413.5µs
  192    306.3µs    305.5µs          -      2.5ms      1.2ms
  256    659.7µs    660.5µs          -      5.9ms      2.8ms
  384        2ms      2.2ms          -     19.7ms        9ms
  512      5.7ms      5.4ms          -     47.4ms     22.2ms
  640      9.7ms      8.2ms          -     91.6ms     41.5ms
  768     16.8ms     12.8ms          -    158.9ms     72.5ms
  896       26ms     17.9ms          -    251.2ms    113.3ms
 1024     41.6ms       24ms          -    378.2ms    173.3ms
```

## QR decomposition with no pivoting

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      126ns      126ns      748ns      122ns      271ns
    8      348ns      348ns      1.5µs      287ns      771ns
   16      1.3µs      1.3µs      4.1µs      1.2µs      2.3µs
   32      5.8µs      5.8µs     16.3µs      5.8µs      7.1µs
   64     30.3µs     30.3µs     63.5µs     34.6µs     40.2µs
   96     61.2µs     61.1µs    269.1µs      102µs     62.6µs
  128    109.3µs    109.1µs    660.7µs    222.5µs    110.8µs
  192      250µs    249.9µs      2.3ms      713µs    235.1µs
  256    478.8µs    580.1µs      3.3ms      1.7ms    464.6µs
  384      1.3ms      1.4ms      6.8ms      5.4ms      1.2ms
  512      2.7ms      2.5ms     12.3ms     12.4ms      2.4ms
  640      4.7ms      3.5ms     17.1ms     23.5ms      4.2ms
  768      7.5ms      5.1ms     28.7ms       40ms      6.7ms
  896     11.2ms      6.9ms     38.2ms     62.8ms     10.3ms
 1024     16.2ms      9.4ms     56.1ms     94.3ms       15ms
```

## QR decomposition with column pivoting

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      192ns      179ns          -      166ns      374ns
    8      438ns      451ns          -      515ns        1µs
   16      1.6µs      1.5µs          -      2.7µs      2.9µs
   32      6.7µs      6.7µs          -     16.4µs      8.9µs
   64     33.5µs     53.2µs          -    116.8µs       32µs
   96     77.2µs     97.5µs          -    379.6µs     73.4µs
  128    147.7µs      173µs          -    884.3µs    149.8µs
  192      398µs    459.2µs          -      2.9ms    401.2µs
  256    860.3µs        1ms          -      6.9ms    882.8µs
  384      2.6ms      2.5ms          -     22.8ms      2.8ms
  512      6.3ms      4.6ms          -     53.6ms      7.7ms
  640     12.2ms      7.5ms          -    103.7ms     14.7ms
  768     20.6ms     10.9ms          -    178.4ms       26ms
  896     32.6ms     14.8ms          -    282.8ms     41.3ms
 1024     48.8ms     17.9ms          -    422.1ms     64.2ms
```

## Matrix inverse

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      870ns        7µs      467ns       65ns      384ns
    8      2.3µs      8.6µs      909ns      697ns      900ns
   16      5.1µs     11.8µs      2.4µs      3.8µs      2.9µs
   32     14.5µs     28.6µs      8.8µs       17µs     11.1µs
   64       44µs     62.7µs     35.7µs     77.4µs     43.3µs
   96    107.9µs    111.6µs    168.6µs    201.6µs      101µs
  128    158.2µs    148.8µs    298.2µs    433.4µs    257.1µs
  192    417.2µs      348µs    672.7µs      1.3ms    725.2µs
  256    692.8µs    502.9µs        1ms      3.1ms      1.7ms
  384      1.9ms      1.1ms      1.9ms     10.1ms      3.6ms
  512      3.7ms      1.8ms      3.5ms     26.9ms      8.3ms
  640      6.9ms      3.7ms      5.1ms     51.5ms       13ms
  768     10.9ms      5.3ms      7.3ms     88.2ms     20.6ms
  896     16.1ms      7.5ms     10.7ms    134.3ms     28.2ms
 1024     23.9ms     10.4ms     14.8ms    199.5ms     44.3ms
```

## Square matrix singular value decomposition

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.4µs      1.6µs      2.7µs        1µs      1.2µs
    8      9.4µs     23.8µs      6.4µs      2.9µs      6.1µs
   16     28.9µs     56.2µs     21.5µs       12µs     38.7µs
   32     95.2µs    126.5µs     77.6µs       64µs    141.1µs
   64    311.5µs    298.2µs    489.9µs    383.6µs    605.4µs
   96    681.4µs    727.4µs      1.3ms      1.2ms      1.6ms
  128      1.2ms      1.1ms      2.3ms      2.9ms      2.5ms
  192      2.6ms      2.9ms      5.1ms      8.2ms      5.5ms
  256      4.9ms        5ms      8.5ms     22.7ms      9.6ms
  384     12.8ms     10.6ms     18.2ms     76.4ms     22.4ms
  512     26.1ms     18.2ms     35.8ms    277.7ms     45.2ms
  640     45.4ms     32.8ms     51.7ms      361ms     70.7ms
  768       75ms     49.6ms     81.3ms    857.9ms    110.5ms
  896      110ms     68.5ms    110.5ms      1.03s    156.3ms
 1024    167.2ms     92.4ms      171ms      2.46s    233.9ms
```

## Thin matrix singular value decomposition

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       60µs     59.7µs      293µs     71.9µs     29.4µs
    8    166.5µs    183.8µs    616.3µs    196.3µs    106.8µs
   16    396.7µs    476.5µs      1.7ms    656.2µs    281.6µs
   32    964.3µs      1.1ms      4.3ms      2.5ms      1.1ms
   64      2.5ms      2.6ms     13.9ms      9.3ms        4ms
   96      4.7ms      4.5ms     27.2ms     20.6ms      8.6ms
  128      7.4ms      6.5ms     41.9ms     37.1ms     13.9ms
  192     14.4ms       12ms     57.1ms       84ms     28.3ms
  256     23.8ms       18ms     73.3ms    151.9ms     46.6ms
  384     51.7ms     32.9ms    106.2ms    363.5ms     96.1ms
  512     90.4ms     52.1ms    148.6ms    794.8ms    164.1ms
  640    139.9ms     73.1ms    192.3ms      1.19s      254ms
  768    204.9ms    110.8ms    254.3ms      2.17s    373.5ms
  896    284.8ms    150.6ms    325.2ms      2.92s    518.7ms
 1024    388.3ms    190.8ms    544.3ms      5.03s    708.6ms
```

## Hermitian matrix eigenvalue decomposition

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.4µs      1.2µs      1.3µs      564ns      840ns
    8      3.8µs      3.4µs      5.3µs      2.1µs      3.1µs
   16     11.2µs     11.9µs     19.4µs      8.2µs       10µs
   32     44.1µs     45.3µs    111.9µs     34.5µs     36.1µs
   64    174.1µs    173.6µs    795.9µs    178.4µs    149.3µs
   96    374.8µs    428.7µs      2.1ms    491.2µs    356.3µs
  128    647.7µs    662.2µs      4.3ms      1.1ms    692.9µs
  192      1.5ms      1.5ms     12.7ms      3.2ms      1.8ms
  256      2.7ms      2.6ms       28ms      7.1ms      3.8ms
  384      6.6ms      6.3ms     87.1ms     22.2ms     10.9ms
  512     12.7ms     11.3ms    151.2ms     51.8ms     25.5ms
  640     21.3ms     18.3ms    237.7ms     97.1ms       48ms
  768     34.4ms     26.5ms    344.9ms      166ms     83.6ms
  896     50.6ms     36.5ms    526.5ms    258.2ms    133.3ms
 1024     73.9ms       48ms    759.8ms    387.3ms      201ms
```

## Non Hermitian matrix eigenvalue decomposition

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      4.6µs      4.5µs      2.9µs          -      3.4µs
    8     14.7µs     14.2µs      7.8µs          -     12.4µs
   16     45.5µs     49.7µs     29.7µs          -     34.7µs
   32    194.5µs      203µs    158.1µs          -    137.9µs
   64      885µs    937.9µs    820.2µs          -    719.5µs
   96      1.9ms        2ms        4ms          -        2ms
  128      3.1ms      3.5ms      7.7ms          -      5.2ms
  192      8.3ms      9.4ms       13ms          -     12.9ms
  256     14.3ms     15.8ms     36.1ms          -     42.9ms
  384     32.1ms     36.1ms     64.3ms          -    141.7ms
  512     69.3ms     75.9ms    172.9ms          -    521.5ms
  640    117.9ms      126ms    251.4ms          -    580.4ms
  768      185ms      177ms    340.2ms          -      1.28s
  896    246.6ms    232.3ms    417.5ms          -      1.62s
 1024    398.9ms    377.5ms    599.6ms          -       4.2s
```
