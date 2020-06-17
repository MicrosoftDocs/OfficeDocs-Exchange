---
localization_priority: Normal
description: 'Summary: Learn how to search for and purge messages from Exchange Server 2016 and Exchange Server 2019 mailboxes.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8c36bb03-e716-4fdd-9958-4aa7a2a1db42
ms.reviewer:
title: Search for and delete messages in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Search for and delete messages in Exchange Server

 You can use the **New-ComplianceSearch** and **New-ComplianceSearchAction** cmdlets to search for and delete an email message from all mailboxes in your organization. This can help you find and remove potentially harmful or high-risk email, such as:

- Messages that contain dangerous attachment or virus

- Phishing messages

- Messages that contain sensitive data

 **Why use the New-ComplianceSearch and New-ComplianceSearchAction cmdlets instead of using the Search-Mailbox cmdlet to delete messages?** In previous versions of Exchange, you could run the `Search-Mailbox -DeleteContent` command to search for and delete email messages. You can still do that in Exchange Server, but you can only search a maximum of 10,000 mailboxes in a single search by using the **Search-Mailbox** cmdlet. For **New-ComplianceSearch**, there are no limits for the number of mailboxes in a single search. This lets large organizations perform organization-wide search and delete operations.

Here's the workflow for the search and delete process:

[Step 1: Create and run a Compliance Search to find the message to delete](#step-1-create-and-run-a-compliance-search-to-find-the-message-to-delete)

[Step 2: Delete the message](#step-2-delete-the-message)

See the [More information](#more-information) section for description of what happens to deleted messages and how to get the status of a search and delete operation.

> [!CAUTION]
> Search and delete is a powerful feature that allows anyone that is assigned the necessary permissions to delete email messages from mailboxes in your organization.

## Before you begin

- To use the **New-ComplianceSearch** and **Start-ComplianceSearchAction** cmdlets to create and run a Compliance Search, and to use the **New-ComplianceSearchAction** cmdlet to delete messages, you have to be assigned the Mailbox Search management role. Administrators aren't assigned this role by default. To assign yourself this role so that you can search mailboxes and delete messages, add yourself as a member of the Discovery Management role group. See [Assign eDiscovery permissions in Exchange Server](assign-permissions.md).

- A maximum of 10 items per mailbox can be removed at once. Because the capability to search for and remove messages is intended to be an incident-response tool, this limit helps ensure that messages are quickly removed from mailboxes. This feature isn't intended to clean up user mailboxes.

## Step 1: Create and run a Compliance Search to find the message to delete

The first step is to create and run a Compliance Search to find the message that you want to remove from mailboxes in your organization. You can create the search by running the **New-ComplianceSearch** and **Start-ComplianceSearch** cmdlets. The messages that match the query for this search will be deleted by running the **New-ComplianceSearchAction** cmdlet in Step 2.

In this example, the commands will create and start a search of all mailboxes in the organization for a message that contains the words "Update your account information" in the subject line.

1. [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

2. Run the following commands.

   ```PowerShell
   New-ComplianceSearch -Name "Remove Phishing Message" -ExchangeLocation all -ContentMatchQuery 'subject:"Update your account information"'
   ```

   ```PowerShell
   Start-ComplianceSearch -Identity "Remove Phishing Message"
   ```

For information about creating a Compliance Search and configuring search queries, see the following topics:

- [New-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/new-compliancesearch)

- [Start-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/start-compliancesearch)

- [Message properties and search operators for In-Place eDiscovery in Exchange Server](message-properties-and-search-operators.md)

### Tips for finding messages to remove

The goal of the search query is to narrow the results of the search to only the message or messages that you want to remove. Here are some tips:

- If you know the exact text or phrase used in the subject line of the message, use the **Subject** property in the search query.

- If you know that exact date (or date range) of the message, include the **Received** property in the search query.

- If you know who sent the message, include the **From** property in the search query.

- Preview the search results to verify that the search returned only the message (or messages) that you want to delete.

- Use the search estimate statistics (by running the [Get-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/get-compliancesearch) cmdlet) to get a count of the total number of search results.

Here are two examples of queries to find suspicious email messages.

- This query returns messages that were received by users between April 13, 2016 and April 14, 2016 and that contain the words "action" and "required" in the subject line.

  ```PowerShell
  (Received:4/13/2016..4/14/2016) AND (Subject:'Action required')
  ```

- This query returns messages that were sent by chatsuwloginsset12345@outlook.com and that contain the exact phrase "Update your account information" in the subject line.

  ```PowerShell
  (From:chatsuwloginsset12345@outlook.com) AND (Subject:"Update your account information")
  ```

## Step 2: Delete the message


After you've created and refined a Compliance Search to return the message that you want to remove, the final step is to run the **New-ComplianceSearchAction** cmdlet to delete the message. Deleted messages are moved to a user's Recoverable Items folder.

In this example, the command will delete the search results returned by a Compliance Search named "Remove Phishing Message".

1. [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

2. Run the following command.

   ```PowerShell
   New-ComplianceSearchAction -SearchName "Remove Phishing Message" -Purge -PurgeType SoftDelete
   ```

## More information

- **What happens after you delete a message?**: A message that is deleted by using the `New-ComplianceSearchAction -Purge -PurgeType SoftDelete` command is moved to the Deletions folder in the user's Recoverable Items folder. It isn't immediately purged from the Exchange database. The user can recover messages in the Deleted Items folder for the duration based on the deleted item retention period configured for the mailbox. After this retention period expires (or if user purges the message before it expires), the message is moved to the Purges folder and can no longer be accessed by the user. Once in the Purges folder, the message is again retained for the duration based on the deleted item retention period configured for the mailbox if single items recovery is enabled for the mailbox. (In Exchange, single item recovery is enabled by default when a new mailbox is created. ) After the deleted item retention period expires, the message is marked from permanent deletion and will be purged from the Exchange database the next time that the mailbox is processed by the Managed Folder assistant.

- **How do you know that messages are deleted and moved to the users' Recoverable Items folder?**: If you run the same Compliance Search after you delete a message, you will still see the same number of search results (and might assume that the message wasn't deleted from user mailboxes). This is because a Compliance Search searches the Recoverable Items folder, which is where the deleted message is moved to after you run the `New-ComplianceSearchAction -Purge -PurgeType SoftDelete` command. To verify that messages where moved to the Recoverable Items folder, you can run an In-Place eDiscovery search (using the same source mailboxes and search criteria as the Compliance Search created in Step 1) and the copy the search results to discovery mailbox. Then you can view the search results in the discovery mailbox and verify that the messages was moved to the Recoverable Items folder. See [Use Compliance Search to search all mailboxes in Exchange Server](compliance-search.md) for details about creating an In-Place eDiscovery search that uses the list of source mailboxes and search query from a Compliance Search.

- **What happens if a message is deleted from a mailbox that has been placed on In-Place Hold or Litigation Hold?**: After the message is purged (either by the user or after the deleted item retention period expires), the message is retained until the hold duration expires. If the hold duration is unlimited, then items are retained until the hold is removed or the hold duration is changed.

- **How to get status on the search and delete operation?** Run the **Get-ComplianceSearchAction**: to get the status on the delete operation. Note that the object that is created when you run the **New-ComplianceSearchAction** cmdlet is named by using this format: `<name of Compliance Search>_Purge`.
