---
title: 'Configure federated sharing: Exchange 2013 Help'
TOCTitle: Configure federated sharing
ms:assetid: b25ae450-def3-4797-a5fc-6e9bcee71a5d
ms:mtpsurl: https://technet.microsoft.com/library/JJ657483(v=EXCHG.150)
ms:contentKeyID: 49289377
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure federated sharing

_**Applies to:** Exchange Server 2013_

With federated sharing in Exchange Server 2013, users can share information with recipients in external federated organizations. This includes sharing their free/busy (also known as calendar availability) information for scheduling purposes or, depending on the nature of the business relationship, sharing more detailed calendar information. To learn more about federation sharing, see [Sharing](sharing-exchange-2013-help.md).

> [!IMPORTANT]
> This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see <A href="https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china">Learn about Office 365 operated by 21Vianet</A>.

## What do you need to know before you begin?

- Estimated time to complete this task: 1 hour.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Create and configure a federation trust

A federation trust establishes a trust relationship between an Exchange 2013 organization and the Azure Active Directory authentication system and is a requirement for federated sharing.

For detailed instructions, see [Configure a federation trust](configure-a-federation-trust-exchange-2013-help.md).

## Step 2: Create an organization relationship

An organization relationship enables users in your Exchange organization to share calendar free/busy information as part of federated sharing with other federated Exchange organizations. Federated sharing can be configured between two federated Exchange 2013 organizations or between a federated Exchange 2013 organization and federated Exchange 2010 organizations.

For detailed instructions, see [Create an organization relationship](create-an-organization-relationship-exchange-2013-help.md).

## Step 3: Create a sharing policy

Sharing policies enable user-established, people-to-people sharing of calendar information with different types of external users. They support the sharing of calendar and contact information with external federated organizations, external non-federated organizations, and individuals with Internet access. If you don't need to configure people-to-people or contact sharing (organization-level sharing only), you don't need to configure a sharing policy.

For detailed instructions, see [Create a sharing policy](create-a-sharing-policy-exchange-2013-help.md).

## Step 4: Configure an Autodiscover public DNS record

You need to add an alias canonical name (CNAME) resource record to your public-facing DNS. The new CNAME record should point to an Internet-facing Exchange 2013 Client Access server that's running the Autodiscover service.

For detailed instructions about how to add CNAME records, see the host service for your public DNS records. Typically this is an Internet-based service that may also host your domain website.
