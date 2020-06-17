---
title: 'Configuring SSL offloading in Exchange 2013: Exchange 2013 Help'
TOCTitle: Configuring SSL offloading in Exchange 2013
ms:assetid: 654cc2c2-918b-48fc-9532-9c8e3012810d
ms:mtpsurl: https://technet.microsoft.com/library/Dn635115(v=EXCHG.150)
ms:contentKeyID: 61200287
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configuring SSL offloading in Exchange 2013

_**Applies to:** Exchange Server 2013_

The following helps you in configuring SSL offloading for the protocols and related services on Exchange 2013 Client Access servers with Service Pack 1 (SP1) installed. If you have multiple Client Access servers, you must perform the required steps for each protocol or service on every Client Access server with SP1 installed in your on-premises organization. That is not to mention that each Client Access server in your organization must be configured identically. If you are upgrading to newer Cumulative Updates (CUs) or service packs and you want to continue to use SSL offloading, you must perform the following steps again after you have upgraded or applied those updates on your Exchange 2013 Client Access servers.

One of the biggest advantages to SSL offloading is having the ability to more easily manage certificates that are used. Instead of having separate SSL certificates for each Client Access server with SP1 installed, a single SSL certificate is used and imported to all Client Access servers. The certificate used can be an existing or newly created SSL certificate.

> [!WARNING]
> When you use Internet Information Services (IIS) Manager, the Exchange Management Shell, or a command-line interface to configure SSL offloading, notice that there is a <STRONG>Default Web Site</STRONG> and an <STRONG>Exchange Back End</STRONG> site. For SSL offloading, only configure the <STRONG>Default Web Site</STRONG> and don't make any changes to the <STRONG>Exchange Back End</STRONG> site.

## What do you need to know before you begin?

- Install all of the required Client Access and Mailbox servers in your organization.

- Install Service Pack 1 (SP1) on each Client Access and Mailbox server in your organization. To download SP1, see [Updates for Exchange 2013](updates-for-exchange-2013-exchange-2013-help.md).

- Determine the required permissions for Exchange 2013 by seeing [Feature permissions](feature-permissions-exchange-2013-help.md).

- To see what permissions you need for Client Access servers, see "Client Access server permissions" in [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md).

- To see what permissions you need for Client Access servers, see "Outlook Web App permissions" in [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md).

- You might be able to use only the Shell to perform some procedures. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- To use an existing certificate on your Client Access servers and on the device you are terminating the SSL connections with, export the certificate with the private key on a Client Access server and import or install it on the device. For details, see [Export-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/Export-ExchangeCertificate).

- To use a new certificate, you must use EAC or the Shell to create, import, and enable the new certificate. For details, see [Exchange 2013 certificate management UI](exchange-2013-certificate-management-ui-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Configure SSL offloading for Outlook Web App

To enable SSL offloading for Outlook Web App, you need to remove the SSL requirement on the **owa** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **owa** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **owa** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

    ```powershell
    appcmd set config "Default Web Site/owa" /section:access /sslFlags:None /commit:APPHOST
    ```

- **Step 2**: You need to recycle the correct application pool or restart Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeOWAAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeOWAAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Configure SSL offloading for the Exchange admin center (EAC)

To enable SSL offloading for EAC, you need to remove the SSL requirement on the **ecp** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **ecp** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **ecp** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

    ```powershell
    appcmd set config "Default Web Site/ecp" /section:access /sslFlags:None /commit:APPHOST
    ```

- **Step 2**: You need to recycle the correct application pool or restart Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeECPAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeECPAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Configuring SSL offloading for Outlook Anywhere

SSL offloading for Outlook Anywhere is enabled by default. Outlook Anywhere clients can get email from a private or public network. By default, the internal host name or FQDN of the server is used to enable internal Outlook clients to connect. However, if Outlook Anywhere isn't used internally, then you should remove the internal host name. To allow both internal and external access for Outlook clients, you must configure the internal and external host names, set the authentication method for each, and set both the internal and external clients to require SSL. To configure the authentication method for the external clients, you can use EAC or the Exchange Management Shell, but for internal clients, you must use the Shell:

- **Step 1**: You can use EAC or the Shell if you haven't added an external host name for Outlook Anywhere:

  - Using EAC, go to **Servers**, select the name of the Client Access server in the list, and then click **Edit**. In the **Exchange Server** window, click **Outlook Anywhere**, and then in the **Specify the external host name (for example, contoso.com) that users will use to connect to your organization** box, enter the external host name. Verify that the **Allow SSL offloading** option is selected, and then click **Save**.

  - Using the Exchange Management Shell, click **Start**, and then on the **Start** menu, click **Exchange Management Shell**. In the window, type the following and then press Enter:

    ```powershell
    Set-OutlookAnywhere -Identity ClientAccessServer1\Rpc* -Externalhostname ClientAccessServer1.contoso.com -ExternalClientsRequireSsl:$True -ExternalClientAuthenticationMethod Basic
    ```

- **Step 2**: By default, SSL offloading is enabled. However, you can use EAC or the Exchange Management Shell if SSL offloading has been disabled and you want to enable it:

  - Using EAC, go to **Servers**, select the name of the Client Access server in the list, and then click **Edit**. In the **Exchange Server** window, click **Outlook Anywhere**, click the **Allow SSL offloading** option, and then click **Save**.

  - Using the Shell, type the following and then press Enter.

    ```powershell
    Set-OutlookAnywhere -Identity ClientAccessServer1\Rpc* -SSLOffloading $true
    ```

- **Step 3**: By default, **Require SSL** is not selected on the **Rpc** virtual directory, but if you want to verify that SSL is disabled, you can use Internet Information Services (IIS) Manager.

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **Rpc** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, verify that the **Require SSL** check box is cleared, and then click **Apply** in the **Actions** pane.

- **Step 4**: You need to recycle the correct application pool or restart Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeRpcProxyFrontEndAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeRpcProxyFrontEndAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

> [!IMPORTANT]
> You must wait for the Service Host process to apply any changes from Active Directory to Internet Information Services (IIS) every 15 minutes even if you restart IIS on a Client Access server.

## Configuring SSL offloading for the Offline Address Book (OAB)

To enable SSL offloading for the Offline Address Book (OAB), you need to remove the SSL requirement on the **OAB** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **OAB** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **OAB** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

    ```powershell
    appcmd set config "Default Web Site/OAB" /section:access /sslFlags:None /commit:APPHOST
    ```

- **Step 2**: You need to recycle the correct application pool or restart Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeOABAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeOABAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Configuring SSL offloading for Exchange ActiveSync (EAS)

To enable SSL offloading for Exchange ActiveSync (EAS), you need to remove the SSL requirement on the **Microsoft-Server-ActiveSync** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **Microsoft-Server-ActiveSync** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **Microsoft-Server-ActiveSync** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

    ```powershell
    appcmd set config "Default Web Site/MSExchangeSyncAppPool" /section:access /sslFlags:None /commit:APPHOST
    ```

- **Step 2**: You need to recycle the correct application pool or restart the Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeSyncAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeSyncAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Configuring SSL offloading for Exchange Web Services (EWS)

To enable SSL offloading for Exchange Web Services (EWS), you need to remove the SSL requirement on the **EWS** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **EWS** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **EWS** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

  ```powershell
  appcmd set config "Default Web Site/EWS" /section:access /sslFlags:None /commit:APPHOST
  ```

- **Step 2**: You need to recycle the correct application pool or restart Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeServicesAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeServicesAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Configuring SSL offloading for the Autodiscover service

To enable SSL offloading for the Autodiscover service, you need to remove the SSL requirement on the **Autodiscover** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **Autodiscover** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **Autodiscover** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

    ```powershell
    appcmd set config "Default Web Site/autodiscover" /section:access /sslFlags:None /commit:APPHOST
    ```

- **Step 2**: You need to recycle the correct application pool or restart Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeAutodiscoverAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeAutodiscoverAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Configuring SSL Offloading for the Mailbox Replication Proxy Service (MRSProxy)

The Mailbox Replication Proxy (MRSProxy) service is installed on every Exchange 2013 Client Access server. MRSProxy helps you to make cross-forest move requests on-premises as well as moving on-premises mailboxes to Office 365. However, by default, MRSProxy is disabled. If you are enabling it, you should enable it in the remote Exchange forest for cross-forest, on-premises mailbox moves or in the on-premises Exchange forest for moving a mailbox to Office 365. Although the MRSProxy service runs under Exchange Web Services (EWS), it's not supported to configure SSL offloading.

The reason for this is that the MRSProxy service expects traffic to be signed/encrypted. Any hardware load balancer or firewall must reencrypt the MRSProxy traffic before sending it to Client Access servers. If this is the case, it is recommended that you configure SSL bridging for offloading to work.

**Reverse SSL or SSL Bridging**: If you enable reverse SSL or SSL bridging on hardware load balancers, you won't need to perform the preceding steps on each CAS server. However, enabling reverse SSL on your hardware load balancers means that SSL encryption and decryption will stay with the Client Access servers. In this case, the SSL encryption and decryption will occur on both the hardware load balancers and the Client Access servers. Choosing to use Exchange 2013 SSL offloading or reverse SSL (SSL bridging) is dependent on the organizational goals and the security practices that must be implemented. The following picture shows client connectivity with SSL bridging (reverse SSL) enabled.

![SSL Bridging](images/Dn635115.a08aacc1-0ab4-46b3-bdae-b9518a3f5748(EXCHG.150).jpg "SSL Bridging")

## Configuring SSL offloading for Outlook clients (MAPI virtual directory)

To enable SSL offloading for Outlook clients, you need to remove the SSL requirement on the **MAPI** virtual directory on the **Default Web Site**:

- **Step 1**: You can use Internet Information Services (IIS) Manager or a command line to disable SSL on the **MAPI** virtual directory:

  - Using Internet Information Services (IIS) Manager, expand **Sites** \> **Default Web Site**, and then select the **MAPI** virtual directory. In the results pane under **IIS**, double-click **SSL Settings**. In the **SSL Settings** results pane, clear the **Require SSL** check box, and then click **Apply** in the **Actions** pane.

  - Using the command line, type the following and then press Enter.

    ```powershell
    appcmd set config "Default Web Site/MAPI" /section:access /sslFlags:None /commit:APPHOST
    ```

- **Step 2**: You need to recycle the correct application pool or restart the Internet Information Services by using one of the following methods:

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    appcmd Recycle AppPool MSExchangeMapiFrontEndAppPool
    ```

  - Using a Windows PowerShell cmdlet, type the following and then press Enter.

    ```powershell
    IIS:\>Restart-WebAppPool MSExchangeMapiFrontEndAppPool
    ```

  - Using a command line: Go to **Start** \> **Run**, type **cmd**, and then press Enter. In the Command Prompt window, type the following and then press Enter.

    ```powershell
    iisreset /noforce
    ```

  - Using Internet Information Services (IIS) Manager: In Internet Information Services (IIS) Manager, in the **Actions** pane, click **Restart**.

## Using a script to enable SSL offloading for all protocols and services

If you're working with a large organization with multiple Exchange 2013 Client Access servers, you might want to speed up the preceding steps that you went through. You can copy and paste the commands in either of the following scripts into Notepad, make any changes, save the file with a .ps1 extension, and then run it from the Exchange Management Shell. Depending on your needs, both of these scripts can be used to configure SSL offloading for all protocols and services for a single Client Access server or for multiple ones.

> [!NOTE]
> For the <STRONG>Set-OutlookAnywhere</STRONG> cmdlet entries, replace "MyServer" with the name of your Client Access server(s).

### Using Set-WebConfigurationProperty

```powershell
Set-OutlookAnywhere -Identity MyServer\Rpc* -Externalhostname MyServer.mail.contoso.com -ExternalClientsRequireSsl $True -ExternalClientAuthenticationMethod Basic
Set-OutlookAnywhere -Identity MyServer\Rpc* -SSLOffloading $true
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/OWA"
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/ecp"
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/EWS"
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/Autodiscover"
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/Microsoft-Server-ActiveSync"
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/OAB"
Set-WebConfigurationProperty -Filter //security/access -name sslflags -Value "None" -PSPath IIS: -Location "Default Web Site/MAPI"
```

```powershell
iisreset /noforce
```

### Using appcmd

> [!NOTE]
> For the <STRONG>Set-OutlookAnywhere</STRONG> cmdlet entries, replace "MyServer" with the name of your Client Access server(s).

```powershell
Set-OutlookAnywhere -Identity MyServer\Rpc* -Externalhostname MyServer.mail.contoso.com -ExternalClientsRequireSsl $True -ExternalClientAuthenticationMethod Basic
Set-OutlookAnywhere -Identity MyServer\Rpc* -SSLOffloading $true
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/owa" /section:access /sslFlags:None /commit:APPHOST
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/ecp" /section:access /sslFlags:None /commit:APPHOST
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/EWS" /section:access /sslFlags:None /commit:APPHOST
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/Autodiscover" /section:access /sslFlags:None /commit:APPHOST
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/Microsoft-Server-ActiveSync" /section:access /sslFlags:None /commit:APPHOST
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/OAB" /section:access /sslFlags:None /commit:APPHOST
&$env:systemroot\system32\inetsrv\appcmd set config "Default Web Site/MAPI" /section:access /sslFlags:None /commit:APPHOST
```

```powershell
iisreset /noforce
```

## Configuring coexistence with Exchange 2007 and Exchange 2010

During a coexistence scenario where you have a mix of Exchange 2003 and Exchange 2010 servers in the organization, one of the first steps you need to perform after deploying the Exchange 2010 Client Access Servers is to change DNS so that Exchange 2003 users access their mailboxes from a group of Exchange 2010 Client Access servers. In such a scenario, it's fully supported to enable SSL offloading on the load balancer used to distribute client traffic across the Client Access servers.

### Coexistence with other versions of Outlook Web App

With SSL offloading configured on the Exchange 2013 Client Access servers, coexistence works with Exchange 2007 and Exchange 2010:

- To coexist with Exchange 2007, an earlier namespace is required, and redirection will happen to it only for Outlook Web App and Exchange Web Services. Autodiscover, Outlook Anywhere, and Exchange ActiveSync will be proxied over to the earlier versions.

- To coexist with Exchange 2010, if you have the external URL set, a redirect will be used. If not, a proxy will be used.
