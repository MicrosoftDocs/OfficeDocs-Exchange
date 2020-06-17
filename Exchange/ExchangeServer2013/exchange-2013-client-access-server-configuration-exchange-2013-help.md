---
title: 'Exchange 2013 Client Access server configuration: Exchange 2013 Help'
TOCTitle: Exchange 2013 Client Access server configuration
ms:assetid: 01432ae4-2a00-44a4-a4dd-4eb8d7e6cfae
ms:mtpsurl: https://technet.microsoft.com/library/Hh529912(v=EXCHG.150)
ms:contentKeyID: 49668969
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 Client Access server configuration

_**Applies to:** Exchange Server 2013_

After you've installed the Exchange 2013 Client Access server, there are a variety of configuration tasks that you can perform. Although the Client Access server in Exchange 2013 doesn't handle processing for the client protocols, several settings need to be applied to the Client Access server, including virtual directory settings and certificate settings.

## Configuring server certificates

In Exchange 2013, you can use the Certificate Wizard to request a digital certificate from a certification authority. After you've requested a digital certificate, you'll need to install it on the Client Access server.

You don't need to install digital certificates on the Mailbox servers in your organization. A self-signed certificate is installed by default on the Mailbox servers, and it doesn't need to be replaced. The Client Access servers in your organization implicitly trust the self-signed certificate on the Mailbox servers. For more information, see [Exchange 2013 certificate management UI](exchange-2013-certificate-management-ui-exchange-2013-help.md).

## Configuring virtual directories

There are several settings that you can configure on the virtual directories for the Offline Address Book (OAB), Exchange Web Services, Exchange ActiveSync, Outlook Web App, and the Exchange admin center. For additional information about virtual directory management, see [Virtual directory management](virtual-directory-management-exchange-2013-help.md). You can configure the virtual directories using the following commands.

- Exchange 2013 provides two sets of HTTP connectivity settings for Outlook Anywhere configuration so that administrators may configure both an internal and external endpoint.

  To configure Outlook Anywhere with a single URL for connectivity, you must provide the host name, indicate whether SSL is required, and specify an authpackage using the following command in the Exchange Management Shell:

  ```powershell
  Get-OutlookAnywhere | Set-OutlookAnywhere -InternalHostname "internalServer.contoso.com" -InternalClientAuthenticationMethod Ntlm -InternalClientsRequireSsl $true -IISAuthenticationMethods Negotiate,NTLM,Basic
  ```

  You may also specify an externally reachable endpoint by using the following command in the Exchange Management Shell:

  ```powershell
  Get-OutlookAnywhere | Set-OutlookAnywhere -InternalHostname "internalServer.contoso.com" -InternalClientAuthenticationMethod Ntlm -InternalClientsRequireSsl $true -ExternalHostname "externalServer.company.com" -ExternalClientAuthenticationMethod Basic -ExternalClientsRequireSsl $true -IISAuthenticationMethods Negotiate,NTLM,Basic
  ```

  > [!TIP]
  > While Exchange 2013 supports Negotiate for Outlook Anywhere HTTP authentication, this should only be used when all servers in the environment are running Exchange 2013.

- To configure Exchange ActiveSync, run the following command.

   ```powershell
   Set-ActiveSyncVirtualDirectory -Identity "<CAS2013>\Microsoft-Server-ActiveSync (Default Web Site)" -ExternalUrl "https://mail.contoso.com/Microsoft-Server-ActiveSync"
   ```

- To configure the Exchange Web Services virtual directory, run the following command.

  ```powershell
  Set-WebServicesVirtualDirectory -Identity "<CAS2013>\EWS (Default Web Site)" -ExternalUrl https://mail.contoso.com/EWS/Exchange.asmx
  ```

- To configure the Offline Address Book, run the following command.

  ```powershell
  Set-OABVirtualDirectory -Identity "<CAS2013>\OAB (Default Web Site)" -ExternalUrl "https://mail.contoso.com/OAB"
  ```

- To configure the Service Connection Point, run the following command.

  ```powershell
  Set-ClientAccessServer -Identity <CAS2013> -AutoDiscoverServiceInternalURI https://autodiscover.contoso.com/AutoDiscover/AutoDiscover.xml
  ```

## Upgrade from Exchange 2007 and 2010 Client Access

Use this section to help you configure external access to protocols on the Exchange 2013 Client Access server. Run the Exchange Management Shell commands in the configuring virtual directories section above, as well as the commands below.

You'll have to run the following commands to configure the virtual directories for Exchange 2013.

1. To configure an external URL for Outlook Web App, run the following command in Exchange Management Shell.

   ```powershell
   Set-OwaVirtualDirectory "<CAS2013>\OWA (Default Web Site)" -ExternalUrl https://mail.contoso.com/OWA
   ```

   Run the following commands at a command prompt after you set the Outlook Web App virtual directory.

   ```powershell
   Net stop IISAdmin /y
   ```

   ```powershell
   Net start W3SVC
   ```

2. To configure external EAC access, run the following command in Exchange Management Shell.

   ```powershell
   Set-EcpVirtualDirectory "<CAS2013>\ECP (Default Web Site)" -ExternalUrl https://mail.contoso.com/ECP -InternalURL https://mail.contoso.com/ECP
   ```

3. To configure the Availability service, run the following command in Exchange Management Shell.

   ```powershell
   Set-WebServicesVirtualDirectory -Identity "<CAS2013>\EWS (Default Web Site)" -ExternalURL https://mail.contoso.com/EWS/Exchange.asmx
   ```

To verify that the external URL has been configured correctly for Exchange ActiveSync or Outlook Web App, you can use the Remote Connectivity Analyzer, a free Web-based tool provided by Microsoft. You can find the Remote Connectivity Analyzer [here](https://testconnectivity.microsoft.com/tests/o365).

To verify that authentication has been configured correctly for Exchange ActiveSync or Outlook Web App, you can also use ExRCA.

To verify that direct file access has been configured correctly for Outlook Web App, log on as a user to Outlook Web App using the public computer option and then try to access and save a file attached to an email message.

## Configure protocols on the Exchange 2007 Client Access servers

You'll have to run the following commands to configure the virtual directories for Exchange 2007.

- To configure the external URL on the Exchange ActiveSync virtual directory, run the following command in Exchange Management Shell.

  ```powershell
  Set-ActiveSyncVirtualDirectory -Identity "<CAS2007>\Microsoft-Server-ActiveSync (Default Web Site)" -ExternalUrl https://mail.contoso.com/Microsoft-Server-ActiveSync
  ```

- To configure the external URL on the Outlook Web App virtual directory, run the following command in Exchange Management Shell.

  ```powershell
  Set-OwaVirtualDirectory -Identity "<CAS2007>\owa (Default Web Site)" -ExternalUrl https://legacy.contoso.com/owa
  ```
