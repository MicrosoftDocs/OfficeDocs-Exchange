---
title: "What's new in Exchange 2013: Exchange 2013 Help"
TOCTitle: What's new in Exchange 2013
ms:assetid: 97501135-2149-4590-8373-98e638ac8eb1
ms:mtpsurl: https://technet.microsoft.com/library/JJ150540(v=EXCHG.150)
ms:contentKeyID: 47560059
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# What's new in Exchange 2013

_**Applies to:** Exchange Server 2013_

Check out all of the newest capabilities in Exchange 2013.

Microsoft Exchange Server 2013 brings a new rich set of technologies, features, and services to the Exchange Server product line. Its goal is to support people and organizations as their work habits evolve from a communication focus to a collaboration focus. At the same time, Exchange Server 2013 helps lower the total cost of ownership whether you deploy Exchange 2013 on-premises or provision your mailboxes in the cloud. New features and functionality in Exchange 2013 are designed to do the following:

- **Support a multigenerational workforce**: Social integration and making it easier to find people is important to users. *Smart Search* learns from users' communication and collaboration behavior to enhance and prioritize search results in Exchange. Also, with Exchange 2013, users can merge contacts from multiple sources to provide a single view of a person, by linking contact information pulled from multiple locations.

- **Provide an engaging experience**: Microsoft Outlook 2013 and Microsoft Outlook Web App have a fresh new look. Outlook Web App emphasizes a streamlined user interface that also supports the use of touch, enhancing the mobile device experience with Exchange.

- **Integrate with SharePoint and Lync**: Exchange 2013 offers greater integration with Microsoft SharePoint 2013 and Microsoft Lync 2013 through site mailboxes and In-Place eDiscovery. Together, these products offer a suite of features that make scenarios such as enterprise eDiscovery and collaboration using site mailboxes possible.

- **Help meet evolving compliance needs**:  Compliance and eDiscovery are challenging for many organizations. Exchange 2013 helps you to find and search data not only in Exchange, but across your organization. With improved search and indexing, you can search across Exchange 2013, Lync 2013, SharePoint 2013, and Windows file servers. In addition, data loss prevention (DLP) can help keep your organization safe from users mistakenly sending sensitive information to unauthorized people. DLP helps you identify, monitor, and protect sensitive data through deep content analysis.

- **Provide a resilient solution**: Exchange 2013 builds upon the Exchange Server 2010 architecture and has been redesigned for simplicity of scale, hardware utilization, and failure isolation.

For information about the changes made to Exchange Server 2013 since release to manufacturing (RTM), see [Updates for Exchange 2013](updates-for-exchange-2013-exchange-2013-help.md).

See the following sections for more information about what's new in Exchange 2013:

Exchange admin center

Exchange 2013 architecture

Setup

Messaging policy and compliance

Anti-malware protection

Mail flow

Recipients

Sharing and collaboration

Integration with SharePoint and Lync

Clients and mobile

Unified Messaging

Batch mailbox moves

[High availability and site resilience](high-availability-and-site-resilience-exchange-2013-help.md)

Exchange workload management

> [!NOTE]
> For information about features in earlier versions of Exchange that have been removed, discontinued, or replaced in Exchange Server 2013, see <A href="what-s-discontinued-in-exchange-2013-exchange-2013-help.md">What's discontinued in Exchange 2013</A>. Also, you may be interested in <A href="release-notes-for-exchange-2013-exchange-2013-help.md">Release notes for Exchange 2013</A>.

## Exchange admin center

Exchange 2013 provides a single unified management console that allows for ease of use and is optimized for management of on-premises, online, or hybrid deployments. The *Exchange admin center* (EAC) in Exchange 2013 replaces the Exchange 2010 Exchange Management Console (EMC) and the Exchange Control Panel (ECP). (However, "ECP" is still the name of the virtual directory used by the EAC.) Some EAC features include:

- **List view**: The list view in EAC has been designed to remove key limitations that existed in ECP. ECP was limited to displaying up to 500 objects and, if you wanted to view objects that weren't listed in the details pane, you needed to use searching and filtering to find those specific objects. In Exchange 2013, the viewable limit from within the EAC list view is approximately 20,000 objects. After the EAC returns the results, the EAC client performs the searching and sorting, which greatly increases the performance compared to the ECP in Exchange 2010. In addition, paging has been added so that you can page to the results. You can also configure page size and export to a .csv file.

- **Add/Remove columns to the Recipient list view**: You can choose which columns to view, and with local cookies, you can save your custom list views per machine that you use to access the EAC.

- **Secure the ECP virtual directory**: You can partition access from the Internet and intranets from within the ECP IIS virtual directory to allow or disallow management features. With this feature, you can permit or deny access to users trying to access the EAC from the Internet outside of your organizational environment, while still allowing access to an end-user's Outlook Web App Options.

- **Public Folder management**: In Exchange 2010 and Exchange 2007, public folders were managed through the Public Folder administration console. Public folders are now in the EAC, and you don't need a separate tool to manage them.

- **Notifications**: In Exchange 2013, the EAC now has a Notification viewer so that you can view the status of long-running processes and, if you choose, receive notification via an email message when the process completes.

- **Role Based Access Control (RBAC) User Editor**: In Exchange 2010, you could use the RBAC User Editor in the Exchange Toolbox to add users to management role groups. In Exchange 2013, the RBAC User Editor functionality is now in the EAC and you don't need a separate tool to manage RBAC.

- **Unified Messaging Tools**: In Exchange 2010, you could use the Call Statistics and User Call Logs tools to help provide UM statistics and information about specific calls for a UM-enabled user. In Exchange 2013, the Call Statistics and User Call Logs tools are now in the EAC and you don't need a separate tool to manage them.

- **Groups enhancements**: The Exchange admin center (EAC) can now display up to 10,000 recipients in the **Groups** **Select Members** window. By default, up to 500 recipients are returned when you open the **Select Members** window, however, you can choose to list up to 10,000 recipients by clicking **Get All Results** beneath the recipient list. We now support browsing more than 500 recipients by using the scroll bar and we've also added enhanced search features to enable you to filter recipients that are displayed in the recipient list. You can filter by:

  - city

  - company

  - country/region

  - department

  - office

  - title

For more information, see [Exchange admin center in Exchange 2013](exchange-admin-center-in-exchange-2013-exchange-2013-help.md).

## Exchange 2013 architecture

Previous versions of Exchange were optimized and architected with certain technological constraints that existed at that time. For example, during development for Exchange 2007, one of the key constraints was CPU performance. To alleviate that constraint, Exchange 2007 was split into different server roles that allowed scale out through server separation. However, server roles in Exchange 2007 and Exchange 2010 were tightly coupled. The tight coupling of the roles had several downsides including version dependency, geo-affinity (requiring all roles in a specific site), session affinity (requiring expensive layer 7 hardware load balancing), and namespace complexity.

Today, CPU horsepower is significantly less expensive and is no longer a constraining factor. With that constraint lifted, the primary design goal for Exchange 2013 is for simplicity of scale, hardware utilization, and failure isolation. With Exchange 2013, we reduced the number of server roles to three: the Client Access, Mailbox, and Edge Transport server roles.

The Mailbox server includes all the traditional server components found in Exchange 2010: the Client Access protocols, Transport service, Mailbox databases, and Unified Messaging. The Mailbox server handles all activity for the active mailboxes on that server. The Client Access server provides authentication, limited redirection, and proxy services. The Client Access server itself doesn't do any data rendering. The Client Access server is a thin and stateless server. There is never anything queued or stored on the Client Access server. The Client Access server offers all the usual client access protocols: HTTP, POP and IMAP, and SMTP.

With this new architecture, the Client Access server and the Mailbox server have become "loosely coupled." All processing and activity for a specific mailbox occurs on the Mailbox server that houses the active database copy where the mailbox resides. All data rendering and data transformation is performed local to the active database copy, eliminating concerns of version compatibility between the Client Access server and the Mailbox server.

With Exchange 2013 Service Pack 1, we've re-introduced the Edge Transport server role. The Edge Transport role is typically deployed in your perimeter network, outside your internal Active Directory forest, and is designed to minimize the attack surface of your Exchange deployment. By handling all Internet-facing mail flow, it also adds additional layers of message protection and security against viruses and spam, and can apply transport rules to control message flow. For more information about the Edge Transport server role, see [Edge Transport servers](edge-transport-servers-exchange-2013-help.md).

The Exchange 2013 architecture provides the following benefits:

- **Version upgrade flexibility**: No more rigid upgrade requirements. A Client Access server can be upgraded independently and in any order in relation to the Mailbox server.

- **Session indifference**: With Exchange 2010, session affinity to the Client Access server role was required for several protocols. In Exchange 2013, the client access and mailbox components reside on the same Mailbox server. Because the Client Access server simply proxies all connections for a user to a specific Mailbox server, no session affinity is required at the Client Access servers. This allows inbound connections to Client Access servers to be balanced using techniques provided by load-balancing technology like least connection or round-robin.

- **Deployment simplicity**: With an Exchange 2010 site-resilient design, you needed up to eight different namespaces: two Internet Protocol namespaces, two for Outlook Web App fallback, one for Autodiscover, two for RPC Client Access, and one for SMTP. A legacy namespace was also required if you were upgrading from Exchange 2003 or Exchange 2007. With Exchange 2013, the minimum number of namespaces drops to two. If you're coexisting with Exchange 2007, you still need to create a legacy hostname, but if you're coexisting with Exchange 2010 or you're installing a new Exchange 2013 organization, the minimum number of namespaces you need is two: one for client protocols and one for Autodiscover. You may also need an SMTP namespace.

As a result of these architectural changes, there have been some changes to client connectivity. First, RPC is no longer a supported direct access protocol. This means that all Outlook connectivity must take place using RPC over HTTP (also known as Outlook Anywhere). At first glance, this may seem like a limitation, but it actually has some added benefits. The most obvious benefit is that there is no need to have the RPC client access service on the Client Access server. This results in the reduction of two namespaces that would normally be required for a site-resilient solution. In addition, there is no longer any requirement to provide affinity for the RPC client access service.

Second, Outlook clients no longer connect to a server FQDN as they have done in all previous versions of Exchange. Outlook uses Autodiscover to create a new connection point comprised of mailbox GUID, @ symbol, and the domain portion of the user's primary SMTP address. This simple change results in a near elimination of the unwelcome message of "Your administrator has made a change to your mailbox. Please restart." Only Outlook 2007 and higher versions are supported with Exchange 2013.

The high availability model of the mailbox component has not changed significantly since Exchange 2010. The unit of high availability is still the database availability group (DAG). The DAG still uses Windows Server failover clustering. Continuous replication still supports both file mode and block mode replication. However, there have been some improvements. Failover times have been reduced as a result of transaction log code improvements and deeper checkpoint on the passive databases. The Exchange Store service has been re-written in managed code (see the "Managed Store" section later in this topic). Now, each database runs under its own process, allowing for isolation of store issues to a single database.

## Managed Store

In Exchange 2013, the *Managed Store* is the name of the newly rewritten Information Store processes, Microsoft.Exchange.Store.Service.exe and Microsoft.Exchange.Store.Worker.exe. The new Managed Store is written in C\# and tightly integrated with the Microsoft Exchange Replication service (MSExchangeRepl.exe) to provide higher availability through improved resiliency. In addition, the Managed Store has been architected to enable more granular management of resource consumption and faster root cause analysis through improved diagnostics.

The Managed Store works with the Microsoft Exchange Replication service to manage mailbox databases, which continues to use Extensible Storage Engine (ESE) as the database engine. Exchange 2013 includes significant changes to the mailbox database schema that provide many optimizations over previous versions of Exchange. In addition to these changes, the Microsoft Exchange Replication service is responsible for all service availability related to Mailbox servers. The architectural changes enable faster database failover and better physical disk failure handling.

The Managed Store is also integrated with the Search Foundation search engine (the same search engine used by SharePoint 2013) to provide more robust indexing and searching when compared to Microsoft Search in previous versions of Exchange.

For more information, see [High availability and site resilience](high-availability-and-site-resilience-exchange-2013-help.md).

## Certificate management

Managing digital certificates is one of the most important security-related tasks for your Exchange organization. Ensuring that certificates are appropriately configured is key to delivering a secure messaging infrastructure for the enterprise. In Exchange 2010, the Exchange Management Console was the primary method of managing certificates. In Exchange 2013, certificate management functionality is provided in the Exchange admin center, the new Exchange 2013 administrator user interface.

The work in Exchange 2013 related to certificates focused around minimizing the number of certificates that an Administrator must manage, minimizing the interaction the Administrator must have with certificates, and allowing management of certificates from a central location. Benefits resulting from the changes in certificate management are:

- Certificate management can be performed on the Client Access server or the Mailbox server. The Mailbox server has a self-signed certificate installed by default. The Client Access server automatically trusts the self-signed certificate on the Exchange 2013 Mailbox server, so clients will not receive warnings about a self-signed certificate not being trusted provided that the Exchange 2013 Client Access server has a non-self-signed certificate from either a Windows certificate authority (CA) or a trusted third party.

- In previous versions of Exchange, it was difficult to see when a digital certificate was nearing expiration. In Exchange 2013, the Notifications center will display warnings when a certificate stored on any Exchange 2013 server is about to expire. Administrators can also choose to receive these notifications via email.

For more information, see [Digital certificates and SSL](digital-certificates-and-ssl-exchange-2013-help.md).

## Setup

Setup has been completely rewritten so that installing Exchange 2013 and making sure you've got the latest product rollups and security fixes is easier than ever. Here are some of the improvements we've made:

- **Always up-to-date Setup**: When you run the Setup wizard, you'll be given the option to download and use the latest product rollups, security fixes, and language packs. This option doesn't just update the files that'll be used to run Exchange; Setup itself can be updated. This design enables us to continue to improve Setup post-release and include and update readiness checks as requirements are updated or changed.

    If you're using unattended Setup mode, we can't automatically download updates. However, you can still take advantage of running the latest version of Setup by downloading the latest updates beforehand, and use the `/UpdatesDir: <path>` parameter to allow Setup to update itself before the installation process begins.

- **Improved readiness checks**: Readiness checks make sure that your computer and your organization are ready for Exchange 2013. After you've provided the necessary information about your installation to Setup, the readiness checks are run before installation begins. The new readiness check engine now runs through all checks before reporting back to you on what actions need to be performed before Setup can continue, and it does so faster than ever. As with previous versions of Exchange, you can tell Setup to install the Windows features required by Setup so you don't have to install them manually.

- **Simplified and modern wizard**: We've removed all the steps in the Setup wizard that aren't absolutely required for you to install Exchange. What's left is an easy-to-follow wizard that takes you through the installation process one step at a time.

For more information, see [Planning and deployment](planning-and-deployment-for-exchange-2013-installation-instructions.md).

## Messaging policy and compliance

There are two new message policy and compliance features in Exchange 2013: Data loss prevention and the Microsoft Rights Management connector.

*Data loss prevention* (DLP) capabilities help you protect your sensitive data and inform users of internal compliance policies. DLP can also help keep your organization safe from users who might mistakenly send sensitive information to unauthorized people. DLP helps you identify, monitor, and protect sensitive data through deep content analysis. Exchange 2013 offers built-in DLP policies based on regulatory standards such as personally identifiable information (PII) and payment card industry data security standards (PCI), and is extensible to support other policies important to your business. Additionally, the new PolicyTips in Outlook 2013 inform users about policy violations before sensitive data is sent.

The Microsoft Rights Management connector (RMS connector) is an optional application that helps you enhance data protection for your Exchange 2013 server by connecting to cloud-based Microsoft Rights Management services. Once you install the RMS connector, it provides continuous data protection throughout the lifespan of the information and because these services are customizable, you can define the level of protection you need. For example, you can limit email message access to specific users or set view-only rights for certain messages.

To learn more about these features see:

[Data loss prevention](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention)

[Rights Management connector](https://docs.microsoft.com/azure/information-protection/deploy-rms-connector)

## In-place Archiving, retention, and eDiscovery

Exchange 2013 includes the following improvements to In-Place Archiving, retention, and eDiscovery to help your organization meet its compliance needs:

- **In-Place Hold**: In-Place Hold is a new unified hold model that allows you to meet legal hold requirements in the following scenarios:

  - Preserve the results of the query (query-based hold), which allows for scoped immutability across mailboxes.

  - Place a time-based hold to meet retention requirements (for example, retain all items in a mailbox for seven years, a scenario that required the use of Single Item Recovery/Deleted Item Retention in Exchange 2010).

  - Place a mailbox on indefinite hold (similar to litigation hold in Exchange 2010).

  - Place a user on multiple holds to meet different case requirements.

- **In-Place eDiscovery**: In-Place eDiscovery allows authorized users to search mailbox data across all mailboxes and In-Place Archives in an Exchange 2013 organization and copy messages to a discovery mailbox for review. In Exchange 2013, In-Place eDiscovery has been enhanced to allow discovery managers to perform more efficient searches and hold. These enhancements include:

  - **Federated search** allows you to search and preserve data across multiple data repositories. With Exchange 2013, you can perform in-place eDiscovery searches across Exchange, SharePoint 2013, and Lync 2013. You can use the eDiscovery Center in SharePoint 2013 to perform In-Place eDiscovery search and hold.

  - **Query-based In-Place Hold** allows you to save the results of the query, which allows for scoped immutability across mailboxes.

  - **Export search results** Discovery Managers can export mailbox content to a .pst file from the SharePoint 2013 eDiscovery Console. Mailbox export request cmdlets are no longer required to export a mailbox to a .pst file.

  - **Keyword statistics**: Search statistics are offered on a per search term basis. This enables a Discovery Manager to quickly make intelligent decisions about how to further refine the search query to provide better results. eDiscovery search results are sorted by relevance.

  - **KQL syntax**: Discovery Managers can use Keyword Query Language (KQL) syntax in search queries. KQL is similar to the Advanced Query Syntax (AQS), which was used for discovery searches in Exchange 2010.

  - **In-Place eDiscovery and Hold wizard**: Discovery Managers can use the new In-Place eDiscovery and Hold wizard to perform eDiscovery and hold operations.

        > [!NOTE]
        > If SharePoint 2013 isn't available, a subset of the eDiscovery functionality is available in the Exchange admin center.

- **Search across primary and archive mailboxes in Outlook Web App**: Users can search across their primary and archive mailboxes in Outlook Web App. Two separate searches are no longer necessary.

- **Archive Lync content**: Exchange 2013 supports archiving of Lync 2013 content in a user's mailbox. You can place Lync content on hold using In-Place Hold and use In-Place eDiscovery to search Lync content archived in Exchange.

- **Retention policies**: Retention policies help your organization reduce risks associated with email and other communications and also meet email retention requirements. Retention policies include the following enhancements:

  - **Support for Calendar and Tasks retention tags**: You can create retention policy tags for the Calendar and Tasks default folders to expire items in these folders. Items in these folders are also moved to the user's archive based on the archive policy settings applied to the mailbox.

  - **Improved ability to retain items for a specified period**: You can use retention policy and a time-based In-Place Hold to enforce retention of items for a set period.

For more information, see [Messaging policy and compliance](messaging-policy-and-compliance-exchange-2013-help.md).

## Transport rules

Transport rules in Exchange Server 2013 are a continuation of the features that are available in Exchange Server 2010. However, several improvements have been made to transport rules in Exchange 2013. The most important change is the support for data loss prevention (DLP). There are also new predicates and actions, enhanced monitoring, and a few architectural changes.

For detailed information, see [What's new for transport rules](what-s-new-for-transport-rules-exchange-2013-help.md).

## Information Rights Management

Information Rights Management (IRM) is compatible with Cryptographic Mode 2, an Active Directory Rights Management Services (AD RMS) cryptography mode that supports stronger encryption by allowing you to use 2048-bit keys for RSA and 256-bit keys for SHA-1. Additionally, Mode 2 enables you to use the SHA-2 hashing algorithm. For more information about cryptographic modes in AD RMS, see [AD RMS Cryptographic Modes](https://go.microsoft.com/fwlink/p/?linkid=263219).

## Auditing

Exchange 2013 includes the following improvements to auditing:

- **Auditing reports**: The EAC includes auditing functionality so that you can run reports or export entries from the mailbox audit log and the administrator audit log. The mailbox audit log records whenever a mailbox is accessed by someone other than the person who owns the mailbox. This can help you determine who has accessed a mailbox and what they have done. The administrator audit log records any action, based on an Exchange Management Shell cmdlet, performed by an administrator. This can help you troubleshoot configuration issues or identify the cause of problems related to security or compliance. For more information, see [Exchange auditing reports](https://docs.microsoft.com/exchange/security-and-compliance/exchange-auditing-reports/exchange-auditing-reports).

- **Viewing the administrator audit log**: Instead of exporting the administrator audit log, which can take up to 24 hours to receive in an email message, you can view administrator audit log entries in the EAC. To do this, go to **Compliance Management** \> **Auditing** and click **View the administrator audit log**. Up to 1000 entries will be displayed on multiple pages. To narrow the search, you can specify a date range. For more information, see [View the administrator audit log](https://docs.microsoft.com/exchange/security-and-compliance/exchange-auditing-reports/view-administrator-audit-log).

## Anti-malware protection

The built-in malware filtering capabilities of Exchange 2013 helps protect your network from malicious software transferred through email messages. All messages sent or received by your Exchange server are scanned for malware (viruses and spyware). If malware is detected, the message is deleted. Notifications may also be sent to senders or administrators when an infected message is deleted and not delivered. You can also choose to replace infected attachments with either default or custom messages that notify the recipients of the malware detection.

For more information about anti-malware protection, see [Anti-malware protection](anti-malware-protection-exchange-2013-help.md).

## Mail flow

How messages flow through an organization and what happens to them has changed significantly in Exchange 2013. Following is a brief overview of the changes:

- **Transport pipeline**: The transport pipeline in Exchange 2013 is now made up of several different services: the Front End Transport service on Client Access servers, the Transport service on Mailbox servers, and the Mailbox Transport service on Mailbox servers. For more information, see [Mail flow](mail-flow-exchange-2013-help.md).

- **Routing**: Mail routing in Exchange 2013 recognizes DAG boundaries as well as Active Directory site boundaries. Also, mail routing has been improved to queue messages more directly for internal recipients. For more information, see [Mail routing](mail-routing-exchange-2013-help.md).

- **Connectors**: The default maximum message size for a Send connector or a Receive connector, as specified by the *MaxMessageSize* parameter, has been increased from 10MB to 25MB. For more information about how to set parameters on a connector, see [Set-SendConnector](https://docs.microsoft.com/powershell/module/exchange/Set-SendConnector) and [Set-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/Set-ReceiveConnector).

    You can set a Send connector in the Transport service of a Mailbox server to route outbound mail through a Front End transport server in the local Active Directory site, by means of the *FrontEndProxyEnabled* parameter of the **Set-SendConnector** cmdlet, thus consolidating how email is routed from the Transport service.

- **Edge Transport**: You can optionally install an Edge Transport server in your perimeter network to reduce your attack surface and provide message protection and security. For more information, see [Edge Transport servers](edge-transport-servers-exchange-2013-help.md).

## Recipients

This section describes the enhancements for managing recipients in Exchange 2013:

- **Group naming policy**: Administrators can now use the EAC to create a *group naming policy*, which lets you standardize and manage the names of distribution groups created by users in your organization. You can require a specific prefix and suffix be added to the name for a distribution group when it's created, and you can block specific words from being used. This capability helps you minimize the use of inappropriate words in group names.

    For more information, see [Create a distribution group naming policy](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-distribution-groups/create-group-naming-policy).

- **Message tracking**: Administrators can also use the EAC to track delivery information for email messages sent to or received by any user in your organization. You just select a mailbox, and then search for messages sent to or received by a different user. You can narrow the search by searching for specific words in the subject line. The resulting delivery report tracks a message through the delivery process and specifies if the message was successfully delivered, pending delivery, or if it wasn't delivered.

    For more information, see [Track messages with delivery reports](track-messages-with-delivery-reports-exchange-2013-help.md).

## Sharing and collaboration

This section describes the sharing and collaboration enhancements in Exchange 2013.

- **Public folders**: Public folders now take advantage of the existing high availability and storage technologies of the mailbox store. The public folder architecture uses specially designed mailboxes to store both the hierarchy and the public folder content. This new design also means that there is no longer a public folder database. Public folder replication now uses the continuous replication model. High availability for the hierarchy and content mailboxes is provided by the database availability group (DAG). With this design, we're moving away from a multi-master replication model to a single-master replication model.

    Outlook Web App users in your organization now have the ability to add public folders to, or remove them from, their Favorites. Previously, this could only be done in Outlook.

    For more information, see [Public folders](public-folders-exchange-2013-help.md).

- **Site mailboxes**: Email and documents are traditionally kept in two unique and separate data repositories. Most teams would typically collaborate using both mediums. The challenge is that email and documents are accessed using different clients, which usually results in a reduction in user productivity and a degraded user experience.

    The *site mailbox* is a new concept in Exchange 2013 that attempts to solve these problems. Site mailboxes improve collaboration and user productivity by allowing access to both documents in a SharePoint site and email messages in Outlook 2013, using the same client interface. A site mailbox is functionally comprised of SharePoint site membership (owners and members), shared storage through an Exchange mailbox for email messages and a SharePoint site for documents, and a management interface that addresses provisioning and lifecycle needs.

    For more information, see [Site mailboxes](site-mailboxes-exchange-2013-help.md).

- **Shared mailboxes**: In previous versions of Exchange, creating a shared mailbox was a multi-step process in which you had to use the Exchange Management Shell to set the delegate permissions. In Exchange 2013, you can now create a shared mailbox in one step via the Exchange admin center. In the EAC, go to **Recipients** \> **Shared Mailboxes** to create a shared mailbox. Shared mailbox is now a recipient type, so you can easily search for your shared mailboxes in either the user interface or by using the Shell.

    For more information, see [Shared mailboxes](shared-mailboxes-exchange-2013-help.md).

## Integration with SharePoint and Lync

Exchange 2013 offers greater integration with SharePoint 2013 and Lync 2013. Benefits of this enhanced integration include:

- Exchange 2013 integrates with SharePoint 2013 to allow users to collaborate more effectively by using site mailboxes.

- Lync Server 2013 can archive content in Exchange 2013 and use Exchange 2013 as a contact store.

- Discovery Managers can perform In-Place eDiscovery and Hold searches across SharePoint 2013, Exchange 2013, and Lync 2013 data.

- Oauth authentication allows partner applications to authenticate as a service or impersonate users where required.

For more information, see [Integration with SharePoint and Lync](integration-with-sharepoint-and-lync-exchange-2013-help.md).

## Clients and mobile

The Outlook Web App user interface is new and optimized for tablets and smartphones as well as desktop and laptop computers. New features include apps for Outlook, which allow users and administrators to extend the capabilities of Outlook Web App; Contact linking, the ability for users to add contacts from their LinkedIn accounts; and updates to the look and features of the calendar.

For more information, see [What's new for Outlook Web App in Exchange 2013](what-s-new-for-outlook-web-app-in-exchange-2013-exchange-2013-help.md).

## Unified Messaging

Unified Messaging in Exchange 2013 contains essentially the same voice mail features included in Exchange 2010. However, some new and enhanced features and functionality have been added to those existing features. More importantly, architectural changes in Exchange 2013 Unified Messaging resulted in components, services, and functionality that were included with the Unified Messaging server role in Exchange 2010 to be divided between the Exchange 2013 Client Access and Mailbox server roles.

For more details, see [What's new for Unified Messaging in Exchange 2013](what-s-new-for-unified-messaging-in-exchange-2013-exchange-2013-help.md).

## Batch mailbox moves

Exchange 2013 introduces the concept of batch moves. The new move architecture is built on top of MRS (Mailbox Replication service) moves with enhanced management capability. The new batch move architecture features the following enhancements:

- Ability to move multiple mailboxes in large batches.

- Email notification during move with reporting.

- Automatic retry and automatic prioritization of moves.

- Primary and personal archive mailboxes can be moved together or separately.

- Option for manual move request finalization, which allows you to review a move before you complete it.

- Periodic incremental syncs to migrate the changes.

For more information, see [Manage on-premises moves](manage-on-premises-moves-exchange-2013-help.md).

## High availability and site resilience

Exchange 2013 uses DAGs and mailbox database copies, along with other features such as single item recovery, retention policies, and lagged database copies, to provide high availability, site resilience, and Exchange native data protection. The high availability platform, the Exchange Information Store and the Extensible Storage Engine (ESE), have all been enhanced to provide greater availability, easier management, and to reduce costs. These enhancements include:

- **Managed availability**: With managed availability, internal monitoring and recovery-oriented features are tightly integrated to help prevent failures, proactively restore services, and initiate server failovers automatically or alert administrators to take action. The focus is on monitoring and managing the end user experience rather than just server and component uptime to help keep the service continuously available.

- **Managed Store**: The Managed Store is the name of the newly rewritten Information Store processes in Exchange 2013. The new Managed Store is written in C\# and tightly integrated with the Microsoft Exchange Replication service (MSExchangeRepl.exe) to provide higher availability through improved resiliency.

- **Support for multiple databases per disk**: Exchange 2013 includes enhancements that enable you to support multiple databases (mixtures of active and passive copies) on the same disk, thereby leveraging larger disks in terms of capacity and IOPS as efficiently as possible.

- **Automatic reseed**: Enables you to quickly restore database redundancy after disk failure. If a disk fails, the database copy stored on that disk is copied from the active database copy to a spare disk on the same server. If multiple database copies were stored on the failed disk, they can all be automatically re-seeded on a spare disk. This enables faster reseeds, as the active databases are likely to be on multiple servers and the data is copied in parallel.

- **Automatic recovery from storage failures**: This feature continues the innovation introduced in Exchange 2010 to allow the system to recover from failures that affect resiliency or redundancy. In addition to the Exchange 2010 bugcheck behaviors, Exchange 2013 includes additional recovery behaviors for long I/O times, excessive memory consumption by MSExchangeRepl.exe, and severe cases where the system is in such a bad state that threads can't be scheduled.

- **Lagged copy enhancements**: Lagged copies can now care for themselves to a certain extent using automatic log play down. Lagged copies will automatically play down log files in a variety of situations, such as single page restore and low disk space scenarios. If the system detects that page patching is required for a lagged copy, the logs will be automatically replayed into the lagged copy to perform page patching. Lagged copies will also invoke this auto replay feature when a low disk space threshold has been reached, and when the lagged copy has been detected as the only available copy for a specific period of time. In addition, lagged copies can leverage Safety Net, making recovery or activation much easier. *Safety Net* is improved functionality in Exchange 2013 based on the transport dumpster of Exchange 2010.

- **Single copy alert enhancements**: The single copy alert introduced in Exchange 2010 is no longer a separate scheduled script. It's now integrated into the managed availability components within the system and is a native function within Exchange.

- **DAG network auto-configuration**: DAGs networks can be automatically configured by the system based on configuration settings. In addition to manual configuration options, DAGs can also distinguish between MAPI and Replication networks and configure DAG networks automatically.

For more information about both of these features, see [High availability and site resilience](high-availability-and-site-resilience-exchange-2013-help.md) and [Changes to high availability and site resilience over previous versions](changes-to-high-availability-and-site-resilience-over-previous-versions-exchange-2013-help.md).

## Exchange workload management

An Exchange workload is an Exchange server feature, protocol, or service that has been explicitly defined for the purposes of Exchange system resource management. Each Exchange workload consumes system resources such as CPU, mailbox database operations, or Active Directory requests to execute user requests or run background work. Examples of Exchange workloads include Outlook Web App, Exchange ActiveSync, mailbox migration, and mailbox assistants.

There are two ways to manage Exchange workloads in Exchange 2013:

- **Monitor the health of system resources**: Managing workloads based on the health of system resources is new in Exchange 2013.

- **Control how resources are consumed by individual users**: Controlling how resources are consumed by individual users was possible in Exchange 2010 (where it's called user throttling), and this capability has been expanded for Exchange 2013.

For more information about these features, see [Exchange workload management](exchange-workload-management-exchange-2013-help.md).
