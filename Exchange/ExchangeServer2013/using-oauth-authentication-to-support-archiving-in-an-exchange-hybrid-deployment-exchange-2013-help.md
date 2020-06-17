---
title: 'Using OAuth authentication to support Archiving in an Exchange hybrid deployment'
TOCTitle: Using OAuth authentication to support Archiving in an Exchange hybrid deployment
ms:assetid: deb882b1-1ae2-40f3-a71c-423fafe3d66a
ms:mtpsurl: https://technet.microsoft.com/library/Dn689104(v=EXCHG.150)
ms:contentKeyID: 62380199
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Using OAuth authentication to support Archiving in an Exchange hybrid deployment

_**Applies to:** Exchange Server 2013_

If you're in an Exchange 2013 hybrid deployment and use Exchange Online Archiving (EOA) for Exchange Server, you must configure OAuth authentication between your on-premises and Exchange Online organizations after upgrading to Exchange 2013 Cumulative Update 5 (CU5). EOA allows you to have a cloud-based archive for your users with on-premises mailboxes. In this scenario, the Messaging Records Management (MRM) assistant on your on-premises mailbox server applies archiving policies and moves messages automatically from a user's mailbox to their cloud-based archive. In Exchange 2013 CU5, it uses OAuth authentication.

For step-by-step instructions for configuring OAuth authentication, see [Configure OAuth authentication between Exchange and Exchange Online organizations](configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help.md).

## What is OAuth authentication?

OAuth authentication is a server-to-server authentication protocol that allows applications to authenticate to each other. With OAuth authentication, user credentials and passwords are not passed from one computer to another. Instead, authentication and authorization is based on the exchange of security tokens, which grant access to a specific set of resources for a specific amount of time.

OAuth authentication typically involves three parties: a single authorization server and the two realms that need to communicate with one another. Security tokens are issued by the authorization server (also known as a security token server) to the two realms that need to communicate; these tokens verify that communications originating from one realm should be trusted by the other realm. When using OAuth authentication between an on-premises Exchange organization and Exchange Online, the function of the authorization server is provided by Azure Active Directory Access Control Services (ACS) in your Microsoft 365 or Office 365 organization. For example, during a cross-premises eDiscovery search, Azure Active Directory ACS issues tokens that verify that an administrator or compliance officer from the Exchange on-premises organization is able to access mailboxes in the Exchange Online organization, and vice-versa.

## Configuring OAuth authentication to support Archiving

As previously stated, see [Configure OAuth authentication between Exchange and Exchange Online organizations](configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help.md) for instructions to configure OAuth authentication to support Archiving in an Exchange hybrid deployment.

If OAuth isn't configured for your Exchange hybrid deployment, you can't use archive policies to automatically move items from a user's primary mailbox in your on-premises organization to the user's cloud-based archive in Exchange Online.

## More information

- You must also configure OAuth authentication to perform cross-premises eDiscovery searches of your on-premises and cloud-based mailboxes in a single eDiscovery search. See [Using OAuth authentication to support eDiscovery in an Exchange hybrid deployment](using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help.md).

- You can also configure OAuth authentication to allow other applications, such as SharePoint 2013 and Lync Server 2013, to authenticate to Exchange 2013. For more information, see [Configure OAuth authentication with SharePoint 2013 and Lync 2013](configure-oauth-authentication-with-sharepoint-2013-and-lync-2013-exchange-2013-help.md).

- You can configure server-to-server authentication between Exchange 2013 and SharePoint 2013 so administrators and compliance officers can use the eDiscovery Center in SharePoint 2013 to search Exchange 2013 mailboxes. For more information, see [Configure Exchange for SharePoint eDiscovery Center](configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help.md).

- You can configure an Exchange hybrid deployment using the Mail migration advisor. For more information, see [Use the Microsoft 365 and Office 365 mail migration advisor](https://docs.microsoft.com/exchange/mail-migration-jump).
