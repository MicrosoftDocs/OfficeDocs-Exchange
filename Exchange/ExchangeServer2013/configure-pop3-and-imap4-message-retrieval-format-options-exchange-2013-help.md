---
title: 'Configure POP3 and IMAP4 message retrieval format options: Exchange 2013 Help'
TOCTitle: Configure POP3 and IMAP4 message retrieval format options
ms:assetid: 481096e0-4492-46c2-8ca8-bdf84a84531e
ms:mtpsurl: https://technet.microsoft.com/library/Aa997869(v=EXCHG.150)
ms:contentKeyID: 50395398
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure POP3 and IMAP4 message retrieval format options

_**Applies to:** Exchange Server 2013_

You can configure the message retrieval format for users who connect to their email using POP3 and IMAP4. Message retrieval options can be configured at the server level using the EAC or the Shell, and can be configured at the user level using the Shell.

For additional information related to POP3 and IMAP4, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 settings" and "IMAP4 settings" entries in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Set the POP3 message retrieval format at the server level

## Use the EAC to set the POP3 message retrieval format at the server level

1. In the EAC, navigate to **Servers** \> **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **POP3**.

4. Under **Message MIME format**, choose from the following settings:

    - Text

    - HTML

    - HTML and alternative text

    - Enriched text

    - Enriched text and alternative text

    - Best body format

    - TNEF

5. Click **Save**.

After you've set the message retrieval format settings for POP3, you must restart the POP3 services for the settings to take effect. For information about how to restart the POP3 services, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

## Use the Shell to set the POP3 message retrieval format at the server level

This example sets the message retrieval format option to text only for all POP3 users on server CAS01.

```powershell
Set-PopSettings -Server CAS01 -MessageRetrievalMimeFormat TextOnly
```

You can choose from the following settings. You can specify the value for the *MessageRetrievalMimeFormat* parameter by using a numerical value or a text string.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Message format</strong></p></td>
<td><p><strong>Value</strong></p></td>
</tr>
<tr class="even">
<td><p>Text</p></td>
<td><p><code>0</code> or <code>TextOnly</code></p></td>
</tr>
<tr class="odd">
<td><p>HTML</p></td>
<td><p><code>1</code> or <code>HtmlOnly</code></p></td>
</tr>
<tr class="even">
<td><p>HTML and alternative text</p></td>
<td><p><code>2</code> or <code>HtmlAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Enriched text</p></td>
<td><p><code>3</code> or <code>TextEnriched</code></p></td>
</tr>
<tr class="even">
<td><p>Enriched text and alternative text</p></td>
<td><p><code>4</code> or <code>TextEnrichedAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Best body format</p></td>
<td><p><code>5</code> or <code>BestBodyFormat</code></p></td>
</tr>
<tr class="even">
<td><p>TNEF</p></td>
<td><p><code>6</code> or <code>Tnef</code></p></td>
</tr>
</tbody>
</table>

After you've set the message retrieval format settings for POP3, you must restart the POP3 services for the settings to take effect. For information about how to restart the POP3 services, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-PopSettings](https://technet.microsoft.com/library/aa997154\(v=exchg.150\)).

## How do you know this worked?

Do the following to verify that you've successfully set POP3 message retrieval settings on a server.

1. Run the following command in the Shell.

    ```powershell
    Get-PopSettings | format-list
    ```

2. Verify the *MessageRetrievalMimeFormat* setting is correct.

## Set the IMAP4 message retrieval format at the server level

## Use the EAC to set the IMAP4 message retrieval format at the server level

1. In the EAC, navigate to **Servers** \> **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **IMAP4**.

4. Under **Message MIME format**, choose from the following settings:

    - Text

    - HTML

    - HTML and alternative text

    - Enriched text

    - Enriched text and alternative text

    - Best body format

    - TNEF

5. Click **Save**.

After you've set the message retrieval format settings for IMAP4, you must restart the IMAP4 services for the settings to take effect. For information about how to restart the IMAP4 services, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

## Use the Shell to set the IMAP4 message retrieval format at the server level

This example sets the message retrieval format option to text only for all IMAP4 users on server CAS01.

```powershell
Set-ImapSettings -Server CAS01 -MessageRetrievalMimeFormat TextOnly
```

You can choose from the following settings. You can specify the value for the *MessageRetrievalMimeFormat* parameter by using a numerical value or a text string.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Message format</strong></p></td>
<td><p><strong>Value</strong></p></td>
</tr>
<tr class="even">
<td><p>Text</p></td>
<td><p><code>0</code> or <code>TextOnly</code></p></td>
</tr>
<tr class="odd">
<td><p>HTML</p></td>
<td><p><code>1</code> or <code>HtmlOnly</code></p></td>
</tr>
<tr class="even">
<td><p>HTML and alternative text</p></td>
<td><p><code>2</code> or <code>HtmlAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Enriched text</p></td>
<td><p><code>3</code> or <code>TextEnriched</code></p></td>
</tr>
<tr class="even">
<td><p>Enriched text and alternative text</p></td>
<td><p><code>4</code> or <code>TextEnrichedAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Best body format</p></td>
<td><p><code>5</code> or <code>BestBodyFormat</code></p></td>
</tr>
<tr class="even">
<td><p>TNEF</p></td>
<td><p><code>6</code> or <code>Tnef</code></p></td>
</tr>
</tbody>
</table>

After you've set the message retrieval format settings for IMAP4, you must restart the IMAP4 services for the settings to take effect. For information about how to restart the IMAP4 services, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-ImapSettings](https://technet.microsoft.com/library/aa998252\(v=exchg.150\)).

## How do you know this worked?

Do the following to verify that you've successfully set IMAP4 message retrieval settings on a server.

1. Run the following command in the Shell.

    ```powershell
    Get-ImapSettings | format-list
    ```

2. Verify the *MessageRetrievalMimeFormat* setting is correct.

## Set the POP3 message retrieval format for a user

## Use the Shell to set the POP3 message retrieval format for a user

This example sets the message retrieval format to text only for POP3 access for `USER01`.

```powershell
Set-CASMailbox -Identity USER01 -PopMessagesRetrievalMimeFormat TextOnly
```

You can choose from the following settings. You can specify the value for the *PopMessagesRetrievalMimeFormat* parameter by using a numerical value or a text string.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Message format</strong></p></td>
<td><p><strong>Value</strong></p></td>
</tr>
<tr class="even">
<td><p>Text</p></td>
<td><p><code>0</code> or <code>TextOnly</code></p></td>
</tr>
<tr class="odd">
<td><p>HTML</p></td>
<td><p><code>1</code> or <code>HtmlOnly</code></p></td>
</tr>
<tr class="even">
<td><p>HTML and alternative text</p></td>
<td><p><code>2</code> or <code>HtmlAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Enriched text</p></td>
<td><p><code>3</code> or <code>TextEnriched</code></p></td>
</tr>
<tr class="even">
<td><p>Enriched text and alternative text</p></td>
<td><p><code>4</code> or <code>TextEnrichedAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Best body format</p></td>
<td><p><code>5</code> or <code>BestBodyFormat</code></p></td>
</tr>
<tr class="even">
<td><p>TNEF</p></td>
<td><p><code>6</code> or <code>Tnef</code></p></td>
</tr>
</tbody>
</table>

After you've set the message retrieval format settings for POP3, you must restart the POP3 services for the settings to take effect. For information about how to restart the POP3 services, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-CASMailbox](https://technet.microsoft.com/library/bb125264\(v=exchg.150\)).

## How do you know this worked?

Do the following to verify that you've successfully set POP3 message retrieval format options for a user.

1. Run the following command in the Shell.

    ```powershell
    Get-CASMailbox <identity> | format-list
    ```

2. Verify the value for *PopMessagesRetrievalMimeFormat* is correct.

## Set the IMAP4 message retrieval format for a user

## Use the Shell to set the IMAP4 message retrieval format for a user

This example sets the message retrieval format to text only for IMAP4 access for `USER01`.

```powershell
Set-CASMailbox -Identity USER01 -ImapMessagesRetrievalMimeFormat TextOnly
```

You can specify the value for the *ImapMessagesRetrievalMimeFormat* parameter by using a numerical value or a text string.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Message format</strong></p></td>
<td><p><strong>Value</strong></p></td>
</tr>
<tr class="even">
<td><p>Text</p></td>
<td><p><code>0</code> or <code>TextOnly</code></p></td>
</tr>
<tr class="odd">
<td><p>HTML</p></td>
<td><p><code>1</code> or <code>HtmlOnly</code></p></td>
</tr>
<tr class="even">
<td><p>HTML and alternative text</p></td>
<td><p><code>2</code> or <code>HtmlAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Enriched text</p></td>
<td><p><code>3</code> or <code>TextEnriched</code></p></td>
</tr>
<tr class="even">
<td><p>Enriched text and alternative text</p></td>
<td><p><code>4</code> or <code>TextEnrichedAndTextAlternative</code></p></td>
</tr>
<tr class="odd">
<td><p>Best body format</p></td>
<td><p><code>5</code> or <code>BestBodyFormat</code></p></td>
</tr>
<tr class="even">
<td><p>TNEF</p></td>
<td><p><code>6</code> or <code>Tnef</code></p></td>
</tr>
</tbody>
</table>

After you've set the message retrieval format settings for IMAP4, you must restart the IMAP4 services for the settings to take effect. For information about how to restart the IMAP4 services, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-CASMailbox](https://technet.microsoft.com/library/bb125264\(v=exchg.150\)).

## How do you know this worked?

Do the following to verify that you've successfully set IMAP4 message retrieval format options for a user.

1. Run the following command in the Shell.

    ```powershell
    Get-CASMailbox <identity> | format-list
    ```

2. Verify the value for *ImapMessagesRetrievalMimeFormat* is correct.

## For more information

After you set the message retrieval format for IMAP4 and POP3 users, you may also want to:

[Enable or disable POP3 access for a user](enable-or-disable-pop3-access-for-a-user-exchange-2013-help.md)

[Enable or disable IMAP4 access for a user](enable-or-disable-imap4-access-for-a-user-exchange-2013-help.md)

[Configure calendar options for IMAP4](configure-calendar-options-for-imap4-exchange-2013-help.md)

[Configure calendar options for POP3](configure-calendar-options-for-pop3-exchange-2013-help.md)
