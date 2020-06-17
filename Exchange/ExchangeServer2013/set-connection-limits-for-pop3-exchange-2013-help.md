---
title: 'Set connection limits for POP3: Exchange 2013 Help'
TOCTitle: Set connection limits for POP3
ms:assetid: 512d61c2-2a34-4813-92a9-875339d3388b
ms:mtpsurl: https://technet.microsoft.com/library/Aa997988(v=EXCHG.150)
ms:contentKeyID: 50395399
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set connection limits for POP3

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to manage POP3 connection limits for your organization.

When you specify connection limits for POP3, you can select connection limits for the server, an IP address, or a specific user.

For additional information related to POP3, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EMC to set POP3 connection limits for a server, an IP address, or a user

1. In the EAC, navigate to **Servers** **\>** **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **POP3**.

4. Scroll down and click **More options**.

5. Under **Connection limits**, use the following settings:

   - **Maximum connections**: Specifies the total number of connections the specified server will accept. This includes authenticated and unauthenticated connections. The default value is 2,147,483,647. The possible values are from 1 through 2,147,483,647.

   - **Maximum connections from a single IP address**: Specifies the number of connections that the server will accept from a single IP address. The default value is 2,147,483,647. The possible values are from 1 through 2,147,483,647.

   - **Maximum connections from a single user**: Specifies the maximum number of connections that the server will accept from a particular user. The default value is 16. The possible values are from 1 through 2,147,483,647.

   - **Maximum command size (bytes)**: specifies the maximum size of a single command. The default size is 512. The possible values are from 40 through 1,024.

6. Click **Apply**, and then click **OK** to save your changes.

After you set connection limits, you must restart the POP3 services. For information about how to restart the POP3 services, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

## Use the Shell to set POP3 connection limits for a server, an IP address, or a user

This example sets the connection limit for a server named CAS01.

```powershell
Set-PopSettings -Server CAS01 -MaxConnections Value
```

This example sets the connection limit for an IP address to a server named CAS01.

```powershell
Set-PopSettings -Server CAS01 -MaxConnectionsFromSingleIP Value
```

This example sets the connection limit for a user.

```powershell
Set-PopSettings -MaxConnectionsPerUser Value
```

This example sets the maximum command size.

```powershell
Set-PopSettings -MaxCommandSize Value
```

After you set connection limits, you must restart the POP3 services. For information about how to restart the POP3 services, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-PopSettings](https://docs.microsoft.com/powershell/module/exchange/Set-PopSettings).

## How do you know this worked?

To verify that you've successfully set connection limits, do one of the following:

1. In the EAC, navigate to **Servers** **\>** **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **POP3**.

4. Scroll down and click **More options**.

5. Under **Connection limits**, verify the connection settings are correct.

Or

1. Run the following command in the Shell.

    ```powershell
    Get-PopSettings | format-list
    ```

2. Verify the connection settings are correct.

## For more information

After you set POP3 connection limits for a server, an IP address, or a user, you may also want to:

[Enable POP3 in Exchange 2013](enable-pop3-in-exchange-2013-exchange-2013-help.md)

[Set connection time-out limits for POP3](set-connection-time-out-limits-for-pop3-exchange-2013-help.md)
