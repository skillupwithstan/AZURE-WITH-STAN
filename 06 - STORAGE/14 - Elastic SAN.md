# Azure Storage - Elastic SAN

[![Azure Elastic SAN Video 1](https://img.youtube.com/vi/YcGXo0zOM7A/0.jpg)](https://youtu.be/YcGXo0zOM7A)
*(Click the image above to watch Video 1)*

[![Azure Elastic SAN Video 2](https://img.youtube.com/vi/1IKt5YJT4EA/0.jpg)](https://youtu.be/1IKt5YJT4EA)
*(Click the image above to watch Video 2)*

## Overview
Azure Elastic Storage Area Network (SAN) is a cloud-native, scalable, and cost-effective SAN service built for enterprise workloads. It brings the familiar management concepts, high performance, and rich feature set of traditional on-premises SAN solutions directly into Azure, making it ideal for large-scale database systems, mission-critical applications, and containerized storage environments.

## Key Features
* **High Performance at Scale:** Delivers massive IOPS and throughput capabilities with low latencies, supporting performance-intensive relational and non-relational databases.
* **Granular Provisioning:** Provision storage capacity and performance independently or dynamically scale volumes to match fluctuating workload demands without over-provisioning.
* **Native Integration:** Seamlessly integrates with Azure compute options, including Azure Virtual Machines and Azure Kubernetes Service (AKS).
* **Enterprise Management:** Simplifies volume management, snapshots, and group-level administrative operations through a unified control plane.

## Common Use Cases
1. **Large-Scale Databases:** Run demanding database engines (such as SQL Server, Oracle, or PostgreSQL) that require consistent high IOPS and low-latency block storage.
2. **Containerized Workloads (AKS):** Provision persistent storage volumes reliably for stateful applications and container orchestration platforms.
3. **Migration of On-Premises SAN Workloads:** Lift-and-shift legacy enterprise applications that depend on traditional SAN architectures directly into Azure without rearchitecting the application layer.

## Architecture & Configuration
* **Elastic SAN Appliance:** The top-level resource representing the SAN footprint in your subscription.
* **Volume Groups:** Logical containers within the SAN used to manage shared settings, network rules, and policies across multiple volumes.
* **Volumes:** Individual block storage endpoints exposed via protocols like iSCSI to connect directly to your Azure compute instances.