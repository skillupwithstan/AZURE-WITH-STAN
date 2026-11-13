# Azure Storage - Integration with Microsoft Entra ID

[![Azure Storage Integration with Entra ID](https://img.youtube.com/vi/_iq_XDU0fuA/0.jpg)](https://youtu.be/_iq_XDU0fuA)
*(Click the image above to watch the video)*

## Overview
Integrating Azure Storage with Microsoft Entra ID (formerly Azure Active Directory) enables you to authorize requests to Blob, Queue, and Table storage using Microsoft Entra credentials. By utilizing OAuth 2.0 token credentials, you can rely on Azure role-based access control (Azure RBAC) to grant granular permissions to a security principal, which can be a user, group, application service principal, or managed identity.

## Key Benefits
* **Enhanced Security:** Eliminates the need to use or distribute Shared Key authorization or Shared Access Signatures (SAS), significantly reducing the risk of leaked credentials.
* **Centralized Identity Management:** Manage your storage access policies centrally alongside your other Azure resources. This allows you to enforce organizational security policies, such as Conditional Access and Multi-Factor Authentication (MFA).
* **Granular Access Control (RBAC):** Assign specific data actions to identities. For example, you can grant one team *Storage Blob Data Reader* access and an application *Storage Blob Data Contributor* access scoped down to a specific container.

## Common Authentication Scenarios
1. **Managed Identities for Azure Resources:** Allow Azure services (like VMs, Functions, or App Services) to securely authenticate to the storage account without storing any credentials in code.
2. **Interactive User Access:** Authenticate developers or administrators securely when they use tools like Azure Storage Explorer, the Azure CLI, or the Azure Portal.
3. **Automated Workloads / Service Principals:** Securely authenticate CI/CD pipelines or third-party applications interacting with your storage data.

## Configuration Steps
1. **Navigate to Access Control (IAM):** Go to your storage account (or specific container/queue) in the Azure Portal.
2. **Add Role Assignment:** Select the appropriate data-plane role, such as *Storage Blob Data Contributor* or *Storage Queue Data Reader*.
3. **Assign to Principal:** Select the Microsoft Entra user, group, service principal, or managed identity.
4. **(Optional) Disable Shared Key Access:** To enforce maximum security, configure the storage account to reject requests made with Account Keys, mandating Microsoft Entra ID authentication for all data access.