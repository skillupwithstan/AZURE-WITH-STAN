# Choosing Azure Disk Storage Types

### Companion Video Tutorials

[](https://youtu.be/ULQmTZj1EuI)
*(Click the image above to watch Part 1)*

[](https://youtu.be/Ad3rpMaUDwM)
*(Click the image above to watch Part 2)*

---

## 1. Introduction to Azure Managed Disks

Azure Managed Disks are block-level storage volumes that are managed by Azure and used with Azure Virtual Machines. Managed disks are essentially virtualized versions of the physical disks in an on-premises server. Selecting the right disk type is critical for optimizing both workload performance and overall cloud expenditure.

## 2. Disk Storage Types Available in Azure

Azure offers several disk types, each designed to meet specific performance and budget requirements.

### A. Ultra Disk Storage

* **Best For:** Top-tier databases (e.g., SAP HANA, top-tier SQL, Oracle) and other transaction-heavy workloads.
* **Performance:** Sub-millisecond latency. High IOPS and throughput that can be scaled dynamically without restarting the virtual machine.
* **Key Feature:** You can configure capacity, IOPS, and throughput independently.

### B. Premium SSD v2

* **Best For:** Production and performance-sensitive workloads that require consistent low latency and high scalability but don't need the absolute maximums of Ultra Disks.
* **Performance:** Sub-millisecond latency. Provides a baseline performance that can be independently scaled (IOPS and throughput separate from size) making it highly cost-effective for modern architectures.

### C. Premium SSD

* **Best For:** Production and performance-sensitive workloads, mission-critical applications, and enterprise databases.
* **Performance:** Single-digit millisecond latency. Performance (IOPS and throughput) scales alongside the provisioned disk size.

### D. Standard SSD

* **Best For:** Web servers, lightly used enterprise applications, and dev/test environments.
* **Performance:** Combines the cost-efficiency of HDD with the reliability and smoother performance of SSDs.

### E. Standard HDD

* **Best For:** Backup, non-critical, and infrequent access storage.
* **Performance:** Higher latency and lower IOPS compared to SSDs. Backed by magnetic drives.

## 3. Storage Selection Matrix

Use the following table to quickly compare and determine the best fit for your infrastructure architecture:

| Disk Type | Workload Scenario | Latency | Storage Media | Dynamic Scaling |
| --- | --- | --- | --- | --- |
| **Ultra Disk** | Data-intensive, SAP HANA, top tier databases | Sub-millisecond | SSD | Yes (Size, IOPS, Throughput independent) |
| **Premium SSD v2** | Enterprise production apps, SQL, Oracle | Sub-millisecond | SSD | Yes (Size, IOPS, Throughput independent) |
| **Premium SSD** | Production workloads, general databases | Single-digit ms | SSD | No (Performance tied to size) |
| **Standard SSD** | Dev/Test, low-traffic web servers | Single-digit ms | SSD | No |
| **Standard HDD** | Backups, non-critical data | High | Magnetic | No |

## 4. Key Decision Factors

When architecting your Azure environment and choosing a disk, evaluate these four pillars:

1. **Capacity:** How much data do you need to store?
2. **IOPS (Input/Output Operations Per Second):** How many read/write operations will your application perform per second? Databases typically require high IOPS.
3. **Throughput:** How much data (in MB/s) needs to be transferred simultaneously? Data warehousing and streaming workloads rely heavily on throughput.
4. **Cost:** Balance your performance needs against your budget. For instance, using Standard SSDs for non-production environments can heavily reduce costs while maintaining acceptable reliability.