# Azure Storage Deep Dive - Day 1

## Video Reference
[![Azure Storage - Day 1](https://img.youtube.com/vi/tqnZHyVO2DY/maxresdefault.jpg)](https://youtu.be/tqnZHyVO2DY)
*(Click the thumbnail above to watch the tutorial)*

---

## 1. Introduction to Azure Storage
Azure Storage is Microsoft's cloud storage solution that provides highly available, massively scalable, durable, and secure storage for various data objects in the cloud. Data in Azure Storage is accessible from anywhere in the world over HTTP or HTTPS.

## 2. Core Storage Services
Azure offers several distinct storage services depending on your data format and access requirements:

* **Azure Blob Storage:** A massively scalable object store for unstructured data (text or binary data like images, documents, and backups).
* **Azure Files:** Fully managed cloud file shares that can be accessed via industry-standard Server Message Block (SMB) or Network File System (NFS) protocols.
* **Azure Queue Storage:** A messaging store for reliable delivery and asynchronous processing of messages between application components.
* **Azure Table Storage:** A NoSQL datastore for schemaless storage of structured data (key/attribute store).
* **Azure Disks:** Block-level storage volumes managed by Azure and used with Azure Virtual Machines.

## 3. Storage Account Types
A storage account provides a unique namespace in Azure for your data. The most common types include:

* **Standard General-Purpose v2 (GPv2):** The default and recommended standard account type. Supports Blobs, Files, Queues, and Tables. Uses magnetic hard drives (HDDs).
* **Premium Block Blobs:** Optimized for high transaction rates and single-digit millisecond storage latency. Uses solid-state drives (SSDs).
* **Premium File Shares:** Designed for high-performance enterprise file share workloads.
* **Premium Page Blobs:** Used primarily to store premium Azure Virtual Machine disks.

## 4. Storage Redundancy (Replication)
To protect against hardware failures, network outages, or regional disasters, Azure always replicates your data. 

* **LRS (Locally Redundant Storage):** Copies data 3 times within a single physical data center. Protects against server rack and drive failures.
* **ZRS (Zone-Redundant Storage):** Copies data synchronously across 3 separate Availability Zones within the primary region. Protects against entire data center failures.
* **GRS (Geo-Redundant Storage):** Copies data 3 times locally (LRS), then asynchronously replicates it to a secondary paired region. Protects against regional disasters.
* **GZRS (Geo-Zone-Redundant Storage):** Copies data across 3 zones (ZRS), then asynchronously replicates it to a secondary paired region. Provides the highest level of durability.