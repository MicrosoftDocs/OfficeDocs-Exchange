---
localization_priority: Normal
description: Various types of limits are applied to In-Place eDiscovery searches in Exchange Online and Office 365. These limits help to maintain the health and quality of services provided to Microsoft 365 or Office 365 organizations. In most cases, you can't modify these limits, but you should be aware of them so that you can take these limits into consideration when planning, running, and troubleshooting eDiscovery searches.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 65864987-f734-4fab-be97-1ba190a083d4
ms.reviewer: 
f1.keywords:
- NOCSH
title: Search limits for In-Place eDiscovery in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid: MET150
audience: Admin
ms.service: exchange-online
manager: serdars

---

# Search limits for In-Place eDiscovery in Exchange Online

Various types of limits are applied to In-Place eDiscovery searches in Exchange Online. These limits help to maintain the health and quality of services provided to Exchange Online organizations. In most cases, you can't modify these limits, but you should be aware of them so that you can take these limits into consideration when planning, running, and troubleshooting eDiscovery searches.

## Source mailbox limits
<a name="mailboxlimits"> </a>

In-Place eDiscovery has limits on the number of source mailboxes that can be searched in a single search. The following table describes these limits and suggests alternative ways to work around them. These limits apply to eDiscovery searches created by using the Exchange admin center (EAC) or Remote Windows PowerShell.

|**Description of limit**|**Limit**|**More information and suggested workarounds**|
|:-----|:-----|:-----|
|The maximum number of mailboxes that can be searched in a single In-Place eDiscovery search.|10,000|If you have more than 10,000 mailboxes in your organization, you won't be able to use the **Search all mailboxes** option on the **Mailboxes** page in the EAC. To search large numbers of mailboxes (up to 10,000 mailboxes total), you can organize users into distribution groups or dynamic distribution groups and then specify a group on the **Mailboxes** page in the EAC. <sup>1</sup> <br/> |
|The maximum number of mailboxes that can be searched in a single In-Place eDiscovery search that still allows you to view keyword statistics.|100|After you run an eDiscovery search estimate, you can view keyword statistics. These statistics show details about the number of items returned for each keyword used in the search query. If more than 100 source mailboxes are included in the search, an error will be returned if you try to view keyword statistics. <br/> To view keyword statistics, reduce the number of source mailboxes to 100 or fewer, and then rerun the search estimate. When you're satisfied with the search query, you can add additional source mailboxes to the search and then copy or export the search results.|
|The maximum number of mailboxes that can be placed on In-Place Hold in a single In-Place eDiscovery search.|10,000|You can place up to 10,000 mailboxes on In-Place Hold by using a single eDiscovery search. However, if you select the **Search all mailboxes** option on the **Sources** page, you won't be able to enable an In-Place Hold for that search. To place a large number of mailboxes on hold using a single In-Place Hold, use distribution groups or dynamic distribution groups to group mailboxes together, and then specify one of those groups on the **Mailboxes** page in the EAC. <sup>1</sup> <br/> A better option for placing a hold on a large number of mailboxes is to use a Litigation Hold. Using lots of single In-Place eDiscovery searches to place mailboxes on hold isn't recommended.|

> [!NOTE]
> <sup>1</sup> Group membership is calculated only when the search or a hold is created. If a user gets added to the group after the search is created, the user's mailbox won't be added automatically as a source mailbox. You'll have to edit the search and add the mailbox. The same thing applies when a user is removed from a group that is used to create a search or hold. You'll have to edit the search to remove the mailbox.

## Exchange admin center limits
<a name="eaclimits"> </a>

There are also limits when you use the EAC to create and run In-Place eDiscovery searches. These limits are primarily related to the number of source mailboxes that are displayed in the EAC when you select source mailboxes to search. The following table describes these limits and suggests alternative ways to work around them.

|**Description of limit**|**Limit**|**More information and suggested workarounds**|
|:-----|:-----|:-----|
|The maximum number of mailboxes that are displayed in the mailbox picker for selecting source mailboxes when creating a new In-Place eDiscovery or In-Place Hold search.|500| Only 500 mailboxes, distribution groups, and dynamic distribution groups are listed in the mailbox picker to select source mailboxes from when you create a new search. A message is displayed saying that there are more recipients than the ones displayed. Here are some workarounds for this limit:  <br/>  Use the search box to find a mailbox that isn't listed in the mailbox picker. <br/>  Use distribution groups or dynamic distribution groups to group large numbers of mailboxes together. Then pick the group from the mailbox list or search for it using the search box. Groups are expanded into source mailboxes when you create an eDiscovery search. <br/>  Select **Search all mailboxes** on the **Mailbox** page if your organization has less than 10,000 mailboxes and you're not going to place mailboxes on hold. <br/>  Use distribution groups or dynamic distribution groups to group users if you want to place more than 500 mailboxes on In-Place Hold.|
|The maximum number of mailboxes that are displayed when editing an In-Place eDiscovery or In-Place Hold search.|3,000|Up to 3,000 mailboxes are displayed on the **Sources** page in the EAC when you edit an In-Place eDiscovery search or hold. To add a mailbox to the list of sources, you can use the search box to find a mailbox that isn't listed in the mailbox picker (a maximum of 500 recipients are listed in the mailbox picker). To remove a mailbox that's listed, you can select it and then click **Remove**. To remove a mailbox that isn't listed, you have to use Exchange Online PowerShell to remove it. For example, the following commands are run to remove the user Ann Beebe from an In-Place Hold named ContosoHold. <br/> `$SourceMailboxes = Get-MailboxSearch "ContosoHold"` <br/> `$SourceMailboxes.Sources.Remove("/o=contoso/ou=Exchange Administrative Group (FYDIBOHF23SPDLT)/cn=Recipients/cn=28e3edb87e29422998ec8f3a946dd1e5-annb")` <br/> `Set-MailboxSearch "ContosoHold" -SourceMailboxes $SourceMailboxes.Sources` <br/> The first command creates a variable that contains the properties of ContosoHold. The second command removes the user Ann Beebe (by specifying the value of the **LegacyExchangeDN** property) from the list of source mailboxes. The third command edits ContosoHold with the updated list of source mailboxes. <br/> To add a user to an In-Place Hold, use the following syntax in the second command in the previous example. <br/> `$SourceMailboxes.Sources.Add("<LegacyExchangeDN of the user>")` <br/> **Note**: The **Sources** property of an In-Place eDiscovery search or an In-Place Hold identifies the source mailboxes by their **LegacyExchangeDN** property. Because this property uniquely identifies a user mailbox, using the **Sources** property helps prevent adding or removing the wrong mailbox. This also helps to avoid issues if two mailboxes have the same alias or primary SMTP address.|

## Other limits
<a name="otherlimits"> </a>

The following table describes other limits that affect In-Place eDiscovery searches.

|**Description of limit**|**Limit**|**More information**|
|:-----|:-----|:-----|
|The maximum number of In-Place eDiscovery searches that can run at the same time in your organization.|2|If an eDiscovery search is started while two previous searches are still running, the third search won't be queued and will instead fail. You have to wait until one of the running searches is completed before you can successfully start a new search. <br/> Also, estimate-only and copy searches are both considered In-Place eDiscovery searches. So, if you are running an estimate-only search and a copy search at the same time, you can't start another search until one of the running searches is completed. However, you can preview or export the search results from another search while two searches are running.|
|The maximum number of keywords that can be specified in a single In-Place eDiscovery search query.|500|Boolean operators, such as **AND** and **OR** aren't counted against the total number of keywords. For example, the keyword query `cat AND dog AND bird AND fish` consists of four keywords.|
|The maximum number of items displayed on the search preview page when previewing In-Place eDiscovery search results.|200|When you preview search results, the mailboxes that were searched are listed in the right pane on the eDiscovery search preview page. For each mailbox, the number of items returned and the total size of these items are also displayed. Items returned by the search are listed in the right pane. Up to 200 items are displayed on the preview page. <br/> **Note**: Items from each mailbox can't be displayed in the right pane by clicking a mailbox in the left pane. To view the items returned from a specific mailbox, you can copy the search results and view the items in the discovery mailbox.|
|The maximum number of keywords that can be specified in all In-Place Holds placed on a single mailbox.|500|If multiple In-Place Holds are placed on a user's mailbox, the maximum number of keywords in all search queries is 500. That's because Exchange Online combines all the keyword search parameters from of all In-Place Holds by using the **OR** operator. If there are more than 500 keywords in the hold queries, then all content in the mailbox is placed on hold (and not just that content that matches the search criteria of any query-based hold). All content is held until the total number of keywords in all In-Place Holds is reduced to 500 or less. Holding all mailbox content is similar in functionality to a Litigation Hold.|
|Maximum number of variants returned when using a prefix wildcard to search for an exact phrase in a keyword search query or when using a prefix wildcard and the **NEAR** operator.|10,000|For non-phrase queries we use a special prefix index. This only tells us that a word occurs in a document, not where in the document it occurs. To do a phrase query we need to compare the position within the document for the words in the phrase. This means that we cannot use the prefix index for phrase queries. In this case we are internally expanding the query with all possible words that the prefix expands to (i.e. "time\*" can expand to "time OR timer OR times OR timex OR timeboxed OR ..."). 10,000 is the maximum number of variants the word can expand to, not the number of documents matching the query. For non-phrase terms there are no upper limit.|
