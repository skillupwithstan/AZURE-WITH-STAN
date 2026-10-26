# Azure Network Watcher: Topology Troubleshooting

When managing complex cloud infrastructures, it can be difficult to visualize how different network resources are connected. **Azure Network Watcher Topology** is a powerful visualization tool that automatically generates a visual representation of your network architecture, allowing CloudOps engineers to quickly understand resource relationships and identify misconfigurations.

[![AZURE AZ 104 - NETWORK WATCHER SERIES - TOPOLOGY](https://img.youtube.com/vi/vA-Lf2wz35Y/hqdefault.jpg)](https://youtu.be/vA-Lf2wz35Y)

## 1. What is the Topology Tool?

The Topology tool provides an interactive, visual map of your Azure network infrastructure. It is designed **strictly for Azure environments** (unlike Connection Monitor, which handles hybrid on-premises connectivity).

It visually maps the relationships between various components:
*   Virtual Networks (VNets) and Subnets
*   Virtual Machines (VMs)
*   Network Interfaces (NICs)
*   Public IP Addresses
*   Network Security Groups (NSGs)
*   Route Tables (UDRs)
*   Load Balancers, Application Gateways, and VNet Peering connections

*Note: Topology is classified under the **Monitoring** category in Network Watcher. It is a visualization tool, not an active diagnostic tool (like Packet Capture or IP Flow Verify).*

---

## 2. How Topology Helps in Troubleshooting

While it doesn't actively send traffic to test connectivity, Topology is often the crucial **first step** in Root Cause Analysis (RCA). Instead of clicking through dozens of Azure Portal blades to verify configurations, Topology allows you to verify relationships at a glance.

### Key Troubleshooting Use Cases:

1.  **Validating Infrastructure as Code (IaC) Deployments:**
    If you deploy resources using Terraform, ARM templates, or Bicep, you can use Topology to visually confirm that all resources were created and attached correctly. *Did the NIC attach to the correct Subnet? Is the NSG properly applied?*
2.  **Identifying Misconfigured NSGs:**
    Sometimes an NSG is applied to the NIC, but a different NSG is applied to the Subnet. Topology visually shows both attachments, helping you identify overlapping or conflicting security rules that might be blocking traffic.
3.  **Detecting Routing Errors (UDRs):**
    If traffic is taking an unintended path, you can visually inspect if the correct Route Table is attached to the intended Subnet.
4.  **VNet Peering Verification:**
    Quickly confirm if VNet peerings are established and which networks are interconnected across different regions or resource groups.

---

## 3. Using the Topology Tool

1.  Navigate to **Network Watcher** in the Azure Portal.
2.  Under the **Monitoring** section, select **Topology**.
3.  **Filter your Scope:**
    *   Select the specific **Subscription**, **Resource Group**, and **Region/Location**.
    *   *Note: Topology only displays resources within the same region at one time. If you have a global architecture, you must filter by specific regions (e.g., East US vs. Central India).*
4.  **Interactive Navigation:**
    *   Click the **(+)** icon to drill down into specific VNets to see the Subnets.
    *   Drill down further into Subnets to see attached VMs, NICs, and Public IPs.
    *   Clicking on any specific resource node in the topology map will open a side panel allowing you to navigate directly to that resource's configuration blade for immediate remediation.
5.  **Exporting:** You can download the generated topology map as an SVG file, which is highly useful for High-Level Design (HLD), Low-Level Design (LLD), architecture reviews, or auditing purposes.

---

## 4. Prerequisites and Common Issues

If you cannot view the topology or resources are missing, check the following:

*   **Network Watcher Must Be Enabled:** Network Watcher must be explicitly enabled for the specific region (e.g., Central India) where the resources reside.
*   **RBAC Permissions:** You must have the appropriate Role-Based Access Control permissions. At a minimum, your account needs `Network Contributor` or `Reader` access to view the resources and the topology map.
*   **Resource Support:** Not all Azure resources are currently supported in the visualizer. If a newer PaaS service isn't showing up, check the official Microsoft documentation for the list of supported resource types.
