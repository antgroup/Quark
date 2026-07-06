# Quark

> **Stop Pretending to be Busy: A Case for Serverless Paradigms in Co-located Batch Workloads**

Quark is a production-proven framework for improving the efficiency of **co-located batch workloads** in overcommitted cloud environments.  
This repository serves as the **companion page** for our paper and provides public materials related to the work.

> **Note**
>  
> The production implementation of Quark is **not open-sourced at this time**.  
> This repository is intended to host the paper, documentation, and other publicly shareable artifacts.

---

## Paper

**Title:** Stop Pretending to be Busy: A Case for Serverless Paradigms in Co-located Batch Workloads  
**Authors:** Xiaohu Chai, Jianfeng Tan, Congsi Yuan, Bowen Yang, Hao Dai, Tongkai Yang, Chao Huang, Dong Du, Yu Chen

- Paper: [PDF](./paper/quark.pdf)
- Venue: OSDI / Operational Systems

---

## Overview

Modern cloud providers commonly improve cluster utilization through **overcommitment** and **workload co-location**.  
A typical practice is to run low-priority batch analytics together with high-priority online services, while preserving online SLOs.

Our production study shows that although co-location increases raw utilization, existing batch systems still waste substantial resources due to a mismatch between:

- **coarse-grained, long-lived execution models**, and
- **dynamic, interference-prone co-located environments**

We identify four major forms of resource idleness in production batch workloads:

- **Slot Idle** — coarse-grained executors cannot adapt to stage-level resource variation
- **Gap Idle** — hardware heterogeneity and online interference create stragglers
- **Start Idle** — launching analytics instances is slow
- **Stop Idle** — idle executors retain resources before being destroyed

## Why Quark?

Traditional Spark uses **long-lived executors** with fixed resource sizes.  
This model works reasonably well on dedicated clusters, but becomes inefficient in modern co-located environments where resources are:

- dynamic
- overcommitted
- heterogeneous
- interference-prone

Quark replaces this mismatch with a **fine-grained, elastic, task-centric execution model**, making batch workloads better suited for real cloud conditions.

---

---

## Key Contributions

Quark makes three main contributions:

### 1. Characterizing inefficiency in co-located batch workloads
We provide a production-scale study showing that a significant portion of allocated batch resources is spent in non-effective states rather than useful computation.

### 2. A serverless-inspired design for batch analytics
Quark replaces the coarse-grained executor-centric model with a fine-grained task-centric model, and introduces:

- scalable resource control
- interference-aware scheduling
- fast task provisioning

### 3. Large-scale production validation
Quark has been deployed at production scale in Ant Group and demonstrates substantial reductions in resource waste and tail latency.

---

## Main Findings

### Production observations

In our production environment:

- online services use only **22.0%** of available CPU on average
- co-located batch workloads harvest an additional **26.8%** CPU capacity
- but only **67%** of batch-allocated resources are used for effective computation

Breakdown of batch workload utilization:

- **Compute:** 67%
- **Stop Idle:** 17%
- **Slot Idle:** 13%
- **Gap Idle:** 2%
- **Start Idle:** 1%

### Results

Quark achieves:

- **56.01% average reduction** in resource consumption on TPC-H
- **37.37% reduction** in resource consumption in production environments
- **89.7% reduction** in task startup overhead
- **18%–33% lower** average task execution time in microbenchmarks
- reduction of long-tail job proportion from **15% to 2%**
- reduction of tail latency ratio from about **20× to 8×**

---

## System Design

Quark is built around three key techniques:

### Scalable Resource Control
To support task-level scheduling at scale, Quark introduces:

- a **Slots Ring** to regulate task parallelism
- a **Quota Manager** to explicitly control global overcommitment capacity
- an **asynchronous control path** for efficient refill / grant / invoke handling

### Interference-aware Scheduler
To mitigate stragglers caused by co-location noise and heterogeneous hardware, Quark:

- models effective per-node capacity
- normalizes resource views across nodes
- uses a variance-aware placement strategy to better align task completion times

### Fast Task Provision
To make fine-grained execution practical, Quark reduces cold start overhead through:

- **state reuse**
- **state pre-prepare**
- **state lazy-load**

---

## Deployment at Scale

According to the paper, Quark has been deployed in production to process:

- **350,000 offline query jobs daily**
- **7,500 TB to 10,000 TB data per day**
- across **600,000 CPU cores**
- while saving more than **100,000 CPU cores**

The paper further reports long-term production operation over:

- **6,000+ servers**
- **902K jobs/day on average**
- **99.11% success rate**
- **105.4 PB average daily I/O**

---

## Repository Scope

This repository is intended for publicly shareable materials related to the paper, such as:

- paper PDF
- figures
- supplementary notes
- trace description
- citation information
- updates and errata

> The production source code and internal deployment components are currently **not publicly available**.

---

## Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{chai2026quark,
  title={Stop Pretending to be Busy: A Case for Serverless Paradigms in Co-located Batch Workloads},
  author={Xiaohu Chai and Jianfeng Tan and Congsi Yuan and Bowen Yang and Hao Dai and Tongkai Yang and Chao Huang and Dong Du and Yu Chen},
  booktitle={Proceedings of the USENIX Symposium on Operating Systems Design and Implementation (OSDI)},
  year={2026}
}
