# 03 - TRAFFIC MANAGER

[![AZURE - DNS - TRAFFIC MANAGER - LOAD BALANCER - TAMIL - ARCHIVE (2020)](https://img.youtube.com/vi/vC38fDpE1ro/0.jpg)](https://youtu.be/vC38fDpE1ro)
*Watch the full session archive on the [Skill Up with Stan YouTube channel](https://youtu.be/vC38fDpE1ro).*

---

## 1. What is Azure Traffic Manager?

**Azure Traffic Manager** is a DNS-based global traffic load balancer. Unlike standard network load balancers that proxy the actual data payload, Traffic Manager operates entirely at the DNS layer (Layer 7 for DNS). 

It allows you to distribute user traffic for service endpoints situated in different Azure regions across the globe. It acts as a highly available and responsive DNS routing service that directs client requests to the most appropriate endpoint based on the routing method you select.

*   **Global Scope:** It load-balances traffic globally, not just within a single region.
*   **Protocol Agnostic:** Because it operates at the DNS level, it can route traffic for any protocol (HTTP, HTTPS, RDP, TCP, UDP, etc.).
*   **Resiliency:** Traffic Manager provides high availability and quick responsiveness by monitoring endpoints and automatically failing over to healthy nodes if an endpoint goes down.

---

## 2. How Azure Traffic Manager Works

When a client wants to connect to your application, the process works as follows:

1.  **DNS Query:** The client’s browser or application makes a DNS query to resolve your service's domain name (e.g., `www.myapp.com`).
2.  **CNAME Resolution:** Your enterprise DNS routes the query using a Canonical Name (CNAME) record to the Azure Traffic Manager profile (e.g., `myapp.trafficmanager.net`).
3.  **Routing Decision:** Traffic Manager checks the health of all endpoints, applies the configured **Routing Method**, and determines which endpoint IP should receive the traffic.
4.  **DNS Response:** Traffic Manager returns the IP address of the chosen endpoint back to the client.
5.  **Direct Connection:** The client connects **directly** to the returned IP address. Traffic Manager does *not* see or proxy the actual application data.

---

## 3. Traffic Routing Methods

Traffic Manager provides six distinct traffic routing methods to determine how to distribute traffic to various endpoints:

| Routing Method | How it Works | Best Used For |
| :--- | :--- | :--- |
| **Priority** | You assign a priority number to each endpoint (1, 2, 3...). All traffic goes to the primary (Priority 1) endpoint. If it fails, traffic automatically fails over to Priority 2. | Active/Passive architectures, Disaster Recovery (DR) failovers. |
| **Performance** | Routes the user to the endpoint with the lowest network latency from the user's geographic location. | Global applications where you want users to experience the fastest possible loading times. |
| **Geographic** | Routes users to specific endpoints based on the geographic location their DNS query originates from (e.g., Europe users to the EU datacenter, India users to the India datacenter). | Data sovereignty compliance, geo-fencing, and localized content delivery. |
| **Weighted** | Distributes traffic across endpoints based on custom weights you assign (e.g., 70% to Server A, 30% to Server B). | A/B testing, gradual blue/green deployments, or migrating traffic slowly to a new datacenter. |
| **Subnet** | Routes traffic to endpoints based on the specific end-user IP address ranges (CIDR blocks) you define. | Routing corporate intranet traffic to specific endpoints, or overriding normal routing for specific ISPs. |
| **Multi-value** | Returns multiple healthy endpoints (IPv4 or IPv6) in a single DNS response. The client side handles selecting the endpoint. | Client-side load balancing, increasing reliability if a single endpoint fails during the connection. |

---

## 4. Supported Endpoints

An endpoint is the destination where Traffic Manager routes the DNS request. Traffic Manager supports three types of endpoints:

*   **Azure Endpoints:** Native Azure services hosted in an Azure region (e.g., Azure App Service, Azure Public IPs attached to a Load Balancer or VM).
*   **External Endpoints:** Endpoints located outside of Azure. This could be an on-premises datacenter, a different cloud provider (like AWS or GCP), or any service with a public IP or FQDN.
*   **Nested Endpoints:** You can nest Traffic Manager profiles within another Traffic Manager profile. This allows you to combine routing methods (e.g., Geographic routing at the top level, and Performance routing at the nested level) for complex hybrid architectures.

---

## 5. Health Monitoring and Failover

Traffic Manager continuously monitors the health of all endpoints using HTTP, HTTPS, or TCP probes. 

*   **Probing Intervals:** You can configure how often Traffic Manager checks the endpoints (e.g., 10 seconds or 30 seconds).
*   **Endpoint Status:** If an endpoint fails consecutive probes, its status is marked as *Degraded*. 
*   **Automatic Failover:** Once an endpoint is degraded, Traffic Manager stops returning its IP in DNS queries and automatically routes traffic to the next healthy endpoint based on your routing rules. When the degraded endpoint recovers, traffic automatically resumes.
