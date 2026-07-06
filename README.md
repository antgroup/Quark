# Quark

Quark is a **serverless-inspired batch analytics framework** for **co-located and overcommitted cloud clusters**.  
It rethinks the traditional Spark executor model and introduces **task-level, on-demand resource provisioning** to eliminate hidden inefficiencies in production batch workloads.

In large-scale production deployment at Ant Group, Quark has been used to process:

- **350,000+ offline query jobs daily**
- **7,500 TB ~ 10,000 TB data per day**
- **600,000 CPU cores deployment footprint**
- **100,000+ CPU cores saved**

---

## Overview

Cloud providers commonly improve utilization by **co-locating low-priority batch jobs** with **high-priority online services**.  
However, our production study shows that although overcommitment increases raw utilization, batch workloads still waste a large fraction of allocated resources.

This paper identifies four major forms of idleness in co-located Spark workloads:

- **Slot Idle**: coarse-grained executor allocation wastes resources across stages
- **Gap Idle**: stragglers caused by interference and hardware heterogeneity
- **Start Idle**: slow startup of analytics instances
- **Stop Idle**: delayed teardown and idle holding of resources

Quark addresses these inefficiencies by adopting a **serverless paradigm for batch workloads**, where resources are provisioned and released at the **task granularity** instead of the executor granularity.

---

## Why Quark?

Traditional Spark uses **long-lived executors** with fixed resource sizes.  
This model works reasonably well on dedicated clusters, but becomes inefficient in modern co-located environments where resources are:

- dynamic
- overcommitted
- heterogeneous
- interference-prone

Quark replaces this mismatch with a **fine-grained, elastic, task-centric execution model**, making batch workloads better suited for real cloud conditions.

---

## Key Ideas

Quark is built around three core techniques:

### 1. Scalable Resource Control
To support massive task-level scheduling without overwhelming the control plane, Quark introduces:

- **Slots Ring** for bounded task parallelism
- **Quota Manager** for explicit overcommitment-aware resource grants
- **Asynchronous scheduling pipeline** to decouple refill / grant / invoke operations

### 2. Interference-aware Scheduler
To mitigate stage-level stragglers caused by noisy co-location and heterogeneous machines, Quark:

- normalizes node capacity using runtime interference signals
- models effective batch capacity across nodes
- applies a **variance-optimal scheduler** to better align task completion times

### 3. Fast Task Provision
To make per-task provisioning practical, Quark reduces cold-start overhead using:

- **State Reuse via fork/vmfork**
- **State Pre-Prepare** for task-specific states such as codegen artifacts
- **State Lazy-Load** for non-critical runtime components

---



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
