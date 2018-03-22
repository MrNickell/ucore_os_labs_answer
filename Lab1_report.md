## Lab1 report

**Reference：**

**1.[操作系统试验报告ucore_lab1](http://blog.csdn.net/a312024054/article/details/53353574)**

**2.[ucore操作系统实验笔记 - Lab1](https://segmentfault.com/a/1190000009386091)**



### [练习1]

####[练习1.1] 操作系统镜像文件 ucore.img 是如何一步一步生成的?(需要比较详细地解释 Makefile 中 每一条相关命令和命令参数的含义,以及说明命令导致的结果)：

首先打开Lab1_answer 输入

~~~makefile
make V=
~~~

输出结果首先为:

```Shell
+ cc kern/init/init.c 
#这里首先是使用了cc命令 CC是UNIX中常用的编译工具
#而在linux中用的是GCC，有一些在UNIX中写好的程序要放在linux中要指定命令CC编译器，所以将CC指定为GCC。其实就是一个东西。
#这里是将init.c编译 会生成一个.o文件
```

接下来是

~~~Shell
gcc -Ikern/init/ -fno-builtin -Wall -ggdb  -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/init/init.c -o obj/kern/init/init.o
#gcc -Ikern/init/表示将/kern/init目录作为第一个寻找头文件的目录
#-fno-builtin   表示只接受以"__"开头的内建函数
#-Wall  显示告警信息
#-ggdb  让gcc为gdb生成比较丰富的调试信息
#-m32   编译32位程序
#-gstabs 此选项以stabs格式生成调试信息，但是不包括gdb调试信息
#-nostdinc 不在标准系统目录中搜索头文件，只在-l指定的目录中搜索
#-fno-stack-protector 启用堆栈保护，为所有函数插入保护代码
~~~

接下来的这段命令都是为了生成Kernel模块(操作系统本身):

~~~Shell
+ cc kern/libs/stdio.c
gcc -Ikern/libs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/libs/stdio.c -o obj/kern/libs/stdio.o
+ cc kern/libs/readline.c
gcc -Ikern/libs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/libs/readline.c -o obj/kern/libs/readline.o
+ cc kern/debug/panic.c
gcc -Ikern/debug/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/debug/panic.c -o obj/kern/debug/panic.o
+ cc kern/debug/kdebug.c
gcc -Ikern/debug/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/debug/kdebug.c -o obj/kern/debug/kdebug.o
+ cc kern/debug/kmonitor.c
gcc -Ikern/debug/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/debug/kmonitor.c -o obj/kern/debug/kmonitor.o
+ cc kern/driver/clock.c
gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/clock.c -o obj/kern/driver/clock.o
+ cc kern/driver/console.c
gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/console.c -o obj/kern/driver/console.o
+ cc kern/driver/picirq.c
gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/picirq.c -o obj/kern/driver/picirq.o
+ cc kern/driver/intr.c
gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/intr.c -o obj/kern/driver/intr.o
+ cc kern/trap/trap.c
gcc -Ikern/trap/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/trap/trap.c -o obj/kern/trap/trap.o
+ cc kern/trap/vectors.S
gcc -Ikern/trap/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/trap/vectors.S -o obj/kern/trap/vectors.o
+ cc kern/trap/trapentry.S
gcc -Ikern/trap/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/trap/trapentry.S -o obj/kern/trap/trapentry.o
+ cc kern/mm/pmm.c
gcc -Ikern/mm/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/mm/pmm.c -o obj/kern/mm/pmm.o
+ cc libs/string.c
gcc -Ilibs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/  -c libs/string.c -o obj/libs/string.o
+ cc libs/printfmt.c
gcc -Ilibs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/  -c libs/printfmt.c -o obj/libs/printfmt.o
~~~

最后这个几个

~~~shell
#ld命令将这些目标文件变成可执行文件 比如这次的kernel
#-m elf_i386 使用elf_i386模拟器
#-nostdlib   不连接系统标准启动文件和标准库文件，只把指定的文件传递给连接器
#-Ttext addr  是连接时将初始地址重定向为addr （若不注明此，则程序的起始地址为0）
+ ld bin/kernel
ld -m    elf_i386 -nostdlib -T tools/kernel.ld -o bin/kernel  obj/kern/init/init.o 
obj/kern/libs/stdio.o obj/kern/libs/readline.o obj/kern/debug/panic.o obj/kern/debug/kdebug.o obj/kern/debug/kmonitor.o obj/kern/driver/clock.o obj/kern/driver/console.o obj/kern/driver/picirq.o obj/kern/driver/intr.o obj/kern/trap/trap.o obj/kern/trap/vectors.o obj/kern/trap/trapentry.o obj/kern/mm/pmm.o  obj/libs/string.o obj/libs/printfmt.o
~~~

接下来是编译bootLoader:

~~~shell
+ cc boot/bootasm.S
gcc -Iboot/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Os -nostdinc -c boot/bootasm.S -o obj/boot/bootasm.o
+ cc boot/bootmain.c
gcc -Iboot/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Os -nostdinc -c boot/bootmain.c -o obj/boot/bootmain.o
+ cc tools/sign.c
gcc -Itools/ -g -Wall -O2 -c tools/sign.c -o obj/sign/tools/sign.o
gcc -g -Wall -O2 obj/sign/tools/sign.o -o bin/sign
+ ld bin/bootblock
ld -m    elf_i386 -nostdlib -N -e start -Ttext 0x7C00 obj/boot/bootasm.o obj/boot/bootmain.o -o obj/bootblock.o

'obj/bootblock.out' size: 500 bytes
build 512 bytes boot sector: 'bin/bootblock' success!
~~~

生成ucore.img:

~~~shell
#dd -转换和拷贝文件
#if 代表输入文件 如果不指定if 默认就会从stdin中输入
#of 代表输出文件 如果不指定of 默认会讲stdout作为默认输出
#count 代表被赋值的块数
#/dev是一个字符设备 会不断返回0字节
#conv=notrunc    输入文件的时候，源文件不会被截断
#seek=1 从输出文件开头跳过1个块后再开始复制
dd if=/dev/zero of=bin/ucore.img count=10000
10000+0 records in
10000+0 records out
5120000 bytes (5.1 MB, 4.9 MiB) copied, 0.0149934 s, 341 MB/s
dd if=bin/bootblock of=bin/ucore.img conv=notrunc
1+0 records in
1+0 records out
512 bytes copied, 9.0074e-05 s, 5.7 MB/s
dd if=bin/kernel of=bin/ucore.img seek=1 conv=notrunc
146+1 records in
146+1 records out
74876 bytes (75 kB, 73 KiB) copied, 0.0002111 s, 355 MB/s
~~~



####[练习1.2] 一个被系统认为是符合规范的硬盘主引导区的特征是什么？

1. 一个磁盘的主引导区只有512个字节
2. 第510个字节是0x55 511个是0xAA


### [练习2]

#### [练习2.1] 一个被系统认为是符合规范的硬盘主引导区的特征是什么？



### [练习3]

（背景）

在32位的的系统中,CPU有两种工作模式—实模式与保护模式,直观的看,当我们的系统开始工作时CPU是处于实模式的,在之后经过一种机制的变化,CPU会进入保护模式。

在8086/8088的架构中，只有20根地址总线，所以可以访问的地址是2^20=1M，但由于8086/8088是16位地址模式，能够表示的地址范围是0-64K，所以为了在8086/8088下能够访问1M内存，Intel采取了分段的模式：16位段基地址:16位偏移。其绝对地址计算方法为：16位基地址左移4位+16位偏移=20位地址        **即：物理地址=段值*16 + 偏移**

**但这种方式引起了新的问题，通过上述分段模式，能够表示的最大内存为:**

**FFFF0h+FFFFh =10FFEFh**  **(1M + 64K - 16Bytes)**

**所以当访问超过1M的地址的时候,我们必须加第21根地址线来判断,将超出的值去除而重新按0来计算！所以A20代表的是第21根地址线！这称为A20Gate：如果A20 Gate被打开，则当程序员给出100000H-10FFEFH之间的地址的时候，系统将真正访问这块内存区域；如果A20Gate被禁止，则当程序员给出100000H-10FFEFH之间的地址的时候，系统仍然使用8086/8088的方式Motherfucker~**

实模式：实模式就是， 为了实现系统升级的兼容性，如80286的系统表现（包括80286以后的CPU）要与8086/8088 的系统表现一致，就需要80286 CPU访问100000H-10FFEFH之间的地址的时候， 按照对1M求模的方式进行， 无论A20地址线开启关闭与否， 这种内存访问情况 称为实模式。

保护模式：保护模式就是， 以A20地址线开启为前提，80286 CPU访问100000H-10FFEFH之间的地址的时候， 是访问真实的内存地址，不是求模访问，如访问100001H，就是真真切切地 访问 0x 100001H，而不是求模的 0x000001H 地址， 这种内存访问情况称为保护模式。



（解答）

分析bootloader 进入保护模式的过程。

从`%cs=0 $pc=0x7c00`，进入后

首先清理环境：包括将flag置0和将段寄存器置0

```assembly
	.code16
	    cli
	    cld
	    xorw %ax, %ax
	    movw %ax, %ds
	    movw %ax, %es
	    movw %ax, %ss
```

开启A20：通过将键盘控制器上的A20线置于高电位，全部32条地址线可用，
可以访问4G的内存空间。

```Assembly
	seta20.1:               # 等待8042键盘控制器不忙
	    inb $0x64, %al      # 
	    testb $0x2, %al     #
	    jnz seta20.1        #
	
	    movb $0xd1, %al     # 发送写8042输出端口的指令
	    outb %al, $0x64     #
	
	seta20.1:               # 等待8042键盘控制器不忙
	    inb $0x64, %al      # 
	    testb $0x2, %al     #
	    jnz seta20.1        #
	
	    movb $0xdf, %al     # 打开A20
	    outb %al, $0x60     # 
```



~~~assembly
#【补充】保护模式下，有两个段表：GDT（Global Descriptor Table）和LDT（Local Descriptor Table），每一张段表可以包含8192 (2^13)个描述符[1]，因而最多可以同时存在2 * 2^13 = 2^14个段。虽然保护模式下可以有这么多段，逻辑地址空间看起来很大，但实际上段并不能扩展物理地址空间，很大程度上各个段的地址空间是相互重叠的。目前所谓的64TB（2^(14+32)=2^46）逻辑地址空间是一个理论值，没有实际意义。在32位保护模式下，真正的物理空间仍然只有2^32字节那么大。注：在ucore lab中只用到了GDT，没有用LDT。
保护模式之所以被称之为保护模式 即可认为多了GDT的存在,在实模式下任何程序都能访问整个1M的空间,而在保护模式,通过分段的方式，程序并不能访问整个内存空间。
~~~



初始化GDT表：一个简单的GDT表和其描述符已经静态储存在引导区中，载入即可

```assembly
lgdt gdtdesc

#我们根据CPU给的逻辑地址分离出段选择子。
#利用这个段选择子选择一个段描述符。
#将段描述符里的Base Address和段选择子的偏移量相加而得到线性地址。这个地址就是我们需要的地址。
```

进入保护模式：通过将cr0寄存器PE位置1便开启了保护模式

```Assembly
	    movl %cr0, %eax
	    orl $CR0_PE_ON, %eax
	    movl %eax, %cr0
```

通过长跳转更新cs的基地址

```assembly
	 ljmp $PROT_MODE_CSEG, $protcseg
	.code32
	protcseg:
```

设置段寄存器，并建立堆栈

```assembly
	    movw $PROT_MODE_DSEG, %ax
	    movw %ax, %ds
	    movw %ax, %es
	    movw %ax, %fs
	    movw %ax, %gs
	    movw %ax, %ss
	    movl $0x0, %ebp
	    movl $start, %esp
```

转到保护模式完成，进入boot主方法

```assembly
	    call bootmain
```

