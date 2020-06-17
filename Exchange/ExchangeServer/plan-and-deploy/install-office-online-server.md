---
localization_priority: Critical
description: 'Summary: Improve the attachment experience for Outlook on the web (formerly known as Outlook Web App) users by installing Office Online Server.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9c2b4186-be7d-4c57-b119-17a1c36fd6a0
ms.reviewer: 
title: Install Office Online Server in an Exchange organization
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Install Office Online Server in an Exchange organization

An optional prerequisite for Exchange 2016 Cumulative Update 1 (CU1) or later, as well as for Exchange 2019, is the installation of Office Online Server on one or more servers in your organization. Office Online Server enables users to view supported file attachments within Outlook on the web (formerly known as Outlook Web App) without downloading them first and without having a local installation of the program. Without Office Online Server installed, Outlook users need to download attachments to their local computer and then open them in a local application.

> [!NOTE]
> Office Online Server is available for download as part of a volume licensing agreement. If you don't have a volume license agreement, you can skip the instructions in this step. However, without Office Online Server installed, Outlook users will need to download attachments to their local computer to view them; they won't be able to view them in Outlook.

You can configure an Office Online Server endpoint in two places in Exchange 2016 and later: at the organization level, and at the Mailbox server level. Where you configure the endpoint depends on the size of your organization and the location of your servers and users.

- **Organization**: There are a couple of reasons why you might configure the Office Online Server endpoint at the organization level:

  - **Single-server or single-location deployment**: You can configure the endpoint at the organization level if all of your Exchange 2016 Mailbox servers are in the same location and you don't plan on having geographically distributed Office Online Server servers.

  - **Fallback for large deployments**: You can configure endpoint at the organization level as a fallback if the endpoint configured on a Mailbox server isn't available. If an Office Web Apps server isn't available, the client will try to connect to the endpoint configured at the organization level.

    **Notes**:

    - If you have Exchange 2013 servers in your organization, don't configure an endpoint at the organization level. Doing so will direct Exchange 2013 servers to use the Office Online Server server. This isn't supported.

    - Previewing attachments in S/MIME messages in Outlook on the web isn't supported by Office Online Server.

- **Mailbox server**: If you want to distribute client requests between two or more Office Online Server servers, if you want to geographically distribute Office Online Server servers, or if you have Exchange 2013 in your organization, configure the endpoints at the Exchange Mailbox server level. When you configure an endpoint at the server level, mailboxes located on that server will send requests to the configured Office Online Server server.

If you want users outside of your network to view supported file attachments in Outlook, Office Online Server needs to be accessible from the Internet. TCP port 443 needs to be opened on your firewall and forwarded to the Office Online Server server. If you deploy more than one Office Online Server server, each server needs its own fully qualified domain name (FQDN). Each server also needs to be accessible from the Internet via TCP port 443.

## Office Online Server system requirements

To set up Office Online Server, you will need the following:

- Windows Server 2012 R2 or Windows Server 2016

- Exchange 2016 Cumulative Update 1 (CU1) or later, or Exchange 2019

   > [!NOTE]
   > If you're running Windows Server 2016, you will need Exchange 2016 CU3 or later, as detailed in [Exchange Server prerequisites](prerequisites.md).

- [Microsoft .NET Framework 4.5.2](https://www.microsoft.com/download/details.aspx?id=42643)

- [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

- [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)

- [Microsoft.IdentityModel.Extention.dll](https://download.microsoft.com/download/0/1/D/01D06854-CA0C-46F1-ADBA-EBF86010DCC6/MicrosoftIdentityExtensions-64.msi)

- All available Windows updates installed

> [!NOTE]
>
> Office Online Server can't be installed on an Exchange server, SharePoint server, Active Directory domain controller, or any other computer with existing applications installed.

## Install Office Online Server

1. To install Office Online Server, follow Steps 1 through 3 in the section **Prepare servers to run Office Online Server** of the article [Deploy Office Online Server](https://docs.microsoft.com/officeonlineserver/deploy-office-online-server#prepare-servers-to-run-office-online-server) before proceeding.

5. Obtain and import an SSL certificate with the fully qualified domain name(s) (FQDN) of the Office Online Server server. If your organization is configured for split DNS, you only need to configure one FQDN on the certificate. For example, oos.contoso.com. If you have different internal and external FQDNs, you'll need to configure both FQDNs on the certificate. For example, oos.internal.contoso.com and oos.contoso.com.

6. Configure DNS records to point the FQDN(s) on the certificate to your Office Online Serverserver. If you have different DNS servers for internal and external users, you'll need to configure the appropriate FQDN on each server.

7. Open Windows PowerShell and run the following commands. When you run the commands, replace the example FQDNs and certificate friendly name with your own.

   ```powershell
   New-OfficeWebAppsFarm -InternalURL "https://oos.contoso.com" -ExternalURL "https://oos.contoso.com" -CertificateName "Office Online Server Preview Certificate"`
   ```

    > [!NOTE]
    > You can configure different internal and external URLs, but in the next step you'll see that you can only configure one URL for Exchange. In this case, if you use the internal URL in the next step, this function will only work internally and external users will get an unexpected error. If you use the external URL, this function will only work for external users and internal users will get an unexpected error.

## Configure the Office Online Server endpoint at the Mailbox server level

After you've configured the Office Online Server server, do the following on your Exchange 2016 server. This will allow Outlook to send requests to the Office Online Server server.

1. Open the Exchange Management Shell and run the following command. Replace the example server name and URL with your own.

   ```powershell
   Set-MailboxServer MBX -WacDiscoveryEndpoint "https://oos.contoso.com/hosting/discovery"
   ```

2. Restart the MsExchangeOwaAppPool by running the following command.

   ```powershell
   Restart-WebAppPool MsExchangeOwaAppPool
   ```

## Configure the Office Online Server endpoint at the organization level

After you've configured the Office Online Server server, do the following on your Exchange 2016 server. This will allow Outlook to send requests to the Office Online Server server.

1. Open the Exchange Management Shell and run the following command. Replace the example URL with your own.

   ```powershell
   Set-OrganizationConfig -WacDiscoveryEndpoint "https://oos.internal.contoso.com/hosting/discovery"
   ```

   > [!IMPORTANT]
   > If you have Exchange 2013 servers in your organization, don't configure an endpoint at the organization level. Doing so will direct Exchange 2013 servers to use the Office Online Server server. This isn't supported.

2. Restart the MsExchangeOwaAppPool by running the following command.

   ```powershell
   Restart-WebAppPool MsExchangeOwaAppPool
   ```
