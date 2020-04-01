---
localization_priority: Normal
description: 'Summary: How organizations in the Office 365 U.S. Government Community Cloud (GCC) can enable Outlook for iOS and Android for their users.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 73b693d9-39bb-4689-a1ff-4be505a5945b
title: Using Outlook for iOS and Android in the Government Community Cloud
ms.collection: 
- exchange-online
- M365-email-calendar
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Using Outlook for iOS and Android in the Government Community Cloud

 **Summary**: How organizations in the Office 365 U.S. Government Community Cloud (GCC) can enable Outlook for iOS and Android for their Exchange Online users.

Outlook for iOS and Android is fully architected in the Microsoft Cloud and meets the security and compliance requirements needs of all United States Government customers when the mailboxes reside in Exchange Online.

For customers with Exchange Online mailboxes operating in the Government Community Cloud (GCC Moderate, GCC High or Department of Defense), Outlook for iOS and Android leverages the [native Microsoft sync technology](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/outlook-for-ios-and-android). This architecture is FedRAMP-compliant (defined by NIST Special Publication 800-145) and approved, and meets GCC High and DoD requirements DISA SRG Level 4 (GCC-High) and Level 5 (DoD), Defense Federal Acquisition Regulations Supplement (DFARS), and International Traffic in Arms Regulations (ITAR), which have been approved by a third-party assessment organization and are FISMA-compliant based on the NIST 800-53 rev 4.

For more information, please see the Office 365 FedRAMP System Security plan located in the FedRAMP Audit Reports section of the [Microsoft Service Trust Portal](https://servicetrust.microsoft.com/).

> [!IMPORTANT]
> Customers operating in the Government Community Cloud may have user mailboxes that also reside on-premises via an Exchange hybrid topology. Accessing on-premises mailboxes with Outlook for iOS and Android does not utilize an architecture that is FedRAMP-compliant. For more information on this architecture, see [Using Basic authentication with Outlook for iOS and Android](https://docs.microsoft.com/Exchange/clients/outlook-for-ios-and-android/use-basic-auth).

This article covers how to:

- Enable Outlook for iOS and Android for Office 365 GCC customers.

- Unlock non-FedRAMP compliant features, if needed.

## Enabling Outlook for iOS and Android for Office 365 GCC customers

GCC (Moderate, High, and Department of Defense) customers can leverage Outlook for iOS and Android without any special configuration.

For Office 365 GCC customers who are not currently using Outlook for iOS and Android, enabling the app requires unblocking Outlook for iOS and Android in the organization, downloading the app on users' devices, and having end users add their account on their devices.

 **1. Unblock Outlook for iOS and Android**

Remove any restrictions placed within your Exchange environment that may be blocking Outlook for iOS and Android by updating your Exchange mobile device access rules or any relevant Azure Active Directory Conditional Access policies so that the app is no longer blocked. See [Securing Outlook for iOS and Android in Exchange Online](secure-outlook-for-ios-and-android.md) for information about enabling Outlook as the only mobile messaging client in an organization.

 **2. Download and install Outlook for iOS and Android**

End users need to install the app on their devices. How the installation happens depends on whether or not the devices are enrolled in a mobile device management (MDM) solution, such as Microsoft Intune. Users with enrolled devices can install the app through their MDM solution, like the Intune Company Portal. Users with devices that are not enrolled in an MDM solution can search for "Microsoft Outlook" in the Apple App Store or Google Play Store and download it from one of those locations.

> [!NOTE]
> To leverage app-based conditional access policies, the Microsoft Authenticator app must be installed on iOS devices. For Android devices, the Intune Company Portal app is leveraged. For more information, see [App-based conditional access with Intune](https://docs.microsoft.com/intune/app-based-conditional-access-intune).

## Disabled services and features

By default, certain services and features of Outlook for iOS and Android are disabled automatically for the Office 365 U.S. Government Community Cloud (GCC) because they do not meet FedRAMP requirements:

- **In-app support**: Users are not able to submit support tickets from within the app. They should contact their internal help desk and provide logs (via the Share Diagnostics Logs option in Setting -> Help). If necessary, the organization's IT department can then contact Microsoft Support directly.

- **In-app feature requests**: Users are not able to submit in-app feature requests. Instead, users are directed to use [Outlook UserVoice](http://outlook.uservoice.com).

- **Multiple accounts**: Only the user's Office 365 GCC account and OneDrive for Business account can be added to a single device. Personal accounts cannot be added. Customers can use another device for personal accounts, or an Exchange ActiveSync client from another provider.

- **Calendar Apps**: Calendar apps (Facebook, Wunderlist, Evernote, Meetup) are not available with GCC accounts.

- **Add-Ins**: Add-ins are not available with GCC accounts.

- **Storage Providers**: Only the GCC account's OneDrive for Business storage account can be added within Outlook for iOS and Android. Third-party storage accounts (for example, Dropbox, Box) cannot be added.

- **Office Lens**: Office Lens technology (for example, scanning business cards and taking pictures) included in Outlook for iOS and Android is not available with GCC accounts.

- **File picker**: The file picker used for adding attachments during email composition is limited to email attachments, iCloud & Device, OneDrive for Business files, and SharePoint sites. The Recent Files list is limited to email attachments.

- **TestFlight**: GCC accounts are not able to access pre-release features when using the TestFlight version of Outlook for iOS.

Executing the below Exchange Online cmdlet enables GCC users using Outlook for iOS and Android access to the above features and services that are not FedRAMP compliant:

```PowerShell
Set-OrganizationConfig -OutlookMobileGCCRestrictionsEnabled $false
```

At any time, access to the above features can be revoked by resetting the parameter back to the default value:

```PowerShell
Set-OrganizationConfig -OutlookMobileGCCRestrictionsEnabled $true
```

Changing this setting typically takes affect within 48 hours. As this setting is a tenant-based change, all Outlook for iOS and Android users in the GCC organization are affected.

For more information on the cmdlet, see [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/organization/set-organizationconfig?view=exchange-ps).

## Services and features not available

Certain services and features of Outlook for iOS and Android are not available for the Office 365 U.S. Government Community Cloud (GCC) because they do not meet FedRAMP requirements:

- **Location services**: Bing location services are not available with GCC accounts. Features that rely on location services, like Cortana Time To Leave, are also unavailable.

- **Favorites**: Favorite folders, groups, and people are not available with GCC accounts.

- **Privacy settings**: Privacy settings cannot be configured through the Office cloud policy service. 

- **Play My Emails**: Play My Emails is not available for GCC accounts. 
