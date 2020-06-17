---
title: 'Add the Federation Mailbox to the AD RMS Super Users Group: Exchange 2013 Help'
TOCTitle: Add the Federation Mailbox to the AD RMS Super Users Group
ms:assetid: 44618df9-54f0-4474-a450-dcba48a02901
ms:mtpsurl: https://technet.microsoft.com/library/Ee424431(v=EXCHG.150)
ms:contentKeyID: 49319909
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add the Federation Mailbox to the AD RMS Super Users Group

_**Applies to:** Exchange Server 2013_

For the following Microsoft Exchange Server 2013 Information Rights Management (IRM) features to be enabled, you must add the Federation mailbox (a system mailbox created by Exchange 2013 Setup) to the super users group on your organization's [Active Directory Rights Management Services (AD RMS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)) cluster:

- IRM in Microsoft Office Outlook Web App

- IRM in Exchange ActiveSync

- Journal report decryption

- Transport decryption

You can configure a mail-enabled distribution group as a super users group in AD RMS. Members of the distribution group are granted an owner use license when they request a license from the AD RMS cluster. This allows them to decrypt all RMS-protected content published by that cluster. Whether you use an existing distribution group or create a distribution group and configure it as the super users group in AD RMS, we recommend that you dedicate the distribution group for this purpose and configure the appropriate settings to approve, audit, and monitor membership changes.

> [!WARNING]
> Configuring a super users group in AD RMS allows group members to decrypt IRM-protected content. We recommend that you take adequate measures to control and monitor group membership and enable auditing to track membership changes.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution groups" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- An AD RMS cluster must be deployed in the Active Directory forest.

- If a super users group is already configured on an AD RMS cluster, any modifications to the distribution group membership can take up to 24 hours to be refreshed by the AD RMS cluster. This is a result of caching the group membership on the cluster.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Use the Shell to add the Federation mailbox to a distribution group

If a distribution group has been created and configured as a super users group in the AD RMS cluster, you can add the Exchange 2013 Federation mailbox as a member of that group. If a super users group isn't configured, you must create a distribution group and add the Federation mailbox as a member.

1. Create a distribution group dedicated for use as an AD RMS super users group. For details, see [Create and manage distribution groups](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups).

2. Add the user **FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042** to the new distribution group. The Federation mailbox is a system mailbox, and therefore not visible in the EAC. To add it to a distribution group, you must use the [Add-DistributionGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-DistributionGroupMember) cmdlet from the Shell.

   This example adds the Federation mailbox to the ADRMSSuperUsers distribution group.

   ```powershell
   Add-DistributionGroupMember ADRMSSuperUsers -Member FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042
   ```

For detailed syntax and parameter information, see [Add-DistributionGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-DistributionGroupMember).

## Step 2: Use AD RMS to set up a super users group

Perform the following procedure on an AD RMS cluster. The account used to perform this procedure must be a member of the AD RMS Enterprise Administrators local group on the AD RMS server.

1. Open the Active Directory Rights Management Services console and expand the AD RMS cluster.

2. In the console tree, expand **Security Policies**, and then click **Super Users**.

3. In the action pane, click **Enable Super Users**.

4. In the result pane, click **Change Super User Group** to open the **Super Users** property sheet.

5. In the **Super user group** box, type the email address of the distribution group you created in the previous procedure, or click **Browse** to select a distribution group.

## How do you know this worked?

After you have added the Federation mailbox to a new or existing distribution group, use the [Get-DistributionGroupMember](https://docs.microsoft.com/powershell/module/exchange/Get-DistributionGroupMember) cmdlet to check the membership of the group.

For an example of how to check distribution group membership, see [Example 1](https://docs.microsoft.com/powershell/module/exchange/Get-DistributionGroupMember#examples) in **Get-DistributionGroupMember**.

After you have used AD RMS to set up a super users group, you can use the following methods to verify that the super users group has been configured correctly. Additionally, you can use [Test-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Test-IRMConfiguration) cmdlet to verify IRM functionality.

- Use the AD RMS console to verify that the correct group has been configured as the super users group.

- Run the following PowerShell command on an AD RMS server to retrieve the super users group.

  > [!IMPORTANT]
  > The ADRMSAdmin PowerShell module is available in Windows Server 2008 R2 and later.

  ```powershell
  Import-Module ADRMSAdmin
  New-PSDrive -Name MyRmsAdmin -PsProvider AdRmsAdmin -Root https://localhost
  Get-ItemProperty -Path MyRmsAdmin:\SecurityPolicy\SuperUser
  ```
