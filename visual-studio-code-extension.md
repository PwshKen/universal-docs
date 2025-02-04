---
description: Learn about the Visual Studio Code extension for Universal.
---

# Visual Studio Code Extension

PowerShell Universal can be managed with the PowerShell Universal Visual Studio Code extension. It allows you to connect to a local Universal instance and manage APIs, dashboards and scripts. 

## Installation

You can download the extension from the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ironmansoftware.powershell-universal). You can also download the extension from within the Visual Studio Code extension pane. Search for PowerShell Universal and click Install.  

![](.gitbook/assets/image%20%28117%29.png)

## Configuration

The extension will prompt you for the URL and App Token used to connect to your PowerShell Universal instance. Follow the instructions within the extension when it starts up. 

You can also configure the extension by navigating to your PowerShell Universal admin console and then logging in. Navigate to Settings \ Configurations and click Edit with VS Code. A new app token will be generated and the URL will be set automatically. 

![](.gitbook/assets/image%20%28287%29.png)

## Features

The PowerShell Universal extension adds a new activity pane panel for PowerShell Universal. It has the following sections. 

### APIs

You can manage APIs with the extension. You will see a list of APIs. You can click the `Open endpoints.ps1` button to view the endpoints file and add new endpoints. Clicking the refresh button will reload any endpoints you add. You can click the `Insert Invoke-RestMethod to Console` to add a call to the endpoint to the PowerShell Integrated Console. 

![](.gitbook/assets/image%20%28113%29.png)

### Dashboards

You can manage dashboards with the extension. You will see a list of dashboards underneath this section. You can open the dashboards.ps1 script, open the a single dashboard's script, restart a dashboard and view dashboards. When you open a dashboard script, the dashboard modules will automatically be loaded so that IntelliSense works in VS Code. 

![](.gitbook/assets/image%20%28118%29.png)

#### Debug Dashboard Process

{% hint style="info" %}
Debugging a dashboard process only works when running PowerShell Universal locally and as the same user you are logged in as. 
{% endhint %}

You can connect the Visual Studio debugger to the dashboard process by right clicking on the dashboard and click Debug Dashboard Process. This requires the PowerShell extension for Visual Studio Code. 

![](.gitbook/assets/image%20%28130%29.png)

After connecting the debugger, you can run commands such as `Get-Runspace` and `Debug-Runspace` to begin debugging aspects of your dashboard. 

#### View Dashboard Logs

You can view dashboard logs by right clicking on the dashboard and clicking View Logs. They will open in a new tab.

![](.gitbook/assets/image%20%28121%29.png)

### Scripts

You can manage scripts with the extension. You will see a list of available scripts underneath this section. You can edit the scripts.ps1, edit an individual script and run scripts. When running scripts, you will receive feedback about the status of the script. Scripts with parameters are not supported in VS Code. You can still run them in PowerShell Universal. 

![](.gitbook/assets/image%20%28115%29.png)

## Sample Browser

The sample browser can be used to insert samples from the PowerShell Universal Sample Repository into your PowerShell Universal instance. Just save the files it updates and your PowerShell Universal system will reflect the changes. 

{% embed url="https://youtu.be/HkCVe1PUX0Q" %}

## Settings

Settings can be found within the Visual Studio Code Settings dialog. You can open the settings dialog by press `Ctrl+,`

PowerShell Universal settings can be found under Extensions \ PowerShell Universal. 

### App Token

The App Token is used for communicating with the PowerShell Universal management API. You can create an App Token manually by navigating to Security \ Tokens within the PowerShell Universal admin console. 

You can also automatically configure an App Token by click Edit with VS Code within Settings \ Configurations. 

### Check Modules

The extension will check the version of the Universal and UnviersalDashboard PowerShell modules found on the system. If they aren't installed or are not the latest version, new versions will be installed. 

### Local Editing

Local editing causes files to be edited locally rather than via the management API. It will open files from the repository folder on disk and edit those files. PowerShell Universal will listen to file changes and reload those files accordingly. 

When local editing is disabled, the contents of the files will be sent to the management API. This is required for remote systems or on systems where PowerShell Universal is not running as the same user as the on you are logged in as. 

### Samples Directory

This is the directory to store the PowerShell Universal samples. 

### Start Server 

This setting causes the extension to start an instance of PowerShell Universal when it is activated. If you already have PowerShell Universal in some other way, you should disable this setting.

### Server Path

This is the path to the executable that will be started by the Start Server setting.  

### Sync Samples

Whether to download samples from GitHub so they are available within the samples browser. 

### Url

The URL of the PowerShell Universal service. This should be the root URL, such as `http://localhost:5000`. This will be set automatically when clicking Edit in VS Code. 





