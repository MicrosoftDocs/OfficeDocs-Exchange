---
localization_priority: Normal
description: 'Summary: Learn how to create, modify, and remove accepted domains in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 11801f73-4934-4025-a1c1-3935dada7e9b
ms.reviewer:
title: Procedures for accepted domains in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Procedures for accepted domains in Exchange Server

Accepted domains are the SMTP name spaces (also known as address spaces) that you configure in an Exchange organization to receive email messages. You use the Exchange admin center (EAC) or the Exchange Management Shell to configure accepted domains in Exchange Server.

For more information about accepted domains, see [Accepted domains in Exchange Server](accepted-domains.md). The types of accepted domains are summarized in the following list:

- **Authoritative domains**

  - All recipients in the authoritative domain exist in the Exchange organization.

  - Exchange is responsible for generating non-delivery reports (also known as NDRs or bounce messages) for non-existent recipients in an authoritative domain.

- **Internal relay domains**

  - Some recipients in the internal relay domain might exist in the Exchange organization.

  - Exchange isn't responsible for generating NDRs for non-existent recipients in an internal relay domain. Instead, you create a Send connector with the address space of the internal relay domain. You source this Send connector on an internal Mailbox server to relay messages for the non-existent recipients in the domain.

- **External relay domains**

  - None of the recipients in the external relay domain exist in the Exchange organization.

  - Exchange isn't responsible for generating NDRs for non-existent recipients in an external relay domain. Instead, you create a Send connector with the address space of the external relay domain. You source this Send connector on an Edge Transport server or Internet-facing Mailbox server to relay messages for all the recipients in the domain.

## What do you need to know before you begin?

- Estimated time to complete each task: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Accepted domains" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic and the "Email address policies" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- If you have a subscribed Edge Transport server in your perimeter network, you configure accepted domains on a Mailbox server in your Exchange organization. The accepted domains configuration is replicated to the Edge Transport server during EdgeSync synchronization. For more information, see [Edge Subscriptions](../../architecture/edge-transport-servers/edge-subscriptions.md).

- If Exchange accepts mail for recipients in an accepted domain from the Internet, you need to configure an MX record for the domain in your Internet-facing (public) DNS servers. Each MX record should resolve to the Internet-facing server that receives email for your organization.

- You need to create a Send connector to route mail for non-existent recipients in internal or external relay domains. For more information, see [Create a Send connector to route outbound mail through a smart host](../../mail-flow/connectors/outbound-smart-host-routing.md).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create accepted domains

After you create an accepted domain, you can't change the domain value (for example, from contoso.com to \*.contoso.com, or vice-versa).

### Use the EAC to create accepted domains

1. In the EAC, go to **Mail flow** \> **Accepted domains**, and then click **Add** (![Edit icon](../../media/ITPro_EAC_AddIcon.png)).

2. In the **New accepted domain** window that opens, configure the following settings:

   - **Name**: Enter a unique, descriptive name.

   - **Accepted domain**: Enter a single domain (for example, contoso.com) or a domain with subdomains (for example, \*.contoso.com).

   - **This domain is**: Select **Authoritative**, **Internal Relay**, or **External Relay**.

    When you're finished, click **Save**.

### Use the Exchange Management Shell to create accepted domains

To create an accepted domain, use the following syntax:

```PowerShell
New-AcceptedDomain -Name <Name> -DomainName <DomainOrDomainWithSubdomains> -DomainType <Authoritative | InternalRelay | ExternalRelay>
```

This example creates a new authoritative domain named Contoso Corp for contoso.com.

```PowerShell
New-AcceptedDomain -Name "Contoso Corp" -DomainName contoso.com
```

**Note**: We didn't need to use the _DomainType_ parameter, because the default value is `Authoritative`.

For detailed syntax and parameter information, see [New-AcceptedDomain](https://docs.microsoft.com/powershell/module/exchange/new-accepteddomain).

### How do you know this worked?

To verify that you've successfully created an accepted domain, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Accepted domains**, verify that the accepted domain is listed, and the details are correct.

- In the Exchange Management Shell, run the following command to verify the property values:

  ```PowerShell
  Get-AcceptedDomain | Format-Table -Auto Name,DomainName,DomainType,Default,AddressBookEnabled
  ```

## Modify accepted domains

- You can only *replace* the default domain with a new default domain (one accepted domain is always configured as the default domain). For more information about the default domain, see [Default domain](accepted-domains.md#default-domain).

- You can enable and disable Recipient Lookup for an accepted domain only in the Exchange Management Shell. For more information, see [Recipient Lookup in accepted domains](accepted-domains.md#recipient-lookup-in-accepted-domains).

### Use the EAC to modify accepted domains

1. In the EAC, go to **Mail flow** \> **Accepted domains**, select the accepted domain from the list, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

2. In the properties window that opens, you can configure the following settings:

   - **Name**

   - **This domain is**: Authoritative, Internal Relay, or External Relay.

   - **Make this the default domain**: If the check box is cleared, select it to configure the accepted domain as the default domain.

    When you're finished, click **Save**.

### Use the Exchange Management Shell to modify accepted domains

To modify an accepted domain, use the following syntax:

```PowerShell
Set-AcceptedDomain -Identity <AcceptedDomainIdentity> [-Name <Name>]  [-DomainType <Authoritative | InternalRelay | ExternalRelay>] [-AddressBookEnabled <$true | $false>] [-MakeDefault $true]
```

This example configures the authoritative domain named Contoso Corp as the default domain.

```PowerShell
Set-AcceptedDomain -Identity "Contoso Corp" -MakeDefault $true
```

This example enables Recipient Lookup on a Edge Transport server for the internal relay domain named Fabrikam Corp. All external recipients in the fabrikam.com domain are represented in Exchange as mail users.

```PowerShell
Set-AcceptedDomain -Identity "Fabrikam Corp" -AddressBookEnabled $true
```

For detailed syntax and parameter information, see [Set-AcceptedDomain](https://docs.microsoft.com/powershell/module/exchange/set-accepteddomain).

### How do you know this worked?

To verify that you've successfully modified an accepted domain, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Accepted domains**, and verify the property values.

  **Notes**:

  - To verify that the accepted domain is the default domain, you need to select the accepted domain from the list, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)). If **Make this the default domain** is selected, it's the default domain.

  - You can't use the EAC to verify that Recipient Lookup is enabled or disabled for the accepted domain. You need to use the Exchange Management Shell.

- In the Exchange Management Shell, run the following command to verify the property values:

  ```PowerShell
  Get-AcceptedDomain | Format-Table -Auto Name,DomainName,DomainType,Default,AddressBookEnabled
  ```

## Remove accepted domains

- You can't remove the default domain. First, you need to configure another accepted domain as the default domain.

- You can't remove an accepted domain that's defined anywhere in an email address policy (including in the disabled email address templates). To see all the domains that are used in email address policies, run the following command in the Exchange Management Shell:

  ```PowerShell
  Get-EmailAddressPolicy | Format-List Name,*EmailAddressTemplate*
  ```

  For more information about modifying email address policies, see [Modify email address policies](../../email-addresses-and-address-books/email-address-policies/eap-procedures.md#modify-email-address-policies)[Modify email address policies]).

### Use the EAC to remove accepted domains

1. In the EAC, go to **Mail flow** \> **Accepted domains**, select the accepted domain from the list, and then click **Remove** (![Remove icon](../../media/ITPro_EAC_RemoveIcon.png)).

2. In the **Warning** dialog that appears, click **Yes** to confirm.

### Use the Exchange Management Shell to remove accepted domains

To remove an accepted domain, use the following syntax:

```PowerShell
Remove-AcceptedDomain -Identity <AcceptedDomainIdentity>
```

This example removes the accepted domain named Fabrikam Corp.

```PowerShell
Remove-AcceptedDomain -Identity "Fabrikam Corp"
```

For detailed syntax and parameter information, see [remove-AcceptedDomain](https://docs.microsoft.com/powershell/module/exchange/remove-accepteddomain).

### How do you know this worked?

To verify that you've successfully removed an accepted domain, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Accepted domains**, and verify that the accepted domain is no longer listed.

- In the Exchange Management Shell, run the following command to verify that the accepted domain isn't listed:

  ```PowerShell
  Get-AcceptedDomain
  ```

## Configure Exchange to accept mail for multiple authoritative domains

These are some scenarios that require multiple authoritative domains:

- Your organization is changing its SMTP domain name.

- You want to provision different email addresses for different business units within your organization.

- You provide email hosting services, and you need to accept email for more than one SMTP domain.

After you configure one or more authoritative domains, you need to decide how to use those domains in your organization. For example:

- Do you want to replace the existing primary (**Reply-To:**) address for the recipients, or add the new email address as a proxy address?

- Do you want to keep old email addresses as a proxy addresses so the recipients continue to receive mail that's sent to their old email addresses?

- Do you want the email addresses in the new authoritative domain to apply to all recipients and all types of recipients, or only to specific types of recipients or specific recipients based on their user properties (for example, only users in the Finance department)?

These are the steps that are required to configure Exchange to accept mail for multiple authoritative domains:

1. Create one or more authoritative domains as described in the [Create accepted domains](#create-accepted-domains) section.

    For example, if you already have contoso.com configured as an authoritative domain, add fourthcoffee.com as an authoritative domain.

2. Create or modify an email address policy that uses the authoritative domains to meet your requirements. For example:

   - Modify the default email address policy so all recipients get the required primary and proxy email addresses.

    For example, modify the default policy so _\<alias\>_@fourthcoffee.com is the primary SMTP email address, and _\<alias\>_@contoso.com is kept as a proxy address. For instructions, see [Modify email address policies](../../email-addresses-and-address-books/email-address-policies/eap-procedures.md#modify-email-address-policies).

   - Create a new email address policy that applies the required primary and proxy email addresses to a filtered set of recipients.

    For example, create a new policy named Fourth Coffee Recipients with the following settings:

   - **Precanned recipient filter**: All users with mailboxes where the **Company** value is Fourth Coffee.

   - **Primary SMTP email address**: _\<alias\>_@fourthcoffee.com.

   - **Additional proxy email addresses**: None. The affected recipients can no longer receive messages sent to their old @contoso.com primary email address.

   - **Priority**: 1. The first email address policy that identifies a recipient configures the recipient's email addresses. All other policies are ignored, even if the first policy is unapplied and can't configure the recipient's email addresses.

     For instructions, see [Create email address policies](../../email-addresses-and-address-books/email-address-policies/eap-procedures.md#create-email-address-policies).

3. Apply the new or updated email address policy to the affected recipients. For instructions, see [Apply email address policies to recipients](../../email-addresses-and-address-books/email-address-policies/eap-procedures.md#apply-email-address-policies-to-recipients).

To verify that you've configured Exchange to accept mail for multiple authoritative domains, perform the following procedures:

1. Send test messages to an affected recipient from a mailbox that's outside of your Exchange organization, and verify the email addresses that accept or reject mail.

2. Send test messages from an affected mailbox to an external recipient, and verify the From address of the message.
