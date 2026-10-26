# Azure Network Watcher: Connection Monitor Troubleshooting

In Azure Cloud Operations, identifying whether a connectivity issue is caused by a local firewall, a routing misconfiguration, or an underlying platform outage is critical. **Connection Monitor**, a feature within Azure Network Watcher, provides unified end-to-end connection monitoring. 

[![AZURE AZ 104 - NETWORK WATCHER - CONNECTION MONITOR](https://img.youtube.com/vi/CS8jI8VMBeI/hqdefault.jpg)](https://youtu.be/CS8jI8VMBeI)

It is designed to replace older legacy tools (like the classic Connection Monitor and Network Performance Monitor) by bringing hybrid monitoring under one umbrella.

## 1. What is Connection Monitor?

Connection Monitor allows you to verify connectivity between a source and a destination, measuring:
*   **Latency / Round-Trip Time (RTT)**
*   **Packet Loss**
*   **Topology / Hop-by-hop paths**

### Supported Scenarios:
It is a **Hybrid Monitoring Tool**, meaning it supports cross-environment checks:
*   **Azure to Azure:** (e.g., VM in East US to a VM in West Europe, or Jump Server to a Backend Database).
*   **Azure to On-Premises:** (e.g., Azure VM to an on-premise datacenter over ExpressRoute/VPN).
*   **Azure to Multi-Cloud:** (e.g., Azure VM to an AWS EC2 instance).
*   **Azure to SaaS/Endpoints:** (e.g., Azure VM to `www.google.com` or an Office 365 endpoint).

---

## 2. Setting Up a Test Group

When troubleshooting a specific flow (e.g., "Web Server cannot reach Database"), you create a Test Group within Connection Monitor.

### Components of a Test Group:
1.  **Sources:** The origin point. This can be an Azure VM, a Virtual Machine Scale Set (VMSS), or an on-premises machine (requires Log Analytics Agent).
    *   *Note:* When you select an Azure VM as a source, Azure automatically installs the **Network Watcher Extension** on the VM. It takes a few minutes to transition from "Unavailable" to "Ready."
2.  **Destinations:** The endpoint you are trying to reach. This can be an Azure VM, an external IP address, or a Fully Qualified Domain Name (FQDN) like `www.skillupwithstan.com`.
3.  **Test Configurations:** Defines *how* to test the connection.
    *   **Protocol:** TCP, ICMP (Ping), or HTTP/HTTPS.
    *   **Port:** The specific destination port (e.g., Port 80 for HTTP, Port 1433 for SQL).
    *   **Frequency:** How often the test runs (e.g., every 30 seconds).
    *   **Thresholds:** Define what constitutes a "failure" (e.g., if Packet Loss > 10% or Latency > 100ms, mark as failed).

---

## 3. How to Use Connection Monitor for RCA (Root Cause Analysis)

When an outage occurs, Connection Monitor is invaluable for pinpointing the exact failure point.

### A. Topology Visualization
Once a test is running, you can click on the **Topology** view. This provides a visual map of the entire network path.
*   **Hop-by-Hop Visibility:** It shows every router, gateway, and Virtual Network Appliance (NVA) the traffic passes through.
*   **Identifying the Block:** If traffic is dropping, the topology map will highlight exactly which hop is causing the failure (e.g., a specific Azure Firewall or a misconfigured Network Security Group blocking the traffic).

### B. Protocol Testing Limitations
*   If you choose **ICMP** and the test fails, remember that many Azure services and local OS firewalls (like Windows Defender) block ICMP by default.
*   Always test the specific application port (e.g., **TCP 443**) rather than relying solely on ICMP pings to verify true application connectivity.

### C. Log Analytics Integration
All metrics and test results are stored in an **Azure Log Analytics Workspace**. 
*   This allows you to query historical data. If an application owner says, "The connection dropped at 2:00 AM," you can query the Log Analytics workspace to see if there was a spike in latency or a sudden packet loss exactly at that time.
*   You can also set up **Azure Monitor Alerts** based on these logs to proactively email your team or trigger a webhook when a connection drops below the defined thresholds.

---

## 4. Common Troubleshooting Scenarios

*   **Extension Missing:** If you cannot add a VM as a source, log into the VM and check the OS plugins folder (or the Azure Portal extensions blade) to ensure the Network Watcher extension was successfully provisioned.
*   **Cross-Subscription Issues:** Connection Monitor supports cross-subscription monitoring. If it fails, ensure your user account has `Network Contributor` access across both subscriptions, and that the Log Analytics Workspace has the correct RBAC permissions to ingest data.
