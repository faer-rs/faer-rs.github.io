The benchmarks were run on an `11th Gen Intel(R) Core(TM) i5-11400 @ 2.60GHz` with 12 threads.  
- `nalgebra` is used with the `matrixmultiply` backend
- `ndarray` is used with the `openblas` backend
- `eigen` is compiled with `-march=native -O3 -fopenmp`

All computations are done on column major matrices.


# `f32`
## Matrix multiplication (`f32`)

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

## Triangular solve (`f32`)

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

## Triangular inverse (`f32`)

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

## Cholesky decomposition (`f32`)

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

## LU decomposition with partial pivoting (`f32`)

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

## LU decomposition with full pivoting (`f32`)

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

## QR decomposition with no pivoting (`f32`)

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

## QR decomposition with column pivoting (`f32`)

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

## Matrix inverse (`f32`)

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

## Square matrix singular value decomposition (`f32`)

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

## Thin matrix singular value decomposition (`f32`)

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

## Hermitian matrix eigenvalue decomposition (`f32`)

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

## Non Hermitian matrix eigenvalue decomposition (`f32`)

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

# `f64`
## Matrix multiplication (`f64`)

Multiplication of two square matrices of dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       40ns       41ns      139ns       29ns       17ns
    8       77ns       80ns       63ns      161ns       85ns
   16      189ns      193ns      201ns      363ns      219ns
   32      1.1µs      1.1µs      1.1µs      1.5µs      1.2µs
   64      7.9µs      7.9µs      7.9µs     10.5µs      5.1µs
   96     27.5µs     11.2µs     26.2µs     34.9µs     10.1µs
  128     65.5µs     17.1µs     35.7µs     78.3µs     32.9µs
  192    216.6µs     54.4µs     57.3µs    260.7µs     51.7µs
  256    510.8µs    117.8µs    183.2µs    602.6µs    142.9µs
  384      1.7ms    339.1µs    575.8µs        2ms    327.9µs
  512        4ms    785.6µs      1.3ms      4.7ms        1ms
  640      7.9ms      1.6ms      2.3ms      9.2ms      1.9ms
  768     13.8ms      2.9ms      3.6ms       16ms      3.2ms
  896     22.2ms      4.6ms      6.5ms     25.7ms      5.9ms
 1024     33.9ms      6.6ms      9.7ms     39.1ms      8.3ms
```

## Triangular solve (`f64`)

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       20ns       19ns      755ns       39ns       65ns
    8      118ns      118ns      1.5µs      308ns      156ns
   16      498ns      502ns      3.3µs      1.5µs      671ns
   32      2.1µs      2.1µs      8.6µs      6.6µs      2.9µs
   64      9.7µs      9.8µs     25.9µs     34.2µs     13.8µs
   96     27.7µs     24.5µs     55.2µs    101.4µs     36.9µs
  128     56.4µs     39.9µs    145.2µs      232µs     81.7µs
  192    167.8µs       92µs    263.6µs    815.5µs    213.6µs
  256    367.7µs      163µs      660µs      1.9ms    488.1µs
  384      1.1ms    317.5µs      1.4ms      7.4ms      1.4ms
  512      2.6ms    662.7µs      3.5ms     17.2ms      3.3ms
  640      4.7ms      1.2ms      5.7ms     33.6ms      5.5ms
  768        8ms      2.3ms      9.4ms     56.2ms      9.3ms
  896     12.3ms      3.6ms     13.6ms     89.3ms       14ms
 1024     18.7ms      5.2ms     20.1ms    131.9ms     22.9ms
```

## Triangular inverse (`f64`)

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      162ns      5.2µs      771ns       38ns       65ns
    8      514ns      5.9µs      1.5µs      308ns      156ns
   16      1.6µs      7.7µs      3.4µs      1.5µs      672ns
   32      4.2µs     10.5µs      8.7µs      6.6µs      2.9µs
   64     12.5µs     18.1µs     25.7µs     34.2µs     13.8µs
   96     30.6µs     39.8µs       55µs    101.4µs     36.9µs
  128     42.7µs     51.9µs    144.9µs      232µs     81.6µs
  192      110µs     89.7µs    262.9µs    815.7µs    213.3µs
  256    191.7µs    138.3µs    645.5µs      1.9ms    486.9µs
  384    533.5µs    274.7µs      1.4ms      6.7ms      1.4ms
  512      1.1ms    449.4µs      3.5ms     15.6ms      3.3ms
  640        2ms    861.3µs      5.6ms     30.2ms      5.5ms
  768      3.2ms      1.2ms      9.3ms     51.8ms      9.3ms
  896      4.8ms      1.7ms     13.4ms     81.9ms       14ms
 1024      7.2ms      2.4ms     19.9ms    122.8ms     22.7ms
```

## Cholesky decomposition (`f64`)

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4       49ns       49ns      149ns       52ns       43ns
    8      128ns      128ns      329ns       99ns      125ns
   16      408ns      408ns      950ns      412ns      376ns
   32      1.8µs      1.8µs      3.3µs      1.8µs      2.3µs
   64        7µs        7µs     34.6µs     10.5µs        9µs
   96       18µs     18.2µs     70.5µs     31.3µs       21µs
  128     30.1µs     30.4µs    202.2µs     77.4µs     40.3µs
  192     86.4µs     92.7µs    301.3µs    259.8µs    105.2µs
  256    161.7µs    149.4µs    711.5µs    607.4µs    216.6µs
  384    462.9µs    423.9µs      1.2ms      2.1ms    596.5µs
  512      1.1ms    619.5µs      3.8ms      5.4ms      1.3ms
  640      1.9ms      1.3ms      3.3ms     10.4ms      2.2ms
  768      3.3ms      1.8ms      5.4ms     17.9ms      3.7ms
  896        5ms      2.7ms      6.9ms     28.4ms      5.6ms
 1024      7.8ms      3.4ms     14.5ms     41.2ms      8.4ms
```

## LU decomposition with partial pivoting (`f64`)

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      103ns       99ns      180ns       77ns       98ns
    8      210ns      217ns      405ns      241ns      278ns
   16      649ns      625ns      1.4µs      859ns      880ns
   32      2.7µs      2.6µs      5.6µs      4.4µs      3.9µs
   64     12.4µs     12.5µs     17.4µs     22.9µs     15.6µs
   96     30.2µs     31.6µs     34.4µs     67.9µs     36.7µs
  128     61.3µs     60.7µs     97.4µs    159.4µs      126µs
  192    163.5µs    187.3µs    182.4µs    527.8µs    425.5µs
  256      352µs    360.9µs    491.1µs      1.3ms    824.9µs
  384    968.8µs    781.3µs    909.5µs      4.5ms      1.9ms
  512      2.1ms      1.5ms      1.5ms     11.1ms      4.3ms
  640      3.8ms      2.2ms      2.2ms     20.7ms      5.6ms
  768      6.2ms      3.2ms      3.4ms     35.8ms      8.6ms
  896      9.5ms      4.6ms      4.7ms     56.1ms     11.4ms
 1024     14.4ms      6.5ms      6.7ms       88ms     17.1ms
```

## LU decomposition with full pivoting (`f64`)

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      132ns      134ns          -      111ns      164ns
    8      386ns      415ns          -      418ns      493ns
   16      1.7µs      1.7µs          -      2.3µs      2.1µs
   32      5.9µs        6µs          -     14.7µs     12.2µs
   64     25.8µs     25.4µs          -    106.4µs     72.2µs
   96     67.7µs     67.9µs          -    347.3µs    206.3µs
  128    156.4µs    155.2µs          -    819.1µs    460.9µs
  192    463.4µs    460.6µs          -      2.8ms      1.4ms
  256      1.1ms      1.1ms          -      6.6ms      3.3ms
  384      3.8ms      3.8ms          -     22.1ms       11ms
  512     10.1ms      7.9ms          -     53.4ms     27.4ms
  640     17.7ms       12ms          -    102.5ms     50.7ms
  768     31.2ms     17.5ms          -    176.9ms     87.3ms
  896     47.3ms     25.1ms          -      280ms    136.1ms
 1024     76.1ms     33.9ms          -      431ms    207.9ms
```

## QR decomposition with no pivoting (`f64`)

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      132ns      132ns      758ns      138ns      273ns
    8      345ns      346ns      1.7µs      321ns      777ns
   16      1.1µs      1.1µs      4.8µs      1.3µs      2.2µs
   32      4.4µs      4.4µs     15.3µs      6.9µs      7.4µs
   64     30.5µs     30.1µs     61.7µs     43.4µs     45.2µs
   96     65.2µs     65.2µs    322.4µs    141.3µs     79.1µs
  128    118.4µs    118.3µs    842.4µs    320.9µs    154.3µs
  192    315.3µs    316.1µs      1.6ms      1.1ms    383.7µs
  256    643.8µs    693.4µs      2.8ms      2.4ms    794.6µs
  384      1.9ms      1.7ms      7.6ms      8.1ms      2.1ms
  512      4.1ms        3ms     16.1ms       19ms      4.5ms
  640      7.4ms      4.5ms     22.5ms     36.2ms        8ms
  768     12.2ms      6.6ms     34.7ms     62.1ms     13.2ms
  896     18.6ms      9.2ms     46.3ms     97.7ms     20.4ms
 1024     27.7ms     12.9ms     65.9ms      150ms     30.2ms
```

## QR decomposition with column pivoting (`f64`)

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      167ns      185ns          -      172ns      373ns
    8      430ns      433ns          -      552ns        1µs
   16      1.7µs      1.7µs          -      2.8µs      2.9µs
   32      5.9µs        6µs          -     17.6µs      9.5µs
   64     33.2µs     50.6µs          -    126.9µs     37.9µs
   96     85.6µs    104.7µs          -    421.8µs    104.7µs
  128    182.3µs    209.2µs          -    987.7µs    218.1µs
  192    548.2µs    600.4µs          -      3.3ms    628.1µs
  256      1.3ms      1.4ms          -      7.6ms      1.6ms
  384      4.6ms      3.5ms          -     25.4ms      5.6ms
  512     11.4ms      6.7ms          -       60ms     15.1ms
  640     22.2ms     10.5ms          -    116.2ms     26.6ms
  768     37.7ms     14.8ms          -    199.7ms     46.2ms
  896     60.7ms     20.1ms          -    317.9ms     71.1ms
 1024     90.2ms     30.7ms          -    488.3ms      114ms
```

## Matrix inverse (`f64`)

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      795ns      7.5µs      534ns       77ns      381ns
    8      2.2µs      8.9µs      995ns      825ns      794ns
   16      5.3µs       12µs      2.9µs        4µs      2.7µs
   32     15.2µs     29.9µs     10.3µs       19µs     10.8µs
   64     49.8µs     66.2µs     40.5µs    101.2µs     45.9µs
   96    127.1µs    122.7µs    182.1µs    285.3µs    119.2µs
  128    199.9µs    172.7µs    314.9µs    661.3µs      341µs
  192      543µs    419.8µs    587.1µs      2.2ms    963.8µs
  256        1ms    668.3µs      1.1ms      5.6ms        2ms
  384      2.9ms      1.4ms      2.4ms     18.7ms      5.1ms
  512      6.2ms      2.6ms      4.6ms     44.2ms     11.9ms
  640     11.5ms      5.5ms      7.2ms       83ms     19.2ms
  768     19.2ms      8.7ms     11.2ms    142.3ms     30.9ms
  896     29.5ms     12.9ms     16.7ms    223.1ms     44.1ms
 1024     43.5ms     18.2ms     23.9ms    347.1ms     68.8ms
```

## Square matrix singular value decomposition (`f64`)

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4        2µs      1.9µs        3µs      1.3µs      1.8µs
    8      9.7µs     24.4µs      8.2µs      3.9µs      9.1µs
   16       32µs     57.8µs     25.9µs     16.9µs     49.8µs
   32      107µs    132.1µs     90.3µs     95.9µs      222µs
   64    409.1µs    381.5µs    562.5µs      555µs    987.6µs
   96    903.9µs    913.1µs      1.7ms      1.7ms      2.7ms
  128      1.6ms      1.5ms      2.9ms      4.6ms      4.3ms
  192        4ms        4ms      6.7ms     14.8ms      9.9ms
  256      7.8ms        7ms     11.7ms     47.4ms     17.3ms
  384     20.9ms     15.1ms     25.8ms    121.1ms     42.9ms
  512     45.3ms     28.1ms       52ms    472.1ms     83.9ms
  640       80ms     44.5ms     79.1ms    665.7ms    133.8ms
  768    130.9ms     78.5ms    123.9ms      1.48s    208.9ms
  896    198.4ms    110.9ms    182.8ms      2.11s    295.4ms
 1024    297.8ms      152ms    253.8ms      3.95s    433.6ms
```

## Thin matrix singular value decomposition (`f64`)

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     73.4µs     73.5µs      311µs    127.5µs     76.7µs
    8    170.8µs    180.7µs    813.8µs    364.3µs    302.3µs
   16    440.4µs      513µs      2.1ms      1.4ms    775.5µs
   32      1.2ms      1.2ms      5.3ms      5.2ms      3.1ms
   64      3.4ms      3.2ms     15.7ms     19.9ms        8ms
   96      6.8ms      5.4ms     30.1ms     44.5ms     17.2ms
  128     11.2ms      8.3ms     47.4ms     79.4ms     30.9ms
  192     23.6ms     16.1ms       63ms    182.2ms     60.7ms
  256     40.7ms     25.5ms       84ms    353.1ms    101.3ms
  384     90.7ms     48.3ms      133ms    904.4ms    219.7ms
  512    164.7ms     80.2ms    303.4ms      2.02s    400.7ms
  640    258.7ms    119.7ms      289ms      3.24s    646.8ms
  768    381.7ms      187ms    440.1ms      5.15s      952ms
  896    532.6ms    252.7ms    550.2ms      7.23s      1.33s
 1024    724.4ms      327ms    849.6ms     10.64s      1.75s
```

## Hermitian matrix eigenvalue decomposition (`f64`)

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.3µs      1.3µs      1.4µs      675ns        1µs
    8      3.9µs        4µs      6.6µs      2.3µs      3.4µs
   16     13.2µs     13.6µs     25.9µs     10.3µs     12.5µs
   32     50.9µs     51.1µs    167.1µs     50.8µs     49.7µs
   64    223.9µs    217.5µs      1.2ms    293.9µs    211.2µs
   96      519µs    518.2µs      2.6ms      876µs      518µs
  128    931.7µs    885.5µs      5.4ms      1.9ms      1.1ms
  192      2.2ms      2.1ms       16ms      5.8ms      3.1ms
  256      4.1ms      3.5ms     33.9ms     13.2ms      6.6ms
  384     10.5ms      8.8ms    105.5ms     42.7ms     21.2ms
  512     21.9ms     16.5ms      175ms     99.3ms     51.4ms
  640     37.6ms     26.5ms    266.2ms    187.4ms     94.2ms
  768     60.4ms     38.1ms    403.3ms    322.6ms    161.9ms
  896     90.4ms     52.2ms    615.3ms    502.5ms    249.9ms
 1024    132.1ms     68.4ms      909ms    764.1ms      392ms
```

## Non Hermitian matrix eigenvalue decomposition (`f64`)

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      4.8µs      5.1µs      3.5µs          -      3.1µs
    8     15.6µs     16.7µs      9.6µs          -     10.5µs
   16     54.7µs     54.4µs     35.9µs          -     44.4µs
   32    270.7µs    235.6µs    172.6µs          -    199.3µs
   64      1.1ms      1.1ms        1ms          -      1.1ms
   96      2.7ms      2.9ms      5.5ms          -      3.1ms
  128      4.9ms      5.6ms     11.6ms          -      9.2ms
  192     14.4ms     14.3ms     22.4ms          -     26.9ms
  256     24.4ms     26.2ms     49.9ms          -     86.6ms
  384     56.4ms     62.6ms      107ms          -    246.1ms
  512    126.8ms    130.1ms    281.7ms          -    887.6ms
  640    205.8ms    192.6ms    415.6ms          -       1.2s
  768    323.5ms    285.6ms    547.2ms          -      2.84s
  896    438.1ms    375.8ms    704.3ms          -      3.67s
 1024    687.8ms    579.3ms    957.1ms          -         7s
```

# `f128`
Using `qd` crate.

## Matrix multiplication (`f128`)

Multiplication of two square matrices of dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      297ns      297ns          -          -      212ns
    8      1.1µs      8.2µs          -          -      1.6µs
   16      3.7µs      8.7µs          -          -     11.6µs
   32     22.9µs     15.7µs          -          -     88.8µs
   64    165.9µs     79.1µs          -          -    183.9µs
   96    553.2µs      204µs          -          -    419.8µs
  128      1.3ms    429.8µs          -          -      2.7ms
  192      4.3ms      1.3ms          -          -      3.3ms
  256     10.2ms      3.4ms          -          -     11.3ms
  384     34.3ms      9.5ms          -          -     26.5ms
  512     83.1ms     22.9ms          -          -     98.4ms
  640    161.1ms     44.6ms          -          -      166ms
  768    283.3ms     73.6ms          -          -    247.1ms
  896    450.8ms    118.1ms          -          -    478.9ms
 1024    687.4ms      182ms          -          -    647.7ms
```

## Triangular solve (`f128`)

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A`
is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      140ns      137ns          -          -      134ns
    8      1.2µs      1.2µs          -          -      867ns
   16      6.7µs     19.2µs          -          -      6.8µs
   32     29.6µs     86.1µs          -          -     54.4µs
   64    159.1µs    280.9µs          -          -    435.7µs
   96    474.1µs    326.7µs          -          -      1.5ms
  128    986.2µs    590.8µs          -          -      3.5ms
  192        3ms      1.2ms          -          -     11.8ms
  256      6.6ms      2.4ms          -          -       28ms
  384     20.9ms      6.9ms          -          -     94.6ms
  512     47.4ms     15.2ms          -          -      200ms
  640     92.5ms     31.4ms          -          -    390.7ms
  768    155.6ms     49.2ms          -          -    677.6ms
  896    245.2ms     79.8ms          -          -      1.08s
 1024    367.7ms    112.7ms          -          -      1.54s
```

## Triangular inverse (`f128`)

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      286ns      5.4µs          -          -      134ns
    8      1.3µs      7.9µs          -          -      864ns
   16      5.5µs     20.4µs          -          -      6.8µs
   32     22.6µs     44.1µs          -          -     54.4µs
   64    102.2µs    168.2µs          -          -    435.6µs
   96    298.5µs    376.5µs          -          -      1.5ms
  128      520µs    580.8µs          -          -      3.5ms
  192      1.6ms      1.1ms          -          -     11.8ms
  256        3ms      1.7ms          -          -       28ms
  384      9.4ms      3.8ms          -          -     94.6ms
  512     19.5ms      7.1ms          -          -      200ms
  640     38.7ms     13.8ms          -          -    390.6ms
  768       62ms     19.9ms          -          -    677.6ms
  896     95.2ms     34.2ms          -          -      1.08s
 1024    136.5ms     46.8ms          -          -      1.54s
```

## Cholesky decomposition (`f128`)

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      311ns      311ns          -          -      325ns
    8      840ns      840ns          -          -        1µs
   16      3.2µs      3.2µs          -          -      4.6µs
   32       20µs     47.9µs          -          -     26.6µs
   64    107.3µs    247.2µs          -          -    170.3µs
   96    291.1µs    604.2µs          -          -    528.3µs
  128    568.4µs      1.1ms          -          -      1.1ms
  192      1.6ms      2.2ms          -          -      3.6ms
  256      3.2ms      3.4ms          -          -      8.2ms
  384      9.3ms      6.9ms          -          -     26.6ms
  512     20.6ms     11.6ms          -          -     61.8ms
  640     37.7ms     19.8ms          -          -    119.3ms
  768     63.4ms     27.2ms          -          -    204.6ms
  896     97.6ms     45.3ms          -          -      323ms
 1024    142.9ms     60.8ms          -          -    480.6ms
```

## LU decomposition with partial pivoting (`f128`)

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix,
`L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      241ns      242ns          -          -      183ns
    8      620ns      640ns          -          -      725ns
   16      2.2µs      2.3µs          -          -      3.6µs
   32     17.8µs     42.4µs          -          -     37.8µs
   64    112.3µs    223.4µs          -          -      283µs
   96    326.9µs    535.2µs          -          -    937.3µs
  128    699.2µs    961.7µs          -          -      1.4ms
  192      2.1ms        2ms          -          -      3.6ms
  256      4.7ms      3.8ms          -          -      8.9ms
  384     14.7ms      8.4ms          -          -       23ms
  512     33.1ms     17.4ms          -          -     51.2ms
  640       64ms     29.8ms          -          -     91.5ms
  768    107.9ms     44.9ms          -          -    134.4ms
  896    169.4ms     71.2ms          -          -    223.7ms
 1024    251.3ms    103.9ms          -          -    350.7ms
```

## LU decomposition with full pivoting (`f128`)

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are
permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      345ns      331ns          -          -      261ns
    8      914ns      921ns          -          -      1.1µs
   16        4µs        4µs          -          -      5.7µs
   32     17.5µs     17.8µs          -          -     61.7µs
   64    101.6µs    101.2µs          -          -    509.9µs
   96    284.3µs    284.7µs          -          -      1.7ms
  128    635.4µs    632.7µs          -          -        4ms
  192        2ms        2ms          -          -     13.1ms
  256      4.7ms      4.7ms          -          -     31.1ms
  384     14.7ms     13.7ms          -          -    103.3ms
  512     35.5ms     21.4ms          -          -    245.1ms
  640     65.3ms     31.7ms          -          -    475.8ms
  768      117ms     47.3ms          -          -    827.2ms
  896    199.7ms     80.4ms          -          -      1.35s
 1024    331.4ms    163.3ms          -          -      2.08s
```

## QR decomposition with no pivoting (`f128`)

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper
triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      921ns      920ns          -          -      649ns
    8      3.1µs      3.1µs          -          -      2.2µs
   16     15.5µs     15.4µs          -          -      9.3µs
   32     69.5µs     69.5µs          -          -     51.9µs
   64    335.7µs    334.7µs          -          -      651µs
   96    847.4µs    845.4µs          -          -        2ms
  128      1.7ms      1.7ms          -          -      4.7ms
  192        5ms        5ms          -          -     14.8ms
  256     10.8ms      9.7ms          -          -     34.1ms
  384     33.7ms     22.3ms          -          -    110.2ms
  512       77ms     44.6ms          -          -    255.6ms
  640    140.2ms     74.1ms          -          -    492.3ms
  768    238.5ms      120ms          -          -    843.1ms
  896    374.6ms    179.4ms          -          -      1.33s
 1024    559.4ms      257ms          -          -      1.98s
```

## QR decomposition with column pivoting (`f128`)

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix,
`Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.5µs      1.4µs          -          -      1.2µs
    8      5.1µs      5.2µs          -          -      4.2µs
   16       23µs     22.9µs          -          -     17.2µs
   32    101.3µs    101.3µs          -          -     83.5µs
   64    534.9µs    572.9µs          -          -    479.6µs
   96      1.4ms      1.4ms          -          -      1.4ms
  128      2.8ms      2.8ms          -          -      3.1ms
  192      7.5ms      7.5ms          -          -      9.9ms
  256     15.8ms     11.9ms          -          -     23.5ms
  384     46.3ms     18.3ms          -          -     73.7ms
  512      102ms     29.5ms          -          -      171ms
  640    190.5ms     46.3ms          -          -    327.7ms
  768    321.1ms     70.5ms          -          -    562.8ms
  896    507.2ms    106.6ms          -          -    910.3ms
 1024    762.2ms    181.3ms          -          -       1.4s
```

## Matrix inverse (`f128`)

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.7µs      9.7µs          -          -      620ns
    8      5.6µs     21.1µs          -          -      2.8µs
   16     19.5µs     33.9µs          -          -       18µs
   32     91.3µs    112.2µs          -          -    148.4µs
   64    461.8µs    467.9µs          -          -      1.2ms
   96      1.4ms      1.1ms          -          -      3.9ms
  128      2.5ms        2ms          -          -      8.5ms
  192      7.8ms      4.4ms          -          -     27.4ms
  256     15.8ms      7.5ms          -          -     65.3ms
  384     49.7ms     17.9ms          -          -    213.6ms
  512    108.1ms     36.9ms          -          -    454.1ms
  640    212.3ms     67.2ms          -          -    878.3ms
  768    346.9ms    119.5ms          -          -       1.5s
  896    535.2ms    182.4ms          -          -      2.38s
 1024      783ms    259.4ms          -          -      3.43s
```

## Square matrix singular value decomposition (`f128`)

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     14.5µs       14µs          -          -     11.4µs
    8     94.4µs    120.9µs          -          -     74.6µs
   16    423.8µs    438.5µs          -          -    546.7µs
   32      1.8ms      1.6ms          -          -      2.8ms
   64      7.3ms      6.7ms          -          -       17ms
   96     17.2ms     14.1ms          -          -     45.6ms
  128     31.9ms     25.1ms          -          -     84.5ms
  192     77.6ms     53.2ms          -          -      208ms
  256    148.7ms       95ms          -          -    399.9ms
  384    388.9ms    215.1ms          -          -      1.04s
  512    788.4ms      403ms          -          -       2.1s
  640      1.37s    657.5ms          -          -      3.63s
  768      2.21s      1.01s          -          -       5.7s
  896      3.33s      1.44s          -          -      8.52s
 1024      4.81s      1.95s          -          -     12.27s
```

## Thin matrix singular value decomposition (`f128`)

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4    606.8µs    947.8µs          -          -    932.1µs
    8      1.8ms      2.1ms          -          -      3.4ms
   16      5.2ms      5.8ms          -          -      9.4ms
   32     16.9ms       16ms          -          -     35.7ms
   64     59.8ms     48.7ms          -          -      208ms
   96    119.3ms     80.3ms          -          -    467.9ms
  128    208.3ms    134.1ms          -          -    794.5ms
  192    460.3ms    269.9ms          -          -      1.73s
  256    874.6ms    503.8ms          -          -      2.96s
  384      1.79s    889.2ms          -          -      6.35s
  512      3.48s      1.82s          -          -     10.98s
  640      5.16s      2.48s          -          -     16.84s
  768      7.68s      3.67s          -          -     23.88s
  896     10.48s      4.81s          -          -      32.3s
 1024        15s      7.13s          -          -      41.8s
```

## Hermitian matrix eigenvalue decomposition (`f128`)

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      6.9µs      7.5µs          -          -      5.3µs
    8     31.6µs     32.2µs          -          -     22.7µs
   16    125.8µs    132.1µs          -          -     93.6µs
   32      552µs    554.7µs          -          -    449.5µs
   64      3.2ms      3.5ms          -          -      2.5ms
   96      8.5ms      7.8ms          -          -      7.1ms
  128     15.8ms     13.8ms          -          -     15.4ms
  192     40.4ms     30.3ms          -          -     46.7ms
  256     77.4ms     55.9ms          -          -    104.1ms
  384    203.4ms    122.9ms          -          -    332.1ms
  512    410.1ms      232ms          -          -    759.9ms
  640    710.5ms      372ms          -          -      1.46s
  768      1.14s    552.7ms          -          -      2.52s
  896      1.72s    796.4ms          -          -      4.02s
 1024      2.46s      1.07s          -          -      6.08s
```

## Non Hermitian matrix eigenvalue decomposition (`f128`)

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     17.2µs     16.9µs          -          -     11.5µs
    8    105.8µs    111.6µs          -          -     47.4µs
   16    532.6µs    596.4µs          -          -    200.8µs
   32      2.9ms      3.3ms          -          -      1.4ms
   64     19.7ms       21ms          -          -      9.9ms
   96     35.7ms       45ms          -          -     29.1ms
  128     72.2ms     82.2ms          -          -     67.4ms
  192    230.1ms    200.1ms          -          -    212.2ms
  256    427.6ms    374.9ms          -          -    537.8ms
  384      1.04s    897.3ms          -          -      1.66s
  512      2.15s      1.76s          -          -       4.1s
  640       3.6s      3.08s          -          -       7.7s
  768      5.64s      4.48s          -          -     13.76s
  896      8.11s       6.1s          -          -     21.47s
 1024     11.81s      8.61s          -          -      32.4s
```

# `c32`
## Matrix multiplication (`c32`)

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

## Triangular solve (`c32`)

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

## Triangular inverse (`c32`)

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

## Cholesky decomposition (`c32`)

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

## LU decomposition with partial pivoting (`c32`)

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

## LU decomposition with full pivoting (`c32`)

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

## QR decomposition with no pivoting (`c32`)

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

## QR decomposition with column pivoting (`c32`)

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

## Matrix inverse (`c32`)

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

## Square matrix singular value decomposition (`c32`)

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

## Thin matrix singular value decomposition (`c32`)

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

## Hermitian matrix eigenvalue decomposition (`c32`)

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

## Non Hermitian matrix eigenvalue decomposition (`c32`)

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

# `c64`
## Matrix multiplication (`c64`)

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

## Triangular solve (`c64`)

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

## Triangular inverse (`c64`)

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

## Cholesky decomposition (`c64`)

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

## LU decomposition with partial pivoting (`c64`)

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

## LU decomposition with full pivoting (`c64`)

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

## QR decomposition with no pivoting (`c64`)

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

## QR decomposition with column pivoting (`c64`)

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

## Matrix inverse (`c64`)

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

## Square matrix singular value decomposition (`c64`)

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

## Thin matrix singular value decomposition (`c64`)

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

## Hermitian matrix eigenvalue decomposition (`c64`)

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

## Non Hermitian matrix eigenvalue decomposition (`c64`)

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

# `c128`
Using the `num_complex` and `qd` crates.

## Matrix multiplication (`c128`)

Multiplication of two square matrices of dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.1µs      1.1µs          -          -      949ns
    8        7µs     38.6µs          -          -      8.5µs
   16     20.8µs     51.8µs          -          -     61.2µs
   32    109.7µs    105.6µs          -          -    466.8µs
   64    734.4µs    401.6µs          -          -    961.2µs
   96      2.4ms      990µs          -          -        2ms
  128      5.4ms        2ms          -          -       14ms
  192     17.9ms      6.2ms          -          -     16.2ms
  256       42ms       15ms          -          -     59.3ms
  384      141ms     41.4ms          -          -    150.8ms
  512    339.4ms     97.1ms          -          -      497ms
  640    652.9ms    187.2ms          -          -      794ms
  768      1.15s    306.2ms          -          -       1.2s
  896      1.84s    504.7ms          -          -      2.36s
 1024       2.8s    763.9ms          -          -      3.16s
```

## Triangular solve (`c128`)

Solving `AX = B` in place where `A` and `B` are two square matrices of dimension `n`, and `A` is a triangular matrix.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      420ns      418ns          -          -      707ns
    8        4µs        4µs          -          -      5.2µs
   16     28.3µs     75.2µs          -          -     40.6µs
   32    172.8µs    485.6µs          -          -      321µs
   64    886.5µs      1.6ms          -          -      2.6ms
   96      2.6ms      1.9ms          -          -      8.6ms
  128      5.2ms      3.3ms          -          -     20.4ms
  192     15.4ms      6.4ms          -          -     68.9ms
  256     32.2ms     12.3ms          -          -    163.3ms
  384     99.3ms     33.5ms          -          -    470.4ms
  512    217.4ms     72.3ms          -          -      1.12s
  640    414.1ms    144.6ms          -          -      2.05s
  768    690.1ms    214.7ms          -          -      3.55s
  896      1.07s      352ms          -          -      5.65s
 1024      1.59s      511ms          -          -      8.15s
```

## Triangular inverse (`c128`)

Computing `A^-1` where `A` is a square triangular matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      595ns      6.2µs          -          -      709ns
    8      3.3µs     12.1µs          -          -      5.2µs
   16     18.9µs       62µs          -          -     40.6µs
   32     89.3µs    182.6µs          -          -    320.4µs
   64    480.6µs    828.1µs          -          -      2.6ms
   96      1.5ms      1.8ms          -          -      8.6ms
  128      2.6ms        3ms          -          -     20.4ms
  192      7.9ms      5.9ms          -          -     69.2ms
  256     14.9ms      8.7ms          -          -    163.8ms
  384     45.8ms     18.6ms          -          -    470.5ms
  512     91.9ms     34.3ms          -          -      1.12s
  640    183.2ms     65.4ms          -          -      2.06s
  768    287.2ms     93.6ms          -          -      3.56s
  896    431.6ms    140.9ms          -          -      5.65s
 1024    609.4ms    205.1ms          -          -      8.15s
```

## Cholesky decomposition (`c128`)

Factorizing a square matrix with dimension `n` as `L×L.T`, where `L` is lower triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      440ns      440ns          -          -      874ns
    8      1.8µs      1.8µs          -          -      3.6µs
   16      8.3µs      8.3µs          -          -       18µs
   32       74µs    161.3µs          -          -    130.2µs
   64    446.6µs      1.1ms          -          -    909.2µs
   96      1.3ms      2.5ms          -          -      2.9ms
  128      2.4ms      4.4ms          -          -      6.3ms
  192      7.5ms      9.3ms          -          -     20.7ms
  256     13.8ms     13.4ms          -          -     46.7ms
  384     39.9ms     29.6ms          -          -    153.3ms
  512       90ms     47.1ms          -          -    358.1ms
  640    163.3ms     85.7ms          -          -      694ms
  768    277.9ms    114.6ms          -          -      1.19s
  896    427.5ms    169.2ms          -          -      1.88s
 1024    622.1ms    232.4ms          -          -      2.81s
```

## LU decomposition with partial pivoting (`c128`)

Factorizing a square matrix with dimension `n` as `P×L×U`, where `P` is a permutation matrix, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      578ns      546ns          -          -      1.8µs
    8      1.7µs      1.7µs          -          -        8µs
   16      7.1µs      7.1µs          -          -     43.3µs
   32     76.7µs    163.7µs          -          -    265.5µs
   64    534.7µs        1ms          -          -      1.8ms
   96      1.6ms      2.6ms          -          -      5.6ms
  128      3.3ms      4.7ms          -          -      8.4ms
  192     10.2ms      9.7ms          -          -     20.7ms
  256     21.5ms     18.2ms          -          -     48.4ms
  384     66.9ms     40.6ms          -          -    122.7ms
  512    146.9ms     81.7ms          -          -    271.3ms
  640    281.5ms    137.7ms          -          -    481.9ms
  768    468.3ms    204.8ms          -          -    718.4ms
  896    723.6ms    317.7ms          -          -      1.19s
 1024      1.06s    467.8ms          -          -      1.76s
```

## LU decomposition with full pivoting (`c128`)

Factorizing a square matrix with dimension `n` as `P×L×U×Q.T`, where `P` and `Q` are permutation matrices, `L` is unit lower triangular and `U` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      738ns      734ns          -          -      2.7µs
    8      2.4µs      2.4µs          -          -     15.7µs
   16       11µs       11µs          -          -    102.7µs
   32     60.4µs       61µs          -          -    774.1µs
   64      386µs    385.8µs          -          -      5.9ms
   96      1.2ms      1.2ms          -          -     19.5ms
  128      2.7ms      2.7ms          -          -     45.8ms
  192      8.7ms      8.7ms          -          -    152.9ms
  256     20.2ms     20.2ms          -          -    360.7ms
  384     65.9ms     59.8ms          -          -      1.21s
  512    154.6ms     82.8ms          -          -      2.86s
  640    307.2ms    120.8ms          -          -      5.58s
  768    546.5ms      220ms          -          -      9.65s
  896    876.7ms    398.4ms          -          -     15.34s
 1024      1.34s    666.6ms          -          -     22.93s
```

## QR decomposition with no pivoting (`c128`)

Factorizing a square matrix with dimension `n` as `QR`, where `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      1.8µs      1.8µs          -          -      1.6µs
    8      7.1µs      7.1µs          -          -      8.2µs
   16       31µs       31µs          -          -     52.6µs
   32    145.7µs    145.6µs          -          -    380.7µs
   64      1.5ms      1.5ms          -          -      4.2ms
   96        4ms        4ms          -          -     12.1ms
  128      8.3ms      8.3ms          -          -       28ms
  192     24.9ms     24.9ms          -          -     86.2ms
  256     54.8ms     49.3ms          -          -    195.4ms
  384    172.9ms    130.4ms          -          -    621.2ms
  512    398.6ms    274.4ms          -          -      1.43s
  640    663.9ms    388.3ms          -          -      2.73s
  768      1.13s      624ms          -          -      4.65s
  896      1.76s    938.1ms          -          -      7.32s
 1024      2.61s      1.36s          -          -     10.85s
```

## QR decomposition with column pivoting (`c128`)

Factorizing a square matrix with dimension `n` as `QRP`, where `P` is a permutation matrix, `Q` is unitary and `R` is upper triangular.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      2.6µs      2.6µs          -          -      2.7µs
    8      9.2µs      9.3µs          -          -     12.6µs
   16     39.7µs     39.8µs          -          -       70µs
   32    190.2µs    189.9µs          -          -    450.7µs
   64      1.2ms      1.2ms          -          -      3.2ms
   96      3.2ms        3ms          -          -     10.4ms
  128      6.6ms      6.3ms          -          -     24.4ms
  192     19.2ms     18.4ms          -          -       80ms
  256       42ms     30.7ms          -          -      187ms
  384    130.8ms     48.4ms          -          -      622ms
  512    298.8ms     81.9ms          -          -      1.47s
  640    582.8ms    137.2ms          -          -      2.85s
  768         1s    247.1ms          -          -      4.93s
  896      1.59s      479ms          -          -      7.84s
 1024       2.4s    732.7ms          -          -     11.58s
```

## Matrix inverse (`c128`)

Computing the inverse of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      4.5µs     14.9µs          -          -      3.5µs
    8     18.4µs     61.1µs          -          -     19.2µs
   16     72.3µs    126.7µs          -          -    126.5µs
   32    384.3µs    431.1µs          -          -    917.6µs
   64      2.2ms      2.1ms          -          -      6.9ms
   96      6.7ms      5.3ms          -          -       23ms
  128     12.2ms      9.1ms          -          -     49.4ms
  192     37.8ms     20.9ms          -          -    159.2ms
  256     73.6ms     36.2ms          -          -    376.7ms
  384    231.4ms     85.3ms          -          -      1.07s
  512    482.3ms    166.3ms          -          -      2.51s
  640    952.3ms    314.4ms          -          -       4.6s
  768      1.54s    525.3ms          -          -      7.84s
  896      2.34s    795.4ms          -          -      12.5s
 1024      3.37s      1.11s          -          -     18.11s
```

## Square matrix singular value decomposition (`c128`)

Computing the SVD of a square matrix with dimension `n`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     20.5µs     20.4µs          -          -     48.1µs
    8    120.5µs    151.5µs          -          -    465.9µs
   16    521.7µs      536µs          -          -    790.8µs
   32      2.2ms      2.1ms          -          -      4.5ms
   64     12.1ms     13.2ms          -          -       33ms
   96     30.3ms     28.5ms          -          -     97.3ms
  128     59.4ms     53.6ms          -          -    189.8ms
  192    159.1ms      113ms          -          -    540.2ms
  256    328.9ms    210.4ms          -          -      1.13s
  384    951.8ms    543.9ms          -          -      3.29s
  512      2.11s      1.15s          -          -      7.17s
  640      3.62s      1.69s          -          -     13.33s
  768      6.02s      2.74s          -          -     22.18s
  896       9.3s      4.03s          -          -     34.46s
 1024     13.76s      5.63s          -          -      51.4s
```

## Thin matrix singular value decomposition (`c128`)

Computing the SVD of a rectangular matrix with shape `(4096, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4      4.4ms      5.8ms          -          -     12.5ms
    8     10.4ms     11.8ms          -          -     64.9ms
   16     27.1ms     29.3ms          -          -     73.6ms
   32     80.4ms       77ms          -          -    282.9ms
   64      270ms    227.2ms          -          -      1.13s
   96      501ms    357.3ms          -          -      2.44s
  128    862.1ms    580.6ms          -          -      4.08s
  192      1.87s      1.13s          -          -       8.8s
  256      3.56s      2.22s          -          -     15.03s
  384      6.95s      3.71s          -          -     32.23s
  512     13.15s      7.14s          -          -     55.69s
  640     19.35s      9.65s          -          -     1:25.5
  768     28.88s     14.35s          -          -     2:01.3
  896     38.81s     18.59s          -          -     2:43.5
 1024     54.31s        27s          -          -     3:31.5
```

## Hermitian matrix eigenvalue decomposition (`c128`)

Computing the EVD of a hermitian matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     10.3µs       10µs          -          -      8.9µs
    8     39.8µs       43µs          -          -     49.9µs
   16    178.3µs      178µs          -          -    298.5µs
   32    813.1µs    813.2µs          -          -      1.9ms
   64      5.7ms      6.8ms          -          -       14ms
   96     15.2ms     14.6ms          -          -     44.8ms
  128     29.5ms       28ms          -          -    103.1ms
  192     80.5ms     66.1ms          -          -    340.2ms
  256    166.4ms    128.5ms          -          -    789.5ms
  384    484.4ms    303.1ms          -          -      2.63s
  512      1.07s    607.1ms          -          -      6.13s
  640      1.83s    903.6ms          -          -     11.93s
  768      3.04s      1.42s          -          -     20.67s
  896      4.69s      2.12s          -          -      32.6s
 1024      6.91s      2.91s          -          -     48.33s
```

## Non Hermitian matrix eigenvalue decomposition (`c128`)

Computing the EVD of a matrix with shape `(n, n)`.

```
    n       faer  faer(par)    ndarray   nalgebra      eigen
    4     32.6µs     32.8µs          -          -     34.3µs
    8    169.2µs    223.1µs          -          -    209.7µs
   16    893.4µs    923.5µs          -          -      1.4ms
   32      4.8ms        5ms          -          -     10.3ms
   64     30.7ms     33.9ms          -          -     72.6ms
   96    160.5ms    205.4ms          -          -    245.8ms
  128      304ms    358.9ms          -          -    570.2ms
  192    768.2ms    671.8ms          -          -      1.83s
  256      1.47s      1.22s          -          -      4.34s
  384      3.91s      3.09s          -          -     14.47s
  512      8.08s      6.21s          -          -     33.86s
  640      19.7s     16.15s          -          -     1:06.7
  768     31.82s     24.19s          -          -     1:56.2
  896      47.2s     34.63s          -          -     3:05.2
 1024     1:11.4     49.51s          -          -     4:42.2
```
