# Autopilot-App-Registration

This project provides a PowerShell GUI tool that automates the creation of a Microsoft Entra app registration for Windows Autopilot.

## What the code does

The `AutopilotApp_GUI` file is a PowerShell script with a Windows Forms interface and performs the following workflow:

1. **Launches a GUI**
   - Opens a small desktop window with a connection status label, Login/Logout buttons, a button to create the Autopilot app, and a progress bar.

2. **Signs in to Microsoft Graph**
   - The **Login** button triggers `Connect-MgGraph` with these scopes:
     - `Application.ReadWrite.All`
     - `AppRoleAssignment.ReadWrite.All`
     - `Directory.Read.All`
   - After successful login, it displays account, tenant ID, and organization details.

3. **Ensures required modules are available**
   - Installs the NuGet package provider if needed.
   - Installs `Microsoft.Graph` and `ps2exe` if they are not already available.
   - Imports the Graph submodules needed for app and directory management.

4. **Creates an app registration**
   - Checks whether an app named `AutopilotInfoApp1` already exists.
   - If it does not exist, creates a new app registration in Entra.
   - Creates a client secret (`PowershellCreds1`) for the app.
   - Creates a corresponding service principal.

5. **Configures Graph application permissions**
   - Assigns the Microsoft Graph application role:
     - `DeviceManagementServiceConfig.ReadWrite.All`
   - Shows a message reminding the operator to grant **Admin Consent** manually in the Azure portal.

6. **Generates local Autopilot scripts**
   - Creates `C:\Autopilot_Scripts` if it does not already exist.
   - Generates two PowerShell scripts containing Tenant ID, App ID, and App Secret:
     - `Autopilot_<Company>_GPO_v1.ps1` (with a predefined group tag)
     - `Autopilot_<Company>_v1.ps1` (prompts the user for a group tag)
   - Both scripts install the required Autopilot tooling and run `Get-WindowsAutoPilotInfo` in online upload mode.

7. **Builds an EXE**
   - Converts `Autopilot_<Company>_v1.ps1` into an executable `.exe` file using `ps2exe`.

8. **Supports logout/reset**
   - The **Logout** button disconnects from Microsoft Graph and resets the GUI state.

## Outcome

With this tool, an admin can quickly:
- create the required app registration for Autopilot,
- assign the needed Graph application permission,
- and generate ready-to-run upload scripts (including an EXE variant) for client-side usage.
