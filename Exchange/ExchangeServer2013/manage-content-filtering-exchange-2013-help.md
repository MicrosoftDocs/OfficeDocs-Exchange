---
title: 'Manage content filtering: Exchange 2013 Help'
TOCTitle: Manage content filtering
ms:assetid: 05bd9d39-81dc-4514-8b75-7be386d5bcad
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Aa995953(v=EXCHG.150)
ms:contentKeyID: 49248674
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Manage content filtering

_**Applies to:** Exchange Server 2013_

Content filtering is provided by the Content Filter agent. The Content Filter agent filters all messages that come through all Receive connectors on the Exchange server. Only messages that come from non-authenticated sources are filtered.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Anti-spam feature" entry in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- By default, anti-spam features aren't enabled in the Transport service on a Mailbox server. Typically, you only enable the anti-spam features on a Mailbox server if your Exchange organization doesn't do any prior anti-spam filtering before accepting incoming messages. For more information, see [Enable anti-spam functionality on Mailbox servers](enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable content filtering

To disable content filtering, run the following command:

```powershell
Set-ContentFilterConfig -Enabled $false
```

To enable content filtering, run the following command:

```powershell
Set-ContentFilterConfig -Enabled $true
```

> [!NOTE]
> When you disable content filtering, the underlying Content Filter agent is still enabled. To disable the Content Filter agent, run the command: <CODE>Disable-TransportAgent "Content Filter Agent"</CODE>.

## How do you know this worked?

To verify that you have successfully enabled or disabled content filtering, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List Enabled
    ```

2. Verify the value of the *Enabled* property that's displayed.

## Use the Shell to enable or disable content filtering for external messages

By default, content filtering functionality is enabled for external messages.

To disable content filtering for external messages, run the following command:

```powershell
Set-ContentFilterConfig -ExternalMailEnabled $false
```

To enable content filtering for external messages, run the following command:

```powershell
Set-ContentFilterConfig -ExternalMailEnabled $true
```

## How do you know this worked?

To verify that you have successfully enabled or disabled content filtering for external messages, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List ExternalMailEnabled
    ```

2. Verify the value of the *ExternalMailEnabled* property that's displayed.

## Use the Shell to enable or disable content filtering for internal messages

As a best practice, you should not filter messages from trusted partners or from inside your organization. When you run anti-spam filters, there's always a chance that the filters will detect false positives. To reduce the chance that filters will mishandle legitimate email messages, you should enable anti-spam agents to run only on messages from potentially untrusted and unknown sources.

To enable content filtering for internal messages, run the following command:

```powershell
Set-ContentFilterConfig -InternalMailEnabled $true
```

To disable content filtering for internal messages, run the following command:

```powershell
Set-ContentFilterConfig -InternalMailEnabled $false
```

## How do you know this worked?

To verify that you have successfully enabled or disabled content filtering for internal messages, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List InternalMailEnabled
    ```

2. Verify the value of the *InternalMailEnabled* property that's displayed.

## Use the Shell to configure recipient and sender exceptions

To replace the existing values, run the following command:

```powershell
Set-ContentFilterConfig -BypassedRecipients <recipient1,recipient2...> -BypassedSenders <sender1,sender2...> -BypassedSenderDomains <domain1,domain2...>
```

This example configures the following exceptions in content filtering:

- The recipients laura@contoso.com and julia@contoso.com aren't checked by content filtering.

- The senders steve@fabrikam.com and cindy@fabrikam.com aren't checked by content filtering.

- All senders in the domain nwtraders.com and all subdomains aren't checked by content filtering.

```powershell
Set-ContentFilterConfig -BypassedRecipients laura@contoso.com,julia@contoso.com -BypassedSenders steve@fabrikam.com,cindy@fabrikam.com -BypassedSenderDomains *.nwtraders.com
```

To add or remove entries without modifying any existing values, run the following command:

```powershell
Set-ContentFilterConfig -BypassedRecipients @{Add="<recipient1>","<recipient2>"...; Remove="<recipient1>","<recipient2>"...} -BypassedSenders @{Add="<sender1>","<sender2>"...; Remove="<sender1>","<sender2>"...} -BypassedSenderDomains @{Add="<domain1>","<domain2>"...; Remove="<domain1>","<domain2>"...}
```

This example configures the following exceptions in content filtering:

- Add tiffany@contoso.com and chris@contoso.com to the list of existing recipients who aren't checked by content filtering.

- Add joe@fabrikam.com and michelle@fabrikam.com to the list of existing senders who aren't checked by content filtering.

- Add blueyonderairlines.com to the list of existing domains whose senders aren't checked by content filtering.

- Remove the domain woodgrovebank.com and all subdomains from the list of existing domains whose senders aren't checked by content filtering.

```powershell
Set-ContentFilterConfig -BypassedRecipients @{Add="tiffany@contoso.com","chris@contoso.com"} -BypassedSenders @{Add="joe@fabrikam.com","michelle@fabrikam.com"} -BypassedSenderDomains @{Add="blueyonderairlines.com"; Remove="*.woodgrovebank.com"}
```

## How do you know this worked?

To verify that you have successfully configured the recipient and sender exceptions, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List Bypassed*
    ```

2. Verify the values displayed match the settings you specified.

## Use the Shell to configure allowed and blocked phrases

To add allowed and blocked words and phrases, run the following command:

```powershell
Add-ContentFilterPhrase -Influence GoodWord -Phrase <Phrase> -Influence BadWord -Phrase <Phrase>
```

This example allows all messages that contain the phrase "customer feedback".

```powershell
Add-ContentFilterPhrase -Influence GoodWord -Phrase "customer feedback"
```

This example blocks all messages that contain the phrase "stock tip".

```powershell
Add-ContentFilterPhrase -Influence BadWord -Phrase "stock tip"
```

To remove allowed or blocked phrases, run the following command:

```powershell
Remove-ContentFilterPhrase -Phrase <Phrase>
```

This example removes the phrase "stock tip":

```powershell
Remove-ContentFilterPhrase -Phrase "stock tip"
```

## How do you know this worked?

To verify that you have successfully configured the allowed and block phrases, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterPhrase | Format-List Influence,Phrase
    ```

2. Verify the values displayed match the settings you specified.

## Use the Shell to configure SCL thresholds

To configure the spam confidence level (SCL) thresholds and actions, run the following command:

```powershell
Set-ContentFilterConfig -SCLDeleteEnabled <$true | $false> -SCLDeleteThreshold <Value> -SCLRejectEnabled <$true | $false> -SCLRejectThreshold <Value> -SCLQuarantineEnabled <$true | $false> -SCLQuarantineThreshold <Value>
```

> [!NOTE]
> The Delete action takes precedence over the Reject action, and the Reject action takes precedence over the Quarantine action. Therefore, the SCL threshold for the Delete action should be greater than the SCL threshold for the Reject action, which in turn should be greater than the SCL threshold for the Quarantine action. Only the Reject action is enabled by default, and it has the SCL threshold value 7.

This example configures the following values for the SCL thresholds:

- The Delete action is enabled and the corresponding SCL threshold is set to 9.

- The Reject action is enabled and the corresponding SCL threshold is set to 8.

- The Quarantine action is enabled and the corresponding SCL threshold is set to 7.

```powershell
Set-ContentFilterConfig -SCLDeleteEnabled $true -SCLDeleteThreshold 9 -SCLRejectEnabled $true -SCLRejectThreshold 8 -SCLQuarantineEnabled $true -SCLQuarantineThreshold 7
```

## How do you know this worked?

To verify that you have successfully configured the SCL thresholds, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List SCL*
    ```

2. Verify the values displayed match the settings you specified.

## Use the Shell to configure the rejection response

When the Reject action is enabled, you can customize the rejection response that's sent to the message sender. The rejection response can't exceed 240 characters.

To configure a custom rejection response, run the following command:

```powershell
Set-ContentFilterConfig -RejectionResponse "<Custom Text>"
```

This example configures the Content Filter agent to send a customized rejection response.

```powershell
Set-ContentFilterConfig -RejectionResponse "Your message was rejected because it appears to be SPAM."
```

## How do you know this worked?

To verify that you have successfully configured the rejection response, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List *Reject*
    ```

2. Verify the values displayed match the settings you specified.

## Use the Shell to enable or disable Outlook Email Postmarking

*Outlook Email Postmarking* validation is a computational proof that Microsoft Outlook applies to outgoing messages to help recipient messaging systems distinguish legitimate email from junk email. Postmarking is available in Outlook 2007 or newer. Postmarking helps reduce false positives. Outlook Email Postmarking is enabled by default.

To disable Outlook Email Postmarking, run the following command:

```powershell
Set-ContentFilterConfig -OutlookEmailPostmarkValidationEnabled $false
```

To enable Outlook Email Postmarking, run the following command:

```powershell
Set-ContentFilterConfig -OutlookEmailPostmarkValidationEnabled $true
```

## How do you know this worked?

To verify that you have successfully configured Outlook Email Postmarking, do the following:

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List OutlookEmailPostmarkValidationEnabled
    ```

2. Verify the value displayed matches the setting you specified.
