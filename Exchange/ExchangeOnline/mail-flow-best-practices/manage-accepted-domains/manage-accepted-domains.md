---
localization_priority: Normal
description: Admins can learn how to view and modify accepted domains in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 0fc0ecc0-e133-48fa-9d72-cb4793a73960
ms.reviewer: 
title: Manage accepted domains in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage accepted domains in Exchange Online

When you add your domain to Microsoft 365 or Office 365, it's called an accepted domain. This means that users in this domain can send and receive mail. For more information on how to add your domain to Microsoft 365 or Office 365 using the Microsoft 365 admin center, see [Add a domain to Microsoft 365 or Office 365](https://docs.microsoft.com/microsoft-365/admin/setup/add-domain).

After you add your domain using the Microsoft 365 admin center, you can use the Exchange admin center (EAC) to view your accepted domains and configure the domain type.

There are two types of accepted domains in Exchange Online:

- **Authoritative**: Email is delivered to email addresses that are listed for recipients in Microsoft 365 or Office 365 for this domain. Emails for unknown recipients are rejected.

  - If you just added your domain to Microsoft 365 or Office 365 and you select this option, it's critical that you add your recipients to Microsoft 365 or Office 365 before setting up mail to flow through the service.

  - Typically, you use this option when all the email recipients in your domain are using Microsoft 365 or Office 365. You can also use it if some recipients exist on your own email servers. However, if recipients exist on your own email servers, you must add your recipients to this Microsoft 365 or Office 365 domain in order to make sure that mail is delivered as expected. For more information about how to manage your recipients, see these topics:

    - **Exchange Online**: [Manage mail users](../../recipients-in-exchange-online/manage-mail-users.md)

    - **Exchange Online Protection**: [Manage Mail Users in EOP](https://docs.microsoft.com/microsoft-365/security/office-365-security/manage-mail-users-in-eopx)

  - Setting this option enables Directory Based Edge Blocking (DBEB), which rejects messages for invalid recipients at the service network perimeter. For more information about configuring DBEB during a migration, see [Use Directory Based Edge Blocking to reject messages sent to invalid recipients](../../mail-flow-best-practices/use-directory-based-edge-blocking.md).

- **Internal relay (also known as non-authoritative**): Recipients for this domain can be in Microsoft 365 or Office 365 or your own email servers. Email is delivered to known recipients in Office 365 or is relayed to your own email server if the recipients aren't known to Microsoft 365 or Office 365.

  - **You should not select this option if all of the recipients for this domain are in Microsoft 365 or Office 365.**

  - If you select this option, you must create a connector for mail flow from Microsoft 365 or Office 365 to your on-premises email server; otherwise recipients on the domain who are not hosted in Microsoft 365 or Office 365 won't be able to receive mail on your own email servers. For more information about setting up connectors, see [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](../../mail-flow-best-practices/use-connectors-to-configure-mail-flow/set-up-connectors-to-route-mail.md).

  - This option is required if you enable the subdomain routing option on a domain in order to let email pass through the service and be delivered to any subdomains of your accepted domains. For more information, see [Enable mail flow for subdomains in Exchange Online](enable-mail-flow-for-subdomains.md).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Domains" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## View accepted domains

### Use the EAC to view accepted domains

1. In the EAC, go to **Mail flow** \> **Accepted domains**.

2. Click the **Name**, **Accepted Domain**, or **Domain Type** column heading to sort alphabetically in ascending or descending order. By default, accepted domains are sorted alphabetically by name in ascending order.

### Use Exchange Online PowerShell to view accepted domains

To view summary information about all accepted domains, run the following command:

```powershell
Get-AcceptedDomain
```

To view details about a specific accepted domain, use the following syntax.

```powershell
Get-AcceptedDomain -Identity <Name> | Format-List
```

This example shows details about the accepted domain named contoso.com.

```powershell
Get-AcceptedDomain -Identity contoso.com | Format-List
```

## Configure the domain type

After you add a domain to your Exchange Online organization in the Microsoft 365 admin center, you can configure the domain type.

### Use the EAC to change the domain type

1. In the EAC, go to **Mail flow** \> **Accepted domains**.

2. Select the domain and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. In the **Accepted Domain** window, in the **This accepted domain is** section, select the domain type. The possible values are **Authoritative** and **Internal relay**.

   - If you select **Authoritative**, you must confirm that you want to enable Directory Based Edge Blocking.

   - If you select **Internal Relay**, you can enable match subdomains to enable mail flow to all subdomains. For more information, see [Enable mail flow for subdomains in Exchange Online](enable-mail-flow-for-subdomains.md).

4. When you're finished, click **Save**.

### Use Exchange Online PowerShell to change the domain type

To configure the domain type, use the following syntax:

```powershell
Set-AcceptedDomain -Identity <Name> -DomainType <Authoritative | InternalRelay>
```

This example configures the accepted domain named contoso.com as an internal relay domain.

```powershell
Set-AcceptedDomain -Identity contoso.com -DomainType InternalRelay
```

For detailed syntax and parameter information, see [Set-AcceptedDomain](https://docs.microsoft.com/powershell/module/exchange/set-accepteddomain).

### How do you know this worked?

To verify that you've successfully configured the domain type, do either of the following steps:

- In the EAC at **Mail flow** \> **Accepted domains**, click **Refresh** ![Refresh Icon](../../media/ITPro_EAC_RefreshIcon.png). In the list of accepted domains, verify the domain type value of the accepted domain is configured correctly.

- In Exchange Online PowerShell, run the command `Get-AcceptedDomain`. In the list of accepted domains, verify the domain type value of the accepted domain is configured correctly.
