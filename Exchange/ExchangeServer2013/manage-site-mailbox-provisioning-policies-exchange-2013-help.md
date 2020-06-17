---
title: 'Manage site mailbox provisioning policies: Exchange 2013 Help'
TOCTitle: Manage site mailbox provisioning policies
ms:assetid: 2f160d1a-a031-461f-8d29-c9cd49ca1645
ms:mtpsurl: https://technet.microsoft.com/library/JJ710340(v=EXCHG.150)
ms:contentKeyID: 49382860
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage site mailbox provisioning policies

_**Applies to:** Exchange Server 2013_

Site mailbox provisioning policies apply only to email that's sent to and from the site mailbox and to the size of the site mailbox on the Exchange server.

To learn more about site mailboxes, see [Site mailboxes](site-mailboxes-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Site mailboxes" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

- Although you can create multiple site mailbox provisioning policies, only the default provisioning policy will be applied to all site mailboxes. You can't apply multiple policies within your organization.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a site mailbox provisioning policy

This example creates the default provisioning policy SM\_ProvisioningPolicy with the following settings:

- The warning quota for the site mailboxes is 9 GB.

- The site mailboxes are prohibited from receiving messages when the mailbox size reaches 10 GB.

- The maximum size of email messages that can be sent to site mailboxes is 50 MB.

```powershell
New-SiteMailboxProvisioningPolicy -Name SM_ProvisioningPolicy -IsDefault -IssueWarningQuota 9GB -ProhibitSendReceiveQuota 10GB -MaxReceiveSize 50MB
```

## View the settings of a site mailbox provisioning policy

This example returns detailed information about all site mailbox provisioning policies in your organization.

```powershell
Get-SiteMailboxProvisioningPolicy | Format-List
```

This example returns all policies in your organization, but only displays the `IsDefault` information to identify which policy is the default policy.

```powershell
Get-SiteMailboxProvisioningPolicy | Format-List IsDefault
```

## Make changes to an existing site mailbox provisioning policy

This example changes the site mailbox provisioning policy named Default to allow the maximum size of email messages that can be received by the site mailbox to 25 MB. (When you install Exchange, a provisioning policy is created with the name **Default**.)

```powershell
Set-SiteMailboxProvisioningPolicy -Identity Default -MaxReceiveSize 25MB
```

This example changes the warning quota to 9.5 GB and the prohibit send and receive quota to 10 GB.

```powershell
Set-SiteMailboxProvisioningPolicy -Identity Default -IssueWarningQuota 9GB -ProhibitSendReceiveQuota 10GB
```

## Configure a site mailbox name prefix

When a new site mailbox is created, by default its email address will have a prefix. The email address prefix allows you to easily search for and query site mailboxes and may help users recognize them as well. If you choose, you can disable the prefix, or change the prefix for your tenant in Office 365 or for a given forest in an on-premises deployment. With the default prefix behavior, if your site mailbox is created in Office 365, the default prefix is **SMO-**. Alternatively, if your site mailbox is created in your on-premises deployment, the prefix is **SM-**. The default behavior differs between these premises so that hybrid customers will not experience conflicts if site mailboxes are created in both locations and are then synced cross-premises.

This example disables the prefix naming by setting the *DefaultAliasPrefixEnabled* parameter to $false.

```powershell
Set-SiteMailboxProvisioningPolicy -Identity Default -DefaultAliasPrefixEnabled $false -AliasPrefix $null
```

This example changes the default provisioning policy and sets the *AliasPrefix* to FOREST01.

> [!NOTE]
> For deployments with multiple forests, it is recommended that a different prefix is used in each forest in order to prevent conflicts when objects are synced across forests, in the event that site mailboxes have been created with the same name in two or more forests.

```powershell
Set-SiteMailboxProvisioningPolicy -Identity Default -AliasPrefix FOREST01 -DefaultAliasPrefixEnabled $false
```

> [!NOTE]
> In the case of a hybrid deployment where you have Exchange on-premises and in Office 365, all cloud-based site mailboxes are created with the prefix <STRONG>SMO-</STRONG>. The prefixes are different in Office 365 and Exchange on-premises so that hybrid customers will not experience conflicts if site mailboxes are created in both locations and are then synced cross-premises.The AliasPrefix parameter takes precedence over the DefaultAliasPrefixEnabled parameter; therefore, if the <EM>AliasPrefix</EM> parameter is set to a valid, non-null string, each new site mailbox will have that string prepended to the alias.

## Delete a site mailbox provisioning policy

This example deletes the default site mailbox policy that was created during Exchange Setup.

```powershell
Remove-SiteMailboxProvisioningPolicy -Identity Default
```

> [!IMPORTANT]
> You must first create and designate another default policy before you can remove the policy named <STRONG>Default</STRONG>.

## For more information

For detailed syntax and parameter information, see the following topics:

[New-SiteMailboxProvisioningPolicy](https://docs.microsoft.com/powershell/module/exchange/New-SiteMailboxProvisioningPolicy)

[Get-SiteMailboxProvisioningPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-SiteMailboxProvisioningPolicy)

[Set-SiteMailboxProvisioningPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-SiteMailboxProvisioningPolicy)

[Remove-SiteMailboxProvisioningPolicy](https://docs.microsoft.com/powershell/module/exchange/Remove-SiteMailboxProvisioningPolicy)
