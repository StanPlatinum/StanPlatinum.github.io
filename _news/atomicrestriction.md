---
layout: post
title: Paper accepted!
date: 2020-10-23 20:28:00-0400
inline: false
---

Our paper, Atomic Restriction: Hardware Atomization to Defend Against Preemption Attacks, has been accepted at [ISPA](https://hpcn.exeter.ac.uk/ispa2020/).

***

Attacks aiming at parallelism and concurrency system have a common basis, which is preemptive execution among multi-tasks. We call them preemptive execution based attacks, which include some side channel attacks and TOCTTOU exploits. This paper summarizes the victim's preemption windows in some attacks and proposes Atomic Restriction. Based on Intel TSX, Atomic Restriction blocks the victim's preemption window by atomizing critical operations. As a universal solution, Atomic Restriction can protect the victim from preemptive execution based attacks. We verified Atomic Restriction on page cache attacks and shadow stack TOCTTOU exploits by using LLVM to implement Atomic Restriction as an automated instrumentation tool ARES.