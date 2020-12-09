---
title: "Configure Exchange to support delegated mailbox permissions in a hybrid deployment"
ms.author: dmaguire
author: msdmaguire
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- M365-email-calendar
ms.assetid: a2a10cb3-4557-4ff5-8191-c653522f4512
ms.reviewer:
description: "Delegated mailbox permissions enable someone to manage some part of another user's mailbox. A common example of this is an administrative assistant who needs to manage an executive's mailbox and calendar. Hybrid deployments between an on-premises Exchange organization and Microsoft 365 or Office 365 support the Full Access and Send on Behalf delegated mailbox permissions. However, depending on the version of Exchange you have installed in your on-premises organization, you might need to perform additional configuration to use delegated mailbox permissions in a hybrid deployment. The following lists the versions of Exchange that support delegated mailbox permissions in a hybrid deployment and whether additional configuration is needed for that version."
---

# Configure Exchange to support delegated mailbox permissions in a hybrid deployment

Delegated mailbox permissions enable someone to manage some part of another user's mailbox. A common example of this is an administrative assistant who needs to manage an executive's mailbox and calendar. Hybrid deployments between an on-premises Exchange organization and Microsoft 365 or Office 365 support the **Full Access** and **Send on Behalf** delegated mailbox permissions. However, depending on the version of Exchange you have installed in your on-premises organization, you might need to perform additional configuration to use delegated mailbox permissions in a hybrid deployment. The following lists the versions of Exchange that support delegated mailbox permissions in a hybrid deployment and whether additional configuration is needed for that version.

- **Exchange 2016**: Additional configuration is required.

- **Exchange 2013**: A supported Exchange 2013 cumulative update (CU) and additional configuration are required.

- **Exchange 2010**: Not supported anymore.

For more information about the specific requirements to support delegated mailbox permissions in a hybrid deployment, take a look at [Permissions in Exchange hybrid deployments](../permissions.md).

The following sections step you through the configuration of Exchange 2013 and Exchange 2016 on-premises deployments to enable support for delegated mailbox permissions. Before you follow these steps, you need to make sure you're on the latest Exchange 2013 CU or Exchange SP3 RU. For more information, see [Hybrid deployment prerequisites](../hybrid-deployment-prerequisites.md).

## Exchange 2013 And Exchange 2016

What you need to do to enable support for delegated mailbox permissions depends on a few factors. If you moved mailboxes to Microsoft 365 or Office 365 and at that time:

|**The following was installed...**|**And ACLable object synchronization at the organization was...**|**Then you need to...**|
|:-----|:-----|:-----|
|Exchange 2013 CU9 or earlier|This feature isn't available in Exchange 2013 CU9 and earlier.|Manually configure each mailbox to support ACLs|
|Exchange 2013 CU10 or later|Disabled|Enable ACLable object synchronization at the organization level <br/> Manually enable ACLs on each mailbox moved to Microsoft 365 or Office 365 before ACLable object synchronization was enabled at the organization level. <br/> No additional configuration is needed for mailboxes moved to Microsoft 365 or Office 365 after ACLable object synchronization is enabled at the organization level.|
|Exchange 2013 CU10 or later|Enabled|No additional configuration is needed|
|Exchange 2016|Disabled|Enable ACLable object synchronization at the organization level <br/> Manually enable ACLs on each mailbox moved to Microsoft 365 or Office 365 before ACLable object synchronization was enabled at the organization level. <br/> No additional configuration is needed for mailboxes moved to Microsoft 365 or Office 365 after ACLable object synchronization is enabled at the organization level.|
|Exchange 2016|Enabled|No additional configuration is needed|

### Enable ACLable object synchronization

To enable ACLable object synchronization at the organization level, do the following.

1. Install the latest version of Azure Active Directory Connect (AAD Connect) on all of your AAD Connect servers. This is needed to allow AAD Connect to synchronize the attributes needed to support hybrid permissions. You can download AAD Connect from [Microsoft Azure Active Directory Connect](https://www.microsoft.com/download/details.aspx?id=47594).

2. Open the Exchange Management Shell on an Exchange 2013 or Exchange 2016 server running the latest available CU, or the immediately previous CU.

3. Run the following command.

   ```PowerShell
   Set-OrganizationConfig -ACLableSyncedObjectEnabled $True
   ```

After you do this, any mailboxes that you move to Microsoft 365 or Office 365 will be properly configured to support delegated mailbox permissions. If mailboxes were moved to Microsoft 365 or Office 365 prior to you completing these steps, you'll need to manually enable ACLs on those mailboxes using the steps in [Enable ACLs on remote mailboxes](#enable-acls-on-remote-mailboxes).

> [!IMPORTANT]
> ACLs aren't enabled on remote mailboxes created in Microsoft 365 or Office 365. If you create a remote mailbox in Microsoft 365 or Office 365, you'll need to follow the steps in the Enable ACLs on remote mailboxes section to enable ACLs on that remote mailbox. To avoid this extra step, we recommend that you create the mailbox on an on-premises Exchange server and then move the mailbox to Microsoft 365 or Office 365.

### Enable ACLs on remote mailboxes

To enable ACLs on mailboxes moved to Microsoft 365 or Office 365 before ACLable object synchronization was enabled at the organization level, do the following.

1. Open the Exchange Management Shell on an Exchange 2013 or Exchange 2016 server running the latest available CU, or the immediately previous CU.

2. To enable ACLs on a single mailbox, run the following command:

   ```PowerShell
   Get-AdUser <UserMailbox's Identity> | Set-AdObject -Replace @{msExchRecipientDisplayType=-1073741818}
   ```

3. To enable ACLs on all mailboxes moved to Microsoft 365 or Office 365, run the following command:

   ```PowerShell
   Get-RemoteMailbox -ResultSize unlimited | where {$_.RecipientTypeDetails -eq "RemoteUserMailbox"} | foreach {Get-AdUser -Identity $_.Guid | Set-ADObject -Replace @{msExchRecipientDisplayType=-1073741818}}
   ```

4. To verify that the mailboxes have been successfully updated, run the following command:

   ```PowerShell
   Get-RemoteMailbox -ResultSize unlimited | ForEach {Get-AdUser -Identity $_.Guid -Properties msExchRecipientDisplayType | Format-Table DistinguishedName,msExchRecipientDisplayType -Auto}
   ```

> [!IMPORTANT]
> The msExchRecipientDisplayType value -1073741818 should only be set for user mailboxes, not for resource mailboxes.


