# 📘 Microsoft Azure: Jargons and Vocabulary

Welcome to the Azure glossary! When navigating Microsoft Azure, you will come across specific terminologies and jargons. This guide breaks down the essential vocabulary across different Azure domains to help you speak and understand the "language of the cloud."

---

## 🏗️ 1. General & Core Concepts

* **Resource:** The most basic building block in Azure. It is a manageable item available through Azure (e.g., a Virtual Machine, a Storage Account, a Web App, a Database).
* **Resource Group (RG):** A logical container that holds related resources for an Azure solution. Every resource *must* belong to exactly one resource group. 
* **Subscription:** A logical container used to provision resources in Azure. It serves as a billing and management boundary. All resources deployed are billed to a specific subscription.
* **Tenant / Directory:** A dedicated, isolated instance of Microsoft Entra ID (formerly Azure Active Directory) that represents an organization. It houses users, groups, and app registrations.
* **Management Group:** Containers that help you manage access, policy, and compliance across multiple Azure subscriptions. 
* **Region:** A geographical area on the planet containing at least one, but potentially multiple datacenters that are networked together with a low-latency network (e.g., `East US`, `West Europe`, `Central India`).
* **Availability Zone (AZ):** Unique, physically separate datacenters within an Azure region. They are designed to protect applications and data from datacenter failures.
* **Azure Resource Manager (ARM):** The deployment and management service for Azure. It provides a management layer that enables you to create, update, and delete resources in your Azure account.
* **ARM Template:** A JavaScript Object Notation (JSON) file that defines the infrastructure and configuration for your project (Infrastructure as Code).
* **Bicep:** A domain-specific language (DSL) that uses declarative syntax to deploy Azure resources. It is Microsoft's cleaner, easier-to-read alternative to ARM JSON templates.

---

## 💻 2. Compute Services

* **Virtual Machine (VM):** Software emulations of physical computers. They provide Infrastructure as a Service (IaaS) computing power in the cloud.
* **Virtual Machine Scale Sets (VMSS):** An Azure compute resource that lets you create and manage a group of identical, load-balanced VMs. They can automatically increase or decrease in number based on demand.
* **App Service:** A fully managed Platform as a Service (PaaS) offering used to build, deploy, and scale web apps, mobile app backends, and RESTful APIs.
* **Azure Functions:** A serverless compute service that lets you run event-triggered code without having to explicitly provision or manage infrastructure.
* **Azure Kubernetes Service (AKS):** A highly available, secure, and fully managed Kubernetes container orchestration service.

---

## 🌐 3. Networking

* **Virtual Network (VNet):** The fundamental building block for your private network in Azure. It enables many types of Azure resources, such as Azure VMs, to securely communicate with each other, the internet, and on-premises networks.
* **Subnet:** A range of IP addresses within your VNet. You can divide a VNet into multiple subnets for organization and security.
* **Network Security Group (NSG):** Acts as a virtual firewall for your VNet. It contains security rules that allow or deny inbound and outbound network traffic to Azure resources (like VMs).
* **Azure Load Balancer:** Distributes incoming network traffic evenly across a group of backend resources or VMs to ensure high availability and reliability.
* **ExpressRoute:** A service that lets you create private connections between Azure datacenters and your on-premises infrastructure. This connection does not go over the public internet.
* **VPN Gateway:** A specific type of virtual network gateway that is used to send encrypted traffic between an Azure virtual network and an on-premises location over the public internet.
* **Application Gateway:** A web traffic load balancer that enables you to manage traffic to your web applications (Layer 7 load balancing).

---

## 💾 4. Storage

* **Storage Account:** An Azure resource that contains all your Azure Storage data objects, including blobs, file shares, queues, tables, and disks. It provides a unique namespace for your Azure Storage data.
* **Blob Storage (Binary Large Object):** Object storage optimized for storing massive amounts of unstructured data, such as text or binary data (images, videos, documents).
* **Managed Disks:** Block-level storage volumes that are managed by Azure and used with Azure Virtual Machines (similar to physical hard drives on an on-prem server).
* **Azure Files:** Fully managed file shares in the cloud that are accessible via standard protocols like Server Message Block (SMB) and Network File System (NFS).
* **LRS / GRS / ZRS:** Redundancy options for storage. 
    * **LRS (Locally Redundant Storage):** Copies data 3 times within a single datacenter.
    * **ZRS (Zone-Redundant Storage):** Copies data across 3 availability zones in a single region.
    * **GRS (Geo-Redundant Storage):** Copies data to a secondary region hundreds of miles away.

---

## 🔐 5. Identity, Governance, and Security

* **Microsoft Entra ID (formerly Azure AD):** Microsoft’s cloud-based identity and access management (IAM) service. It helps your employees sign in and access internal and external resources.
* **Role-Based Access Control (RBAC):** An authorization system built on ARM that provides fine-grained access management to Azure resources. It determines *who* can do *what* to *which* resources.
* **Azure Policy:** A service used to create, assign, and manage policies. These policies enforce different rules and effects over your resources, ensuring those resources stay compliant with corporate standards.
* **Azure Key Vault:** A cloud service for securely storing and accessing secrets (like API keys, passwords, certificates, and cryptographic keys).
* **Microsoft Defender for Cloud:** A unified cloud native application protection platform (CNAPP) that helps prevent, detect, and respond to threats across your multi-cloud and hybrid environments.

---

## 📊 6. Monitoring and Management

* **Azure Monitor:** A comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments.
* **Log Analytics Workspace:** The core of Azure Monitor's log data platform. It is a unique environment for Azure Monitor log data, where you can write queries (using Kusto Query Language, or KQL) to analyze infrastructure telemetry.
* **Azure Advisor:** A personalized cloud consultant that analyzes your resource configuration and usage telemetry, then recommends solutions to help you improve cost effectiveness, performance, reliability, and security.
