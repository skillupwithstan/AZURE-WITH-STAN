# Azure Network Watcher: Next Hop Troubleshooting

In an Azure networking environment, routing issues are a common cause of connectivity failures. When you have multiple default system routes, Virtual Network Peering, VPN Gateways, ExpressRoute, and custom User-Defined Routes (UDRs), it can be difficult to determine exactly where a packet will be sent.

**Next Hop**, a diagnostic tool within Azure Network Watcher, helps you determine the immediate next routing jump for a packet destined for a specific IP address.

[![AZURE AZ 104 - NETWORK WATCHER - NEXT HOP](https://img.youtube.com/vi/wAL-EgpH_jg/hqdefault.jpg)](https://youtu.be/wAL-EgpH_jg)

## 1. What is Next Hop?

The Next Hop tool answers a simple but critical question: *"If a packet leaves this Virtual Machine heading towards Destination IP X, what is the exact next device/network it will hit, and which route table is forcing it there?"*

### Common Next Hop Types
When you run a Next Hop test, Azure evaluates the "Effective Routes" applied to the VM's Network Interface (NIC) and returns one of the following Hop Types:

*   **Internet:** The traffic is routed directly to the public internet using the default Azure system route.
*   **VirtualNetwork:** The traffic stays within the same Azure VNet (e.g., VM to VM communication within the same or different subnets in the same VNet).
*   **VirtualNetworkPeering:** The traffic is routed to another VNet across a configured VNet peering connection.
*   **VirtualNetworkGateway:** The traffic is routed to a VPN Gateway or ExpressRoute gateway (usually destined for an on-premises network).
*   **VirtualAppliance:** The traffic is being forced to a Network Virtual Appliance (NVA), such as a Palo Alto, Fortinet, or Azure Firewall, usually due to a User-Defined Route (UDR).
*   **None:** The traffic is being dropped (black-holed). This means there is no valid route to the destination, or a UDR has explicitly routed the traffic to an invalid/unreachable IP.

---

## 2. Troubleshooting Routing Issues with Next Hop

Next Hop is primarily used to validate if traffic is being routed *as intended* or if a misconfiguration is sending traffic to a "black hole."

### Scenario 1: Validating Internet Access
*   **Symptom:** A Web Server VM cannot reach a public API or download an OS update.
*   **Action:** Run a Next Hop test from the Web Server VM to `8.8.8.8` (Google DNS).
*   **Expected Result:** The Next Hop type should be **Internet**.
*   **Troubleshooting:** If the result returns **VirtualAppliance**, it means a UDR (often `0.0.0.0/0` Forced Tunneling) is intercepting the traffic and sending it to a firewall. If that firewall is misconfigured or blocking the traffic, the connection will fail.

### Scenario 2: Troubleshooting UDR Precedence
Azure routing follows a strict precedence rule. If multiple routes exist, the most specific route (Longest Prefix Match) wins. If the prefixes are the same, UDRs override System Routes.
*   **Symptom:** You added a Route Table to force traffic to a Firewall (NVA), but traffic is still bypassing the firewall.
*   **Action:** Run a Next Hop test to the destination IP.
*   **Troubleshooting:** The output will tell you the **Route Table ID** responsible for the routing decision. If it doesn't show your custom Route Table name, you likely forgot to **Associate** the Route Table with the VM's Subnet. (A Route Table does nothing until it is explicitly associated with a Subnet).

### Scenario 3: Black-hole Routing (None)
*   **Symptom:** A VM completely loses connectivity to a specific subnet.
*   **Action:** Run Next Hop to that subnet's IP range.
*   **Troubleshooting:** If the result is **None**, look at the associated Route Table name in the output. This often happens if someone creates a UDR pointing traffic to a Virtual Appliance IP (e.g., `10.0.0.11`) that does not exist, is powered off, or has IP forwarding disabled on its NIC.

---

## 3. How to Use the Next Hop Tool

1.  Navigate to **Network Watcher** in the Azure Portal.
2.  Under the **Network diagnostic tools** section, select **Next hop**.
3.  Provide the following details:
    *   **Subscription & Resource Group:** Select the group containing your source VM.
    *   **Virtual Machine:** Select the VM you are troubleshooting.
    *   **Network Interface:** Select the primary NIC of the VM.
    *   **Source IP address:** This will auto-populate with the internal IP of the VM.
    *   **Destination IP address:** Enter the IP you are trying to reach (e.g., an on-premise IP, a peered VM IP, or `8.8.8.8`).
4.  Click **Next hop**.

The result will display the Next Hop Type, the IP address of that hop (if applicable, like an NVA IP), and the specific Route Table ID that made the routing decision.

---

### CloudOps Pro Tip 💡
> *If you remove a custom Route Table from a subnet to fix a "None" routing issue, the Subnet immediately falls back to the default Azure System Routes. Always check the **Effective Routes** blade on the VM's Network Interface (Help > Effective routes) to see a complete, prioritized list of all active routes before and after making UDR changes.*
