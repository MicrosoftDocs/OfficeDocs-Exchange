---
title: 'Release notes for Exchange 2013: Exchange 2013 Help'
TOCTitle: Release notes for Exchange 2013
ms:assetid: 1879fd5e-3d63-4264-9cc2-9c050c6ab3c5
ms:mtpsurl: https://technet.microsoft.com/library/JJ150489(v=EXCHG.150)
ms:contentKeyID: 47559950
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Release notes for Exchange 2013

_**Applies to:** Exchange Server 2013_

Welcome to Microsoft Exchange Server 2013\! This topic contains important information that you need to know to successfully deploy Exchange 2013. Please read this topic completely before beginning your deployment.

This topic contains the following sections:

- Setup and deployment

- Exchange Management Shell

- Mailbox

- Public folders

- Mail flow

- Client connectivity

- Exchange 2010 coexistence

## Setup and deployment

- **msExchProductId doesn't reflect release version of Exchange 2013 installed** After Exchange extends your Active Directory schema and prepares Active Directory for Exchange, several properties are updated to show that preparation is complete. One of these properties is *msExchangeProductId* under the `CN=<your organization>, CN=Microsoft Exchange, CN=Services, CN=Configuration, DC=<domain>` container in the `Configuration` naming context. If no Active Directory schema changes are introduced in the release of Exchange 2013 you're installing, this property won't be updated or may show an unexpected value. This could cause confusion if the value doesn't match the version of Exchange 2013 being installed.

  This behavior is expected as the value of *msExchProductId* doesn't reflect the version of Exchange 2013 being installed. This property reflects the version of Exchange 2013 that last made changes to the Active Directory schema. To avoid confusion, we recommend that you follow the steps in the [How do you know this worked?](prepare-active-directory-and-domains-exchange-2013-help.md) section of [Prepare Active Directory and domains](prepare-active-directory-and-domains-exchange-2013-help.md) to verify that your Active Directory has been updated and is ready for the release of Exchange 2013 you're installing.

- **Setup incorrectly requests .NET Framework 4.0**: If you try to install Exchange 2013 without .NET Framework installed on the computer, Setup incorrectly requests that you install .NET Framework 4.0 when, in fact, .NET Framework 4.5 or later is required.

  To work around this issue, install .NET Framework 4.5 or later. You don't need to install .NET Framework 4.0. For a complete list of prerequisites, see [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

- **Exchange XML application configuration files are overwritten during cumulative update installation**: Any customized Exchange or Internet Information Server per-server settings you make in Exchange XML application configuration files, for example, web.config files on Client Access servers or the EdgeTransport.exe.config file on Mailbox servers, will be overwritten when you install an Exchange Cumulative Update or Service Pack. Make sure that you save this information so you can easily re-configure your server after the install. You must re-configure these settings after you install an Exchange Cumulative Update or Service Pack.

- **Installing Exchange using Delegate Admin permissions causes Setup to fail** When a user who's a member of only the Delegated Setup role group attempts to install Exchange on a pre-provisioned server, Setup will fail. This happens because the Delegated Setup group lacks the permissions required to create and configure certain objects in Active Directory.

  To work around this issue, do one of the following:

  - Add the user installing Exchange to the Domain Admins Active Directory security group.

  - Install Exchange using a user that's a member of the Organization Management role group.

For more information about how to install Exchange 2013, see [Planning and deployment](planning-and-deployment-for-exchange-2013-installation-instructions.md).

## Exchange Management Shell

- **The Shell unexpectedly loads Exchange 2007 or Exchange 2010 cmdlets** Previously, opening the Shell on an Exchange 2013 server would result in the Shell opening a connection to the local server or another server running Exchange 2013. When the connection is made, Exchange 2013 cmdlets are loaded. Starting with Exchange 2013 CU11, the Shell will connect to the Exchange server where the logged on user's mailbox is located. If the logged on user doesn't have a mailbox, the Shell will connect to the server where the SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c} arbitration mailbox is located. The target server can be any supported version of Exchange. This means if the logged on user's mailbox (or the arbitration mailbox if the user has no mailbox) is located on an Exchange 2010 server, the Shell will connect to that server and load Exchange 2010 cmdlets. This may prevent you from performing certain tasks because Exchange 2010 cmdlets can't manage Exchange 2013 configuration or servers.

  Starting with Exchange 2013 CU11, this behavior is by design. To make sure the Shell loads Exchange 2013 cmdlets, move the logged on user's mailbox to Exchange 2013. If the logged on user doesn't have a mailbox, move the SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c} arbitration mailbox to an Exchange 2013 server.

  For details and information on how to move the arbitration mailbox, see [Exchange Management Shell and Mailbox Anchoring](https://techcommunity.microsoft.com/t5/exchange-team-blog/exchange-management-shell-and-mailbox-anchoring/ba-p/604653) on the Exchange Team blog.

## Mailbox

- **Mailbox servers running different versions of Exchange can be added to the same database availability group** The **Add-DatabaseAvailabilityGroupServer** cmdlet and the Exchange admin center incorrectly allow an Exchange 2013 server to be added to an Exchange 2016-based database availability group (DAG), and vice versa. Exchange supports adding only Mailbox servers running the same version (Exchange 2013 versus Exchange 2016, for example) to a DAG. Additionally, the Exchange admin center displays both Exchange 2013 and Exchange 2016 servers in the list of servers available to add to a DAG. This could allow an administrator to inadvertently add a server running an incompatible version of Exchange to a DAG (for example, adding an Exchange 2013 server to an Exchange 2016-based DAG).

  There is currently no workaround for this issue. Administrators must be diligent when adding a Mailbox server to a DAG. Add only Exchange 2013 servers to Exchange 2013-based DAGs, and only Exchange 2016 servers to Exchange 2016-based DAGs. You can differentiate each version of Exchange by looking at the **Version** column in the list of servers in the Exchange admin center. The following are the server versions for Exchange 2013 and Exchange 2016:

  - **Exchange 2013** 15.0 (Build xxx.xx)

  - **Exchange 2016** 15.1 (Build xxx.xx)

- **Mailbox size increase when migrating from previous Exchange versions**: When you move a mailbox from a previous version of Exchange to Exchange 2013, the mailbox size reported may increase 30 percent to 40 percent. Disk space used by the mailbox database has not increased, only the attribution of space used by each mailbox has increased. The increase in mailbox size is due to the inclusion of all item properties into quota calculations, providing a more accurate computation of space consumed by items within their mailbox. This increase may cause some users to exceed their mailbox size quotas when their mailbox is moved to Exchange 2013.

  To prevent users from exceeding their mailbox size quotas, increase the database or mailbox quota values to accommodate the new quota calculation. To configure database or mailbox quota values, use the *IssueWarningQuota*, *ProhibitSendQuota*, and *ProhibitSendReceiveQuota* parameters on the **Set-MailboxDatabase** and **Set-Mailbox** cmdlets, respectively.

- **Outlook 2007 and Outlook 2010 clients may be unable to download the Offline Address Book**: If the Offline Address Book (OAB) internal URL isn't accessible from the Internet, Outlook 2007 and Outlook 2010 clients may be unable to download the OAB.

  To work around this issue for Outlook 2007 and Outlook 2010 clients, make the OAB internal URL accessible from the Internet. Outlook 2013 isn't affected by this issue.

- **Installing Exchange 2013 in an existing Exchange organization may cause all clients to download the OAB**: Installing the first Exchange 2013 server into an existing Exchange 2007 or Exchange 2010 organization may cause all clients in the organization to download a new copy of the OAB, resulting in network saturation and server performance issues. This issue occurs because Exchange 2013 creates a new default OAB in the organization that supersedes the Exchange 2007 or Exchange 2010 OAB. Mailboxes that don't have a specific OAB assigned, or that are located on a mailbox database that doesn't have a specific OAB assigned, will download the new default OAB.

  To prevent clients from downloading a new copy of the OAB when Exchange 2013 is installed, assign an OAB to every mailbox or to the mailbox database the mailboxes are located on. This must be done prior to Exchange 2013 being installed in the organization.

- **Users may be routed to an OAB generation mailbox that's not responsible for the requested OAB**: Exchange 2013 CU5 and later CUs have changed how OABs are linked to OAB generation mailboxes. This change makes it possible for a user to be routed to an OAB generation mailbox that isn't responsible for the OAB that the user is requesting. This can happen if all of the following are true:

  - You have more than one OAB generation mailbox in your organization.

  - You upgrade the Mailbox servers that host OAB generation mailboxes before you upgrade your Client Access servers.

  - You're upgrading your Exchange 2013 servers from a release prior to CU5 to a later release (for example, upgrading from Exchange 2013 CU3 to Exchange 2013 CU6).

  - Your Client Access servers are running a release prior to CU5.

  To work around this issue, make sure that you upgrade your Client Access servers to Exchange 2013 CU6 or later before you upgrade your Mailbox servers. This will make sure the Client Access servers know how to proxy the requests to the OAB generation mailbox that is responsible for generating the user's OAB.

  To read more about the OAB changes in Exchange 2013 CU5, see [OAB Improvements in Exchange 2013 Cumulative Update 5](https://techcommunity.microsoft.com/t5/exchange-team-blog/oab-improvements-in-exchange-2013-cumulative-update-5/ba-p/586510).

## Public folders

- **Unauthorized senders can no longer send messages to mail-enabled public folders**: Prior to Exchange 2013 CU6, unauthorized senders could send messages to mail-enabled public folders. This allowed the possibility for external senders to send mail to mail-enabled public folders regardless of the permissions set on the public folder.

  Starting with Exchange 2013 CU6, if you want external senders to send mail to a mail-enabled public folders, the **Anonymous** user needs to be granted at least the **Create Items** permission. If you've set up mail-enabled public folders and haven't done this, external senders will receive a delivery failure notification and the messages won't be delivered to the mail-enabled public folder.

  You can use the Shell or Outlook to set the permissions on the Anonymous user. To read more about how to set permissions on the Anonymous user, see [Mail-enable or mail-disable a public folder](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/enable-or-disable-mail-for-public-folder).

- The maximum number of public folders that can be migrated to Exchange 2013 from legacy Exchange servers is 500,000. For more information about public folder migration, see [Use batch migration to migrate public folders to Exchange 2013 from previous versions](use-batch-migration-to-migrate-public-folders-to-exchange-2013-from-previous-versions-exchange-2013-help.md).

## Mail flow

- **TransportAgent cmdlets on Client Access servers require local Windows PowerShell**: An issue exists with the **\*-TransportAgent** cmdlets that prevents those cmdlets from installing, uninstalling, and managing transport agents on Client Access servers using the Exchange Management Shell. To install, uninstall, and manage transport agents on Client Access servers, you must manually load the Exchange Windows PowerShell snap-in and then run the **\*-TransportAgent** cmdlets. If you attempt to install, uninstall, or manage transport agents using the Exchange Management Shell, your changes will be applied to the Exchange 2013 Mailbox server you're connected to.

  To install, uninstall, or manage transport agents on Client Access servers, do the following on the Client Access server you want to manage:

  > [!WARNING]
  > Loading the <CODE>Microsoft.Exchange.Management.PowerShell.SnapIn</CODE> Windows PowerShell snap-in and running cmdlets other than the <STRONG>*-TransportAgent</STRONG> cmdlets is not supported and may result in irreparable damage to your Exchange deployment.<BR>You must be a local Administrator on the Client Access server where you want to install, uninstall, or manage transport agents. We do not support the modification of access control lists (ACLs) on Exchange files, directories, or Active Directory objects.

  > [!IMPORTANT]
  > Perform the following procedure on Client Access servers only. You don't need to load the Exchange&nbsp;Windows PowerShell snap-in if you want to manage transport agents on Mailbox servers.

  1. Open a new Windows PowerShell window.

  2. Run the following command.

     ```powershell
     Add-PSSnapin Microsoft.Exchange.Management.PowerShell.SnapIn
     ```

  3. Perform transport agent management tasks as normal.

  4. Repeat this procedure on each Client Access server you want to manage.

## Client connectivity

- **NTLM authentication fails for non-domain joined clients**: Authentication between a client, such as Windows Live Mail, and Exchange 2013 may fail when the following conditions are true:

  - Then authentication method the client uses is NTLM.

  - The computer isn't joined to the domain.

  To work around this issue, you can do one of the following:

  - Join the computer the client is running on to the domain.

  - Change the authentication type the client uses from NTLM to Basic Auth over TLS.

- **GSSAPI authentication fails when used with the Send-MailMessage cmdlet**: Generic Security Service Application Program Interface (GSSAPI) authentication may fail when the **Send-MailMessage** cmdlet, which is included with default installs of Windows PowerShell, is used to send authenticated mail to Exchange 2013. When this happens, you'll see an entry in the **Application** event log on the Exchange 2013 Client Access server that received the connection with the following information:

  - **Source**: MSExchangeFrontEndTransport

  - **Event ID**: 1035

  - **Description**: Inbound authentication failed with error `IllegalMessage` for Receive connector Client Frontend \<*server name*\>. The authentication mechanism is Gssapi. The source IP address of the client who tried to authenticate to Exchange is \[\<*client IP address*\>\].

  To work around this issue, you need to remove the `Integrated` authentication method from the client receive connector on your Exchange 2013 Client Access servers. To remove the `Integrated` authentication method from a client receive connector, run the following command on each Exchange 2013 Client Access server that could receive connections from computers running the **Send-MailMessage** cmdlet:

  ```powershell
  Set-ReceiveConnector "<server name>\Client Frontend <server name>" -AuthMechanism Tls, BasicAuth, BasicAuthRequireTLS
  ```

- **MAPI over HTTP may experience poor performance when you upgrade to Exchange 2013 SP1**: If you upgrade from an Exchange 2013 cumulative update to Exchange 2013 SP1 and enable MAPI over HTTP, clients that connect to an Exchange 2013 SP1 server using the protocol may experience poor performance. This is because required settings aren't configured during an upgrade from a cumulative update to Exchange 2013 SP1. This issue doesn't occur if you upgrade to Exchange 2013 SP1 from Exchange 2013 RTM or if you install a new Exchange 2013 SP1 or later server.

  > [!NOTE]
  > This is only an issue if the MAPI over HTTP protocol is enabled on your Client Access servers. It's disabled by default. If MAPI over HTTP is disabled, clients use the RPC over HTTP protocol instead.

  To work around this issue, do the following:

  1. On servers running the Client Access server role, run the following commands in a Windows Command Prompt:

     ```powershell
     set AppCmdLocation=%windir%\System32\inetsrv
     set ExchangeLocation=%ProgramFiles%\Microsoft\Exchange Server\V15

     %AppCmdLocation%\appcmd.exe SET AppPool "MSExchangeMapiFrontEndAppPool" /CLRConfigFile:"%ExchangeLocation%\bin\MSExchangeMapiFrontEndAppPool_CLRConfig.config"
     %AppCmdLocation%\appcmd.exe RECYCLE AppPool "MSExchangeMapiFrontEndAppPool"
     ```

  2. On servers running the Mailbox server role, run the following commands in a Windows Command Prompt:

     ```powershell
     set AppCmdLocation=%windir%\System32\inetsrv
     set ExchangeLocation=%ProgramFiles%\Microsoft\Exchange Server\V15

     %AppCmdLocation%\appcmd.exe SET AppPool "MSExchangeMapiMailboxAppPool" /CLRConfigFile:"%ExchangeLocation%\bin\MSExchangeMapiMailboxAppPool_CLRConfig.config"
     %AppCmdLocation%\appcmd.exe RECYCLE AppPool "MSExchangeMapiMailboxAppPool"

     %AppCmdLocation%\appcmd.exe SET AppPool "MSExchangeMapiAddressBookAppPool" /CLRConfigFile:"%ExchangeLocation%\bin\MSExchangeMapiAddressBookAppPool_CLRConfig.config"
     %AppCmdLocation%\appcmd.exe RECYCLE AppPool "MSExchangeMapiAddressBookAppPool"
     ```

## Exchange 2010 coexistence

- **Requests to access Exchange 2010 mailboxes may not work when proxied through Exchange 2013 Client Access servers**: In some situations, the proxy request between the Exchange 2013 and Exchange 2010 Service Pack 3 (SP3) Client Access servers without any update rollups installed may not work correctly and an error appears. This can happen if all of the following conditions are true:

  - A user with an Exchange 2013 mailbox tries to open an Exchange 2010 mailbox using one of the following methods:

    - The **Open Another Mailbox** option in Outlook Web App **-OR-**

    - The **Another user** option in the Exchange admin center

    - The Client Access server the user connected to is running Exchange 2013.

    - The Exchange 2010 Client Access server was upgraded to Exchange 2010 SP3 from the release to manufacturing (RTM) version of Exchange 2010 or a previous Exchange 2010 service pack.

  If all the conditions above are true, the user won't be able to access the other user's Exchange 2010 Outlook Web App options and a blank page may appear.

  To work around this issue, install Exchange 2010 SP3 Update Rollup 1 or later on each Exchange 2010 server.
