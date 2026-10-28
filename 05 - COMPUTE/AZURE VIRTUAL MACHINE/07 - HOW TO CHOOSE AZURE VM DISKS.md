# 07 - HOW TO CHOOSE AZURE VM DISKS

[![AZURE ARCHITECT SERIES IN TAMIL - HOW TO CHOOSE AZURE VM DISKS](https://img.youtube.com/vi/q9gRjrGLLFc/0.jpg)](https://youtu.be/q9gRjrGLLFc)
*Watch the full architectural breakdown on the [Skill Up with Stan YouTube channel](https://youtu.be/q9gRjrGLLFc).*

Choosing the right disk for your Azure Virtual Machine is critical for balancing cost, performance, and reliability. Azure provides block-level storage through **Managed Disks** (Note: Unmanaged disks are deprecating by March 2026, so all new architectures must use Managed Disks).

Here is a comprehensive guide to selecting the right disk tier, redundancy, and architecture for your workloads.

---

## 1. Disk Selection Decision Tree

When architecting a solution, your first decision is whether the workload requires persistent state or can run on local, temporary host storage.

```mermaid
graph TD
    A[Choose Azure VM Disk] --> B{Need persistent data?}
    B -- No --> C[Ephemeral OS Disk <br> Free, Local NVMe, Ultra-Fast]
    B -- Yes --> D{Workload Type?}
    D -- Mission Critical / SAP HANA --> E[Ultra Disk <br> Up to 400k IOPS]
    D -- High Performance DBs --> F[Premium SSD v2 <br> Granular IOPS Tuning]
    D -- General Production OS/Data --> G[Premium SSD <br> Predictable Latency]
    D -- Dev/Test & Web Servers --> H[Standard SSD <br> Cost-effective]
    D -- Backups / Archives --> I[Standard HDD <br> Cheapest, Lowest IOPS]

2. Persistent vs. Ephemeral DisksA. Persistent Disks (Managed Disks)Data on persistent disks is stored externally on Azure Storage clusters, independently of the physical VM host.Pros: Highly reliable; data survives VM reboots, stop/starts, and hardware failures.Cons: Network traversal adds minor latency compared to local hardware.B. Ephemeral OS DisksEphemeral disks are created on the local physical storage (often NVMe) of the host server running the VM.Pros:Free: No external storage costs are incurred.Ultra-Low Latency: Data is local to the hardware.Fast Re-imaging: Extremely quick to provision.Cons: Data is completely lost if the VM is stopped (deallocated) or moved to another host.Best For: Stateless applications, Kubernetes (AKS) worker nodes, Virtual Machine Scale Sets (VMSS), and CI/CD build agents (like Jenkins, GitLab runners).3. The 5 Storage TiersAzure provides five primary performance tiers for persistent Managed Disks.Disk TierStorage MediaMax IOPSMax ThroughputOS Disk Support?Best Use CasesUltra DiskNVMe SSDUp to 400,00010,000 MB/sNoSAP HANA, top-tier SQL/Oracle databases, transaction-heavy systems demanding sub-millisecond latency.Premium SSD v2SSDUp to 80,0002,000 MB/sNoHigh-performance DBs. Allows tuning capacity, IOPS, and throughput independently without downtime.Premium SSDSSDUp to 20,000900 MB/sYesStandard production workloads, mission-critical OS disks, enterprise applications.Standard SSDSSDUp to 6,000750 MB/sYesDev/test environments, low-traffic web servers, lightweight enterprise apps.Standard HDDMagneticUp to 2,000500 MB/sYes (retiring)Backups, archives, and infrequently accessed data. Not recommended for OS disks.4. Storage Redundancy OptionsYou must define how your disk data replicates across Azure hardware:Locally Redundant Storage (LRS): Replicates your data synchronously three times within a single physical data center.Zone-Redundant Storage (ZRS): Replicates your data synchronously across three distinct Azure Availability Zones within a region. Protects against complete data center outages.Availability Note: ZRS is supported for Premium SSD and Standard SSD, but not for Standard HDD or Ultra Disks.5. Performance Tuning & BottlenecksVM Limits vs. Disk LimitsYour total storage performance is bottlenecked by whichever is lower: the VM SKU limit or the Disk limit. If your VM maxes out at 12,800 IOPS, attaching an Ultra Disk provisioned for 40,000 IOPS will not increase performance beyond 12,800 IOPS.Disk BurstingAzure allows disks to handle temporary traffic spikes beyond their provisioned IO limits.Credit-Based Bursting: Accumulates credits during idle periods and uses them during spikes (free; enabled by default on smaller Standard/Premium SSDs).On-Demand Bursting: Allows larger Premium SSDs to burst indefinitely for a per-transaction fee.Disk Striping (RAID 0)If a single disk does not provide enough IOPS for your workload, you can stripe multiple disks together. For instance, combining four P30 (5,000 IOPS) disks yields a 20,000 IOPS volume.Monitoring Disk PerformanceYou can monitor if you are hitting IOPS bottlenecks using Azure Monitor metrics (OS Disk Used Burst IO Credits Percentage, Data Disk Latency) or local OS tools.

# PowerShell: Monitor physical disk reads, writes, and queue length (Windows)
Get-Counter -Counter "\PhysicalDisk(_Total)\Disk Reads/sec","\PhysicalDisk(_Total)\Disk Writes/sec","\PhysicalDisk(_Total)\Current Disk Queue Length" -SampleInterval 5 -MaxSamples 12

# Bash: Check real-time IO statistics (Linux)
iostat -x 5

