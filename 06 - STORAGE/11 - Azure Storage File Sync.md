# Azure Storage - File Sync

[![Azure File Sync](https://img.youtube.com/vi/-mLy7Eo-buA/0.jpg)](https://youtu.be/-mLy7Eo-buA)
*(Click the image above to watch the video)*

## Overview
Azure File Sync allows you to centralize your organization's file shares in Azure Files while keeping the flexibility, performance, and compatibility of an on-premises file server. It transforms your Windows Server into a quick cache of your Azure file share, enabling seamless multi-site synchronization and local access to data using standard industry protocols (SMB, NFS, and FTPS).

## Key Features
* **Cloud Tiering:** Automatically tier infrequently accessed cold data to Azure Files while keeping hot data cached locally on the Windows Server for immediate access.
* **Multi-Site Synchronization:** Keep file servers across different branch offices synchronized in near-real-time by leveraging the cloud as the single source of truth.
* **Backup and Disaster Recovery:** Eliminate the need for complex local backup systems by backing up centralized Azure file shares, complete with native support for volume shadow copies (VSS).
* **Familiar Access Protocols:** Users can access data locally via Windows Explorer or Mac/Linux clients using standard SMB protocols without altering their everyday workflows.

## Common Use Cases
1. **Branch Office Consolidation:** Replace aging on-premises file servers with lightweight local caches while centralizing data management and backups in Azure.
2. **Disaster Recovery and Business Continuity:** Maintain fast, local recovery points at edge locations while utilizing Azure's high availability and geo-redundancy.
3. **Hybrid Cloud Workflows:** Share data seamlessly between cloud-native applications and legacy on-premises systems.

## Core Components
* **Storage Sync Service:** The top-level Azure resource that manages your sync topology, relationships, and policies.
* **Sync Group:** Defines the sync topology for a specific set of files, containing a cloud endpoint (the Azure file share) and one or more server endpoints.
* **Registered Server:** Represents your Windows Server instance that has been linked to the Storage Sync Service.