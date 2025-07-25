---
title: "Launching AWS EC2 Instances"
date: 2025-08-06
categories: [AWS, Cloud]
tags: [aws, ec2, cloud, instance, launch]
image: assets/images/images.png
---

- AMI - Amazon Machine Instances - Marketplace of all the operating systems

## Launching an EC2 instance

1. Go to Launch an EC2 instance
2. Initialize the Name and the tags
3. Select the Amazon Machine Image
4. Select your required instance type

### AWS Instance Families

| **Family**     | **Use Case**                        | **Examples**       |
| -------------- | ----------------------------------- | ------------------ |
| **t-series**   | Burstable, low-cost general purpose | `t3`, `t4g`        |
| **m-series**   | General purpose                     | `m6g`, `m5`, `m7i` |
| **c-series**   | Compute-optimized                   | `c7g`, `c6i`, `c5` |
| **r-series**   | Memory-optimized                    | `r6g`, `r5`, `r7i` |
| **x-series**   | High memory for in-memory databases | `x2idn`, `x2gd`    |
| **i-series**   | IOPS optimized, storage-intensive   | `i4i`, `i3`        |
| **g/p-series** | GPU/accelerated computing           | `g5`, `p4`, `inf1` |
| **d/h-series** | Dense storage                       | `d3en`, `h1`       |
| **z-series**   | High frequency compute              | `z1d`              |
| **a-series**   | Arm-based (Graviton processors)     | `a1`, `t4g`, `m6g` |

### GCP Instance Families

| **Family**     | **Use Case**                        | **Examples**       |
| -------------- | ----------------------------------- | ------------------ |
| **t-series**   | Burstable, low-cost general purpose | `t3`, `t4g`        |
| **m-series**   | General purpose                     | `m6g`, `m5`, `m7i` |
| **c-series**   | Compute-optimized                   | `c7g`, `c6i`, `c5` |
| **r-series**   | Memory-optimized                    | `r6g`, `r5`, `r7i` |
| **x-series**   | High memory for in-memory databases | `x2idn`, `x2gd`    |
| **i-series**   | IOPS optimized, storage-intensive   | `i4i`, `i3`        |
| **g/p-series** | GPU/accelerated computing           | `g5`, `p4`, `inf1` |
| **d/h-series** | Dense storage                       | `d3en`, `h1`       |
| **z-series**   | High frequency compute              | `z1d`              |
| **a-series**   | Arm-based (Graviton processors)     | `a1`, `t4g`, `m6g` |

### Azure Instance Series

| **Series**     | **Use Case**                               | **Examples**           |
| -------------- | ------------------------------------------ | ---------------------- |
| **B-series**   | Burstable VMs                              | `B2s`, `B4ms`          |
| **D-series**   | General purpose                            | `D4s_v3`, `D2as_v5`    |
| **E-series**   | Memory-optimized                           | `E4s_v3`, `E8as_v5`    |
| **F-series**   | Compute-optimized                          | `F4s`, `F72s_v2`       |
| **M-series**   | High memory (SAP, databases)               | `M128ms`, `Mv2`        |
| **L-series**   | Storage optimized                          | `L8s`, `L64s_v3`       |
| **N-series**   | GPU/AI workloads                           | `NC6`, `ND96asr_v4`    |
| **H-series**   | High Performance Computing                 | `HB120rs_v3`, `HC44rs` |
| **A-series**   | Entry-level                                | `A2`, `A4_v2`          |
| **Av2-series** | Budget workloads                           | `A2_v2`, `A4_v2`       |
| **Dv5/Dasv5**  | Latest general-purpose with Intel/AMD CPUs | `D4as_v5`, `D8_v5`     |

5. Select or create your key pair (.ppk or .pem)
6. Select your Network
    - Select your VPC
    - Select the required subnets (e.g., 3 subnets in Mumbai ---- 1a,1b,1c)
    - Select the Availability Zones
7. Security Groups
    - Firewall protection for EC2 instances
    - NACL - network access control list
    - WAF - web application firewall
8. Select the required storage
9. Select the required number of Instances
10. **Finally launch the instance**
