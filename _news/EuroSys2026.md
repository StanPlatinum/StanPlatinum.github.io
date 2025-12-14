---
layout: post
title: EuroSys!
date: 2025-12-13 20:28:00-0400
inline: false
---

Our paper "Pyramid: A Secure, Resource-Efficient, and Pluggable Kubernetes for Multi-Tenancy" is accepted at EuroSys 2026.

***

This paper proposes Pyramid, a set of extensions to k8s to support verifiable, hardware-based isolation between tenants while enabling efficient resource allocation. The extensions utilize TEEs for hosting tenant specific instances control and data planes. However, resource requirements are routed to an untrusted global control plane through a shadow pod abstraction. This allows the untrusted control plane to allocate resources on the cluster based on a global view of resource consumption. Experiments suggest that Pyramid can offer strong isolation guarantees with low control plane overheads. Furthermore, Pyramid can improve performance on the data plane by enabling better global resource allocation.