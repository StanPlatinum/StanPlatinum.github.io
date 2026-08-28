---
layout: about
title: About
permalink: /
subtitle: <a href='#'>Affiliations</a>. Address. Contacts. Moto. Etc.

profile:
  align: right
  # image: koichi.jpeg
  image: jojo8wp.jpg
  # address: >
  #   Luddy Hall, Bloomington, IN

news: true  # includes a list of news items
social: true  # includes social icons at the bottom of the page
---

### Weijie Liu's Here.

I am currently an Associate Professor at Nankai University. Prior to this, I served as a technical expert at Ant Group (2022-2024) and Tencent Group (2018).

I embarked on my academic journey at Wuhan University, where I earned my Bachelor's degree in 2012 and later my Doctoral degree in 2018, advised by Prof. Lina Wang.
From 2015 to 2016, I had the privilege of being a visiting student at Singapore Management University. During this enriching period, I was fortunate to have [Prof. Debin Gao](https://dbgao.github.io/) and [Prof. Mike Reiter](https://reitermk.github.io/) as my instructors.
Subsequently, I completed a two-year postdoctoral fellowship at Indiana University Bloomington, followed by one year as a Research Assistant Professor (RAP), working with [Prof. XiaoFeng Wang](https://wangxiaofeng7.github.io/) and [Prof. Haixu Tang](https://homes.luddy.indiana.edu/hatang/).

My research spans systems and software security across the cloud stack, from cloud-native runtimes and TEE-enabled systems software to processor microarchitecture.

### Research Interests

#### Cloud-Native Application and Runtime Security

I study isolation and runtime security in containerized and multi-tenant cloud platforms. My work includes sandboxing untrusted CI workloads with gVisor [S&P 22], identifying and mitigating container-escape vectors caused by path misresolution and cross-boundary desynchronization [CCS 23, NDSS 26], characterizing trust-boundary vulnerabilities in TEE container systems [FSE 26], and building a secure, resource-efficient, and pluggable Kubernetes architecture for multi-tenancy [EuroSys 26].

#### Trusted Systems Software and Hardware-Software Co-Design

I build systems software that leverages TEEs to protect sensitive applications and data, including library operating systems, secure storage, enclave services, and confidential-computing platforms. I contributed to Occlum, an open-source library OS for Intel SGX that has been deployed in Alipay. My work also uncovered eviction attacks against SGX-PFS and introduced sync-atomic secure storage [FAST 25], alongside research on in-enclave policy verification, user isolation, privacy-preserving analytics, preemption defense, and enclave-assisted secure computation [DSN 21, TDSC 23, CLOUD 21, ISPA 20, CHES 26].

#### Processor and Microarchitectural Security

I investigate microarchitectural attacks and hardware-assisted mechanisms for side-channel defense, detection, and forensics. My work includes VMFUNC-based time blurring for mitigating timing-dependent side channels [ESORICS 17], hardware-virtualization-based detection of controlled-channel attacks [ICA3PP 18], LBR-assisted virtual machine introspection [TIFS 22], and practical Rowhammer attack techniques [Tsinghua ST 19].
