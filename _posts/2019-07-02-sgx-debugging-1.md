---
layout: post
title: SGX in-enclave debugging 1
date: 2019-07-02 11:12:00-0400
description: SGX in-enclave debugging
---

最近在[Ascii0x03](https://github.com/a3135134)的帮助下，做了一个能在enclave内部第三方库中对信息进行输出的小demo。

“在第三方库中对enclave内部信息进行输出”，这句话听起来挺绕的，有人会问为什么要做这么复杂的事情呢，我的回答是：确实是事出有因。在最开始的时候，我只是想把一个第三方库(capstone)的某些功能放到sgx里。然而在实现的过程中，我发现这个看似简单的工作其实并没有那么容易，总会需要输出一些东西，借此来查看是哪里出了问题。具体遇到的坑在本文中就不详述，这里就讲讲我在实现时是怎么进行调试的。

在讲demo之前我得先介绍一下相关的背景。首先，sgx运行起来所生成的实例叫做enclave。这个enclave是一个异常麻烦的东西：为了保证运行环境的安全，任何的系统调用，某些特权指令，都不能在enclave里面运行。因此，想要单步调试enclave基本不可能。Jo Van Bulck写过一个工具叫sgx-step，这个玩意儿也不是用来调试的，而是一种侧信道攻击。所以说，想要调试enclave内部的代码，最直接的方法就是在写代码的时候插入一些输出语句，在运行时打印出来观察变量的变化。SGX SDK也提供了GDB的集成，还算不错。但是如果想调试不是SDK编译的部分(比如之后load进来的binary，在Graphene-SGX和Occlum里都被用到)，就没有很好的办法了。

在调试某些不方便调试的一些底层软件时(如Xen、KVM)的常用套路。其实，Xen/KVM/SGX也都是自带一些trace机制，其本质也是通过打印来输出变量和状态。这是在系统软件工程领域一个非常有意思的技巧，有时间我会专门讲它们在这方面的设计和实现。

既然不能使用系统调用，那就意味着没法使用printf这样简单方便的函数。所以首先得在enclave内部实现一个自己的printf trampoline——为了在需要的地方打印。这个printf trampoline要在执行时调用Ocall，Ocall再去调用系统自带的printf函数。这样的例子在网上有很多，一般的SGX入门教程也会讲，在此不赘述。

接着，为了使得这个printf trampoline可以被enclave之外的第三方库调用，就得把这个printf trampoline暴露出来，形成一个Ecall。这个也简单，在edl文件中定义一下就行。这一步和上一步合起来就建立好了我们的输出功能。

做好上述这些，就意味着在用户层可以调用输出函数了。但是，往往在我们开发的过程中，很多时候我们需要去调试引入到enclave里的第三方库。这些个第三方库是我们Enclave.cpp需要去静态链接的(enclave里不支持动态链接)，第三方库想要再回头去调用我们在Enclave.cpp里定义的输出函数是不行的。这时就需要利用回调函数来实现调试功能了。使用回调函数的部分在下一篇文章里讲，有兴趣的小伙伴可以移步至下一篇。