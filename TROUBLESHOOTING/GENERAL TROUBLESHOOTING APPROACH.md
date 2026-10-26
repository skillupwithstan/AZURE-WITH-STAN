# General Troubleshooting Approach for Azure Virtual Machines

When a critical issue or outage occurs involving Azure Virtual Machines or supporting infrastructure, it is vital to follow a structured, top-down approach. Often, administrators immediately jump into checking the OS or application logs, missing broader platform-level outages that are outside their control.

This guide outlines the standard operating procedure (SOP) for troubleshooting Azure environments effectively.

---

[![Troubleshooting Series - Azure - General Approach](https://img.youtube.com/vi/1uqXL-QFHao/hqdefault.jpg)](https://youtu.be/1uqXL-QFHao)

## Step 1: Check Global and Regional Platform Health

Before investigating individual resources, verify if Microsoft Azure itself is experiencing an outage or degradation in the region where your resources are hosted.

1. **Azure Status Page:**
   * Visit [status.azure.com](https://status.azure.com/). This page provides a global view of the health of all Azure services across all regions.
   * If there is a major outage (e.g., Compute issues in East US), it will be reflected here.
2. **Azure DevOps Status (If applicable):**
   * If your CI/CD pipelines are failing to deploy to your VMs, check [status.dev.azure.com](https://status.dev.azure.com/).
3. **Twitter / X Alerts:**
   * Follow or check `@AzureSupport` on Twitter for real-time updates and acknowledgments of emerging platform issues.

## Step 2: Check Tenant/Subscription Specific Service Health

The public status page only shows massive global issues. For issues specifically affecting your tenant, subscription, or underlying hardware, you must check **Azure Service Health** within the Azure Portal.

1. Navigate to **Service Health** in the Azure Portal search bar.
2. **Service Issues:** Check if there are localized outages affecting your specific subscriptions or regions.
3. **Planned Maintenance:** Verify if Microsoft is performing scheduled maintenance on the host clusters where your VMs reside. This can cause unexpected reboots or unresponsiveness.
4. **Health Advisories:** Review advisories for deprecated features, security vulnerabilities, or required migrations that might be impacting your services.
5. **Create Health Alerts:** As a proactive measure, configure **Service Health Alerts** to notify your CloudOps team via email or webhook whenever an Azure outage or maintenance event impacts your specific resources.

## Step 3: Check Resource Health (VM Level)

If the platform is healthy, narrow your focus to the specific Virtual Machine.

1. Go to the Virtual Machine blade in the Azure Portal.
2. Scroll down to **Help > Resource health**.
3. This blade will tell you if the VM is **Available**, **Unavailable**, or in an **Unknown** state. 
4. Crucially, it will tell you *why*. It can distinguish between user-initiated actions (e.g., "You stopped this VM") and platform issues (e.g., "The host node experienced a hardware failure").

## Step 4: Investigate Changes (Change Tracking)

*“What changed recently?”* is the most important question in troubleshooting.
* Use **Azure Change Analysis** to see if anyone recently modified the VM size, Network Security Group (NSG) rules, or routing tables.
* If configured, check **Change Tracking and Inventory** to see if a recent OS patch, software installation, or service modification occurred right before the issue started.

## Step 5: Boot Diagnostics & Serial Console

If the VM is running but inaccessible (RDP/SSH is failing):
1. **Boot Diagnostics:** Check the boot diagnostics screenshot and serial log. This will show you if the OS is stuck on a blue screen (BSOD), a boot menu, or running a disk check (fsck/chkdsk).
2. **Serial Console:** Use the Azure Serial Console for direct, out-of-band management access to the VM's command line to fix firewall rules, GRUB bootloaders, or restart crashed services.

## Step 6: Redeploy / Reapply

If the VM appears completely stuck due to an underlying Azure host fault that hasn't automatically resolved:
* **Reapply:** This forces Azure to update the VM's state and configuration without moving it. It can fix VMs stuck in a "Failed" or "Updating" state.
* **Redeploy:** This shuts down the VM, moves it to a brand new physical host server in the Azure datacenter, and boots it back up. *Note: You will lose any data stored on the temporary disk.*

---

### 💡 Pro Tip for Cloud Operations
Always start from the outside in (Platform -> Network -> Host -> Guest OS). Never waste hours troubleshooting a local firewall or application issue when the entire Azure region is experiencing a compute degradation!
