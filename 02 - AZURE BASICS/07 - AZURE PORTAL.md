```markdown
# 07 - Microsoft Azure Portal

The **[Microsoft Azure Portal](https://portal.azure.com/)** is a unified, web-based graphical user interface (GUI) that allows you to build, manage, and monitor everything from simple web apps to complex cloud deployments. It serves as your central hub for interacting with Azure services, configuring security, and tracking cloud spending.

---

## Methods to Connect and Log In

Accessing Azure is flexible and can be done through various interfaces depending on your role and current task.

### 1. Web Portal (Primary Method)
The most common way to access Azure is through a supported web browser (Edge, Chrome, Firefox, Safari).
* **URL:** Navigate directly to `https://portal.azure.com`.
* **Authentication:** Sign in using your Microsoft account (for personal/free tier) or your organizational credentials (via Microsoft Entra ID). 
* **Multi-Factor Authentication (MFA):** Organizations typically require MFA. You will be prompted to approve the sign-in via the Microsoft Authenticator app, an SMS, or a security key.

### 2. Azure Mobile App
Microsoft provides the **Azure App** for iOS and Android, allowing you to stay connected to your cloud resources on the go.
* **Use Cases:** Checking resource status, monitoring performance metrics, stopping/starting virtual machines, and reviewing alerts.
* **Authentication:** Uses the same Microsoft Entra ID or Microsoft account login as the web portal.

### 3. Command-Line and Scripting (Programmatic Access)
While not the "GUI Portal," administrators frequently log into Azure environments via command-line tools that interact with the same underlying Azure Resource Manager (ARM) API.
* **[Azure CLI](https://learn.microsoft.com/en-us/cli/azure/):** Type `az login` in your terminal. This will pop up a browser window asking you to authenticate.
* **Azure PowerShell:** Run `Connect-AzAccount` to prompt the login sequence.

---

## High-Level Walkthrough of portal.azure.com

When you successfully log into the Azure Portal, you will be presented with a customized interface. Here is a walkthrough of the core components you need to know.

### 1. The Home Page & Dashboards
* **Home Page:** By default, you land on the Home page. It provides quick access to recently visited resources, commonly used services (like Virtual Machines, Storage Accounts, and Entra ID), and quick links to Microsoft Learn and Azure Monitor.
* **Custom Dashboards:** You can switch from the Home view to a custom Dashboard view. Dashboards allow you to pin metrics, resource shortcuts, and charts to create a personalized, single-pane-of-glass view of your environment's health.

### 2. The Global Search Bar (Top Middle)
This is arguably the most powerful tool in the portal. Instead of clicking through menus, use the search bar to instantly find:
* **Resources:** (e.g., typing "Web-VM-01" to find a specific virtual machine).
* **Services:** (e.g., typing "Storage accounts" or "Cost Management").
* **Documentation:** Direct links to official Microsoft documentation regarding your search query.

### 3. Top Right Navigation Bar
The top-right corner houses several critical utility icons:
* **Cloud Shell (`>_`):** Opens an embedded, browser-based command-line interface (Bash or PowerShell) pre-authenticated to your account. It allows you to run scripts without installing local tools.
* **Directories + Subscriptions:** Allows you to switch between different Azure tenants (directories) if your account is a guest or member of multiple organizations.
* **Notifications (Bell Icon):** Displays the real-time status of your actions, such as "Deployment in progress," "Deployment succeeded," or billing alerts.
* **Settings (Gear Icon):** Allows you to customize the portal's appearance, contrast, language, and default startup view (Home vs. Dashboard).

### 4. Left Navigation Pane (The "Hamburger" Menu)
Clicking the three horizontal lines in the top left opens the resource menu. 
* **Favorites:** A customizable list of your most-used services (like Resource Groups, App Services, or SQL Databases).
* **All Services:** A comprehensive catalog of every service offered in Azure, categorized by Compute, Networking, Storage, Web, AI, etc.

### 5. Creating a Resource
Whenever you need to spin up a new service, you will follow a standard flow:
1. Click **+ Create a resource** from the Home page or left menu.
2. You will be taken to the **Azure Marketplace**, where you can search for Microsoft or third-party partner templates (e.g., Windows Server 2022, Ubuntu Web Server, WordPress).
3. Clicking "Create" on a service opens a step-by-step wizard. You will always start by selecting a **Subscription** and a **Resource Group** to house the new service.

### 6. Cost Management + Billing
A crucial stop for any Azure administrator. Searching for "Cost Management" brings you to a dedicated hub where you can:
* View current spending and forecasted monthly costs.
* Set budget alerts (e.g., email me if spending exceeds $500).
* Analyze which specific resources or resource groups are driving up your bill.

```
