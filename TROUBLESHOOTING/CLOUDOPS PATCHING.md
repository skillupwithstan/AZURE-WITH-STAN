# CloudOps Patching: Azure Update Management Troubleshooting

Patching is a critical component of Cloud Operations (CloudOps), falling under both Security and Change Management. In Azure, patching is primarily orchestrated through **Azure Update Management** (part of Azure Automation Accounts) or the newer **Azure Update Manager**. 

When a Virtual Machine fails to patch or shows as "Disconnected" / "Not Assessed," the issue usually lies with the local monitoring agents, network connectivity to Azure fabric, or OS-level configurations. This guide outlines the standard troubleshooting approach.

---

[![Troubleshooting Series - Azure - CloudOps Patching](https://img.youtube.com/vi/sZT7IhPdqhU/hqdefault.jpg)](https://youtu.be/sZT7IhPdqhU)

----

## 1. Verify Platform Level Requirements

Before diving into the OS, verify that the Azure subscription and workspace are properly configured.

*   **Resource Providers:** Go to your **Subscription > Resource providers**. Ensure that `Microsoft.Automation` and `Microsoft.OperationalInsights` are registered. If they are stuck, you can try re-registering them.
*   **Workspace Linkage:** Verify that your Azure Automation Account is correctly linked to the Log Analytics Workspace where the VMs are reporting.

## 2. Troubleshooting "Disconnected" or "Not Assessed" VMs

If a VM appears in the Update Management portal but its status is "Disconnected" or "Not Assessed," the management agent on the VM is not communicating properly with Azure.

### For Linux VMs (OMS Agent / Log Analytics Agent)
1.  **Check Agent Status:** Use the Serial Console or SSH to log in and check if the OMS agent is running:
    ```bash
    sudo /opt/microsoft/omsagent/bin/service_control status
    ```
2.  **Restart the Agent:** If it is stopped or hung, restart it:
    ```bash
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```
3.  **Run the Troubleshooter Tool:** Microsoft provides a built-in troubleshooter script for the OMS agent:
    ```bash
    sudo /opt/microsoft/omsagent/bin/troubleshooter
    ```
    This interactive tool will check network connectivity, agent health, and heartbeat status.

### For Windows VMs (Microsoft Monitoring Agent - MMA)
1.  **Check Service Status:** Use `Run Command` in the Azure portal or log in via RDP and check the Microsoft Monitoring Agent (`HealthService`).
    ```cmd
    sc query healthservice
    ```
2.  **Run the Connection Test Tool:** In the agent installation directory, there is a tool to test connectivity to Azure Log Analytics:
    ```cmd
    cd "C:\Program Files\Microsoft Monitoring Agent\Agent"
    TestCloudConnection.exe
    ```
    This tool will verify if the VM can successfully reach the required `.ods.opinsights.azure.com` and `.oms.opinsights.azure.com` endpoints.
3.  **Clear the Agent Cache:** Sometimes the agent gets corrupted state data. Stop the service, delete the contents of `C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State`, and restart the service.

---

## 3. Network and Proxy Issues

Both Windows and Linux agents require outbound internet access (port 443) to communicate with the Azure Automation service and Log Analytics.

*   **Required URLs:** Ensure your Network Security Group (NSG), Azure Firewall, or third-party proxy allows outbound traffic to:
    *   `*.ods.opinsights.azure.com`
    *   `*.oms.opinsights.azure.com`
    *   `*.automation.ext.azure.com`
*   If your VM uses an internal proxy, ensure the agent configuration explicitly defines the proxy settings.

---

## 4. OS-Level Conflicts and Configurations

Sometimes the Azure fabric is perfectly healthy, but the OS itself is preventing the patch cycle from executing.

*   **Group Policy Objects (GPO) Conflicts (Windows):** If your VM is joined to an Active Directory domain, a domain GPO might be overriding local Windows Update policies. For Azure Update Management to work effectively, the VM's Windows Update settings should generally be set to manual or disabled for automatic updates, allowing Azure Automation to trigger the orchestrator. If a GPO enforces WSUS (Windows Server Update Services) or SCCM, it will conflict with Azure Update Management.
*   **Sysprep Issues:** If a VM was cloned or created from a generalized image but the `sysprep` process failed or did not generate a new unique SID, Azure Update Management might confuse multiple VMs as the same machine, leading to "Duplicate Node" registration errors.
*   **WSUS/Repository Issues:** 
    *   *Windows:* Ensure the VM can reach the Windows Update catalog or your internal WSUS server. 
    *   *Linux:* Ensure the VM can reach the public package repositories (e.g., `archive.ubuntu.com`, `repo.almalinux.org`) or your internal repository mirror. If `apt-get update` or `yum update` fails locally, Azure Update Management will also fail.

---

## 5. RBAC and Permissions

If you are a CloudOps engineer attempting to trigger a patch deployment and it fails immediately or the option is greyed out:

*   Verify you have the correct **Role-Based Access Control (RBAC)** permissions. You need at least `Automation Operator` or `Contributor` rights on the Automation Account and `Virtual Machine Contributor
