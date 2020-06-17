---
localization_priority: Critical
description: 'Summary: Learn about the new features that are available in Exchange Server 2016 and Exchange Server 2019 when you upgrade from previous versions of Exchange.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 97501135-2149-4590-8373-98e638ac8eb1
monikerRange: exchserver-2016 || exchserver-2019
title: What's new in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.reviewer:
ms.prod: exchange-server-it-pro
manager: serdars

---

# What's new in Exchange Server

::: moniker range="exchserver-2019"
Exchange Server 2019 brings a new set of technologies, features, and services to Exchange Server, the messaging platform that provides email, scheduling, and tools for custom collaboration and messaging service applications. Its goal is to support people and organizations as their work habits evolve from a communication focus to a collaboration focus. At the same time, Exchange 2019 helps lower the total cost of ownership whether you deploy Exchange 2019 on-premises or provision your mailboxes in the cloud.

Choose the section below that matches the version of Exchange that you're upgrading from. If you want to know about features that have been removed or replaced in Exchange 2019, see [What's discontinued in Exchange Server](discontinued-features.md).

For more information about deploying Exchange 2019, see [Planning and deployment for Exchange Server](../plan-and-deploy/plan-and-deploy.md).

## What's new when upgrading from Exchange 2016 to Exchange 2019?

### Security

- **Windows Server Core support**: Running Exchange on a Windows deployment with less surface area means less attack surface area and fewer components to service.

- **Block external access to Exchange admin center (EAC) and the Exchange Management Shell**: You can use Client Access Rules to only allow administration of Exchange from the internal network instead of using complex network and firewall rules.

- **TLS 1.2 is the only version that's enabled by default**: Exchange Server 2019 includes important changes to improve the security of client and server connections. The default configuration for encryption will enable TLS 1.2 only and disable support for older algorithms (namely, DES, 3DES, RC2, RC4 and MD5). It will also configure elliptic curve key exchange algorithms with priority over non-elliptic curve algorithms. In Exchange Server 2016 and later, all cryptography settings are inherited from the configuration specified in the operating system. For additional information, see [Exchange Server TLS Guidance](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Exchange-Server-TLS-guidance-part-1-Getting-Ready-for-TLS-1-2/ba-p/607649).

### Performance

- **Improved search infrastructure**: The completely rebuilt search infrastructure for cloud scale and reliability in Exchange Online is now available in Exchange 2019. This new search infrastructure allows for indexing of bigger files, simpler management, and better search performance.

- **Faster, more reliable failovers**: The changes to the search architecture result in significantly faster and more reliable failover over between servers.

- **Metacache database**: Improvements at the core of Exchange's database engine enable better overall performance and take advantage of the latest storage hardware, including larger disks and SSDs.

- **Modern hardware support**: Exchange now supports up to 256 GB of memory and 48 CPU cores.

- **Dynamic database cache**: The information store process employs dynamic memory cache allocation optimizing memory usage to active database usage.

### Clients

- **Calendar - Do Not Forward**: This is similar to Information Rights Management (IRM) for calendar items without the IRM deployment requirements. Attendees can't forward the invitation to other people, and only the organizer can invite additional attendees.

- **Calendar - Better Out of Office**: Additional options when you won't be in the office. Key options include: add an event to your calendar that shows you as Away/Out of Office, and a quick option to cancel/decline meetings that will happen while you're away.

- **Calendar - Remove-CalendarEvents cmdlet**: Enables administrators to cancel meetings that were organized by a user that has left the company. Previously, conference rooms or meeting attendees would have these defunct meetings permanently on their calendars.

- **Email address internationalization (EAI)**: Email addresses that contain non-English characters can now be routed and delivered natively.

## What's new when upgrading from Exchange 2013 to Exchange 2019?

### Exchange 2019 architecture

Today, CPU horsepower is significantly less expensive and is no longer a constraining factor. With that constraint lifted, the primary design goal for Exchange 2019 is for simplicity of scale, hardware utilization, and failure isolation. With Exchange 2019, we reduced the number of server roles to two: the Mailbox and Edge Transport server roles.

Unified Messaging (UM) has been removed from Exchange 2019. Other than that, the Mailbox server in Exchange 2019 includes all of the server components from the Exchange 2013 Mailbox and Client Access server roles:

- Client Access services provide authentication, limited redirection, and proxy services. Client Access services don't do any data rendering and offer all the usual client access protocols: HTTP, POP and IMAP, and SMTP.

- Mailbox services include all the traditional server components found in the Exchange 2013 Mailbox server role except Unified Messaging: the backend client access protocols, Transport service, and Mailbox databases. The Mailbox server handles all activity for the active mailboxes on that server.

 The Edge Transport role is typically deployed in your perimeter network, outside your internal Active Directory forest, and is designed to minimize the attack surface of your Exchange deployment. By handling all Internet-facing mail flow, it also adds additional layers of message protection and security against viruses and spam, and can apply mail flow rules (also known as transport rules) to control message flow.

For more information about the Exchange 2019 architecture, see [Exchange architecture](../architecture/architecture.md).

Along with the new Mailbox role, Exchange 2019 now allows you to proxy traffic from Exchange 2013 Client Access servers to Exchange 2019 mailboxes. This new flexibility gives you more control in how you move to Exchange 2019 without having to worry about deploying enough front-end capacity to service new Exchange 2019 servers.

### Clients

#### Outlook on the web (formerly known as Outlook Web App)

Outlook Web App is now known as Outlook on the web, which continues to let users access their Exchange mailbox from almost any web browser.

> [!NOTE]
> Supported Web browsers for Outlook on the web in Exchange 2019 are Microsoft Edge, Internet Explorer 11, and the most recent versions of Mozilla Firefox, Google Chrome, and Apple Safari.

The former Outlook Web App user interface has been updated and optimized for tablets and smart phones, in addition to desktop and laptop computers. New Exchange 2019 features include:

- **Platform-specific experiences for phones** for both iOS and Android.

- **Premium Android experience** using Chrome on devices running Android version 4.2 or later.

- **Email improvements**, including a new single-line view of the Inbox with an optimized reading pane, archiving, emojis, and the ability to undo mailbox actions like deleting a message or moving a message.

- **Contact linking** the ability for users to add contacts from their LinkedIn accounts.

- **Calendar** has an updated look and new features, including email reminders for Calendar events, ability to propose a new time in meeting invitations, improved search, and birthday calendars.

- **Search suggestions and refiners** for an improved search experience that helps users find the information they want, faster. Search suggestions try to anticipate what the user's looking for and returns results that might be what the user is looking for. Search refiners will help a user more easily find the information they're looking for by providing contextually-aware filters. Filters might include date ranges, related senders, and so on.

- **New themes** Thirteen new themes with graphic designs.

- **Options** for individual mailboxes have been overhauled.

- **Link preview** which enables users to paste a link into messages, and Outlook on the web automatically generates a rich preview to give recipients a peek into the contents of the destination. This works with video links as well.

- **Inline video** player saves the user time by keeping them in the context of their conversations. An inline preview of a video automatically appears after inserting a video URL.

- **Pins and Flags** which allow users to keep essential emails at the top of their inbox (Pins) and mark others for follow-up (Flags). Pins are now folder specific, great for anyone who uses folders to organize their email. Quickly find and manage flagged items with inbox filters or the new Task module, accessible from the app launcher.

- **Performance improvements** in a number of areas across Outlook on the web, including creating calendar events, composing, loading messages in the reading pane, popouts, search, startup, and switching folders.

- **New Outlook on the web action pane** that allows you to quickly click those actions you most commonly use such as New, Reply all, and Delete. A few new actions have been added as well including Archive, Sweep, and Undo.

#### MAPI over HTTP

MAPI over HTTP is now the default protocol that Outlook uses to communicate with Exchange. MAPI over HTTP improves the reliability and stability of the Outlook and Exchange connections by moving the transport layer to the industry-standard HTTP model. This allows a higher level of visibility of transport errors and enhanced recoverability. Additional functionality includes support for an explicit pause-and-resume function, which enables supported clients to change networks or resume from hibernation while maintaining the same server context.

 **Note**: MAPI over HTTP isn't enabled in organizations where the following conditions are both true:

- You're installing Exchange 2019 in an organization that already has Exchange 2013 servers installed.

- MAPI over HTTP wasn't enabled in Exchange 2013.

While MAPI over HTTP is now the default communication protocol between Outlook and Exchange, clients that don't support it will fall back to Outlook Anywhere (RPC over HTTP).

For more information, see [MAPI over HTTP in Exchange Server](../clients/mapi-over-http/mapi-over-http.md).

#### Document collaboration

Exchange 2019, along with SharePoint Server 2019, enables Outlook on the web users to link to and share documents that are stored in OneDrive for Business in an on-premises SharePoint server instead of attaching files to messages. Users in an on-premises environment can collaborate on files in the same manner that's used in Office 365.

For more information about SharePoint Server 2019, see [New and improved features in SharePoint Server 2019](https://docs.microsoft.com/SharePoint/what-s-new/new-and-improved-features-in-sharepoint-server-2019).

When an Exchange 2019 user receives a Word, Excel, or PowerPoint file in an email attachment, and the file is stored in OneDrive for Business or on-premises SharePoint, the user will now have the option of viewing and editing that file in Outlook on the web alongside the message. To do this, you'll need a separate computer in your on-premises organization that's running Office Online Server. For more information, see [Install Office Online Server in an Exchange organization](../plan-and-deploy/install-office-online-server.md).

Exchange 2019 also brings the following improvements to document collaboration:

- Saving files to OneDrive for Business.

- Uploading a file to OneDrive for Business.

- Most Recently Used lists populated with both local and online files.

### Office 365 hybrid

The Hybrid Configuration Wizard (HCW) that was included with Exchange 2013 is moving to become a cloud-based application. When you choose to configure a hybrid deployment in Exchange 2019, you'll be prompted to download and install the wizard as a small app. The wizard will function the same in previous versions of Exchange, with a few new benefits:

- The wizard can be updated quickly to support changes in the Office 365 service.

- The wizard can be updated to account for issues detected when customers try to configure a hybrid deployment.

- Improved troubleshooting and diagnostics to help you resolve issues that you run into when running the wizard.

- The same wizard will be used by everyone configuring a hybrid deployment who's running Exchange 2013 or later.

In addition to Hybrid Configuration Wizard improvements, multi-forest hybrid deployments are being simplified with Azure Active Directory Connect (AADConnect). AADConnect introduces management agents that will make it significantly easier to synchronize multiple on-premises Active Directory forests with a single Office 365 organization. For more information about AADConnect, see [What is hybrid identity with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

Exchange ActiveSync clients will be seamlessly redirected to Office 365 when a user's mailbox is moved to Exchange Online. To support this, ActiveSync clients need to support HTTP 451 redirect. When a client is redirected, the profile on the device is updated with the URL of the Exchange Online service. This means the client will no longer attempt to contact the on-premises Exchange server when trying to find the mailbox.

### Messaging policy and compliance

There are several new and updated message policy and compliance features in Exchange 2019.

#### Data loss prevention

To comply with business standards and industry regulations, organizations need to protect sensitive information and prevent its inadvertent disclosure. Examples of sensitive information that you might want to prevent from leaking outside your organization include credit card numbers, social security numbers, health records, or other personally identifiable information (PII). With a DLP policy and mail flow rules (also known as transport rules) in Exchange 2019, you can now identify, monitor, and protect 80 different types of sensitive information with new conditions and actions:

- With the new condition **Any attachment has these properties, including any of these words**, a mail flow rule can match messages where the specified property of the attached Office document contains specified words. This condition makes it easy to integrate your Exchange mail flow rules and DLP policies with SharePoint, Windows Server 2012 R2 File Classification Infrastructure (FCI), or a third-party classification system.

- With the new action **Notify the recipient with a message**, a mail flow rule can send a notification to the recipient with the text you specify. For example, you can inform the recipient that the message was rejected by a mail flow rule, or that it was marked as spam and will be delivered to their Junk Email folder.

- The action **Generate incident report and send it to** has been updated to enable the notification of multiple recipients by allowing a group address to be configured as the recipient.

To learn more about DLP, see [Data loss prevention in Exchange Server](../policy-and-compliance/data-loss-prevention/data-loss-prevention.md).

#### In-place Archiving, retention, and eDiscovery

Exchange 2019 includes the following improvements to In-Place Archiving, retention, and eDiscovery to help your organization meet its compliance needs:

- **Public folder support for In-Place eDiscovery and In-Place Hold**: Exchange 2019 integrates public folders into the In-Place eDiscovery and Hold workflow. You can use In-Place eDiscovery to search public folders in your organization, and you can put an In-Place Hold on public folders. And similar to placing a mailbox on hold, you can place a query-based and a time-based hold on public folders. Currently, you can only search and place a hold on all public folders. In later releases, you'll be able to choose specific public folders to search and place on hold. For more information, see [Search and place a hold on public folders using In-Place eDiscovery](../policy-and-compliance/ediscovery/search-public-folders.md).

- **Compliance Search**: Compliance Search is a new eDiscovery search tool in Exchange 2019 with new and improved scaling and performance capabilities. You can use it to search very large numbers of mailboxes in a single search. In fact, there's no limit on the number of mailboxes that can be included in a single search, so you can search all mailboxes in your organization at once. There's also no limit on the number of searches that can run at the same time. For In-Place eDiscovery in Exchange 2019, the limits are the same as in Exchange 2013: you can search up to 10,000 mailboxes in a single search and your organization can run a maximum of two In-Place eDiscovery searches at the same time.

  In Exchange 2019, Compliance Search is only available by using the Exchange Management Shell. For information about using the Compliance Search cmdlets, see the following topics:

  - [Get-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/get-compliancesearch)

  - [New-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/new-compliancesearch)

  - [Remove-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/remove-compliancesearch)

  - [Set-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/set-compliancesearch)

  - [Start-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/start-compliancesearch)

  - [Stop-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/stop-compliancesearch)

  > [!NOTE]
  > To have access to the Compliance Search cmdlets, an administrator or eDiscovery manager must be assigned the Mailbox Search management role or be a member of the Discovery Management role group.

For more information, see [Messaging policy and compliance in Exchange Server](../policy-and-compliance/policy-and-compliance.md).

#### Improved performance and scalability

In Exchange 2019, the search architecture has been redesigned. Previously, search was a synchronous operation that was not very fault-tolerant. The new architecture is asynchronous and decentralized. It distributes the work across multiple servers and keeps retrying if any servers are too busy. This means that we can return results more reliability, and faster.

Another advantage of the new architecture is that search scalability is improved. The number of mailboxes you can search at once using the console has increased from 5k to 10k for both mailboxes and archive mailboxes, allowing you to search a total of 20k mailboxes at the same time.
::: moniker-end

::: moniker range="exchserver-2016"
Microsoft Exchange Server 2016 brings a new set of technologies, features, and services to Exchange Server, the messaging platform that provides email, scheduling, and tools for custom collaboration and messaging service applications. Its goal is to support people and organizations as their work habits evolve from a communication focus to a collaboration focus. At the same time, Exchange 2016 helps lower the total cost of ownership whether you deploy Exchange 2016 on-premises or provision your mailboxes in the cloud.

Choose the section below that matches the version of Exchange that you're upgrading from. If you want to know about features that have been removed or replaced in Exchange 2016, see [What's discontinued in Exchange Server](discontinued-features.md).

For more information about deploying Exchange 2016, see [Planning and deployment](../plan-and-deploy/plan-and-deploy.md).

## What's new when updating from Exchange 2016 RTM to Exchange 2016 CU1?

When you update to Exchange 2016 Cumulative Update 1 (CU1) from Exchange 2016 RTM, you'll get the following new features:

- **SHA-2 compliant S/MIME in Outlook on the web**: We've updated the certificate that's used by the S/MIME control in Outlook on the web. The certificate is now SHA-2 compliant. Users who downloaded the control from Exchange 2016 RTM will need to download the control again after you install CU1.

- **Additional languages for Outlook on the web**: With CU1, we're adding 17 new languages to Outlook on the web.

- **Improved download package**: Exchange 2016 releases, starting with CU1, are now packaged as ISO files instead of a self-extracting EXE file. The ISO file can be mounted directly in Windows Server 2012 or later. If you need to install Exchange across a network, you can create a network share from the mounted ISO drive.

## What's new when upgrading from Exchange 2013 to Exchange 2016 RTM?

[Exchange 2016 architecture](new-features.md#Architecture2013)

[Clients](new-features.md#Clientsfrom2013)

[Outlook on the web (formerly Outlook Web App)](new-features.md#OutlookAppfrom2013)

[MAPI over HTTP](new-features.md#MAPIHTTP2013)

[Document collaboration](new-features.md#DocCollab2013)

[Office 365 hybrid](new-features.md#O3652013)

[Messaging policy and compliance](new-features.md#Compliance2013)

[Data loss prevention](new-features.md#DLP2013)

[In-place Archiving, retention, and eDiscovery](new-features.md#Archiving2013)

### Exchange 2016 architecture
<a name="Architecture2013"> </a>

Today, CPU horsepower is significantly less expensive and is no longer a constraining factor. With that constraint lifted, the primary design goal for Exchange 2016 is for simplicity of scale, hardware utilization, and failure isolation. With Exchange 2016, we reduced the number of server roles to two: the Mailbox and Edge Transport server roles.

The Mailbox server in Exchange 2016 includes all of the server components from the Exchange 2013 Mailbox and Client Access server roles:

- Client Access services provide authentication, limited redirection, and proxy services. Client Access services don't do any data rendering and offer all the usual client access protocols: HTTP, POP and IMAP, and SMTP.

- Mailbox services include all the traditional server components found in the Exchange 2013 Mailbox server role: the backend client access protocols, Transport service, Mailbox databases, and Unified Messaging. The Mailbox server handles all activity for the active mailboxes on that server.

 The Edge Transport role is typically deployed in your perimeter network, outside your internal Active Directory forest, and is designed to minimize the attack surface of your Exchange deployment. By handling all Internet-facing mail flow, it also adds additional layers of message protection and security against viruses and spam, and can apply mail flow rules (also known as transport rules) to control message flow.

For more information about the Exchange 2016 architecture, see [Exchange 2016 architecture](../architecture/architecture.md).

Along with the new Mailbox role, Exchange 2016 now allows you to proxy traffic from Exchange 2013 Client Access servers to Exchange 2016 mailboxes. This new flexibility gives you more control in how you move to Exchange 2016 without having to worry about deploying enough front-end capacity to service new Exchange 2016 servers.

### Clients
<a name="Clientsfrom2013"> </a>

#### Outlook on the web (formerly Outlook Web App)
<a name="OutlookAppfrom2013"> </a>

Outlook Web App is now known as Outlook on the web, which continues to let users access their Exchange mailbox from almost any web browser.

> [!NOTE]
> Supported Web browsers for Outlook on the web in Exchange 2016 are Microsoft Edge, Internet Explorer 11, and the most recent versions of Mozilla Firefox, Google Chrome, and Apple Safari.

The former Outlook Web App user interface has been updated and optimized for tablets and smart phones, in addition to desktop and laptop computers. New Exchange 2016 features include:

- **Platform-specific experiences for phones** for both iOS and Android.

- **Premium Android experience** using Chrome on devices running Android version 4.2 or later.

- **Email improvements**, including a new single-line view of the Inbox with an optimized reading pane, archiving, emojis, and the ability to undo mailbox actions like deleting a message or moving a message.

- **Contact linking** the ability for users to add contacts from their LinkedIn accounts.

- **Calendar** has an updated look and new features, including email reminders for Calendar events, ability to propose a new time in meeting invitations, improved search, and birthday calendars.

- **Search suggestions and refiners** for an improved search experience that helps users find the information they want, faster. Search suggestions try to anticipate what the user's looking for and returns results that might be what the user is looking for. Search refiners will help a user more easily find the information they're looking for by providing contextually-aware filters. Filters might include date ranges, related senders, and so on.

- **New themes** Thirteen new themes with graphic designs.

- **Options** for individual mailboxes have been overhauled.

- **Link preview** which enables users to paste a link into messages, and Outlook on the web automatically generates a rich preview to give recipients a peek into the contents of the destination. This works with video links as well.

- **Inline video** player saves the user time by keeping them in the context of their conversations. An inline preview of a video automatically appears after inserting a video URL.

- **Pins and Flags** which allow users to keep essential emails at the top of their inbox (Pins) and mark others for follow-up (Flags). Pins are now folder specific, great for anyone who uses folders to organize their email. Quickly find and manage flagged items with inbox filters or the new Task module, accessible from the app launcher.

- **Performance improvements** in a number of areas across Outlook on the web, including creating calendar events, composing, loading messages in the reading pane, popouts, search, startup, and switching folders.

- **New Outlook on the web action pane** that allows you to quickly click those actions you most commonly use such as New, Reply all, and Delete. A few new actions have been added as well including Archive, Sweep, and Undo.

#### MAPI over HTTP
<a name="MAPIHTTP2013"> </a>

MAPI over HTTP is now the default protocol that Outlook uses to communicate with Exchange. MAPI over HTTP improves the reliability and stability of the Outlook and Exchange connections by moving the transport layer to the industry-standard HTTP model. This allows a higher level of visibility of transport errors and enhanced recoverability. Additional functionality includes support for an explicit pause-and-resume function, which enables supported clients to change networks or resume from hibernation while maintaining the same server context.

 **Note**: MAPI over HTTP isn't enabled in organizations where the following conditions are both true:

- You're installing Exchange 2016 in an organization that already has Exchange 2013 servers installed.

- MAPI over HTTP wasn't enabled in Exchange 2013.

While MAPI over HTTP is now the default communication protocol between Outlook and Exchange, clients that don't support it will fall back to Outlook Anywhere (RPC over HTTP).

For more information, see [MAPI over HTTP in Exchange 2016](../clients/mapi-over-http/mapi-over-http.md).

#### Document collaboration
<a name="DocCollab2013"> </a>

Exchange 2016, along with SharePoint Server 2016, enables Outlook on the web users to link to and share documents that are stored in OneDrive for Business in an on-premises SharePoint server instead of attaching files to messages. Users in an on-premises environment can collaborate on files in the same manner that's used in Office 365.

For more information about SharePoint Server 2016, see [New and improved features in SharePoint Server 2016](https://docs.microsoft.com/SharePoint/what-s-new/new-and-improved-features-in-sharepoint-server-2016).

When an Exchange 2016 user receives a Word, Excel, or PowerPoint file in an email attachment, and the file is stored in OneDrive for Business or on-premises SharePoint, the user will now have the option of viewing and editing that file in Outlook on the web alongside the message. To do this, you'll need a separate computer in your on-premises organization that's running Office Online Server. For more information, see [Install Office Online Server in an Exchange 2016 organization](../plan-and-deploy/install-office-online-server.md).

Exchange 2016 also brings the following improvements to document collaboration:

- Saving files to OneDrive for Business.

- Uploading a file to OneDrive for Business.

- Most Recently Used lists populated with both local and online files.

### Office 365 hybrid
<a name="O3652013"> </a>

The Hybrid Configuration Wizard (HCW) that was included with Exchange 2013 is moving to become a cloud-based application. When you choose to configure a hybrid deployment in Exchange 2016, you'll be prompted to download and install the wizard as a small app. The wizard will function the same in previous versions of Exchange, with a few new benefits:

- The wizard can be updated quickly to support changes in the Office 365 service.

- The wizard can be updated to account for issues detected when customers try to configure a hybrid deployment.

- Improved troubleshooting and diagnostics to help you resolve issues that you run into when running the wizard.

- The same wizard will be used by everyone configuring a hybrid deployment who's running Exchange 2013 or Exchange 2016.

In addition to Hybrid Configuration Wizard improvements, multi-forest hybrid deployments are being simplified with Azure Active Directory Connect (AADConnect). AADConnect introduces management agents that will make it significantly easier to synchronize multiple on-premises Active Directory forests with a single Office 365 organization. For more information about AADConnect, see [What is hybrid identity with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

Exchange ActiveSync clients will be seamlessly redirected to Office 365 when a user's mailbox is moved to Exchange Online. To support this, ActiveSync clients need to support HTTP 451 redirect. When a client is redirected, the profile on the device is updated with the URL of the Exchange Online service. This means the client will no longer attempt to contact the on-premises Exchange server when trying to find the mailbox.

### Messaging policy and compliance
<a name="Compliance2013"> </a>

There are several new and updated message policy and compliance features in Exchange 2016.

#### Data loss prevention
<a name="DLP2013"> </a>

To comply with business standards and industry regulations, organizations need to protect sensitive information and prevent its inadvertent disclosure. Examples of sensitive information that you might want to prevent from leaking outside your organization include credit card numbers, social security numbers, health records, or other personally identifiable information (PII). With a DLP policy and mail flow rules (also known as transport rules) in Exchange 2016, you can now identify, monitor, and protect 80 different types of sensitive information with new conditions and actions:

- With the new condition **Any attachment has these properties, including any of these words**, a mail flow rule can match messages where the specified property of the attached Office document contains specified words. This condition makes it easy to integrate your Exchange mail flow rules and DLP policies with SharePoint, Windows Server 2012 R2 File Classification Infrastructure (FCI), or a third-party classification system.

- With the new action **Notify the recipient with a message**, a mail flow rule can send a notification to the recipient with the text you specify. For example, you can inform the recipient that the message was rejected by a mail flow rule, or that it was marked as spam and will be delivered to their Junk Email folder.

- The action **Generate incident report and send it to** has been updated to enable the notification of multiple recipients by allowing a group address to be configured as the recipient.

To learn more about DLP, see [Data loss prevention in Exchange 2016](../policy-and-compliance/data-loss-prevention/data-loss-prevention.md).

#### In-place Archiving, retention, and eDiscovery
<a name="Archiving2013"> </a>

Exchange 2016 includes the following improvements to In-Place Archiving, retention, and eDiscovery to help your organization meet its compliance needs:

- **Public folder support for In-Place eDiscovery and In-Place Hold**: Exchange 2016 integrates public folders into the In-Place eDiscovery and Hold workflow. You can use In-Place eDiscovery to search public folders in your organization, and you can put an In-Place Hold on public folders. And similar to placing a mailbox on hold, you can place a query-based and a time-based hold on public folders. Currently, you can only search and place a hold on all public folders. In later releases, you'll be able to choose specific public folders to search and place on hold. For more information, see [Search and place a hold on public folders using In-Place eDiscovery](../policy-and-compliance/ediscovery/search-public-folders.md).

- **Compliance Search**: Compliance Search is a new eDiscovery search tool in Exchange 2016 with new and improved scaling and performance capabilities. You can use it to search very large numbers of mailboxes in a single search. In fact, there's no limit on the number of mailboxes that can be included in a single search, so you can search all mailboxes in your organization at once. There's also no limit on the number of searches that can run at the same time. For In-Place eDiscovery in Exchange 2016, the limits are the same as in Exchange 2013: you can search up to 10,000 mailboxes in a single search and your organization can run a maximum of two In-Place eDiscovery searches at the same time.

    In Exchange 2016, Compliance Search is only available by using the Exchange Management Shell. For information about using the Compliance Search cmdlets, see the following topics:

  - [Get-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/get-compliancesearch)

  - [New-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/new-compliancesearch)

  - [Remove-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/remove-compliancesearch)

  - [Set-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/set-compliancesearch)

  - [Start-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/start-compliancesearch)

  - [Stop-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/stop-compliancesearch)

    > [!NOTE]
    > To have access to the Compliance Search cmdlets, an administrator or eDiscovery manager must be assigned the Mailbox Search management role or be a member of the Discovery Management role group.

For more information, see [Messaging policy and compliance in Exchange 2016](../policy-and-compliance/policy-and-compliance.md).

#### Improved performance and scalability
<a name="Archiving2013"> </a>

In Exchange 2016, the search architecture has been redesigned. Previously, search was a synchronous operation that was not very fault-tolerant. The new architecture is asynchronous and decentralized. It distributes the work across multiple servers and keeps retrying if any servers are too busy. This means that we can return results more reliability, and faster.

Another advantage of the new architecture is that search scalability is improved. The number of mailboxes you can search at once using the console has increased from 5k to 10k for both mailboxes and archive mailboxes, allowing you to search a total of 20k mailboxes at the same time.

## What's new when upgrading from Exchange 2010 to Exchange 2016 RTM?
<a name="WhatsNew2010"> </a>

[Exchange admin center](new-features.md#EAC2010)

[Exchange 2016 architecture](new-features.md#Architecture2010)

[Setup](new-features.md#Setup2010)

[Office 365 hybrid](new-features.md#O3652010)

[Messaging policy and compliance](new-features.md#Compliance2010)

[Antimalware protection](new-features.md#Malware2010)

[Mail flow and the transport pipeline](new-features.md#Mail2010)

[Sharing and collaboration](new-features.md#Sharing2010)

[Integration with SharePoint and Skype for Business](new-features.md#Integrate2010)

[Clients](new-features.md#Clientsfrom2010)

[Batch mailbox moves](new-features.md#Batch2010)

[High availability and site resilience](new-features.md#HA2010)

[Exchange workload management](new-features.md#WLM2010)

### Exchange admin center
<a name="EAC2010"> </a>

Exchange 2016 provides a single unified management console that allows for ease of use and is optimized for management of on-premises, online, or hybrid deployments. The *Exchange admin center* (EAC) in Exchange 2016 replaces the Exchange Management Console (EMC) and the Exchange Control Panel (ECP) that were used in Exchange 2010 (but the name of the EAC virtual directory is still "ECP"). Some EAC features include:

- **List view**: The list view in EAC has been designed to remove key limitations that existed in ECP. ECP was limited to displaying up to 500 objects and, if you wanted to view objects that weren't listed in the details pane, you needed to use searching and filtering to find those specific objects. In Exchange 2016, the viewable limit from within the EAC list view is approximately 20,000 objects. After the EAC returns the results, the EAC client performs the searching and sorting, which greatly increases the performance compared to the ECP in Exchange 2010. In addition, paging has been added so that you can page to the results. You can also configure page size and export to a .csv file.

- **Add/Remove columns to the Recipient list view**: You can choose which columns to view, and with local cookies, you can save your custom list views per machine that you use to access the EAC.

- **Secure the ECP virtual directory**: You can control access to the EAC from inside and outside your corporate network without affecting user access to their Outlook on the web options. For more information, see [Turn off access to the Exchange admin center](../architecture/client-access/disable-exchange-admin-center-access.md).

- **Tool consolidation**: The functionality of these management tools has been integrated into the EAC:

  - The Public Folder administration console.

  - The Role Based Access Control (RBAC) User Editor in the Exchange Toolbox.

  - The Call Statistics and User Call Logs tools for Unified Messaging.

- **Notifications**: In Exchange 2016, the EAC now has a Notification viewer so that you can view notifications and alerts for:

  - Exchange certificates that are installed on any Exchange 2016 server in your organization that are expired or within 30 days of expiring.

  - Mailbox moves and migrations (Mailbox Replication Service or MRS tasks). Notifications are displayed when migrations are started, in process, and completed.

     You can also configure email addresses to receive these notifications in the EAC, or all events by using the **Set-Notification** cmdlet in the Exchange Management Shell.

- **Groups enhancements**: By default, up to 500 recipients are returned when you open the **Select Members** window, however, you can choose to list up to 10,000 recipients by clicking **Get All Results** beneath the recipient list. We now support browsing more than 500 recipients by using the scroll bar, and we've also added enhanced search features so you can filter recipients in the recipient list. You can filter by:

  - City

  - Company

  - Country/region

  - Department

  - Office

  - Title

- **Delivery reports**: Administrators can use the EAC to track delivery information for email messages sent to or received by any user in the organization. You just select a mailbox, and then search for messages sent to or received by a different user. You can filter the search by words in the subject line. The results track a message through the delivery process, and indicate whether the message was successfully delivered, is pending delivery, or wasn't delivered. For more information, see [Track messages with delivery reports](../mail-flow/transport-logs/track-messages-with-delivery-reports.md).

- **Certificate management**: Administrators can use the EAC to manage Exchange certificates on multiple servers from a central location, which helps to minimize the amount of interaction that's required to manage Exchange certificates. For more information about certificate management procedures in Exchange 2016, see [Certificate procedures in Exchange 2016](../architecture/client-access/certificate-procedures.md).

For more information about the EAC, see [Exchange admin center in Exchange 2016](../architecture/client-access/exchange-admin-center.md).

### Exchange 2016 architecture
<a name="Architecture2010"> </a>

Today, CPU horsepower is significantly less expensive and is no longer a constraining factor. With that constraint lifted, the primary design goal for Exchange 2016 is for simplicity of scale, hardware utilization, and failure isolation. With Exchange 2016, we reduced the number of server roles to two: the Mailbox and Edge Transport server roles.

The Mailbox server in Exchange 2016 includes all of the server components from the Mailbox, Client Access, Hub Transport, and Unified Messaging server roles in Exchange 2010:

- Client Access services provide authentication, limited redirection, and proxy services. Client Access services don't do any data rendering and offer all the usual client access protocols: HTTP, POP and IMAP, and SMTP.

- Mailbox services include: the backend client access protocols, Transport service, Mailbox databases, and Unified Messaging. The Mailbox server handles all activity for the active mailboxes on that server.

Like previous versions of Exchange. the Edge Transport role is typically deployed in your perimeter network, outside your internal Active Directory forest, and is designed to minimize the attack surface of your Exchange deployment. By handling all Internet-facing mail flow, it also adds additional layers of message protection and security against viruses and spam, and can apply mail flow rules (also known as transport rules) to control message flow.

The Exchange 2016 architecture provides the following benefits:

- **Version upgrade flexibility**: No more rigid upgrade requirements. Mailbox servers can be upgraded independently and in any order in relation to other Mailbox servers.

- **Session indifference**: With Exchange 2010, session affinity to the Client Access server role was required for several protocols. In Exchange 2016, the client access and mailbox components reside on the same Mailbox server. No session affinity is required between Mailbox servers, Edge Transport servers, or mail servers on the Internet. This allows inbound client connections to Mailbox servers to be balanced using techniques provided by load-balancing technology like least connection or round-robin.

- **Deployment simplicity**: With an Exchange 2010 site-resilient design, you needed up to eight different namespaces: two Internet Protocol namespaces, two for Outlook Web App fallback, one for Autodiscover, two for RPC Client Access, and one for SMTP. With Exchange 2016, the most organizations only need two namespaces for coexistence with Exchange 2010: one for client protocols and one for Autodiscover. Depending on how you configure your mail routing, you might also need an additional namespace for SMTP routing.

For more information about the Exchange 2016 architecture, see [Exchange 2016 architecture](../architecture/architecture.md).

### Setup
<a name="Setup2010"> </a>

Setup has been completely rewritten so that installing Exchange 2016 and making sure you've got the latest product rollups and security fixes is easier than ever. Here are some of the improvements we've made:

- **Improved readiness checks**: Readiness checks make sure that your computer and your organization are ready for Exchange 2016. After you've provided the necessary information about your installation to Setup, the readiness checks are run before installation begins. The new readiness check engine now runs through all checks before reporting back to you on what actions need to be performed before Setup can continue, and it does so faster than ever. As with previous versions of Exchange, you can tell Setup to install the Windows features that are required by Setup so you don't have to install them manually.

- **Simplified and modern wizard**: We've removed all the steps in the Setup wizard that aren't absolutely required for you to install Exchange. What's left is an easy-to-follow wizard that takes you through the installation process one step at a time.

For more information, see [Planning and deployment](../plan-and-deploy/plan-and-deploy.md).

### Office 365 hybrid
<a name="O3652010"> </a>

The Hybrid Configuration Wizard (HCW) that was included with Exchange 2013 is moving to become a cloud-based application. When you choose to configure a hybrid deployment in Exchange 2016, you'll be prompted to download and install the wizard as a small app. The wizard will function the same in previous versions of Exchange, with a few new benefits:

- The wizard can be updated quickly to support changes in the Office 365 service.

- The wizard can be updated to account for issues that are encountered when customers try to configure a hybrid deployment.

- Improved troubleshooting and diagnostics to help you resolve issues that you encounter.

- The same wizard will be used by everyone who's configuring a hybrid deployment with Exchange 2013 or Exchange 2016.

In addition to Hybrid Configuration Wizard improvements, multi-forest hybrid deployments are being simplified with Azure Active Directory Connect (AADConnect). AADConnect introduces management agents that will make it significantly easier to synchronize multiple on-premises Active Directory forests with a single Office 365 organization.

Exchange ActiveSync clients will be seamlessly redirected to Office 365 when a user's mailbox is moved to Exchange Online. To support this, ActiveSync clients need to support HTTP 451 redirect. When a client is redirected, the profile on the device is updated with the URL of the Exchange Online service. This means the client will no longer attempt to contact the on-premises Exchange server when trying to find the mailbox.

### Messaging policy and compliance
<a name="Compliance2010"> </a>

There are several new and updated message policy and compliance features in Exchange 2016.

#### Data loss prevention
<a name="DLP2010"> </a>

Data loss prevention (DLP) capabilities help you protect your sensitive data and inform users of internal compliance policies. DLP can also help keep your organization safe from users who might mistakenly send sensitive information to unauthorized people. DLP helps you identify, monitor, and protect sensitive data through deep content analysis. Exchange 2016 offers built-in DLP policies based on regulatory standards such as personally identifiable information (PII) and payment card industry data security standards (PCI), and is extensible to support other policies important to your business. With a DLP policy in Exchange 2016, you can now identify, monitor, and protect 80 different types of sensitive information. For more information, see [Sensitive information types in Exchange 2016](../policy-and-compliance/data-loss-prevention/sensitive-information-types.md). Additionally, the new policy tips in Outlook 2016 inform users about policy violations before sensitive data is sent.

To learn more, see [Data loss prevention in Exchange 2016](../policy-and-compliance/data-loss-prevention/data-loss-prevention.md)

#### Mail flow rules (transport rules)

You can use Exchange mail flow rules (also known as transport rules) to look for specific conditions in messages that pass through your organization and take action on them. For example, your organization might require that certain types of messages are blocked or rejected in order to meet legal or compliance requirements, or to implement specific business needs. Mail flow rules are similar to the Inbox rules that are available in Outlook. The main difference between mail flow rules and Inbox rules is that mail flow rules take action on messages while they're in transit as opposed to after the message is delivered. Mail flow rules also contain a richer set of conditions, exceptions, and actions, which gives you the flexibility to implement many types of messaging policies.

These features are new to mail flow rules in Exchange 2016:

- Exchange mail flow rules can now identify 80 different types of sensitive information, including 30 new sensitive information types focusing on identifiers from South America, Europe, and Asia. These 80 built-in types are included, but you can also develop your own type from scratch. For more information on these sensitive information types, see [Sensitive information types in Exchange 2016](../policy-and-compliance/data-loss-prevention/sensitive-information-types.md).

- With the new condition **Any attachment has these properties, including any of these words**, a mail flow rule can match messages where the specified property of the attached Office document contains specified words. This condition makes it easy to integrate your Exchange mail flow rules and DLP policies with SharePoint, Windows Server 2012 R2 File Classification Infrastructure (FCI), or a third-party classification system.

- With the new action **Notify the recipient with a message**, a mail flow rule can send a notification to the recipient with the text you specify. For example, you can inform the recipient that the message was rejected by a mail flow rule, or that it was marked as spam and will be delivered to their Junk Email folder.

- The action **Generate incident report and send it to** has been updated to enable the notification of multiple recipients by allowing a group address to be configured as the recipient

- Additional mail flow rules predicates and actions.

For more information, see [Mail flow rules in Exchange 2016](../policy-and-compliance/mail-flow-rules/mail-flow-rules.md).

#### Azure Rights Management connector
<a name="MRM2010"> </a>

The Azure Rights Management connector (also known as the Microsoft Rights Management connector or RMS connector) is an optional application that helps you enhance data protection for your Exchange 2016 server by connecting to the cloud-based Azure Rights Management service (also known as Microsoft Rights Management or Azure RMS). Once you install the RMS connector, it provides continuous data protection throughout the life span of the information and because these services are customizable, you can define the level of protection you need. For example, you can limit email message access to specific users or set view-only rights for certain messages.

For more information, see [Deploying the Azure Rights Management connector](https://docs.microsoft.com/azure/information-protection/deploy-rms-connector).

#### In-place Archiving, retention, and eDiscovery
<a name="Archiving2010"> </a>

Exchange 2016 includes the following improvements to In-Place Archiving, retention, and eDiscovery to help your organization meet its compliance needs:

- **In-Place Hold**: In-Place Hold is a unified hold model that allows you to meet legal hold requirements in the following scenarios:

  - Preserve the results of the query (query-based hold), which allows for scoped immutability across mailboxes.

  - Place a time-based hold to meet retention requirements (for example, retain all items in a mailbox for seven years, a scenario that required the use of Single Item Recovery/Deleted Item Retention in Exchange 2010).

  - Place a mailbox on indefinite hold (similar to litigation hold in Exchange 2010).

  - Place a user on multiple holds to meet different case requirements.

- **In-Place eDiscovery**: In-Place eDiscovery allows authorized users to search mailbox data across all mailboxes and In-Place Archives in an Exchange 2016 organization and copy messages to a discovery mailbox for review. In Exchange 2016, In-Place eDiscovery allows discovery managers to perform more efficient searches and hold.

  - **Federated search** allows you to search and preserve data across multiple data repositories. With Exchange 2016, you can perform in-place eDiscovery searches across Exchange, SharePoint, and Skype for Business. You can use the eDiscovery Center in SharePoint 2013 to perform In-Place eDiscovery search and hold.

  - **Query-based In-Place Hold** allows you to save the results of the query, which allows for scoped immutability across mailboxes.

  - **Export search results** Discovery Managers can export mailbox content to a .pst file from the SharePoint 2013 eDiscovery Console. Mailbox export request cmdlets are no longer required to export a mailbox to a .pst file.

  - **Keyword statistics**: Search statistics are offered on a per search term basis. This enables a Discovery Manager to quickly make intelligent decisions about how to further refine the search query to provide better results. eDiscovery search results are sorted by relevance.

  - **KQL syntax**: Discovery Managers can use Keyword Query Language (KQL) syntax in search queries. KQL is similar to the Advanced Query Syntax (AQS), that was used for discovery searches in Exchange 2010.

  - **In-Place eDiscovery and Hold wizard**: Discovery Managers can use the In-Place eDiscovery and Hold wizard to perform eDiscovery and hold operations.

    > [!NOTE]
    > If SharePoint 2013 isn't available, a subset of the eDiscovery functionality is available in the Exchange admin center.

  - **Public folder support for In-Place eDiscovery and In-Place Hold**: Exchange 2016 has integrated public folders into the In-Place eDiscovery and Hold workflow. You can use In-Place eDiscovery to search public folders in your organization, and you can put a In-Place Hold on public folders. And similar to placing a mailbox on hold, you can place a query-based and a time-based hold on public folders. Currently, you can only search and place a hold on all public folders. In later releases, you'll be able to choose specific public folders to search and place on hold. For more information, see [Search and place a hold on public folders using In-Place eDiscovery](../policy-and-compliance/ediscovery/search-public-folders.md).

  - **Compliance Search**: Compliance Search is a new eDiscovery search tool in Exchange 2016 with new and improved scaling and performance capabilities. You can use it to search very large numbers of mailboxes in a single search. In fact, there's no limit on the number of mailboxes that can be included in a single search, so you can search all mailboxes in your organization at once. There's also no limit on the number of searches that can run at the same time. For In-Place eDiscovery in Exchange 2016, the limits are the same as in Exchange 2013: you can search up to 10,000 mailboxes in a single search and your organization can run a maximum of two In-Place eDiscovery searches at the same time.

    In Exchange 2016, Compliance Search is only available by using the Exchange Management Shell. For information about using the Compliance Search cmdlets, see the following topics:

  - [Get-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/get-compliancesearch)

  - [New-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/new-compliancesearch)

  - [Remove-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/remove-compliancesearch)

  - [Set-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/set-compliancesearch)

  - [Start-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/start-compliancesearch)

  - [Stop-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/stop-compliancesearch)

    > [!NOTE]
    > To have access to the Compliance Search cmdlets, an administrator or eDiscovery manager must be assigned the Mailbox Search management role or be a member of the Discovery Management role group.

- **Search across primary and archive mailboxes in Outlook on the web**: Users can search across their primary and archive mailboxes in Outlook on the web. Two separate searches are no longer necessary.

- **Archive Skype for Business content**: Exchange 2016 supports archiving of Skype for Business content in a user's mailbox. You can place Skype for Business content on hold using In-Place Hold and use In-Place eDiscovery to search Skype for Business content archived in Exchange.

For more information, see [Messaging policy and compliance in Exchange 2016](../policy-and-compliance/policy-and-compliance.md).

#### Auditing
<a name="Auditing2010"> </a>

Exchange 2016 includes the following improvements to auditing:

- **Auditing reports**: The EAC includes auditing functionality so that you can run reports or export entries from the mailbox audit log and the administrator audit log. The mailbox audit log records whenever a mailbox is accessed by someone other than the person who owns the mailbox. This can help you determine who has accessed a mailbox and what they have done. The administrator audit log records any action, based on an Exchange Management Shell cmdlet, performed by an administrator. This can help you troubleshoot configuration issues or identify the cause of problems related to security or compliance. For more information, see [Exchange Auditing Reports](https://docs.microsoft.com/Exchange/exchange-auditing-reports-exchange-2013-help).

- **Viewing the administrator audit log**: Instead of exporting the administrator audit log, which can take up to 24 hours to receive in an email message, you can view administrator audit log entries in the EAC. To do this, go to **Compliance Management** \> **Auditing** and click **View the administrator audit log**. Up to 1000 entries will be displayed on multiple pages. To narrow the search, you can specify a date range.

    As an additional improvement, you can also export the audit log data in a format that's common to both Exchange 2016 and SharePoint Server 2016. This makes it easier to integrate with third-party tools to view the data and create richer reports.

    For more information, see [View the Administrator Audit Log](https://docs.microsoft.com/Exchange/view-administrator-audit-log-exchange-2013-help).

### Antimalware protection
<a name="Malware2010"> </a>

The built-in malware filtering capabilities of Exchange 2016 helps protect your network from malicious software that's transferred by email messages. All messages sent or received by your Exchange 2016 Mailbox server are scanned for malware (viruses and spyware) by using the built-in Malware Agent. If malware is detected, the message is deleted. Notifications may also be sent to senders or administrators when an infected message is deleted and not delivered. You can also choose to replace infected attachments with either default or custom messages that notify the recipients of the malware detection.

For more information about antimalware protection, see [Antimalware protection in Exchange 2016](../antispam-and-antimalware/antimalware-protection/antimalware-protection.md).

### Mail flow and the transport pipeline
<a name="Mail2010"> </a>

How messages flow through an organization and what happens to them has changed significantly in Exchange 2016. Following is a brief overview of the changes:

- **Transport pipeline**: The transport pipeline in Exchange 2016 is now made up of several different services: the Front End Transport service, the Transport service, and the Mailbox Transport service. For more information, see [Mail flow and the transport pipeline](../mail-flow/mail-flow.md).

- **Routing**: Mail routing in Exchange 2016 recognizes DAG boundaries as well as Active Directory site boundaries. Also, mail routing has been improved to queue messages more directly for internal recipients. For more information, see [Mail routing](../mail-flow/mail-routing/mail-routing.md).

- **Connectors**: The default maximum message size for a Send connector or a Receive connector has increased from 10MB to 25MB. For more information, see [Connector limits](../mail-flow/message-size-limits.md#connector-limits).

    You can configure Send connectors to route outbound mail through the Front End transport service on Mailbox servers. For more information, see [Configure Send connectors to proxy outbound mail](../mail-flow/connectors/proxy-outbound-mail.md).

### Recipients
<a name="Recipients2010"> </a>

Administrators can now use the EAC to create a *group naming policy*, which lets you standardize and manage the names of distribution groups that are created by users in your organization. You can automatically add a prefix or suffix to the name of the distribution group when it's created, and you can block specific words from being used in group names. For more information, see [Create a Distribution Group Naming Policy](https://docs.microsoft.com/Exchange/create-group-naming-policy-exchange-2013-help).

For more information about recipients in Exchange 2016, see [Recipients](../recipients/recipients.md).

### Sharing and collaboration
<a name="Sharing2010"> </a>

Exchange 2016 includes the following enhancements for sharing and collaboration:

- **Public folders**: Public folders now take advantage of the existing high availability and storage technologies of the mailbox store. The public folder architecture uses specially designed mailboxes to store both the hierarchy and the public folder content. This new design also means that there is no longer a public folder database. Public folder replication now uses the continuous replication model. High availability for the hierarchy and content mailboxes is provided by the database availability group (DAG). With this design, we're moving away from a multi-master replication model to a single-master replication model. For more information, see [Public folders](../collaboration/public-folders/public-folders.md).

- **Shared mailboxes**: In previous versions of Exchange, creating a shared mailbox was a multi-step process in which you had to use the Exchange Management Shell to set the delegate permissions. Now you can create a shared mailbox in one step via the Exchange admin center (EAC). In the EAC, go to **Recipients** \> **Shared** to create a shared mailbox. Shared mailboxes are a recipient type so you can easily search for your shared mailboxes in either the EAC or by using the Exchange Management Shell. For more information, see [Shared mailboxes](../collaboration/shared-mailboxes/shared-mailboxes.md).

### Integration with SharePoint and Skype for Business
<a name="Integrate2010"> </a>

Exchange 2016 offers greater integration with SharePoint and Skype for Business. Benefits of this enhanced integration include:

- Skype for Business Server 2015 can archive content in Exchange 2016 and use Exchange 2016 as a contact store.

- Discovery Managers can perform In-Place eDiscovery and Hold searches across SharePoint, Exchange, and Skype for Business data.

For more information, see [Plan Exchange 2016 integration with SharePoint and Skype for Business](../plan-and-deploy/integration-with-sharepoint-and-skype/integration-with-sharepoint-and-skype.md).

### Clients
<a name="Clientsfrom2010"> </a>

#### Outlook on the web (formerly Outlook Web App)
<a name="OutlookAppfrom2010"> </a>

Outlook Web App is now known as Outlook on the web, which continues to let users access their Exchange mailbox from almost any web browser.

> [!NOTE]
> Supported Web browsers for Outlook on the web in Exchange 2016 are Microsoft Edge, Internet Explorer 11, and the most recent versions of Mozilla Firefox, Google Chrome, and Apple Safari.

In Exchange 2016, the former Outlook Web App user interface is updated and optimized for tablets and smart phones, in addition to desktop and laptop computers. New Exchange 2016 features include:

- **Platform-specific experiences for phones** for both iOS and Android.

- **Premium Android experience** using Chrome on devices running Android version 4.2 or later.

- **Apps for Outlook** which allow users and administrators to extend the capabilities of Outlook on the web.

- **Email improvements**, including a new single-line view of the Inbox with an optimized reading pane, archiving, emojis, and the ability to undo mailbox actions like deleting a message or moving a message.

- **Contact linking** the ability for users to add contacts from their LinkedIn accounts.

- **Calendar** has an updated look and new features, including email reminders for Calendar events, ability to propose a new time in meeting invitations, improved search, and birthday calendars.

- **Search suggestions and refiners** for an improved search experience that helps users find the information they want, faster. Search suggestions try to anticipate what the user's looking for and returns results that might be what the user is looking for. Search refiners will help a user more easily find the information they're looking for by providing contextually-aware filters. Filters might include date ranges, related senders, and so on.

- **Themes** Exchange 2016 provides over 50 built-in themes.

- **Options** for individual mailboxes have been overhauled.

- **Link preview** which enables users to paste a link into messages, and Outlook on the web automatically generates a rich preview to give recipients a peek into the contents of the destination. This works with video links as well.

- **Inline video** player saves the user time by keeping them in the context of their conversations. An inline preview of a video automatically appears after inserting a video URL.

- **Link preview** which enables users to paste a link into messages, and Outlook on the web automatically generates a rich preview to give recipients a peek into the contents of the destination. This works with video links as well.

- **Pins and Flags** which allow users to keep essential emails at the top of their inbox (Pins) and mark others for follow-up (Flags). Pins are now folder specific, great for anyone who uses folders to organize their email. Quickly find and manage flagged items with inbox filters or the new Task module, accessible from the app launcher.

- **Performance improvements** in a number of areas across Outlook on the web, including creating calendar events, composing, loading messages in the reading pane, popouts, search, startup, and switching folders.

- **New Outlook on the web action pane** that allows you to quickly click those actions you most commonly use such as New, Reply all, and Delete. A few new actions have been added as well including Archive, Sweep, and Undo.

#### Offline Outlook on the web
<a name="OutlookAppfrom2010"> </a>

Internet Explorer 11 and Windows Store apps using JavaScript support the Application Cache API (or AppCache) as defined in the HTML5 specification, which allows you to create offline web applications. AppCache enables webpages to cache (or save) resources locally, including images, script libraries, style sheets, and so on. In addition, AppCache allows URLs to be served from cached content using standard Uniform Resource Identifier (URI) notation. The following is a list of the browsers that support AppCache:

- Microsoft Edge

- Internet Explorer 11 or later versions

- Google Chrome 44 or later versions

- Mozilla Firefox 39 or later versions

- Apple Safari 8 or later (only on OS X/iOS) versions

#### MAPI over HTTP
<a name="MAPIHTTP2010"> </a>

MAPI over HTTP is now the default protocol that Outlook uses to communicate with Exchange. MAPI over HTTP improves the reliability and stability of the Outlook and Exchange connections by moving the transport layer to the industry-standard HTTP model. This allows a higher level of visibility of transport errors and enhanced recoverability. Additional functionality includes support for an explicit pause-and-resume function, which enables supported clients to change networks or resume from hibernation while maintaining the same server context.

 **Note**: MAPI over HTTP isn't enabled in organizations where the following conditions are both true:

- You're installing Exchange 2016 in an organization that already has Exchange 2013 servers installed.

- MAPI over HTTP wasn't enabled in Exchange 2013.

While MAPI over HTTP is now the default communication protocol between Outlook and Exchange, clients that don't support it will fall back to Outlook Anywhere (RPC over HTTP). RPC (RPC over TCP) is no longer supported.

For more information, see [MAPI over HTTP in Exchange 2016](../clients/mapi-over-http/mapi-over-http.md).

#### Document collaboration
<a name="DocCollab2010"> </a>

Exchange 2016, along with SharePoint Server 2016, enables Outlook on the web users to link to and share documents that are stored in OneDrive for Business in an on-premises SharePoint server instead of attaching files to messages. Users in an on-premises environment can collaborate on files in the same manner that's used in Office 365.

For more information about SharePoint Server 2016, see [New and improved features in SharePoint Server 2016](https://docs.microsoft.com/SharePoint/what-s-new/new-and-improved-features-in-sharepoint-server-2016).

When an Exchange 2016 user receives a Word, Excel, or PowerPoint file in an email attachment, and the file is stored in OneDrive for Business or on-premises SharePoint, the user will now have the option of viewing and editing that file in Outlook on the web alongside the message. To do this, you'll need a separate computer in your on-premises organization that's running Office Online Server. For more information, see [Install Office Online Server in an Exchange 2016 organization](../plan-and-deploy/install-office-online-server.md).

Exchange 2016 also brings the following improvements to document collaboration:

- Saving files to OneDrive for Business.

- Uploading a file to OneDrive for Business.

- Most Recently Used lists populated with both local and online files.

### Batch mailbox moves
<a name="Batch2010"> </a>

Exchange 2016 makes use of batch moves. The move architecture is built on top of MRS (Mailbox Replication service) moves with enhanced management capability. The batch move architecture features the following enhancements:

- Ability to move multiple mailboxes in large batches.

- Email notification during move with reporting.

- Automatic retry and automatic prioritization of moves.

- Primary and personal archive mailboxes can be moved together or separately.

- Option for manual move request finalization, which allows you to review a move before you complete it.

- Periodic incremental syncs to migrate the changes.

For more information, see [Manage on-premises mailbox moves in Exchange 2016](../architecture/mailbox-servers/manage-mailbox-moves.md).

### High availability and site resilience
<a name="HA2010"> </a>

The high availability model of the mailbox component has not changed significantly since Exchange 2010. The unit of high availability is still the database availability group (DAG). The DAG still uses Windows Server failover clustering. Continuous replication still supports both file mode and block mode replication. However, there have been some improvements. Failover times have been reduced as a result of transaction log code improvements and deeper checkpoint on the passive databases. The Exchange Store service has been re-written in managed code. Now, each database runs under its own process, which isolates store issues to a single database.

Exchange 2016 uses DAGs and mailbox database copies, along with other features such as single item recovery, retention policies, and lagged database copies, to provide high availability, site resilience, and Exchange native data protection. The high availability platform, the Exchange Information Store and the Extensible Storage Engine (ESE), have all been enhanced to provide greater availability, easier management, and to reduce costs. These enhancements include:

- **Managed availability**: With managed availability, internal monitoring and recovery-oriented features are tightly integrated to help prevent failures, proactively restore services, and initiate server failovers automatically or alert administrators to take action. The focus is on monitoring and managing the end user experience rather than just server and component uptime to help keep the service continuously available.

- **Managed Store**: See the [Managed Store](new-features.md#ManagedStore) section.

- **Support for multiple databases per disk**: Exchange 2016 includes enhancements that enable you to support multiple databases (mixtures of active and passive copies) on the same disk, thereby leveraging larger disks in terms of capacity and IOPS as efficiently as possible.

- **Automatic reseed**: Enables you to quickly restore database redundancy after disk failure. If a disk fails, the database copy stored on that disk is copied from the active database copy to a spare disk on the same server. If multiple database copies were stored on the failed disk, they can all be automatically re-seeded on a spare disk. This enables faster reseeds, as the active databases are likely to be on multiple servers and the data is copied in parallel.

- **Automatic recovery from storage failures**: This feature continues the innovation that was introduced in Exchange 2010 to allow the system to recover from failures that affect resiliency or redundancy. In addition to the Exchange 2010 bugcheck behaviors, Exchange 2016 includes additional recovery behaviors for long I/O times, excessive memory consumption by `MSExchangeRepl.exe`, and severe cases where the system is in such a bad state that threads can't be scheduled.

- **Lagged copy enhancements**: Lagged copies can now use automatic log play down to care for themselves (to a certain extent). Lagged copies will automatically play down log files in a variety of situations, such as single page restore and low disk space scenarios. If the system detects that page patching is required for a lagged copy, the logs will be automatically replayed into the lagged copy. Lagged copies will also invoke this auto replay feature when a low disk space threshold has been reached, and when the lagged copy has been detected as the only available copy for a specific period of time. In addition, lagged copies can leverage Safety Net, making recovery or activation much easier. *Safety Net* is improved functionality in Exchange 2016 based on the transport dumpster of Exchange 2010.

- **Single copy alert enhancements**: The single copy alert that was introduced in Exchange 2010 is no longer a separate scheduled script. It's now integrated into the managed availability components within the system and is a native function within Exchange.

- **DAG network auto-configuration**: DAGs networks can be automatically configured by the system based on configuration settings. In addition to manual configuration options, DAGs can also distinguish between MAPI and Replication networks and configure DAG networks automatically.

For more information about these features, see [High availability and site resilience](../high-availability/high-availability.md) and [Changes to high availability and site resilience over previous versions](../high-availability/ha-changes.md).

#### Managed Store
<a name="ManagedStore"> </a>

In Exchange 2016, *Managed Store* is the name of the Information Store processes, `Microsoft.Exchange.Store.Service.exe` and `Microsoft.Exchange.Store.Worker.exe`. The new Managed Store is written in C# and is tightly integrated with the Microsoft Exchange Replication service (`MSExchangeRepl.exe`) to provide higher availability through improved resiliency. In addition, the Managed Store allows more granular management of resource consumption, and has improved diagnostics for faster root cause analysis.

The Managed Store works with the Microsoft Exchange Replication service to manage mailbox databases, which continue to use the Extensible Storage Engine (ESE) database engine. Exchange 2016 includes significant changes to the mailbox database schema that provide many optimizations over previous versions of Exchange. The Microsoft Exchange Replication service is also responsible for all service availability related to Mailbox servers. These architectural changes enable faster database failover and better physical disk failure handling.

The Managed Store uses the same search platform as SharePoint Server 2016 to provide more robust indexing and searching when compared to Microsoft Search engine that was used in previous versions of Exchange.

For more information, see [High availability and site resilience](../high-availability/high-availability.md).

### Exchange workload management
<a name="WLM2010"> </a>

An Exchange workload is an Exchange server feature, protocol, or service that has been explicitly defined for the purposes of Exchange system resource management. Each Exchange workload consumes system resources such as CPU, mailbox database operations, or Active Directory requests to execute user requests or run background work. Examples of Exchange workloads include Outlook on the web, Exchange ActiveSync, mailbox migration, and mailbox assistants.

There are two ways to manage Exchange workloads in Exchange 2016:

- **Monitor the health of system resources**: Managing workloads based on the health of system resources.

- **Control how resources are consumed by individual users**: Controlling how resources are consumed by individual users was possible in Exchange 2010 (user throttling), and this capability has been expanded for Exchange 2016.

For more information about these features, see [User workload management in Exchange 2016](../server-health/workload-management.md).
::: moniker-end
