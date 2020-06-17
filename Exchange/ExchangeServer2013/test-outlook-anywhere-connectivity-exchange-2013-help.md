---
title: 'Test Outlook Anywhere connectivity: Exchange 2013 Help'
TOCTitle: Test Outlook Anywhere connectivity
ms:assetid: 0dc5b68f-2316-446a-84c9-5f1c50dc3776
ms:mtpsurl: https://technet.microsoft.com/library/Ee633453(v=EXCHG.150)
ms:contentKeyID: 50396598
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Test Outlook Anywhere connectivity

_**Applies to:** Exchange Server 2013_

You can test for end-to-end client Outlook Anywhere connectivity by using either the Shell or the Exchange Remote Connectivity Analyzer (ExRCA). This includes testing for connectivity through the Autodiscover service, creating a user profile, and signing in to the user's mailbox. All the required values are retrieved from the Autodiscover service.

For additional management tasks related to Outlook Anywhere, see [Outlook Anywhere](outlook-anywhere-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook Anywhere" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to test Outlook Anywhere connectivity

To use the Shell to test Outlook Anywhere connectivity, use the **Test-OutlookConnectivity** cmdlet.

Run the following command.

```powershell
Test-OutlookConnectivity -ProbeIdentity 'OutlookMailboxDeepTestProbe' -MailboxId tony@contoso.com -Hostname contoso.com
```

> [!NOTE]
> The <EM>OutlookMailboxDeepTestProbe</EM> parameter value tests connectivity from the Mailbox server. To test connectivity from the Client Access server, use <EM>OutlookMailboxCTPProbe</EM> for the <EM>ProbeIdentity</EM> parameter value.

## Use the Exchange Remote Connectivity Analyzer to test Outlook Anywhere connectivity

The Exchange Remote Connectivity Analyzer (ExRCA) is a web-based tool designed to test connectivity with a variety of Exchange protocols. You can access the ExRCA [here](https://testconnectivity.microsoft.com/tests/o365).

1. On the ExRCA website, under **Microsoft Office Outlook Connectivity Tests**, select **Outlook Anywhere**, and then select Next at the bottom of the page.

2. Enter the required information on the next screen, including email address, domain and user name, and password.

3. Choose whether to use Autodiscover to detect server settings or to manually specify server settings.

4. Accept the disclaimer, enter the verification code, and then select **Verify**.

5. Select **Perform Test**.

## How do you know this worked?

When the ExRCA tests complete, the output will be displayed on the web page. Any failures will be listed.
