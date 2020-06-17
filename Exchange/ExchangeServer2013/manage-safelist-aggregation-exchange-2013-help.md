---
title: 'Manage safelist aggregation: Exchange 2013 Help'
TOCTitle: Manage safelist aggregation
ms:assetid: 5ac17168-f411-4cb7-ae98-ebefb865b210
ms:mtpsurl: https://technet.microsoft.com/library/Aa998280(v=EXCHG.150)
ms:contentKeyID: 49248682
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage safelist aggregation

_**Applies to:** Exchange Server 2013_

*Safelist aggregation* refers to anti-spam functionality that's shared from Microsoft Outlook to Microsoft Exchange Server 2013. This functionality collects data from the Safe Recipients Lists, Safe Senders Lists, Blocked Senders Lists, and contact data that Outlook users configure, and makes this data available to the Exchange anti-spam agents. Safelist aggregation can help reduce the instances of false-positives in anti-spam filtering performed by the Exchange servers where the anti-spam agents are running.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic, and the "Anti-spam features" section in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- By default, anti-spam features aren't enabled in the Transport service on a Mailbox server. Typically, you only enable the anti-spam features on a Mailbox server if your Exchange organization doesn't do any prior anti-spam filtering before accepting incoming messages. For more information, see [Enable anti-spam functionality on Mailbox servers](enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md).

- Be mindful of the network and replication traffic that may be generated when you run the **Update-SafeList** cmdlet. Running the command on multiple mailboxes where safelists are heavily used may generate a significant amount of traffic. We recommend that if you run the command on multiple mailboxes, you should run the command during off-peak, non-business hours.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure the mailbox safelist collection limits

You can configure the maximum number of safe senders and blocked senders a user can configure. By default, users can configure up to 5,000 safe senders and 500 blocked senders.

To configure the maximum number of safe senders and blocked senders, run the following command:

```powershell
Set-Mailbox <MailboxIdentity> -MaxSafeSenders <Integer> -MaxBlockedSenders <Integer>
```

This example configures the mailbox john@contoso.com to have a maximum of 2,000 safe senders and 200 blocked senders.

```powershell
Set-Mailbox john@contoso.com -MaxSafeSenders 2000 -MaxBlockedSenders 200
```

## How do you know this worked?

To verify that you have successfully configured the mailbox safelist collection limits, do the following:

1. Run the following command:

    ```powershell
    Get-Mailbox <Identity> | Format-List Name,Max*Senders
    ```

2. Verify the values displayed match the values you configured.

## Use the Shell to run the Update-Safelist command

In Exchange 2013, safelist aggregation is done automatically, so you don't need to schedule or manually run the **Update-Safelist** cmdlet. However, you may want to occasionally run this cmdlet to test safelist aggregation.

This example writes the safe senders list for the mailbox john@contoso.com to Active Directory.

```powershell
Update-Safelist john@contoso.com -Type SafeSenders
```

For detailed syntax and parameter information, see [Update-SafeList](https://docs.microsoft.com/powershell/module/exchange/Update-SafeList).

## How do you know this worked?

To verify that you have successfully configured safelist aggregation, perform the following steps:

## Step 1: Use the Shell to verify the Content Filter agent is enabled on the Exchange server

1. Run the following command:

    ```powershell
    Get-ContentFilterConfig | Format-List Enabled
    ```

2. If the output shows the *Enabled* parameter to be `True`, content filtering is enabled. If it isn't, run the following command to enable content filtering and the Content Filter agent on the Exchange server:

    ```powershell
    Set-ContentFilterConfig -Enabled $true
    ```

## Step 2: (Optional) Use ADSI Edit to verify replication of the safelist aggregation data to Edge Transport servers

This step is only required if you run the Content Filter agent on an Edge Transport server in your perimeter network.

You can view the user objects in the Active Lightweight Directory Services (AD LDS) instance on the Edge Transport server to verify that the safelist collection data is updated for the user objects and that the Microsoft Exchange EdgeSync service has replicated the data to the AD LDS instance.

There are three safelist collection attributes for each user object:

- **msExchSafeRecipientsHash**: This attribute stores the hash of the Safe Recipients List collection for the user.

- **msExchSafeSendersHash**: This attribute stores the hash of the Safe Senders List collection for the user.

- **msExchBlockedSendersHash**: This attribute stores the hash of the Blocked Senders List collection for the user.

If a hexadecimal string, such as `0xac 0xbd 0x03 0xca`, is present on the attribute, the user object was updated. If the attribute has a value of `<Not Set>`, the attribute wasn't updated.

You can search for and view the attributes by using the AD LDS Active Directory Service Interfaces (ADSI) Edit snap-in.

## Step 3: Send a test message to verify safelist aggregation is working

To test whether safelist aggregation is functioning, you need to send yourself a message from a safe sender that would otherwise be blocked by content filtering. If safelist aggregation is functioning, the message should arrive in your Inbox.

1. Find an existing external email account to use, or create an email account at a free web-based email provider like Microsoft Hotmail.

2. Add that account to your Safe Senders List in Microsoft Outlook.

3. Use the **Update-SafeList** cmdlet to have the safelist collection from that mailbox copied to Active Directory.

4. Optional: if you are running the Content Filter agent on an Edge Transport server in the perimeter network, run the **Start-EdgeSynchronization** cmdlet to force EdgeSync replication.

5. Add a specific word as a blocked phrase to your content filtering configuration. For detailed steps, see [Manage content filtering](manage-content-filtering-exchange-2013-help.md).

6. From the external email account in step 1, send a message to your Exchange mailbox that includes the blocked phrase you configured in step 5.

    If the message is successfully delivered to your Inbox, safelist aggregation is working correctly.
