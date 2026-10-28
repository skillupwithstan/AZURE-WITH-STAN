
# 02 - HOW TO CHOOSE LOAD BALANCER

[![AZURE ARCHITECT SERIES IN TAMIL - HOW TO CHOOSE LOAD BALANCER](https://img.youtube.com/vi/G9_zKPLqkEc/0.jpg)](https://youtu.be/G9_zKPLqkEc)
*Watch the full architectural breakdown on the [Skill Up with Stan YouTube channel](https://youtu.be/G9_zKPLqkEc).*

---

## 1. Decision-Making Framework

Choosing the right load balancer in Azure is not a one-size-fits-all scenario. As an architect, you must evaluate several distinct variables to determine the most effective, secure, and cost-efficient traffic distribution method for your application.

Azure provides a native "Help me choose" flow in the portal, but fundamentally, your choice rests on three primary pillars:

1.  **Traffic Type (OSI Layer):** Is it HTTP/HTTPS (Web) traffic, or non-web TCP/UDP traffic?
2.  **Traffic Scope:** Is your application deployed in a single region (Regional) or across multiple regions globally (Global)?
3.  **Application Requirements:** Do you need SSL/TLS offloading, a Web Application Firewall (WAF), or performance acceleration (caching)?

---

## 2. Decision Tree Matrix

Use this matrix to rapidly narrow down your Azure Load Balancer choice:

| Traffic Scope | Traffic Type | Recommended Load Balancer | Key Capabilities |
| :--- | :--- | :--- | :--- |
| **Regional** | Non-HTTP/HTTPS (TCP/UDP) | **Azure Load Balancer (ALB)** | Layer 4 routing, ultra-low latency, handles millions of TCP/UDP flows. Good for internal database tiers or generic TCP services. |
| **Regional** | HTTP/HTTPS (Web) | **Azure Application Gateway** | Layer 7 routing, URL path-based routing, SSL offloading, Web Application Firewall (WAF). |
| **Global** | Non-HTTP/HTTPS (TCP/UDP) | **Azure Traffic Manager** | DNS-based global routing, endpoints can be anywhere (Azure, AWS, On-Premises), handles any protocol. |
| **Global** | HTTP/HTTPS (Web) | **Azure Front Door** | Global Edge network, performance acceleration (CDN caching), global WAF, SSL offloading. |

---

## 3. Deep Dive: When to use which?

### A. Azure Load Balancer (Layer 4)
*   **Best For:** Workloads that are not strictly web-based. Examples include Active Directory, SMTP servers, real-time voice/video streaming, gaming servers, or generic database clusters (SQL, MySQL).
*   **Visibility:** It only looks at the Source IP, Source Port, Destination IP, Destination Port, and Protocol type (TCP/UDP). It does not understand HTTP headers or URL paths.
*   **Variants:** 
    *   **Basic SKU:** Retiring soon (September 2025). Only suitable for non-production sandboxes. Open by default (less secure).
    *   **Standard SKU:** The production default. Operates on a "Zero Trust" model (closed by default, requires explicitly allowed NSG rules). Supports Availability Zones (Zonal redundancy).

### B. Azure Application Gateway (Layer 7)
*   **Best For:** Complex, single-region web applications.
*   **Visibility:** Understands HTTP/HTTPS traffic. It can route traffic based on URL paths (e.g., sending `/images` to one backend pool and `/video` to another) or host headers.
*   **Key Features:** SSL termination (offloading decryption from the backend servers), cookie-based session affinity, and Web Application Firewall (WAF) to block SQL injection or cross-site scripting (XSS).

### C. Azure Front Door (Global Layer 7)
*   **Best For:** Internet-facing web applications with a globally distributed user base.
*   **Visibility:** Understands HTTP/HTTPS.
*   **Key Features:** Global routing to the closest available region, built-in Content Delivery Network (CDN) to accelerate static content delivery, global WAF, and DDoS protection at the Azure network edge.

### D. Azure Traffic Manager (Global DNS Layer)
*   **Best For:** Global distribution of non-web traffic, or hybrid architectures where you route traffic between Azure and on-premises datacenters.
*   **Visibility:** It is merely a DNS-level router. It does not touch the actual network traffic; it simply resolves a user's DNS query to the most appropriate IP address based on routing methods (Priority, Weighted, Performance, Geographic).

---

## 4. Example Architectures

**Scenario 1: Global Web E-Commerce Application**
*   **Requirement:** Users worldwide need fast access, protection against web exploits, and traffic must failover between the East US and West Europe regions.
*   **Solution:** Use **Azure Front Door** at the global edge to provide caching, WAF, and global routing. Behind Front Door, in each region, use an **Internal Azure Load Balancer** to distribute traffic across your backend VM scale sets.

**Scenario 2: Internal 3-Tier Enterprise Application**
*   **Requirement:** A regional HR application. The Web Tier must talk to the App Tier, which talks to the Database Tier. Everything must remain private.
*   **Solution:** 
    *   Web Tier to App Tier: Use an **Internal Application Gateway** if you need URL routing (Layer 7), or an **Internal Standard Azure Load Balancer** if it's purely internal API traffic (Layer 4).
    *   App Tier to DB Tier: Use an **Internal Standard Azure Load Balancer** (Layer 4) since databases communicate over TCP, not HTTP.
