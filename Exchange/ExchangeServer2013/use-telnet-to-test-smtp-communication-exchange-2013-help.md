---
title: 'Use Telnet to test SMTP communication: Exchange 2013 Help'
TOCTitle: Use Telnet to test SMTP communication
ms:assetid: 8a5f6715-baa4-48dd-8600-02c6b3d1aa9d
ms:mtpsurl: https://technet.microsoft.com/library/Bb123686(v=EXCHG.150)
ms:contentKeyID: 50934219
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Use Telnet to test SMTP communication

_**Applies to:** Exchange Server 2013_

This topic explains how to use Telnet to test Simple Mail Transfer Protocol (SMTP) communication between messaging servers. By default, SMTP listens on port 25. If you use Telnet on port 25, you can enter the SMTP commands that are used to connect to an SMTP server and send a message exactly as if your Telnet session was an SMTP messaging server. You can see the success or failure of each step in the connection and message submission process.

Here are the scenarios where you may want to use Telnet to test SMTP communication to or from the transport servers that exist in your Microsoft Exchange organization:

- Connect to your organization's Internet-facing Exchange server from a host that is located outside your perimeter network and send a test message.

- Connect to a remote messaging server from your organization's Internet-facing Exchange server and send a test message.

The procedure in this topic shows you how to use Telnet Client, which is a component that is included with Microsoft Windows. Third-party Telnet clients may require a syntax that is different from that of the Windows Telnet component.

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes

- Exchange permissions don't apply to the procedures in this topic. These procedures are performed in the operating system of the Exchange Server or a client computer.

- The procedures in this topic are best used to connect to and from Internet-facing servers that allow anonymous connections. Message transmission between internal Exchange servers is encrypted and authenticated. To use Telnet to connect to the Hub Transport service on a Mailbox server, you'll need to create a Receive connector that's configured to allow anonymous access or Basic authentication to receive messages. If the connector allows Basic authentication, you need a utility to convert the text strings that are used for the username and password into the Base64 format. Because the username and password are easily discernible when Basic authentication is used, we don't recommend Basic authentication without encryption.

- If you connect to a remote messaging server, consider performing the procedures in this topic on your Internet-facing Exchange server. This will help to avoid rejection of the test message by remote messaging servers that are configured to validate the source IP address, the corresponding domain name system (DNS) domain name, and the reverse lookup IP address of any Internet host that tries to send a message to the server.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Install the Telnet Client in Windows

By default, the Telnet Client isn't installed in most client or server versions of the Microsoft Windows operating systems. To install it, see [Install Telnet Client](https://go.microsoft.com/fwlink/p/?linkid=179054).

## Step 2: Use Nslookup to find the FQDN or IP address in the MX record of the remote SMTP server

To connect to a destination SMTP server by using Telnet on port 25, you must use the fully qualified domain name (FQDN) or the IP address of the SMTP server. If the FQDN or IP address is unknown, the easiest way to find this information is to use the Nslookup command-line tool to find the MX record for the destination domain.

1. At a command prompt, type **nslookup**, and then press ENTER. This command opens the Nslookup session.

2. Type **set type=mx** and then press ENTER.

3. Type **set timeout=20** and then press ENTER. By default, Windows DNS servers have a 15-second recursive DNS query time-out limit.

4. Type the name of the domain for which you want to find the MX record. For example, to find the MX record for the fabrikam.com domain, type **fabrikam.com.**, and then press ENTER.

   > [!NOTE]
   > The trailing period (&nbsp;<STRONG>.</STRONG>&nbsp;) indicates a FQDN. The use of the trailing period prevents any default DNS suffixes that are configured for your network from being unintentionally added to the domain name.

   The output of the command will resemble the following:

   ```console
   fabrikam.com mx preference=10, mail exchanger = mail1.fabrikam.com
   fabrikam.com mx preference=20, mail exchanger = mail2.fabrikam.com
   mail1.fabrikam.com internet address = 192.168.1.10
   mail2 fabrikam.com internet address = 192.168.1.20
    ```

    You can use any of the host names or IP addresses that are associated with the MX records as the destination SMTP server. A lower value of preference indicates a preferred SMTP server. You can use multiple MX records and different values of preference for load balancing and fault tolerance.

5. When you're ready to end the Nslookup session, type **exit**, and then press ENTER.

> [!NOTE]
> Firewall or Internet proxy restrictions that are imposed on your organization's internal network may prevent you from using the Nslookup tool to query public DNS servers on the Internet.

## Step 3: Use Telnet on Port 25 to test SMTP communication

In this example, the following values are used:

- **Destination SMTP server**: mail1.fabrikam.com

- **Source domain**: contoso.com

- **Sender's e-mail address**: chris@contoso.com

- **Recipient's e-mail address**: kate@fabrikam.com

- **Message subject**: Test from Contoso

- **Message body**: This is a test message

> [!NOTE]
> <UL>
> <LI>
> <P>The commands in Telnet Client are not case-sensitive. The SMTP command verbs are capitalized for clarity.</P>
> <LI>
> <P>You can't use the backspace key after you have connected to the destination SMTP server within the Telnet session. If you make a mistake as you type an SMTP command, you must press ENTER and then type the command again. Unrecognized SMTP commands or syntax errors result in an error message that resembles the following:</P>
>
> ```console
> 500 5.3.3 Unrecognized command
> ```
> </LI></UL>

1. At a command prompt, type **telnet**, and then press ENTER. This command opens the Telnet session.

2. Type **set localecho** and then press ENTER. This optional command lets you view the characters as you type them. This setting may be required for some SMTP servers.

3. Type **set logfile** *\<filename\>*. This optional command enables logging of the Telnet session to the specified log file. If you only specify a file name, the location of the log file is the current working directory. If you specify a path and a file name, the path must be local to the computer. Both the path and the file name that you specify must be entered in the Microsoft DOS 8.3 format. The path that you specify must already exist. If you specify a log file that doesn't exist, it will be created for you.

4. Type **open mail1.fabrikam.com 25** and then press ENTER.

5. Type **EHLO contoso.com** and then press ENTER.

6. Type **MAIL FROM:chris\@contoso.com** and then press ENTER.

7. Type **RCPT TO:kate@fabrikam.com NOTIFY=success,failure** and then press ENTER. The optional NOTIFY command defines the particular delivery status notification (DSN) messages that the destination SMTP server must provide to the sender. DSN messages are defined in RFC 1891. In this case, you're requesting a DSN message for successful or failed message delivery.

8. Type **DATA** and then press ENTER. You will receive a response that resembles the following:

    ```console
    354 Start mail input; end with <CLRF>.<CLRF>
    ```

9. Type **Subject: Test from Contoso** and then press ENTER.

10. Press ENTER. RFC 2822 requires a blank line between the `Subject:` header field and the message body.

11. Type **This is a test message** and then press ENTER.

12. Press ENTER, type a period ( **.** ) and then press ENTER. You will receive a response that resembles the following:

    ```console
    250 2.6.0 <GUID> Queued mail for delivery
    ```

13. To disconnect from the destination SMTP server, type **QUIT** and then press ENTER. You will receive a response that resembles the following:

    ```console
    221 2.0.0 Service closing transmission channel
    ```

14. To close the Telnet session, type **quit** and then press ENTER.

## Step 4: Evaluate the Results of the Telnet Session

This section provides information about responses that may be provided to the following commands, which were used in the previous example:

- Open mail1.fabrikam.com 25

- EHLO contoso.com

- MAIL FROM:chris@contoso.com

- RCPT TO:kate@fabrikam.com NOTIFY=success,failure

    > [!NOTE]
    > The 3-digit SMTP response codes that are defined in RFC 2821 are the same for all SMTP messaging servers. The text descriptions may differ slightly for some SMTP messaging servers.

## Open mail1.fabrikam.com 25

**Successful Response**: `220 mail1.fabrikam.com Microsoft ESMTP MAIL Service ready at <day-date-time>`

**Failure Response**: `Connecting to mail1.fabrikam.com...Could not open connection to the host, on port 25: Connect failed`

**Possible Reasons for Failure**:

- The destination SMTP service is unavailable.

- There are restrictions on the destination firewall.

- There are restrictions on the source firewall.

- An incorrect FQDN or IP address for the destination SMTP server was specified.

- An incorrect port number was specified.

## EHLO contoso.com

**Successful Response**: `250 mail1.fabrikam.com Hello [<sourceIPaddress>]`

**Failure Response**: `501 5.5.4 Invalid domain name`

**Possible Reasons for Failure**: There are invalid characters in the domain name. Alternatively, there are connection restrictions on the destination SMTP server.

> [!NOTE]
> EHLO is the Extended Simple Message Transfer Protocol (ESMTP) verb that is defined in RFC 2821. ESMTP servers can advertise their capabilities during the initial connection. These capabilities include their maximum accepted message size and their supported authentication methods. HELO is the older SMTP verb that is defined in RFC 821. Most SMTP messaging servers support ESMTP and EHLO.

## MAIL FROM:chris@contoso.com

**Successful Response**: `250 2.1.0 Sender OK`

**Failure Response**: `550 5.1.7 Invalid address`

**Possible Reasons for Failure**: There is a syntax error in the sender's e-mail address.

**Failure Response**: `530 5.7.1 Client was not authenticated`

**Possible Reasons for Failure**: The destination server does not accept anonymous message submissions. You receive this error if you try to use Telnet to submit a message directly to a Hub Transport server.

## RCPT TO:kate@fabrikam.com NOTIFY=success,failure

**Successful Response**: `250 2.1.5 Recipient OK`

**Failure Response**: `550 5.1.1 User unknown`

**Possible Reasons for Failure**: The specified recipient does not exist in the organization.
