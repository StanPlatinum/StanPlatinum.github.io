---
layout: post
title: Graphene-SGX attestation 1
date: 2021-01-21 11:12:00-0400
description: Graphene-SGX attestation key functions
---

因为要完成主人的任务，要研究和二次开发Graphene-SGX里local attestation的部分；然而这个任务又比较繁琐，于是这次就记录一下过程，供后人（我自己）查阅。

在做attestation之前，需要通过DH来建立安全信道。Graphene里用到的是：
```c
int _DkStreamKeyExchange(PAL_HANDLE stream, PAL_SESSION_KEY* key)
```

Intel SGX 在做attestation的时候，都需要获取在CPU中的report。这个时候就需要用EREPORT这个指令。Graphene-SGX调用EREPORT的核心函数有仨，分别是：
```c
sgx_get_report(),
__sgx_get_report(),
sgx_report(),
```
他们的参数都是三个，而且都是targetinfo, reportdata, 和report。
不知道他们的区别，但是莽就完事了。主要用这么一句：
```c
int ret = sgx_report(&targetinfo, &reportdata, &report);
if (ret) {
    SGX_DBG(DBG_E, "failed to get self report: %d\n", ret);
    return -PAL_ERROR_INVAL;
}
```
然后就是请求和回复report的函数，有这么俩：
```c
int _DkStreamReportRequest(PAL_HANDLE stream, sgx_sign_data_t* data,
                           mr_enclave_check_t is_mr_enclave_ok)

int _DkStreamReportRespond(PAL_HANDLE stream, sgx_sign_data_t* data,
                           mr_enclave_check_t is_mr_enclave_ok)
```
这俩函数调用上面说的__sgx_get_report，还调用了一些用于同一个host上（因为是local attestation）两个不用enclave之间的通信函数，比如：
_DkStreamRead 和 _DkStreamWrite。

这之后做完attestation，就可以建立secure session了。建立session和安全通信的函数也有一堆，也都在```pal_linux.h```这个头里，这里就不说了。


