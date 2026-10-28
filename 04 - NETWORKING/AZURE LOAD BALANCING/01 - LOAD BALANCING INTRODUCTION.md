
# 01 - AZURE LOAD BALANCING INTRODUCTION

[![AZURE IN TAMIL - DAY 22 - PART 1 - INTRODUCTION TO LOAD BALANCERS](https://img.youtube.com/vi/nbNh0WxUQlc/0.jpg)](https://youtu.be/nbNh0WxUQlc)
*Watch the full architectural breakdown on the [Skill Up with Stan YouTube channel](https://youtu.be/nbNh0WxUQlc).*

---

## 1. What is Load Balancing?

Load balancing is the process of efficiently distributing incoming network traffic across a group of backend systems (such as a pool of virtual machines, container nodes, or appliances). 

In a cloud environment like Azure, a load balancer acts as the "traffic cop" sitting in front of your architecture. It routes client requests—originating from the internet, a VPN, or a corporate intranet—across all available servers capable of fulfilling those requests. This ensures maximum speed, optimal capacity utilization, and high availability.

### Public vs. Private Load Balancers
*   **Public (Internet-Facing) Load Balancers:** Handle incoming traffic directly from the internet. They are provisioned with a Public IP address on the frontend and distribute traffic to backend systems (which typically remain securely on private IPs).
*   **Private (Internal) Load Balancers:** Used to balance traffic within an isolated virtual network or corporate intranet. They use a Private IP address on the frontend. Common use cases include routing traffic from a web tier to a backend database tier, or handling traffic arriving via ExpressRoute/VPN from an on-premises corporate network.

---

## 2. Top 7 Benefits of Azure Load Balancing

Implementing a load balancing strategy provides several critical architectural advantages:

1.  **Optimal Resource Utilization:** Ensures no single server is overwhelmed while others remain idle, maximizing the efficiency of your compute, network, and storage resources.
2.  **Prevents Overloading:** Evenly distributes demand to prevent bottlenecks on a single resource or network path.
3.  **Avoids Single Point of Failure (SPOF):** Drastically improves High Availability (HA) and resilience. If a datacenter or server goes down, traffic is automatically routed to healthy nodes.
4.  **Seamless Scalability:** You can add or remove backend servers dynamically without disrupting incoming traffic or experiencing downtime.
5.  **Improved Throughput:** By utilizing multiple paths and endpoints across different availability zones or datacenters, overall network bandwidth and throughput increase.
6.  **Consistent Performance (Low Latency):** Routes users to the most appropriate, available, or fastest-responding node, ensuring a consistent end-user experience.
7.  **Advanced Security Capabilities:** Many load balancers offer native security features like SSL Offloading (decrypting traffic at the load balancer to save backend CPU cycles) and Network Address Translation (NAT) to hide backend private IPs from external exposure.

*(Bonus Feature: **Health Probes** continuously monitor the health of backend nodes. If a node fails a health check, the load balancer automatically stops sending traffic to it until it recovers).*

---

## 3. The 5 Types of Azure Load Balancers

Azure provides five primary load-balancing services, each designed for specific architectures and OSI layer requirements:

| Load Balancer Type | Scope | OSI Layer | Best Used For |
| :--- | :--- | :--- | :--- |
| **Azure Load Balancer** | Regional | Layer 4 (TCP/UDP) | High-performance, ultra-low latency network-level load balancing for VMs and VM Scale Sets (VMSS). |
| **Azure Application Gateway** | Regional | Layer 7 (HTTP/HTTPS) | Web traffic routing, URL-path based routing, SSL offloading, and Web Application Firewall (WAF) integration. |
| **Azure Front Door** | Global | Layer 7 (HTTP/HTTPS) | Global web applications requiring site acceleration (CDN), global failover, caching, and advanced WAF. |
| **Azure Traffic Manager** | Global | DNS-level | DNS-based global traffic routing across regions for non-HTTP(S) traffic or hybrid/on-premises environments. |
| **Azure Gateway Load Balancer** | Regional | Layer 3/4 | Transparently chaining Network Virtual Appliances (NVAs) like firewalls or intrusion detection systems directly into your network path. |

---

## 4. Key Factors When Choosing a Load Balancer

With multiple options available, Azure provides a "Help Me Choose" wizard in the portal. Here are the core factors an architect must evaluate when deciding which load balancer fits the design:

*   **Traffic Scope:** Is your application deployed in a single region (Regional) or across multiple geographical regions (Global)?
*   **Traffic Type:** Is the traffic standard web traffic (HTTP/HTTPS) requiring Layer 7 visibility, or is it non-web traffic (TCP/UDP) requiring raw Layer 4 routing?
*   **Security & Encryption:** Do you need SSL/TLS offloading, or deep packet inspection via a Web Application Firewall (WAF)?
*   **Performance Acceleration:** Does your application need edge caching and site acceleration to serve static content faster to end-users?
*   **Application Architecture:** Are you routing traffic to traditional Virtual Machines, Containerized microservices, or App Services?
*   **Budget & SLAs:** What is the maximum acceptable downtime, and what is the budget for the load-balancing service, data processing, and maintenance?
