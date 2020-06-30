---
localization_priority: Normal
description: Admins can learn how to enable mail flow for subdomains in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4033a30a-f506-481c-8ef0-fd9a0508ae38
ms.reviewer: 
title: Enable mail flow for subdomains in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- NOCSH
ms.service: exchange-online
manager: serdars

---

# Enable mail flow for subdomains in Exchange Online

If you have a hybrid environment, with mailboxes hosted both in Exchange Online and on-premises Exchange, and you have subdomains of the accepted domains that only exist in your on-premises environment, you can enable email flow to and from these on-premises subdomains. For example, if you have an accepted domain called Contoso.com, and you enable match subdomains, users can send email to, or receive email from all subdomains of Contoso.com that exist in your on-premises environment, such as marketing.contoso.com and nwregion.contoso.com. In Microsoft Forefront Online Protection for Exchange (FOPE), this feature was called _catch-all domains_.

> [!IMPORTANT]
> - If you have a limited number of subdomains, and know all the subdomain names, we recommend setting up each subdomain as an accepted domain in the Microsoft 365 admin center, instead of using the procedures in this topic. By setting up each subdomain separately, you can have finer control over mail flow and can include unique mail flow rules (also known transport rules) for each subdomain. For more information about adding a domain in the Microsoft 365 admin center, see [Add your domain to Microsoft 365](https://docs.microsoft.com/microsoft-365/admin/setup/add-domain).
>
> - In order to enable match subdomains, an accepted domain must be set up as an internal relay domain. For information about setting the domain type to internal relay, see [Manage accepted domains in Exchange Online](manage-accepted-domains.md).
>
> - After you enable match subdomains, in order for the service to deliver mail for all subdomains to your organization's email server (outside Microsoft 365 or Office 365), you must also change the outbound connector. For instructions, see [Use the EAC to add the domain to your outbound connector](enable-mail-flow-for-subdomains.md#use-the-eac-to-add-the-domain-to-your-outbound-connector).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Domains" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to set up match subdomains on a domain

1. In the EAC, go to **Mail Flow** \> **Accepted domains**, and select the domain.

2. In the Details pane, Verify that **Internal Relay** is selected.

3. Select **Match subdomains for this domain for sending and receiving emails**.

## Use the EAC to add the domain to your outbound connector

1. In the EAC, go to **Mail Flow** \> **Connectors**.

2. Under **Outbound Connectors**, select the connector for your organization's email server, and then select **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. Select **Scope**, and then select one of the following:

   - Select **Route all accepted domains through this connector**.

   - In the **Recipient domains** section, select **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.png). In the **Add domain** box, enter a wildcard domain entry for the domain for which you enabled match subdomains. For example, if you enabled match subdomains for contoso.com, enter \*.contoso.com as a recipient domain.

> [!NOTE]
> If you don't yet have an outbound connector, see [Configure mail flow using connectors](../../mail-flow-best-practices/use-connectors-to-configure-mail-flow/use-connectors-to-configure-mail-flow.md).

## Use Exchange Online PowerShell to set up match subdomains on a domain

To add match subdomains to a domain that is set up as an internal relay, use this syntax:

```powershell
Set-AcceptedDomain -Identity <Domain Name> -MatchSubdomains $true
```

This example sets up match subdomains for the contoso.com domain.

```powershell
Set-AcceptedDomain -Identity contoso.com -MatchSubdomains $true
```

For detailed syntax and parameter information, see [Set-AcceptedDomain](https://docs.microsoft.com/powershell/module/exchange/set-accepteddomain).

#### How do you know this worked?

To verify that you've successfully added match subdomains to a domain using Exchange Online PowerShell, run the following command to verify the _MatchSubdomains_ property value:

```powershell
Get-AcceptedDomain | Format-List Name,MatchSubdomains
```
