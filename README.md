# operating-system
## 一、计算机系统漫游
### 1.2 GCC编译器驱动程序读取源程序文件hello.c，并把它翻译成一个可执行文件hello。这个翻译过程可分为四个阶段：预处理器、编译器、汇编器和链接器。
- 预处理阶段：预处理器（CPP）读取头文件并插入程序文本中，得到另一个c程序，通常以.i作为文件扩展名。
- 编译阶段：编译器（ccl）将文本文件hello.i翻译成文本文件hello.s,它包含一个汇编语言程序。
- 汇编阶段：汇编器（as）将hello.s翻译成机器语言指令，把这种指令打包成一种叫做可重定位目标程序，并将结果保存在目标文件hello.o中，hello.o是一个二进制文件.
- 链接阶段：hello程序调用了printf函数，位于一个名为printf.o的单独的预编译好了的文件中，链接器（ld）将printf.o合并到hello中，得到可执行的hello文件，可以被加载到内存中，有系统执行。
### 1.3 了解编译系统如何工作的好处
- 优化程序性能
- 理解链接时出现的错误
- 避免安全漏洞
### 1.4 处理器读并解释存储在内存中的指令
shell是一个命令解释器，输入一个命令行，如果第一个单词不是一个内置的shell命令，那么shell会假设这是一个可执行文件，它将加载并执行这个文件
#### 1.4.1 系统的硬件组成
1.总线
2.I/O设备
3.主存
主存是一个临时存储设备，在处理器执行程序时，用来存放程序和程序处理的数据。主存是由一组动态随机存储器（DRAM）芯片组成的。
4.处理器
中央处理单元（CPU），简称处理器，解释（或执行）存储在主存指令的引擎。处理器的核心是一个大小一个字的存储设备（或寄存器），称为程序计数器（PC），在任何时刻，PC都指向主存中的某条机器语言指令。
简单操作：
- 加载：从主存复制一个字或一个字节到寄存器，覆盖寄存器原有内容
- 存储：从寄存器复制一个字节或一个字到主存的某个位置，覆盖原有内容
- 操作：把两个寄存器的内容复制到ALU做算术运算，将结果存放到一个寄存器中，覆盖原有内容
- 跳转：从指令本身中抽取一个字，复制到程序计数器PC中，覆盖原有内容
#### 1.4.2 运行hello程序
### 1.5 高速缓存
高速缓存存储器（cache memory），作为暂时的集结区域，存放处理器近期可能会需要的信息
### 1.6 存储设备形成层次结构
![image](https://user-images.githubusercontent.com/54796147/226382460-f5945e90-54d1-47c9-a509-1bd2be985325.png)
### 1.7 操作系统管理硬件
操作系统看成是应用程序和硬件之间插入的一层软件，有两个基本功能
- 防止硬件被失控的应用程序滥用
- 向应用程序提供简单一致的机制来控制复杂而又通常大不相同的低级硬件设备

操作系统通过几个基本的抽象概念（进程、虚拟内存和文件）来实现这两个功能。
![image](https://user-images.githubusercontent.com/54796147/226385130-fa6ba030-7259-45ef-a9e8-3e236b93fd0a.png)
#### 1.7.1 进程
进程是操作系统对一个正在进行的程序的一种抽象。在一个系统上可以同时运行多个进程，而每个进程都好像在独占的使用硬件。而并发运行，则是一个进程的指令和另一个进程的指令是交错执行的。
操作系统实现处理器在进程间切换的交错执行的机制称为上下文切换。未简化讨论，只讨论包含一个CPU的单处理器系统的情况。

操作系统保持跟踪进程运行所需的所有状态信息。 这种状态， 也就是上下文， 它包括许多信息， 例如 PC 和寄存器文件的当前值， 以及主存的内容。 在任何一个时刻， 单处理器系统都只能
执行一个进程的代码。 当操作系统决定要把控制权从当前进程转移到某个新进程时， 就会进行上下文切换， 即保存当前进程的上下文、 恢复新进程的上下文， 然后将控制权传递到新进程。 新进
程就会从上次停止的地方开始。
#### 1.7.2 线程
尽管通常我们认为一个进程只有单一的控制流， 但是在现代系统中， 一个进程实际上可以由多个称为线程的执行单元组成， 每个线程都运行在进程的上下文中， 并共享同样的代码和全局数据。
#### 1.7.3 虚拟存储器
虚拟存储器是一个抽象概念， 它为每个进程提供了一个假象， 即每个进程都在独占地使用主存。 每个进程看到的是一致的存储器， 称为虚拟地址空间。 
在 Linux 中， 地址空间最上面的区域是为操作系统中的代码和数据保留的， 这对所有进程来说都是一样的。 地址空间的底部区域存放用户进程定义的代码和数据。 请注意， 图中的地址是从下往上增大的。
![image](https://user-images.githubusercontent.com/54796147/226503222-6e54e706-11c5-4ce2-9be9-19de3351c5ef.png)
- 程序代码和数据。 对于所有的进程来说， 代码是从同一固定地址开始， 紧接着的是和 C 全局变量相对应的数据位置。 代码和数据区是直接按照可执行目标文件的内容初始化的， 在示例中就是可执行文件 hello。
- 堆。 代码和数据区后紧随着的是运行时堆。 代码和数据区是在进程一开始运行时就被规定了大小， 与此不同， 当调用如 malloc 和 free 这样的 C 标准库函数时， 堆可以在运行时动态地扩展和收缩。
- 共享库。 大约在地址空间的中间部分是一块用来存放像 C 标准库和数学库这样共享库的代码和数据的区域。 
- 栈。 位于用户虚拟地址空间顶部的是用户栈， 编译器用它来实现函数调用。 和堆一样， 用户栈在程序执行期间可以动态地扩展和收缩。 特别是每次我们调用一个函数时， 栈就会增长 ； 从一个函数返回时， 栈就会收缩。 
- 内核虚拟存储器。 内核总是驻留在内存中， 是操作系统的一部分。 地址空间顶部的区域是为内核保留的， 不允许应用程序读写这个区域的内容或者直接调用内核代码定义的函数。
#### 1.7.4 文件
文件就是字节序列， 仅此而已。 每个 I/O 设备， 包括磁盘、 键盘、 显示器， 甚至网络， 都可以视为文件。 系统中的所有输入输出都是通过使用一小组称为 Unix I/O 的系统函数调用读写文件来实现的。

### 1.9 重要主题
#### 1.9.1 并发和并行
数字计算机的整个历史中， 有两个需求是驱动进步的持续动力 ： 一个是我们想要计算机做得更多， 另一个是我们想要计算机运行得更快。 当处理器同时能够做更多事情时， 这两个因素都会改进。 我们用的术语并发（ concurrency） 是一个通用的概念， 指一个同时具有多个活动的系统 ；而术语并行（ parallelism） 指的是用并发使一个系统运行得更快。 并行可以在计算机系统的多个抽象层次上运用。 在此， 我们按照系统层次结构中由高到低的顺序重点强调三个层次。
1. 线程级并发
2. 指令级并行
在较低的抽象层次上， 现代处理器可以同时执行多条指令的属性称为指令级并行。 
3. 单指令、 多数据并行
在最低层次上， 许多现代处理器拥有特殊的硬件， 允许一条指令产生多个可以并行执行的操作， 这种方式称为单指令、 多数据， 即 SIMD 并行。
#### 1.9.2 计算机系统中抽象的重要性
抽象的使用是计算机科学中最为重要的概念之一。 例如， 为一组函数规定一个简单的应用程序接口（ API） 就是一个很好的编程习惯， 程序员无需了解它内部的工作便可以使用这些代码。 
![image](https://user-images.githubusercontent.com/54796147/226505843-73659e97-b75d-4a80-9903-e004835af6d8.png)
## 二、信息的表示和处理
### 2.1 信息存储
大多数计算机使用 8 位的块， 或者字节（byte）， 作为最小的可寻址的存储器单位， 而不是在存储器中访问单独的位。 机器级程序将存储器视为一个非常大的字节数组，称为虚拟存储器（virtual memory）。 存储器的每个字节都由一个唯一的数字来标识， 称为它的地址（address）， 所有可能地址的集合称为虚拟地址空间（virtual address space）。 顾名思义， 这个虚拟地址空间只是一个展现给机器级程序的概念性映像。 实际的实现（见第 9 章） 是将随机访问存储器（RAM）、 磁盘存储器、 特殊硬件和操作系统软件结合起来， 为程序提供一个看上去统一的字节数组。
#### 2.1.1 十六进制表示法
一个字节由 8 位组成,用十六进制书写， 一个字节的值域为 0016 ～ FF16。在 C 语言中， 以 0x 或 0X 开头的数字常量被认为是十六进制的值。
#### 2.1.2 字
每台计算机都有一个字长（ word size）， 指明整数和指针数据的标称大小（ nominal size）。 因为虚拟地址是以这样的一个字来编码的， 所以字长决定的最重要的系统参数就是虚拟地址空间的最大大小。 也就是说， 对于一个字长为 w 位的机器而言， 虚拟地址的范围为 0 ～ 2w-1， 程序最多访问 2w 个字节。
