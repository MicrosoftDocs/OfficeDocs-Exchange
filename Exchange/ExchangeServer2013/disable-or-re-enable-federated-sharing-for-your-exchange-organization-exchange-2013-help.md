---
title: 'Disable or re-enable federated sharing for your Exchange organization'
TOCTitle: Disable or Re-enable federated sharing for your Exchange organization
ms:assetid: d36490d8-0268-47b9-a6d4-e56427f1b02e
ms:mtpsurl: https://technet.microsoft.com/library/JJ657497(v=EXCHG.150)
ms:contentKeyID: 49289421
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disable or Re-enable federated sharing for your Exchange organization

_**Applies to:** Exchange Server 2013_

There may be situations when you need to temporarily disable federated sharing for your organization. Instead of deleting the existing federation trust or deleting organization relationships and sharing policies that you may need in the future, you can simply disable the organization identifier (OrgID) for the federation trust.

> [!WARNING]
> For hybrid deployments with Office&nbsp;365, disabling the federation trust for your on-premises servers will also disable hybrid features such as shared calendar free/busy information, MailTips, and message tracking. However, secure mail transport won't be disabled in the hybrid deployment if the federation trust for the on-premises organization is disabled.

To learn more about federation trusts, see [Federation](federation-exchange-2013-help.md). To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md).

For additional management tasks related to federated sharing, see [Federation procedures](federation-procedures-exchange-2013-help.md).

> [!IMPORTANT]
> This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see <A href="https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china">Learn about Office 365 operated by 21Vianet</A>.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the *Federation and certificates* permissions entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- Any existing organization relationships and sharing policies for other federated Exchange organizations won't be modified and won't be functional. Sharing policies that are configured to provide Internet recipients with access to calendar information won't be affected.

- You can't use the Exchange admin center (EAC) to disable or enable the OrgID for a federation trust. You must use the Shell.

## Use the Shell to disable or re-enable federated sharing

This example disables the OrgID and disables federation and federated sharing for the Exchange organization.

```powershell
Set-FederatedOrganizationIdentifier -Enabled $false
```

This example enables the OrgID and re-enables federation and federated sharing for the Exchange organization.

```powershell
Set-FederatedOrganizationIdentifier -Enabled $true
```

For detailed syntax and parameter information, see [Set-FederatedOrganizationIdentifier](https://docs.microsoft.com/powershell/module/exchange/Set-FederatedOrganizationIdentifier).

## How do you know this worked?

Successful completion of the **Set-OrganizationIdentifier** cmdlet will be the first indication that the OrgID has been disabled or enabled.

To further verify success, run the following Shell command and verify the value returned for the *Enabled* parameter

```powershell
Get-FederatedOrganizationIdentifier
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
