************************************
比较指令差异
************************************

以一段C代码为例，实现两数相乘

.. literalinclude:: demo_code/add.c
    :language: c

编译并且反汇编出可执行程序的汇编代码：

.. code-block:: shell

    gcc add.c
    objdump -d add.out

.. code-block:: objdump

    Kunpeng                                          |   intel
                                                     |
    0000000000400600 <main>:                         │  000000000040052d <main>:
    400600:   a9be7bfd  stp   x29, x30, [sp,#-32]!   │  40052d:  55                    push   %rbp
    400604:   910003fd  mov   x29, sp                │  40052e:  48 89 e5              mov    %rsp,%rbp
    400608:   52800040  mov   w0, #0x2          // #2│  400531:  48 83 ec 10           sub    $0x10,%rsp
    40060c:   b9001fa0  str   w0, [x29,#28]          │  400535:  c7 45 fc 02 00 00 00  movl   $0x2,-0x4(%rbp)
    400610:   528000a0  mov   w0, #0x5          // #5│  40053c:  c7 45 f8 05 00 00 00  movl   $0x5,-0x8(%rbp)
    400614:   b9001ba0  str   w0, [x29,#24]          │  400543:  8b 45 fc              mov    -0x4(%rbp),%eax
    400618:   b9401fa1  ldr   w1, [x29,#28]          │  400546:  0f af 45 f8           imul   -0x8(%rbp),%eax
    40061c:   b9401ba0  ldr   w0, [x29,#24]          │  40054a:  89 45 f4              mov    %eax,-0xc(%rbp)
    400620:   1b007c20  mul   w0, w1, w0             │  40054d:  8b 45 f4              mov    -0xc(%rbp),%eax
    400624:   b90017a0  str   w0, [x29,#20]          │  400550:  89 c6                 mov    %eax,%esi
    400628:   90000000  adrp  x0, 400000 <_init-0x428│  400552:  bf 00 06 40 00        mov    $0x400600,%edi
    40062c:   911b8000  add   x0, x0, #0x6e0         │  400557:  b8 00 00 00 00        mov    $0x0,%eax
    400630:   b94017a1  ldr   w1, [x29,#20]          │  40055c:  e8 af fe ff ff        callq  400410 <printf@plt>
    400634:   97ffff97  bl    400490 <printf@plt>    │  400561:  c9                    leaveq
    400638:   a8c27bfd  ldp   x29, x30, [sp],#32     │  400562:  c3                    retq
    40063c:   d65f03c0  ret                          │  400563:  66 2e 0f 1f 84 00 00  nopw   %cs:0x0(%rax,%rax,1)
                                                     │  40056a:  00 00 00
                                                     │  40056d:  0f 1f 00              nopl   (%rax)


我们总结差如下

1. ARM的指令都是8字节的（64位机上），intel 是可变长的，如第二行的push指令是1字节，编码是55。
   第三行的mov指令是3字节，编码是48 89 e5。
2. ARM和intel的寻址方式表示不一样
3. ARM的寄存器是X（8字节）或者W（4字节）加数字。 intel的寄存器是eax, ebx等名称

