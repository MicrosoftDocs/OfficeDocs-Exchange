---
localization_priority: Normal
description: Learn how to allow or block guest access to Office 365 Groups.
ms.topic: article
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.assetid: a86bb46f-0e5b-43a3-b6ef-7394f344a8da
ms.reviewer: 
f1.keywords:
- NOCSH
title: Allow/Block guest access to Office 365 groups
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
---

# Allow/Block guest access to Office 365 groups

You can allow or block guest users who are using a specific domain. For example, let's say your business (Contoso) has a partnership with another business (Fabrikam). You can add Fabrikam to your Allow list so your users can add those guests to their groups.

Or, let's say you want to block personal email address domains. You can set up a Block list that contains domains like Gmail.com and Outlook.com.

## Important information about how block lists work

- You can create either an Allow list or Block list. **But you can't set up both types of lists**. By default, whatever domains are not in an Allow list are on a Block list, and vice versa.

- You can create only one policy per organization. You can update that policy with more domains, or you can delete that policy to create a new one.

- This list works independently from SPO allow/block list. You would need to set-up Allow/Block list for SPO if you want to restrict individual file sharing of Group connected site.

- This list doesn't apply to already added guest members. This will be enforced for all the guests added after the list is set-up.

## Install the preview version of the Azure Active Directory Module for Windows PowerShell

 **IMPORTANT**: The procedures in this article require the **PREVIEW** version Azure Active Directory Module for Windows PowerShell, specifically, the **AzureADPreview** module version **2.0.0.98** or later.

1. Open Windows PowerShell as an administrator:

   1. In your search bar, type Windows PowerShell.

   2. Right-click on Windows PowerShell and select **Run as Administrator**.

    The Windows PowerShell window will pop open. The prompt C:\Windows\system32 means you opened it as an administrator.

2. Run this command to see if you have any versions of the Azure Active Directory Module for Windows PowerShell installed on your computer:

   ```PowerShell
   Get-Module -ListAvailable AzureAD*
   ```

   - If no results are returned, run this command to install the latest version of the **AzureADPreview** module:

     ```PowerShell
     Install-Module AzureADPreview
     ```

   - If *only* the **AzureAD** module is shown in the results, run these commands to install the **AzureADPreview** module:

     ```PowerShell
     Uninstall-Module AzureAD
     ```

     ```PowerShell
     Install-Module AzureADPreview
     ```

   - If *only* the **AzureADPreview** module is shown in the results, but the version is less than **2.0.0.98**, run these commands to update it:

     ```PowerShell
     Uninstall-Module AzureADPreview
     ```

     ```PowerShell
     Install-Module AzureADPreview
     ```

   - If both the **AzureAD** *and* **AzureADPreview** modules are shown in the results, but the version of the **AzureADPreview** module is less than **2.0.0.98**, run these commands to update it:

     ```PowerShell
     Uninstall-Module AzureAD
     ```

     ```PowerShell
     Uninstall-Module AzureADPreview
     ```

     ```PowerShell
     Install-Module AzureADPreview
     ```

## Create a new Allow or Block list policy

1. Did you install the **AzureADPreview** module as instructioned above? Not having the **preview** version is the #1 reason these steps don't work for people.

2. Go to [Script for Allow/Block policy](https://go.microsoft.com/fwlink/p/?linkid=857710) at Microsoft Download Center to download the script ( **Set-GuestAllowBlockDomainPolicy.ps1**) for Allow/Block policy.

3. Run the script with this command:

   ```PowerShell
   Set-GuestAllowBlockDomainPolicy.ps1 -Update -AllowList @("contoso.com", "fabrikam.com")
   ```

   Where you replace **contoso.com** and **fabrikam.com** with the domains you want to allow.

   OR

   ```PowerShell
   Set-GuestAllowBlockDomainPolicy.ps1 -Update -BlockList @("contoso.com", "fabrikam.com")
   ```

   Remember, you can create only one policy. You'll get an error if you try to create another one.

## Replace the existing policy with a new list of domains

To replace the existing policy with new list of domains, run this command:

```PowerShell
Set-GuestAllowBlockDomainPolicy.ps1 -Update -AllowList @("contoso.com", "fabrikam.com")
```

Where you replace **contoso.com** and **fabrikam.com** with the domains you want to allow.

OR

```PowerShell
Set-GuestAllowBlockDomainPolicy.ps1 -Update -BlockList @("contoso.com", "fabrikam.com")
```

## Add more domains to an existing policy

To append a new domain to the your policy, run this command:

```PowerShell
Set-GuestAllowBlockDomainPolicy.ps1 -Append -AllowList @("contoso.com")
```

Where you replace **contoso.com** and **fabrikam.com** with the domains you want to allow.

OR

```PowerShell
Set-GuestAllowBlockDomainPolicy.ps1 -Append -BlockList @("contoso.com")
```

## Migrate the existing allow/block policy from SharePoint Online

This list works independently from the SharePoint Online allow/block list. You would need to set up allow/block list for SharePoint Online if you want to restrict individual file sharing of Group connected site.

However, if your organization already has an allow/block list for SharePoint Online, you can migrate that list using this command.

1. Install the [SharePoint Online Management tool](https://go.microsoft.com/fwlink/p/?linkid=854002).

2. Run this command:

   ```PowerShell
   Set-GuestAllowBlockDomainPolicy.ps1 -MigrateFromSharepoint
   ```

## Clear the domain list

To remove all the domains from your policy, run this command:

```PowerShell
Set-GuestAllowBlockDomainPolicy.ps1 -Remove
```

## Script for Allow/Block policy

Go to [Script for Allow/Block policy](https://go.microsoft.com/fwlink/p/?linkid=857710) at Microsoft Download Center to download the script (**Set-GuestAllowBlockDomainPolicy.ps1**) for Allow/Block policy.
