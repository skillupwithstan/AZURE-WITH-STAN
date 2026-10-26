[![AZURE IN ENGLISH - VIRTUAL NETWORK - FILTERING](https://img.youtube.com/vi/-rhG55c0DoM/hqdefault.jpg)](https://youtu.be/-rhG55c0DoM)

Key concepts from video:

Network Security Groups (NSG) at the subnet level and network card level.

Difference between Ingress (Inbound) and Egress (Outbound) rules.

Priority rule processing: Rules starting from 65000+ are default Microsoft rules. User-defined rules typically run 100 to 4096. Lower number = higher priority (processed first).

Components of a rule: Priority, Source, Destination, Port range, Protocol (TCP/UDP/ICMP), Action (Allow/Deny).

Source types: Any, specific IP ranges (CIDR), My IP address, Service Tags (e.g., Azure Storage, Load Balancer), Application Security Groups (ASG).

Best practices: Minimize redundant NSGs (use a common NSG for similar tiers), use ASGs (Web, App, DB tiers) for granular control, use Service tags instead of hardcoding Microsoft IPs.

Troubleshooting: VM unable to reach HTTP due to missing inbound allow rule on port 80. VM locked out due to network interface detachment/IP changes.

Structure the Markdown:

Title: # Azure Virtual Network Filtering: Troubleshooting Guide

Intro: Quick definition of VNet filtering using NSGs and ASGs.

Section 1: Understanding Network Security Groups (NSGs) (Priorities, Ingress/Egress, Default rules vs User rules).

Section 2: Key Components of a Security Rule (Source, Port, Service Tags, ASGs).

Section 3: Application Security Groups (ASGs) Explained (Grouping by role: Web, App, DB).

Section 4: Common Troubleshooting Scenarios (Cannot access web server, conflicting rules).

Section 5: CloudOps Best Practices.

Thumbnail link at the bottom.

Drafting the Content:

Self-Correction during drafting: Keep it consistent with the existing AZURE-WITH-STAN repository style (which I generated in previous turns: clear headings, bullet points, CloudOps pro-tips, and a markdown thumbnail at the end).

Focus: Highlight the "priority" mechanism (lower number = higher precedence) as it's a common source of troubleshooting errors.

Service Tags & ASGs: Explain these clearly as they are crucial for modern Azure deployments (instead of managing hundreds of IPs).

Refining formatting: Ensure no meta-announcements, use bolding for emphasis. No LaTeX needed here.

Final Polish: Output directly as standard Markdown to be copy-pasted. Include the requested thumbnail link format.# Virtual Network Filtering: Troubleshooting and Best Practices

In Azure, limiting or controlling the flow of network traffic—whether it is incoming (Ingress) or outgoing (Egress)—is handled primarily through Network Security Groups (NSGs) and Application Security Groups (ASGs). Unlike the classic Azure model or AWS (which uses Network ACLs for subnets and Security Groups for instances), Azure simplifies this by allowing you to attach an NSG to either a Subnet or a Network Interface (NIC).

This guide outlines how Virtual Network filtering works in Azure and how to troubleshoot common connectivity drops caused by misconfigured rules.

1. How Network Security Groups (NSGs) Evaluate Traffic
An NSG is a virtual firewall containing a list of security rules that allow or deny network traffic. When troubleshooting a blocked connection, understanding rule priority is critical.

Rule Priority (The "Lower Wins" Rule): NSG rules are evaluated based on their priority number. The lowest number has the highest precedence.

If you have a Deny Any rule at priority 1000, and an Allow Port 80 rule at priority 2000, Port 80 will be blocked.

Default Rules (65,000+): Microsoft populates every NSG with default rules starting at priority 65000 (e.g., allowing intra-VNet traffic, allowing Azure Load Balancer probes, and denying all other inbound traffic). You cannot delete these, but you can override them by creating rules with a lower priority number.

User-Defined Rules (100 - 4096): It is a CloudOps best practice to place your custom rules between priority 100 and 4096. Leave gaps (e.g., 100, 110, 120) so you can insert emergency rules later without having to renumber everything.

2. Key Components of an NSG Rule
When creating or troubleshooting an NSG rule, verify these parameters:

Source / Destination: You can specify Any, a single IP (/32), a CIDR block (e.g., 10.0.0.0/24), or a comma-separated list of IPs (without spaces).

Port Range: Can be a single port (80), a range (1024-65535), or a comma-separated list.

Protocol: TCP, UDP, ICMP (Ping), or Any.

Advanced Source Types (Highly Recommended)
Instead of managing hardcoded IP addresses, use Azure's dynamic identifiers:

Service Tags: Microsoft manages predefined IP ranges for specific Azure services. Instead of hardcoding Azure's storage IP addresses (which can change), use the Storage service tag. Other common tags include Sql, AzureLoadBalancer, and Internet.

Application Security Groups (ASGs): ASGs allow you to group VMs based on their application tier/role, completely eliminating the need to manage explicit IP addresses in your firewall rules.

3. Application Security Groups (ASGs): The Modern Approach
In a standard three-tier architecture (Web, App, DB), managing NSGs using static IP addresses becomes chaotic as VMs scale in and out.

Instead, group your VMs using ASGs:

Create an ASG named Web-Tier-ASG and attach your front-end VMs to it.

Create an ASG named App-Tier-ASG and attach your middleware VMs.

Create an ASG named DB-Tier-ASG and attach your database VMs.

Troubleshooting / Configuration Example:
To secure the database, you simply create a single NSG rule on the Database subnet:

Source: App-Tier-ASG

Destination: DB-Tier-ASG

Port: 1433 (SQL)

Action: Allow

If a Web VM is unexpectedly able to reach the Database VM, check if it was accidentally added to the App-Tier-ASG instead of the Web-Tier-ASG.

4. Common Filtering Troubleshooting Scenarios
Scenario A: Web Server is Inaccessible from the Internet
Symptom: You deployed an IIS/Nginx web server, attached a Public IP, but it times out in the browser. You can reach it locally via 127.0.0.1 (loopback).

Root Cause: The default NSG rules block all inbound internet traffic.

Fix: Add an inbound rule on the Subnet or NIC NSG: Source: Any (or Internet Service Tag), Destination: Any, Port: 80/443, Action: Allow. Give it a priority of 100.

Scenario B: Cannot Change Subnet IP Ranges
Symptom: You are trying to alter the CIDR block of a subnet (e.g., expanding from /24 to /23) but Azure blocks the action.

Root Cause: You cannot modify a subnet's address space if there are active Network Interfaces (NICs) connected to it.

Fix: Stop the VM, completely detach the NIC from the VM, delete the NIC, resize the subnet, and then recreate and reattach the NIC.

CloudOps Pro Tip 💡
Do not create a unique NSG for every single Virtual Machine. This creates "NSG Sprawl" and makes centralized auditing nearly impossible. Attach a single, well-defined NSG at the Subnet level and use ASGs to differentiate access rules for individual application tiers within that subnet.
