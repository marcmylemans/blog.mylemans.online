---
image: https://mylemans.online/assets/img/posts/Default.jpg
layout: post
title: "Step-by-Step Guide - Connecting Local Active Directory to Microsoft Entra ID"
date: 2024-11-12
categories: [Cloud, Identity Management]
tags: [microsoft entra, azure ad, cloud sync, single sign-on, sso, active directory, identity, hybrid cloud]
---

In today's tech-driven workplaces, managing user identities across various platforms can be challenging. Connecting your local Active Directory to Microsoft Entra ID (formerly Azure Active Directory) simplifies user management, enhances security, and improves the sign-on experience. This guide walks you through the entire setup process, from adding a custom domain to enabling single sign-on.

## Why Integrate Active Directory with Microsoft Entra ID?

By connecting Active Directory to Microsoft Entra ID, you can:
- Streamline user management and provisioning
- Improve security by centralizing identity management
- Enable self-service password reset for users
- Offer single sign-on (SSO) for smoother access to applications

Let’s dive in!



### Step 1: Accessing the Admin Portal

Head to [admin.microsoft.com](https://admin.microsoft.com) and log in with your global administrator account. This is your starting point for setting up the integration.

### Step 2: Adding a Custom Domain

In the **Setup** section, start the Custom Domain Setup wizard. Adding a custom domain makes your organization’s identity more recognizable. Enter your domain name, then verify ownership by adding a TXT record in your DNS management console. Once the record is added, return to the wizard and click **Verify**.

### Step 3: Adding DNS Records for Services

With your domain verified, configure DNS records for services like Exchange and Teams. Follow the wizard’s instructions to set up the necessary DNS records. Note that DNS changes may take time to propagate.

### Step 4: Setting Up User Synchronization

Go to **Setup** and choose **Add or sync users to Microsoft Entra ID** to start syncing users from your local Active Directory. This ensures that users don’t need separate accounts for local and cloud environments.

### Step 5: Choosing a Synchronization Method

You can choose between **Entra Cloud Sync** and **Microsoft Entra Connect Sync**. For simplicity and resilience, we’ll use **Cloud Sync**. Select it and proceed with setup.

### Step 6: Installing the Provisioning Agent

Download the provisioning agent. For a small lab environment, installing it on a Domain Controller is acceptable, but for production, follow best practices by using a separate member server. The provisioning agent keeps user data synced between Active Directory and Entra ID.

If you encounter an Internet Explorer error during setup, disable **IE Enhanced Security Configuration** in Server Manager.

### Step 7: Configuring the Service Account

Enter domain administrator credentials for the provisioning agent. This account allows the agent to sync data from Active Directory to Entra ID.

### Step 8: Managing Cloud Sync Settings

In the setup wizard, click **Manage Microsoft Entra Cloud Sync** to configure sync settings. Ensure **Password Hash Sync** is enabled to keep passwords consistent across systems.

### Step 9: Setting Up Scoping Filters

By default, all users are synced. You can limit this to specific Organizational Units (OUs) by setting scoping filters. Copy the OU’s distinguished name from Active Directory and paste it into the filter settings to sync only selected users.

### Step 10: Preparing User Accounts for Sync

Ensure each user account has the correct **Email** and **ProxyAddresses** attributes. For example, mark the primary email with `SMTP` in uppercase. If your public domain differs from the local UPN, add it as an **Alternative UPN Suffix**.

### Step 11: Testing the Synchronization

Test the setup by syncing a single user. If you encounter a `JoinNotFound` error, check that the user is in the correct OU and try again. This confirms the sync is functioning properly.

### Step 12: Creating a Security Group for Licensing

Create a security group (e.g., `EntraID_Licensing_BusinessPremium`) for license management. Adding users to this group will automatically assign them the necessary Microsoft 365 licenses.

### Step 13: Enabling the Sync Configuration

Enable cloud sync according to your settings. Initial synchronization may take some time depending on directory size, so be patient. Check the audit logs to monitor progress.

### Step 14: Assigning Licenses to the Group

In the Microsoft 365 admin center, go to **Billing > Licenses**, select a license, switch to the **Groups** tab, and assign licenses to the security group. This ensures that all users in the group receive the appropriate licenses automatically.

### Step 15: Enabling Self-Service Password Reset (SSPR)

Allow users to reset their own passwords by navigating to **entra.microsoft.com** and enabling **Password Reset** for the licensing group. Users will be prompted to set up multi-factor authentication (MFA) and provide a mobile number.

### Step 16: Enabling Single Sign-On (SSO) with Cloud Sync

To enhance the user experience, enable SSO with Cloud Sync. This lets users access multiple services without needing to re-authenticate.

#### Preparing for SSO Configuration

Download **Microsoft Entra Connect**, then cancel the installation after extracting the files. Open PowerShell, import the Azure AD SSO module, and configure SSO for your Active Directory forest.

#### Configuring SSO via PowerShell

Use the following commands in PowerShell:
- `Import-Module .\AzureADSSO.psd1`
- `New-AzureADSSOAuthenticationContext`
- `Enable-AzureADSSOForest`
- `Enable-AzureADSSO -Enable $true`

This sets up SSO for your domain.

#### Verifying SSO Configuration

Check that a new computer account (`AZUREADSSOACC`) has been created in Active Directory. This account enables SSO functionality.

#### Configuring Group Policy for SSO

In the Group Policy Management Console, configure policies to assign Microsoft’s autologon URL to the Intranet zone. Link the GPO to the OU containing your synced users to complete the setup.

### Optional: Setting Up Cloud Kerberos Trust

For enhanced integration, consider using Cloud Sync with Cloud Kerberos Trust. This additional setup allows for seamless authentication across environments.


## Final Recap

Congratulations! You’ve successfully:
- Set up a custom domain
- Synced users between Active Directory and Entra ID
- Configured automated license assignment
- Enabled self-service password reset
- Implemented single sign-on (SSO)

By following these steps, you’ve optimized your organization’s identity management, providing a smoother, more secure experience for users. If you have questions or encounter any issues, feel free to reach out or leave a comment below.

Thank you for following along, and happy syncing!
