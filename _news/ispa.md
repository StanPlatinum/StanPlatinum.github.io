---
layout: post
title: Paper accepted!
date: 2020-10-23 20:28:00-0400
inline: false
---

Our paper, Atomic Restriction: Hardware Atomization to Defend Against Preemption Attacks, has been accepted at ISPA.

***

Attacks aiming at parallelism and concurrency system have a common basis, which is preemptive execution among multi-tasks. We call them preemptive execution based attacks, which include some side channel attacks and TOCTTOU exploits. For this type of attack, attackers and victims are parallel or concurrent processes or threads in general. An attacker needs to complete a series of malicious actions in the preemption window of a victim's execution. This paper summarizes the victim's preemption windows in some attacks and proposes Atomic Restriction. Based on Intel TSX, Atomic Restriction blocks the victim's preemption window by atomizing critical operations. As a universal solution, Atomic Restriction can protect the victim from preemptive execution based attacks. We verified Atomic Restriction on page cache attacks and shadow stack TOCTTOU exploits by using LLVM to implement Atomic Restriction as an automated instrumentation tool ARES. The analysis and evaluation results show that Atomic Restriction can effectively defend against preemptive execution based attacks with low performance overhead in most scenarios.