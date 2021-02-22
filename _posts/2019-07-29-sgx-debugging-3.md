---
layout: post
title: SGX in-enclave debugging 3
date: 2019-07-29 11:12:00-0400
description: SGX in-enclave disassembler
---

这次要介绍一个在项目过程中我所用到的调试sao操作。借此机会打个广告：

https://github.com/StanPlatinum/elf-respect，https://github.com/StanPlatinum/cat-sgx, 欢迎感兴趣的人star、fork、给我们发issue。

在上面提到的项目里，我们做了一件这样的事情：我们把一个C/C++程序略微改造，经由我们自制的编译器编译之后，生成了一个relocatable Elf文件，也就是俗称.o文件。然后我们把这个relocatable Elf文件放到SGX enclave里执行。至于为什么要这么做，搞这么麻烦，这个可以去看我之后的paper或者blog，在此不表。

在做这个麻烦的事情时，我们不可避免地遇到了程序有bug的问题。然而如我前两篇blogs所说，在SGX enclave里想要调试一个被后来load进去的binary是非常困难的。

首先是打印方面的困难。想要打印一些信息，都得写Ocall。其实呢，写Ocall还算是简单的，到我们做这个项目时，由于二进制文件是事先编译好再load到enclave里的，所以我们只能先生成一个Ocall stub，然后在外部编译之前，用汇编写好调用这个Ocall stub的代码，同时在生成.o时也把需要用到的输出函数（比如puts）的实现（我们复用的musl-libc）也生成好并且include进来，才能在enclave被调用时，打印输出。

在使用puts的时候，我们也遇到了一些困难。因为puts只能输出字符串，没法输出变量，于是乎，我使用了一个自己实现的itoa函数 (or “sprintf”)，来将64位变量转换成字符串，再输出来看它们的值。同时，在enclave外部编译binary时，也需要把这个itoa函数生成的.o也链接进去。

其次是单步调试的困难。这个我们发现enclave里根本没单步！不管你是用gdb还是sgx-gdb，生成的是HW mode还是SIM mode，都不能单步。所以我们用到了sao操作：手写汇编调试。当然手写汇编理论上肯定可行，可以它还需要一个反汇编器的支持。而正好我们的项目里就有在enclave里实现了一个十分轻量的反汇编器。再配合sgx-gdb，可以打印出出现segmentation fault的地址和函数。拿到地址加之我们的反汇编信息，就能知道在具体哪一条汇编出现了问题。之后，就可以通过——1.插汇编，2.编译成binary，3.装载到enclave里执行——这三步循环，来进行调试了。