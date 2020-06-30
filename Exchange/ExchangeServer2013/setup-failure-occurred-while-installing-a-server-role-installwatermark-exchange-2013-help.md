---
title: 'Setup failure occurred while installing a server role'
TOCTitle: Setup failure occurred while installing a server role_InstallWatermark
ms:assetid: ad89ebd5-f9bb-40c1-8811-09b145c2b341
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.installwatermark(v=EXCHG.150)
ms:contentKeyID: 46629079
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Setup failure occurred while installing a server role\_InstallWatermark

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because a previous setup failure occurred while installing a server role.

Exchange 2007 setup requires that a failed server role installation be successfully re-installed, or removed from the setup process, before any other setup task can continue.

To address this issue, either reinstall just the failed server role(s), or remove the server role(s).

## To reinstall the failed server role from the command line

1. Open a Command Prompt window, and then navigate to the installation files.

2. Run the following command:

    ```powershell
    Setup /role:<Failed Server Role>
    ```

    Select from one or more of the following roles, in a comma-separated list:

    ClientAccess (or CA, or C)

    EdgeTransport (or ET, or E)

    > [!NOTE]
    > - The Edge Transport server role cannot coexist on the same computer with any other server role.
    >
    > - You must deploy the Edge Transport server role in the perimeter network and outside the Active Directory forest.

    HubTransport (or HT, or H)

    Mailbox (or MB, or M)

    UnifiedMessaging (or UM, or U)

    ManagementTools (or MT, or T)

    > [!NOTE]
    > If you specify ManagementTools, you will install the Exchange Management Console, the Exchange cmdlets for the Exchange Management Shell, the Exchange Help file, the Exchange Best Practices Analyzer Tool, and the Exchange Troubleshooting Assistant. If you install any other server role, the management tools will be installed automatically.

    For example, to add the Hub Transport server role to an existing Mailbox server, type the following: **%LocalExchangeInstallationDir%\\bin\\Setup.com /role:HubTransport /Mode:Install**

> [!NOTE]
> If any Exchange Server&nbsp;2007 server role previously installed successfully, the Setup wizard will run in maintenance mode. If no Exchange 2007 server roles were previously successfully installed, the Setup wizard will start from where it stopped.

## To use the Exchange Server 2007 Setup wizard in maintenance mode to reinstall the failed server role

1. Log on to the server for which you want to reinstall a server role.

2. Open Control Panel and then double-click **Add or Remove Programs**.

3. On the **Change or Remove Programs** page, select **Microsoft Exchange Server**, and then click **Change**.

4. In the Exchange Server 2007 Setup wizard, on the **Exchange Maintenance Mode** page, click **Next**.

5. On the **Server Role Selection** page, select the check boxes for the server roles that you want to install, and then click **Next**.

    > [!NOTE]
    > The Edge Transport server role cannot coexist on the same computer with any other server role. <br/><br/> You must deploy the Edge Transport server role in the perimeter network and outside the Active Directory forest. <br/><br/> If you select Management Tools, you will install the Exchange Management Console, the Exchange cmdlets for the Exchange Management Shell, and the Exchange Help file. The management tools will be installed automatically if you install any other server role.

6. If you selected **Hub Transport Role**, and if you are installing Exchange 2007 in a forest that has an existing Exchange Server 2003 or Exchange 2000 Server organization, on the **Mail Flow Settings** page, select a bridgehead server in the existing organization that is a member of the Exchange 2003 or Exchange 2000 routing group to which you want to create a routing group connector.

7. On the **Readiness Checks** page, view the status to determine if the organization and server role prerequisite checks completed successfully. If they have completed successfully, click **Install** to install Exchange 2007.

8. On the **Completion** page, click **Finish**.

## To use the Exchange Server 2007 Setup wizard to reinstall the failed server role when no other server role was previously successfully installed

1. Follow the guidance in "How to Perform a Custom Installation Using Exchange Server 2007 Setup" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb125143(v=exchg.80)](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb125143(v=exchg.80))) in the Exchange Server 2007 product documentation.

## To remove the failed server role

Follow the guidance in [How to Remove Exchange 2007 Server Roles](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb124115(v=exchg.80)) in the Exchange Server 2007 product documentation.

## For More Information

For more information about how to install Exchange 2007 in unattended mode, see "How to Install Exchange 2007 in Unattended Mode" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa997281(v=exchg.80)](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa997281(v=exchg.80))).
