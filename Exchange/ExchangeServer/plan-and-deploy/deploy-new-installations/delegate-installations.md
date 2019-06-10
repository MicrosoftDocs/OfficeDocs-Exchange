---
localization_priority: Normal
description: "Summary: Learn how to configure Exchange 2016 or Exchange 2019 server objects in Active Directory so users who aren't Exchange administrators can install Exchange."
ms.topic: get-started-article
author: chrisda
ms.author: chrisda
ms.assetid: f2fc8680-0c7c-4a29-b8f5-d77404fec280
ms.date:
ms.reviewer: 
title: Delegate the installation of Exchange servers
ms.collection:
- Strat_EX_Admin
- exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Delegate the installation of Exchange servers

In large companies, people who install and configure new Windows servers often aren't Exchange administrators. In Exchange 2016 and Exchange 2019, these users can still install Exchange on Windows servers, but only _after_ an Exchange administrator *provisions* the Exchange server object in Active Directory. Provisioning an Exchange server object makes all of the required Active Directory changes independently of the actual installation of Exchange on a server. An Exchange administrator can provision a new Exchange server object hours or even days before Exchange is installed.

After an Exchange administrator provisions the Exchange server object, the only requirement for installing Exchange on the server is membership in the [Delegated Setup](https://technet.microsoft.com/library/dd876881(v=exchg.150).aspx) role group, which allows members to install Exchange on provisioned servers. If this sounds like something you want to do, then this topic is for you.

## What do you need to know before you begin?

- Estimated time to complete this procedure: Less than 10 minutes.

- You can only provision an Exchange server from the command line (Unattended Setup). You can't use the Exchange Setup wizard.

- You can't provision the first Exchange server object in your organization for the installation of Exchange by a delegate. An Exchange administrator needs to install the first Exchange server in the organization. After that, you can provision _additional_ Exchange server objects so users who aren't Exchange administrators can install Exchange using delegated setup.

- A delegated user can't uninstall an Exchange server. To uninstall an Exchange server, you need to be an Exchange administrator.

- Download and use the latest available release of [Updates for Exchange Server](../../new-features/updates.md).

- To provision an Exchange server object, you need to be a member of the [Organization Management](https://technet.microsoft.com/library/dd335087(v=exchg.150).aspx) role group.

- You can provision the Exchange server object in Active Directory from the target server itself, or from another computer.

- Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Command Prompt to provision Exchange 2019 servers

1. In File Explorer, right-click on the Exchange ISO image file that you downloaded, and then select **Mount**. Note the virtual DVD drive letter that's assigned.

2. Open a Windows Command Prompt window. For example:

    - Press the Windows key + 'R' to open the **Run** dialog, type cmd.exe, and then press **OK**.

    - Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

3. In the Command Prompt window, use the following syntax:

    ```
    <Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms /NewProvisionedServer[:<ServerName>]
    ```

    If you run the command on the target server, you can use the _/NewProvisionedServer_ switch by itself. Otherwise, you need to specify the Name of the server to provision.

    This example uses the Exchange installation files on drive E: to provision the server Mailbox01:

    ```
    E:\Setup.exe /IAcceptExchangeServerLicenseTerms /NewProvisionedServer:Mailbox01
    ```

    This example uses the Exchange installation files on drive E: to provision the local server where you're running the command:

    ```
    E:\Setup.exe /IAcceptExchangeServerLicenseTerms /NewProvisionedServer
    ```

    **Note**: To remove a provisioned Exchange server object from Active Directory _before_ Exchange is installed on it, replace the _/NewProvisionedServer_ switch with _/RemoveProvisionedServer_.

4. Add the appropriate users to the Delegated Setup role group so they can install Exchange on the provisioned server. To add users to a role group, see [Add members to a role group](../../permissions/role-group-members.md#add). The delegates can use the procedures in [Install Exchange Mailbox servers using the Setup wizard](install-mailbox-role.md) to install Exchange on the provisioned server.

## How do you know this worked?

To verify that you've successfully provisioned an Exchange server for a delegate installation of Exchange, do the following steps:

1. In Active Directory Users & Computers, select **Microsoft Exchange Security Groups**, double-click **Exchange Servers**, and then select the **Members** tab.

2. On the **Members** tab, verify that the provisioned server is a member of the security group. A member of the Delegated Setup role group can now install Exchange on the server.

If your server is listed as a member of the Exchange Servers security group, it was properly provisioned. Someone who's a member of the Delegated Setup role group can now install Exchange on that server.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## More information

An Exchange administrator might need to complete the deployment by performing the tasks provided in [Exchange post-installation tasks](../../plan-and-deploy/post-installation-tasks/post-installation-tasks.md).

The high-level Active Directory changes that are made when you provision an Exchange server object are described in the following list:

- A server object is created in the **CN=Servers,CN=Exchange Administrative Group (FYDIBOHF23SPDLT),CN=Administrative Groups,CN=\<Organization Name\>,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=\<Root Domain\>** configuration partition.

- The following access control entries (ACEs) are added to the server object within the configuration partition for the Delegated Setup role group:

  - Full Control on the server object and its child objects

  - Deny access control entry for the Send As extended right

  - Deny access control entry for the Receive As extended right

  - Deny CreateChild and DeleteChild permissions for Exchange Public Folder Store objects

     **Note**: Public folders are administered at an organizational level; therefore, the creation and deletion of public folder stores is restricted to Exchange administrators.

- The Active Directory computer account for the server is added to the Exchange Servers group.

- The server is added as a provisioned server in the Exchange admin center (EAC).

Only members of the Organization Management role group in Exchange have the permissions required to make these changes to Active Directory.
