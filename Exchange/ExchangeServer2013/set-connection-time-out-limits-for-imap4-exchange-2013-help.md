---
title: 'Set connection time-out limits for IMAP4: Exchange 2013 Help'
TOCTitle: Set connection time-out limits for IMAP4
ms:assetid: 6b6a5bd1-a878-4a70-8e21-14d5042a58f1
ms:mtpsurl: https://technet.microsoft.com/library/Aa998665(v=EXCHG.150)
ms:contentKeyID: 50395405
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set connection time-out limits for IMAP4

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to configure the connection time-out limits for idle authenticated and unauthenticated IMAP4 connections.

For additional information related to IMAP4, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "IMAP4 settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to set connection time-out limits for IMAP4

1. In the EAC, navigate to **Servers** **\>** **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **IMAP4**.

4. Scroll down and click **More options**.

5. Under **Time-out settings**, use the following settings:

   - **Authenticated time-out (seconds)**:  Specifies the time to wait before closing an idle authenticated connection. The default value is 1,800. The possible values are from 30 through 86,400.

   - **Unauthenticated time-out (seconds)**: Specifies the time to wait before closing an idle connection that isn't authenticated. The default value is 60. The possible values are from 30 through 3,600.

6. Click **Apply**, and then click **OK** to save your changes.

After you've set the connection time-out limits for IMAP4, you must restart the IMAP4 services for the settings to take effect. For information about how to restart the IMAP4 services, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

## Use the Shell to set connection time-out limits for IMAP4

This example sets the connection time-out limit for idle authenticated connections.

```powershell
Set -ImapSettings -Identity CAS01 -AuthenticatedConnectionTimeout TimeValue
```

This example sets the connection time-out limit for idle unauthenticated connections.

```powershell
Set -ImapSettings -Identity CAS01 -PreAuthenticatedConnectionTimeout TimeValue
```

After you've set the connection time-out limits for IMAP4, you must restart the IMAP4 services for the settings to take effect. For information about how to restart the IMAP4 services, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-ImapSettings](https://docs.microsoft.com/powershell/module/exchange/Set-ImapSettings).

## How do you know this worked?

To verify that you've successfully set connection limits, do one of the following:

1. In the EAC, navigate to **Servers** **\>** **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **IMAP4**.

4. Scroll down and click **More options**.

5. Under **Time-out settings**, verify the connection settings are correct.

Or

1. Run the following command in the Shell.

    ```powershell
    Get-ImapSettings | format-list
    ```

2. Verify the connection settings are correct.

## For more information

After you set authentication time-out limits for IMAP4, you may also want to:

[Enable IMAP4 in Exchange 2013](enable-imap4-in-exchange-2013-exchange-2013-help.md)

[Set connection limits for IMAP4](set-connection-limits-for-imap4-exchange-2013-help.md)
