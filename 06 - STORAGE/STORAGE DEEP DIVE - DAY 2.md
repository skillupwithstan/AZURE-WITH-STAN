# Azure Storage Deep Dive - Day 2

## Video Reference
[![Azure Storage - Day 2](https://img.youtube.com/vi/p8GrfysH8cg/maxresdefault.jpg)](https://youtu.be/p8GrfysH8cg)
*(Click the thumbnail above to watch the tutorial)*

---

## 1. Blob Storage Access Tiers
Azure offers different access tiers to store blob data in the most cost-effective manner based on how frequently it is accessed.

* **Hot Tier:** Optimized for storing data that is accessed or modified frequently. Highest storage costs, but lowest access costs.
* **Cool Tier:** Optimized for storing data that is infrequently accessed and stored for at least 30 days. Lower storage costs and higher access costs compared to the hot tier.
* **Cold Tier:** Optimized for storing data that is rarely accessed and stored for at least 90 days. 
* **Archive Tier:** Optimized for storing data that is rarely accessed and stored for at least 180 days with flexible latency requirements (retrieval can take hours). Lowest storage costs, but highest data retrieval costs.

## 2. Lifecycle Management
Azure Storage lifecycle management offers rule-based policies that you can use to transition blob data to the appropriate access tiers or expire data at the end of its lifecycle.

* **Automate Tiering:** Move blobs from Hot to Cool, or Cool to Archive automatically based on the last modified date or creation date.
* **Data Deletion:** Automatically delete old versions of blobs, snapshots, or base blobs to save on storage costs and ensure compliance.

## 3. Storage Security and Access Control
Controlling who can access your storage account and what they can do is a critical security pillar. 

* **Shared Keys (Access Keys):** Every storage account has two 512-bit access keys. These provide full administrative, root-level access to the storage account and should be protected carefully.
* **Shared Access Signatures (SAS):** A secure URI that grants restricted, delegated access to Azure Storage resources. You can define the exact resource, permissions (read, write, delete), and the precise time window the signature is valid.
* **Microsoft Entra ID (formerly Azure AD):** Uses Role-Based Access Control (RBAC) to assign specific permissions to users, groups, or managed identities. This is the recommended approach for the highest security.

## 4. Storage Management Tools
To interact with and transfer data to your Azure Storage accounts, Microsoft provides several robust tools:

* **Azure Storage Explorer:** A free, standalone desktop application that allows you to visually manage your Azure Storage data on Windows, macOS, and Linux.
* **AzCopy:** A powerful command-line utility used to copy blobs or files to or from a storage account with high performance and reliability.