---
localization_priority: Normal
description: This topic describes the properties of Exchange email messages that you can search by using In-Place eDiscovery & Hold in Exchange Server and Exchange Online. The topic also describes Boolean search operators and other search query techniques that you can use to refine eDiscovery search results.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 402b74e4-8853-4c51-9737-1a9c19f8e3dd
ms.reviewer: 
f1.keywords:
- NOCSH
title: Message properties and search operators for In-Place eDiscovery
ms.collection: 
- exchange-online
- M365-email-calendar
audience: Admin
ms.service: exchange-online
manager: serdars

---

# Message properties and search operators for In-Place eDiscovery

This topic describes the properties of Exchange email messages that you can search by using In-Place eDiscovery & Hold in Exchange Server and Exchange Online. The topic also describes Boolean search operators and other search query techniques that you can use to refine eDiscovery search results.

In-Place eDiscovery uses Keyword Query Language (KQL). For more details, see [Keyword Query Language syntax reference](https://go.microsoft.com/fwlink/p/?LinkId=269603).

## Searchable properties in Exchange

The following table lists email message properties that can be searched using an In-Place eDiscovery search or by using the **New-MailboxSearch** or the **Set-MailboxSearch** cmdlet. The table includes an example of the _property:value_ syntax for each property and a description of the search results returned by the examples.

|**Property**|**Property description**|**Examples**|**Search results returned by the examples**|
|:-----|:-----|:-----|:-----|
|Attachment|The names of files attached to an email message.|attachment:annualreport.ppt <br/><br/> attachment:annual\*|Messages that have an attached file named annualreport.ppt. <br/> In the second example, using the wildcard returns messages with the word "annual" in the file name of an attachment.|
|Bcc|The BCC field of an email message.<sup>1</sup>|bcc:pilarp@contoso.com <br/><br/> bcc:pilarp <br/><br/> bcc:"Pilar Pinilla"|All examples return messages with Pilar Pinilla included in the Bcc field.|
|Category| The categories to search. Categories can be defined by users by using Outlook or Outlook on the web (formerly known as Outlook Web App). The possible values are: <br/>•  blue <br/>• green <br/>• orange <br/>• purple <br/>• red <br/>•  yellow|category:"Red Category"|Messages that have been assigned the red category in the source mailboxes.|
|Cc|The CC field of an email message.<sup>1</sup>|cc:pilarp@contoso.com <br/><br/> cc:"Pilar Pinilla"|In both examples, messages with Pilar Pinilla specified in the CC field.|
|From|The sender of an email message.<sup>1</sup>|from:pilarp@contoso.com <br/><br/> from:contoso.com|Messages sent by the specified user or sent from a specified domain.|
|Importance|The importance of an email message, which a sender can specify when sending a message. By default, messages are sent with normal importance, unless the sender sets the importance as **high** or **low**.|importance:high <br/><br/> importance:medium <br/><br/> importance:low|Messages that are marked as high importance, medium importance, or low importance.|
|Kind| The message type to search. Possible values: <br/>• contacts <br/>• docs <br/>• email <br/>• faxes <br/>• im <br/>• journals <br/>• meetings <br/>• notes <br/>• posts <br/>• rssfeeds <br/>• tasks <br/>• voicemail|kind:email <br/><br/> kind:email OR kind:im OR kind:voicemail|Email messages that meet the search criteria. The second example returns email messages, instant messaging conversations, and voice messages that meet the search criteria.|
|Participants|All the people fields in an email message; these fields are From, To, CC, and BCC.<sup>1</sup>|participants:garthf@contoso.com <br/><br/> participants:contoso.com|Messages sent by or sent to garthf@contoso.com. <br/> The second example returns all messages sent by or sent to a user in the contoso.com domain.|
|Received|The date that an email message was received by a recipient.|received:04/15/2014 <br/><br/> received\>=01/01/2014 AND received\<=03/31/2014|Messages that were received on April 15, 2014. The second example returns all messages received between January 1, 2014 and March 31, 2014.|
|Recipients|All recipient fields in an email message; these fields are To, CC, and BCC.<sup>1</sup>|recipients:garthf@contoso.com <br/><br/> recipients:contoso.com|Messages sent to garthf@contoso.com. <br/> The second example returns messages sent to any recipient in the contoso.com domain.|
|Sent|The date that an email message was sent by the sender.|sent:07/01/2014 <br/><br/> sent\>=06/01/2014 AND sent\<=07/01/2014|Messages that were sent on July 01, 2014. The second example returns all messages sent between June 01, 2014 and July 01, 2014.|
|Size|The size of an item, in bytes.|size\>26214400 <br/><br/> size:1..1048576|Messages larger than 25 MB. <br/> The second example returns messages from 1 through 1,048,576 bytes (1 MB) in size.|
|Subject|The text in the subject line of an email message.|subject:"Quarterly Financials" <br/><br/> subject:northwind|Messages that contain the exact phrase "Quarterly Financials" anywhere in the text of the subject line. <br/> The second example returns all messages that contain the word northwind in the subject line.|
|To|The To field of an email message.<sup>1</sup>|to:annb@contoso.com <br/><br/> to:annb <br/><br/> to:"Ann Beebe"|All examples return messages where Ann Beebe is specified in the To: line.|

> [!NOTE]
> <sup>1</sup> For the value of a recipient property, you can use the SMTP address, display name, or alias to specify a user. For example, you can use annb@contoso.com, annb, or "Ann Beebe" to specify the user Ann Beebe.

## Supported search operators

Boolean search operators, such as **AND**, **OR**, help you define more-precise mailbox searches by including or excluding specific words in the search query. Other techniques, such as using property operators (such as \>= or ..), quotation marks, parentheses, and wildcards, help you refine eDiscovery search queries. The following table lists the operators that you can use to narrow or broaden search results.

> [!IMPORTANT]
> You must use uppercase Boolean operators in a search query. For example, use **AND**; don't use **and**. Using lowercase operators in search queries will return an error.

|**Operator**|**Usage**|**Description**|
|:-----|:-----|:-----|
|AND|keyword1 AND keyword2|Returns messages that include all of the specified keywords or `property:value` expressions.|
|+|keyword1 +keyword2 +keyword3|Returns items that contain *either* `keyword2` or `keyword3` *and* that also contain `keyword1`. Therefore, this example is equivalent to the query `(keyword2 OR keyword3) AND keyword1`. <br/> Note that the query `keyword1 + keyword2` (with a space after the **+** symbol) isn't the same as using the **AND** operator. This query would be equivalent to `"keyword1 + keyword2"` and return items with the exact phase `"keyword1 + keyword2"`.|
|OR|keyword1 OR keyword2|Returns messages that include one or more of the specified keywords or `property:value` expressions.|
|NOT|keyword1 NOT keyword2 <br/><br/> NOT from:"Ann Beebe"|Excludes messages specified by a keyword or a `property:value` expression. For example, `NOT from:"Ann Beebe"` excludes messages sent by Ann Beebe.|
|-|keyword1 -keyword2|The same as the **NOT** operator. This query returns items that contain `keyword1` and excludes items that contain `keyword2`.|
|NEAR|keyword1 NEAR(n) keyword2|Returns messages with words that are near each other, where n equals the number of words apart. For example, `best NEAR(5) worst` returns messages where the word "worst" is within five words of "best". If no number is specified, the default distance is eight words.|
|:|property:value|The colon (:) in the `property:value` syntax specifies that the property value being searched for equals the specified value. For example, `recipients:garthf@contoso.com` returns any message sent to garthf@contoso.com.|
|\<|property\<value|Denotes that the property being searched is less than the specified value. <sup>1</sup>|
|\>|property\>value|Denotes that the property being searched is greater than the specified value.<sup>1</sup>|
|\<=|property\<=value|Denotes that the property being searched is less than or equal to a specific value.<sup>1</sup>|
|\>=|property\>=value|Denotes that the property being searched is greater than or equal to a specific value.<sup>1</sup>|
|..|property:value1..value2|Denotes that the property being searched is greater than or equal to value1 and less than or equal to value2.<sup>1</sup>|
|" "|"fair value" <br/><br/> subject:"Quarterly Financials"|Use double quotation marks (" ") to search for an exact phrase or term in keyword and `property:value` search queries.|
|\*|cat\* <br/><br/> subject:set\*|Prefix wildcard searches (where the asterisk is placed at the end of a word) match for zero or more characters in keywords or `property:value` queries. For example, `subject:set*` returns messages that contain the word set, setup, and setting (and other words that start with "set") in the subject line.|
|( )|(fair OR free) AND from:contoso.com <br/><br/> (IPO OR initial) AND (stock OR shares) <br/><br/> (quarterly financials)|Parentheses group together Boolean phrases, `property:value` items, and keywords. For example, `(quarterly financials)` returns items that contain the words quarterly and financials.|

> [!NOTE]
> <sup>1</sup> Use this operator for properties that have date or numeric values.

## Unsupported characters in search queries

Unsupported characters in a search query typically cause a search error or return unintended results. Unsupported characters are often hidden and they're typically added to a query when you copy the query or parts of the query from other applications (such as Microsoft Word or Microsoft Excel) and copy them to the keyword box on the query page of In-Place eDiscovery search.

Here's a list of the unsupported characters for an In-Place eDiscovery search query.

- **Smart quotation marks**: Smart single and double quotation marks (also called curly quotes) aren't supported. Only straight quotation marks can be used in a search query.

- **Non-printable and control characters**: Non-printable and control characters don't represent a written symbol, such as a alpha-numeric character. Examples of non-printable and control characters include characters that format text or separate lines of text.

- **Left-to-right and right-to-left marks**: These are control characters used to indicate text direction for left-to-right languages (such as English and Spanish) and right-to-left languages (such as Arabic and Hebrew).

- **Lowercase Boolean operators**: As previous explained, you have to use uppercase Boolean operators, such as **AND** and **OR**, in a search query. Note that the query syntax will often indicate that a Boolean operator is being used even though lowercase operators might be used; for example, `(WordA or WordB) and (WordC or WordD)`.

 **How to prevent unsupported characters in your search queries?** The best way to prevent unsupported characters is to just type the query in the keyword box. Alternatively, you can copy a query from Word or Excel and then paste it to file in a plain text editor, such as Microsoft Notepad. Then save the text file and select **ANSI** in the **Encoding** drop-down list. This will remove any formatting and unsupported characters. Then you can copy and paste the query from the text file to the keyword query box.

## Search tips and tricks

- Keyword searches are not case sensitive. For example, **cat** and **CAT** return the same results.

- A space between two keywords or two `property:value` expressions is the same as using **AND**. For example, `from:"Sara Davis" subject:reorganization` returns all messages sent by Sara Davis that contain the word **reorganization** in the subject line.

- Use syntax that matches the `property:value` format. Values are not case-sensitive, and they can't have a space after the operator. If there is a space, your intended value will just be full-text searched. For example **to: pilarp** searches for "pilarp" as a keyword, rather than for messages that were sent to pilarp.

- When searching a recipient property, such as To, From, Cc, or Recipients, you can use an SMTP address, alias, or display name to denote a recipient. For example, you can use pilarp@contoso.com, pilarp, or "Pilar Pinilla".

- You can use only prefix wildcard searches (for example, **cat\*** or **set\***). Suffix wildcard searches (\*cat) or substring wildcard searches (\*cat\*) aren't supported.

- When searching a property, use double quotation marks (" ") if the search value consists of multiple words. For example **subject:budget Q1** returns messages that contain **budget** in the in the subject line and that contain **Q1** anywhere in the message or in any of the message properties. Using **subject:"budget Q1"** returns all messages that contain **budget Q1** anywhere in the subject line.
