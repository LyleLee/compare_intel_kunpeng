*********************
对比加解压能力
*********************

鲲鹏加速器目前包括以下加速引擎

+ ZIP Engine ZIP加速引擎
+ SEC Engine 硬件安全加速引擎（SEC， Security）
+ HPRE Engine 高性能RSA加速引擎（HPRE，High Performance RSA Engine）
+ RDE Engine RAID DIF运算加速引擎（RDE，RAID DIF Engine）

.. csv-table:: ZIP压缩解压对比
    :header: 加速引擎, Kunpeng, Kunpeng ZIP加速引擎, Intel, Intel ZIP 加速引擎

    ZIP压缩(Ratio 36%),   55 MB/s,  2380 MB/s(Ratio 45%), 75 MB/s, 待测
    ZIP解压(Ratio 36%),   219 MB/S, 1530 MB/s(Ratio 45%), 278 MB/s, 待测


软件压缩，可以看到在同等压缩效率的情况下， Intel速度要快。 使用加速器时加解压速度获得大幅度提升，但是硬件能获得的压缩率没有软件好。





安装鲲鹏加速器
================

首先需要具备一下条件

1. CPU必须是kunpeng 920
2. BMC需要升级到V365 [#v365]_
3. 安装有加速器license。 在 `BMC web->配置->许可证管理` 添加。如何购买不在讨论范围。
4. lspci确认加速引擎已经存在。

.. code-block:: console

    [user@kunpeng920 ~]$ lspci | grep Engine
    75:00.0 Processing accelerators: Huawei Technologies Co., Ltd. HiSilicon ZIP Engine (rev 21)
    76:00.0 Network and computing encryption device: Huawei Technologies Co., Ltd. HiSilicon SEC Engine (rev 21)
    78:01.0 RAID bus controller: Huawei Technologies Co., Ltd. HiSilicon RDE Engine (rev 21)
    79:00.0 Network and computing encryption device: Huawei Technologies Co., Ltd. HiSilicon HPRE Engine (rev 21)


主要包括以下安装步骤， 如github项目介绍 [#kae_github]_

1. 安装OpenSSL。 只有OpenSSL >= 1.1.1a 版本才能够使用加速器
2. 安装加速器驱动KAEdriver。
3. 安装warpdrive库文件
4. 安装KAE鲲鹏加速器引擎
5. 如果使用ZIP加速引擎，需要将zlib更新到1.2.11并且打入KAE patch [#kae_KAEzip]_




ZIP 加解压测试结果
=========================

评测工具是lzbench [#lzbench]_


Kunpeng 920测试结果

.. code-block:: console

    [user1@kunpeng920 lzbench]$ ./lzbench -t16,16 -ezlib silesia.tar
    lzbench 1.8 (64-bit Linux)   Assembled by P.Skibinski
    Compressor name         Compress. Decompress. Compr. size  Ratio Filename
    memcpy                   7274 MB/s  7239 MB/s   211947520 100.00 silesia.tar
    zlib 1.2.11 -1             55 MB/s   219 MB/s    77259029  36.45 silesia.tar
    zlib 1.2.11 -2             51 MB/s   223 MB/s    75002277  35.39 silesia.tar
    zlib 1.2.11 -3             42 MB/s   229 MB/s    72967040  34.43 silesia.tar
    zlib 1.2.11 -4             38 MB/s   228 MB/s    71002817  33.50 silesia.tar
    zlib 1.2.11 -5             28 MB/s   230 MB/s    69161685  32.63 silesia.tar
    zlib 1.2.11 -6             20 MB/s   232 MB/s    68228431  32.19 silesia.tar
    zlib 1.2.11 -7             16 MB/s   233 MB/s    67939647  32.05 silesia.tar
    zlib 1.2.11 -8             10 MB/s   234 MB/s    67693427  31.94 silesia.tar
    zlib 1.2.11 -9           8.27 MB/s   234 MB/s    67644548  31.92 silesia.tar
    done... (cIters=1 dIters=1 cTime=16.0 dTime=16.0 chunkSize=1706MB cSpeed=0MB)
    [user1@kunpeng920 lzbench]$ LD_PRELOAD=/usr/local/zlib/lib/libz.so.1.2.11 ./lzbench -t16,16 -ezlib silesia.tar
    lzbench 1.8 (64-bit Linux)   Assembled by P.Skibinski
    Compressor name         Compress. Decompress. Compr. size  Ratio Filename
    memcpy                   7315 MB/s  7279 MB/s   211947520 100.00 silesia.tar
    zlib 1.2.11 -1           2380 MB/s  1531 MB/s    97081116  45.80 silesia.tar
    zlib 1.2.11 -2           2094 MB/s  1530 MB/s    97081315  45.80 silesia.tar
    zlib 1.2.11 -3           2093 MB/s  1530 MB/s    97081388  45.80 silesia.tar
    zlib 1.2.11 -4           2093 MB/s  1531 MB/s    97081235  45.80 silesia.tar
    zlib 1.2.11 -5           2093 MB/s  1530 MB/s    97081838  45.80 silesia.tar
    zlib 1.2.11 -6           1743 MB/s  1531 MB/s    97081749  45.80 silesia.tar
    zlib 1.2.11 -7           2092 MB/s  1531 MB/s    97081527  45.80 silesia.tar
    zlib 1.2.11 -8           2093 MB/s  1530 MB/s    97081553  45.80 silesia.tar
    zlib 1.2.11 -9           2093 MB/s  1530 MB/s    97081604  45.80 silesia.tar
    done... (cIters=1 dIters=1 cTime=16.0 dTime=16.0 chunkSize=1706MB cSpeed=0MB)
    [user1@kunpeng920 lzbench]$


Intel 6248测试结果

.. code-block:: console

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

.. [#lzbench] 评测工具lzbench https://github.com/inikep/lzbench
.. [#kae_github] KAE github项目地址 https://github.com/kunpengcompute/KAE
.. [#kae_KAEzip] KAE ZIP引擎 https://github.com/kunpengcompute/KAEzip
.. [#kae_install] 加速器安装 https://bbs.huaweicloud.com/forum/thread-34619-1-1.html
.. [#kae_qa] 加速器参考资料 https://bbs.huaweicloud.com/forum/thread-30230-1-1.html
.. [#v365] BMC版本V365以上 https://support.huawei.com/enterprise/zh/doc/EDOC1100048792/ba20dd15
