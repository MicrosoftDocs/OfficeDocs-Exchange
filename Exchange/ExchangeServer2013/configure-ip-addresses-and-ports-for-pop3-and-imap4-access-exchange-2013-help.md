---
title: 'Configure IP addresses and ports for POP3 and IMAP4 access: Exchange 2013 Help'
TOCTitle: Configure IP addresses and ports for POP3 and IMAP4 access
ms:assetid: 8292747b-6626-4d7f-ba73-1e17f5d99fa4
ms:mtpsurl: https://technet.microsoft.com/library/Bb123530(v=EXCHG.150)
ms:contentKeyID: 50395401
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure IP addresses and ports for POP3 and IMAP4 access

_**Applies to:** Exchange Server 2013_

You can use the EAC and the Shell to configure the Microsoft Exchange POP3 and IMAP4 services to use IP addresses and ports that are different from the default settings.

> [!NOTE]
> Enter IP addresses and IP address ranges in the Internet Protocol Version 4 (IPv4) format, Internet Protocol Version 6 (IPv6) format, or both formats. A default installation of Windows Server 2008 enables support for IPv4 and IPv6.

For additional information related to POP3 and IMAP4, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 settings" and "IMAP4 settings" entries in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Configure IP addresses and ports for POP3

## Use the EAC to configure IP addresses and ports for POP3

1. In the EAC, navigate to **Servers** **\>** **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **POP3**.

4. Under **TLS or unencrypted connections**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

5. On the **Add IP address** page, under **IP address**, choose one of the following:

   - **All available IPv4 addresses**: Use all available IPv4 IP addresses for a server.

   - **All available IPv6 addresses**: Use all available IPv6 IP addresses for a server.

   - **Specify an IP address**: Use a specific IP address.

6. Under **Port**, enter a port number, or accept the default port.

7. Click **Save** to save your changes.

After you've set the IP address and port settings for POP3, you must restart the POP3 service for the settings to take effect. For information about how to restart the POP3 service, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

## Use the Shell to configure IP addresses and ports for POP3

This example sets the IP address and port for communicating with Exchange by using POP3 with Secure Sockets Layer (SSL).

```powershell
Set-PopSettings -SSLBindings: IPaddress:Port
```

This example sets the IP address and port for communicating with Exchange by using POP3 with no encryption or Transport Layer Security (TLS) encryption.

```powershell
Set-PopSettings -UnencryptedOrTLSBindings IPaddress:Port
```

After you've set the IP address and port settings for POP3, you must restart the POP3 service for the settings to take effect. For information about how to restart the POP3 service, see [Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-PopSettings](https://docs.microsoft.com/powershell/module/exchange/Set-PopSettings).

## How do you know this worked?

Do the following to verify that you have changed POP3 IP address and port settings on a server.

1. Run the following command in the Shell.

   ```powershell
   Get-PopSettings | format-list
   ```

2. Verify the *UnencryptedOrTLSBindings* and *SSLBindings* settings are correct.

## Configure IP addresses and ports for IMAP4

## Use the EAC to configure IP addresses and ports for IMAP4

1. In the EAC, navigate to **Servers** **\>** **Servers**.

2. In the list of servers, select the Client Access server, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **IMAP4**.

4. If you want to set TLS or unencrypted connection settings, under **TLS or unencrypted connections**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). If you want to change Secure Sockets Layer (SSL) connection settings, under **Secure Sockets Layer (SSL) connections**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

5. On the **Add IP address** page, under **IP address**, choose one of the following:

   - **All available IPv4 addresses**: Use all available IPv4 IP addresses for a server.

   - **All available IPv6 addresses**: Use all available IPv6 IP addresses for a server.

   - **Specify an IP address**: Use a specific IP address.

6. Under **Port**, enter a port number, or accept the default port.

7. Click **Save** to save your changes.

After you've set the IP address and port settings for IMAP4, you must restart the IMAP4 services for the settings to take effect. For information about how to restart the IMAP4 services, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

## Use the Shell to configure IP addresses and ports for IMAP4

This example sets the IP address and port for communicating with Exchange by using IMAP4.

```powershell
Set-ImapSettings -SSLBindings: IPaddress:Port
```

This example sets the IP address and port for communicating with Exchange by using IMAP4 with no encryption or TLS encryption.

```powershell
Set-ImapSettings -UnencryptedOrTLSBindings IPaddress:Port
```

After you've set the IP address and port settings for IMAP4, you must restart the IMAP4 service for the settings to take effect. For information about how to restart the IMAP4 service, see [Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md).

For more information about syntax and parameters, see [Set-ImapSettings](https://docs.microsoft.com/powershell/module/exchange/Set-ImapSettings).

## How do you know this worked?

Do the following to verify that you have changed IMAP4 IP address and port settings on a server.

1. Run the following command in the Shell.

   ```powershell
   Get-ImapSettings | format-list
   ```

2. Verify the *UnencryptedOrTLSBindings* and *SSLBindings* settings are correct.

## For more information

After you configure IP addresses and ports for POP3 and IMAP4, you may also want to:

[Enable IMAP4 in Exchange 2013](enable-imap4-in-exchange-2013-exchange-2013-help.md)

[Enable POP3 in Exchange 2013](enable-pop3-in-exchange-2013-exchange-2013-help.md)
