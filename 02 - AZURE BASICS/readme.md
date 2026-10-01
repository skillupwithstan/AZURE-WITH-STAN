<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/a89793f0-82dd-404d-86b6-bd06aadd916e" />

# ☁️ Microsoft Azure Basics - Cheat Sheet

Welcome to the **Azure Basics** section! This reference guide summarizes essential Microsoft Azure concepts, core benefits, global infrastructure, pricing models, popular services, security best practices, and common CLI commands.

---

## 🖼️ Reference Infographic

<p align="center">
  <img src="https://github.com/user-attachments/assets/a89793f0-82dd-404d-86b0-7d6fbca0b0da" alt="Azure Basics Cheat Sheet" width="100%" />
</p>

---

## 🚀 Quick Navigation
1. [What is Azure?](#1-what-is-azure)
2. [Core Benefits](#2-core-benefits)
3. [Azure Global Infrastructure](#3-azure-global-infrastructure)
4. [Pricing Models & Cost Management](#4-pricing-models--cost-management)
5. [Popular Azure Services (By Category)](#5-popular-azure-services-by-category)
6. [Virtual Machines (VM)](#6-virtual-machines-vm)
7. [Storage Accounts](#7-storage-accounts)
8. [Virtual Network (VNet)](#8-virtual-network-vnet)
9. [Identity & Access Management (IAM)](#9-identity--access-management-iam)
10. [Resource Groups](#10-resource-groups)
11. [Azure CLI - Common Commands](#11-azure-cli---common-commands)
12. [Azure Portal & Tools](#12-azure-portal--tools)
13. [Azure Security Best Practices](#13-azure-security-best-practices)
14. [Basic Architecture Overview](#14-basic-architecture-overview)
15. [Free Tier & Resources](#15-free-tier--resources)

---

### 1. What is Azure?
Microsoft Azure is a cloud computing platform by Microsoft that provides a broad set of global cloud services. It allows you to build, test, deploy, and manage applications and services through Microsoft-managed datacenters.

### 2. Core Benefits
* **High Availability**
* **Scalability & Elasticity**
* **Reliability**
* **Security**
* **Pay-as-you-go** pricing
* **Global reach**
* **Hybrid capability**

### 3. Azure Global Infrastructure
* **Regions:** Geographical areas containing one or more datacenters (e.g., *East US*, *West Europe*, *India Central*). Each region contains paired regions for disaster recovery.
* **Availability Zones (AZ):** Physically separate locations within an Azure region designed to provide high availability.
* **Edge Zones:** Optimized locations for low-latency applications.

### 4. Pricing Model
* **Pay-As-You-Go:** Pay only for the resources you consume.
* **Reserved Instances:** 1 or 3-year commitment for predictable workloads to save on compute costs.
* **Savings Plans:** Flexible pricing models for compute usage.
* **Azure Hybrid Benefit:** Save on Windows Server and SQL Server licenses you already own.

---

### 5. Popular Azure Services (By Category)
* **Compute:** Virtual Machines (VMs), App Service, Functions (Serverless)
* **Storage:** Blob Storage (unstructured), Azure Files, Disk Storage
* **Database:** Azure SQL Database, Cosmos DB (NoSQL), Azure Database for MySQL / PostgreSQL
* **Networking:** Virtual Network (VNet), Load Balancer, Azure DNS
* **Security:** Microsoft Entra ID, Key Vault, Microsoft Defender / Security Center
* **Monitoring:** Azure Monitor, Application Insights, Log Analytics
* **Analytics:** Synapse Analytics, Data Factory, Stream Analytics

---

### 6. Virtual Machines (VM)
* Provides Infrastructure as a Service (IaaS).
* Choose your preferred OS (Windows/Linux), sizing (CPU/RAM), and region.
* **Basic CLI Creation Example:**
  ```bash
  az vm create --resource-group MyResourceGroup \
    --name MyVM --image Ubuntu2204 --admin-username azureuser \
    --generate-ssh-keys
