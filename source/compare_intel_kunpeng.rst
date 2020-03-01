**********************************
对比CPU
**********************************

架构、位数、指令集类型、厂家
==================================

.. csv-table::
   :header: 项目, Intel, kunpeng
   :widths: 30, 20, 20

   架构,        x86/x64,  aarch64
   比特,        32位/64位,     64位
   指令集类型,   CISC,     RISC
   指令集厂家,   Intel,    ARM
   CPU厂家,     Intel,    华为海思

lscpu对比：核数、cache、线程
====================================
lscpu可以列出CPU的详细信息。选取以下服务器：

==================== =========================================
服务器型号           CPU型号
==================== =========================================
泰山 TaiShan 2280 V2 Kunpeng 920-4826
戴尔 PowerEdge R730  Intel(R) Xeon(R) CPU E5-2630 v4 @ 2.20GHz
==================== =========================================


Kunpeng 920
----------------------------

.. code-block:: console

   Architecture:          aarch64          #ARM64架构
   Byte Order:            Little Endian    #小端
   CPU(s):                96               #cpu数量96核
   On-line CPU(s) list:   0-95             #cpu的编号
   Thread(s) per core:    1                #单线程， 不支持超线程
   Core(s) per socket:    48               #每个物理核的core数量
   Socket(s):             2                #一共有两个物理CPU
   NUMA node(s):          4                #numa节点
   Model:                 0
   BogoMIPS:              200.00
   L1d cache:             64K              #L1 data cache 数据cache
   L1i cache:             64K              #L1 instruction cache 指令cache
   L2 cache:              512K             #L2 cache
   L3 cache:              32768K           #L3 cache
   NUMA node0 CPU(s):     0-23             #numa节点1的CPU
   NUMA node1 CPU(s):     24-47            #numa节点2的CPU
   NUMA node2 CPU(s):     48-71            #numa节点3的CPU
   NUMA node3 CPU(s):     72-95            #numa节点4的CPU
   Flags:                 fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp
                          asimdhp cpuid asimdrdm jscvt fcma dcpop

Intel E5-2630
----------------------------

.. code-block:: console

   Architecture:          x86_64           #X86架构
   CPU op-mode(s):        32-bit, 64-bit   #同时支持32位和64位运行模式
   Byte Order:            Little Endian    #小端系统
   CPU(s):                40               #cpu数量40
   On-line CPU(s) list:   0-39             #cpu的编号
   Thread(s) per core:    2                #超线程数量，一般情况下
                  CPUs 40 = Thread(s) per core × Core(s) per socket × Socket(s)
   Core(s) per socket:    10               #每个物理核的core数量
   Socket(s):             2                #一共有两个物理CPU。
   NUMA node(s):          2                #两个numa节点
   Vendor ID:             GenuineIntel
   CPU family:            6
   Model:                 79
   Model name:            Intel(R) Xeon(R) CPU E5-2630 v4 @ 2.20GHz #主频2.2GHz
   Stepping:              1
   CPU MHz:               1720.227
   CPU max MHz:           3100.0000        #超频
   CPU min MHz:           1200.0000        #降频
   BogoMIPS:              4400.03
   Virtualization:        VT-x             #支持超线程技术
   L1d cache:             32K              #L1 data cache 数据cache
   L1i cache:             32K              #L1 instruction cache 指令cache
   L2 cache:              256K             #L2 cache
   L3 cache:              25600K           #L3 cache
   NUMA node0 CPU(s):     0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32,34,36,38 #numa节点1的CPU
   NUMA node1 CPU(s):     1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31,33,35,37,39 #numa节点1的CPU
   Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge
                          mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2
                          ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc
                          arch_perfmon pebs bts rep_good nopl xtopology
                          nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64
                          monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr
                          pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt
                          tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm
                          abm 3dnowprefetch epb cat_l3 cdp_l3 intel_pt ibrs ibpb
                          stibp tpr_shadow vnmi flexpriority ept vpid fsgsbase
                          tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm
                          cqm rdt_a rdseed adx smap xsaveopt cqm_llc
                          cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida
                          arat pln pts spec_ctrl intel_stibp
