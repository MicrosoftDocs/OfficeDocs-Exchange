---
localization_priority: Normal
description: If your organization adheres to legal discovery requirements (related to organizational policy, compliance, or lawsuits), In-Place eDiscovery in Microsoft Exchange Server and Exchange Online can help you perform discovery searches for relevant content within mailboxes. Exchange Server and Exchange Online also offer federated search capability and integration with Microsoft SharePoint 2013 and Microsoft SharePoint Online. Using the eDiscovery Center in SharePoint, you can search for and hold all content related to a case, including SharePoint 2013 and SharePoint Online websites, documents, file shares indexed by SharePoint (SharePoint 2013 only), mailbox content in Exchange, and archived Lync 2013 content. You can also use In-Place eDiscovery in an Exchange hybrid environment to search on-premises and cloud-based mailboxes in the same search.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6377cb7a-3416-4e15-8571-c45d2160fc6f
ms.reviewer: 
f1.keywords:
- NOCSH
title: In-Place eDiscovery
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# In-Place eDiscovery

> [!IMPORTANT]
>  As we continue to invest in different ways to search for mailbox content, we're announcing the retirement of In-Place eDiscovery in the Exchange admin center (EAC) in Exchange Online. Starting July 1, 2020, you won't be able to create new In-Place eDiscovery searches. But you'll still be able to manage In-Place eDiscovery searches in the EAC or by using the **Set-MailboxSearch** cmdlet in Exchange Online PowerShell. However, starting October 1, 2020, you won't be able to manage In-Place eDiscovery searches. You'll only be able to remove them in the EAC or by using the **Remove-MailboxSearch** cmdlet. Using In-Place eDiscovery in Exchange Server and Exchange hybrid deployments will still be supported. For more information about the retirement of In-Place eDiscovery in Exchange Online, see [Retirement of legacy eDiscovery tools](https://docs.microsoft.com/microsoft-365/compliance/legacy-ediscovery-retirement).

If your organization adheres to legal discovery requirements (related to organizational policy, compliance, or lawsuits), In-Place eDiscovery in Microsoft Exchange Server and Exchange Online can help you perform discovery searches for relevant content within mailboxes. Exchange Server and Exchange Online also offer federated search capability and integration with Microsoft SharePoint 2013 and Microsoft SharePoint Online. Using the eDiscovery Center in SharePoint, you can search for and hold all content related to a case, including SharePoint 2013 and SharePoint Online websites, documents, file shares indexed by SharePoint (SharePoint 2013 only), mailbox content in Exchange, and archived Lync 2013 content. You can also use In-Place eDiscovery in an Exchange hybrid environment to search on-premises and cloud-based mailboxes in the same search.

> [!IMPORTANT]
> In-Place eDiscovery is a powerful feature that allows a user with the correct permissions to potentially gain access to all messaging records stored throughout the Exchange Server or Exchange Online organization. It's important to control and monitor discovery activities, including addition of members to the Discovery Management role group, assignment of the Mailbox Search management role, and assignment of mailbox access permission to discovery mailboxes.

## How In-Place eDiscovery works
<a name="howitworks"> </a>

In-Place eDiscovery uses the content indexes created by Exchange Search. Role Based Access Control (RBAC) provides the Discovery Management role group to delegate discovery tasks to non-technical personnel, without the need to provide elevated privileges that may allow a user to make any operational changes to Exchange configuration. The Exchange admin center (EAC) provides an easy-to-use search interface for non-technical personnel such as legal and compliance officers, records managers, and human resources (HR) professionals.

Authorized users can perform an In-Place eDiscovery search by selecting the mailboxes, and then specifying search criteria such as keywords, start and end dates, sender and recipient addresses, and message types. After the search is complete, authorized users can then select one of the following actions:

- **Estimate search results**: This option returns an estimate of the total size and number of items that will be returned by the search based on the criteria you specified.

- **Preview search results**: This option provides a preview of the results. Messages returned from each mailbox searched are displayed.

- **Copy search results**: This option lets you copy messages to a discovery mailbox.

- **Export search results**: After search results are copied to a discovery mailbox, you can export them to a PST file.

![Estimate, Preview, Copy, and Export Search Results](../../media/TA_Discovery_EstimatePreview.gif)

## Exchange Search
<a name="search"> </a>

In-Place eDiscovery uses the content indexes created by Exchange Search. Exchange Search has been retooled to use Microsoft Search Foundation, a rich search platform that comes with significantly improved indexing and querying performance and improved search functionality. Because the Microsoft Search Foundation is also used by other Office products, including SharePoint 2013, it offers greater interoperability and similar query syntax across these products.

With a single content indexing engine, no additional resources are used to crawl and index mailbox databases for In-Place eDiscovery when eDiscovery requests are received by IT departments.

In-Place eDiscovery uses Keyword Query Language (KQL), a querying syntax similar to the Advanced Query Syntax (AQS) used by Instant Search in Microsoft Outlook and Outlook on the web. Users familiar with KQL can easily construct powerful search queries to search content indexes.

For more information about the file formats indexed by Exchange search, see [File Formats Indexed By Exchange Search](https://docs.microsoft.com/exchange/file-formats-indexed-by-exchange-search-exchange-2013-help).

## Discovery Management role group and management roles
<a name="roles"> </a>

For authorized users to perform In-Place eDiscovery searches, you must add them to the Discovery Management role group. This role group consists of two management roles: the Mailbox Search role, which allows a user to perform an In-Place eDiscovery search, and the Legal Hold role, which allows a user to place a mailbox on In-Place Hold or litigation hold. For more information about roles and role groups, see [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md).

By default, permissions to perform In-Place eDiscovery-related tasks aren't assigned to any user or Exchange administrators. Exchange administrators who are members of the Organization Management role group can add users to the Discovery Management role group and create custom role groups to narrow the scope of a discovery manager to a subset of users. To learn more about adding users to the Discovery Management role group, see [Assign eDiscovery permissions in Exchange](assign-ediscovery-permissions.md).

> [!IMPORTANT]
> If a user hasn't been added to the Discovery Management role group or isn't assigned the Mailbox Search role, the **In-Place eDiscovery & Hold** user interface isn't displayed in the EAC, and the In-Place eDiscovery cmdlets aren't available in Exchange Online PowerShell.

Auditing of RBAC role changes, which is enabled by default, makes sure that adequate records are kept to track assignment of the Discovery Management role group. You can use the administrator role group report to search for changes made to administrator role groups. For more information, see [Search the role group changes or administrator audit logs](../../security-and-compliance/exchange-auditing-reports/search-role-group-changes.md).

## Custom management scopes for In-Place eDiscovery
<a name="customscopes"> </a>

You can use a custom management scope to let specific people or groups use In-Place eDiscovery to search a subset of mailboxes in your Exchange Server or Exchange Online organization. For example, you might want to let a discovery manager search only the mailboxes of users in a specific location or department. You do this by creating a custom management scope that uses a custom recipient filter to control which mailboxes can be searched. Recipient filter scopes use filters to target specific recipients based on recipient type or other recipient properties.

For In-Place eDiscovery, the only property on a user mailbox that you can use to create a recipient filter for a custom scope is distribution group membership. If you use other properties, such as _CustomAttributeN_, _Department_, or _PostalCode_, the search fails when it's run by a member of the role group that's assigned the custom scope. For more information, see [Create a custom management scope for In-Place eDiscovery searches](create-custom-management-scope.md).

## Integration with SharePoint Server and SharePoint Online
<a name="SP"> </a>

Exchange Server and Exchange Online offer integration with SharePoint Server and SharePoint Online, allowing a discovery manager to use eDiscovery Center in SharePoint to perform the following tasks:

- **Search and preserve content from a single location**: An authorized discovery manager can search and preserve content across SharePoint and Exchange, including Lync content such as instant messaging conversations and shared meeting documents archived in Exchange mailboxes.

- **Case management** eDiscovery Center uses a case management approach to eDiscovery, allowing you to create cases and search and preserve content across different content repositories for each case.

- **Export search results**: A discovery manager can use eDiscovery Center to export search results. Mailbox content included in search results is exported to a PST file.

SharePoint also uses Microsoft Search Foundation for content indexing and querying. Regardless of whether a discovery manager uses the EAC or the eDiscovery Center to search Exchange content, the same mailbox content is returned.

In on-premises deployments, before you can use eDiscovery Center in SharePoint to search Exchange mailboxes, you must establish trust between the two applications. In Exchange Server and SharePoint 2013, this is done using OAuth authentication. For details, see [Configure Exchange for SharePoint eDiscovery Center](https://docs.microsoft.com/exchange/configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help). eDiscovery searches performed from SharePoint are authorized by Exchange using RBAC. For a SharePoint user to be able to perform an eDiscovery search of Exchange mailboxes, they must be assigned delegated Discovery Management permission in Exchange. To be able to preview mailbox content returned in an eDiscovery search performed using SharePoint eDiscovery Center, the discovery manager must have a mailbox in the same Exchange organization.

For step-by step instructions for setting up an eDiscovery Center in a Microsoft 365 or Office 365 organization, see [Set up an eDiscovery Center in SharePoint Online](https://support.microsoft.com/office/a18f8975-aa7f-43b4-a7d6-001d14744d8e).

## eDiscovery in an Exchange hybrid deployment
<a name="oauth"> </a>

To successfully perform cross-premises eDiscovery searches in an Exchange Server hybrid organization, you will have to configure OAuth (Open Authorization) authentication between your Exchange on-premises and Exchange Online organizations so that you can use In-Place eDiscovery to search on-premises and cloud-based mailboxes. OAuth authentication is a server-to-server authentication protocol that allows applications to authenticate to each other.

OAuth authentication supports the following eDiscovery scenarios in an Exchange hybrid deployment:

- Search on-premises mailboxes that use Exchange Online Archiving for cloud-based archive mailboxes.

- Search on-premises and cloud-based mailboxes in the same eDiscovery search.

- Search on-premises mailboxes by using the eDiscovery Center in SharePoint Online.

For more information about the eDiscovery scenarios that require OAuth authentication to be configured in an Exchange hybrid deployment, see [Using Oauth Authentication to Support eDiscovery in an Exchange Hybrid Deployment](https://docs.microsoft.com/exchange/using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help). For step-by-step instructions for configuring OAuth authentication to support eDiscovery, see [Configure OAuth Authentication Between Exchange and Exchange Online Organizations](https://docs.microsoft.com/exchange/configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help).

## Discovery mailboxes
<a name="discmbxs"> </a>

After you create an In-Place eDiscovery search, you can copy the search results to a target mailbox. The EAC allows you to select a discovery mailbox as the target mailbox. A discovery mailbox is a special type of mailbox that provides the following functionality:

- **Easier and secure target mailbox selection**: When you use the EAC to copy In-Place eDiscovery search results, only discovery mailboxes are made available as a repository in which to store search results. You don't need to sort through a potentially long list of mailboxes available in the organization. This also eliminates the possibility of a discovery manager accidentally selecting another user's mailbox or an unsecured mailbox in which to store potentially sensitive messages.

- **Large mailbox storage quota**: The target mailbox should be able to store a large amount of message data that may be returned by an In-Place eDiscovery search. By default, discovery mailboxes have a mailbox storage quota of 50 gigabytes (GB). This storage quota can't be increased.

- **More secure by default**: Like all mailbox types, a discovery mailbox has an associated Active Directory user account. However, this account is disabled by default. Only users explicitly authorized to access a discovery mailbox have access to it. Members of the Discovery Management role group are assigned Full Access permissions to the default discovery mailbox. Any additional discovery mailboxes you create don't have mailbox access permissions assigned to any user.

- **Email delivery disabled**: Although visible in Exchange address lists, users can't send email to a discovery mailbox. Email delivery to discovery mailboxes is prohibited by using delivery restrictions. This preserves the integrity of search results copied to a discovery mailbox.

Exchange Setup creates one discovery mailbox with the display name **Discovery Search Mailbox**. You can use Exchange Online PowerShell to create additional discovery mailboxes. By default, the discovery mailboxes you create won't have any mailbox access permissions assigned. You can assign Full Access permissions for a discovery manager to access messages copied to a discovery mailbox. For details, see [Create a discovery mailbox](create-a-discovery-mailbox.md).

In-Place eDiscovery also uses a system mailbox with the display name **SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}** to hold In-Place eDiscovery metadata. System mailboxes aren't visible in the EAC or in Exchange address lists. In on-premises organizations, before removing a mailbox database where the In-Place eDiscovery system mailbox is located, you must move the mailbox to another mailbox database. If the mailbox is removed or corrupted, your discovery managers are unable to perform eDiscovery searches until you re-create the mailbox. For details, see [Delete and re-create the default discovery mailbox in Exchange](delete-and-re-create-default-discovery-mailbox.md).

## Using In-Place eDiscovery
<a name="using"> </a>

Users who have been added to the Discovery Management role group can perform In-Place eDiscovery searches. You can perform a search using the web-based interface in the EAC. This makes it easier for non-technical users such as records managers, compliance officers, or legal and HR professionals to use In-Place eDiscovery. You can also use Exchange Online PowerShell to perform a search. For more information, see [Create an In-Place eDiscovery search](create-in-place-ediscovery-search.md)

> [!NOTE]
> In on-premises organizations, you can use In-Place eDiscovery to search mailboxes located on Exchange Server Mailbox servers. To search mailboxes located on Exchange 2010 Mailbox servers, use Multi-Mailbox Search on an Exchange 2010 server. > > In a hybrid deployment, which is an environment where some mailboxes exist on your on-premises Mailbox servers and some mailboxes exist in a cloud-based organization, you can perform In-Place eDiscovery searches of your cloud-based mailboxes using the EAC in your on-premises organization. If you intend to copy messages to a discovery mailbox, you must select an on-premises discovery mailbox. Messages from cloud-based mailboxes that are returned in search results are copied to the specified on-premises discovery mailbox. To learn more about hybrid deployments, see [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid).

The **In-Place eDiscovery & Hold** wizard in the EAC allows you to create an In-Place eDiscovery search and also use In-Place Hold to place search results on hold. When you create an In-Place eDiscovery search, a search object is created in the In-Place eDiscovery system mailbox. This object can be manipulated to start, stop, modify, and remove the search. After you create the search, you can choose to get an estimate of search results, which includes keyword statistics that help you determine query effectiveness. You can also do a live preview of items returned in the search, allowing you to view message content, the number of messages returned from each source mailbox and the total number of messages. You can use this information to further fine-tune your query if required.

When satisfied with the search results, you can copy them to a discovery mailbox. You can also use the EAC or Outlook to export a discovery mailbox or some of its content to a PST file.

When creating an In-Place eDiscovery search, you must specify the following parameters:

- **Name**: The search name is used to identify the search. When you copy search results to a discovery mailbox, a folder is created in the discovery mailbox using the search name and the timestamp to uniquely identify search results in a discovery mailbox.

- **Mailboxes**: You can choose to search all mailboxes in your Exchange Server or Exchange Online organization or specify the mailboxes to search. A user's primary and archive mailboxes are included in the search. If you also want to use the same search to place items on hold, you must specify the mailboxes. You can specify a distribution group to include mailbox users who are members of that group. Membership of the group is calculated once when creating the search and subsequent changes to group membership are not automatically reflected in the search.

    In Exchange Online, you can also specify Microsoft 365 groups as a content source so that the group mailbox is searched (or placed on hold). When you add a Microsoft 365 group to an In-Place eDiscovery search, only the group mailbox is searched; the mailboxes of the group members aren't searched.

- **Search query**: You can either include all mailbox content from the specified mailboxes or use a search query to return items that are more relevant to the case or investigation. You can specify the following parameters in a search query:

  - **Keywords**: You can specify keywords and phrases to search message content. You can also use the logical operators **AND**, **OR**, and **NOT**. Additionally, Exchange Server also supports the **NEAR** operator, allowing you to search for a word or phrase that's in proximity to another word or phrase.

    To search for an exact match of a multiple word phrase, you must enclose the phrase in quotation marks. For example, searching for the phrase "plan and competition" returns messages that contain an exact match of the phrase, whereas specifying **plan AND competition** returns messages that contain the words **plan** and **competition** anywhere in the message.

    Exchange Server also supports the Keyword Query Language (KQL) syntax for In-Place eDiscovery searches.

    > [!NOTE]
    > In-Place eDiscovery does not support regular expressions.

    You must capitalize logical operators such as **AND** and **OR** for them to be treated as operators instead of keywords. We recommend that you use explicit parenthesis for any query that mixes multiple logical operators to avoid mistakes or misinterpretations. For example, if you want to search for messages that contain either WordA or WordB AND either WordC or WordD, you must use **(WordA OR WordB) AND (WordC OR WordD)**.

  - **Start and End dates**: By default, In-Place eDiscovery doesn't limit searches by a date range. To search messages sent during a specific date range, you can narrow the search by specifying the start and end dates. If you don't specify an end date, the search will return the latest results every time you restart it.

  - **Senders and recipients**: To narrow down the search, you can specify the senders or recipients of messages. You can use email addresses, display names, or the name of a domain to search for items sent to or from everyone in the domain. For example, to find email sent by or sent to anyone at Contoso, Ltd, specify **\@contoso.com** in the **From** or the **To/cc** field in the EAC. You can also specify **\@contoso.com** in the _Senders_ or _Recipients_ parameters in Exchange Online PowerShell.

  - **Message types**: By default, all message types are searched. You can restrict the search by selecting specific message types such as email, contacts, documents, journal, meetings, notes and Lync content.

The following screenshot shows an example of a search query in the EAC.

![Configure eDiscovery Search Query](../../media/TA_MRM_SearchQuery.png)

When using In-Place eDiscovery, also consider the following:

- **Attachments**: In-Place eDiscovery searches attachments supported by Exchange Search. For details, see [File Formats Indexed by Exchange Search](https://docs.microsoft.com/exchange/file-formats-indexed-by-exchange-search-exchange-2013-help). In on-premises deployments, you can add support for additional file types by installing search filters (also known as an iFilter) for the file type on Mailbox servers.

- **Unsearchable items**: Unsearchable items are mailbox items that can't be indexed by Exchange Search. Reasons they can't be indexed include the lack of an installed search filter for an attached file, a filter error, and encrypted messages. For a successful eDiscovery search, your organization may be required to include such items for review. When copying search results to a discovery mailbox or exporting them to a PST file, you can include unsearchable items.

- **Encrypted items**: Because messages encrypted using S/MIME aren't indexed by Exchange Search, In-Place eDiscovery doesn't search these messages. If you select the option to include unsearchable items in search results, these S/MIME encrypted messages are copied to the discovery mailbox.

- **IRM-protected items**: Messages protected using Information Rights Management (IRM) are indexed by Exchange Search and therefore included in the search results if they match query parameters. Messages must be protected by using an Active Directory Rights Management Services (AD RMS) cluster in the same Active Directory forest as the Mailbox server. For more information, see [Information Rights Management](https://docs.microsoft.com/Exchange/policy-and-compliance/information-rights-management).

    > [!IMPORTANT]
    > When Exchange Search fails to index an IRM-protected message, either due to a decryption failure or because IRM is disabled, the protected message isn't added to the list of failed items. If you select the option to include unsearchable items in search results, the results may not include IRM-protected messages that could not be decrypted. > > To include IRM-protected messages in a search, you can create another search to include messages with .rpmsg attachments. You can use the query string `attachment:rpmsg` to search all IRM-protected messages in the specified mailboxes, whether successfully indexed or not. This may result in some duplication of search results in scenarios where one search returns messages that match the search criteria, including IRM-protected messages that have been indexed successfully. The search doesn't return IRM-protected messages that couldn't be indexed. > > Performing a second search for all IRM-protected messages also includes the IRM-protected messages that were successfully indexed and returned in the first search. Additionally, the IRM-protected messages returned by the second search may not match the search criteria such as keywords used for the first search.

- **De-duplication**: When copying search results to a discovery mailbox, you can enable de-duplication of search results to copy only one instance of a unique message to the discovery mailbox. De-duplication has the following benefits:

  - Lower storage requirement and smaller discovery mailbox size due to reduced number of messages copied.

  - Reduced workload for discovery managers, legal counsel, or others involved in reviewing search results.

  - Reduced cost of eDiscovery, depending on the number of duplicate items excluded from search results.

## Estimate, preview, and copy search results
<a name="estimate"> </a>

After an In-Place eDiscovery search is completed, you can view search result estimates in the Details pane in the EAC. The estimate includes number of items returned and total size of those items. You can also view keyword statistics, which returns details about number of items returned for each keyword used in the search query. This information is helpful in determining query effectiveness. If the query is too broad, it may return a much bigger data set, which could require more resources to review and raise eDiscovery costs. If the query is too narrow, it may significantly reduce the number of records returned or return no records at all. You can use the estimates and keyword statistics to fine-tune the query to meet your requirements.

> [!NOTE]
> In Exchange Server and Exchange Online, keyword statistics also include statistics for non-keyword properties such as dates, message types, and senders/recipients specified in a search query.

You can also preview the search results to further ensure that messages returned contain the content you're searching for and further fine-tune the query if required. eDiscovery Search Preview displays the number of messages returned from each mailbox searched and the total number of messages returned by the search. The preview is generated quickly without requiring you to copy messages to a discovery mailbox.

After you're satisfied with the quantity and quality of search results, you can copy them to a discovery mailbox. When copying messages, you have the following options:

- **Include unsearchable items**: For details about the types of items that are considered unsearchable, see the eDiscovery search considerations in the previous section.

- **Enable de-duplication**: De-duplication reduces the dataset by only including a single instance of a unique record if multiple instances are found in one or more mailboxes searched.

- **Enable full logging**: By default, only basic logging is enabled when copying items. You can select full logging to include information about all records returned by the search.

- **Send me mail when the copy is completed**: An In-Place eDiscovery search can potentially return a large number of records. Copying the messages returned to a discovery mailbox can take a long time. Use this option to get an email notification when the copying process is completed. For easier access using Outlook on the web, the notification includes a link to the location in a discovery mailbox where the messages are copied.

## Export search results to a PST file
<a name="exportsearchresults"> </a>

After search results are copied to a discovery mailbox, you can export the search results to a PST file.

![Export eDiscovery Search Results to a PST File](../../media/TA_ExportSearchResullts.gif)

After search results are exported to a PST file, you or other users can open them in Outlook to review or print messages returned in the search results. For more information, see [Export eDiscovery search results to a PST file](export-search-results.md).

## Different search results
<a name="differentresults"> </a>

Because In-Place eDiscovery performs searches on live data, it's possible that two searches of the same content sources and using the same search query can return different results. Estimated search results can also be different from the actual search results that are copied to a discovery mailbox. This can happen even when rerunning the same search within a short amount of time. There are several factors that can affect the consistency of search results:

- The continual indexing of incoming email because Exchange Search continuously crawls and indexes your organization's mailbox databases and transport pipeline.

- Deletion of email by users or automated processes.

- Bulk importing large amounts of email, which takes time to index.

If you do experience dissimilar results for the same search, consider placing mailboxes on hold to preserve content, running searches during off-peak hours, and allowing time for indexing after importing large amounts of email.

## Logging for In-Place eDiscovery searches
<a name="log"> </a>

There are two types of logging available for In-Place eDiscovery searches:

- **Basic logging**: Basic logging is enabled by default for all In-Place eDiscovery searches. It includes information about the search and who performed it. Information captured about basic logging appears in the body of the email message sent to the mailbox where the search results are stored. The message is located in the folder created to store search results.

- **Full logging**: Full logging includes information about all messages returned by the search. This information is provided in a comma-separated value (.csv) file attached to the email message that contains the basic logging information. The name of the search is used for the .csv file name. This information may be required for compliance or record-keeping purposes. To enable full logging, you must select the **Enable full logging** option when copying search results to a discovery mailbox in the EAC. If you're using Exchange Online PowerShell, specify the full logging option using the _LogLevel_ parameter.

> [!NOTE]
> When using Exchange Online PowerShell to create or modify an In-Place eDiscovery search, you can also disable logging.

Besides the search log included when copying search results to a discovery mailbox, Exchange also logs cmdlets used by the EAC or Exchange Online PowerShell to create, modify or remove In-Place eDiscovery searches. This information is logged in the admin audit log entries. For details, see [View the administrator audit log](../exchange-auditing-reports/view-administrator-audit-log.md).

## In-Place eDiscovery and In-Place Hold
<a name="hold"> </a>

As part of eDiscovery requests, you may be required to preserve mailbox content until a lawsuit or investigation is disposed. Messages deleted or altered by the mailbox user or any processes must also be preserved. In Exchange Server, this is accomplished by using In-Place Hold. For details, see [In-Place Hold and Litigation Hold](../../security-and-compliance/in-place-and-litigation-holds.md).

In Exchange Server, you can use the new **In-Place eDiscovery & Hold** wizard to search items and preserve them for as long as they're required for eDiscovery or to meet other business requirements. When using the same search for both In-Place eDiscovery and In-Place Hold, be aware of the following:

- You can't use the option to search all mailboxes. You must select the mailboxes or distribution groups.

- You can't remove an In-Place eDiscovery search if the search is also used for In-Place Hold. You must first disable the In-Place Hold option in a search and then remove the search.

## Preserving mailboxes for In-Place eDiscovery
<a name="preserve"> </a>

When an employee leaves an organization, it's a common practice to disable or remove the mailbox. After you disable a mailbox, it is disconnected from the user account but remains in the mailbox for a certain period, 30 days by default. The Managed Folder Assistant does not process disconnected mailboxes and any retention policies are not applied during this period. You can't search content of a disconnected mailbox. Upon reaching the deleted mailbox retention period configured for the mailbox database, the mailbox is purged from the mailbox database.

> [!IMPORTANT]
> In Exchange Online, In-Place eDiscovery can search content in inactive mailboxes. Inactive mailboxes are mailboxes that are placed on In-Place Hold or litigation hold and then removed. Inactive mailboxes are preserved as long as they're placed on hold. When an inactive mailbox is removed from In-Place Hold or when litigation hold is disabled, it is permanently deleted. For details, see [Create and manage inactive mailboxes](https://docs.microsoft.com/microsoft-365/compliance/create-and-manage-inactive-mailboxes).

In on-premises deployments, if your organization requires that retention settings be applied to messages of employees who are no longer in the organization or if you may need to retain an ex-employee's mailbox for an ongoing or future eDiscovery search, do not disable or remove the mailbox. You can take the following steps to ensure the mailbox can't be accessed and no new messages are delivered to it.

1. Disable the Active Directory user account using **Active Directory Users & Computers** or other Active Directory or account provisioning tools or scripts. This prevents mailbox logon using the associated user account.

    > [!IMPORTANT]
    > Users with Full Access mailbox permission will still be able to access the mailbox. To prevent access by others, you must remove their Full Access permission from the mailbox. For information about how to remove Full Access mailbox permissions on a mailbox, see [Manage permissions for recipients](../../recipients-in-exchange-online/manage-permissions-for-recipients.md).

2. Set the message size limit for messages that can be sent from or received by the mailbox user to a very low value, 1 KB for example. This prevents delivery of new mail to and from the mailbox.

3. Configure delivery restrictions for the mailbox so nobody can send messages to it. For details, see [Configure message delivery restrictions for a mailbox](../../recipients-in-exchange-online/manage-user-mailboxes/configure-message-delivery-restrictions.md).

> [!IMPORTANT]
> You must take the above steps along with any other account management processes required by your organization, but without disabling or removing the mailbox or removing the associated user account.

When planning to implement mailbox retention for messaging retention management (MRM) or In-Place eDiscovery, you must take employee turnover into consideration. Long-term retention of ex-employee mailboxes will require additional storage on Mailbox servers and also result in an increase in Active Directory database because it requires that the associated user account be retained for the same duration. Additionally, it may also require changes to your organization's account provisioning and management processes.

## In-Place eDiscovery limits and throttling policies
<a name="throttle"> </a>

In Exchange Server and Exchange Online, the resources In-Place eDiscovery can consume are controlled using throttling policies.

The default throttling policy contains the following throttling parameters.

|**Parameter**|**Description**|**Default value**|
|:-----|:-----|:-----|
|DiscoveryMaxConcurrency|The maximum number of In-Place eDiscovery searches that can run at the same time in your organization.|2  <br/> **Note**: If an eDiscovery search is started while two previous searches are still running, the third search won't be queued and will instead fail. You have to wait until one of the previous searches finishes before you can successfully start a new search.|
|DiscoveryMaxMailboxes|The maximum number of mailboxes that can be searched in a single In-Place eDiscovery search.|Exchange Online: 10,000<sup>1</sup> <br/> Exchange Server: 5,000|
|DiscoveryMaxStatsSearchMailboxes|The maximum number of mailboxes that can be searched in a single In-Place eDiscovery search that still allows you to view keyword statistics.|100  <br/> **Note**: After you run an eDiscovery search estimate, you can view keyword statistics. These statistics show details about the number of items returned for each keyword used in the search query. If more than 100 source mailboxes are included in the search, an error will be returned if you try to view keyword statistics.|
|DiscoveryMaxKeywords|The maximum number of keywords that can be specified in a single In-Place eDiscovery search.|500|
|DiscoveryMaxSearchResultsPageSize|The maximum number of items displayed on a single page when previewing In-Place eDiscovery search results.|200|
|DiscoverySearchTimeoutPeriod|The number of minutes that an In-Place eDiscovery search will run before it times out.|10 minutes|

> [!NOTE]
> <sup>1</sup> If you initiate an eDiscovery search from the eDiscovery Center in SharePoint Online in a Microsoft 365 or Office 365 organization, you can search a maximum of 1,500 mailboxes in a single search.

In Exchange Server, you can change the default values for these parameters to suit your requirements or create additional throttling policies and assign them to users with delegated Discovery Management permission. In Exchange Online, the default values for these throttling parameters can't be changed.

## In-Place eDiscovery documentation
<a name="ediscoverydocumentation"> </a>

The following table contains links to topics that will help you learn about and manage In-Place eDiscovery.

|**Topic**|**Description**|
|:-----|:-----|
|[Assign eDiscovery permissions in Exchange](assign-ediscovery-permissions.md)|Learn how to give a user access to use In-Place eDiscovery in the EAC to search Exchange mailboxes. Adding a user to the Discovery Management role group also allows the person to use the eDiscovery Center in SharePoint 2013 and SharePoint Online to search Exchange mailboxes.|
|[Create a discovery mailbox](create-a-discovery-mailbox.md)|Learn how to use Exchange Online PowerShell to create a discovery mailbox and assign access permissions.|
|[Create an In-Place eDiscovery search](create-in-place-ediscovery-search.md)|Learn how to create an In-Place eDiscovery search, and how to estimate and preview eDiscovery search results.|
|[Message properties and search operators for In-Place eDiscovery](message-properties-and-search-operators.md)|Learn which email message properties can be searched using In-Place eDiscovery. The topic provides syntax examples for each property, information about search operators such as **AND** and **OR**, and information about other search query techniques such as using double quotation marks (" ") and prefix wildcards.|
|[Search limits for In-Place eDiscovery in Exchange Online](search-limits.md)|Learn In-Place eDiscovery limits in Exchange Online that help maintain the health and quality of eDiscovery services for Microsoft 365 or Office 365 organizations.|
|[Start or Stop an In-Place eDiscovery Search](https://docs.microsoft.com/microsoft-365/compliance/content-search)|Learn how to start, stop, and restart eDiscovery searches.|
|[Modify an In-Place eDiscovery Search](https://docs.microsoft.com/microsoft-365/compliance/search-for-ediscovery-activities-in-the-audit-log)|Learn how to modify an existing eDiscovery search.|
|[Export eDiscovery search results to a PST file](export-search-results.md)|Learn how to export the results of an eDiscovery search to a PST file.|
|[Create a custom management scope for In-Place eDiscovery searches](create-custom-management-scope.md)|Learn how to use custom management scopes to limit the mailboxes that a discovery manager can search.|
|[Create and run queries in the eDiscovery Center](https://docs.microsoft.com/SharePoint/governance/create-and-run-queries-in-the-ediscovery-center)|Learn how to delete an eDiscovery search.|
|[Search for and delete email messages](https://docs.microsoft.com/microsoft-365/compliance/search-for-and-delete-messages-in-your-organization)|Learn how to use Content Search to search for and then delete email messages.|
|[Reduce the size of a discovery mailbox in Exchange](reduce-discovery-mailbox-size.md)|Use this process to reduce the size of a discovery mailbox that's larger than 50 GB.|
|[Delete and re-create the default discovery mailbox in Exchange](delete-and-re-create-default-discovery-mailbox.md)|Learn how to delete the default discovery mailbox, re-create it, and then reassign permissions to it. Use this procedure if this mailbox has exceeded the 50 GB limit and you don't need the search results.|
|[Using Oauth Authentication to Support eDiscovery in an Exchange Hybrid Deployment](https://docs.microsoft.com/exchange/using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help)|Learn about the eDiscovery scenarios in an Exchange hybrid deployment that require you to configure OAuth authentication.|
|[Configure Exchange for SharePoint eDiscovery Center](https://docs.microsoft.com/exchange/configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help)|Learn how to configure Exchange Server so that you can use the eDiscovery Center in SharePoint 2013 to search Exchange mailboxes.|

For more information about eDiscovery in Microsoft 365, Office 365, Exchange Server, SharePoint 2013, and Lync 2013, see the [Get started with Core eDiscovery](https://docs.microsoft.com/microsoft-365/compliance/get-started-core-ediscovery).
