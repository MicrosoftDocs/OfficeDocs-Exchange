---
title: 'Using OAuth authentication to support eDiscovery in Exchange hybrid deployment'
TOCTitle: Using OAuth authentication to support eDiscovery in an Exchange hybrid deployment
ms:assetid: b069f8db-fbe1-4047-ad97-d00172ee6a12
ms:mtpsurl: https://technet.microsoft.com/library/Dn497703(v=EXCHG.150)
ms:contentKeyID: 61310597
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Using OAuth authentication to support eDiscovery in an Exchange hybrid deployment

_**Applies to:** Exchange Server 2013_

To successfully perform cross-premises eDiscovery searches in an Exchange 2013 hybrid organization, you will have to configure OAuth (Open Authorization) authentication between your Exchange on-premises and Exchange Online organizations so that you can use In-Place eDiscovery to search on-premises and cloud-based mailboxes. OAuth authentication supports the following eDiscovery scenarios in an Exchange hybrid deployment:

- Search on-premises mailboxes that use Exchange Online Archiving for cloud-based archive mailboxes.

- Search on-premises and cloud-based mailboxes in the same eDiscovery search.

For step-by-step instructions for configuring OAuth authentication to support eDiscovery, see [Configure OAuth authentication between Exchange and Exchange Online organizations](configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help.md).

## What is OAuth authentication?

OAuth authentication is a server-to-server authentication protocol that allows applications to authenticate to each other. With OAuth authentication, user credentials and passwords are not passed from one computer to another. Instead, authentication and authorization is based on the exchange of security tokens, which grant access to a specific set of resources for a specific amount of time.

OAuth authentication typically involves three parties: a single authorization server and the two realms that need to communicate with one another. Security tokens are issued by the authorization server (also known as a security token server) to the two realms that need to communicate; these tokens verify that communications originating from one realm should be trusted by the other realm. When using OAuth authentication between an on-premises Exchange organization and Exchange Online, the function of the authorization server is provided by Microsoft Azure Active Directory Access Control Services (ACS) in your Microsoft 365 or Office 365 organization. For example, during a cross-premises eDiscovery search, Azure ACS issues tokens that verify that an administrator or compliance officer from the Exchange on-premises organization is able to access mailboxes in the Exchange Online organization, and vice-versa.

## eDiscovery scenarios in a hybrid deployment

The follow table identifies the eDiscovery scenarios in an Exchange hybrid deployment that require OAuth authentication.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>eDiscovery scenario</th>
<th>Requires OAuth authentication?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Search Exchange on-premises mailboxes and Exchange Online mailboxes in the same eDiscovery search initiated from the Exchange on-premises organization. For example, searching all mailboxes in the organization in a single eDiscovery search.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>Search Exchange on-premises mailboxes that use Exchange Online Archiving for cloud-based archive mailboxes. When you use In-Place eDiscovery, both the primary and archive mailboxes are searched.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>Search Exchange Online mailboxes from an eDiscovery search initiated from the Exchange on-premises organization by an administrator or compliance officer.</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>Search on-premises mailboxes using an eDiscovery search initiated from the Exchange on-premises organization by an administrator or compliance officer.</p></td>
<td><p>No</p>

> [!NOTE]
> As previously discussed, OAuth authentication would be required if the on-premises mailboxes were configured with cloud-based archive mailboxes.

</td>
</tr>
<tr class="odd">
<td><p>Search Exchange Online mailboxes from an eDiscovery search initiated from Exchange Online or the eDiscovery Center in SharePoint Online by a Microsoft 365 or Office 365 tenant administrator or a compliance officer signed in to a Microsoft 365 or Office 365 user account.</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

## Configuring OAuth authentication to support eDiscovery

As previously stated, see [Configure OAuth authentication between Exchange and Exchange Online organizations](configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help.md) for instructions to configure OAuth authentication to support eDiscovery in an Exchange hybrid deployment.

If OAuth isn't configured for your Exchange hybrid deployment, you can't use eDiscovery to search Exchange on-premises and Exchange Online mailboxes in the same eDiscovery search. You will have to search on-premises mailboxes from an eDiscovery search initiated from your on-premises organization. Similarly, you can only search Exchange Online mailboxes from an eDiscovery search initiated from your Exchange Online organization or by using the eDiscovery Center in SharePoint Online. Additionally, you won't be able to search primary on-premises mailboxes if their corresponding archive mailbox resides in Exchange Online or in an Exchange Online Archiving organization.

## More information

- You can also use OAuth authentication to allow other applications, such as SharePoint 2013 and Lync Server 2013, to authenticate to Exchange 2013. For more information, see [Configure OAuth authentication with SharePoint 2013 and Lync 2013](configure-oauth-authentication-with-sharepoint-2013-and-lync-2013-exchange-2013-help.md).

- You can configure server-to-server authentication between Exchange 2013 and SharePoint 2013 so administrators and compliance officers can use the eDiscovery Center in SharePoint 2013 to search Exchange 2013 mailboxes. For more information, see [Configure Exchange for SharePoint eDiscovery Center](configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help.md).

- You can configure an Exchange hybrid deployment using the Mail migration advisor. For more information, see [Use the Microsoft 365 and Office 365 mail migration advisor](https://docs.microsoft.com/exchange/mail-migration-jump).
