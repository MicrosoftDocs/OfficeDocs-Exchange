---
localization_priority: Normal
description: 'Summary: Learn how to view default and custom NDRs, and how to create, modify, and delete custom NDRs in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 23c9d844-6fc7-44c9-a308-587338281611
ms.reviewer:
title: Procedures for DSNs and NDRs in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Procedures for DSNs and NDRs in Exchange Server

Like previous versions of Exchange, Exchange Server uses delivery status notifications (also known as DSNs, non-delivery reports, NDRs, or bounce messages) to provide delivery status and failure notification messages to message senders. For more information about NDRs, see [DSNs and NDRs in Exchange Server](non-delivery-reports-and-bounce-messages.md).

You can use the default NDRs that are included in Exchange, or you can use the Exchange Management Shell to create NDRs with custom text to meet the needs of your organization. The custom NDR text replaces the default text for a given enhanced status code or quota event. If you remove the custom NDR, the default NDR text is used (you can't completely remove a default NDR). You can also disable custom NDRs to preserve them, but not use them (the default NDR text is used).

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 10 minutes.

- The main focus of this topic is custom NDR text that replaces the text of default NDRs that are used by Exchange. You can create new NDRs for other enhanced status code values (for example, 5.999.999), but no one will see these NDRs if the enhanced status code isn't used by Exchange. You can use a range of custom enhanced status codes as part of an action for a mail flow rule (also known as a transport rule). For more information, see [Mail flow rule actions in Exchange Server](../../policy-and-compliance/mail-flow-rules/actions.md).

- The procedures in this topic are available on Mailbox servers and Edge Transport servers.

- You can't use the Exchange admin center (EAC) for most of the procedures in this topic. You need to use the Exchange Management Shell. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "DSNs" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to view all default NDRs

To output the list of all default NDRs in all languages to an HTML file named C:\My Documents\Default NDRs.html, run this command:

```PowerShell
Get-SystemMessage -Original | Select-Object -Property Identity,DsnCode,Language,Text | ConvertTo-Html | Set-Content -Path "C:\My Documents\Default NDRs.html"
```

 **Note**: You should output the list to a file, because the list is very long, and you'll receive errors if you don't have the required language packs installed.

For detailed syntax and parameter information, see [Get-SystemMessage](https://docs.microsoft.com/powershell/module/exchange/get-systemmessage).

## Use the Exchange Management Shell to view custom NDRs

To view a summary list of all custom NDRs in your organization, run this command:

```PowerShell
Get-SystemMessage
```

 **Note**: By default, there are no custom NDRs, so this command returns no results.

To view detailed information for a custom NDR, use this syntax:

```PowerShell
Get-SystemMessage -Identity <NDRIdentity>
```

For an explanation of the available _\<NDRIdentity\>_ values, see the [Identity values for NDRs](#identity-values-for-ndrs) section in this topic.

This example returns detailed information for the custom NDR for the enhanced status code 5.1.2 that's sent to internal senders in English. If there's no custom NDR for this combination of language, audience, and enhanced status code, you'll receive an error.

```PowerShell
Get-SystemMessage En\Internal\5.1.2 | Format-List
```

This example returns detailed information for the custom English NDR for the **ProhibitSendReceive** quota on mailboxes. If there's no custom NDR for this combination of language and quota, you'll receive an error.

```PowerShell
Get-SystemMessage En\ProhibitSendReceiveMailBox | Format-List
```

For detailed syntax and parameter information, see [Get-SystemMessage](https://docs.microsoft.com/powershell/module/exchange/get-systemmessage).

## Create custom NDRs

### Use the Exchange Management Shell to create custom NDRs for enhanced status codes

To create a custom NDR for an enhanced status code, use this syntax:

```PowerShell
New-SystemMessage -Internal <$true | $false> -Language <Locale> -DSNCode <x.y.z> -Text "<NDR text>"
```

The values are:

- **Internal**: Controls whether the NDR is sent to internal or external senders. For internal senders, use the value `$true`. For external senders, use the value `$false`. For example, in the custom text for internal senders, you can include help desk contact information that you wouldn't want to include in NDRs for external senders.

- **Language**: For the list of available languages, see the [Supported languages for NDRs](#supported-languages-for-ndrs) section in this topic.

- **DSNCode**: The enhanced status code. Valid values are 4. _x_. _y_ or 5. _x_. _y_ where _x_ and _y_ are one to three digit numbers.

- **Text**: You can use plain text or HTML formatting. For more information, see the [HTML tags and special characters in NDRs](ndr-procedures.md#NDRTags) section in this topic.

This example creates a custom plain text NDR for the enhanced status code 5.1.2 that's sent to external senders in English.

```PowerShell
New-SystemMessage -Internal $false -Language En -DSNCode 5.1.2 -Text "You tried to send a message to a disabled mailbox that's no longer accepting messages. Please contact your System Administrator for more information."
```

 This example creates a custom HTML NDR for the enhanced status code 5.1.2 that's sent to internal senders in English.

```PowerShell
New-SystemMessage -DSNCode 5.1.2 -Internal $true -Language En -Text 'You tried to send a message to a <B>disabled</B> mailbox. Please visit <A HREF="https://it.contoso.com">Internal Support</A> or contact &quot;InfoSec&quot; for more information.'
```

For detailed syntax and parameter information, see [New-SystemMessage](https://docs.microsoft.com/powershell/module/exchange/new-systemmessage).

### Use the Exchange Management Shell to create custom NDRs for quotas

To create a custom NDR for quotas, use this syntax:

```PowerShell
New-SystemMessage -Language <Locale> -QuotaMessageType <Quota> -Text "<NDR text>"
```

The values are:

- **Language**: For the list of available languages, see [Supported languages for NDRs](#supported-languages-for-ndrs).

- **QuotaMessageType**: For a list of the available quotas, see [Identity values for NDRs](#identity-values-for-ndrs).

- **Text**: You can use plain text or HTML formatting. For more information, see [HTML tags and special characters in NDRs](ndr-procedures.md#NDRTags).

This example creates a custom English plain text NDR for the **ProhibitSendReceive** quota on mailboxes.

```PowerShell
New-SystemMessage -Language En -QuotaMessageType ProhibitSendReceiveMailBox -Text "Your mailbox is full, and can't send or receive messages. Delete any unwanted large messages (messages with attachments) and empty your Deleted Items folder"
```

For detailed syntax and parameter information, see [New-SystemMessage](https://docs.microsoft.com/powershell/module/exchange/new-systemmessage).

### How do you know this worked?

To verify that you have successfully created a custom NDR, do these steps:

- Run the following command and verify the property values:

  ```PowerShell
  Get-SystemMessage | Format-List Identity,DsnCode,Language,Text
  ```

- Send a test message that will generate the custom NDR that you configured.

## Use the Exchange Management Shell to modify custom NDRs

To modify custom NDRs, use this syntax:

```PowerShell
Set-SystemMessage -Identity <NDRIdentity> [-Text "<NDR text>"] [-Original]
```

For an explanation of the available _\<NDRIdentity\>_ values, see the [Identity values for NDRs](#identity-values-for-ndrs) section in this topic. For an explanation of the _\<NDR text\>_ values, see the [HTML tags and special characters in NDRs](ndr-procedures.md#NDRTags) section in this topic.

This example changes the text in the custom NDR for the enhanced status code 5.1.2 that's sent to internal senders in English.

```PowerShell
Set-SystemMessage -Identity En\Internal\5.1.2 -Text "The mailbox you tried to send an email message to is disabled and is no longer accepting messages. Please contact the Help Desk at extension 123 for assistance."
```

This example changes the text in the custom English NDR for the **ProhibitSendReceive** quota on mailboxes.

```PowerShell
Set-SystemMessage -Identity En\ProhibitSendReceiveMailBox -Text "Your mailbox is full. Delete large messages and empty your Deleted Items folder."
```

This example disables the specified custom NDR. The custom NDR is preserved, and appears in the results of **Get-SystemMessage**, but the default NDR is used instead.

```PowerShell
Set-SystemMessage -Identity En\Internal\5.1.2 -Original
```

 **Note**: If there's no corresponding default NDR, you receive an error when you use the _Original_ switch.

For detailed syntax and parameter information, see [Set-SystemMessage](https://docs.microsoft.com/powershell/module/exchange/set-systemmessage).

### How do you know this worked?

To verify that you have successfully modified a custom NDR, replace _\<NDRIdentity\>_ with the appropriate value, and run this command to verify the property values:

```PowerShell
Get-SystemMessage -Identity <NDRIdentity> | Format-List
```

## Use the Exchange Management Shell to remove custom NDRs

To remove a custom NDR, use this syntax:

```PowerShell
Remove-SystemMessage -Identity <NDRIdentity>
```

For an explanation of the available _\<NDRIdentity\>_ values, see the [Identity values for NDRs](#identity-values-for-ndrs) section in this topic.

This example removes the custom NDR for the enhanced status code 5.1.2 that's sent to internal senders in English.

```PowerShell
Remove-SystemMessage -Identity En\Internal\5.1.2
```

This example removes the custom English NDR for the **ProhibitSendReceive** quota on mailboxes.

```PowerShell
Remove-SystemMessage -Identity En\ProhibitSendReceiveMailBox
```

For detailed syntax and parameter information, see [Remove-SystemMessage](https://docs.microsoft.com/powershell/module/exchange/remove-systemmessage).

### How do you know this worked?

To verify that you have successfully removed a custom NDR, run this command to verify the custom NDR isn't listed:

```PowerShell
Get-SystemMessage
```

## Forward copies of NDRs to the Exchange recipient mailbox

You can configure your Exchange organization to send copies of NDRs to the Exchange recipient. However, by default, no mailbox is assigned to the Exchange recipient, so any messages that are sent to the Exchange recipient are discarded. To send copies of NDRs to the Exchange recipient mailbox, you need to:

1. Assign a mailbox to the Exchange recipient.

2. Specify the enhanced status codes that you want to monitor (not quotas).

### Step 1: Use the Exchange Management Shell to assign a mailbox to the Exchange recipient

 **Note**: Due to the high volume of messages, we recommend using a dedicated mailbox for the Exchange recipient. For more information about creating mailboxes, see [Create shared mailboxes in the Exchange admin center](../../collaboration/shared-mailboxes/create-shared-mailboxes.md) and [Create user mailboxes in Exchange Server](../../recipients/create-user-mailboxes.md).

To assign a mailbox to the Exchange recipient, use this syntax:

```PowerShell
Set-OrganizationConfig -MicrosoftExchangeRecipientReplyRecipient <MailboxIdentity>
```

This example assigns the existing mailbox named "Contoso System Mailbox" to the Exchange recipient.

```PowerShell
Set-OrganizationConfig -MicrosoftExchangeRecipientReplyRecipient "Contoso System Mailbox"
```

### Step 2: Specify the enhanced status codes that you want to monitor

- You can use the EAC or the Exchange Management Shell.

- By default, even though there are no enhanced status codes specified, NDRs for these codes are automatically sent to the Exchange recipient:

  - `5.1.4`

  - `5.2.0`

  - `5.2.4`

  - `5.4.4`

  - `5.4.6`

  - `5.4.8`

- You can only specify enhanced status codes. You can't specify quotas.

#### Use the EAC to specify the enhanced status codes to monitor

For more information about the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md).

1. In the EAC, go to **Mail flow** \> **Receive connectors**.

2. Click **More options** (![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png)) and select **Organization transport settings**.

3. In the **Organization transport settings** window that opens, click the **Delivery** tab. In the **DSN codes** section, do one or more of these steps:

   - To add entries, type the enhanced status code that you want to monitor (4. _\<y.z\>_ or 5. _\<y.z\>_), and then click **Add** (![Add icon](../../media/ITPro_EAC_AddIcon.png)). Repeat this step as many times as you need to.

   - To modify an existing entry, select it click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)), and then modify it inline.

   - To remove an existing entry, select it and then click **Remove** (![Remove icon](../../media/ITPro_EAC_RemoveIcon.png)).

   When you're finished, click **Save**.

#### Use the Exchange Management Shell to specify the enhanced status codes to monitor

To add enhanced status codes to monitor, which replaces any existing values, use this syntax:

```PowerShell
Set-TransportConfig -GenerateCopyOfDSNFor <x.y.z>,<x.y.z>...
```

This example configures the Exchange organization to forward all NDRs for the enhanced status code values 5.7.1, 5.7.2, and 5.7.3 to the Exchange recipient.

```PowerShell
Set-TransportConfig -GenerateCopyOfDSNFor 5.7.1,5.7.2,5.7.3
```

To add or remove entries without modifying any existing values, use this syntax:

```PowerShell
Set-TransportConfig -GenerateCopyOfDSNFor @{Add="<x.y.z>","<x.y.z>"...; Remove="<x.y.z>","<x.y.z>"...}
```

This example adds the enhanced status code 5.7.5 and removes 5.7.1 from the existing list of NDRs that are forwarded to the Exchange recipient.

```PowerShell
Set-TransportConfig -GenerateCopyOfDSNFor @{Add="5.7.5"; Remove="5.7.1"}
```

### How do you know this worked?

To verify that you've successfully configured copies of NDRs to be sent to the Exchange recipient mailbox,

- Run the following command and verify the property values:

  ```PowerShell
  Get-TransportConfig | Format-List GenerateCopyOfDSNFor
  ```

- Monitor the Exchange recipient mailbox to see if NDRs that contain the specified enhanced status codes are delivered there.

## Identity values for NDRs

The identity of an NDR uses one of these formats:

- **NDRs for enhanced status codes**: _\<Language\>_\\<Internal | External\>\ _\<DSNcode\>_. For example, `En\Internal\5.1.2` or `Ja\External\5.1.2`.

  - **\<DSNcode\>**: Valid values are 4. _x_. _y_ or 5. _x_. _y_ where _x_ and _y_ are one to three digit numbers. To generate a list of the enhanced status codes that are used by Exchange, see the [Use the Exchange Management Shell to view all default NDRs](#use-the-exchange-management-shell-to-view-all-default-ndrs) section earlier in this topic.

  - **Internal or External**: You can use different text in NDRs for internal or external senders.

  - **\<Language\>**: For the list of supported languages, see the [Supported languages for NDRs](#supported-languages-for-ndrs) section in this topic.

- **NDRs for quotas**: _\<Language\>_\ _\<QuotaMessageType\>_. For example, `En\ProhibitSendReceiveMailBox`.

  - **\<Language\>**: For the list of supported languages, see the [Supported languages for NDRs](#supported-languages-for-ndrs) section in this topic.

  - **\<QuotaMessageType\>**: Valid values are:

    Mailbox size quotas:

    - **ProhibitSendReceiveMailBox**: A mailbox exceeds its `ProhibitSendReceiveQuota` limit.

    - **ProhibitSendMailbox**: A mailbox exceeds its `ProhibitSendQuota` limit.

    - **WarningMailbox**: A mailbox exceeds its `IssueWarningQuota` limit when it has a `ProhibitSendQuota` or `ProhibitSendReceiveQuota` limit configured.

    - **WarningMailboxUnlimitedSize**: A mailbox exceeds its `IssueWarningQuota` limit when it doesn't have a `ProhibitSendQuota` or `ProhibitSendReceiveQuota` limit configured.

    Public folder size quotas:

    - **ProhibitPostPublicFolder**: A public folder exceeds its `ProhibitPostQuota` limit.

    - **WarningPublicFolder**: A public folder exceeds its `IssueWarningQuota` limit when it has a `ProhibitPostQuota` limit configured.

    - **WarningPublicFolderUnlimitedSize**: A public folder exceeds its `IssueWarningQuota` limit when it doesn't have a `ProhibitPostQuota` limit configured.

    Maximum number of messages in a mailbox folder:

    - **ProhibitReceiveMailboxMessagesPerFolderCount**: A mailbox exceeds its `MailboxMessagesPerFolderCountReceiveQuota` limit.

    - **WarningMailboxMessagesPerFolderCount**: A mailbox exceeds its `MailboxMessagesPerFolderCountWarningQuota` limit when it has a `ailboxMessagesPerFolderCountReceiveQuota` limit configured.

    - **WarningMailboxMessagesPerFolderUnlimitedCount**: A mailbox exceeds its `MailboxMessagesPerFolderCountWarningQuota` limit when it doesn't have a `MailboxMessagesPerFolderCountReceiveQuota` limit configured.

    Maximum number of subfolders in a mailbox folder:

    - **ProhibitReceiveFolderHierarchyChildrenCountCount**: A mailbox exceeds its `FolderHierarchyChildrenCountReceiveQuota` limit.

    - **WarningFolderHierarchyChildrenCount**: A mailbox exceeds its `FolderHierarchyChildrenCountWarningQuota` limit when it has a `FolderHierarchyChildrenCountReceiveQuota` limit configured.

    - **WarningFolderHierarchyChildrenUnlimitedCount**: A mailbox exceeds its `FolderHierarchyChildrenCountWarningQuota` limit when it doesn't have a `FolderHierarchyChildrenCountReceiveQuota` limit configured.

    - **ProhibitReceiveFoldersCount**: A mailbox exceeds its `FoldersCountReceiveQuota` limit.

    - **WarningFoldersCount**: A mailbox exceeds its `FoldersCountWarningQuota` limit when it has a `FoldersCountReceiveQuota` limit configured.

    - **WarningFoldersCountUnlimited** A mailbox exceeds its `FoldersCountWarningQuota` limit when it doesn't have a `FoldersCountReceiveQuota` limit configured.

    Maximum number of levels (depth) in a mailbox folder:

    - **ProhibitReceiveFolderHierarchyDepth**: A mailbox exceeds its `FolderHierarchyDepthWarningQuota` limit.

    - **WarningFolderHierarchyDepth**: A mailbox exceeds its `FolderHierarchyDepthWarningQuota` limit when it has a `FolderHierarchyDepthReceiveQuota` limit configured.

    - **WarningFolderHierarchyDepthUnlimited:**: A mailbox exceeds its `FolderHierarchyDepthWarningQuota` limit when it doesn't have a `FolderHierarchyDepthReceiveQuota` limit configured.

## Supported languages for NDRs

This table lists the supported language that codes you can use in custom NDRs.

|**Language code**|**Language**|
|:-----|:-----|
|af|Afrikaans|
|am-ET|Amharic (Ethiopia)|
|ar|Arabic|
|as-IN|Assamese (India)|
|bg|Bulgarian|
|bn-BD|Bengali (Bangladesh)|
|bn-IN|Bengali (India)|
|bs-Cyrl-BA|Bosnian (Cyrillic, Bosnia and Herzegovina)|
|bs-Latn-BA|Bosnian (Latin, Bosnia and Herzegovina)|
|ca|Catalan|
|cs|Czech|
|cy-GB|Welsh (Great Britain)|
|da|Danish|
|de|German|
|el|Greek|
|en|English|
|es|Spanish|
|et|Estonian|
|eu|Basque|
|fa|Persian|
|fi|Finnish|
|fil-PH|Filipino (Philippines)|
|fr|French|
|ga-IE|Irish (Ireland)|
|gl|Galician|
|gu|Gujarati|
|ha-Latn-NG|Hausa (Latin, Nigeria)|
|he|Hebrew|
|hi|Hindi|
|hr|Croatian|
|hu|Hungarian|
|hy|Armenian|
|id|Indonesian|
|ig-NG|Igbo (Nigeria)|
|is|Icelandic|
|it|Italian|
|iu-Latn-CA|Inuktitut (Latin, Canada)|
|ja|Japanese|
|ka|Georgian|
|kk|Kazakh|
|km-KH|Khmer (Cambodia)|
|kn|Kannada|
|ko|Korean|
|kok|Konkani|
|ky|Kyrgyz|
|lb-LU|Luxembourgish (Luxembourg)|
|lo-LA|Lao (Lao People's Democratic Republic)|
|lt|Lithuanian|
|lv|Latvian|
|mi-NZ|Maori (New Zealand)|
|mk|Macedonian|
|ml-IN|Malayalam (India)|
|mr|Marathi|
|ms|Malay|
|ms-BN|Malay (Brunei Darussalam)|
|mt-MT|Maltese (Malta)|
|ne-NP|Nepali (Nepal)|
|nl|Dutch|
|nn-NO|Norwegian (Nynorsk)|
|no|Norwegian|
|nso-ZA|Sesotho sa Leboa (South Africa)|
|or-IN|Oriya (India)|
|pa|Punjabi|
|pl|Polish|
|ps-AF|Pashto (Afghanistan)|
|pt|Portuguese|
|pt-PT|Portuguese (Portugal)|
|qut-GT|K'iche (Guatemala)|
|quz-PE|Quechua (Peru)|
|ro|Romanian|
|ru|Russian|
|rw-RW|Kinyarwanda (Rwanda)|
|si-LK|Sinhala (Sri Lanka)|
|sk|Slovak|
|sl|Slovenian|
|sq|Albanian|
|sr|Serbian|
|sr-Cyrl-CS|Serbian (Cyrillic, Serbia)|
|sv|Swedish|
|sw|Kiswahili|
|ta|Tamil|
|te|Telugu|
|th|Thai|
|tn-ZA|Setswana (South Africa)|
|tr|Turkish|
|tt|Tatar|
|uk|Ukrainian|
|ur|Urdu|
|uz|Uzbek|
|vi|Vietnamese|
|wo-SN|Wolof (Senegal)|
|xh-ZA|isiXhosa (South Africa)|
|yo-NG|Yoruba (Nigeria)|
|zh-Hans|Chinese (Simplified)|
|zh-Hant|Chinese (Traditional)|
|zh-HK|Chinese (Hong Kong)|
|zu-ZA|isiZulu (South Africa)|

To control the languages that are used in NDRs, you use these parameters on the **Set-TransportConfig** cmdlet:

- _ExternalDsnDefaultLanguage_: Specifies the default language to use on external NDRs. The default value is blank (`$null`), which means the default Windows server language is used.

- _InternalDsnDefaultLanguage_: Specifies the default language to use on internal NDRs. The default value is blank (`$null`), which means the default Windows server language is used.

- _ExternalDsnLanguageDetectionEnabled_

  - `$true`: Exchange tries to send an external NDR in the same language as the original message. This is the default value.

  - `$false`: Language detection is disabled for external NDRs, The NDR language is determined by the _ExternalDsnDefaultLanguage_ parameter.

- _InternalDsnLanguageDetectionEnabled_

  - `$true`: Exchange tries to send an internal NDR in the same language as the original message. This is the default value.

  - `$false`: Language detection is disabled for internal NDRs, The NDR language is determined by the _InternalDsnDefaultLanguage_ parameter.

## HTML tags and special characters in NDRs
<a name="NDRTags"> </a>

The custom text that you include in an NDR can contain a maximum of 512 characters, which includes text and HTML tags. For example, you can include a detailed description of the problem, contact information for your help desk, and a link to your support department's web site.

To control whether Exchange uses HTML or plain text in NDRs, you use these parameters on the **Set-TransportConfig** cmdlet:

- _ExternalDsnSendHtml_

  - `$true`: Use HTML tags in NDRs for external senders. This is the default value.

  - `$false`: Use plain text in NDRs for external senders.

- _InternalDsnSendHtml_

  - `$true`: Use HTML tags in NDRs for internal senders. This is the default value

  - `$false`: Use plain text in NDRs for internal senders.

This table describes the HTML tags that you can use in the NDR text.

|**Description**|**HTML tags**|
|:-----|:-----|
|Bold|`<B>` and `</B>`|
|Italic|`<EM>` and `</EM>`|
|Line break|`<BR>`|
|Paragraph|`<P>` and `</P>`|
|Hyperlink|`<A HREF="url">` and `</A>` <br/><br/> **Note**: Because this tag contains double quotation marks, you need to use single quotation marks (not double quotation marks) around the complete text string if you use this tag in your custom text. Otherwise, you'll receive an error.|

Certain characters in an NDR require escape codes to identify them literally, and not by their function in the NDR. These characters are described in the following table:

|**Character**|**Escape code**|
|:-----|:-----|
|\<|`&lt;`|
|\>|`&gt;`|
|"|`&quot;`|
|&|`&amp;`|

For example, if you want the NDR to display the text `Please contact the Help Desk at <1234>.`, you need to the value `"Please contact the Help Desk at &lt;1234&gt;."`

This is an example of a custom NDR text value that uses HTML tags and escape codes.

```
'You tried to send a message to a <B>disabled</B> mailbox. Please visit <A HREF="https://it.contoso.com">Internal Support</A> or contact &quot;InfoSec&quot; for more information.'
```
