# Azure Network Watcher: Introduction & Overview

In Microsoft Azure, monitoring and troubleshooting your networking infrastructure is critical to ensuring application availability and performance. While [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview) is the overarching tool for all Azure resources (including PaaS, databases, and containers), **Azure Network Watcher** is specifically designed for monitoring and repairing the network health of **Infrastructure-as-a-Service (IaaS)** products.

[![Mastering Microsoft Azure - Network Watcher Introduction](https://img.youtube.com/vi/TesF-WlJbyA/hqdefault.jpg)](https://youtu.be/TesF-WlJbyA)
http://googleusercontent.com/youtube_content/1

## 1. What is Azure Network Watcher?

Azure Network Watcher is a regional service that provides a suite of tools to monitor, diagnose, view metrics, and enable or disable logs for resources in an Azure virtual network.

It is designed to troubleshoot:
*   Virtual Machines (VMs)
*   Virtual Networks (VNets)
*   Application Gateways and Load Balancers
*   VPN Gateways and ExpressRoute
*   Network Security Groups (NSGs)

*Note: Network Watcher is strictly for IaaS. It is not designed or intended for Platform-as-a-Service (PaaS) monitoring or web analytics.*

---

## 2. Core Capabilities & Tool Categories

Network Watcher is not a single tool; it is a collection of distinct utilities categorized into three main areas: Monitoring, Network Diagnostic Tools, and Traffic (Logs & Analytics).

### A. Monitoring Tools
These tools provide end-to-end visibility and visual architecture mapping.
*   **Connection Monitor:** Monitors end-to-end connectivity, latency, and packet loss between Azure resources, hybrid environments (on-premises), and external endpoints (e.g., URLs, FQDNs).
*   **Topology:** Generates a visual map of your Azure networking infrastructure, showing the relationships and interconnectivity between resources (VNets, Subnets, VMs, NICs, NSGs) across a region.

### B. Network Diagnostic Tools
These are active troubleshooting tools used during an incident to find the root cause.
*   **IP Flow Verify:** Checks if a packet is allowed or denied to/from a specific Virtual Machine based on NSG rules. It tells you exactly *which* security rule blocked the traffic.
*   **Next Hop:** Determines the immediate next routing jump for a packet, helping you verify if User-Defined Routes (UDRs) or default system routes are functioning correctly (or if traffic is being sent to a black hole).
*   **Effective Security Rules:** Views all the security rules applied to a network interface, combining NSG rules from both the Subnet level and the NIC level to show the "effective" permissions.
*   **VPN Troubleshoot:** Diagnoses the health and connectivity of Virtual Network Gateways and their specific VPN connections.
*   **Packet Capture:** Creates packet capture sessions (PCAP files) directly on the VM to track traffic entering and leaving the machine without needing to log into the guest OS.

### C. Traffic (Metrics & Logs)
These tools are used for auditing, compliance, and deep traffic analysis over time.
*   **NSG Flow Logs:** Logs information about ingress and egress IP traffic flowing through an NSG. The logs are stored in an Azure Storage Account in JSON format.
*   **Traffic Analytics:** Ingests NSG Flow Logs into a Log Analytics Workspace to provide rich, visual dashboards. It helps identify top talkers, malicious IP traffic, open ports, and hybrid connectivity metrics.

---

## 3. Creation and Architecture Concepts

### Regional Construct
Network Watcher is a **regional service**. 
*   You can only have **one** Network Watcher instance per Azure region, per subscription.
*   When you create a Virtual Network in a region (e.g., East US), Azure will automatically create a Network Watcher instance in a hidden Resource Group named `NetworkWatcherRG` for that specific region.

### Disabling Auto-Creation
Some organizations prefer to manage their infrastructure strictly via Infrastructure as Code (IaC) and want to prevent Azure from automatically creating these instances. 
You can opt out of the automatic creation using the Azure CLI or Azure PowerShell. 

```bash

# Azure CLI Example to disable automatic Network Watcher creation
az feature unregister --namespace Microsoft.Network --name AllowNetworkWatcherAutocreation

4. Limits and Quotas
Like all Azure services, Network Watcher has regional and subscription limits. You can view these directly in the Network Watcher portal under Usage + quotas.

Example Soft Limits:

Maximum Packet Capture sessions per region: 10,000

Maximum Connection Monitors per subscription: 100

Maximum Test Groups per Connection Monitor: 20

If you hit a soft limit (indicated by a pencil icon in the portal), you can request an increase. If a hard limit is reached (indicated by a support icon), you must open a ticket with Azure Support to evaluate your architecture.

CloudOps Pro Tip 💡
Because Network Watcher relies on VM agents for tools like Packet Capture and Connection Monitor, always ensure the Network Watcher Agent virtual machine extension (for Windows/Linux) is properly installed and running on your endpoints. If the extension is broken, many diagnostic tools will fail to execute.

