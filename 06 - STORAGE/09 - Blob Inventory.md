# Azure Storage - Blob Inventory

[![Azure Storage Blob Inventory](https://img.youtube.com/vi/heNXnV285uM/0.jpg)](https://youtu.be/heNXnV285uM)
*(Click the image above to watch the video)*

## Overview
Azure Storage Blob Inventory provides a machine-readable overview of your containers, blobs, snapshots, and blob versions within a storage account. When managing petabytes of data or millions of blobs, this tool helps you understand total data size, age, encryption status, and other critical properties at scale.

## Key Features
* **Automated Reports:** Schedule inventory reports to run on a daily or weekly basis.
* **Flexible Output Formats:** Generate reports in either CSV or Apache Parquet formats.
* **Customizable Rules:** Define specific rules and prefix filters to control exactly which blobs or containers are included in your inventory report.
* **Schema Fields:** Include metadata such as creation time, last modified time, access tier, content length, and more.

## Common Use Cases
1. **Data Lifecycle Management:** Identify old or rarely accessed data that should be transitioned to cooler storage tiers (Cool or Archive) or deleted.
2. **Cost Optimization:** Analyze storage consumption patterns to better forecast and optimize Azure billing.
3. **Security and Auditing:** Verify encryption statuses, immutability policies, and legal holds across large datasets to meet compliance requirements.

## How it Works
To use this feature, you enable Blob Inventory on your storage account and define an **Inventory Policy**. This policy contains rules that dictate the schedule, output format, and filtering criteria. Once configured, Azure Storage automatically generates the inventory files and saves them to a designated container within your account.