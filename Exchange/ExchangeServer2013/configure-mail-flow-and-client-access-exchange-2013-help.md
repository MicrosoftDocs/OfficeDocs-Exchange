---
title: 'Configure mail flow and client access: Exchange 2013 Help'
TOCTitle: Configure mail flow and client access
ms:assetid: 4acc7f2a-93ce-468c-9ace-d5f7eecbd8d4
ms:mtpsurl: https://technet.microsoft.com/library/JJ218640(v=EXCHG.150)
ms:contentKeyID: 48385058
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure mail flow and client access

_**Applies to:** Exchange Server 2013_

Post-installation tasks for Exchange Server 2013 mail flow and client access, including how to configure an SSL certificate.

After you've installed Microsoft Exchange Server 2013 in your organization, you need to configure Exchange Server 2013 for mail flow and client access. Without these additional steps, you won't be able to send mail to the Internet and external clients such as Microsoft Outlook and Exchange ActiveSync devices won't be able to connect to your Exchange organization.

The steps in this topic assume a basic Exchange deployment with a single Active Directory site and a single simple mail transport protocol (SMTP) namespace.

> [!IMPORTANT]
> This topic uses example values such as Ex2013CAS, contoso.com, mail.contoso.com, and 172.16.10.11. Replace the example values with the server names, FQDNs, and IP addresses for your organization.

For additional management tasks related to mail flow and clients and devices, see [Mail flow](mail-flow-exchange-2013-help.md) and [Clients and mobile](clients-and-mobile-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 50 minutes

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- You might receive certificate warnings when you connect to the Exchange admin center (EAC) website until you configure a secure sockets layer (SSL) certificate on the Client Access server. You'll be shown how to do this later in this topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!IMPORTANT]
> Each organization requires at a minimum one Client Access server and one Mailbox server in the Active Directory forest. Additionally, each Active Directory site that contains a Mailbox server must also contain at least one Client Access server. If you're separating your server roles, we recommend installing the Mailbox server role first.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Create a Send connector

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

Before you can send mail to the Internet, you need to create a Send connector on the Mailbox server. Do the following.

1. Open the EAC by browsing to the URL of your Client Access server. For example, https://Ex2013CAS/ECP.

2. Enter your username and password in **Domain\\user name** and **Password** and then click **Sign in**.

3. Go to **Mail flow** \> **Send connectors**. On the **Send connectors** page, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

4. In the **New send connector** wizard, specify a name for the Send connector and then select **Internet**. Click **Next**.

5. Verify that **MX record associated with recipient domain** is selected. Click **Next**.

6. Under **Address space**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **Add domain** window, make sure **SMTP** is selected in the **Type** field. In the **Fully Qualified Domain Name (FQDN)** field, enter **\***. Click **Save**.

7. Make sure **Scoped send connector** isn't selected and then click **Next**.

8. Under **Source server**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **Select a Server** window, select a Mailbox server. After you've selected the server, click **Add** and then click **OK**.

9. Click **Finish**.

> [!NOTE]
> A default inbound Receive connector is created when Exchange 2013 is installed. This Receive connector accepts anonymous SMTP connections from external servers. You don't need to do any additional configuration if this is the functionality you want. If you want to restrict inbound connections from external servers, modify the <STRONG>Default Frontend &lt;Client Access server&gt;</STRONG> Receive connector on the Client Access server.

### How do you know this step worked?

To verify that you have successfully created an outbound Send connector, do the following:

1. In the EAC, verify the new Send connector appears in **Mail flow** \> **Send connectors**.

2. Open Outlook Web App and send an email message to an external recipient. If the recipient receives the message, you've successfully configured the Send connector.

## Step 2: Add additional accepted domains

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Accepted domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

By default, when you deploy a new Exchange 2013 organization in an Active Directory forest, Exchange uses the domain name of the Active Directory domain where Setup /PrepareAD was run. If you want recipients to receive and send messages to and from another domain, you must add the domain as an accepted domain. This domain is also added as the primary SMTP address on the default email address policy in the next step.

> [!IMPORTANT]
> A public Domain Name System (DNS) MX resource record is required for each SMTP domain for which you accept email from the Internet. Each MX record should resolve to the Internet-facing server that receives email for your organization.

1. Open the EAC by browsing to the URL of your Client Access server. For example, https://Ex2013CAS/ECP.

2. Enter your username and password in **Domain\\user name** and **Password** and then click **Sign in**.

3. Go to **Mail flow** \> **Accepted domains**. On the **Accepted domains** page, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

4. In the **New accepted domain** wizard, specify a name for the accepted domain.

5. In the **Accepted domain** field, specify the SMTP recipient domain you want to add. For example, contoso.com.

6. Select **Authoritative domain** and then click **Save**.

### How do you know this step worked?

In the EAC, verify the new accepted domain appears in **Mail flow** \> **Accepted domains**.

## Step 3: Configure the default email address policy

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Email address policies" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

If you added an accepted domain in the previous step and you want that domain to be added to every recipient in the organization, you need to update the default email address policy.

1. Open the EAC by browsing to the URL of your Client Access server. For example, https://Ex2013CAS/ECP.

2. Enter your username and password in **Domain\\user name** and **Password** and then click **Sign in**.

3. Go to **Mail flow** \> **Email address policies**. On the **Email address policies** page, select **Default Policy** and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. On the **Default Policy Email Address Policy** page, click **Email Address Format**.

5. Under **Email address format**, click the SMTP address you want to change and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

6. On the **Email address format** page in the **Email address parameters** field, specify the SMTP recipient domain you want to apply to all recipients in the Exchange organization. This domain must match the accepted domain you added in the previous step. For example, @contoso.com. Click **Save**.

7. Click **Save**

8. In the **Default Policy** details pane, click **Apply**.

> [!NOTE]
> We recommend that you configure a user principal name (UPN) that matches the primary email address of each user. If you don't provide a UPN that matches the email address of a user, the user will be required to manually provide their domain\username or UPN in addition to their email address. If their UPN matches their email address, Outlook Web App, ActiveSync, and Outlook will automatically match their email address to their UPN.

### How do you know this step worked?

To verify that you have successfully configured the default email address policy, do the following:

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. Select a mailbox and then, in the recipient details pane, verify that the **User mailbox** field has been set to *\<alias\>*@*\<new accepted domain\>*. For example, david@contoso.com.

3. Optionally, create a new mailbox and verify the mailbox is given an email address with the new accepted domain by doing the following:

   1. Go to **Recipients** \> **Mailboxes**, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") and then select **User mailbox**.

   2. On the new user mailbox page, provide the information required to create a new mailbox. Click **Save**.

   3. Select the new mailbox and then, in the recipient details pane, verify that the **User mailbox** field has been set to *\<alias\>*@*\<new accepted domain\>*. For example, david@contoso.com.

## Step 4: Configure external URLs

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "*\<Service\>* virtual directory settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

Before clients can connect to your new server from the Internet, you need to configure the external domains, or URLs, on the Client Access server's virtual directories and then configure your public domain name service (DNS) records. The steps below configure the same external domain on the external URL of each virtual directory. If you want to configure different external domains on one or more virtual directory external URLs, you need to configure the external URLs manually. For more information, see [Virtual directory management](virtual-directory-management-exchange-2013-help.md).

1. Open the EAC by browsing to the URL of your Client Access server. For example, https://Ex2013CAS/ECP.

2. Enter your username and password in **Domain\\user name** and **Password** and then click **Sign in**.

3. Go to **Servers** \> **Servers**, select the name of the Internet-facing Client Access server and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. Click **Outlook Anywhere**.

5. In the **Specify the external hostname** field, specify the externally accessible FQDN of the Client Access server. For example, mail.contoso.com.

6. While you're here, let's also set the internally accessible FQDN of the Client Access server. In the **Specify the internal hostname** field, insert the FQDN you used in the previous step. For example, mail.contoso.com.

7. Click **Save**.

8. Go to **Servers** \> **Virtual directories** and then click **Configure external access domain** ![Configure icon](images/JJ218640.a9c33f23-3d44-44e7-a5d0-8446200c746e(EXCHG.150).gif "Configure icon").

9. Under **Select the Client Access servers to use with the external URL**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon")

10. Select the Client Access servers you want to configure and then click **Add**. After you've added all of the Client Access servers you want to configure, click **OK**.

11. In **Enter the domain name you will use with your external Client Access servers**, type the external domain you want to apply. For example, mail.contoso.com. Click **Save**.

    > [!NOTE]
    > Some organizations make the Outlook Web App FQDN unique to protect users against changes to underlying server FQDN changes. Many organizations use owa.contoso.com for their Outlook Web App FQDN instead of mail.contoso.com. If you want to configure a unique Outlook Web App FQDN, do the following after you completed the previous step. This checklist assumes you have configured a unique Outlook Web App FQDN.
    > <OL>
    > <LI>
    > <P>Select <STRONG>owa (Default Web Site)</STRONG> and click <STRONG>Edit</STRONG>&nbsp;<IMG title="Edit icon" alt="Edit icon" src="images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif">.</P>
    > <LI>
    > <P>In <STRONG>External URL</STRONG>, type <STRONG>https://</STRONG>, then the unique Outlook Web App FQDN you want to use, and then append <STRONG>/owa</STRONG>. For example, https://owa.contoso.com/owa.</P>
    > <LI>
    > <P>Click <STRONG>Save</STRONG>.</P>
    > <LI>
    > <P>Select <STRONG>ecp (Default Web Site)</STRONG> and click <STRONG>Edit</STRONG>&nbsp;<IMG title="Edit icon" alt="Edit icon" src="images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif">.</P>
    > <LI>
    > <P>In <STRONG>External URL</STRONG>, type <STRONG>https://</STRONG>, then the same Outlook Web App FQDN that you specified in the previous step, and then append <STRONG>/ecp</STRONG>. For example, https://owa.contoso.com/ecp.</P>
    > <LI>
    > <P>Click <STRONG>Save</STRONG>.</P></LI></OL>

After you've configured the external URL on the Client Access server virtual directories, you need to configure your public DNS records for Autodiscover, Outlook Web App, and mail flow. The public DNS records should point to the external IP address or FQDN of your Internet-facing Client Access server and use the externally accessible FQDNs that you've configured on your Client Access server. The following are examples of recommended DNS records that you should create to enable mail flow and external client connectivity.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>FQDN</th>
<th>DNS record type</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Contoso.com</p></td>
<td><p>MX</p></td>
<td><p>Mail.contoso.com</p></td>
</tr>
<tr class="even">
<td><p>Mail.contoso.com</p></td>
<td><p>A</p></td>
<td><p>172.16.10.11</p></td>
</tr>
<tr class="odd">
<td><p>Owa.contoso.com</p></td>
<td><p>CNAME</p></td>
<td><p>Mail.contoso.com</p></td>
</tr>
<tr class="even">
<td><p>Autodiscover.contoso.com</p></td>
<td><p>CNAME</p></td>
<td><p>Mail.contoso.com</p></td>
</tr>
</tbody>
</table>

### How do you know this step worked?

To verify that you have successfully configured the external URL on the Client Access server virtual directories, do the following:

1. In the EAC, go to **Servers** \> **Virtual directories**.

2. In the **Select server** field, select the Internet-facing Client Access server.

3. Select a virtual directory and then, in the virtual directory details pane, verify that the **External URL** field is populated with the correct FQDN and service as shown below:

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Virtual directory</th>
    <th>External URL value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p><strong>Autodiscover</strong></p></td>
    <td><p>No external URL displayed</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>ECP</strong></p></td>
    <td><p>https://owa.contoso.com/ecp</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>EWS</strong></p></td>
    <td><p>https://mail.contoso.com/EWS/Exchange.asmx</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>Microsoft-Server-ActiveSync</strong></p></td>
    <td><p>https://mail.contoso.com/Microsoft-Server-ActiveSync</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>OAB</strong></p></td>
    <td><p>https://mail.contoso.com/OAB</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>OWA</strong></p></td>
    <td><p>https://owa.contoso.com/owa</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>PowerShell</strong></p></td>
    <td><p>http://mail.contoso.com/PowerShell</p></td>
    </tr>
    </tbody>
    </table>

To verify that you have successfully configured your public DNS records, do the following:

1. Open a command prompt and run `nslookup.exe`.

2. Change to a DNS server that can query your public DNS zone.

3. In `nslookup`, look up the record of each FQDN you created. Verify that the value that's returned for each FQDN is correct.

4. In `nslookup`, type `set type=mx` and then look up the accepted domain you added in Step 1. Verify that the value returned matches the FQDN of the Client Access server.

## Step 5: Configure internal URLs

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "*\<Service\>* virtual directory settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

Before clients can connect to your new server from yourintranet, you need to configure the internal domains, or URLs, on the Client Access server's virtual directories and then configure your private domain name service (DNS) records.

The procedure below lets you choose whether you want users to use the same URL on your intranet and on the Internet to access your Exchange server or whether they should use a different URL. What you choose depends on the addressing scheme you have in place already or that you want to implement. If you're implementing a new addressing scheme, we recommend that you use the same URL for both internal and external URLs. Using the same URL makes it easier for users to access your Exchange server because they only have to remember one address. Regardless of the choice you make, you need to make sure you configure a private DNS zone for the address space you configure. For more information about administering DNS zones, see [Administering DNS Server](https://go.microsoft.com/fwlink/p/?linkid=190631).

For more information about internal and external URLs on virtual directories, see [Virtual directory management](virtual-directory-management-exchange-2013-help.md).

### Configure internal and external URLs to be the same

1. Open the Exchange Management Shell on your Client Access server.

2. Store the host name of your Client Access server in a variable that will be used in the next step. For example, Ex2013CAS.

   ```powershell
   $HostName = "Ex2013CAS"
   ```

3. Run each of the following commands in the Shell to configure each internal URL to match the virtual directory's external URL.

   ```powershell
   Set-EcpVirtualDirectory "$HostName\ECP (Default Web Site)" -InternalUrl ((Get-EcpVirtualDirectory "$HostName\ECP (Default Web Site)").ExternalUrl)

   Set-WebServicesVirtualDirectory "$HostName\EWS (Default Web Site)" -InternalUrl ((get-WebServicesVirtualDirectory "$HostName\EWS (Default Web Site)").ExternalUrl)

   Set-ActiveSyncVirtualDirectory "$HostName\Microsoft-Server-ActiveSync (Default Web Site)" -InternalUrl ((Get-ActiveSyncVirtualDirectory "$HostName\Microsoft-Server-ActiveSync (Default Web Site)").ExternalUrl)

   Set-OabVirtualDirectory "$HostName\OAB (Default Web Site)" -InternalUrl ((Get-OabVirtualDirectory "$HostName\OAB (Default Web Site)").ExternalUrl)

   Set-OwaVirtualDirectory "$HostName\OWA (Default Web Site)" -InternalUrl ((Get-OwaVirtualDirectory "$HostName\OWA (Default Web Site)").ExternalUrl)

   Set-PowerShellVirtualDirectory "$HostName\PowerShell (Default Web Site)" -InternalUrl ((Get-PowerShellVirtualDirectory "$HostName\PowerShell (Default Web Site)").ExternalUrl)
   ```

4. While we're in the Shell, let's also configure the Offline Address Book (OAB) to allow Autodiscover to select the right virtual directory for distributing the OAB. Run the following commands to do this.

   ```powershell
   Get-OfflineAddressBook | Set-OfflineAddressBook -GlobalWebDistributionEnabled $True -VirtualDirectories $Null
   ```

After you've configured the internal URL on the Client Access server virtual directories, you need to configure your private DNS records for Outlook Web App, and other connectivity. Depending on your configuration, you'll need to configure your private DNS records to point to the internal or external IP address or fully qualified domain name (FQDN) of your Client Access server. The following are examples of recommended DNS records that you should create to enable internal client connectivity.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>FQDN</th>
<th>DNS record type</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Mail.contoso.com</p></td>
<td><p>CNAME</p></td>
<td><p>Ex2013CAS.corp.contoso.com</p></td>
</tr>
<tr class="even">
<td><p>Owa.contoso.com</p></td>
<td><p>CNAME</p></td>
<td><p>Ex2013CAS.corp.contoso.com</p></td>
</tr>
</tbody>
</table>

### How do you know this step worked?

To verify that you have successfully configured the internal URL on the Client Access server virtual directories, do the following:

1. In the EAC, go to **Servers** \> **Virtual directories**.

2. In the **Select server** field, select the Internet-facing Client Access server.

3. Select a virtual directory and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. Verify that the **Internal URL** field is populated with the correct FQDN and service as shown below:

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Virtual directory</th>
    <th>Internal URL value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p><strong>Autodiscover</strong></p></td>
    <td><p>No internal URL displayed</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>ECP</strong></p></td>
    <td><p>https://owa.contoso.com/ecp</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>EWS</strong></p></td>
    <td><p>https://mail.contoso.com/EWS/Exchange.asmx</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>Microsoft-Server-ActiveSync</strong></p></td>
    <td><p>https://mail.contoso.com/Microsoft-Server-ActiveSync</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>OAB</strong></p></td>
    <td><p>https://mail.contoso.com/OAB</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>OWA</strong></p></td>
    <td><p>https://owa.contoso.com/owa</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>PowerShell</strong></p></td>
    <td><p>http://mail.contoso.com/PowerShell</p></td>
    </tr>
    </tbody>
    </table>

To verify that you have successfully configured your private DNS records, do the following:

1. Open a command prompt and run `nslookup.exe`.

2. Change to a DNS server that can query your private DNS zone.

3. In `nslookup`, look up the record of each FQDN you created. Verify that the value that's returned for each FQDN is correct.

## Configure different internal and external URLs

1. Open the EAC by browsing to the URL of your Client Access server. For example, https://Ex2013CAS/ECP.

2. Go to **Servers** \> **Virtual directories**.

3. In the **Select server** field, select the Internet-facing Client Access server.

4. Select the virtual directory you want to change and click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

5. In **Internal URL**, replace the host name between **https://** and the first forward slash (**/** ) with the new FQDN you want to use. For example, if you want to change the EWS virtual directory FQDN from Ex2013CAS.corp.contoso.com to internal.contoso.com, change the internal URL from https://Ex2013CAS.corp.contoso.com/ews/exchange.asmx to https://internal.contoso.com/ews/exchange.asmx.

6. Click **Save**.

7. Repeat steps 5 and 6 for each virtual directory you want to change.

   > [!NOTE]
   > The ECP and OWA virtual directory internal URLs must be the same.<BR>You can't set an internal URL on the Autodiscover virtual directory.

8. Finally, we need to open the Shell and configure the Offline Address Book (OAB) to allow Autodiscover to select the right virtual directory for distributing the OAB. Run the following commands to do this.

   ```powershell
   Get-OfflineAddressBook | Set-OfflineAddressBook -GlobalWebDistributionEnabled $True -VirtualDirectories $Null
   ```

After you've configured the internal URL on the Client Access server virtual directories, you need to configure your private DNS records for Outlook Web App, and other connectivity. Depending on your configuration, you'll need to configure your private DNS records to point to the internal or external IP address or FQDN of your Client Access server. The following is an example of recommended DNS record that you should create to enable internal client connectivity if you've configured your virtual directory internal URLs to use internal.contoso.com.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>FQDN</th>
<th>DNS record type</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>internal.contoso.com</p></td>
<td><p>CNAME</p></td>
<td><p>Ex2013CAS.corp.contoso.com</p></td>
</tr>
</tbody>
</table>

### How do you know this step worked?

To verify that you have successfully configured the internal URL on the Client Access server virtual directories, do the following:

1. In the EAC, go to **Servers** \> **Virtual directories**.

2. In the **Select server** field, select the Internet-facing Client Access server.

3. Select a virtual directory and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. Verify that the **Internal URL** field is populated with the correct FQDN. For example, you may have set the internal URLs to use internal.contoso.com.

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Virtual directory</th>
    <th>Internal URL value</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p><strong>Autodiscover</strong></p></td>
    <td><p>No internal URL displayed</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>ECP</strong></p></td>
    <td><p>https://internal.contoso.com/ecp</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>EWS</strong></p></td>
    <td><p>https://internal.contoso.com/EWS/Exchange.asmx</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>Microsoft-Server-ActiveSync</strong></p></td>
    <td><p>https://internal.contoso.com/Microsoft-Server-ActiveSync</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>OAB</strong></p></td>
    <td><p>https://internal.contoso.com/OAB</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>OWA</strong></p></td>
    <td><p>https://internal.contoso.com/owa</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>PowerShell</strong></p></td>
    <td><p>http://internal.contoso.com/PowerShell</p></td>
    </tr>
    </tbody>
    </table>

To verify that you have successfully configured your private DNS records, do the following:

1. Open a command prompt and run `nslookup.exe`.

2. Change to a DNS server that can query your private DNS zone.

3. In `nslookup`, look up the record of each FQDN you created. Verify that the value that's returned for each FQDN is correct.

## Step 6: Configure an SSL certificate

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Certificate management" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

Some services, such as Outlook Anywhere and Exchange ActiveSync, require certificates to be configured on your Exchange 2013 server. The following steps show you how to configure an SSL certificate from a third-party certificate authority (CA):

1. Open the EAC by browsing to the URL of your Client Access server. For example, https://Ex2013CAS/ECP.

2. Enter your username and password in **Domain\\user name** and **Password** and then click **Sign in**.

3. Go to **Servers** \> **Certificates**. On the **Certificates** page, make sure your Client Access server is selected in the **Select server** field, and then click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

4. In the **New Exchange certificate** wizard, select **Create a request for a certificate from a certification authority** and then click **Next**.

5. Specify a name for this certificate and then click **Next**.

6. If you want to request a wildcard certificate, select **Request a wild-card certificate** and then specify the root domain of all subdomains in the **Root domain** field. If you don't want to request a wildcard certificate and instead want to specify each domain you want to add to the certificate, leave this page blank. Click **Next**.

7. Click **Browse** and specify an Exchange server to store the certificate on. The server you select should be the Internet-facing Client Access server. Click **Next**.

8. For each service in the list shown, verify that the external or internal server names that users will use to connect to the Exchange server are correct. For example:

   - If you configured your internal and external URLs to be the same, **Outlook Web App (when accessed from the Internet)** and **Outlook Web App (when accessed from the Intranet)** should show owa.contoso.com. **OAB (when accessed from the Internet)** and **OAB (when accessed from the Intranet)** should show mail.contoso.com.

   - If you configured the internal URLs to be internal.contoso.com, **Outlook Web App (when accessed from the Internet)** should show owa.contoso.com and **Outlook Web App (when accessed from the Intranet)** should show internal.contoso.com.

   These domains will be used to create the SSL certificate request. Click **Next**.

9. Add any additional domains you want included on the SSL certificate.

10. Select the domain that you want to be the common name for the certificate, and then click **Set as common name**. For example, contoso.com. Click **Next**.

11. Provide information about your organization. This information will be included with the SSL certificate. Click **Next**.

12. Specify the network location where you want this certificate request to be saved. Click **Finish**.

After you've saved the certificate request, submit the request to your certificate authority (CA). This can be an internal CA or a third-party CA, depending on your organization. Clients that connect to the Client Access server must trust the CA that you use. After you receive the certificate from the CA, complete the following steps:

1. On the **Server** \> **Certificates** page in the EAC, select the certificate request you created in the previous steps.

2. In the certificate request details pane, click **Complete** under **Status**.

3. On the complete pending request page, specify the path to the SSL certificate file and then click **OK**.

4. Select the new certificate you just added, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

5. On the certificate page, click **Services**.

6. Select the services you want to assign to this certificate. At minimum, you should select **SMTP** and **IIS**. Click **Save**.

7. If you receive the warning **Overwrite the existing default SMTP certificate?**, click **Yes**.

### How do you know this step worked?

To verify that you have successfully added a new certificate, do the following:

1. In the EAC, go to **Servers** \> **Certificates**.

2. Select the new certificate and then, in the certificate details pane, verify that the following are true:

   - **Status** shows **Valid**

   - **Assigned to services** shows, at minimum, **IIS** and **SMTP**.

## How do you know this task worked?

To verify that you have configured mail flow and external client access, do the following:

1. In Outlook, on an Exchange ActiveSync device, or on both, create a new profile. Verify that Outlook or the mobile device successfully creates the new profile.

2. In Outlook, or on the mobile device, send a new message to an external recipient. Verify the external recipient receives the message.

3. In the external recipient's mailbox, reply to the message you just sent from the Exchange mailbox. Verify the Exchange mailbox receives the message.

4. Go to https://owa.contoso.com/owa and verify that there are no certificate warnings.
