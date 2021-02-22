---
layout: post
title: Graphene-SGX attestation 2
date: 2021-01-28 11:12:00-0400
description: Graphene-SGX attestation - adding an Ecall
---

想要在Graphene里做attestation，最优雅的方式是加一个Ecall。但是它目前只有一个正儿八经的Ecall，就是ecall_enclave_start。
其他的Ecall要么没怎么实现，要么就是要在这个最主要的ecall_enclave_start调用完之后，才能再调用，具体是通过加锁来限制的，代码在```enclave_ecalls.c```如下：
```c
if (__atomic_compare_exchange_n(&g_enclave_start_called.counter, &t, 1, /*weak=*/false,
                                    __ATOMIC_SEQ_CST, __ATOMIC_RELAXED)) {
        // ENCLAVE_START not yet called, so only valid ecall is ENCLAVE_START.
        if (ecall_index != ECALL_ENCLAVE_START) {
            // To keep things simple, we treat an invalid ecall_index like an
            // unsuccessful call to ENCLAVE_START.
            return;
        }
```
这就比较恶心了，因为想要加一个Ecall，不仅没有像加Ocall那么方便，同时还要照顾到这个ecall_enclave_start的感受，不得不说这个地方Graphene还没有做得很好。

现在想要加Ecall，就得好好研究```void handle_ecall(long ecall_index, void* ecall_args, void* exit_target, void* enclave_base_addr)```这个ecall的handler了。我和[ya0guang](https://ya0guang.com/)在这个handler里加入了一些debug info，用的Graphene最新的log API：```log_error()```。

我们尝试了在各种地方加入debug info，来试图找到一个合适的添加我们自己ecall的地方。下面是我们的测试代码片段：
```c
void handle_ecall(long ecall_index, void* ecall_args, void* exit_target, void* enclave_base_addr) {
    
    log_error("Handling 1\n");

    if (ecall_index < 0 || ecall_index >= ECALL_NR)
        return;

    if (!g_enclave_top) {
        g_enclave_base = enclave_base_addr;
        g_enclave_top  = enclave_base_addr + GET_ENCLAVE_TLS(enclave_size);
    }

    /* disallow malicious URSP (that points into the enclave) */
    void* ursp = (void*)GET_ENCLAVE_TLS(gpr)->ursp;
    if (g_enclave_base <= ursp && ursp <= g_enclave_top)
        return;

    log_error("Handling 2\n");

    SET_ENCLAVE_TLS(exit_target,     exit_target);
    SET_ENCLAVE_TLS(ustack,          ursp);
    SET_ENCLAVE_TLS(ustack_top,      ursp);
    SET_ENCLAVE_TLS(clear_child_tid, NULL);
    SET_ENCLAVE_TLS(untrusted_area_cache.in_use, 0UL);

    log_error("Handling 3\n");

    int64_t t = 0;
    if (__atomic_compare_exchange_n(&g_enclave_start_called.counter, &t, 1, /*weak=*/false,
                                    __ATOMIC_SEQ_CST, __ATOMIC_RELAXED)) {
        // ENCLAVE_START not yet called, so only valid ecall is ENCLAVE_START.
```
实验结果有点符合预期，有的非常奇怪。handling 1是可以打印出来的，但是到这里也就卡住了。这个我猜是enclave还没有准备好，于是还没有办法处理打印的Ocall。如果我们注释掉handling 1，这个时候输出了handling 3，handling 2没有看到，程序可以正常运行。这里很迷。如果注释掉handling 2，handling 3也可以被打印出来，程序正常运行。关于为什么注释掉handling 1会出现这么反直觉（玄学）的现象，我认为是在handling 2的时候，实现prinf的Ocall并没有被准备好，但是可以被调用，也可以正常返回；到handling 3的时候，Ocall都被准备好了，于是handling 3能够被正常打印。