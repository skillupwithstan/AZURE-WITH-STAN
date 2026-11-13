# Azure Storage - Replication - Account and Object Level

Azure Storage offers robust replication options to ensure your data is highly available, durable, and protected against transient hardware failures, network outages, or massive natural disasters. Replication can be configured at the account level for overall redundancy, or at the object level for specific blob synchronization.

## Account-Level Replication
When you create a storage account, you select a redundancy option that applies to all data in that account:
* **Locally Redundant Storage (LRS):** Copies your data synchronously three times within a single physical location in the primary region.
* **Zone-Redundant Storage (ZRS):** Copies your data synchronously across three Azure availability zones in the primary region.
* **Geo-Redundant Storage (GRS):** Copies your data synchronously three times within a single physical location (LRS), then copies it asynchronously to a single physical location in a secondary region.
* **Geo-Zone-Redundant Storage (GZRS):** Combines the high availability of ZRS with protection from regional outages by copying data synchronously across three zones, then asynchronously to a secondary region.

## Object-Level Replication
Object replication asynchronously copies individual block blobs from a source storage account to a destination storage account. 
* **Use Cases:** Minimizing latency for local users, optimizing compute distribution, and adhering to data sovereignty policies by selectively moving data.
* **Requirements:** Both the source and destination accounts must have Blob versioning and Change feed enabled.

## Video Tutorial

Click the image below to watch the full tutorial on Azure Storage Account and Object Replication:

[![Azure Storage Replication Tutorial](https://img.youtube.com/vi/heNXnV285uM/maxresdefault.jpg)](https://youtu.be/heNXnV285uM)