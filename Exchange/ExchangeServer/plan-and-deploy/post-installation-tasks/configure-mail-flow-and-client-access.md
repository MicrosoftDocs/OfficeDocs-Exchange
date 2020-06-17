---
localization_priority: Critical
description: 'Summary: How to set up mail flow and client access in Exchange 2016 and Exchange 2019.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4acc7f2a-93ce-468c-9ace-d5f7eecbd8d4
ms.reviewer:
title: Configure mail flow and client access on Exchange servers
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure mail flow and client access on Exchange servers

After you've installed Exchange Server 2016 or Exchange 2019 in your organization, you need to configure Exchange for mail flow and client access. Without these additional steps, you won't be able to send mail to the internet and external clients (for example, Microsoft Outlook, and Exchange ActiveSync devices) won't be able to connect to your Exchange organization.

The steps in this topic assume a basic Exchange deployment with a single Active Directory site and a single simple mail transport protocol (SMTP) namespace.

> [!IMPORTANT]
> This topic uses example values such as Mailbox01, contoso.com, mail.contoso.com, and 172.16.10.11. Replace the example values with the server names, FQDNs, and IP addresses for your organization.

For additional management tasks related to mail flow and clients and devices, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md) and [Clients and mobile](../../clients/clients.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 50 minutes

- You might receive certificate warnings when you connect to the Exchange admin center (EAC) website until you configure a secure sockets layer (SSL) certificate on the Mailbox server. You'll be shown how to do this later in this topic.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create an internet Send connector

Before you can send mail to the internet, you need to create a Send connector on the Mailbox server. For instructions, see [Create a Send connector in Exchange Server to send mail to the internet](../../mail-flow/connectors/internet-mail-send-connectors.md).

> [!NOTE]
> By default, a Receive connector named "Default Frontend \<ServerName\>_" is created when Exchange is installed. This Receive connector accepts anonymous SMTP connections from external servers. You don't need to do any additional configuration if this is the functionality you want. If you want to restrict inbound connections from external servers, modify the **Default Frontend \<Mailbox server\>** Receive connector on the Mailbox server. For more information, see [Default Receive connectors created during setup](../../mail-flow/connectors/receive-connectors.md#default-receive-connectors-created-during-setup).

## Step 2: Add additional accepted domains

By default, Exchange uses the Active Directory domain where Setup /PrepareAD was run for email addresses. If you want recipients to receive and send messages to and from another domain, you need to add the domain as an accepted domain. For instructions, see [Create accepted domains](../../mail-flow/accepted-domains/accepted-domain-procedures.md#create-accepted-domains) and [Configure Exchange to accept mail for multiple authoritative domains](../../mail-flow/accepted-domains/accepted-domain-procedures.md#configure-exchange-to-accept-mail-for-multiple-authoritative-domains).

> [!IMPORTANT]
> To receive email from the internet for a domain, you need an MX resource record in your public DNS for that domain. Each MX record should resolve to the internet-facing server that receives email for your organization.

## Step 3: Configure the default email address policy

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Email address policies" entry in the [Email address and address book permissions](../../permissions/feature-permissions/address-book-permissions.md) topic.

If you added an accepted domain in the previous step and you want that domain to be added to every recipient in the organization, you need to update the default email address policy. For instructions, see [Modify email address policies](../../email-addresses-and-address-books/email-address-policies/eap-procedures.md#modify-email-address-policies) and [Apply email address policies to recipients](../../email-addresses-and-address-books/email-address-policies/eap-procedures.md#apply-email-address-policies-to-recipients).

> [!NOTE]
> We recommend that you configure a user principal name (UPN) that matches the primary email address of each user. If you don't provide a UPN that matches the email address of a user, the user will be required to manually provide their domain\username or UPN in addition to their email address. If their UPN matches their email address, Outlook on the web (formerly known as Outlook on the web), ActiveSync, and Outlook will automatically match their email address to their UPN.

## Step 4: Configure external URLs

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the " _\<Service\>_ virtual directory settings" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

Before clients can connect to your new server from the internet, you need to configure the external domains (or URLs) on the virtual directories in the Client Access (frontend) services on the Mailbox server and then in your public DNS records. The steps below configure the same external domain on the external URL of each virtual directory. If you want to configure different external domains on one or more virtual directory external URLs, you need to configure the external URLs manually. For more information, see [Default settings for Exchange virtual directories](../../clients/default-virtual-directory-settings.md).

1. Open the EAC and go to **Servers** \> **Servers**, select your internet-facing Mailbox server that your clients will connect to, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. In the Exchange server properties window that opens, select the **Outlook Anywhere** tab, configure the following settings:

   **Specify the external host name...**: Enter the externally accessible FQDN that your external clients will use to connect to their mailboxes (for example, mail.contoso.com).

   **Specify the internal host name...**: Enter the internally accessible FQDN (for example, mail.contoso.com).

   When you're finished, click **Save**.

3. Go to **Servers** \> **Virtual directories** and then select **Configure external access domain** ![Configure icon](../../media/ITPro_EAC_ConfigureIcon.png).

4. In the **Configure external access domain** window opens, configure the following settings:

   1. **Select the Mailbox servers to use with the external URL**: Click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png)

   2. In the **Select a server** dialog that opens, select the Mailbox server you want to configure and then click **Add**. After you've added all of the Mailbox servers that you want to configure, click **OK**.

   3. **Enter the domain name you will use with your external Mailbox servers**: Enter the external domain that you want to apply (for example, mail.contoso.com). When you're finished, click **Save**.

Some organizations use a unique Outlook on the web FQDN to protect against future changes to the underlying server FQDN. Many organizations use owa.contoso.com for their Outlook on the web FQDN instead of mail.contoso.com. If you want to configure a unique Outlook on the web FQDN, do the following steps. This checklist assumes you have configured a unique Outlook on the web FQDN.

1. Back at **Servers** \> **Virtual directories**, select **owa (Default Web Site)** on the server that you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. The **owa (Default web site)** window opens. On the **General** tab in the **External URL** field, enter the following information:

   - https://

   - The unique Outlook on the web FQDN you want to use (for example, owa.contoso.com), and then append /owa. For example, https://owa.contoso.com/owa.

   - /owa

   In this example, the final value would be https://owa.contoso.com/owa.

    When you're finished, click **Save**.

3. Back at **Servers** \> **Virtual directories**, select **ecp (Default Web Site)** on the server that you want to configure, and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

4. In the **ecp (Default web site)** window that opens, enter the same URL from the previous step, but append the value /ecp instead of /owa (for example, https://owa.contoso.com/ecp). When you're finished, click **Save**.

After you've configured the external URL in the Client Access services virtual directories on the Mailbox server, you need to configure your public DNS records for Autodiscover, Outlook on the web, and mail flow. The public DNS records should point to the external IP address or FQDN of your internet-facing Mailbox server and use the externally accessible FQDNs that you've configured on your Mailbox server. The recommended DNS records that you should create to enable mail flow and external client connectivity are described in the following table:

|**FQDN**|**DNS record type**|**Value**|
|:-----|:-----|:-----|
|Contoso.com|MX|Mail.contoso.com|
|Mail.contoso.com|A|172.16.10.11|
|Owa.contoso.com|CNAME|Mail.contoso.com|
|Autodiscover.contoso.com|CNAME|Mail.contoso.com|

### How do you know this step worked?

To verify that you've successfully configured the external URLs in the Client Access services virtual directories on the Mailbox server, do the following steps:

1. In the EAC, go to **Servers** \> **Virtual directories**.

2. In the **Select server** field, select the internet-facing Mailbox server.

3. Select a virtual directory and then, in the virtual directory details pane, verify that the **External URL** field is populated with the correct FQDN and service as shown in the following table:

   |**Virtual directory**|**External URL value**|
   |:-----|:-----|
   |**Autodiscover**|No external URL displayed|
   |**ECP**|https://owa.contoso.com/ecp|
   |**EWS**|https://mail.contoso.com/EWS/Exchange.asmx|
   |**Microsoft-Server-ActiveSync**|https://mail.contoso.com/Microsoft-Server-ActiveSync|
   |**OAB**|https://mail.contoso.com/OAB|
   |**OWA**|https://owa.contoso.com/owa|
   |**PowerShell**|http://mail.contoso.com/PowerShell|

To verify that you've successfully configured your public DNS records, do the following steps:

1. Open a command prompt and run `nslookup.exe`.

2. Change to a DNS server that can query your public DNS zone.

3. In `nslookup`, look up the record of each FQDN you created. Verify that the value that's returned for each FQDN is correct.

4. In `nslookup`, type `set type=mx` and then look up the accepted domain you added in Step 1. Verify that the value returned matches the FQDN of the Mailbox server.

## Step 5: Configure internal URLs

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the " _\<Service\>_ virtual directory settings" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

Before clients can connect to your new server from your internal network, you need to configure the internal domains (or URLs) on the virtual directories in the Client Access (frontend) services on the Mailbox server and then in your internal DNS records.

The procedure below lets you choose whether you want users to use the same URL on your intranet and on the internet to access your Exchange server or whether they should use a different URL. What you choose depends on the addressing scheme you have in place already or that you want to implement. If you're implementing a new addressing scheme, we recommend that you use the same URL for both internal and external URLs. Using the same URL makes it easier for users to access your Exchange server because they only have to remember one address.

Regardless of your decision, you need to configure a private DNS zone for the address space you choose. For more information about administering DNS zones, see [Administering DNS Server](https://go.microsoft.com/fwlink/p/?LinkID=190631).

For more information about internal and external URLs on virtual directories, see [Default settings for Exchange virtual directories](../../clients/default-virtual-directory-settings.md) [Virtual Directory Management](https://docs.microsoft.com/exchange/virtual-directory-management-exchange-2013-help).

### Configure internal and external URLs to be the same

1. Open the Exchange Management Shell on your Mailbox server.

2. Store the host name of your Mailbox server in a variable that will be used in the next step. For example, Mailbox01.

   ```powershell
   $HostName = "Mailbox01"
   ```

3. Run each of the following commands in the Exchange Management Shell to configure each internal URL to match the virtual directory's external URL.

   ```powershell
   Set-EcpVirtualDirectory "$HostName\ECP (Default Web Site)" -InternalUrl ((Get-EcpVirtualDirectory "$HostName\ECP (Default Web Site)").ExternalUrl)
   ```

   ```powershell
   Set-WebServicesVirtualDirectory "$HostName\EWS (Default Web Site)" -InternalUrl ((Get-WebServicesVirtualDirectory "$HostName\EWS (Default Web Site)").ExternalUrl)
   ```

   ```powershell
   Set-ActiveSyncVirtualDirectory "$HostName\Microsoft-Server-ActiveSync (Default Web Site)" -InternalUrl ((Get-ActiveSyncVirtualDirectory "$HostName\Microsoft-Server-ActiveSync (Default Web Site)").ExternalUrl)
   ```

   ```powershell
   Set-OabVirtualDirectory "$HostName\OAB (Default Web Site)" -InternalUrl ((Get-OabVirtualDirectory "$HostName\OAB (Default Web Site)").ExternalUrl)
   ```

   ```powershell
   Set-OwaVirtualDirectory "$HostName\OWA (Default Web Site)" -InternalUrl ((Get-OwaVirtualDirectory "$HostName\OWA (Default Web Site)").ExternalUrl)
   ```

   ```powershell
   Set-PowerShellVirtualDirectory "$HostName\PowerShell (Default Web Site)" -InternalUrl ((Get-PowerShellVirtualDirectory "$HostName\PowerShell (Default Web Site)").ExternalUrl)
   ```

After you've configured the internal URL on the Mailbox server virtual directories, you need to configure your private DNS records for Outlook on the web and other connectivity. Depending on your configuration, you'll need to configure your private DNS records to point to the internal or external IP address or FQDN of your Mailbox server. Examples of recommended DNS records that you should create are described in the following table:

|**FQDN**|**DNS record type**|**Value**|
|:-----|:-----|:-----|
|Mail.contoso.com|CNAME|Mailbox01.corp.contoso.com|
|Owa.contoso.com|CNAME|Mailbox01.corp.contoso.com|

#### How do you know this step worked?

To verify that you've successfully configured the internal URL on the Mailbox server virtual directories, do the following:

1. In the EAC, go to **Servers** \> **Virtual directories**.

2. In the **Select server** field, select the internet-facing Mailbox server.

3. Select a virtual directory and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

4. Verify that the **Internal URL** field is populated with the correct FQDN and service as shown in the following table:

   |**Virtual directory**|**Internal URL value**|
   |:-----|:-----|
   |**Autodiscover**|No internal URL displayed|
   |**ECP**|https://owa.contoso.com/ecp|
   |**EWS**|https://mail.contoso.com/EWS/Exchange.asmx|
   |**Microsoft-Server-ActiveSync**|https://mail.contoso.com/Microsoft-Server-ActiveSync|
   |**OAB**|https://mail.contoso.com/OAB|
   |**OWA**|https://owa.contoso.com/owa|
   |**PowerShell**|http://mail.contoso.com/PowerShell|

To verify that you have successfully configured your private DNS records, do the following:

1. Open a command prompt and run `nslookup.exe`.

2. Change to a DNS server that can query your private DNS zone.

3. In `nslookup`, look up the record of each FQDN you created. Verify that the value that's returned for each FQDN is correct.

### Configure different internal and external URLs

1. Open the EAC, and go to **Servers** \> **Virtual directories**,

2. On the internet-facing Mailbox server, select the virtual directory that you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. The virtual directory properties window opens. In the **Internal URL** field, replace the existing host name value in the URL (likely, the FQDN of the Mailbox server) with the new value that you want to use (for example, internal.contoso.com).

   For example, in the properties of the Exchange Web Services (EWS) virtual directory, change the existing value from https://**Mailbox01.corp.contoso.com**/ews/exchange.asmx to https://**internal.contoso.com**/ews/exchange.asmx.

   When you're finished, click **Save**.

4. Repeat the previous steps for each virtual directory you want to change.

   > [!NOTE]
   > The ECP and OWA virtual directory internal URLs must be the same. You can't set an internal URL on the Autodiscover virtual directory.

After you've configured the internal URL on the Mailbox server virtual directories, you need to configure your private DNS records for Outlook on the web, and other connectivity. Depending on your configuration, you'll need to configure your private DNS records to point to the internal or external IP address or FQDN of your Mailbox server. An example of the recommended DNS record that you should create is described in the following table:

|**FQDN**|**DNS record type**|**Value**|
|:-----|:-----|:-----|
|internal.contoso.com|CNAME|Mailbox01.corp.contoso.com|

#### How do you know this step worked?

To verify that you've successfully configured the internal URLs in the Client Access services virtual directories on the Mailbox server, do the following steps:

1. In the EAC, go to **Servers** \> **Virtual directories**.

2. In the **Select server** field, select the internet-facing Mailbox server.

3. Select a virtual directory and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

4. Verify that the **Internal URL** field is populated with the correct FQDN. For example, you may have set the internal URLs to use internal.contoso.com.

   |**Virtual directory**|**Internal URL value**|
   |:-----|:-----|
   |**Autodiscover**|No internal URL displayed|
   |**ECP**|https://internal.contoso.com/ecp|
   |**EWS**|https://internal.contoso.com/EWS/Exchange.asmx|
   |**Microsoft-Server-ActiveSync**|https://internal.contoso.com/Microsoft-Server-ActiveSync|
   |**OAB**|https://internal.contoso.com/OAB|
   |**OWA**|https://internal.contoso.com/owa|
   |**PowerShell**|http://internal.contoso.com/PowerShell|

To verify that you've successfully configured your private DNS records, do the following:

1. Open a command prompt and run `nslookup.exe`.

2. Change to a DNS server that can query your private DNS zone.

3. In `nslookup`, look up the record of each FQDN you created. Verify that the value that's returned for each FQDN is correct.

## Step 6: Configure an SSL certificate

Some services, such as Outlook Anywhere and Exchange ActiveSync, require certificates to be configured on your Exchange server. The following steps show you how to configure an SSL certificate from a third-party certificate authority (CA):

1. [Create an Exchange Server certificate request for a certification authority](../../architecture/client-access/create-ca-certificate-requests.md).

   - You should request a certificate from a third-party CA so your clients automatically trust the certificate. For more information, see [Best practices for Exchange certificates](../../architecture/client-access/certificates.md#best-practices-for-exchange-certificates).

   - If you configured your internal and external URLs to be the same, **Outlook on the web (when accessed from the internet)** and **Outlook on the web (when accessed from the Intranet)** should both show owa.contoso.com. **OAB (when accessed from the internet)** and **OAB (when accessed from the Intranet)** should show mail.contoso.com.

   - If you configured the internal URLs to be internal.contoso.com, **Outlook on the web (when accessed from the internet)** should show owa.contoso.com and **Outlook on the web (when accessed from the Intranet)** should show internal.contoso.com.

2. [Complete a pending Exchange Server certificate request](../../architecture/client-access/complete-pending-certificate-requests.md).

3. [Assign certificates to Exchange Server services](../../architecture/client-access/assign-certificates-to-services.md)

   - At minimum, you should select **SMTP** and **IIS**.

   - If you receive the warning **Overwrite the existing default SMTP certificate?**, click **Yes**.

### How do you know this step worked?

To verify that you've successfully added a new certificate, do the following steps:

1. In the EAC, go to **Servers** \> **Certificates**.

2. Select the new certificate and then, in the certificate details pane, verify that the following are true:

   - **Status** shows **Valid**

   - **Assigned to services** shows, at minimum, **IIS** and **SMTP**.

## How do you know this task worked?

To verify that you've configured mail flow and external client access, do the following steps:

1. In Outlook, on an Exchange ActiveSync device, or on both, create a new profile. Verify that Outlook or the mobile device successfully creates the new profile.

2. In Outlook, or on the mobile device, send a new message to an external recipient. Verify the external recipient receives the message.

3. In the external recipient's mailbox, reply to the message you just sent from the Exchange mailbox. Verify the Exchange mailbox receives the message.

4. Go to https://owa.contoso.com/owa and verify that there are no certificate warnings.
