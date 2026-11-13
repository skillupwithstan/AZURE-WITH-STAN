## Choosing an Azure Storage Account Type (Kind)

When provisioning Azure Storage, selecting the correct account type is crucial for balancing performance, feature availability, and cost. Azure offers several storage account types, each optimized for specific workloads.

### Available Storage Account Types

| Storage Account Type | Supported Services | Performance Tiers | Best Used For |
| :--- | :--- | :--- | :--- |
| **General-purpose v2 (GPv2)** | Blob, File, Queue, Table, Data Lake Gen2 | Standard, Premium | Default choice for most workloads. Offers the lowest per-gigabyte capacity prices and supports all latest features. |
| **BlockBlobStorage** | Block Blobs, Append Blobs | Premium | High transaction rates, small objects, or scenarios requiring consistently low storage latency. |
| **FileStorage** | File Shares | Premium | Enterprise-scale or high-performance file share applications requiring sub-millisecond latency. |
| **PageBlobStorage** | Page Blobs | Premium | Premium storage for Azure Virtual Machine unmanaged disks. |
| **General-purpose v1 (GPv1)** | Blob, File, Queue, Table | Standard | Legacy workloads. Microsoft recommends upgrading to GPv2 to access modern tiering and features. |

### Decision Guide

* **Start with General-purpose v2 (Standard)** for the vast majority of deployments. It grants access to all modern features, including lifecycle management and access tiers (Hot, Cool, Cold, and Archive).
* **Choose Premium tiers** (BlockBlobStorage, FileStorage, or PageBlobStorage) only if your application is I/O intensive. Premium tiers cost more for base storage capacity but often have lower transaction costs for high-throughput environments.
* **Avoid General-purpose v1** for new deployments, as it does not support modern features like blob access tiers or Data Lake Storage Gen2 capabilities.

---

### Video Reference

[![Azure Storage Accounts Explained](https://img.youtube.com/vi/uFW3h72rG18/maxresdefault.jpg)](https://youtu.be/uFW3h72rG18)