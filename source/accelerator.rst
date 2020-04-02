*********************
加速器
*********************


Kunpeng 920
-----------------------

.. code-block:: console
    :caption: zlib

    [user1@kunpeng920 ~]$ opensoftware/lzbench/lzbench -t16,16 -ezlib silesia.tar
    lzbench 1.8 (64-bit Linux)   Assembled by P.Skibinski
    Compressor name         Compress. Decompress. Compr. size  Ratio Filename
    memcpy                   6391 MB/s  6401 MB/s   211947520 100.00 silesia.tar
    zlib 1.2.11 -1             65 MB/s   216 MB/s    77259029  36.45 silesia.tar
    zlib 1.2.11 -2             60 MB/s   221 MB/s    75002277  35.39 silesia.tar
    zlib 1.2.11 -3             47 MB/s   226 MB/s    72967040  34.43 silesia.tar
    zlib 1.2.11 -4             42 MB/s   226 MB/s    71002817  33.50 silesia.tar
    zlib 1.2.11 -5             30 MB/s   227 MB/s    69161685  32.63 silesia.tar
    zlib 1.2.11 -6             21 MB/s   229 MB/s    68228431  32.19 silesia.tar
    zlib 1.2.11 -7             17 MB/s   230 MB/s    67939647  32.05 silesia.tar
    zlib 1.2.11 -8             11 MB/s   231 MB/s    67693427  31.94 silesia.tar
    zlib 1.2.11 -9           8.56 MB/s   231 MB/s    67644548  31.92 silesia.tar
    done... (cIters=1 dIters=1 cTime=16.0 dTime=16.0 chunkSize=1706MB cSpeed=0MB)

.. code-block:: console
    :caption: lz4

    [user1@kunpeng920 ~]$ opensoftware/lzbench/lzbench -t16,16 -elz4 silesia.tar
    lzbench 1.8 (64-bit Linux)   Assembled by P.Skibinski
    Compressor name         Compress. Decompress. Compr. size  Ratio Filename
    memcpy                   6381 MB/s  6396 MB/s   211947520 100.00 silesia.tar
    lz4 1.9.2                 354 MB/s  2237 MB/s   100880800  47.60 silesia.tar
    done... (cIters=1 dIters=1 cTime=16.0 dTime=16.0 chunkSize=1706MB cSpeed=0MB)


Intel 6248
-----------------------

.. code-block:: console
    :caption: zlib

    [user1@intel6248 opensoftware]$ lzbench/lzbench -t16,16 -ezlib silesia.tar
    lzbench 1.8 (64-bit Linux)   Assembled by P.Skibinski
    Compressor name         Compress. Decompress. Compr. size  Ratio Filename
    memcpy                   4093 MB/s  3509 MB/s   211947520 100.00 silesia.tar
    zlib 1.2.11 -1             75 MB/s   278 MB/s    77259029  36.45 silesia.tar
    zlib 1.2.11 -2             67 MB/s   287 MB/s    75002277  35.39 silesia.tar
    zlib 1.2.11 -3             52 MB/s   296 MB/s    72967040  34.43 silesia.tar
    zlib 1.2.11 -4             49 MB/s   288 MB/s    71002817  33.50 silesia.tar
    zlib 1.2.11 -5             35 MB/s   291 MB/s    69161685  32.63 silesia.tar
    zlib 1.2.11 -6             23 MB/s   297 MB/s    68228431  32.19 silesia.tar
    zlib 1.2.11 -7             19 MB/s   298 MB/s    67939647  32.05 silesia.tar
    zlib 1.2.11 -8             12 MB/s   300 MB/s    67693427  31.94 silesia.tar
    zlib 1.2.11 -9           9.76 MB/s   300 MB/s    67644548  31.92 silesia.tar
    done... (cIters=1 dIters=1 cTime=16.0 dTime=16.0 chunkSize=1706MB cSpeed=0MB)

.. code-block:: console
    :caption: lz4

    [user1@intel6248 opensoftware]$ lzbench/lzbench -t16,16 -elz4 silesia.tar
    lzbench 1.8 (64-bit Linux)   Assembled by P.Skibinski
    Compressor name         Compress. Decompress. Compr. size  Ratio Filename
    memcpy                   3981 MB/s  3457 MB/s   211947520 100.00 silesia.tar
    lz4 1.9.2                 509 MB/s  2932 MB/s   100880800  47.60 silesia.tar
    done... (cIters=1 dIters=1 cTime=16.0 dTime=16.0 chunkSize=1706MB cSpeed=0MB)


AMD
---------------------