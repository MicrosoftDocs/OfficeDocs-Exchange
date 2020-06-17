---
localization_priority: Normal
description: 'Summary: Learn about ways to help enterprises meet compliance and regulatory requirements using Exchange 2016, SharePoint Server 2016, and Skype for Business Server.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 056b29f6-e0e9-4974-b763-002518857a93
ms.reviewer:
title: Plan Exchange 2016 integration with SharePoint and Skype for Business
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Plan Exchange 2016 integration with SharePoint and Skype for Business

Exchange 2016 integration with SharePoint Server 2016 and Skype for Business allow for services that provide the ability to preserve, archive, and then quickly search email, documents, and other content. Together, these enterprise applications make possible scenarios such as eDiscovery and collaboration using site mailboxes to let your organization preserve important data. Critical in most organizations these days is the ability to archive and then locate email and documents as required to meet compliance and regulatory requirements. You can use Exchange 2016 along with SharePoint 2016 and Skype for Business to:

- Archive Exchange mailboxes

- Archive Skype for Business content

- Preserve SharePoint Server 2016 documents and websites

- Search across stores using eDiscovery

- Authenticate seamlessly across servers

The eDiscovery Center introduced in SharePoint 2013 provides content identification, preservation, collection, processing, and analysis. In an Exchange environment, eDiscovery lets you archive content discovered across SharePoint Server 2016, Skype for Business, andExchange. You can use the eDiscovery Center to create eDiscovery Case sites that are used to organize in-place holds, queries, and exports for a specific case.

Exchange 2016, SharePoint Server 2016, and Skype for Business Server use the standard protocol, Open Authorization (OAuth), for server-to-server authentication to provide the cross-product functionality described here. Using the same protocol allows these applications to seamlessly and securely authenticate to each other. The authorization method supports authentication as an application by means of a linked account and user impersonation where the access request is made in the user context. You can learn more about OAuth later in this article in the [Server-to-server authentication using OAuth](#server-to-server-authentication-using-oauth) section.

> [!NOTE]
> For enterprises that use Lync Server 2013, you can still make full use of the features described in this topic.

## Archive Skype for Business content in Exchange 2016

With Exchange 2016 and Lync Server 2013 deployed in an organization, you can configure Skype for Business to archive instant message and on-line meeting content, including shared presentations or documents in the user's Exchange 2016 mailbox. Archiving Skype for Business data in Exchange 2016 allows you to apply retention policies to the data. Archived Skype for Business content also surfaces in any eDiscovery searches. For more details about Skype for Business archiving and how to deploy it, see the following topics:

- [Planning for Archiving](https://docs.microsoft.com/SkypeForBusiness/plan-your-deployment/archiving/archiving)

- [Deploying Archiving](https://docs.microsoft.com/SkypeForBusiness/deploy/deploy-archiving/deploy-archiving)

## Preserve documents in SharePoint Server 2016

You can create a query-based hold to preserve items that meet your specified criteria with an In-Place Hold.

For example, Litigation Hold preserves until the hold is removed any deleted items as well as original versions of modified items . You can optionally specify a hold duration that preserves a mailbox item for the named duration period. If you specify a hold duration period, it's calculated from the date a message is received or a mailbox item is created. For details, see [Create or remove an In-Place Hold](../../policy-and-compliance/holds/in-place-holds.md).

For more details on eDiscovery see the following topics:

- [In-Place eDiscovery in Exchange Server](../../policy-and-compliance/ediscovery/ediscovery.md)

- [In-Place Hold and Litigation Hold in Exchange Server](../../policy-and-compliance/holds/holds.md)

- [Configure eDiscovery in SharePoint Server](https://docs.microsoft.com/SharePoint/governance/configure-ediscovery-0)

- [What's new in eDiscovery in SharePoint Server](https://docs.microsoft.com/SharePoint/what-s-new/new-and-improved-features-in-sharepoint-server-2016)

- [Configure Exchange for SharePoint eDiscovery Center](https://docs.microsoft.com/exchange/configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help)

## Search across applications by using eDiscovery

SharePoint Server 2016 provides the eDiscovery Center to help you locate and then transfer relevant content as needed to meet regulatory requirements. eDiscovery is the process of finding, preserving, analyzing, and producing content in digital format required by litigation or investigations. You can use eDiscovery across Exchange 2016, SharePoint Server 2016, and Skype for Business files. You can help protect content in-place that you've identified with eDiscovery queries and then export the results into an offline format to hand off for legal review. In-Place Hold in eDiscovery lets you:

- Protect content in-place and in real time at reduced storage costs, without affecting your users' daily work.

- Query to collect up-to-date, relevant content and statistics quickly answer questions.

- Export relevant content in an offline and portable format.

If your organization adheres to legal discovery requirements, that is, anything related to organizational policy, compliance, or lawsuits, In-Place eDiscovery in Exchange Server 2016 can help you perform discovery searches for relevant content within mailboxes. You can also use In-Place eDiscovery in an Exchange hybrid environment to search on-premises and cloud-based mailboxes in the same search.

When you configure server-to-server authentication betweenExchange 2016 and SharePoint Server 2016 in on-premises deployments, administrators and compliance officers can use the eDiscovery Center. For more information, see [Configure Exchange for SharePoint eDiscovery Center](https://docs.microsoft.com/exchange/configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help). In hybrid deployments, for more information see [Using Oauth Authentication to Support eDiscovery in an Exchange Hybrid Deployment](https://docs.microsoft.com/exchange/using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help)

You can identify and reduce your data set by using keyword syntax, property restrictions, and refinements. The query experience focuses on statistics for individual sources and query fragments to help you make decisions about the content you're searching across. You can also preview SharePoint 2016 and Exchange 2016 content to confirm that you have identified the right set of results.

## Server-to-server authentication using OAuth
<a name="BKMK_OAuth"> </a>

The OAuth protocol is used by many web sites and web services to let clients access resources without having to provide a username and password. An authorization server trusted by the resource owner provides the client with an access token that grants access to a specific set of resources for a specified period. Exchange 2016 allows other applications to use OAuth to authenticate to Exchange. You'll need to configure the applications in Exchange as partner applications.

There are two configuration objects used for OAuth andExchange 2016 partner applications: AuthConfig and the partner application configuration.

- **AuthConfig**: Exchange 2016 Setup creates AuthConfig to publish the auth metadata. You only need to manage AuthConfig to provision a new certificate when the existing certificate is close to expiration. When this happens, you can renew the existing certificate and configure the new certificate as the next certificate in the AuthConfig along with its effective date.

    Exchange 2016 Setup creates a self-signed certificate with the friendly name Microsoft Exchange Server Auth Certificate and replicates the certificate to all front-end servers in the Exchange organization. The certificate's thumbprint is specified in the authorization configuration for Exchange 2016, along with its service name, which is a well-known GUID that represents on-premises Exchange 2016. Exchange uses the authorization configuration to publish its auth metadata document.

- **Partner applications**: You enable partner applications by creating a partner application configuration to request access tokens from Exchange. Exchange 2016 provides the `Configure-EnterprisePartnerApplication.ps1` script that lets you quickly and easily create partner application configurations and minimize configuration errors.

    When Exchange 2016 receives an access request from a partner application via Exchange Web Services (EWS), the following events take place.

  - EWS parses the `www-authenticate` header of the https request that contains the access token signed by the calling server using its private key.

  - The auth module validates the access token using the partner application configuration.

  - The module then grants access to resources based on the RBAC permissions granted to the application. If the access token is on behalf of a user, the RBAC permissions granted to the user are checked.

    For example, if a user performs an eDiscovery search using the eDiscovery Center in SharePoint 2016, Exchange checks whether the user is a member of the Discovery Management role group or has the Mailbox Search role assigned and the mailboxes being searched are within the scope of the RBAC role assignment. For more details, see [Permissions](../../permissions/permissions.md).

In on-premises deployments, Exchange 2016, SharePoint Server 2016, and Skype for Business Server 2015 do not require an authorization server to issue tokens. Each application issues self-signed tokens to access the resources provided by other applications. The application that provides access to resources, for example Exchange 2016, trusts the self-signed tokens presented by the calling application. Trust is established by creating a *partner application* configuration for the calling application, which includes the calling application's ApplicationID, certificate, and AuthMetadataUrl. Exchange 2016, SharePoint 2016, and Skype for Business publish their auth metadata document in a well-known URL.

**Auth metadata URLs**

|**Server**|**AuthMetadataUrl**|
|:-----|:-----|
| Exchange 2016| `https://<serverfqdn>/autodiscover/metadata/json/1`|
|SharePoint Server 2016| `https://<serverfqdn>/_layouts/15/metadata/json/1`|
|Skype for Business| `https://<serverfqdn>/metadata/json/1`|

In hybrid deployments, you need to configure OAuth authorization protocol between your on-premises Exchange 2016 and Exchange Online organizations. Hybrid deployments by default continue to use the federation trust process.

Certain Exchange 2016 features are only fully available across your organization by using the new OAuth protocol. For example, before you can use In-Place eDiscovery to search on-premises and cloud-based mailboxes in an Exchange hybrid organization, you need to configure OAuth authentication between your Exchange on-premises and Exchange Online organizations. The Hybrid Configuration Wizard doesn't manage the OAuth authorization connection. For more information, see [Configure OAuth Authentication Between Exchange and Exchange Online Organizations](https://docs.microsoft.com/exchange/configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help).

In online deployments, Exchange Online, SharePoint Online and Skype for Business Online need to be configured for a modern authentication connection. Modern authentication brings Active Directory Authentication Library (ADAL)-based sign in to Office 2013 Windows clients. Office 2013 client applications sign in to the Office 365 service to gain access to Exchange Online, SharePoint Online and Skype for Business Online. We recommend that you enable Exchange Online for modern authentication when enabling modern authentication for Skype for Business. Modern authentication is enabled by default in SharePoint Online. For more information, see [Enable or disable modern authentication for Outlook in Exchange Online](https://docs.microsoft.com/Exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

The per service default state of modern authentication is:

- Skype for Business Online - OFF by default

- Skype for Business Online - OFF by default

- SharePoint Online - ON by default.

> [!IMPORTANT]
> The default Server Auth Certificate created by Exchange 2016 is valid for five years. You need to make sure that the authorization configuration includes a current certificate.

## Manage SharePoint site mailboxes
<a name="BKMK_OAuth"> </a>

In many organizations, information resides in two different stores: email in Exchange and documents in SharePoint. There are two different interfaces to access these stores. This makes for a disjointed user experience that impedes effective collaboration. Site mailboxes in SharePoint let users collaborate effectively by bringing together Exchange emails and SharePoint documents. For users, a site mailbox serves as a central filing cabinet, providing a place to file project emails and documents that can only be accessed and edited by site members. Site mailboxes are visible in Outlook 2016 to give users easy access to the email and documents for the projects they care about. Additionally, the same set of content can be accessed directly from the SharePoint site itself.

In a site mailbox, content is kept where it belongs. Exchange stores the email, providing users with the same message view for email conversations that they use every day for their own mailboxes. SharePoint stores the documents, which allows for document coauthoring and versioning. Exchange synchronizes just enough metadata from SharePoint to create the document view in Outlook (that is, document title, last modified date, last modified author, and size).

You can provision and manage site mailboxes from SharePoint Server 2016. For more information, including how to configure site mailboxes, see the following topics.

- [Site Mailboxes](https://docs.microsoft.com/exchange/site-mailboxes-exchange-2013-help)

- [Configure email integration for a SharePoint Server farm](https://docs.microsoft.com/SharePoint/administration/configure-email-integration)

## Manage access to unified contact store
<a name="BKMK_OAuth"> </a>

The unified contact store (UCS) feature provides a consistent contact experience across Office products. This feature lets users store all contact information in their Exchange 2016 mailbox so that the same contact information is available globally across Skype for Business, SharePoint, Exchange, Outlook and Outlook on the web. When you deploy aSkype for Business Server and publish the topology, UCS is enabled for all users by default and no additional action is needed. For more information, see [Configure Skype for Business Server to use the unified contact store](https://docs.microsoft.com/skypeforbusiness/deploy/integrate-with-exchange-server/use-the-unified-contact-store).

A user's contacts are automatically migrated to the Exchange 2016 server when the user:

- Is assigned a user services policy that has UcsAllowed set to True.

- Was provisioned with an Exchange 2016 mailbox and has signed into the mailbox at least once.

- Logs in by using a Skype for Business rich client.

After you have installed SharePoint Server 2016 in an environment with Exchange 2016 and you have configured server-to-server authentication between the two, users can initiate the migration of existing contacts from SharePoint 2016 or Skype for Business Server 2015 to Exchange 2016. For details, see [Planning and Deploying Unified Contact Store](https://docs.microsoft.com/SkypeForBusiness/plan-your-deployment/integrate-with-exchange/unified-contact-store).

## Manage access to high-resolution user photos
<a name="BKMK_OAuth"> </a>

The user photos feature lets you store high resolution user photos in Exchange 2016 that can be accessed by client applications, including Outlook, Outlook on the web, SharePoint 2016, Skype for Business, and mobile email clients. A low-resolution photo is also stored in Active Directory. The cmdlet *Set-UserPhoto* stores a copy of a high resolution image in the user's Exchange mailbox, and stores a 64×64 pixel copy of the photo as an image in the Active Directory attribute thumbnailPhoto.

As with UCS, user photos allow your organization to maintain a consistent user profile photo that can be consumed by client applications without requiring each application to have its own user photos and different ways to add and manage them. Users can manage their own photos by using Outlook on the web, SharePoint 2016 or Skype for Business. For detail about managing photos on Outlook on the web, see [My account](https://go.microsoft.com/fwlink/p/?LinkId=269646).
