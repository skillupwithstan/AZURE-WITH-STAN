# Azure Storage Redundancy

[![Azure Storage Redundancy Tutorial](https://img.youtube.com/vi/Yz4iav4bG5Q/0.jpg)](https://youtu.be/Yz4iav4bG5Q)
*Click the image above to watch the video tutorial.*

---

Azure Storage always stores multiple copies of your data to protect it from transient hardware failures, network or power outages, and natural disasters. Redundancy ensures your storage account meets its availability and durability targets even in the face of failures.

## 1. Redundancy in the Primary Region
Data in an Azure Storage account is always replicated three times in the primary region. 

* **Locally Redundant Storage (LRS):** Copies your data synchronously three times within a single physical cluster in the primary region. It protects against server rack and drive failures. LRS is the lowest-cost option but offers the least durability.
* **Zone-Redundant Storage (ZRS):** Copies your data synchronously across three distinct Azure availability zones in the primary region. Each zone is a separate physical location with independent power, cooling, and networking. ZRS protects against zone-level failures.

## 2. Redundancy in a Secondary Region
For maximum durability and high availability, you can choose to replicate your data to a secondary region that is geographically distant from the primary region (typically hundreds of miles away).

* **Geo-Redundant Storage (GRS):** Replicates your data synchronously three times within a single physical location in the primary region using LRS. It then replicates the data asynchronously to a single physical location in a secondary region (which also keeps three copies via LRS).
* **Geo-Zone-Redundant Storage (GZRS):** Combines the high availability provided by redundancy across availability zones with protection from regional outages provided by geo-replication. Data is replicated synchronously across three Azure availability zones in the primary region (ZRS) and then replicated asynchronously to a secondary region.

## 3. Read Access to the Secondary Region
By default, data in the secondary region (for GRS and GZRS) is not available for read or write access unless a regional failover occurs. To gain read access to the secondary region data during normal operations, you must enable:

* **Read-Access Geo-Redundant Storage (RA-GRS)**
* **Read-Access Geo-Zone-Redundant Storage (RA-GZRS)**

## Summary Comparison

| Redundancy Option | Number of Copies | Primary Region | Secondary Region | Read Access (Secondary) | Best For |
| :--- | :