---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3bce1321-0bff-40dc-92e1-52c5b955cdf5
description: "Admins can lear about cross-premises email with an on-premises email system other than Exchange and Exchange Online."
title: Plan to coexist with a third-party messaging system using Active Directory Domain Services
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- MBS150
- BCS160
ms.audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars
---

# Plan for third-party email coexistence with Microsoft 365 and Azure Active Directory

Most Microsoft email migration information assumes that you're running Exchange Server in your on-premises organization. This topic is for organizations that use Active Directory as their on-premises identity platform and a third-party messaging system (for example, IBM Lotus Notes or Novell GroupWise) for email.

In this scenario, the goal is to support cross-premises email coexistence. A third-party messaging system remains in the on-premises organization and shares an email namespace (domain) with the Exchange Online messaging system in the cloud. A unified address book in the cloud shows all users in both the on-premises and cloud organizations. This email coexistence might be a short-term or long-term solution.

As you plan for this third-party email coexistence, consider the Azure Active Directory [hybrid identity options](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity) and the [authentication choices](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) for synchronization and end user authentication options.

**Scenario goals**:

- Users with on-premises mailboxes should be represented in the Exchange Online global address list (GAL) as mail-enabled users.

- Mail routing from the cloud to the on-premises organization uses a shared domain namespace.

  Or, as part of a migration strategy, the mail-enabled users in the cloud might be licensed with Exchange Online mailboxes.

- Cross-premises coexistence might last indefinitely. The cloud address list, proper mail routing, and message format fidelity all meet business-class requirements

**Requirements**:

- A subscription to Microsoft 365 (must be an enterprise subscription).

- The on-premises organization is running Active Directory with the Microsoft Exchange 2016 or later schema updates.

- The Exchange Management Shell and the Exchange Server Active Directory schema are required for managing email-related users. To meet these requirements, install the Exchange 2016 Mailbox server role on a server in the on-premises organization.

- Every recipient object from the third-party system needs to have corresponding user object in local Active Directory. The users will need mail-enabled as part of the coexistence process.

## Technical Overview

To enable any cross-premises messaging scenario, you need to determine how you will route email between the on-premises organization and the cloud. From an implementation perspective, the choice comes down to where inbound mail goes first: to the on-premises messaging system or to the cloud. The one you choose depends on the goals of the cross-premises deployment.

### Shared namespace configuration

Generally speaking, if you plan to move all your messaging to the cloud (employing a cross-premises deployment as part of a longer-term mail migration strategy), configuring your mail exchanger (MX) record to direct inbound email to the cloud first is a logical choice. In this configuration, you can take advantage of Exchange Online Protection (EOP), for all inbound email to your organization.

Otherwise, if your long-term goal is to maintain a cross-premises messaging environment indefinitely, and you are not interested in Exchange Online Protection, you can choose to leave the MX record as it is currently configured.

### Email routing in a cross-premises environment

Regardless of where your inbound mail enters the cross-premises deployment, mail-routing requires that users with mailboxes in your on premise messaging system are represented by mail-enabled users in the cloud messaging system. The mail-enabled user object in the cloud directory is has the target SMTP address of the corresponding recipient mailbox in the on-premises organization.

The process of synchronizing mail-enabled users with the correct target address requires installing the [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-design-concepts) tool in your on-premises Active Directory. The Azure Active Directory Connect tool synchronizes the on-premises mail-enabled user in the Active Directory with a target address value that matches the shared namespace and need to be a verified domain in Microsoft 365.

For example, if you've verified the domain in your Microsoft 365 deployment (for example, domino.contoso.com), the Azure Active Directory Connect tool synchronizes mail-enabled user objects in your Active Directory that have a target address with domino.contoso.com in the target address property. This is used to route email cross premises. The user's primary SMTP address in this scenario would remain contoso.com, provided contoso.com is a verified domain in Microsoft 365.

The use of the Exchange admin center and Exchange Management Shell is required to manage all the Exchange recipient properties in the Active Directory.

### Mail formatting

Because you will be configuring Exchange Online to send email to your on-premises mail system, you'll have to make an additional configuration in the cloud to avoid mail formatting issues.

By default, Exchange Online sends messages back to the on-premises email system in rich text or Transport Neutral Encapsulation Format (TNEF), which might result in your users receiving plain text emails with Winmail.dat attachments. As a result, you need to configure Exchange Online to send all mail to your on-premises system in non-TNEF format (HTML or text). To do this, you need to specify the on-premises primary SMTP domain as a *remote domain* in Exchange Online. You can then disable TNEF formatting for all email that is sent to the remote domain.

## Implementation

In many case, the links refer to configuration particulars for an on-premises Exchange messaging system. You will have to translate the goals of the Exchange Server configurations to specific configurations of your third-party messaging solution. As an example, mail-forwarding is a straightforward goal, but it's an area where configuration differs widely across messaging systems.

The following steps outline the process for implementing third-party messaging coexistence with Microsoft 365:

- [Step 1: Sign up for Microsoft 365](#step-1-sign-up-for-microsoft-365)

- [Step 2: Install Exchange Server 2016](#step-2-install-exchange-server-2016)

- [Step 3: Execute the Exchange Hybrid Configuration Wizard](#step-3-execute-the-exchange-hybrid-configuration-wizard)

- [Step 4: Enable mail-enabled users in your on-premises Active Directory](#step-4-enable-mail-enabled-users-in-your-on-premises-active-directory)

- [Step 5: Install and Configure Azure Active Directory Connect to synchronize mail-enabled users into Azure Active Directory (Microsoft 365)](#step-5-install-and-configure-azure-active-directory-connect-to-synchronize-mail-enabled-users-into-azure-active-directory-microsoft-365)

- [Step 6: Configure shared namespace routing](#step-6-configure-shared-namespace-routing)

- [Step 7: Disable TNEF to your on-premises messaging system](#step-7-disable-tnef-to-your-on-premises-messaging-system)

## Step 1: Sign up for Microsoft 365

You need to subscribe to Microsoft 365 to create a service tenant that is used in the deployment with your on-premises email system. Microsoft 365 provides you with an Exchange Online organization in the cloud.

When you subscribe, be sure to verify the primary SMTP domain in your organization with Microsoft 365. The process of verifying a domain proves that you own the domain. The verified domain is also the domain that the Azure Active Directory Sync tool uses to provision objects in the cloud. Then add the mail routing domain representing the third-party system.

Learn more at [Sign up for Microsoft 365](https://products.office.com/business/enterprise-productivity-tools).

## Step 2: Install Exchange Server 2016

1. Read the [system requirements](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2016) and [Active Directory Schema Prep/Domain Prep](https://docs.microsoft.com/Exchange/plan-and-deploy/prepare-ad-and-domains?view=exchserver-2016) information.

2. Complete the appropriate Schema and Domain Prep instructions.

3. Prepare a server to support the [installation of an Exchange 2016 Mailbox Server](https://docs.microsoft.com/Exchange/plan-and-deploy/deploy-new-installations/install-mailbox-role?view=exchserver-2016).

4. [Configure the Accepted Domains](https://docs.microsoft.com/Exchange/plan-and-deploy/post-installation-tasks/configure-mail-flow-and-client-access?view=exchserver-2016#step-2-add-additional-accepted-domains) to match the existing SMTP address domains from the third-party system.

5. Configure a mail routing domain to share the namespace. Typically, a subdomain like domino.contoso.com is a common choice.

6. [Create e-mail address policies](https://docs.microsoft.com/Exchange/plan-and-deploy/post-installation-tasks/configure-mail-flow-and-client-access?view=exchserver-2016#step-3-configure-the-default-email-address-policy) to map the existing naming conventions of the company smtp addresses for primary domains and the mail routing domain.

## Step 3: Execute the Exchange Hybrid Configuration Wizard

1. Use the Exchange Hybrid Configuration Wizard, specifically in Classic mode with the Hybrid Minimal Configuration. In this topic, only do [Step 2: Start express migration](https://docs.microsoft.com/exchange/mailbox-migration/use-minimal-hybrid-to-quickly-migrate#step-2-start-express-migration).

2. Complete the Hybrid Configuration Wizard. Do not use the Express Settings option in the Wizard, AADConnect will be configured later. Do not license the users or migrate any data.

## Step 4: Enable mail-enabled users in your on-premises Active Directory

After you've updated your Active Directory with the Exchange schema, you can now *mail-enable* existing users in your Active Directory. In the context of this scenario, mail-enabled users represent the users (that have mailboxes) in your on-premises messaging system that you want to represent in the cloud address book.

Using the Exchange Management Shell, run [Enable-MailUser](https://docs.microsoft.com/powershell/module/exchange/Enable-MailUser) for each user that you want to be displayed in the cloud address book and who has a mailbox in your on-premises messaging organization.

The **Enable-MailUser** cmdlet only takes the *ExternalEmailAddress* parameter. This is also referred to as the *target address* of the mail-enabled user object. This parameter updates the target SMTP address for the mail-enabled user, which enables cross-premises mail flow.

The *ExternalEmailAddress* parameter is an email address that you enter for the user. The email address must meet the following criteria:

- It must be the valid primary SMTP email address of the user in your on-premises organization.

- The domain part of the email address (to the right of the @ sign) must match the verified domain in Microsoft 365.

- The domain part of the email address must match the UPN domain for the user in the on-premises directory.

Here's an example of an **Enable-MailUser** command:

```powershell
Enable-MailUser -Identity "Gabriela Laureano" -ExternalEmailAddress glaureano@domino.contoso.com -PrimarySMTPAddress glaureano@contoso.com
```

To learn more about how to install, configure, and run Exchange Management Shell, see [Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/exchange-management-shell).

If you need to create or modify users in your on-premises Active Directory, see the following topics:

- **Create new users**: [New-MailUser](https://docs.microsoft.com/powershell/module/exchange/new-mailuser)

- **Modify existing users**: [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/set-mailuser)

## Step 5: Install and Configure Azure Active Directory Connect to synchronize mail-enabled users into Azure Active Directory (Microsoft 365)

1. [Download and install](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-roadmap) Azure AD Connect.

2. View the [prerequisites](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites) and choose the **Customize** option.

3. In the optional features section, select **Exchange Hybrid Deployment**.

The Exchange Hybrid Deployment feature allows for the co-existence of Exchange mailboxes in both on-premises and Microsoft 365. Azure AD Connect is synchronizing a specific set of [attributes](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-attributes-synchronized#exchange-hybrid-writeback) from Azure AD back into your on-premises directory.

## Step 6: Configure shared namespace routing

In the context of this cross-premises email scenario, a *shared namespace* refers to an SMTP addressing namespace. When you configure a shared namespace, you define how messages will be routed between your on-premises mail system and the cloud, and how messages will be routed between your on-premises system, the cloud, and the internet.

The procedure for implementing a shared namespace depends on:

- You on-premises email system.

- Whether you will be configuring your MX record to point to your on-premises email system or to Microsoft 365.

In either case, the cloud-based Exchange Online configurations are similar. After you've configured a shared namespace, you should be able to send email between the two messaging systems. If free busy is required as part of the coexistence strategy, ensure to work with the software vendor to ensure the namespace planning will work with their free busy application.

## Step 7: Disable TNEF to your on-premises messaging system

As previously mentioned, Exchange Online will, by default, send TNEF-encoded messages to the on-premises system. To disable this functionality, see [Manage remote domains in Exchange Online](https://docs.microsoft.com/exchange/mail-flow-best-practices/remote-domains/manage-remote-domains)

## Mailbox migration

This section provides links to more information about migrating mailboxes from your on-premises organization to the cloud.

### Moving messaging-related data

As previously stated, the majority of messaging migration tools that are included with Microsoft 365 are designed to work with Exchange Server. However, Microsoft 365 also includes the [IMAP migration tool](migrating-imap-mailboxes/imap-migration-in-the-admin-center.md) for generic email data migration.

For organizations that use Outlook as an email client, you can also use the [PST Capture tool](https://docs.microsoft.com/microsoft-365/compliance/find-copy-and-delete-pst-files-in-your-organization) to migrate messaging data to the cloud.

For other messaging migration solutions, you might need to work with a third-party solution provider.

Here are some third-party migration tools and partners that can assist with Exchange migrations from third-party platforms:

- **[Binary Tree](https://binarytree.com/)**: Provider of cross-platform messaging migration and coexistence software, with products that provide for the analysis of and the coexistence and migration between on-premises and online enterprise messaging and collaboration environments based on IBM Lotus Notes and Domino and Microsoft Exchange and Microsoft SharePoint.

- **[BitTitan](https://www.bittitan.com/)**: Provider of migration solutions to Exchange Online.

- **[Quest](https://www.quest.com/)**: Provider of on-premises and hosted migration and coexistence software, including pre-migration analysis and complete user and application coexistence. Full-featured migrations from on-premises Microsoft Exchange, IBM Domino, Novell GroupWise, Zimbra and other environments to Microsoft Microsoft 365, Exchange Online, and SharePoint Online.

- **[Transvault](http://www.transvault.com/)**: Provider of migration solutions to Exchange Online.

### Converting cloud users to mailbox-enabled users

If you've already deployed a cross-premises mail routing environment as described in this topic, the users that you've created in the cloud with directory synchronization are mail-enabled users.

To provision mailboxes for these users, license them for Exchange Online in the Microsoft 365 admin console. For more information, see [Sync with existing users in Azure AD](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-existing-tenant#sync-with-existing-users-in-azure-ad)
