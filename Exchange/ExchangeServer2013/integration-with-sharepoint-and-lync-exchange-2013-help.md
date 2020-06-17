---
title: 'Integration with SharePoint and Lync: Exchange 2013 Help'
TOCTitle: Integration with SharePoint and Lync
ms:assetid: 056b29f6-e0e9-4974-b763-002518857a93
ms:mtpsurl: https://technet.microsoft.com/library/JJ150480(v=EXCHG.150)
ms:contentKeyID: 47559934
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Integration with SharePoint and Lync

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 includes many features that integrate with Microsoft SharePoint 2013 and Microsoft Lync 2013. Together, these products offer a suite of features that make scenarios such as enterprise eDiscovery and collaboration using site mailboxes possible.

## Archiving, hold, and eDiscovery

Archiving of email and documents, preserving them for the duration required to meet regulatory compliance and business requirements, and ability to search them quickly to fulfill eDiscovery requests is critical in most organizations. Exchange 2013, SharePoint 2013 and Lync Server 2013 together provide integrated archiving, hold and eDiscovery functionality, allowing you to preserve important data in-place across Exchange mailboxes, SharePoint documents and web sites, and archived Lync content. The eDiscovery Center in SharePoint 2013 allows authorized discovery managers to perform an eDiscovery search for content across these stores, preview search results, and export the data for legal review.

## Archive Lync Server 2013 content in Exchange 2013

With Lync Server 2013 deployed in an organization with Exchange 2013, you can configure Lync to archive instant messaging and on-line meeting content such as shared presentations or documents in the user's Exchange 2013 mailbox. Archiving Lync data in Exchange 2013 allows you to apply retention policies to it. Archived Lync content also surfaces in any eDiscovery searches. For more details about Lync Archiving and how to deploy it, see the following topics:

- [Planning for Archiving](https://docs.microsoft.com/lyncserver/lync-server-2013-planning-for-archiving)

- [Deploying Archiving](https://docs.microsoft.com/lyncserver/lync-server-2013-deploying-archiving)

## Search Exchange, SharePoint and Lync data using the SharePoint 2013 eDiscovery Center

Exchange 2013 allows SharePoint 2013 to search Exchange mailbox content using Federated search API. SharePoint 2013 provides an eDiscovery Center to allow authorized personnel to perform eDiscovery. Microsoft Search Foundation provides a common indexing and search infrastructure to both Exchange 2013 and SharePoint 2013 and allows you to use the same query syntax across both applications. This ensures an eDiscovery search performed in SharePoint 2013 will return the same Exchange 2013 content as the same search performed using In-Place eDiscovery in Exchange 2013. SharePoint 2013 eDiscovery Center also allows you to export content returned in an eDiscovery search, including export of Exchange 2013 content to a PST file.

For more details, see the following topics:

- [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery)

- [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds)

- [Configure eDiscovery in SharePoint 2013](https://docs.microsoft.com/SharePoint/governance/configure-ediscovery-0)

- [What's new in eDiscovery in SharePoint Server 2013](https://support.microsoft.com/office/2229681c-8a19-4efb-a59a-fc9ece9e9557)

- [Configure Exchange for SharePoint eDiscovery Center](configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help.md)

## Site mailboxes

In many organizations, information resides in two different stores - email in Microsoft Exchange and documents in SharePoint, with two different interfaces to access them. This causes a disjointed user experience and impedes effective collaboration. Site mailboxes allow users to collaborate effectively by bringing together Exchange emails and SharePoint documents. For users, a site mailbox serves as a central filing cabinet, providing a place to file project emails and documents that can only be accessed and edited by site members. Site mailboxes are surfaced in Outlook 2013 and give users easy access to the email and documents for the projects they care about. Additionally, the same set of content can be accessed directly from the SharePoint site itself.

Under the covers of a site mailbox, the content is kept where it belongs. Exchange stores the email, providing users with the same message view for email conversations that they use every day for their own mailboxes. SharePoint stores the documents, bringing document coauthoring and versioning to the table. Exchange synchronizes just enough metadata from SharePoint to create the document view in Outlook (e.g. document title, last modified date, last modified author, size).

Site mailboxes are provisioned and managed from SharePoint 2013. For more details, see the following topics:.

- [Site mailboxes](site-mailboxes-exchange-2013-help.md)

- [Configure site mailboxes in SharePoint Server 2013](https://docs.microsoft.com/SharePoint/administration/configure-site-mailboxes-in-sharepoint)

## Unified contact store

Unified contact store (UCS) is a feature that provides a consistent contact experience across Microsoft Office products. This feature enables users to store all contact information in their Exchange 2013 mailbox so that the same contact information is available globally across Lync, Exchange, Outlook and Outlook Web App.

After Lync Server 2013 is installed in an environment with Exchange 2013 and you have configured server-to-server authentication between the two, users can initiate the migration of existing contacts from Lync Server 2013 to Exchange 2013. For details, see [Planning and Deploying Unified Contact Store](https://docs.microsoft.com/lyncserver/lync-server-2013-planning-and-deploying-unified-contact-store).

## User photos

User photos is a feature that allows you to store high resolution user photos in Exchange 2013 that can be accessed by client applications, including Outlook, Outlook Web App, SharePoint 2013, Lync 2013, and mobile email clients. A low-resolution photo is also stored in Active Directory. As with Unified contact stores, user photos allow your organization to maintain a consistent user profile photo that can be consumed by client applications without requiring each application to have its own user photos and different ways to add and manage them. Users can manage their own photos using Outlook Web App, SharePoint 2013 or Lync 2013. For detail about managing photos on Outlook Web App, see [My account](https://go.microsoft.com/fwlink/p/?linkid=269646).

## Lync presence in Outlook Web App

In Exchange 2013 environments with Lync Server 2013 installed, you can configure them to enable users to see presence information in Outlook Web App. Users can see their instant messaging contacts and groups in the Navigation Pane of Outlook Web App, respond to or initiate instant messaging sessions from Outlook Web App and manage their instant messaging contacts and groups.

## OAuth authentication

Exchange 2013, SharePoint 2013 and Lync Server 2013 provide the rich cross-product functionality described above using OAuth authorization protocol for server-to-server authentication. Using the same authentication protocol allows these applications to seamlessly and securely authenticate to each other. The authentication mechanism supports authentication as an application using the context of a linked account and user impersonation where the access request is made in the user context.

OAuth is a standard authorization protocol used by many web sites and web services. It allows clients to access resources provided by a resource server without having to provide a username and password. Authentication is performed by an authorization server trusted by the resource owner, which provides the client with an access token. The token grants access to a specific set of resources for a specified period. For more details about Exchange 2013's implementation of OAuth, see [\[MS-XOAUTH\]: OAuth 2.0 Authorization Protocol Extensions](https://docs.microsoft.com/openspecs/exchange_server_protocols/ms-xoauth/0b717658-4ceb-4401-9da9-7860c9ca2f2f).

**OAuth in on-premises deployments**

Within an on-premises deployment, Exchange 2013, SharePoint 2013 and Lync Server 2013 do not require an authorization server to issue tokens. Each of these applications issue self-signed tokens to access resources provided by other application. The application that provides access to resources, for example Exchange 2013, must trust the self-signed tokens presented by the calling application. Trust is established by creating a *partner application* configuration for the calling application, which includes the calling application's ApplicationID, certificate, and AuthMetadataUrl. Exchange 2013, SharePoint 2013 and Lync Server 2013 publish their auth metadata document in a well-known URL.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Server</strong></p></td>
<td><p><strong>AuthMetadataUrl</strong></p></td>
</tr>
<tr class="even">
<td><p>Exchange 2013</p></td>
<td><p><code>https://&lt;serverfqdn&gt;/autodiscover/metadata/json/1</code></p></td>
</tr>
<tr class="odd">
<td><p>Lync Server 2013</p></td>
<td><p><code>https://&lt;serverfqdn&gt;/metadata/json/1</code></p></td>
</tr>
<tr class="even">
<td><p>SharePoint 2013</p></td>
<td><p><code>https://&lt;serverfqdn&gt;/_layouts/15/metadata/json/1</code></p></td>
</tr>
</tbody>
</table>

**Exchange 2013 Server Auth Certificate**

Exchange 2013 Setup creates a self-signed certificate with the friendly name Microsoft Exchange Server Auth Certificate. The certificate is replicated to all front-end servers in the Exchange 2013 organization. The certificate's thumbprint is specified in Exchange 2013's authorization configuration, along with its service name, a well-known GUID that represents on-premises Exchange 2013. Exchange uses the authorization configuration to publish its auth metadata document.

> [!IMPORTANT]
> The default Server Auth Certificate created by Exchange 2013 is valid for five years. You must ensure the authorization configuration includes a current certificate.

When Exchange 2013 receives an access request from a partner application via Exchange Web Services (EWS), it parses the `www-authenticate` header of the https request, which contains the access token signed by the calling server using its private key. The auth module validates the access token using the partner application configuration. It then grants access to resources based on the RBAC permissions granted to the application. If the access token is on behalf of a user, the RBAC permissions granted to the user are checked. For example, if a user performs an eDiscovery search using the eDiscovery Center in SharePoint 2013, Exchange checks whether the user is a member of the Discovery Management role group or has the Mailbox Search role assigned and the mailboxes being searched are within the scope of the RBAC role assignment. For more details, see [Permissions](permissions-exchange-2013-help.md).

## Managing OAuth Authentication

In Exchange 2013, there are two configuration objects you must manage for OAuth authentication with partner applications:

  - **AuthConfig**: The AuthConfig is created by Exchange 2013 Setup and is used to publish the auth metadata. You don't need to manage the auth config except to provision a new certificate when the existing certificate is close to expiration. When this happens, you can renew the existing certificate and configure the new certificate as the next certificate in the AuthConfig along with its effective date. The new certificate is automatically replicated to other Exchange 2013 in the organization, the auth metadata document is refreshed with details of the new certificate, and the AuthConfig rolls over to the new certificate on the effective date.

  - **Partner applications**: To enable partner applications to request access tokens from Exchange 2013, you must create a partner application configuration. Exchange 2013 provides the `Configure-EnterprisePartnerApplication.ps1` script, which allows you to quickly and easily create partner application configurations and minimize configuration errors. For details, see [Configure OAuth authentication with SharePoint 2013 and Lync 2013](configure-oauth-authentication-with-sharepoint-2013-and-lync-2013-exchange-2013-help.md).
