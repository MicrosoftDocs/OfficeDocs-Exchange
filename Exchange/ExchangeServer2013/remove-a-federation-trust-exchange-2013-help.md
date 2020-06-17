---
title: 'Remove a federation trust: Exchange 2013 Help'
TOCTitle: Remove a federation trust
ms:assetid: dc4d126d-b567-470d-a5d0-e1402bf8f369
ms:mtpsurl: https://technet.microsoft.com/library/JJ657500(v=EXCHG.150)
ms:contentKeyID: 49289432
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a federation trust

_**Applies to:** Exchange Server 2013_

A federation trust establishes a trust relationship between a Microsoft Exchange 2013 organization and the Azure Active Directory authentication system and supports sharing with other federated Exchange organizations. Removing a federation trust from your on-premises Exchange organization will disable federated sharing with other federated Exchange organizations and with Microsoft 365 or Office 365 organizations connected to your organization as part of a hybrid deployment. You should carefully consider the overall impact to your organization before removing a federation trust.

For additional management tasks related to federation trusts, see [Federation procedures](federation-procedures-exchange-2013-help.md).

> [!IMPORTANT]
> This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see <A href="https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china">Learn about Office 365 operated by 21Vianet</A>.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Federation and certificates" permissions entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- After removing the federation trust, you can remove the TXT records from your public DNS server for each federated domain. Review the requirements for removing a TXT record with the organization that hosts your public DNS records.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to remove a federation trust

1. On an Exchange 2013 server in your on-premises organization, navigate to **organization** \> **sharing**.

2. In the **Federation Trust** section, click **Remove**.

3. In the warning, click **yes** to confirm that you want to remove the federation trust.

4. After the federation trust is removed, click **Close**.

## Use the Shell to remove a federation trust

This example removes the federation trust.

```powershell
Remove-FederationTrust
```

For detailed syntax and parameter information, see [Remove-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Remove-FederationTrust).

## How do you know this worked?

To verify that you have successfully removed the federation trust, do one of the following:

- In the EAC, navigate to **organization** \> **sharing**. If you successfully removed the federation trust, only the **Enable** button will be available under **Federation Trust**.

- In the Shell, run the following command to verify that federation trust information isn't returned for your Exchange organization.

  ```powershell
  Get-FederationTrust
  ```

  For detailed syntax and parameter information, see [Get-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Get-FederationTrust).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
