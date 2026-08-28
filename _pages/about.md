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

### Weijie Liu

I am an Associate Professor at Nankai University. Previously, I served as a technical expert at Ant Group (2022–2024) and Tencent (2018).

I received my Bachelor's and Ph.D. degrees from Wuhan University in 2012 and 2018, respectively, under the supervision of Prof. Lina Wang. During my doctoral studies, I visited Singapore Management University and worked with [Prof. Debin Gao](https://dbgao.github.io/) and [Prof. Mike Reiter](https://reitermk.github.io/). I later completed a two-year postdoctoral fellowship and spent one year as a Research Assistant Professor at Indiana University Bloomington, working with [Prof. XiaoFeng Wang](https://wangxiaofeng7.github.io/) and [Prof. Haixu Tang](https://homes.luddy.indiana.edu/hatang/).

## Research

My research focuses on building secure and trustworthy computing systems across the software–hardware stack. It connects application isolation, TEE-enabled systems software, and processor-level security through the following three areas.

<div class="research-areas">
  <section class="research-area">
    <h3><span class="research-area-number">01</span> Secure Sandboxing and Runtime Isolation</h3>
    <p>I design security sandboxes and runtime isolation mechanisms for untrusted workloads. My work includes sandboxing CI workloads with gVisor [S&amp;P 22], identifying and mitigating container-escape vectors caused by path misresolution and cross-boundary desynchronization [CCS 23, NDSS 26], and characterizing trust-boundary vulnerabilities in TEE container systems [FSE 26].</p>
  </section>

  <section class="research-area">
    <h3><span class="research-area-number">02</span> Trusted Systems Software and Hardware-Software Co-Design</h3>
    <p>I build systems software that leverages trusted hardware to protect sensitive applications and data, spanning library operating systems, secure storage, enclave services, and confidential-computing platforms. I contributed to Occlum, an open-source library OS for Intel SGX that has been deployed in Alipay, and developed a secure, resource-efficient, and pluggable Kubernetes architecture for multi-tenancy [EuroSys 26]. My work also uncovered eviction attacks against SGX-PFS and introduced sync-atomic secure storage [FAST 25], alongside research on in-enclave policy verification, user isolation, privacy-preserving analytics, preemption defense, enclave-assisted secure computation [DSN 21, TDSC 23, CLOUD 21, ISPA 20, CHES 26], and hardware-event-based covert-channel detection in cloud environments <span class="paper-ref">[SCN 16]</span>.</p>
  </section>

  <section class="research-area">
    <h3><span class="research-area-number">03</span> Processor and Microarchitectural Security</h3>
    <p>I investigate microarchitectural attacks and hardware-assisted defenses. My work covers VMFUNC-based time blurring <span class="paper-ref">[ESORICS 17]</span>, virtualization-based controlled-channel detection <span class="paper-ref">[ICA3PP 18]</span>, LBR-assisted virtual machine introspection <span class="paper-ref">[TIFS 22]</span>, and practical Rowhammer attacks <span class="paper-ref">[Tsinghua ST 19]</span>.</p>
  </section>
</div>
