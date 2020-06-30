---
title: 'Upgrade Exchange 2010 UM to Exchange 2013 UM: Exchange 2013 Help'
TOCTitle: Upgrade Exchange 2010 UM to Exchange 2013 UM
ms:assetid: 01aa5dab-689b-4738-afab-0d2f11a60b39
ms:mtpsurl: https://technet.microsoft.com/library/Dn169226(v=EXCHG.150)
ms:contentKeyID: 53382778
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Upgrade Exchange 2010 UM to Exchange 2013 UM

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you're upgrading a Microsoft Exchange 2010 organization with Unified Messaging (UM) to Exchange 2013 Unified Messaging, there are steps that are required and other steps that were already completed as part of your Exchange 2010 UM deployment. Depending on your telephony environment and the UM components that were created and configured to support Unified Messaging in Exchange 2010, you may need to deploy additional telephony equipment including Voice over IP (VoIP) gateways, IP Private Branch eXchanges (PBXs), or traditional or SIP-enabled PBXs and then create and configure any additional UM components that will be required for Exchange 2013 UM.

## What do you need to know before you begin?

- Estimated time to complete this task: 45-90 minutes.

- Verify that you have the appropriate permissions in the Exchange 2010 and Exchange 2013 organization to create and configure all the required components.

- Verify that you've deployed and correctly configured your telephony components, including VoIP gateways and PBXs, IP PBXs, or Session Initiation Protocol (SIP)-enabled PBXs.

- Verify that you've correctly installed and configured the Client Access servers running the Microsoft Exchange Unified Messaging Call Router (UM Call Router) service and Mailbox servers running the Microsoft Exchange Unified Messaging (UM) service. To learn more about UM services, see [UM services](um-services-exchange-2013-help.md).

    > [!WARNING]
    > You must deploy at least one Exchange 2013 Mailbox server in your organization before you configure the VoIP gateways or IP PBXs to send UM SIP and RTP traffic to the Exchange 2013 Client Access servers.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Download and install the required UM language packs

UM language packs enable callers and Outlook Voice Access users to interact with the voice mail system in multiple languages. After you install an additional language on an Exchange 2013 Mailbox server, callers and Outlook Voice Access users can hear email messages and interact with the voice mail system in that language. However, to make the language available for all incoming calls, you must install the required UM language packs on all Exchange 2013 Mailbox servers. This is because every Exchange 2013 Mailbox server can answer incoming calls for Unified Messaging.

By default, when you install an Exchange 2013 Mailbox server, the U.S. English (en-US) language pack is installed. It's the only available language option for your dial plan unless you install another UM language pack. (U. S. English can't be removed unless you remove the Mailbox server from the computer.) After you install a UM language pack on an Exchange 2013 Mailbox server, the language associated with the language pack will be listed as an available option when you configure the default language for the dial plan. By default, because a UM auto attendant is linked to a UM dial plan when the auto attendant is created, it uses the default language setting of the linked UM dial plan. However, this setting can be changed after the UM auto attendant is created.

> [!NOTE]
> If U.S. English is the only language that you want to provide for your dial plan, you can skip this step and go to step 2.

You can add UM language packs by using the setup.exe command or by running the *\<UMLanguagePack\>*.exe installation program after you've downloaded the UM language pack from [Exchange Server 2013 UM Language Packs](https://www.microsoft.com/download/details.aspx?id=35368). For more information, see [Install a UM language pack](install-a-um-language-pack-exchange-2013-help.md).

This example uses setup.exe to install the Japanese (ja-JP) UM language pack.

```powershell
setup.exe /AddUmLanguagePack:ja-JP /s:d:\Exchange\UMLanguagePacks /IAcceptExchangeServerLicenseTerms
```

## Step 2: Move the Exchange 2010 system mailbox used for UM custom greetings, announcements, menus, and prompts to Exchange 2013

Custom greetings, announcements, menus, and prompts are used by Unified Messaging dial plans and auto attendants. The system mailbox named {e0dc1c29-89c3-4034-b678-e6c29d823ed9} is created when you install Exchange 2010 or Exchange 2013 and is used to support features such as Message Approval and Multi-Mailbox Search. This system mailbox is also used to store dial plan and auto attendant custom greetings, announcements, menus, and prompts. If the system mailbox doesn't exist, you can use the **Setup /PrepareAD** command to create it.

By default, system mailboxes aren't visible in the Exchange admin center (EAC). You can get a list of the system mailboxes by running one the following:

This command returns a list of all the system mailboxes.

```powershell
Get-Mailbox -Arbitration
```

This command returns a list of system mailboxes and their individual properties or settings.

```powershell
Get-Mailbox -Arbitration |fl
```

By using this system mailbox, custom greetings, announcements, menus, and prompts can be backed up and restored along with other mailboxes in a database. This reduces the amount of resources that are needed. Storing custom greetings, announcements, menus, and prompts in a system mailbox removes any possible inconsistencies that may have occurred. To learn more about mailbox moves, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md).

## Optional: Manually export and import dial plan and auto attendant custom greetings, announcements, menus, and prompts

There could be situations that the Exchange 2010 system mailbox has been moved and you still need to export and import the custom greetings, announcements, menus, and prompts that are used with UM dial plans and auto attendants from the Exchange 2010 system mailbox to the Exchange 2013 system mailbox. The system mailbox in both versions is named {e0dc1c29-89c3-4034-b678-e6c29d823ed9}.

Custom greetings, announcements, menus, and prompts are audio files (in .wav or .wma format) that are used by UM for the following purposes:

- On UM dial plans, the audio files are used for customized welcome greetings and informational announcements. They're played when Outlook Voice Access users call in to an Outlook Voice Access number.

- On UM auto attendants, the audio files are used for customized non-business and business hours greetings, informational announcements, menu prompts, and navigation menus. They're played when callers call in to a UM auto attendant.

When you're exporting and importing custom greetings, announcements, menus, and prompts from Exchange 2010 to Exchange 2013, you must use the **Export-UMPrompt** and **Import-UMPrompt** cmdlets. You can't use the EAC to export or import custom prompts. On an Exchange 2010 server, use the **Export-UMPrompt** cmdlet to export the Exchange 2010 dial plan and auto attendant prompts. After you've exported the prompts, you can import them to the Exchange 2013 Mailbox server. When you run the **Export-UMPrompt** cmdlet from your Exchange 2010 server, the command performs a GUID or object identifier lookup for the dial plan or auto attendant in Active Directory and queries it to determine if there are any custom greetings, announcements, menus, or prompts. If found, the custom greetings, announcements, menus, or prompts will be saved to the directory that you specify. After you've exported all custom greetings, announcements, menus, and prompts, use the **Import-UMPrompt** cmdlet to import the prompts into your Exchange 2013 system mailbox.

This example exports the welcome greeting for the UM dial plan `MyUMDialPlan` and saves it as the file `welcomegreeting.wav`.

```powershell
$prompt = Export-UMPrompt -PromptFileName "customgreeting.wav" -UMDialPlan MyUMDialPlan
set-content -Path "d:\DialPlanPrompts\welcomegreeting.wav" -Value $prompt.AudioData -Encoding Byte
```

This example imports the welcome greeting `welcomegreeting.wav` from d:\\UMPrompts into the UM dial plan `MyUMDialPlan`.

```powershell
[byte[]]$c = Get-content -Path "d:\UMPrompts\welcomegreeting.wav" -Encoding Byte -ReadCount 0
Import-UMPrompt -UMDialPlan MyUMDialPlan -PromptFileName "welcomegreeting.wav" -PromptFileData $c
```

This example exports a custom greeting for the UM auto attendant `MyUMAutoAttendant` and saves it to the file `welcomegreetingbackup.wav`.

```powershell
Export-UMPrompt -PromptFileName "welcomegreeting.wav" -UMAutoAttendant MyUMAutoAttendant
set-content -Path "e:\UMPromptsBackup\welcomegreeting.wav" -Value $prompt.AudioData -Encoding Byte
```

This example imports the welcome greeting `welcomegreeting.wav` from d:\\UMPrompts into the UM auto attendant `MyUMAutoAttendant`.

```powershell
[byte[]]$c = Get-content -Path "d:\UMPrompts\welcomegreeting.wav" -Encoding Byte -ReadCount 0
Import-UMPrompt -UMAutoAttendant MyUMAutoAttendant -PromptFileName "welcomegreeting.wav" -PromptFileData $c
```

To learn more about custom prompts for UM, see:

- [Import and export custom greetings, announcements, menus, and prompts](import-and-export-custom-greetings-announcements-menus-and-prompts-exchange-2013-help.md)

- [Import-UMPrompt](https://docs.microsoft.com/powershell/module/exchange/Import-UMPrompt)

- [Export-UMPrompt](https://docs.microsoft.com/powershell/module/exchange/Export-UMPrompt)

- [UM languages, prompts, and greetings](um-languages-prompts-and-greetings-exchange-2013-help.md)

## Step 4: Export and import certificates

If you're using SIP secured or Secured dial plans in your Exchange 2010 organization, you'll need to export and import the certificates that were used to your Exchange 2013 Client Access and Mailbox servers. Mutual Transport Layer Security (mutual TLS) is used to encrypt data sent between your Exchange 2013 servers and the VoIP gateways, IP PBXs, and SIP-enabled PBXs. Certificates bind the identity of the certificate owner to a pair of electronic keys (public and private) that are used to encrypt and sign information digitally. You can use one of the following certificates for the UM and UM Call Router services:

- A self-signed (Exchange) certificate

- An internal public key infrastructure (PKI) certificate

- A third-party commercial certificate

By default, when you install Exchange 2013, two self-signed certificates are created: **Microsoft Exchange Server Auth Certificate** and **Microsoft Exchange**. The **Microsoft Exchange** self-signed certificate can be used for UM to encrypt data, but you must assign the certificate to the UM and UM Call Router services. This self-signed certificate can be copied and then imported on the VoIP gateways, IP PBXs, and SIP-enabled PBXs. However, it can't be used when you're integrating UM with Microsoft Lync Server.

To enable UM to encrypt data that's sent between your Exchange 2013 servers and VoIP gateways, IP PBXs, and SIP-enabled PBXs, you need to do the following:

- Use an existing self-signed UM certificate, create a new self-signed Exchange certificate, submit a certificate request to an internal certification authority for a PKI certificate, or purchase a third-party commercial certificate that you can use for mutual TLS between your Exchange 2013 Mailbox and Client Access servers and VoIP gateways, IP PBXs, and SIP-enabled PBXs.

  Create an Exchange self-signed certificate by using the EAC, as follows:

  1. In the EAC, navigate to **Servers** \> **Certificates**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

  2. On the **New Exchange certificate** page, choose **Create a self-signed certificate**, and then select **Next**.

  3. Enter a friendly name for the certificate, and then select **Next**.

  4. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to select the Exchange servers that you want to apply this certificate to, and then select **Next**.

  5. Specify the domains that you want to be included in your certificate, and then select **Next**. If you want to add a domain for a service, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

  6. Verify that the domains you included are correct, and then select **Finish**.

  > [!IMPORTANT]
  > When you use the EAC to create a certificate, you won't be prompted to enable the services for the certificate. After the certificate has been created, you can use the EAC to enable the services. For more information about how to enable a certificate for services, see <A href="assign-a-certificate-to-the-um-and-um-call-router-services-exchange-2013-help.md">Assign a certificate to the UM and UM Call Router services</A>.

  Create an Exchange self-signed certificate by running the following command in the Shell.

  ```powershell
  New-ExchangeCertificate -Services 'UM, UMCallRouter' -DomainName '*.northwindtraders.com' -FriendlyName 'UMSelfSigned' -SubjectName 'C=US,S=WA,L=Redmond,O=Northwindtraders,OU=Servers,CN= Northwindtraders.com' -PrivateKeyExportable $true
  ```

  > [!NOTE]
  > If you specify the services you want to enable by using the <EM>Services</EM> parameter, you will be prompted to enable the services for the certificate you created. In this example, you will be prompted to enable the certificate for the Unified Messaging and Unified Messaging Call Router services. For more information about how to enable a certificate for services, see <A href="assign-a-certificate-to-the-um-and-um-call-router-services-exchange-2013-help.md">Assign a certificate to the UM and UM Call Router services</A>.

- Import the certificate that will be used on all Exchange 2013 Client Access and Mailbox servers in your organization. If you use the Exchange 2013 self-signed certificate, you'll need to copy the certificate, then import it on the VoIP gateways, IP PBXs, or SIP-enabled PBXs. If you use the self-signed certificate from Exchange 2010, the Subject Alternative Name (SAN) must contain the machine names of all the Exchange 2013 servers. If you have Exchange 2010 Unified Messaging servers in your organization, you can use the Exchange 2013 self-signed certificate, but you must add the machine names of the Exchange 2010 UM servers to the SAN in the Exchange 2013 certificate.

- Enable or assign the certificate to be used to the UM and UM Call Router services on the Client Access and Mailbox servers in your organization.

  Enable the UM service and UM Call Router service on all Exchange 2013 servers to use the Exchange self-signed certificate by using the EAC, as follows:

  1. In the EAC, navigate to **Servers** \> **Certificates**, select the certificate you want to enable services on, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

  2. On the **Procedure** page, select **Services**, select **Unified Messaging**, and then select **Unified Messaging call router**.

  Enable an Exchange self-signed certificate by running the following command in the Shell.

  ```powershell
  Enable-ExchangeCertificate -Thumbprint 5113ae0233a72fccb75b1d0198628675333d010e -Services 'UM, UMCallRouter'
  ```

- Configure any new or existing UM dial plans as SIP secured or Secured.

- Configure the UM startup mode to TLS or Dual on the Client Access and Mailbox servers in your organization.

- Create and configure new or existing UM IP gateways with a fully qualified domain name (FQDN).

- Configure the listening port on the UM IP gateways to use TLS port 5061.

- Restart the UM Call Router service on all Exchange 2013 Client Access servers and restart the UM service on all Exchange 2013 Mailbox servers. To learn more about UM services, see [UM services](um-services-exchange-2013-help.md).

## Step 5: Configure the UM startup mode on all Exchange 2013 Client Access servers

If you're using SIP secured or Secured dial plans, you must configure the UM startup mode on your Exchange 2013 Client Access servers. You can specify the UM startup mode for the UM Call Router service on an Exchange 2013 Client Access server by using the EAC or the Exchange Management Shell. By default, the Client Access server will start up in TCP mode, but if you're using Transport Layer Security (TLS) to encrypt Voice over IP (VoIP) traffic, you must configure the Exchange 2013 Client Access server to use TLS or Dual mode. We recommend that all Exchange 2013 Client Access servers be configured to use Dual as the UM startup mode. This is because all Exchange 2013 Client Access servers can answer incoming calls for all UM dial plans, and those dial plans can have different security settings. If you change the UM startup mode, you must restart the UM Call Router service for the change to take effect. To learn more about UM services, see [UM services](um-services-exchange-2013-help.md).

Configure the UM startup mode on an Exchange 2013 Client Access server by using the EAC, as follows:

1. In the EAC, navigate to **Servers** \> **Servers**.

2. In the list view, select the Exchange server that you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the **Exchange Server** page, click **Unified Messaging**.

4. Under **UM Call Router settings** \> **UM startup mode**, select one of the following from the drop-down list:

   - **TCP**: Use this option if you aren't using mTLS and are using only Unsecured dial plans.

   - **TLS**: Use this option if you're using mTLS and are using only SIP Secured or Secured dial plans.

   - **DUAL**: Use this option if you're using mTLS and are using Unsecured, SIP Secured, and Secured dial plans.

5. After you select the UM startup mode, click **Save**.

Configure the UM startup mode on an Exchange 2013 Client Access server by running the following command in the Shell.

```powershell
Set-UMCallRouterSettings -Server MyUMCallRouter.northwindtraders.com -UMStartupMode Dual
```

## Step 6: Configure the UM startup mode on all Exchange 2013 Mailbox servers

If you're using SIP secured or Secured dial plans, you must configure the UM startup mode on your Exchange 2013 Mailbox servers. You can specify the UM startup mode for the UM service on an Exchange 2013 Mailbox server by using the EAC or the Shell. By default, an Exchange 2013 Mailbox server will start up in TCP mode, but if you're using Transport Layer Security (TLS) to encrypt Voice over IP (VoIP) traffic, you must configure the Exchange 2013 Mailbox server to use TLS or Dual mode. We recommend that all Exchange 2013 Mailbox servers be configured to use Dual as the UM startup mode. This is because all Exchange 2013 Mailbox servers can answer incoming calls for all UM dial plans, and those dial plans can have different security settings. If you change the UM startup mode, you must restart the UM service for the change to take effect. To learn more about UM services, see [UM services](um-services-exchange-2013-help.md).

Configure the UM startup mode on an Exchange 2013 Mailbox server by using the EAC, as follows:

1. In the EAC, navigate to **Servers** \> **Servers**.

2. In the list view, select the Exchange server that you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the **Exchange Server** page, click **Unified Messaging**.

4. Under **UM Service settings** \> **UM startup mode**, select one of the following from the drop-down list:

   - **TCP**: Use this option if you aren't using mTLS and are using only Unsecured dial plans.

   - **TLS**: Use this option if you're using mTLS and are using only SIP Secured or Secured dial plans.

   - **DUAL**: Use this option if you're using mTLS and are using Unsecured, SIP Secured, and Secured dial plans.

5. After you select the UM startup mode, click **Save**.

Configure the UM startup mode on an Exchange 2013 Mailbox server by running the following command in the Shell.

```powershell
Set-UMService -Identity MyUMServer -ExternalHostFqdn host.external.contoso.com -IPAddressFamily Any -UMStartupMode Dual
```

## Step 7: Create or configure existing UM dial plans

Depending on your existing Exchange 2010 deployment, you may be required to create new UM dial plans or configure your existing dial plans. A UM dial plan represents a set of traditional or SIP-enabled Private Branch eXchanges (PBXs) or IP PBXs that share common user extension numbers. All users' extensions hosted on traditional or SIP-enabled PBXs or IP PBXs within a dial plan contain the same number of digits. Users can dial one another's telephone extensions without appending a special number to the extension or dialing a full telephone number.

UM dial plans are used in Unified Messaging to make sure that user telephone extensions are unique. In some telephony networks, multiple PBXs or IP PBXs exist. In these telephony networks, there could be two users who have the same telephone extension number. UM dial plans resolve this situation. Putting the two users into two separate UM dial plans makes their extensions unique. For more information, see [UM dial plans](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-dial-plans).

If required, you can create a UM dial plan by using the EAC:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**, and then click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. On the **New UM Dial Plan** page, complete the following boxes:

   - **Name**: Type the name of the dial plan. A UM dial plan name is required and must be unique. However, the name you type is used only for display purposes in the EAC and the Shell. The maximum length of a UM dial plan name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

     Although the box for the name of the dial plan can accept 64 characters, the name of the dial plan can't be longer than 49 characters. If you try to create a dial plan name that contains more than 49 characters, you'll receive an error message. The message will say that the UM mailbox policy couldn't be generated because the UM dial plan name is too long. This happens because, when you create a dial plan a default UM mailbox policy named *\<DialPlanName\>* **Default Policy** is also created. When the 15 characters in the default policy are added to the name of the dial plan, the total characters exceed the limit. The *name* parameter for both the UM dial plan and UM mailbox policy can be 64 characters. However, if the name of the dial plan is longer than 49 characters, the name of the default UM mailbox policy will be longer than 64 characters, and this isn't allowed by the system.

   - **Extension length (digits)**: Enter the number of digits for extension numbers in the dial plan. The number of digits for extension numbers is based on the telephony dial plan created on a PBX. For example, if a user associated with a telephony dial plan dials a four-digit extension to call another user in the same telephony dial plan, you select 4 as the number of digits in the extension.

     This is a required box that has a value range from 1 through 20. The typical extension length is from 3 through 7 numbers. If your existing telephony environment includes extension numbers, you must specify a number of digits that matches the number of digits in those extensions.

     When you create a Telephone extension dial plan, you're required to enter an extension number for the user if they're linked to a Telephone extension dial plan. An extension number is also required with Session Initiation Protocol (SIP) dial plans or E.164 dial plans when a UM-enabled user is linked to a SIP URI or E.164 dial plan. The extension number is used by Outlook Voice Access users when they access their Exchange mailbox.

   - **Dial plan type**: A Uniform Resource Identifier (URI) is a string of characters that identifies or names a resource. The main purpose of this identification is to enable VoIP devices to communicate with other devices over a network using specific protocols. URIs are defined in schemes that define a specific syntax and format and the protocols for the call. In simple terms, this format is passed from the IP PBX or PBX. After you create a UM dial plan, you won't be able to change the URI type without deleting the dial plan, and then re-creating the dial plan to include the correct URI type. You can select one of the following URI types for the dial plan:

     - **Telephone extension**: This is the most common URI type. The calling and called party information from the VoIP gateway or IP Private Branch eXchange (PBX) is listed in one of the following formats: Tel:512345 or 512345@\<*IP address*\>. This is the default URI type for dial plans.

     - **SIP URI**: Use this URI type if you must have a Session Initiation Protocol (SIP) URI dial plan such as an IP PBX that supports SIP routing or if you're integrating Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server and Unified Messaging. The calling and called party information from the VoIP gateway. IP PBX, or Communications Server 2007 R2 or Lync Server is listed as a SIP address in the following format: sip:\<*username*\>@\<*domain* or *IP address*\>:Port.

     - **E.164**: E.164 is an international numbering plan for public telephone systems in which each assigned number contains a country code, a national destination code, and a subscriber number. The calling and called party information sent from the VoIP gateway or IP PBX is listed in the following format: Tel:+14255550123.

     > [!WARNING]
     > After you create a dial plan, you will be unable to change the URI type without deleting the dial plan, and then re-creating the dial plan to include the correct URI type.

   - **VoIP security mode**: Use this drop-down list to select the VoIP security setting for the UM dial plan. You can select one of the following security settings for the dial plan:

     - **Unsecured**: By default, when you create a UM dial plan, it is set to not encrypt the SIP signaling or RTP traffic. In unsecured mode, the Client Access and Mailbox servers associated the UM dial plan send and receive data from VoIP gateways, IP PBXs, SBCs and other Client Access and Mailbox servers using no encryption. In unsecured mode, neither the Realtime Transport Protocol (RTP) media channel nor the SIP signaling information is encrypted.

     - **SIP secured**  When you select **SIP secured**, only the SIP signaling traffic is encrypted, and the RTP media channels still use TCP, which isn't encrypted. With SIP secured, Mutual Transport Layer Security (TLS) is used to encrypt the SIP signaling traffic and VoIP data.

     - **Secured**: When you select **Secured**, both the SIP signaling traffic and the RTP media channels are encrypted. Both the secure signaling media channel that uses Secure Realtime Transport Protocol (SRTP) and the SIP signaling traffic use mutual TLS to encrypt the VoIP data.

   - **Country/Region code**: Use this box to type the country/region code number to be used for outgoing calls. This number will automatically be prepended to the telephone number that's dialed. This box accepts from 1 through 4 digits. For example, in the United States, the country/region code is 1. In the United Kingdom, it's 44.

3. Click **Save**.

If required, you can create a UM dial plan by running the following command in the Shell.

```powershell
New-UMDialplan -Name MyUMDialPlan -URIType E164 -NumberOfDigitsInExtension 5 -VoIPSecurity Secured
```

If required, you can configure an existing UM dial plan by using the EAC, as follows:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to view or modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the **UM Dial Plan** page, click **Configure**. Use the configuration options to view specific dial plan settings and to enable or disable features.

If required, you can configure an existing UM dial plan by using the Shell:

```powershell
Set-UMDialplan -Identity MyDialPlan -AccessTelephoneNumbers 4255551234 -AudioCodec Wma -CallAnsweringRulesEnabled $false -OutsideLineAccessCode 9 -VoIPSecurity SIPSecured
```

When you deployed Exchange 2010 Unified Messaging, you were required to add a Unified Messaging server to a UM dial plan for it to answer incoming calls. This is no longer required. In Exchange 2013, Client Access and Mailbox servers can't be linked with a Telephone extension or E.164 dial plan, but must be linked to SIP URI dial plans. Client Access and Mailbox servers will answer all incoming calls for all types of dial plans.

## Step 8: Create or configure existing UM IP gateways

Depending on your existing Exchange 2010 deployment, you may be required to create new UM IP gateways or configure your existing ones. If you're using SIP secured or Secured dial plans, you must create a UM IP gateway with an FQDN and use the Shell to configure it to listen on port 5061. For existing UM IP gateways, verify that they're configured with an FQDN and are listening on port 5061. If the UM IP gateway doesn't use an FQDN, use the EAC or the Shell to change the address. If the UM IP gateway doesn't use port 5061, use the Shell to change the port. You can view the settings of a UM IP gateway by using the **Get-UMIPGateway** cmdlet.

A UM IP gateway represents a physical Voice over IP (VoIP) gateway, IP PBX, or SIP-enabled PBX. Before a VoIP gateway, IP PBX, or SIP-enabled PBX can be used to answer incoming calls and send outgoing calls for voice mail users, a UM IP gateway must be created in the directory service.

The combination of the UM IP gateway and a UM hunt group establishes a link between a VoIP gateway, IP PBX, or SIP-enabled PBX and a UM dial plan. By creating multiple UM hunt groups, you can associate a single UM IP gateway with multiple UM dial plans. For more information, see [UM IP gateways](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateways).

If required, you can create a UM IP gateway by using the EAC, as follows:

1. In the EAC, navigate to **Unified Messaging** \> **UM IP gateways**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. On the **New UM IP Gateway** page, enter the following information:

      - **Name**: Use this box to specify a unique name for the UM IP gateway. This is a display name that appears in the EAC. If you have to change the display name of the UM IP gateway after it's been created, you must first delete the existing UM IP gateway, and then create another UM IP gateway that has the appropriate name. The UM IP gateway name is required, but it's used for display purposes only. Because your organization may use multiple UM IP gateways, we recommend that you use meaningful names for your UM IP gateways. The maximum length of a UM IP gateway name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

      - **Address**: You can configure a UM IP gateway with either an IPv4 or IPv6 address or an FQDN. Use this box to specify the IP address or FQDN configured on the VoIP gateway, IP PBX, or SIP-enabled PBX. This box accepts only FQDNs that are valid and formatted correctly.

        You can enter alphabetical and numeric characters. IPv4 addresses, IPv6 addresses, and FQDNs are supported. If you want to use mutual TLS between a UM IP gateway and a dial plan operating in either SIP secured or Secured mode, you must configure the UM IP gateway with an FQDN. You must also configure it to listen on port 5061 and verify that any VoIP gateways or IP PBXs have also been configured to listen for mutual TLS requests on port 5061. To configure a UM IP gateway, run the following command in the Shell: `Set-UMIPGateway -identity MyUMIPGateway -Port 5061`

        If you use an FQDN, you must also make sure that you've correctly configured a DNS host record for the VoIP gateway, IP PBX, or SIP-enabled PBX so that the host name will be correctly resolved to an IP address. Also, if you use an FQDN instead of an IP address, and the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that configuration information for the UM IP gateway is updated correctly.

      - **UM Dial Plan**: Click **Browse** to select the UM dial plan that you want to associate with the UM IP gateway. When you select a UM dial plan to associate with a UM IP gateway, a default UM hunt group is also created and associated with the UM dial plan that you selected. If you don't select a UM dial plan, you must manually create a UM hunt group and then associate that UM hunt group with a UM IP gateway that you have created.

3. Click **Save**.

If required, you can create a UM IP gateway by run the following command.

```powershell
New-UMIPGateway -Identity MyUMIPGateway -Address "MyUMIPGateway.contoso.com"
```

To configure an existing UM IP gateway by using the EAC:

1. In the EAC, navigate to **Unified Messaging** \> **UM IP gateways**, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM IP gateway** page, click **Configure**. Use the configure options to view specific UM IP gateway settings and to enable or disable features.

To configure an existing UM IP gateway in the Shell, running the following command in the Shell.

```powershell
Set-UMIPGateway -Identity MyUMIPGateway -Address fe80::39bd:88f7:6969:d223%11 -IPAddressFamily Any -Status Disabled -OutcallsAllowed $false
```

## Step 9: Create a UM hunt group

Depending on your existing Exchange 2010 deployment, you may be required to create new UM hunt groups. A telephony hunt group provides a way to distribute telephone calls from a single number to multiple extensions or telephone numbers. In Unified Messaging, a UM hunt group is a logical representation of a telephony hunt group, and it links a UM IP gateway to a UM dial plan.

You need to have at least one UM hunt group for every IP PBX or PBX hunt group. When you complete the following procedure, one UM hunt group is created by default. If you have more than one IP PBX or PBX hunt group, you need to create additional UM hunt groups. To learn more about UM hunt groups, see [UM hunt groups](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-hunt-groups).

If required, you can To create a UM hunt group by using the EAC, as follows:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM Dial Plan** page, under **UM Hunt Groups**, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. On the **New UM Hunt Group** page, enter the following information:

      - **Name**: Use this box to create the display name for the UM hunt group. A UM hunt group name is required and must be unique, but it's used only for display purposes in the EAC and the Shell. If you have to change the display name of the hunt group after it's been created, you must first delete the existing hunt group and then create another hunt group that has the appropriate name. If your organization uses multiple hunt groups, we recommend that you use meaningful names for your hunt groups. The maximum length of a UM hunt group name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

      - **UM IP Gateway**: Use this box to select a UM IP gateway. This box shows the name of the UM IP gateway that will be linked with the UM hunt group. To link a UM IP gateway to the UM hunt group, click **Browse**.

      - **Pilot Identifier**: Use this box to specify a string that uniquely identifies the pilot identifier configured on the PBX or IP PBX. An extension number or a SIP Uniform Resource Identifier (URI) can be used in this box. Alphanumeric characters are accepted in this box. For legacy PBXs, a numeric value is used as a pilot identifier. However, some IP PBXs and SIP-enabled PBXs can use SIP URIs.

4. Click **Save**.

If required, you can create a UM hunt group by running the following command in the Shell.

```powershell
New-UMHuntGroup -Name MyUMHuntGroup -PilotIdentifier 5551234,55555 -UMDialPlan MyUMDialPlan -UMIPGateway MyUMIPGateway
```

> [!TIP]
> You can't configure or change settings for a UM hunt group. If you want to change the configuration settings for a UM hunt group, you must delete it and add a new UM hunt group with the correct settings.

## Step 10: Create or configure UM auto attendants

Depending on your existing Exchange 2010 deployment, you may be required to create new UM auto attendants. You can use UM auto attendants to create a voice menu system that lets external and internal callers use the UM auto attendant menu system to locate people and place or transfer calls to company users or departments in an organization. For more information, see [Automatically answer and route incoming calls](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/automatically-answer-and-route-calls).

In smaller deployments you may only want to deploy UM so that callers can leave voice mail for users. In these deployments, creating an auto attendant isn't required. However, in most cases, using auto attendants is very useful for external callers when they call in to your organization.

If required, you can create a UM auto attendant by using the EAC, as follows:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. Select the UM dial plan for which you want to add an auto attendant, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM dial plan** page, under **UM Auto Attendants**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. On the **New UM Auto Attendant** page, complete the following boxes:

      - **Name**: Use this box to create the display name for the UM auto attendant. A UM auto attendant name is required and must be unique. However, it's used only for display purposes in the EAC and the Shell. If you have to change the display name of the auto attendant after it's created, you must first delete the existing UM auto attendant and then create another auto attendant that has the appropriate name. If your organization uses multiple UM auto attendants, we recommend that you use meaningful names for your UM auto attendants. The maximum length of a UM auto attendant name is 64 characters, and it can include spaces.

        Although you can name a new UM auto attendant to include spaces, if you integrate Unified Messaging with Lync Server, the name of the auto attendant can't include spaces. Therefore, if you created an auto attendant that has spaces in the display name and you're integrating with Lync Server, you must first delete that auto attendant and then create another auto attendant that doesn't include spaces in the display name.

      - **Create this auto attendant as enabled**: Select this check box to enable the auto attendant to answer incoming calls when you finish creating the UM auto attendant. By default, a new auto attendant is created as disabled. If you decide to create the UM auto attendant as disabled, you can use the EAC or the Shell to enable the auto attendant after you finish creating it.

      - **Set the auto attendant to respond to voice commands**: Select this check box to speech-enable the UM auto attendant. If the auto attendant is speech-enabled, callers can respond to the system or custom prompts used by the UM auto attendant using touchtone or voice inputs. By default, the auto attendant isn't speech-enabled when it's created. For callers to use a speech-enabled auto attendant in a language other than U.S. English (en-US), you must install the appropriate UM language pack and configure the properties of the auto attendant to use this language. The en-US UM language pack is installed by default when you install an Exchange 2013 Mailbox server.

      - **Access numbers**: Enter the extension or telephone numbers that callers will use to reach the auto attendant. Type an extension number or telephone number in the box, and then click **Add ** to add the number to the list. The number of digits in the extension number or telephone number that you provide doesn't have to match the number of digits for an extension number configured on the associated UM dial plan. This is because direct calls are allowed to UM auto attendants.

        The number of extension or access numbers you can enter is unlimited. However, you may create a new auto attendant without listing an extension number or telephone number. An extension number or telephone number isn't required. You can edit or remove an existing extension number or pilot identifier. To edit an existing extension number or telephone number, click **Edit**. To remove an existing extension number or telephone number from the list, click **Remove**.

4. Click **Save**.

If required, you can create a UM auto attendant by running the following command in the Shell.

```powershell
New-UMAutoAttendant -Name MyUMAutoAttendant -UMDialPlan MyUMDialPlan -PilotIdentifierList 56000,56100 -SpeechEnabled $true -Status Enabled
```

If required, you can configure an existing auto attendant by using the EAC, as follows:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM dial plan** page, under **UM Auto Attendants**, select the UM auto attendant that you want to view or configure, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"). Use the configuration options to view specific auto attendant settings and to enable or disable features.

If required, you can configure an existing auto attendant by running the following command in the Shell.

```powershell
Set-UMAutoAttendant -Identity MySpeechEnabledAA -DTMFFallbackAutoAttendant MyDTMFAA -OperatorExtension 50100 -AfterHoursTransferToOperatorEnabled $true -StaroutToDialPlanEnabled $true
```

## Step 11: Create or configure UM mailbox policies

Depending on your existing Exchange 2010 deployment, you may be required to create new UM mailbox policies or configure existing UM mailbox policies. UM mailbox policies are required when you enable users for Unified Messaging. The mailbox of each UM-enabled user must be linked to a single UM mailbox policy. After you create a UM mailbox policy, you link one or more UM-enabled mailboxes to the UM mailbox policy. This lets you control PIN security settings such as the minimum number of digits in a PIN or the maximum number of logon attempts for the UM-enabled users who are linked to the UM mailbox policy. For more information, see [UM mailbox policies](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policies).

If required, you can create a UM mailbox policy by using the EAC:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan that you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM dial plan** page, under **UM Mailbox Policies**, click **Add** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the **New UM Mailbox Policy** page, in the **Name** box, enter the name of the new UM mailbox policy.

    > [!NOTE]
    > Use this box to specify a unique name for the UM mailbox policy. This is a display name that appears in the EAC. If you must change the display name of the UM mailbox policy after it's been created, you must first delete the existing UM mailbox policy, and then create another UM mailbox policy that has the appropriate name. You can't delete a UM mailbox policy if any UM-enabled users are associated with it.The UM mailbox policy name is required, but it's used for display purposes only. Because your organization may use multiple UM mailbox policies, we recommend that you use meaningful names for your UM mailbox policies. The maximum length of a UM mailbox policy name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \ [ ] : ; | = , + * ? &lt; &gt;.

4. Click **Save**.

    > [!NOTE]
    > When you save the UM mailbox policy, all the default settings including PIN policies, voice mail features, and Protected Voice Mail settings are enabled. If you want to customize or change any default settings for the UM mailbox policy you just created, use the <STRONG>Set-UMMailbox</STRONG> cmdlet or the EAC.

If required, you can create a UM mailbox policy by running for following command in the Shell.

```powershell
New-UMMailboxPolicy -Name MyUMMailboxPolicy -UMDialPlan MyUMDialPlan
```

If required, you can configure an existing UM mailbox policy by using the EAC:

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM dial plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to view or configure, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"). Use the configuration options to view specific UM mailbox policy settings and to enable or disable features.

If required, you can configure an existing UM mailbox policy by running the following command in the Shell.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 8 -MaxLogonAttempts 12 -MinPINLength 8 -PINHistoryCount 10 -PINLifetime 60 -ResetPINText "The PIN used to allow you access to your mailbox using Outlook Voice Access has been reset."
```

## Step 12: Move existing UM-enabled mailboxes to Exchange 2013

In Exchange 2010 Unified Messaging, after you've enabled users within the organization to use voice mail, a default set of UM properties is applied to the user so they can use UM features. Learn more at [Voice mail for users](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/voice-mail-for-users).

During the process of upgrading, there will be a period of time during which you'll have mailboxes that are UM enabled both on Exchange 2010 Mailbox servers and on Exchange 2013 Mailbox servers. However, if you're moving all UM-enabled users over to Exchange 2013 Mailbox servers, you must use the EAC or the **New-MoveRequest** cmdlet in the Shell from an Exchange 2013 server to retain all of the properties and settings, including the user's PIN.

A move request is the process of moving a mailbox from one mailbox database to another. A local move request is a mailbox move that occurs within a single forest. For more information about mailbox moves, see:

- [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md)

- [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest)

- [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch)

- [Managing Move Requests](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124797(v=exchg.141))

To move an Exchange 2010 mailbox to an Exchange 2013 Mailbox server by using the EAC:

1. In the EAC, click **Recipients** \> **Migration**, and then click **Add** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. In the **New local mailbox move** wizard, select the user you want to move, click **OK**, and then click **Next**.

3. On the **Move configuration** page, specify a name for the new batch. Select which options you want for the archive mailbox and the mailbox database location, and then click **New**.

To move an Exchange 2010 mailbox to an Exchange 2013 Mailbox server by using the Shell, run the following command.

```powershell
New-MoveRequest -Identity 'tony@alpineskihouse.com' -TargetDatabase "DB01"
```

## Step 13: Enable new users for UM or configure settings for an existing UM-enabled user

A user must have a mailbox before they can be enabled for Unified Messaging. But, by default, a user who has a mailbox isn't enabled for UM. After the user is UM-enabled, you can manage, modify, and configure the UM properties and voice mail features for them. You can enable a user for UM by using the EAC or the Shell. Learn more at [Voice mail for users](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/voice-mail-for-users).

When you enable a user for UM, you must define at least one extension number that UM will use when voice mail is submitted to the user's mailbox and to allow the user to use Outlook Voice Access. After you enable the user for UM, you can add secondary extension numbers to the user's mailbox, modify or remove them by configuring the Exchange Unified Messaging (EUM) proxy address on the user's mailbox, or add or remove additional or secondary extensions for the user in the EAC. To add, modify, or remove extension numbers, E.164 numbers, or SIP addresses, see [Voice mail-enabled user procedures](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/voice-mail-enabled-user-procedures).

To enable a user for Unified Messaging by using the EAC:

1. In the EAC, click **Recipients**.

2. In the list view, select the user whose mailbox you want to enable for Unified Messaging.

3. In the Details pane, under **Phone and Voice Features**, click **Enable**.

4. On the **Enable UM mailbox** page, click the **Browse** button next to **UM mailbox policy**, locate the UM mailbox policy to assign the user from the list, and then click **OK**.

5. On the **Enable UM mailbox** page, complete the following boxes:

      - **SIP address or E.164 number**: Enter the SIP address or E.164 number for the user. These options are available if the user that you enable for Unified Messaging is assigned to a UM mailbox policy that's linked to either a SIP URI or an E.164 dial plan. Adding a SIP address or E.164 number for a user isn't available if the user is associated with a telephone extension dial plan. When you assign the user to a UM mailbox policy that's linked to a SIP URI or E.164 dial plan, you must still also enter an extension number for the user. This extension number is used when users access their mailbox using Outlook Voice Access. The number of digits that you configure in this box must match the number of digits configured on the SIP URI or E.164 dial plan.

      - **Extension number**: Enter the extension number for the user you're enabling for UM.

        You must provide a valid extension number for the user, and it must match the number of digits specified on the dial plan. You can only enter numeric characters or digits from 1 through 20. The typical extension number is 3 to 7 digits long. The number of digits in the extension is set on the dial plan that's linked to the UM mailbox policy that's assigned to the user.

      - Under **PIN settings**, complete the following:

          - **Automatically generate PIN**: Click this button to automatically generate a PIN for the UM-enabled user to use for voice mail access via Outlook Voice Access. This is the default setting. When you click this button, a PIN is automatically generated based on the PIN policies configured on the UM mailbox policy assigned to the user. We recommend that you use this setting to help protect the user's PIN. The PIN is sent to the user in the welcome message they receive after they're enabled for UM. By default, they'll have to change this PIN when they first sign in to their mailbox to get their voice mail.

          - **Type a PIN**: Click this button to manually specify a PIN that the user will use to access the voice mail system.The PIN must comply with the PIN policy settings configured on the UM mailbox policy associated with this UM-enabled user. For example, if the UM mailbox policy is configured to accept only PINs that contain seven or more digits, the PIN you enter in this box must be at least seven digits long.

          - **Require the user to reset their PIN the first time they sign in**: Select this check box to force the user to reset their voice mail PIN when they access the voice mail system from a telephone using Outlook Voice Access for the first time. They will be prompted to enter a PIN that's more familiar to them.It's a security best practice to force UM-enabled users to change their PIN when they first sign in to help protect against unauthorized access to their data and Inbox. This check box is selected by default.

6. On the **Enable UM mailbox** page, review your settings. Click **Finish** to enable the user for Unified Messaging. Click **Back** to make configuration changes.

To enable a user for Unified Messaging by using the Shell, run the following command.

```powershell
Enable-UMMailbox -Identity tonysmith@contoso.com -UMMailboxPolicy MyUMMailboxPolicy -Extensions 51234 -PIN 5643892 -NotifyEmail administrator@contoso.com -PINExpired $true
```

If required, you can configure a user that's been enabled for UM by using the EAC:

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox for which you want to change the UM mailbox policy.

3. In the details pane, under **Phone and Voice Features** \> **Unified Messaging**, click **View details**.

4. On the **UM Mailbox** page, click **UM mailbox settings** to view or change the following UM properties for an existing UM-enabled user:

      - **PIN Status**: This display-only box shows the status of the user's mailbox. By default, when a user is UM-enabled, the PIN status is listed as **Not locked out**. However, if the user has input an incorrect Outlook Voice Access PIN multiple times, the status is listed as **Locked Out**.

      - **UM mailbox policy**: This box shows the name of the UM mailbox policy associated with the UM-enabled user. You can click **Browse** to locate and specify the UM mailbox policy to be associated with this UM mailbox.

      - **Personal operator extension**: Use this box to specify the operator extension number for the user. By default, an extension number isn't configured. The length of the extension number can be from 1 through 20 characters. This enables incoming calls for the UM-enabled user to be forwarded to the extension number that you specify in this box.

        You can configure other types of operator extension numbers on dial plans and auto attendants. However, those extensions are generally meant for company-wide receptionists or operators. The personal operator extension setting could be used when an administrative assistant or personal assistant answers incoming calls for a specific user.

5. On the **UM Mailbox** page, under **Other extensions**, you can add, change, and view extension numbers for the user.

      - To add an extension number, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). On the **Add another extension** page, use **Browse** to select the UM dial plan, and then enter the extension number in the **Extension number** box.

      - To remove an extension number, select the extension number you want to remove, and then click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon").

6. If you make any changes, click **Save**.

If required, you can configure a user that's been enabled for UM in the Shell by running the following command.

```powershell
Set-UMMailbox -Identity tony@contoso.com -CallAnsweringAudioCodec Wma -CallAnsweringRulesEnabled $false -FaxEnabled $false -UMSMSNotificationOption VoiceMail
```

## Step 14: Configure your VoIP gateways, IP PBXs, and SIP-enabled PBXs to send all incoming calls to the Exchange 2013 Client Access servers

When Exchange 2013 Client Access and Mailbox servers are installed, they're automatically enabled so they can answer incoming and outgoing voice calls and route voice mail messages to the intended recipients. When you're installing your Exchange 2013 Client Access and Mailbox servers and deploying Unified Messaging, you don't have to link or add Exchange 2013 Client Access or Mailbox servers to UM dial plans. Exchange 2013 Client Access and Mailbox servers answer all incoming calls and then use the UM dial plans to locate users.

The Exchange 2013 Client Access server is the entry point for any inbound calls or Session Initiation Protocol (SIP) requests for Unified Messaging. The service that handles the SIP requests on an Exchange 2013 Client Access server is the UM Call Router service and it runs on each Exchange 2013 Client Access server in your organization.

When you're upgrading to Exchange 2013 UM, you should have already installed and configured single or multiple VoIP gateways to connect to the PBXs in your telephony network, or installed and configured Session Initiation Protocol (SIP)-enabled PBXs or IP PBXs.

The last step in the process of upgrading to Exchange 2013 UM is to configure the VoIP gateways, IP PBXs, or SIP-enabled PBXs to send incoming calls (including callers who want to leave voice mail for a user, calls from UM-enabled users calling in to Outlook Voice Access, and calls from callers that dial in to a UM auto attendant) to your Exchange 2013 Client Access servers. All these calls are received first by a VoIP gateway, IP PBX, or SIP-enabled PBX and then forwarded on to the Exchange 2013 Client Access servers in your Exchange 2013 organization. For more information, see the following resources:

- [UM services](um-services-exchange-2013-help.md)

- [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-voip-gateways)

- [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013)

## Step 15: Disable call answering on an Exchange 2010 Unified Messaging server

You can disable call answering by disabling UM on an Exchange 2010 Unified Messaging server or by removing the UM server from a dial plan. When you disable UM, you prevent the UM server from answering incoming calls. You can choose to disconnect all calls immediately or wait for existing calls to be processed before disabling the Unified Messaging server. You'll want to disable call answering before removing the server from a dial plan so that it will finish processing any incoming calls.

To disable Unified Messaging on an Exchange 2010 UM server by using the Exchange Management Console:

1. In the console tree of the EMC, navigate to **Server Configuration** \> **Unified Messaging**.

2. In the result pane, select the Unified Messaging server to disable.

3. In the action pane, click one of the following:

      - When you select the **Disable Immediately** option, the Unified Messaging server disconnects all calls connected to the Unified Messaging server.

      - When you select the **Disable After Completing Calls** option, the Unified Messaging server won't accept new calls and won't be disabled until all calls have been processed.

4. In the confirmation dialog box, click **Yes** to continue.

To disable Unified Messaging on an Exchange 2010 UM server by using the Shell, run the following command.:

```powershell
Disable-UMServer -Identity MyUMServer -Immediate $true
```

> [!TIP]
> You can use the <STRONG>Disable-UMServer</STRONG> cmdlet from an Exchange 2010 UM server or the <STRONG>Disable-UMService</STRONG> cmdlet from an Exchange 2013 Mailbox server to disable call answering.

## Step 16: Remove an Exchange 2010 Unified Messaging server from a dial plan

To process calls, an Exchange 2010 UM server must be added to at least one UM dial plan. A UM server can be added to multiple UM dial plans. You can remove an Exchange 2010 UM server from a UM dial plan. When you remove a UM server from a dial plan, the UM server will no longer answer calls or process UM calls for UM-enabled users.

To remove an Exchange 2010 UM server from a dial plan by using the Exchange Management Console:

1. In the console tree of the EMC, navigate to **Server Configuration** \> **Unified Messaging**.

2. In the result pane, select the Unified Messaging server.

3. In the action pane, click **Properties**.

4. On the **UM Settings** tab, in the **Associated Dial Plans** section, click **Remove**.

5. In the confirmation dialog box, click **Yes** to confirm the deletion of the Exchange 2010 server from the UM dial plan.

6. Click **OK** to close the properties window.

To remove an Exchange 2010 UM server from a dial plan using the Shell, run the following command.

```powershell
$dp= Get-UMDialPlan "MySIPDialPlan"
$s=Get-UMServer -id MyUMServer
$s.dialplans-=$dp.identity
Set-UMServer -id MyUMServer -dialplans:$s.dialplans
```

In this example, there are three SIP URI dial plans: SipDP1, SipDP2 and SipDP3. This example removes the UM server named `MyUMServer` from the SipDP3 dial plan.

```powershell
Set-UMServer -id MyUMServer -DialPlans SipDP1,SipDP2
```

In this example, there are two SIP URI dial plans: SipDP1 and SipDP2. This example removes the UM server named `MyUMServer` from the SipDP2 dial plan.

```powershell
Set-UMServer -id MyUMServer -DialPlans SipDP1
```

> [!TIP]
> You can use the <STRONG>Set-UMServer</STRONG> cmdlet in the Shell on an Exchange 2010 Unified Messaging server or the <STRONG>Set-UMService</STRONG> cmdlet on an Exchange 2013 Mailbox server to remove an Exchange 2010 UM server from a single or multiple dial plans. For example, to remove a UM server from all dial plans, run the following command: <CODE>Set-UMServer -identity MyUMServer -DialPlans $null</CODE>

## How do you know this worked?

After you've set up Unified Messaging, verify the following to ensure it's working correctly:

- A user you've enabled for voice mail can sign in to Outlook Web App or Outlook and see a Welcome Message for Unified Messaging.

- UM users can receive voice messages.

- UM users can call in to an Outlook Voice Access number to listen to email, calendar items, and voice mail.

- UM is routing calls from outside of your organization, and you can place a call.
