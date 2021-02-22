---
layout: post
title: Graphene-SGX attestation 3
date: 2021-02-12 11:12:00-0400
description: Graphene-SGX attestation - LAS and RAS
---

上次说到在Graphene里加一个Ecall来做attestation，后来发现根本做不到。具体的原因还不清楚，只是我无论如何也没办法让控制流走到我customized的Ecall里去。

所以索性就不做了，我们直接在唯一的大Ecall里加attestation的代码也是一样。保证attestation在和Local attestation做完认证之后再把控制权交给loaded binary就行。 

这次要来讲讲Remote attestation service。这个东西放在control node上，用来管理用户的key，还有和LAS通信。它的 和LAS做attestation 的部分，感觉是没必要用Intel的EPID那一套，可能用DCAP是可以的，但我觉得可能也不需要，只需要一个握手协议，互相验证对方的report没有变化，再用DH交换一下key，就可以了。