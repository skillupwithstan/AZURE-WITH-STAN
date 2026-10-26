```markdown
# Create and Configure Azure Virtual Machines

[![YouTube Video](https://img.youtube.com/vi/BRoWL_w9HSw/hqdefault.jpg)](https://youtu.be/BRoWL_w9HSw)

This document covers the fundamental concepts and administrative tasks for deploying, configuring, and managing Virtual Machines (VMs) in Microsoft Azure.

---

## 1. Create a Virtual Machine

Azure Virtual Machines provide on-demand, scalable computing resources. You can create a VM using multiple tools: Azure Portal, Azure CLI, Azure PowerShell, ARM Templates, or Bicep.

### Key Configuration Tabs in Azure Portal:
*   **Basics:** Define Subscription, Resource Group, VM Name, Region, Availability options, Image (OS), Size, and Administrator account credentials (SSH public key or Password).
*   **Disks:** Select OS disk type (Premium SSD, Standard SSD, Standard HDD) and attach any necessary data disks.
*   **Networking:** Configure the Virtual Network (VNet), Subnet, Public IP (optional), Network Security Group (NSG), and Load Balancing options.
*   **Management:** Enable Azure Security Center, boot diagnostics, OS guest diagnostics, and system-assigned managed identities.
*   **Monitoring:** Configure alert rules and diagnostics.
*   **Advanced:** Add custom data (cloud-init scripts), extensions (e.g., Antimalware, Custom Script Extension), and Proximity Placement Groups.

**Azure CLI Example:**
```bash
az vm create \
  --resource-group StanResourceGroup \
  --name StanVM01 \
  --image Ubuntu2204 \
  --admin-username stan \
  --generate-ssh-keys

```

---

## 2. Configure Encryption at Host for Azure Virtual Machines

Encryption at host ensures that data stored on the VM host is encrypted at rest and flows encrypted to the Storage service. This provides end-to-end encryption for your VM data.

### Key Characteristics:

* Temporary disks and OS/Data disk caches are encrypted at rest with platform-managed keys (PMK) or customer-managed keys (CMK).
* Does not use VM CPU resources, meaning no performance impact on the VM itself.
* Must be enabled on the subscription before it can be used.

### Configuration Prerequisites:

1. Register the `EncryptionAtHost` feature for your subscription.
2. Ensure your VM size supports encryption at host.

### Enabling via Azure CLI:

```bash
az vm create \
  --resource-group StanResourceGroup \
  --name StanVM01 \
  --image Ubuntu2204 \
  --encryption-at-host true

```

---

## 3. Move a Virtual Machine to Another Resource Group, Subscription, or Region

As architectural needs change, you may need to relocate resources.

### Moving across Resource Groups or Subscriptions

* **Tooling:** Use the Azure Portal ("Move" button on the resource page), Azure CLI, or PowerShell.
* **Considerations:**
* Both the source and destination subscriptions must exist within the same Azure Active Directory (Entra ID) tenant.
* You must move all associated resources together (e.g., VM, NIC, OS Disk, Data Disks) for the VM to function properly.
* VMs cannot be moved if Azure Backup is actively running on them. You must stop the backup (retaining data) before moving.



### Moving across Regions

* **Tooling:** Use **Azure Resource Mover**.
* **Process:**
1. **Select resources:** Choose the VM and related dependencies.
2. **Resolve dependencies:** Resource Mover ensures all required components (like VNets) are accounted for.
3. **Prepare:** Azure creates ARM templates for the target region and prepares replicas.
4. **Initiate Move:** Resources are provisioned in the target region.
5. **Commit/Discard:** Commit finalizing the move (you must manually delete source resources afterward) or discard to abort.



---

## 4. Manage Virtual Machine Sizes

The VM size determines the compute power, memory, and temporary storage allocated to the VM.

### Common VM Families:

* **General Purpose (B, D-series):** Balanced CPU-to-memory ratio. Good for testing and web servers.
* **Compute Optimized (F-series):** High CPU-to-memory ratio. Ideal for batch processing and analytics.
* **Memory Optimized (E, M-series):** High memory-to-CPU ratio. Best for relational database servers and in-memory analytics.
* **Storage Optimized (L-series):** High disk throughput and IO. Good for NoSQL databases.
* **GPU (N-series):** Heavy graphics rendering and video editing.

### Resizing a VM:

* You can resize a VM at any time.
* If the new size is available on the current hardware cluster, the VM will simply reboot.
* If the new size requires different hardware, you must **deallocate** (stop) the VM first, resize it, and then start it again.

**Azure CLI Example:**

```bash
az vm resize --resource-group StanResourceGroup --name StanVM01 --size Standard_D4s_v3

```

---

## 5. Manage Virtual Machine Disks

Azure VMs use virtual hard disks (VHDs) for storage.

### Types of Disks:

1. **OS Disk:** Contains the operating system. Usually assigned a drive letter like `C:` (Windows) or mounted at `/` (Linux). Maximum capacity is usually 4 TiB.
2. **Temporary Disk:** Provides short-term, ephemeral storage for page files/swap files. Data is **lost** during a VM deallocation or maintenance event. `D:` (Windows) or `/dev/sdb` (Linux).
3. **Data Disk:** Managed disks attached to the VM to store application data.

### Disk Performance Tiers:

* **Standard HDD:** Backup, non-critical, infrequent access.
* **Standard SSD:** Web servers, lightly used enterprise applications.
* **Premium SSD:** Production workloads, highly sensitive latency needs.
* **Ultra Disk:** Sub-millisecond latency, massive IOPS (e.g., SAP HANA, top-tier databases).

### Common Management Tasks:

* **Attach a Data Disk:** Can be done while the VM is running. Requires logging into the guest OS to initialize, format, and mount the disk.
* **Expand a Disk:** You generally must deallocate the VM to increase disk size. Afterward, extend the volume within the guest OS.

---

## 6. Deploy Virtual Machines to Availability Zones and Availability Sets

To ensure high availability and protect against hardware or datacenter failures, Azure provides Availability Sets and Availability Zones. *Note: These can only be configured at VM creation.*

### Availability Sets (99.95% SLA)

Protects against localized hardware failures within a single datacenter.

* **Fault Domains (FD):** Physical racks with their own power and network switch. VMs in an AvSet are distributed across up to 3 FDs.
* **Update Domains (UD):** Logical groups of VMs that can be rebooted at the same time during planned maintenance. Azure guarantees that only one UD is rebooted at a time.

### Availability Zones (99.99% SLA)

Protects against entire datacenter failures.

* Zones are physically separate datacenters within the same Azure region.
* Each zone has independent power, cooling, and networking.
* Deploying VMs across Zone 1, Zone 2, and Zone 3 ensures maximum resiliency.

---

## 7. Deploy and Configure Azure Virtual Machine Scale Sets (VMSS)

Virtual Machine Scale Sets let you create and manage a group of load-balanced VMs. The number of VM instances can automatically increase or decrease in response to demand or a defined schedule.

### Key Benefits:

* **High Availability:** Distributes VMs across Availability Zones or Fault Domains.
* **Auto-scaling:** Automatically match capacity to workload demand based on metrics (e.g., CPU percentage) or schedules (e.g., Monday at 8 AM).
* **Centralized Management:** Manage, configure, and update a large number of VMs from a single central profile.

### Orchestration Modes:

* **Uniform Orchestration:** Uses a single VM profile/template. All VMs are identical. Ideal for stateless web tiers.
* **Flexible Orchestration:** Allows you to mix and match different VM sizes and OS types within the same scale set. Acts as a modern replacement for Availability Sets.

### Upgrade Policies:

When you update the scale set model (e.g., changing the custom script extension), how instances are updated depends on the policy:

* **Automatic:** Instances are updated randomly.
* **Rolling:** Updates are rolled out in batches, pausing in between to ensure health.
* **Manual:** You must manually select instances to be upgraded to the latest model.

```

```
