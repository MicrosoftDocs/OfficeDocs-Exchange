---
title: 'Using AD FS claims-based authentication with Outlook Web App and EAC'
TOCTitle: Using AD FS claims-based authentication with Outlook Web App and EAC
ms:assetid: 919a9bfb-c6df-490a-b2c4-51796b0f0596
ms:mtpsurl: https://technet.microsoft.com/library/Dn635116(v=EXCHG.150)
ms:contentKeyID: 61200292
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Using AD FS claims-based authentication with Outlook Web App and EAC

_**Applies to:** Exchange Server 2013 SP1_

**Summary**:

For on-premises Exchange 2013 Service Pack 1 (SP1) deployments, installing and configuring Active Directory Federation Services (AD FS) means you can now use AD FS claims-based authentication to connect to Outlook Web App and EAC. You can integrate AD FS and claims-based authentication with Exchange 2013 SP1. Using claims-based authentication replaces traditional authentication methods, including the following:

- Windows authentication

- Forms authentication

- Digest authentication

- Basic authentication

- Active Directory client certificate authentication

Authentication is the process of confirming the identity of a user. Authentication validates that the user is who he or she claims to be. Claims-based identity is another approach to authentication. Claims-based authentication removes the management of authentication from the application (in this case, Outlook Web App and EA) to make it easier to manage accounts by centralizing authentication. Outlook Web App and EAC aren't responsible for authenticating users, storing user accounts and passwords, looking up user identity details, or integrating with other identity systems. Centralizing authentication helps make it easier to upgrade to authentication methods in the future.

> [!NOTE]
> OWA for Devices doesn't support AD FS claims-based authentication.

There are multiple versions of AD FS that can be used, as summarized by the following table.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Windows Server version</th>
<th>Installation</th>
<th>AD FS version</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Windows Server 2008 R2</p></td>
<td><p><strong>Download and install</strong> AD FS 2.0, which is an add-on Windows component.</p></td>
<td><p>AD FS 2.0</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012</p></td>
<td><p>Install the <strong>built-in</strong> AD FS server role.</p></td>
<td><p>AD FS 2.1</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Install the <strong>built-in</strong> AD FS server role.</p></td>
<td><p>AD FS 3.0</p></td>
</tr>
</tbody>
</table>

The tasks that you will perform here are based on Windows Server 2012 R2 that includes the AD FS role service.

**Overview of the required steps**

Step 1 - Review the certificate requirements for AD FS

Step 2 - Install and configure Active Directory Federation Services (AD FS)

Step 3 - Create a relying party trust and custom claim rules for Outlook Web App and EAC

Step 4 - Install the Web Application Proxy role service (optional)

Step 5 - Configure the Web Application Proxy role service (optional)

Step 6 - Publish Outlook Web App and EAC using Web Application Proxy (optional)

Step 7 - Configure Exchange 2013 to use AD FS authentication

Step 8 - Enable AD FS authentication on the OWA and ECP virtual directories

Step 9 - Restart or recycle Internet Information Services (IIS)

Step 10 - Test the AD FS claims for Outlook Web App and EAC

Additional information you might want to know

## What do I need to know before I begin?

- At a minimum, you need to install separate Windows Server 2012 R2 servers: one as a domain controller that uses Active Directory Domain Services (AD DS), an Exchange 2013 server, a Web Application Proxy server, and an Active Directory Federation Services (AD FS) server. Verify that all updates are installed.

- Install AD DS on the appropriate number of Windows Server 2012 R2 servers in your organization. You can also use **Notifications** in **Server Manager** \> **Dashboard** to **Promote this server to a domain controller**.

- Install the appropriate number of Client Access and Mailbox servers for your organization. Verify that all updates are installed, including SP1, on all Exchange 2013 servers in your organization. To download SP1, see [Updates for Exchange 2013](updates-for-exchange-2013-exchange-2013-help.md).

- Deploying Web Application Proxy on a server requires local administrator permissions. You must deploy AD FS on a server running Windows Server 2012 R2 in your organization before you can deploy Web Application Proxy.

- Install and configure the AD FS role and create relying party trusts and claim rules on Windows Server 2012 R2. To do this, you need to log on with a user account that is a member of the Domain Admins, Enterprise Admins, or local Administrators group.

- Determine the required permissions for Exchange 2013 by seeing [Feature permissions](feature-permissions-exchange-2013-help.md).

- You need to be assigned permissions for managing Outlook Web App. To see what permissions you need, see the "Outlook Web App permissions" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- You need to be assigned permissions for managing EAC. To see what permissions you need, see the "Exchange admin center connectivity" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- You might be able to use only the Shell to perform some procedures. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://technet.microsoft.com/library/dd638134\(v=exchg.150\)).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1 - Review the certificate requirements for AD FS

Certificates play a critical role in securing communications between Exchange 2013 SP1 servers; web clients such as Outlook Web App; and EAC, Windows Server 2012 R2 servers, including Active Directory Federation Services (AD FS) servers and Web Application Proxy servers. The requirements for certificates vary depending on whether you are setting up an AD FS server, AD FS Proxy, or Web Application Proxy server. The certificates that are used for AD FS services including the SSL and token signing certificates must be imported into the Trust Root Certification Authorities store on all of your Exchange, AD FS and Web Application Proxy servers. The thumbprint for the certificate that is imported is also used on the Exchange 2013 SP1 servers when you use the [Set-OrganizationConfig](https://technet.microsoft.com/library/aa997443\(v=exchg.150\)) cmdlet.

In any AD FS design, various certificates must be used to secure communication between users on the Internet and AD FS servers. Each federation server must have a service communication certificate or Secure Socket Layer (SSL) certificate and a token-signing certificate before AD FS servers, Active Directory domain controllers, and Exchange 2013 servers can communicate and authenticate. Depending on your security and budget requirements, carefully consider which of your certificates will be obtained by a public CA or an Enterprise CA. If you want to install and configure an Enterprise Root or Subordinate CA, you can use Active Directory Certificate Services (AD CS). If you want to know more about AD CS, see [Active Directory Certificate Services Overview](https://go.microsoft.com/fwlink/?linkid=392697).

Although AD FS doesn't require certificates be issued by a CA, the SSL certificate (the SSL certificate that is also used by default as the service communications certificate) must be trusted by the AD FS clients. We recommend that you don't use self-signed certificates. Federation servers use an SSL certificate to secure web services traffic for SSL communication with web clients and with federation server proxies. Because the SSL certificate must be trusted by client computers, we recommend that you use a certificate that is signed by a trusted CA. All certificates that you select must have a corresponding private key. After you receive a certificate from a CA (Enterprise or public), make sure that all certificates are imported into the Trust Root Certification Authorities store on all servers. You can import certificates into the store with the **Certificates** MMC snap-in or distribute the certificates by using Active Directory Certificate Services. It's important that if the certificate that you imported expires, you manually import another valid certificate.

> [!IMPORTANT]
> If you use the self-signed token signing certificate from AD FS, you must import this certificate into the Trust Root Certification Authorities store on all of your Exchange 2013 servers. If the self-signed token signing certificate isn't used and Web Application Proxy is deployed, then you must update the public key in the Web Application Proxy configuration and all AD FS relying party trusts.

When you are setting up Exchange 2013 SP1, AD FS, and Web Application Proxy, follow these certificate recommendations:

- **Mailbox servers**: The certificates that are used on the Mailbox servers are self-signed certificates are they are created when Exchange 2013 is installed. Because all clients connect to an Exchange 2013 Mailbox server through an Exchange 2013 Client Access server, the only certificates that you need to manage are those on the Client Access servers.

- **Client Access servers**: An SSL certificate used for service communications is required. If your existing SSL certificate already includes the FQDN you are using to set up the relying party trust endpoint, no additional certificates are required.

- **AD FS**: Two types of certificates are required by AD FS:

  - SSL certificate used for service communications

    - Subject name: **adfs.contoso.com** (AD FS deployment name)

    - Subject Alternative Name (SAN): None

  - Token signing certificate

    - Subject name: **tokensigning.contoso.com**

    - Subject Alternative Name (SAN): None

  > [!NOTE]
  > When you are replacing the token signing certificate on AD FS any existing relying party trusts must be updated to use the new token-signing certificate.

- **Web Application Proxy**

     - SSL certificate used for service communications

        - Subject name: **owa.contoso.com**

        - Subject Alternative Name (SAN): None

   > [!NOTE]
   > If your Web Application Proxy External URL is the same as your internal URL, you can reuse Exchange's SSL certificate here.

     - AD FS Proxy SSL certificate

        - Subject name: **adfs.contoso.com** (AD FS deployment name)

        - Subject Alternative Name (SAN): None

     - Token signing certificate - This will be copied over from AD FS automatically as part of the steps below. If this certificate is used, it must be trusted by the Exchange 2013 servers in your organization.

See the certificate requirements section in [Review the requirements for deploying AD FS](https://go.microsoft.com/fwlink/?linkid=392699) for more information about certificates.

> [!NOTE]
> An SSL encryption certificate is still needed for Outlook Web App and EAC even if you have an SSL certificate for AD FS. The SSL certificate is used on the OWA and ECP virtual directories.

## Step 2 - Install and configure Active Directory Federation Services (AD FS)

AD FS in Windows Server 2012 R2 provides simplified, secured identity federation and web single sign-on (SSO) capabilities. AD FS includes a federation service that enables browser-based web SSO, multifactor, and claims-based authentication. AD FS simplifies access to systems and applications by using a claims-based authentication and access authorization mechanism to maintain application security.

To install AD FS on Windows Server 2012 R2:

1. Open **Server Manager** on the **Start** screen or **Server Manager** on the taskbar on the desktop. Click **Add Roles and Features** on the **Manage** menu.

2. On the **Before You Begin** page, click **Next**.

3. On the **Select Installation Type** page, click **Role-based or Feature-based installation**, and then click **Next**.

4. On the **Select Destination Server** page, click **Select a server from the server pool**, verify that the local computer is selected, and then click **Next**.

5. On the **Select Server Roles** page, click **Active Directory Federation Services**, and then click **Next**.

6. On the **Select Features** page, click **Next**. The required prerequisites or features are already selected for you. You do not need to select any other features.

7. On the **Active Directory Federation Service (AD FS)** page, click **Next**.

8. On the **Confirm Installation Selections** page, check **Restart the destination server automatically if required**, and then click **Install**.

    > [!NOTE]
    > Do not close the wizard during the installation process.

After you install the required AD FS servers and generate the required certificates, you must configure AD FS and then test that AD FS is working correctly. You can also use the checklist here to help you in setting up and configuring AD FS: [Checklist: Setting Up a Federation Server](https://go.microsoft.com/fwlink/?linkid=392700).

To configure Active Directory Federation Services:

1. On the **Installation Progress** page, in the window under **Active Directory Federation Services**, click **Configure the federation service on this server**. The Active Directory Federation Service Configuration Wizard opens.

2. On the **Welcome** page, click **Create the first federation server in a federation server farm**, and then click **Next**.

3. On the **Connect to AD DS** page, specify an account with domain administrator rights for the correct Active Directory domain that this computer is joined to, and then click **Next**. If you need to select a different user, click **Change**.

4. On the **Specify Service Properties** page, do the following, and then click **Next**:

   - Import the SSL certificate that you obtained earlier from AD CS or a public CA. This certificate is the required service authentication certificate. Browse to the location of your SSL certificate. For details on creating and importing SSL certificates, see [Server Certificates](https://go.microsoft.com/fwlink/?linkid=392703).

   - Enter a name for your federation service (for example, type **adfs.contoso.com**).

   - To provide a display name for your federation service, type the name of your organization (for example, **Contoso, Ltd.**).

5. On the **Specify Service Account** page, select **Use an existing domain user account or group Managed Service Account**, and then specify the GMSA account (FsGmsa) that you created when you created the domain controller. Include the account password, and then click **Next**.

    > [!NOTE]
    > Globally Managed Service Account (GMSA) is an account that must be created when you configure a domain controller. The GMSA account is required during the AD FS installation and configuration. If you haven't created this account yet, run the following Windows PowerShell command. It creates the account for the contoso.com domain and the AD FS server:

6. Run the following command.

   ```powershell
   Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
   ```

7. This example creates a new GMSA account named FsGmsa for the Federation Service named adfs.contoso.com. The Federation Service name is the value that's visible to clients.

   ```powershell
   New-ADServiceAccount FsGmsa -DNSHostName adfs.contoso.com -ServicePrincipalNames http/adfs.contoso.com
   ```

8. On the **Specify Configuration Database** page, select **Create a database on this server using Windows Internal Database**, and then click **Next**.

9. On the **Review Options** page, verify your configuration selections. You can optionally use the **View Script** button to automate additional AD FS installations. Click **Next**.

10. On the **Prerequisite Checks** page, verify that all the prerequisite checks were successfully completed, and then click **Configure**.

11. On the **Installation progress** page, verify that everything installed correctly, and then click **Close**.

12. On the **Results** page, review the results, check whether the configuration completed successfully, and then click **Next steps required for completing your federation service deployment**.

The following Windows PowerShell commands do the same thing as the preceding steps.

```powershell
Import-Module ADFS
```

```powershell
Install-AdfsFarm -CertificateThumbprint 0E0C205D252002D535F6D32026B6AB074FB840E7 -FederationServiceDisplayName "Contoso Corporation" -FederationServiceName adfs.contoso.com -GroupServiceAccountIdentifier "contoso\FSgmsa`$"
```

For details and syntax, see [Install-AdfsFarm](https://go.microsoft.com/fwlink/?linkid=392704).

To verify the installation: On the AD FS server, open your web browser, and then browse to the URL of the federation metadata (for example, **https://adfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml**).

## Step 3 - Create a relying party trust and custom claim rules for Outlook Web App and EAC

For all applications and services that you want to publish through Web Application Proxy, you must configure a relying party trust on the AD FS server. For deployments with multiple Active Directory sites that use separate namespaces, a relying party trust for Outlook Web App and EAC must be added for each namespace.

EAC uses the ECP virtual directory. You can view or configure settings for EAC by using the [Get-EcpVirtualDirectory](https://technet.microsoft.com/library/dd351058\(v=exchg.150\)) and the [Set-EcpVirtualDirectory](https://technet.microsoft.com/library/dd297991\(v=exchg.150\)) cmdlets. To access EAC, you must use a web browser and go to **http://server1.contoso.com/ecp**.

> [!NOTE]
> The inclusion of the trailing slash <STRONG>/</STRONG> in the URL examples shown below is intentional. It's important to ensure that both the AD FS relying party trusts and Exchange Audience URI's <STRONG>are identical</STRONG>. This means the AD FS relying party trusts and Exchange Audience URI's should <STRONG>both have</STRONG> or <STRONG>both emit</STRONG> the trailing slashes in their URLs. The examples in this section contain the trailing <STRONG>/</STRONG>'s after any url ending with "owa" ( /owa/) or "ecp" (/ecp/).

For Outlook Web App, to create relying party trusts by using the AD FS Management snap-in in Windows Server 2012 R2:

1. In **Server Manager**, click **Tools**, and then select **AD FS Management**.

2. In **AD FS snap-in**, under **AD FS\\Trust Relationships**, right-click **Relying Party Trusts**, and then click **Add Relying Party Trust** to open the **Add Relying Party Trust** wizard.

3. On the **Welcome** page, click **Start**.

4. On the **Select Data Source** page, click **Enter data about the relying party manually**, and then click **Next**.

5. On the **Specify Display Name** page, in the **Display Name** box, type **Outlook Web App**, and then under **Notes**, type a description for this relying party trust (such as **This is a trust for https://mail.contoso.com/owa/**) and then click **Next**.

6. On the **Choose Profile** page, click **AD FS profile**, and then click **Next**.

7. On the **Configure Certificate** page, click **Next**.

8. On the **Configure URL** page, click **Enable support for the WS-Federation Passive protocol**, and then under **Relying party WS-Federation Passive protocol URL**, **type https://mail.contoso.com/owa/**, and then click **Next**.

9. On the **Configure Identifiers** page, specify one or more identifiers for this relying party, click **Add** to add them to the list, and then click **Next**.

10. On the **Configure Multi-factor Authentication Now?** page, select **Configure multi-factor authentication settings for this relying party trust**.

11. On the **Configure Multi-factor Authentication** page, verify that **I do not want to configure multi-factor authentication settings for this relying party trust at this time** is selected, and then click **Next**.

12. On the **Choose Issuance Authorization Rules** page, select **Permit all users to access this relying party**, and then click **Next**.

13. On the **Ready to Add Trust** page, review the settings, and then click **Next** to save your relying party trust information.

14. On the **Finish** page, verify that **Open the Edit Claim Rules dialog for this relying party trust when the wizard closes** isn't selected, and then click **Close**.

To create a relying party trust for EAC, you must do these steps again and create a second relying party trust, but instead of putting in **Outlook Web App** for the display name, enter **EAC**. For the description, enter **This is a trust for the Exchange admin center**, and the **Relying party WS-Federation Passive protocol URL** is **https://mail.contoso.com/ecp**.

In a claims-based identity model, the function of Active Directory Federation Services (AD FS) as a federation service is to issue a token that contains a set of claims. Claims rules govern the decisions in regard to claims that AD FS issues. Claim rules and all server configuration data are stored in the AD FS configuration database.

It's required that you create two claim rules:

- Active Directory user SID

- Active Directory UPN

To add the required claims rules:

1. In **Server Manager**, click **Tools**, and then click **AD FS Management**.

2. In the console tree, under **AD FS\\Trust Relationships**, click either **Claims Provider Trusts** or **Relying Party Trusts**, and then click the relying party trust for Outlook Web App.

3. In the **Relying Party Trusts** window, right-click the Outlook Web App trust, and then click **Edit Claim Rules**.

4. In the **Edit Claim Rules** window, on the **Issuance Transform Rules** tab, click **Add Rule** to start the Add Transform Claim Rule Wizard.

5. On the **Select Rule Template** page, under **Claim rule template**, select **Send Claims Using a Custom Rule** in the list, and then click **Next**.

6. On the **Configure Rule** page, in the **Choose Rule Type** step, under **Claim rule name**, enter the name for the claim rule. Use a descriptive name for the claim rule (for example, **ActiveDirectoryUserSID**). Under **Custom rule**, enter the following claim rule language syntax for this rule:

   ```console
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"), query = ";objectSID;{0}", param = c.Value);
   ```

7. On the **Configure Rule** page, click **Finish**.

8. In the **Edit Claim Rules** window, on the **Issuance Transform Rules** tab, click **Add Rule** to start the Add Transform Claim Rule Wizard.

9. On the **Select Rule Template** page, under **Claim rule template**, select **Send Claims Using a Custom Rule** in the list, and then click **Next**.

10. On the **Configure Rule** page, on the **Choose Rule Type** step, under **Claim rule name**, enter the name for the claim rule. Use a descriptive name for the claim rule (for example, **ActiveDirectoryUPN**). Under **Custom rule**, enter the following claim rule language syntax for this rule:

    ```console
    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"] => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);
    ```

11. Click **Finish**.

12. In the **Edit Claim Rules** window, click **Apply**, and then **OK**.

13. Repeat steps 3-12 of this procedure for the EAC relying party trust.

Alternatively, you can create relaying party trusts and claim rules by using Windows PowerShell:

1. Create the two .txt files IssuanceAuthorizationRules.txt and IssuanceTransformRules.txt.

2. Import their content into two variables.

3. Run the following two cmdlets to create the relying party trusts. In this example, this will also configure the claim rules.

  **IssuanceAuthorizationRules.txt contains:**

   ```console
   @RuleTemplate = "AllowAllAuthzRule" => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
   ```

  **IssuanceTransformRules.txt contains:**

   ```console
   @RuleName = "ActiveDirectoryUserSID" c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"), query = ";objectSID;{0}", param = c.Value);
   ```

   ```console
   @RuleName = "ActiveDirectoryUPN" c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"] => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);
   ```

  **Run the following commands:**

   ```powershell
   [string]$IssuanceAuthorizationRules=Get-Content -Path C:\IssuanceAuthorizationRules.txt

   [string]$IssuanceTransformRules=Get-Content -Path c:\IssuanceTransformRules.txt

   Add-ADFSRelyingPartyTrust -Name "Outlook Web App" -Enabled $true -Notes "This is a trust for https://mail.contoso.com/owa/" -WSFedEndpoint https://mail.contoso.com/owa/ -Identifier https://mail.contoso.com/owa/ -IssuanceTransformRules $IssuanceTransformRules -IssuanceAuthorizationRules $IssuanceAuthorizationRules

   Add-ADFSRelyingPartyTrust -Name "Exchange admin center (EAC)" -Enabled $true -Notes "This is a trust for https://mail.contoso.com/ecp/" -WSFedEndpoint https://mail.contoso.com/ecp/ -Identifier https://mail.contoso.com/ecp/ -IssuanceTransformRules $IssuanceTransformRules -IssuanceAuthorizationRules $IssuanceAuthorizationRules
   ```

## Step 4 - Install the Web Application Proxy role service (optional)

> [!NOTE]
> Step 4, Step 5, and Step 6 are for users want to publish Exchange OWA and ECP using Web Application Proxy, and who want to have Web Application Proxy perform the AD FS authentication. However, publishing Exchange with Web Application Proxy is not required, so you can skip to Step 7 if you don't use Web Application Proxy and you do want Exchange to perform the AD FS authentication itself.

Web Application Proxy is a new Remote Access role service in Windows Server 2012 R2. Web Application Proxy provides reverse proxy functionality for web applications inside your corporate network to allow users on many devices to access them from outside the corporate network. Web Application Proxy preauthenticates access to web applications by using Active Directory Federation Services (AD FS) and also functions as an AD FS proxy. Although Web Application Proxy isn't required, it is recommended when AD FS is accessible to external clients. However, offline access in Outlook Web App isn't supported when using AD FS authentication through Web Application Proxy. You can find more information about integrating with Web Application Proxy by seeing [Installing and Configuring Web Application Proxy for Publishing Internal Applications](https://go.microsoft.com/fwlink/?linkid=392705)

> [!WARNING]
> You can't install Web Application Proxy on the same server with AD FS installed.

To deploy Web Application Proxy, you must install the Remote Access server role with the Web Application Proxy role service on a server that will act as the Web Application Proxy server. To install the Web Application Proxy role service:

1. On the Web Application Proxy server, in **Server Manager**, click **Manage**, and then click **Add Roles and Features**.

2. In the Add Roles and Features Wizard, click **Next** three times to get to the **Server Roles** page.

3. On the **Server Roles** page, select **Remote Access** in the list, and then click **Next**.

4. On the **Features** page, click **Next**.

5. On the **Remote Access** page, read the information, and then click **Next**.

6. On the **Role Services** page, select **Web Application Proxy**. Then in the **Add Roles and Features Wizard** window, click **Add Features**, and then click **Next**.

7. In the **Confirmation** window, click **Install**. You can also check **Restart the destination server automatically if required**.

8. In the **Installation progress** dialog box, verify that the installation was successful, and then click **Close**.

The following Windows PowerShell cmdlet does the same thing as the preceding steps.

```powershell
Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools
```

## Step 5 - Configure the Web Application Proxy role service (optional)

You must configure Web Application Proxy to connect to the AD FS server. Repeat this procedure for all of the servers that you want to deploy as Web Application Proxy servers.

To configure the Web Application role service:

1. On the Web Application Proxy server, in **Server Manager**, click **Tools**, and then click **Remote Access Management**.

2. In the **Configuration** pane, click **Web Application Proxy**.

3. In the **Remote Access Management** console, in the middle pane, click **Run the Web Application Proxy Configuration Wizard**.

4. In the Web Application Proxy Configuration Wizard, in the **Welcome** dialog box, click **Next**.

5. On the **Federation Server** page, do the following, and then click **Next**:

   - In the **Federation service name** box, enter the fully qualified domain name (FQDN) of the AD FS server (for example, **adfs.contoso.com**).

   - In the **User name** and **Password** boxes, type the credentials of a local administrator account on the AD FS servers.

6. In the **AD FS Proxy Certificate** dialog box, in the list of certificates currently installed on the Web Application Proxy server, select the certificate to be used by Web Application Proxy for AD FS proxy functionality, and then click **Next**. The certificate you choose here should be the one whose subject is the Federation Service name (for example, **adfs.contoso.com**).

7. In the **Confirmation** dialog box, review the settings. If required, you can copy the Windows PowerShell cmdlet to automate additional installations. Click **Configure**.

8. In the **Results** dialog box, verify that the configuration was successful, and then click **Close**.

The following Windows PowerShell cmdlet does the same thing as the preceding steps.

```powershell
Install-WebApplicationProxy -CertificateThumprint 1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b -FederationServiceName adfs.contoso.com
```

## Step 6 - Publish Outlook Web App and EAC by using Web Application Proxy (optional)

In step 3, you created claims relaying party trusts for Outlook Web App and EAC, and you now need to publish both of these applications. But before you do this, verify that a relying party trust for them was created, and verify that you have a certificate on the Web Application Proxy server that is suitable for Outlook Web App and EAC. For all the AD FS endpoints that you require to be published by Web Application Proxy, in the AD FS Management console, you must set the endpoint to be **Proxy Enabled**.

Follow the steps to publish Outlook Web App by using Web Application Proxy. For EAC, you repeat these steps. When you publish EAC, you need to change the name, external URL, external certificate, and back-end URL.

To publish Outlook Web App and EAC by using Web Application Proxy:

1. On the Web Application Proxy server, in the **Remote Access Management** console, in the **Navigation** pane, click **Web Application Proxy**, and then in the **Tasks** pane, click **Publish**.

2. In the Publish New Application Wizard, on the **Welcome** page, click **Next**.

3. On the **Preauthentication** page, click **Active Directory Federation Services (AD FS)**, and then click **Next**.

4. On the **Relying Party** page, in the list of relying parties, select the relying party for the application that you want to publish, and then click **Next**.

5. On the **Publishing Settings** page, do the following, and then click **Next**:

   1. In the **Name** box, enter a friendly name for the application. This name is used only in the list of published applications in the **Remote Access Management console**. You can use **OWA** and **EAC** for the names.

   2. In the **External URL** box, enter the external URL for this application (for example, **https://external.contoso.com/owa/** for Outlook Web App and **https://external.contoso.com/ecp/** for EAC).

   3. In the **External certificate** list, select a certificate whose subject name matches the host name of the external URL.

   4. In the **Backend server URL** box, enter the URL of the back-end server. Note that this value is automatically entered when you enter the external URL, and you should change it only if the back-end server URL is different (for example, **https://mail.contoso.com/owa/** for Outlook Web App and **https://mail.contoso.com/ecp/** for EAC).

   > [!NOTE]
   > Web Application Proxy can translate host names in URLs but cannot translate paths. Therefore, you can enter different host names, but you must enter the same path. For example, you can enter an external URL of <EM>https://external.contoso.com/app1/</EM> and a back-end server URL of <EM>https://mail.contoso.com/app1/</EM>. However, you cannot enter an external URL of <EM>https://external.contoso.com/app1/</EM> and a back-end server URL of <EM>https://mail.contoso.com/internal-app1/</EM>.

6. On the **Confirmation** page, review the settings, and then click **Publish**. You can copy the Windows PowerShell command to set up additional published applications.

7. On the **Results** page, make sure that the application published successfully, and then click **Close**.

The following Windows PowerShell cmdlet performs the same tasks as the preceding procedure for Outlook Web App.

```powershell
Add-WebApplicationProxyApplication -BackendServerUrl 'https://mail.contoso.com/owa/' -ExternalCertificateThumbprint 'E9D5F6CDEA243E6E62090B96EC6DE873AF821983' -ExternalUrl 'https://external.contoso.com/owa/' -Name 'OWA' -ExternalPreAuthentication ADFS -ADFSRelyingPartyName 'Outlook Web App'
```

The following Windows PowerShell cmdlet performs the same tasks as the preceding procedure for EAC.

```powershell
Add-WebApplicationProxyApplication -BackendServerUrl 'https://mail.contoso.com/ecp/' -ExternalCertificateThumbprint 'E9D5F6CDEA243E6E62090B96EC6DE873AF821983' -ExternalUrl 'https://external.contoso.com/ecp/' -Name 'EAC' -ExternalPreAuthentication ADFS -ADFSRelyingPartyName 'Exchange admin center'
```

After you complete these steps, Web Application Proxy will perform AD FS authentication for Outlook Web App and EAC clients, and it will also proxy the connections to Exchange on their behalf. You do not need to configure Exchange itself for AD FS authentication, so proceed to step 10 to test your configuration.

## Step 7 - Configure Exchange 2013 to use AD FS authentication

When you are configuring AD FS to be used for claims-based authentication with Outlook Web App and EAC in Exchange 2013, you must enable AD FS for your Exchange organization. You must use the [Set-OrganizationConfig](https://technet.microsoft.com/library/aa997443\(v=exchg.150\)) cmdlet to configure AD FS settings for your organization:

- Set the AD FS issuer to **https://adfs.contoso.com/adfs/ls/**.

- Set the AD FS URIs to **https://mail.contoso.com/owa/** and **https://mail.contoso.com/ecp/**.

- Find the AD FS token signing certificate thumbprint by using Windows PowerShell on the AD FS server and entering `Get-ADFSCertificate -CertificateType "Token-signing"`. Then, assign the token-signing certificate thumbprint that you found. If the AD FS token-signing certificate has expired, the thumbprint from the new AD FS token-signing certificate must be updated by using the [Set-OrganizationConfig](https://technet.microsoft.com/library/aa997443\(v=exchg.150\)) cmdlet.

Run the following commands in the Exchange Management Shell.

```powershell
$uris = @("https://mail.contoso.com/owa/","https://mail.contoso.com/ecp/")
Set-OrganizationConfig -AdfsIssuer "https://adfs.contoso.com/adfs/ls/" -AdfsAudienceUris $uris -AdfsSignCertificateThumbprint "88970C64278A15D642934DC2961D9CCA5E28DA6B"
```

> [!NOTE]
> The <EM>-AdfsEncryptCertificateThumbprint</EM> parameter isn't supported for these scenarios.

For details and syntax, see [Set-OrganizationConfig](https://technet.microsoft.com/library/aa997443\(v=exchg.150\)) and [Get-ADFSCertificate](https://go.microsoft.com/fwlink/?linkid=392706).

## Step 8 - Enable AD FS authentication on the OWA and ECP virtual directories

For the OWA and ECP virtual directories, enable AD FS authentication as the only authentication method and disable all other forms of authentication.

> [!WARNING]
> You must configure the ECP virtual directory before you configure the OWA virtual directory.

Configure the ECP virtual directory by using the Exchange Management Shell. In the Shell window, run the following command.

```powershell
Get-EcpVirtualDirectory | Set-EcpVirtualDirectory -AdfsAuthentication $true -BasicAuthentication $false -DigestAuthentication $false -FormsAuthentication $false -WindowsAuthentication $false
```

Configure the OWA virtual directory by using the Exchange Management Shell. In the Shell window, run the following command.

```powershell
Get-OwaVirtualDirectory | Set-OwaVirtualDirectory -AdfsAuthentication $true -BasicAuthentication $false -DigestAuthentication $false -FormsAuthentication $false -WindowsAuthentication $false -OAuthAuthentication $false
````

> [!NOTE]
> The preceding Exchange Management Shell commands configure the OWA and ECP virtual directories on every Client Access server in your organization. If you don't want to apply these settings to all Client Access servers, use the <EM>-Identity</EM> parameter and specify the Client Access server. It's likely you will want to apply these settings only to the Client Access servers in your organization that are Internet facing.

For details and syntax, see [Get-OwaVirtualDirectory](https://technet.microsoft.com/library/aa998588\(v=exchg.150\)) and [Set-OwaVirtualDirectory](https://technet.microsoft.com/library/bb123515\(v=exchg.150\)) or [Get-EcpVirtualDirectory](https://technet.microsoft.com/library/dd351058\(v=exchg.150\)) and [Set-EcpVirtualDirectory](https://technet.microsoft.com/library/dd297991\(v=exchg.150\)).

## Step 9 - Restart or recycle Internet Information Services (IIS)

After you have completed all of the required steps, including making changes to Exchange virtual directories, you need to restart Internet Information Services. To do this, you can use one of the following methods:

- Using Windows PowerShell:

  ```powershell
  Restart-Service W3SVC,WAS -force
  ```

- Using a command line: Click **Start**, click **Run**, type `IISReset /noforce`, and then click **OK**.

- Using Internet Information Servers (IIS) Manager: In **Server Manager** \> **IIS**, click **Tools**, and then click **Internet Information Services (IIS) Manager**. In the **Internet Information Servers (IIS) Manager** window, in the action pane under **Manage Server**, click **Restart**.

## Step 10 - Test the AD FS claims for Outlook Web App and EAC

To test the AD FS claims for Outlook Web App:

- In your web browser, sign in to Outlook Web App (for example, **https://mail.contoso.com/owa**).

- In the browser window, if you get a certificate error, just continue on to the Outlook Web App website. You should be redirected to the ADFS sign-in page or the ADFS prompt for credentials.

- Type your user name (domain\\user) and password, and then click **Sign in**.

Outlook Web App will load in the window.

To test the AD FS claims for EAC:

1. In your web browser, go to **https://mail.contoso.com/ecp**.

2. In the browser window, if you get a certificate error, just continue on to the ECP website. You should be redirected to the ADFS sign-in page or the ADFS prompt for credentials.

3. Type your user name (domain\\user) and password, and then click **Sign in**.

4. EAC should load in the window.

## Additional information you might want to know

### Multifactor authentication

For on-premises Exchange 2013 SP1 deployments, deploying and configuring Active Directory Federation Services (AD FS) 2.0 by using claims means that Outlook Web App and EAC in Exchange 2013 SP1 can support multifactor authentication methods, such as certificate-based authentication, authentication or security tokens, and fingerprint authentication. Two-factor authentication is often confused with other forms of authentication. Multifactor authentication requires the use of two of the three authentication factors. These factors are:

- Something only the user knows (for example, the password, PIN, or pattern)

- Something only the user has (for example, an ATM card, security token, smart card, or mobile phone)

- Something only the user is (for example, a biometric characteristic, such as a fingerprint)

For details on multifactor authentication in Windows Server 2012 R2, see [Overview: Manage Risk with Additional Multi-Factor Authentication for Sensitive Applications](https://go.microsoft.com/fwlink/?linkid=392707) and [Walkthrough Guide: Manage Risk with Additional Multi-Factor Authentication for Sensitive Applications](https://go.microsoft.com/fwlink/?linkid=392708).

In the Windows Server 2012 R2 AD FS role service, the federation service functions as a security token service, provides the security tokens that are used with claims, and gives you the ability to support multifactor authentication. The federation service issues tokens based on the credentials that are presented. After the account store verifies a user's credentials, the claims for the user are generated according to the rules of the trust policy and then added to a security token that is issued to the client. For more information about claims, see [Understanding Claims](https://go.microsoft.com/fwlink/?linkid=392709).

### Co-Existing with Other Versions of Exchange

It's possible to use AD FS authentication for Outlook Web App and EAC when you have more than one version of Exchange deployed in your organization. This scenario is only supported for Exchange 2010 and Exchange 2013 deployments, and only if all clients are connecting through the Exchange 2013 servers **and** those Exchange 2013 servers have been configured for AD FS authentication.

Users with a mailbox on an Exchange 2010 Server can access their mailboxes through an Exchange 2013 server configured for AD FS authentication. The initial client connection to the Exchange 2013 server will use AD FS authentication. The proxied connection to the Exchange 2010 server, however, will use Kerberos. There is no supported way to configure Exchange Server 2010 for direct AD FS authentication.
