---
categories: Windows Server 2022
image: https://mylemans.online/assets/img/posts/Default.jpg
layout: post
tags: hyper-v virtualisation home
title: Enable Hyper-V on Windows 10/11 Home Edition!
---


# Exciting Update: Enable Hyper-V on Windows 10/11 Home Edition!

Welcome back to our blog! As you know, we've been exploring various ways to enhance your home lab experience using Hyper-V, which typically requires a Windows Pro or Server environment. Today, I'm thrilled to share a workaround that enables Hyper-V on Windows 10/11 Home editions!

### Discovering the Workaround

A script developed by the Microsoft Virtualization team can activate Hyper-V on systems running Home editions of Windows 10 or Windows 11. While this script is an unofficial method and not guaranteed to work in perpetuity (especially through system updates), it presents a valuable opportunity for home users to engage with virtualization technology without the need for an immediate system upgrade.

### How to Check if Virtualization is Enabled

Before proceeding, ensure your system supports virtualization:
1. Open `Command Prompt`.
2. Type `systeminfo` and press Enter.
3. Look for `Virtualization Enabled in Firmware`. If it says `No`, you'll need to enable this in your BIOS settings.

### Running the Hyper-V Enabling Script

To enable Hyper-V using the script:
1. **Create a Batch File**: Paste the provided script into a text file and save it as `Enable-Hyper-V.bat`.
2. **Run as Administrator**: Right-click the batch file and select 'Run as Administrator'. This is crucial, as the script requires elevated privileges to make changes to your system.

```
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V -All /LimitAccess /ALL
pause
```

This script will install necessary components and enable Hyper-V on your machine. Please make sure to restart your computer once the script completes its execution.

### Activating Hyper-V Features

Once the script has successfully run, the next step is to enable the Hyper-V features. This can be done using PowerShell to ensure that all necessary features are activated without needing to restart your computer immediately.

#### Using PowerShell to Enable Hyper-V

To enable Hyper-V via PowerShell, follow these steps:

1. **Open PowerShell as Administrator**: Right-click on the Start button, select "Windows PowerShell (Admin)" or "Command Prompt (Admin)" if PowerShell isn't available.
2. **Run the Activation Command**: Copy and paste the following command into PowerShell and press Enter:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All -NoRestart
```

This command enables all necessary Hyper-V features but omits the automatic restart (-NoRestart), allowing you to manually reboot at a more convenient time.

3. Check the Installation: You can verify that Hyper-V is enabled by typing Get-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V -Online in PowerShell and checking that the status is "Enabled".
4. Reboot Your Computer: To complete the setup and start using Hyper-V, manually restart your computer. This ensures that all changes are applied and Hyper-V can run smoothly.

### Final Steps

After your system restarts, Hyper-V will be fully operational. You can access the Hyper-V Manager from the Start Menu to begin creating and managing virtual machines. For those new to virtualization, the Hyper-V Manager provides a user-friendly interface to configure your virtual environments.

### Recommended Hardware

For those starting with Hyper-V:
- **RAM**: At least 8 GB.
- **Storage**: An SSD for better performance.

### Moving Forward

For long-term and stable use of Hyper-V, upgrading from Windows Home to Pro is advisable, as it provides official support and stability for Hyper-V.

For more detailed instructions on enabling Hyper-V using the script and other virtualization tips, check out the [original guide on The Windows Club](https://www.thewindowsclub.com/how-to-install-and-enable-hyper-v-on-windows-10-home) and [CloudBytes](https://cloudbytes.dev/snippets/enable-hyper-v-on-windows-1011-home).

After enabling Hyper-V and setting up your environment, you might be interested in taking it a step further by creating Hyper-V templates, which can significantly streamline the management of multiple virtual machines. Here's how you can proceed:

### Exploring Hyper-V Templates
Once you've successfully activated Hyper-V on your system, you can begin to explore more advanced features like creating and using virtual machine templates. Templates in Hyper-V can save you a lot of time if you frequently deploy virtual machines with similar configurations.

#### Getting Started with Hyper-V Templates
If you're ready to dive into creating Hyper-V templates, you can follow my detailed guide on this topic. This guide will walk you through the process of setting up server templates, which can be a great way to get familiar with more complex Hyper-V functionalities.

You can find the guide here: [Creating Hyper-V Virtual Machine Templates](https://mylemans.online/posts/Server2022-Hyper-V-VirtualMachineTemplates/).

This guide is designed to help you understand the steps involved in creating a template, from preparing the virtual machine to converting it into a template, which you can then use to deploy new servers quickly and efficiently.

By following these instructions, you'll be able to enhance your virtualization skills and optimize your workflow when managing virtual environments. Whether you're setting up a lab for testing or a production environment, using Hyper-V templates can be incredibly beneficial.
