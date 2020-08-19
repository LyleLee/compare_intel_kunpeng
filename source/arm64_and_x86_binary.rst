#########################################
在x86上编译和运行arm64程序
#########################################


在x86上交叉编译arm64程序
------------------------------------

有时候我们只有x86环境， 想要编译出arm64的目标二进制。这个时候需要交叉编译工具， 交叉编译工具的安装有很多种。这里选择

::

    docker run --rm  dockcross/linux-arm64 > ./dockcross-linux-arm64
    dockcross-linux-arm64 bash -c '$CC hello_world_c.c -o hello_arm64 -static'

查看编译结果

::

    user1@intel:~/hello_world_c$ file hello_arm64
    hello_arm64: ELF 64-bit LSB executable, ARM aarch64, version 1 (GNU/Linux), statically linked, for GNU/Linux 4.10.8, with debug_info, not stripped


一般情况下执行会出错。 因为平台是x86的， 但是目标文件是arm64的。

::

    ./hello_arm64
    -bash: ./hello_arm64: cannot execute binary file: Exec format error


在x86平台上运行arm64目标程序 [#qemu_static]_
-------------------------------------------------

背后的原理是利用了kernel binfmt特性. [#binfmt-misc]_ 我们需要告诉内核哪种二进制需要哪种解释器去执行，
binfmt通过二进制文件的指定魔术字节识别二进制， 然后让注册的解释器执行。如果是我们自己操作，挂载虚拟文件系统、设置配置文件等操作。
一个便捷的方式是使用别人已经准备好的操作步骤。 执行下面的命令会在host上完成注册过程。

::

    docker run --rm --privileged multiarch/qemu-user-static --reset -p yes


一段C程序
----------------

.. code-block:: c

    #include <stdio.h>

    int main()
    {
            printf("hello world c\n");
    }

编译后拷贝到其他设备X86上运行，也可以用前面编译出来的hello_world, 注意编译要带 `-static`,要不然会因为x86主机上没有ARM上的ld解释器、c库等额外依赖导致报错

::

    gcc -o hello_world_c -static hello_world_c.c

.. code-block:: console

    user1@intel:~$ uname -m
    x86_64
    user1@intel:~$ file hello_world_c
    hello_world_c: ELF 64-bit LSB executable, ARM aarch64, version 1 (GNU/Linux), statically linked, for GNU/Linux 3.7.0, BuildID[sha1]=58b303f958cea549f2333edbc6e5e6ea56aa476f, not stripped
    user1@intel:~$ ./hello_world_c
    hello world c


一段Go程序
--------------

.. code-block:: go

    package main

    import "fmt"

    func main(){
            fmt.Println("hello world go")
    }


编译后拷贝到其他设备X86上运行,

::

    go build -o hello_world_go .

.. code-block:: console

    user1@intel:~$ uname -m
    x86_64
    user1@intel:~$ file hello_world_go
    hello_world_go: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, not stripped
    user1@intel:~$
    user1@intel:~$ ./hello_world_go
    hello world go


.. [#qemu_static] https://github.com/multiarch/qemu-user-static
.. [#binfmt-misc] https://www.kernel.org/doc/html/latest/admin-guide/binfmt-misc.html
