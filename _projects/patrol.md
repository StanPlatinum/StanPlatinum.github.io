---
layout: page
title: Patrol
description: Path lookup access control for Linux containers
category: Projects

---

## Kernel-based filesystem isolation extension for containers

Kernel-based filesystem isolation is ideal way to ensure isolation always in place during host-containerinteractions. In our research, we design and implement the first suchan approach that extends the filesystem isolation to dentry objects, by enforcing access control on host-container interactions throughthe filesystem. Our design addresses the fundamental limitation of one-way isolation characterizing today’s container, uses carefully-designed policies to ensure accurate and comprehensive interactioncontrol, and implants the protection into the right kernel locationto minimize the performance impact. We verify our approach usingmodel checking, which demonstrates its effectiveness in eliminatingthe path lookup mis-resolution risk. Our evaluation further shows that our approachincurs negligible overheads.

Please refer to our ccs 2023 paper "Lost along the Way: Understanding and MitigatingPath-Misresolution Threats to Container Isolation" for more details.

[Project Portal](https://github.com/CGCL-codes/Patrol)

[Paper Link](https://www.researchgate.net/publication/375808473_Lost_along_the_Way_Understanding_and_Mitigating_Path-Misresolution_Threats_to_Container_Isolation#fullTextFileContent)