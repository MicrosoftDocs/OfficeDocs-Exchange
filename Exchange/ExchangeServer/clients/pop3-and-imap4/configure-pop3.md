---
localization_priority: Normal
description: 'Summary: Learn how to enable and configure POP3 on an Exchange server 2016 or 2019 for access by POP3 clients.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e226a5f1-429d-4046-b925-da6cc151709e
ms.reviewer:
title: Enable and configure POP3 on an Exchange server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Enable and configure POP3 on an Exchange server

By default, POP3 client connectivity isn't enabled in Exchange. To enable POP3 client connectivity, you need to perform the following steps:

1. Start the POP3 services, and configure the services to start automatically:

   - **Microsoft Exchange POP3**: This is the Client Access (frontend) service that POP3 clients connect to.

   - **Microsoft Exchange POP3 Backend**: POP3 client connections from the Client Access service are proxied to the backend service on the server that hold the active copy of the user's mailbox. For more information, see [Client Access protocol architecture](../../architecture/architecture.md#client-access-protocol-architecture).

2. Configure the POP3 settings for external clients.

   By default, Exchange uses the following settings for **internal** POP3 connections:

   - **POP3 server FQDN**: `<ServerFQDN>`. For example, `mailbox01.contoso.com`.

   - **TCP port and encryption method**: 995 for always TLS encrypted connections, and 110 for unencrypted connections, or for opportunistic TLS (**STARTTLS**) that results in an encrypted connection after the initial plain text protocol handshake.

   To allow **external** POP3 clients to connect to mailboxes, you need to configure the POP3 server FQDN, TCP port, and encryption method for external connections. This step causes the external POP3 settings to be displayed in Outlook on the web (formerly known as Outlook Web App) at **Settings** \> **Options** \> **Mail** \> **Accounts** \> **POP and IMAP**.

   ![POP settings in Outlook on the web](../../media/8c89500e-90ad-4fb5-9334-7013de6607a2.png)

3. Restart the POP3 services to save the changes.

4. Configure the authenticated SMTP settings for internal and external clients. For more information, see [Configure authenticated SMTP settings for POP3 and IMAP4 clients in Exchange Server](configure-authenticated-smtp.md).

For more information about POP3, see [POP3 and IMAP4 in Exchange Server](pop3-and-imap4.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- Secure Sockets Layer (SSL) is being replaced by Transport Layer Security (TLS) as the protocol that's used to encrypt data sent between computer systems. They're so closely related that the terms "SSL" and "TLS" (without versions) are often used interchangeably. Because of this similarity, references to "SSL" in Exchange topics, the Exchange admin center, and the Exchange Management Shell have often been used to encompass both the SSL and TLS protocols. Typically, "SSL" refers to the actual SSL protocol only when a version is also provided (for example, SSL 3.0). To find out why you should disable the SSL protocol and switch to TLS, check out [Protecting you against the SSL 3.0 vulnerability](https://blogs.office.com/2014/10/29/protecting-ssl-3-0-vulnerability/).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 and IMAP4 Permissions" section in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Start the POP3 services, and configure the services to start automatically

You can perform this step by using the Windows Services console, or the Exchange Management Shell.

### Use the Windows Services console to start the POP3 services, and configure the services to start automatically

1. On the Exchange server, open the Windows Services console. For example:

   - Run the command `services.msc` from the **Run** dialog, a Command Prompt window, or the Exchange Management Shell.

   - Open Server Manager, and then click **Tools** \> **Services**.

2. In the list of services, select **Microsoft Exchange POP3**, and then click **Action** \> **Properties**.

3. The **Microsoft Exchange POP3 Properties** window opens. On the **General** tab, configure the following settings:

   - **Startup type**: Select **Automatic**.

   - **Service status**: Click **Start**.

   When you're finished, click **OK**.

4. In the list of services, select **Microsoft Exchange POP3 Backend**, and then click **Action** \> **Properties**.

5. The **Microsoft Exchange POP3 Backend Properties** window opens. On the **General** tab, configure the following settings:

   - **Startup type**: Select **Automatic**.

   - **Service status**: Click **Start**.

    When you're finished, click **OK**.

### Use the Exchange Management Shell to start the POP3 services, and configure the services to start automatically

1. Run the following command to start the POP3 services:

   ```powershell
   Start-Service MSExchangePOP3; Start-Service MSExchangePOP3BE
   ```

2. Run the following command to configure the POP3 services to start automatically:

   ```powershell
   Set-Service MSExchangePOP3 -StartupType Automatic; Set-Service MSExchangePOP3BE -StartupType Automatic
   ```

For more information about these cmdlets, see [Start-Service](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/start-service) and [Set-Service](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-service).

### How do you know this step worked?

To verify that you've successfully started the POP3 services, use either of the following procedures:

- On the Exchange server, open Windows Task Manager. On the **Services** tab, verify that the **Status** value for the **MSExchangePOP3** and **MSExchangePOP3BE** services is **Running**.

- In the Exchange Management Shell, run the following command to verify that the POP3 services are running:

  ```powershell
  Get-Service MSExchangePOP3; Get-Service MSExchangePOP3BE
  ```

## Step 2: Use the Exchange Management Shell to configure the POP3 settings for external clients

To configure the POP3 settings for external clients, use the following syntax:

```powershell
Set-PopSettings -ExternalConnectionSettings "<FQDN1>:<TCPPort1>:<SSL | TLS | blank>", "<FQDN2>:<TCPPort2>:<SSL | TLS | blank>"...  -X509CertificateName <FQDN> [-SSLBindings "<IPv4Orv6Address1>:<TCPPort1>","<IPv4Orv6Address2>:<TCPPort2>"...] [-UnencryptedOrTLSBindings "<IPv4Orv6Address1>:<TCPPort1>","<IPv4Orv6Address2>:<TCPPort2>"...]
```

This example allows configures the following settings for external POP3 connections:

- **POP3 server FQDN**: mail.contoso.com

- **TCP port**: 995 for always TLS encrypted connections, and 110 for unencrypted connections or opportunistic TLS (STARTTLS) encrypted connections.

- **Internal Exchange server IP address and TCP port for always TLS encrypted connections**: All available IPv4 and IPv6 addresses on the server on port 995 (we aren't using the _SSLBindings_ parameter, and the default value is `[::]:995,0.0.0.0:995`).

- **Internal Exchange server IP address and TCP port for unencrypted or opportunistic TLS (STARTTLS) encrypted connections**: All available IPv4 and IPv6 addresses on the server on port 110 (we aren't using the _UnencryptedOrTLSBindings_ parameter, and the default value is `[::]:110,0.0.0.0:110`).

- **FQDN used for encryption**: mail.contoso.com. This value identifies the certificate that matches or contains the POP3 server FQDN.

```powershell
Set-PopSettings -ExternalConnectionSettings "mail.contoso.com:995:SSL","mail.contoso.com:110:TLS" -X509CertificateName mail.contoso.com
```

**Notes**:

- For detailed syntax and parameter information, see [Set-PopSettings](https://docs.microsoft.com/powershell/module/exchange/set-popsettings).

- The external POP3 server FQDN that you configure needs to have a corresponding record in your public DNS, and the TCP port (110 or 995) needs to be allowed through your firewall to the Exchange server.

- The combination of encryption methods and TCP ports that you use for the _ExternalConnectionSettings_ parameter need to match the corresponding TCP ports and encryption methods that you use for the _SSLBindings_ or _UnencryptedOrTLSBindings_ parameters.

- Although you can use a separate certificate for POP3, we recommend that you use the same certificate as the other Exchange IIS (HTTP) services, which is likely a wildcard certificate or a subject alternative name (SAN) certificate from a commercial certification authority that's automatically trusted by all clients. For more information, see [Certificate requirements for Exchange services](../../architecture/client-access/certificates.md#certificate-requirements-for-exchange-services).

- If you use a single subject certificate, or a SAN certificate, you also need to assign the certificate to the Exchange POP service. You don't need to assign a wildcard certificate to the Exchange POP service. For more information, see [Assign certificates to Exchange Server services](../../architecture/client-access/assign-certificates-to-services.md).

### How you do know this step worked?

To verify that you've successfully configured the POP3 settings for external clients, run the following command in the Exchange Management Shell and verify the settings:

```powershell
Get-PopSettings | Format-List *ConnectionSettings,*Bindings,X509CertificateName
```

For more information, see [Get-POPSettings](https://docs.microsoft.com/powershell/module/exchange/get-popsettings).

## Step 3: Restart the POP3 services

After you enable and configure POP3, you need to restart the POP3 services on the server by using the Windows Services console, or the Exchange Management Shell.

### Use the Windows Services console to restart the POP3 services

1. On the Exchange server, open the Windows Services console.

2. In the list of services, select **Microsoft Exchange POP3**, and then click **Action** \> **Restart**.

3. In the list of services, select **Microsoft Exchange POP3 Backend**, and then click **Action** \> **Restart**.

### Use the Exchange Management Shell to restart the POP3 services

Run the following command to restart the POP3 services.

```powershell
Restart-Service MSExchangePOP3; Restart-Service MSExchangePOP3BE
```

For more information about this cmdlet, see [Restart-Service](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-service).

To verify that you've successfully restarted the POP3 services, run the following command:

```powershell
Get-Service MSExchangePOP3; Get-Service MSExchangePOP3BE
```

## Step 4: Configure the authenticated SMTP settings for POP3 clients

Because POP3 isn't used to send email messages, you need to configure the authenticated SMTP settings that are used by internal and external POP3 clients. For more information, see [POP3 and IMAP4 in Exchange Server](pop3-and-imap4.md).

## How do you know this task worked?

To verify that you have enabled and configured POP3 on the Exchange server, perform the following procedures:

1. Open a mailbox in Outlook on the web, and then click **Settings** \> **Options**.


    ![Options menu location in Outlook on the web](../../media/f1227a01-7f83-4af9-abf5-2c3dec6cf3d0.png)

2. Click **Mail** \> **Accounts** \> **POP and IMAP** and verify the correct POP3 settings are displayed.

    ![POP settings in Outlook on the web](../../media/8c89500e-90ad-4fb5-9334-7013de6607a2.png)

    **Note**: If you configured 995/SSL **and** 110/TLS values for the _ExternalConnectionSettings_ parameter on the **Set-PopSettings** cmdlet, only the 995/SSL value is displayed in Outlook on the web. Also, if the external POP3 settings that you configured don't appear as expected in Outlook on the web after you restart the POP3 services, run the commands `net stop was /y` and `net start w3svc` to restart Internet Information Services (IIS).

3. You can test POP3 client connectivity to the Exchange server by using the following methods:

   - **Internal clients**: Use the **Test-PopConnectivity** cmdlet. For example, `Test-PopConnectivity -ClientAccessServer <ServerName> -Lightmode -MailboxCredential (Get-Credential)`. For more information, see [Test-PopConnectivity](https://docs.microsoft.com/powershell/module/exchange/test-popconnectivity).

     **Note**: The _Lightmode_ switch tells the command test POP3 logons to the server. To test sending (SMTP) and receiving (POP3) a message, you need to configure the authenticated SMTP settings as described in [POP3 and IMAP4 in Exchange Server](pop3-and-imap4.md).

   - **External clients**: Use the **POP Email** test in the [Microsoft Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/tests/o365).

     **Note**: You can't use POP3 to connect to the Administrator mailbox. This limitation was intentionally included in Exchange 2016 and Exchange 2019 to enhance the security of the Administrator mailbox.

## Next steps

To enabled or disable POP3 access to individual mailboxes, see [Enable or disable POP3 or IMAP4 access to mailboxes in Exchange Server](configure-mailbox-access.md).
