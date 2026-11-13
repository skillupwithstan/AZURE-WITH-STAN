# Azure Storage - Authentication and Authorization options

[![Azure Storage Authentication and Authorization Options](https://img.youtube.com/vi/WcHjoEzBD5s/0.jpg)](https://youtu.be/WcHjoEzBD5s)
*(Click the image above to watch the video)*

## Overview
Azure Storage provides a robust, multi-layered security model to ensure that your data is accessed only by authorized clients and applications. Depending on your workload requirements, you can choose from several authentication and authorization mechanisms to secure Blob, Queue, Table, and File storage.

## Core Authorization Methods
1. **Microsoft Entra ID (Recommended)**
   * **What it is:** Modern, token-based identity and access management using OAuth 2.0.
   * **Best For:** Enterprise applications, web apps, managed services, and human users. 
   * **Key Advantage:** Eliminates long-lived shared secrets. Combined with **Azure RBAC (Role-Based Access Control)**, you can assign fine-grained permissions down to the container or prefix level.

2. **Account Access Keys (Shared Keys)**
   * **What it is:** Grants full, unrestricted access to all services and resources within a storage account using the account's primary or secondary secret key.
   * **Best For:** Administrative setups, legacy applications, or scenarios where full root-level control is required.
   * **Key Consideration:** Because these keys carry total administrative privileges, they pose security risks if leaked, and Microsoft recommends disabling Shared Key access in favor of Microsoft Entra ID wherever possible.

3. **Shared Access Signatures (SAS)**
   * **What it is:** A URI that grants delegated access to storage resources with specified restrictions (e.g., time expiry, specific permissions like read/write, and allowed IP ranges).
   * **Best For:** Granting third-party clients or transient applications limited, time-bound access to specific objects without revealing your account keys.
   * **Types:** User Delegation SAS (secured by Entra ID), Account SAS, and Service SAS.

4. **Access Control Lists (ACLs) for Azure Data Lake Storage (ADLS Gen2)**
   * **What it is:** POSIX-compliant permissions layered on top of Azure RBAC.
   * **Best For:** Big data analytics workloads (such as HDInsight, Databricks, or Synapse) requiring deep directory and file-level security down to individual files and folders.

## Summary Checklist for Best Practices
* **Enforce Microsoft Entra ID:** Make Entra ID your default choice for all applications and interactive users.
* **Minimize Shared Keys:** Restrict or disable account key access to prevent credential compromise.
* **Scope SAS Tokens:** Keep SAS token lifetimes as short as possible and use User Delegation SAS when feasible.
* **Network Restrictions:** Pair your authorization strategy with Virtual Network (VNet) service endpoints, private endpoints, and IP firewalls for defense-in-depth.