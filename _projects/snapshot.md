---
layout: page
title: Snapshot-attack
description: Unanticipated Snapshot Attack against TEEs (CVE-2022-27499)
category: Vuln_discovery

---

## A disk-level attack against Trusted Execution Environments

[Project Portal](https://github.com/StanPlatinum/snapshot-attack-demo)

[Website](https://sites.google.com/view/transient-snapshot-attack/home)

***

### Introduction

We present an attack against Intel SGX which goes by transient snapshot attacks (CVE-2022-27499). This new type of attack has not been reported by anyone before us. This attack affects the Intel SGX Protected File System Library (SGX-PFS), which provides the essential ability to secure the file I/O for SGX applications, which is a popular feature among SGX users. Given the history and popularity of this feature (SGX-PFS), the vulnerability that we found may impact a great portion of SGX SDK's users.
