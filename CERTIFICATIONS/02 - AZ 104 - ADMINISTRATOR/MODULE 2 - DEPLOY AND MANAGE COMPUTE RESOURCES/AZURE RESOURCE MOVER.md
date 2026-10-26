# Microsoft Azure Resource Mover

**Azure Resource Mover** is a centralized, unified hub within the Azure portal that simplifies and orchestrates the movement of Azure resources. It significantly reduces the complexity, time, and manual effort historically required to clone, back up, or recreate resources in a new location.

With Azure Resource Mover, you can move resources across:
1. **Azure Regions** (e.g., from East US to Central India)
2. **Azure Subscriptions** (within the same tenant)
3. **Resource Groups** (within the same subscription)

---
[![A TO Z OF AZURE IN ENGLISH - AZURE RESOURCE MOVER](https://img.youtube.com/vi/X5YeTXZRN1Y/hqdefault.jpg)](https://youtu.be/X5YeTXZRN1Y)
---
[![YouTube Video](https://img.youtube.com/vi/bnJl1Asrl9A/hqdefault.jpg)](https://youtu.be/bnJl1Asrl9A)
---

## 1. Why Use Azure Resource Mover? (Use Cases)

Organizations move resources for a variety of strategic, compliance, and operational reasons:

*   **Regional Expansion:** Microsoft constantly launches new Azure regions. You may want to move workloads to a newly opened region that is physically closer to your end-users to reduce latency.
*   **Data Residency & Compliance:** To meet General Data Protection Regulation (GDPR) or local government data classification laws, data and compute resources must sometimes be physically relocated to specific geographical boundaries.
*   **Mergers, Acquisitions & Splits:** When organizations merge or split departments, IT assets often need to be reorganized, consolidating or separating billing boundaries by moving resources to new subscriptions.
*   **Feature Availability:** Not all Azure features (like specific VM SKUs or Availability Zones) are available in every region. You might move a resource to a region that supports the specific capability you need.
*   **Decommissioning:** Migrating resources out of an older region or subscription before decommissioning it.

---

## 2. What Resources Can Be Moved?

While Azure Resource Mover supports a wide array of core IaaS and PaaS services, it does *not* support everything (e.g., Azure Spot VMs cannot be moved due to their capacity-dependent pricing model). 

**Commonly Supported Resources:**
*   Virtual Machines (VMs) and associated Disks
*   Virtual Networks (VNets), Network Interface Cards (NICs), and Public IPs
*   Network Security Groups (NSGs)
*   Load Balancers (Internal and Public)
*   Availability Sets
*   Azure SQL Databases & Elastic Pools

---

## 3. The Movement Process: Step-by-Step

Azure Resource Mover handles
