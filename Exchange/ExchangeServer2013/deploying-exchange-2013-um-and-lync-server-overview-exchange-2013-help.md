---
title: 'Deploying Exchange 2013 UM and Lync Server overview: Exchange 2013 Help'
TOCTitle: Deploying Exchange 2013 UM and Lync Server overview
ms:assetid: 96fcb0dd-79ee-4e55-9e59-3ee7fbe3c4a1
ms:mtpsurl: https://technet.microsoft.com/library/Bb676409(v=EXCHG.150)
ms:contentKeyID: 49315469
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deploying Exchange 2013 UM and Lync Server overview

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Unified Messaging (UM) and Microsoft Lync Server can be deployed together to provide voice messaging, instant messaging, enhanced user presence, audio/video conferencing, and an integrated email and messaging experience for users in your organization. Unified Messaging is used to provide call answering for voice mail, Outlook Voice Access, and auto attendant services. Microsoft Lync Server enables more advanced features found in Enterprise Voice, such as instant messaging (IM), conferencing, and inbound and outbound calling. This topic describes how to configure Unified Messaging and Microsoft Lync Server to support these features.

> [!TIP]
> Microsoft Office Communications Server 2007 R2 can also be deployed together with Unified Messaging. In this topic, "Microsoft Lync Server" refers to Microsoft Lync Server 2010 or Microsoft Lync Server 2013.

Looking for more information about Microsoft Lync Server? See [Microsoft Lync Server](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013).

## Deploying Exchange UM and Lync Server overview

Unified Messaging combines voice and email messaging into a single messaging infrastructure. Microsoft Lync Server Enterprise Voice takes advantage of the UM infrastructure to provide voice mail, Outlook Voice Access, call notifications, and auto attendants.

The following list shows the simplified deployment steps for UM and Lync Server. Details about each step are included later in this topic.

1. Install Microsoft Lync Server in the same topology where the Client Access servers running the Microsoft Exchange Unified Messaging Call Router service and the Mailbox servers running the Microsoft Exchange Unified Messaging service will be installed. Confirm that at least one Lync pool is created.

2. Install a certificate that's valid and signed by a private or public certification authority (CA) and is trusted by Lync Server.

3. Install the Client Access servers and Mailbox servers. Verify installation.

4. Install a certificate that's valid and signed by the same CA as the certificate you installed on your Lync servers.

5. Create and configure a Session Initiation Protocol (SIP) URI dial plan.

6. Add all Client Access and Mailbox servers to the SIP URI dial plan. However, if you have multiple SIP URI dial plans, you must add all Client Access and Mailbox servers to all SIP URl dial plans.

7. Run the ExchUcUtil.ps1 script from the \<Exchange Installation folder\>\\Exchange Server\\Script folder on a Mailbox server.

   > [!IMPORTANT]
   > The ExchUcUtil.ps1 script creates one or more UM IP gateways for Lync integration. You must disable outgoing calls on all UM IP gateways except one gateway that the script created. This includes disabling outgoing calls on UM IP gateways that were created before you ran the script. To disable outgoing calls on a UM IP gateway, see <A href="disable-outgoing-calls-on-https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateways">Disable outgoing calls on UM IP gateways</A>.

8. Run **OcsUmUtil.exe** from the %CommonProgramFiles%\\Microsoft Lync Server 2013\\Support folder on a Lync Server.

9. Deploy the Mediation Server and media gateways.

10. Install a certificate on your Mediation Server that's valid and signed by the same CA as the certificate you installed on your Lync servers.

11. Enable your users for UM and Enterprise Voice.

## Certificate configuration recommendations

You must have a certificate that's trusted by both the computers running Exchange and the computers running Lync Server. In an environment that has Lync Server and Unified Messaging, use the following guidelines for deploying a trusted certificate:

- On your Lync servers, Client Access servers, Mailbox servers, Mediation Server, and media gateways, import a certificate that's valid and signed by a private or public CA. This should be a trusted third-party commercial certificate or a public key infrastructure (PKI) certificate.

- It's less complex if you import the same third-party commercial or PKI certificate to each Exchange server. Also, install this trusted certificate on each computer running Microsoft Lync Server and Mediation Server. This will make your certificate deployment less complicated and reduce the administrative overhead associated with deploying certificates. However, make sure you obtain a trusted certificate that supports subject alternative names (SANs).

  When you're deploying Transport Layer Security (TLS) with UM, the certificates that are used on the Client Access server and the Mailbox server both must contain the local computer's fully qualified domain name (FQDN) in the certificate's Subject Name. To work around this issue, use a public certificate and import the certificate on all Client Access and Mailbox servers, any VoIP gateways, IP PBXs, and all the Lync servers.

  If your deployment includes VoIP gateways or IP PBXs, and if you use a SIP secured or Secured dial plan, a trusted certificate is required between the Client Access and Mailbox servers and the VoIP gateways or IP PBXs. A trusted certificate is also required if a direct SIP connection is used. If you use a SIP secured or Secured dial plan, you can use the same trusted certificate on your Lync and Exchange servers that's used on your VoIP gateways or IP PBXs.

- When you connect Exchange Client Access and Mailbox servers to Microsoft Lync servers or to third-party SIP gateways or Private Branch eXchange (PBX) telephony equipment, you must use a certificate that's valid and signed by an internal or public, third-party certification authority (CA) to establish secured sessions. You can use a single certificate on all the Client Access and Mailbox servers as long as the certificate has the FQDNs of all the Client Access and Mailbox servers in its SAN list. Or, you can generate a different certificate for each Client Access and Mailbox server, with the FQDN of the local computer present in the subject common name (CN) or SAN list of the certificate for that server. Exchange UM doesn't support wildcard certificates with Microsoft Lync Server.

  A non-wildcard Subject Name is required for Lync Server and Exchange to work together. UM and Lync Server use the Subject Name as a way to indicate that they're trusted SIP peers. Lync Server also needs a non-wildcard Subject Name in some call-routing scenarios. The FQDN must be used as the "Issued to" value.

  For Exchange UM, it isn't supported to put a wildcard in the Certificate Name. However, you can put a wildcard in the SAN.

The following table shows the certificate requirements for installing and configuring certificates for Exchange UM.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Topology</th>
<th>Certificate configuration</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Client Access and Mailbox on the same server (without Lync 2010 or 2013; non-SIP dial plans)</p></td>
<td><p>A certificate is required between Client Access and Mailbox servers. This is the same certificate that's used between the Client Access and Mailbox servers and the VoIP gateway, IP PBX, or SBC.</p></td>
</tr>
<tr class="even">
<td><p>Client Access and Mailbox on different servers (without Lync 2010 or 2013; non-SIP dial plans)</p></td>
<td><p>A certificate is required. The certificate must match on the Client Access and Mailbox servers. A certificate is also required between Client Access and Mailbox servers and the VoIP gateway, IP PBX, or SBC. This can be the same or a different certificate than the certificate that is used between the Client Access and Mailbox servers.For Client Access and Mailbox servers, you can run the <strong>Create-ExchangeCertificate</strong> cmdlet from either server.</p></td>
</tr>
<tr class="odd">
<td><p>Client Access and Mailbox on the same server (with Lync 2010 or 2013 and SIP dial plans)</p></td>
<td><p>A certificate is required. The Client Access and Mailbox servers must have the same root certificate as the Lync 2010 or 2013 servers.</p></td>
</tr>
<tr class="even">
<td><p>Client Access and Mailbox on different servers (with Lync 2010 or 2013 and SIP dial plans)</p></td>
<td><p>A certificate is required. The Client Access and Mailbox servers must have the same root certificate as the Lync 2010 or 2013 servers.</p></td>
</tr>
</tbody>
</table>

## Deployment steps

After you install the required servers in your organization, there's a recommended sequence of steps that you must perform in your Exchange Unified Messaging and Lync Server deployments to correctly deploy Enterprise Voice for your users.

For details about Microsoft Lync Server, see [Microsoft Lync Server](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013).

You must complete the following steps to configure Unified Messaging to work with the Enterprise Voice features in Lync Server:

1. Create one or more Unified Messaging SIP URI dial plans that each map to a corresponding Lync Server location profile. An Enterprise Voice location profile must be created for each Exchange UM dial plan. You can use the **Get-UMDialPlan** cmdlet to obtain the FQDN of a SIP URI dial plan. For more information about how to create a SIP URI dial plan, see [Create a UM dial plan](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan).

   > [!IMPORTANT]
   > When you're integrating Exchange UM and Lync Server, you'll probably find it unnecessary to configure dialing rules or dialing rule groups in Exchange UM. Lync Server is designed to perform call routing and number translation for users in your organization, and will also do this when the calls are made by Unified Messaging on behalf of users.

2. Install a certificate on the Client Access and Mailbox servers that's valid and signed by a private or public CA. This is the same CA that was used on the Lync Servers.

3. Encrypt the Voice over IP (VoIP) traffic by configuring the SIP URI dial plan as SIP secured or Secured.

   > [!WARNING]
   > If you set your security setting to SIP Secured to require encryption for SIP traffic only, this setting is insufficient on a dial plan if the Front End pool is configured to require encryption (which means that the pool requires encryption for both SIP and RTP traffic). When the dial plan and pool security settings aren't compatible, all calls to Exchange UM from the Front End pool will fail, resulting in an error indicating that you have an "Incompatible security setting".

   Although a UM dial plan can be configured as SIP secured or Secured, we recommend that you configure the dial plan as Secured to enable Lync Phone Edition devices to work correctly. This is recommended because of the default encryption level settings configured in Lync Server. A Lync Phone Edition device will work only if the encryption settings are configured as shown in the following table. This table shows the relationship between the encryption settings for Lync Server and UM dial plans.

   **Encryption settings for Lync Phone Edition**

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Lync Server</th>
    <th>UM dial plan</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Encryption required (default)</p></td>
    <td><p>Secured</p></td>
    </tr>
    <tr class="even">
    <td><p>Encryption optional</p></td>
    <td><p>SIP secured/Secured</p></td>
    </tr>
    <tr class="odd">
    <td><p>No encryption</p></td>
    <td><p>SIP secured</p></td>
    </tr>
    </tbody>
    </table>

4. Add all Client Access and Mailbox servers to the SIP dial plan. To enable the server to answer incoming calls, you must add all Exchange servers to a dial plan if you want them to answer calls from Lync Server.

5. Set the startup mode and the TLS listening port on the Client Access and Mailbox servers that are added to the SIP URI dial plan to Dual and then restart the Microsoft Exchange Unified Messaging service on each Mailbox server and the Microsoft Exchange Unified Messaging Call Router service on each Client Access server.

6. Create and configure a UM auto attendant. For details, see [Set up a UM auto attendant](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/set-up-um-auto-attendant).

7. When you enable users for voice mail, create a SIP address for the users who will use Enterprise Voice. In most cases, this SIP address will be the same SIP address that will be used when a user is enabled for Enterprise Voice. For details, see [Enable a user for voice mail](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail).

   > [!IMPORTANT]
   > Users who are associated with a SIP URI dial plan can't receive incoming faxes. This is because incoming voice and fax calls are routed through a Mediation Server and faxing isn't supported when using a Mediation Server.

8. Open the Exchange Management Shell and run the exchucutil.ps1 script located in the %Program Files%\\Microsoft\\Exchange Server\\V15\\Scripts folder. The exchucutil.ps1 script does the following:

   - Grants Lync Server permission to read Exchange UM Active Directory components, specifically, the SIP URI dial plan that was created in the previous task. For details about how to configure permissions in Active Directory, see [How to Use ADSI Edit to Apply Permissions](https://go.microsoft.com/fwlink/p/?linkid=82751).

   - Creates a UM IP gateway for each Lync Server pool or for each server running Lync Server Standard Edition that hosts users who will be enabled for Enterprise Voice. For details, see [Create a UM IP gateway](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway).

   - Create an Exchange UM hunt group for each UM IP gateway. The hunt group pilot identifier will be the name of the dial plan associated with the corresponding UM IP gateway. The hunt group must specify the UM SIP dial plan used with the UM IP gateway.

9. Enable users for voice mail. When you enable them, make sure you enter a valid SIP address for the user and link them to a SIP dial plan. For details, see [Enable a user for voice mail](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail).

You must also complete the following tasks to configure Lync Server to work with Exchange UM:

- Create location profiles or Lync dial plans. The location profile name doesn't have to match the FQDN of the corresponding UM dial plans.

- Assign location profiles to the Lync Server pools.

- Deploy and configure media gateways or Mediation Servers. You must also import a certificate from the same trusted CA as was used for the certificates on the Client Access and Mailbox servers and Lync Server.

- Define telephone usage, create and assign voice policies and outbound call routes.

- Configure the users for Enterprise Voice and add a TEL URI and SIP identifier.

- Run **ocsumutil.exe**, which creates the contact objects for Outlook Voice Access and for the auto attendants.

  > [!NOTE]
  > When you install Lync Server, the <STRONG>msRTC-SIPLine</STRONG> attribute is added to Active Directory. If you haven't installed Lync Server in your environment, this attribute isn't added to Active Directory, and caller ID name resolution across dial plans in a single forest and in cross-forest scenarios won't work correctly unless you configure Unified Messaging proxy addresses for users who aren't UM-enabled.

After you configure the Lync Server and the Unified Messaging servers, you must enable the user to use Lync Server and install Lync on the user's client computer.

> [!IMPORTANT]
> When you're integrating Unified Messaging and Lync Server, missed call notifications aren't available to users who have a mailbox located on an Exchange 2007 or Exchange 2010 Mailbox server. A missed call notification is generated when a user disconnects before the call is sent to a Mailbox server.

## For more information

For more information about how to perform the tasks that must be completed for Microsoft Lync Server, see [Microsoft Lync Server](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013).
