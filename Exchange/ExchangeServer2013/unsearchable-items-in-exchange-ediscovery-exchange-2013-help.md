---
title: 'Unsearchable items in Exchange eDiscovery: Exchange 2013 Help'
TOCTitle: Unsearchable items in Exchange eDiscovery
ms:assetid: 32550081-9af9-474b-ae7b-28f1e68cad41
ms:mtpsurl: https://technet.microsoft.com/library/Dn602498(v=EXCHG.150)
ms:contentKeyID: 61071880
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Unsearchable items in Exchange eDiscovery

_**Applies to:** Exchange Server 2013_

In In-Place eDiscovery for Exchange Server 2013 and Exchange Online, unsearchable items are mailbox items that can't be indexed by Exchange Search or that have been only partially indexed. An unsearchable item typically contains a file, which can't be indexed, attached to an email message. Here are a few reasons why files can't be indexed for search and are returned as an unsearchable item when attached to an email message:

- The file type is unsupported for indexing because a search filter (also known as an *IFilter*) to index the file format isn't installed.

- The file type is disabled for indexing.

- The file type is supported for indexing but an indexing error occurred for a specific file.

- A file is encrypted with non-Microsoft technologies.

- A file is password-protected.

For a successful eDiscovery search, your organization may be required to review unsearchable items. You can specify whether to include unsearchable items when you copy eDiscovery search results to a discovery mailbox or export results to a PST file.

## File types not supported for search

Certain types of files, such as Bitmap or MP3 files, don't contain content that can be indexed. As a result, Exchange Search doesn't perform full-text indexing on these types of files. These types of files are considered as unsupported file types. There are also file types for which full-text indexing has been disabled, either by default or by an administrator. Unsupported and disabled file types are considered unsearchable items in eDiscovery searches. These types of files, which are typically attached to an email message, are included in the result set when you include unsearchable items when copying or exporting search results. For a list of supported and disabled file formats, see [File formats indexed by Exchange Search](file-formats-indexed-by-exchange-search-exchange-2013-help.md). In Exchange Server 2013, administrators can disable indexing for a supported file format by using the [Set-SearchDocumentFormat](https://docs.microsoft.com/powershell/module/exchange/Set-SearchDocumentFormat) cmdlet. This cmdlet isn't available in Exchange Online.

To identify the unsearchable items in a specific mailbox, you can run the [Get-FailedContentIndexDocuments](https://docs.microsoft.com/powershell/module/exchange/Get-FailedContentIndexDocuments) cmdlet to get a list of items that would be copied or exported when you choose to include unsearchable items with the search results.

## Messages with unsupported file types returned in search results

Not every message with an unsupported file attachment is automatically returned as an unsearchable item. That's because other file properties, such as the filename, are indexed and available to be searched. For example, a keyword search for "financial" will return a message with an unsupported file attachment if that keyword appears in the file name. If the keyword only appears in the body of the attached file, the message would be returned as an unsearchable item.

Similarly, messages with unsupported file attachments are included in search results when other properties of a mailbox item, which are indexed and searchable, meet the search criteria. Message properties that are indexed for search include sent and received dates, sender and recipient, the file name of an attachment, and text in the message body. So even though a message attachment may be unsearchable, the message will be included in the regular search results if the value of other message properties matches the search criteria. In fact, it's common that messages with unsearchable attachments are included in the regular search results.

For a list of email message properties indexed for search, see [Message properties indexed by Exchange Search](message-properties-indexed-by-exchange-search-exchange-2013-help.md).

## Including unsearchable items in the search results

Your organization may be required to identify and perform additional processing on unsearchable items to determine what they are, what they contain, and whether they're relevant. To include unsearchable items with the eDiscovery search results, you can use the unsearchable items option when you copy or export search results. To include unsearchable items when using In-Place eDiscovery in Exchange or Exchange Online, select the **Include unsearchable items** option when copying search results to a discovery mailbox or exporting them to a PST file. To include unsearchable items when using the eDiscovery Center in SharePoint or SharePoint Online, select the **Include items that are encrypted or have an unrecognized format** option.

Keep the following in mind when copying or exporting unsearchable items:

- When you copy unsearchable items to a discovery mailbox, any unsearchable items are copied to a separate folder named **Unsearchable**, which is located under the folder that contains the search results. When you export search results and include unsearchable items, the unsearchable items are exported to a separate PST file.

- When you include unsearchable items in the search results, all unsearchable items in the mailboxes that are searched will be returned, regardless of the search criteria.

- If you choose to include all mailbox items in the search results or if a search query doesn't specify any keywords or only specifies a date range, unsearchable items may not be copied to the **Unsearchable** folder if you select the option to include unsearchable items. This is because all items, including any unsearchable items, will be automatically included in the regular search results.

- As previously stated, because message properties and metadata are indexed, a keyword search may return results if that keyword appears in the properties or metadata of a message with an unsearchable file attached to it. In this case, two copies of the same mailbox item will also be included in the search results. To prevent this type of duplication and only include one copy of the item in the regular search results, you can select the **Enable deduplication** option when you copy or export the search results.

- Including unsearchable items in the search results can also affect the estimated search results that are displayed. If you include unsearchable items when copying search results, the total estimated item count and the total estimated size will include the unsearchable items.

For more information about including unsearchable items in search results, see:

- [Create an In-Place eDiscovery search](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/create-in-place-ediscovery-search)

- [Export eDiscovery search results to a PST file](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/export-search-results)

- [SharePoint: Export eDiscovery content and create reports](https://docs.microsoft.com/SharePoint/governance/export-content-and-create-reports-in-the-ediscovery-center)

## More information about unsearchable items

- Although a file type supported by Exchange Search and is full-text indexed, there can be indexing or search errors that will cause a file to be returned as an unsearchable item. For example, searching a very large Excel file might be partially successful, but will then fail as the size limit is exceeded. In this case, it's possible that the same file is returned with the search results and as an unsearchable item.

- Attached files encrypted with Microsoft technologies are indexed by Exchange Search and will be searched. Files encrypted with non-Microsoft technologies are returned as unsearchable.

- Email messages encrypted with S/MIME aren't indexed and are considered unsearchable items. This includes encrypted messages with or without file attachments.

- Messages protected using Information Rights Management (IRM) are indexed by Exchange Search and included in the search results if they match query parameters. For more information about IRM, see [Information Rights Management](information-rights-management-exchange-2013-help.md).

- As previously stated, because message properties and metadata are indexed, a keyword search may return results if that keyword appears in the indexed metadata. However, that same keyword search may not return the same item if the keyword only appears in the content of an attached item with an unsupported file type. In this case, the item would be returned only as an unsearchable item.

- In eDiscovery in Exchange 2010, there is the concept of a *safelist*. These are file types that contain content that isn't searchable and so aren't indexed by Exchange Search; for example, Windows Media Video (.wmv) and Waveform Audio (.wav) files. Because these types of files don't contain searchable content, they aren't considered unsearchable items in Exchange 2010. Mailbox items containing these file types weren't returned as unsearchable items and weren't copied to a discovery mailbox.

  There is no longer a safelist in Exchange 2013 or Exchange Online. File types are either enabled or disabled for indexing or they are unsupported. Disabled and unsupported file types are considered unsearchable items.
