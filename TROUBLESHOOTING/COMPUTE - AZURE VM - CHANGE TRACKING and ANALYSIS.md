# Azure Compute VM: Change Tracking & Analysis

In cloud operations (CloudOps), one of the most critical steps during **Root Cause Analysis (RCA)** and major incident management is identifying what recently changed. Whether an application suddenly breaks, a service stops, or connectivity is lost, identifying configuration changes is usually the fastest path to resolution.

In Azure, monitoring these changes across your Virtual Machines is primarily handled through two distinct but complementary tools:
1. **Azure Change Analysis** (For Platform/Resource-level changes)
2. **Change Tracking & Inventory** (For OS/Guest-level changes)

This guide breaks down how to use both tools to effectively troubleshoot and resolve issues in Azure VMs.

[![Azure CloudOps - Change Tracking & Analysis](https://img.youtube.com/vi/uP1MDwKFGcA/hqdefault.jpg)](https://youtu.be/uP1MDwKFGcA)

---

## 1. Azure Change Analysis (Resource Level)

**Change Analysis** tracks changes made to the Azure resources themselves through the Azure Resource Manager (ARM). It gives you a tenant-wide view of modifications without requiring you to log into the server.

### What It Tracks
* **Network Security Group (NSG) Rule modifications** (e.g., a port 80/445 rule was unexpectedly added or deleted).
* **Virtual Machine configuration changes** (e.g., changes to tags, VM sizing, or IP address allocation).
* **Platform-level settings** that might affect connectivity or availability.

### How to Use for Troubleshooting
1. Search for **Change Analysis** in the Azure Portal search bar.
2. Filter by **Subscription**, **Resource Group**, or specific **Virtual Machine**.
3. Set the **Time Range** (e.g., Last 24 hours, or a Custom range mapping to the time the incident started).
4. Review the **Old Value** vs. **New Value**. 
5. Identify the **Changed By** field to see the exact user ID, service principal, or automation account that triggered the change. 
6. **Remediation:** If an unauthorized or breaking change occurred (like an NSG port block), you can click into the resource directly from the Change Analysis pane and roll back the configuration.

*Note: Change Analysis data is automatically retained and queryable for **14 days** by default using Azure Resource Graph.*

---

## 2. Change Tracking & Inventory (OS/Guest Level)

While Change Analysis handles the Azure Resource level, **Change Tracking & Inventory** tracks what is happening *inside* the Virtual Machine operating system. This is crucial for tracking down application-level disruptions.

### What It Tracks
* **Windows & Linux Software:** Identifies newly installed patches, hotfixes, or software packages.
* **Windows Services & Linux Daemons:** Tracks when critical services stop, start, or have their startup type modified.
* **Windows Registry:** Monitors critical OS registry key modifications.
* **Linux & Windows Files:** Tracks file integrity, such as modifications to `/etc/hosts` or web server configuration files (e.g., `/etc/apache.conf`).

### Setting Up and Troubleshooting
To use this, your VMs must forward data to an **Azure Log Analytics Workspace**. 
1. Navigate to your Virtual Machine in the Azure Portal.
2. Under the **Operations** section, select **Inventory** or **Change Tracking**.
3. Here, you can view all installed software (including background hotfixes), and active services/daemons. 
4. In the **Change Tracking** tab, a visual timeline will show exact events (e.g., at 6:00 AM, `sshd` or `chronyd` was restarted; at 6:30 AM, a Microsoft Defender update was installed).

### ⚙️ Configuration Best Practices & Cost 
If you are managing this solution as a Cloud Engineer, keep these best practices in mind:
* **Collection Frequency:** You can configure how often the agent checks for changes (e.g., 10 seconds, 1 minute, 30 minutes). **Do not set this too aggressively.** If you track Windows Services every 10 seconds, short-lived service state changes might still be missed, and the system may generate excessive, noisy logs. A 30-minute interval is a standard baseline.
* **Avoid General Paths:** When configuring File Tracking, avoid tracking entire root directories (like `C:\` or `/etc/`). Specify exact files (e.g., `/etc/apache.conf`) to prevent the agent from traversing thousands of files, which causes performance degradation and high storage costs.
* **Cost Implications:** The average Log Analytics data ingestion cost for a machine using Change Tracking and Inventory is approximately **40 MB per month**. If you apply tracking to hundreds of VMs with aggressive collection intervals, your Log Analytics billing will spike significantly. 
* **Limits:** Be aware of Azure's hard limits. For instance, Windows Software tracking handles up to 250 items (excluding hotfixes), and Linux daemons have a limit of 250 items.

---

### CloudOps Pro Tip 💡
> *When an incident triggers, do not log directly into the VM server (RDP/SSH) right away. Start your RCA from the Azure Portal. Check **Change Analysis** for immediate network/ARM changes, and then verify the **Change Tracking** timeline to see if a recent patch update or service failure triggered the outage. This top-down approach saves critical minutes during a Sev-1 incident.*
