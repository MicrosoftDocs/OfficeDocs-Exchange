---
title: 'Configure federated sharing between Exchange organizations'
TOCTitle: Configuring federated sharing between Exchange organizations
ms:assetid: 94e31454-b027-4757-b52f-d3c2ead6d916
ms:mtpsurl: https://technet.microsoft.com/library/JJ657473(v=EXCHG.150)
ms:contentKeyID: 49289355
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configuring federated sharing between Exchange organizations

_**Applies to:** Exchange Server 2013_

With federated sharing, users in your on-premises Exchange organization can share free/busy calendar information with recipients in other Exchange organizations that are also configured for federated sharing. Free/busy sharing can be enabled between two organizations running Exchange 2013 and also between organizations with a mixed Exchange deployment. To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md).

This topic provides a summary of the requirements and configuration steps necessary to enable free/busy sharing between different types of the following common Exchange deployments:

- Two Exchange 2013 organizations.

- An Exchange 2013 organization and an Exchange 2010 SP2 organization.

- An Exchange 2007 organization (or mixed Exchange 2007 and Exchange 2010 SP2 organization) and an Exchange 2013 organization.

- An Exchange 2003 organization (or mixed Exchange 2003 and Exchange 2010 SP2 organization) and an Exchange 2013 organization.

For additional management tasks related to federated sharing, see [Federation procedures](federation-procedures-exchange-2013-help.md).

> [!IMPORTANT]
> This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see [Office 365 operated by 21Vianet](https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china)..

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 hours.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- Before you perform the procedures in this topic, make sure you understand the limitations associated with sharing free/busy information across Exchange organizations. For details, see [Limitations of free/busy sharing](sharing-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Configure free/busy sharing between Exchange 2013 organizations

Complete the steps in [Configure federated sharing](configure-federated-sharing-exchange-2013-help.md) for both organizations.

## Configure free/busy sharing between Exchange 2013 and Exchange 2010 SP2 organizations

- Configure federated sharing for the Exchange 2013 organization. Complete the steps in [Configure federated sharing](configure-federated-sharing-exchange-2013-help.md).

- Configure federated delegation (previous name for federated sharing) for the Exchange 2010 SP2 organization. Complete the steps in [Configure federated delegation](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff601760(v=exchg.141)).

## Configure free/busy sharing between Exchange 2013 and Exchange 2007 organizations

- Configure federated sharing for the Exchange 2013 organization. Complete the steps in [Configure federated sharing](configure-federated-sharing-exchange-2013-help.md).

- Complete the following steps in the Exchange 2007 organization:

  1. **Add an Exchange 2010 SP2 server**

     An Exchange 2010 SP2 server with the Client Access server role must be installed in the Exchange 2007 organization. If you have existing Exchange 2010 servers, they should also be updated to Exchange 2010 SP2. For information about installing Exchange 2010 in an Exchange 2007 organization, see [Exchange 2007 - Planning Roadmap for Upgrade and Coexistence](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/dd638158(v=exchg.141)).

  2. **Configure federated delegation**

     Configure federated delegation for the Exchange 2007 organization. On an Exchange 2010 SP2 server in the Exchange 2007 organization, complete the steps in [Configure federated delegation](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff601760(v=exchg.141)).

  3. **Configure Active Directory synchronization**

     Active Directory synchronization must be configured for all users that need to share free/busy information between the organizations. You can either configure the Active Directory synchronization manually or use an automated Active Directory synchronization service. To configure Active Directory synchronization, see the steps below:

     - **Prerequisites**: Make sure your organization meets the requirements for installing Active Directory synchronization.

       To learn more, see [Prepare for Active Directory Synchronization](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization)

     - **Plan**: Understand the Microsoft Online Services Directory Synchronization tool and installation roadmap.

       To learn more, see [Prerequisites for Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites)

     - **Install and Configure**: Configure Active Directory synchronization between your on-premises organization and the Microsoft 365 or Office 365 organization.

       To learn more, see [Select which installation type to use for Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation).

  4. **Create an availability address space**

     Create an availability address space for the remote Exchange 2013 organization that directs availability requests from Exchange 2007 mailbox users to the Exchange 2010 SP2 Client Access server in the Exchange 2007 organization. This setting enables user availability requests from Exchange 2007 users for users in the remote Exchange 2013 organization to be proxied through the Exchange 2010 Client Access server in the Exchange 2007 organization. The Exchange 2010 Client Access server in the Exchange 2007 organization uses the federation trust and organization relationship to send the availability requests to the remote Exchange 2013 organization forest availability endpoint.

     To configure the availability address space, on the Exchange 2010 Client Access server in the Exchange 2007 organization, run the following command in the Exchange Management Shell:

     ```powershell
     Add-AvailabilityAddressSpace -AccessMethod InternalProxy -ProxyUrl https://<Exchange 2010 CAS server name>/ews/exchange.asmx -ForestName <SMTP domain of the remote Exchange organization> -UseServiceAccount $True
     ```

     For detailed syntax and parameter information, see [Add-AvailabilityAddressSpace](https://docs.microsoft.com/powershell/module/exchange/Add-AvailabilityAddressSpace)

## Configure Free/Busy Sharing Between Exchange 2013 and Exchange 2003 Organizations

- Configure federated sharing for the Exchange 2013 organization. Complete the steps in [Configure federated sharing](configure-federated-sharing-exchange-2013-help.md).

- Complete the following steps in the Exchange 2003 organization:

  1. **Add Exchange 2010 SP2 server**.

     An Exchange 2010 SP2 server with the Client Access server role must be installed in the Exchange 2003 organization. If you have existing Exchange 2010 servers, they should also be updated to Exchange 2010 SP2. For information about installing Exchange 2010 in an Exchange 2003 organization, see [Exchange 2003 - Planning Roadmap for Upgrade and Coexistence](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/aa998186(v=exchg.141)).

     > [!IMPORTANT]
     > For free/busy sharing to work properly between Exchange 2013 and Exchange 2003 organizations, the **OU=EXTERNAL (FYDIBOHF25SPDLT)** public folder must exist in the public folder hierarchy. This folder is automatically created on the Exchange 2010 Mailbox server in the Exchange 2003 organization only if you select the option to create public folders as part of configuring client settings for Outlook 2003 support during Exchange 2010 Setup. Additionally, this option is presented during the setup process only if the Exchange 2010 Mailbox server is the first Mailbox server installed in the organization. If the **OU=EXTERNAL (FYDIBOHF25SPDLT)** public folder wasn't created during Setup, you must manually create it. For details about how to create this public folder, see [How to troubleshoot free/busy issues in a hybrid deployment of on-premises Exchange Server and Exchange Online in Office 365](https://support.microsoft.com/help/2555008/how-to-troubleshoot-free-busy-issues-in-a-hybrid-deployment-of-on-prem).

  2. **Configure federated delegation**.

     Configure federated delegation for the Exchange 2003 organization. On an Exchange 2010 SP2 server in the Exchange 2003 organization, complete the steps in [Configure federated delegation](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff601760(v=exchg.141)).

  3. **Configure Active Directory synchronization**.

     Active Directory synchronization must be configured for all users that need to share free/busy information between the organizations. You can either configure the Active Directory synchronization manually or use an automated Active Directory synchronization service. To learn more about Active Directory synchronization, see [Forefront Identity Management](https://technet.microsoft.com/library/ff684076.aspx).

     - **Prerequisites**: Make sure your organization meets the requirements for installing Active Directory synchronization.

       To learn more, see [Prepare for Active Directory Synchronization](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization)

     - **Plan**: Understand the Microsoft Online Services Directory Synchronization tool and installation roadmap.

       To learn more, see [Prerequisites for Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites)

     - **Install and Configure**: Configure Active Directory synchronization between your on-premises organization and the Microsoft 365 or Office 365 organization.

       [Select which installation type to use for Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation).

  4. **Configure public folders for free/busy sharing in your Exchange 2003 organization.**

     Complete the following steps on an Exchange 2003 server:

     1. In Exchange System Manager, in the console tree, navigate to **Administrative Groups** \> **First Administrative Group** \> **Servers**.

     2. Select your Exchange 2003 server, and then navigate to **First Storage Group** \> **Public Folder Store** \> **Public Folders** \> **Schedule+ FREE BUSY**.

     3. In the action pane, select the **OU=EXTERNAL (FYDIBOHF25SPDLT)** folder for the **First Administrative Group**.

     4. Right-click the **OU=EXTERNAL (FYDIBOHF25SPDLT)** folder, and then click **Properties**.

     5. In **OU=EXTERNAL (FYDIBOHF25SPDLT) Properties**, select the **Replication** tab.

        - To replicate the **OU=EXTERNAL (FYDIBOHF25SPDLT)** folder to the Exchange 2010 Client Access/Mailbox server, click **Add**.

        - In **Select a Public Folder Store**, select the **Public Folder Database** for the Exchange 2010 Client Access/Mailbox server, and then click **OK**.

        > [!NOTE]
        > By default, Exchange uses the replication schedule set on the public folder database.

        - Click **OK** to close **OU=EXTERNAL (FYDIBOHF25SPDLT) Properties** and save your changes.

     6. Complete the same steps above for the **OU=Exchange Administrative Group (FYDIBOHF23SPDLT)** folder.

        > [!WARNING]
        > Depending on the size of your public folders, this replication could take several hours to complete.

     7. After the **OU=EXTERNAL (FYDIBOHF25SPDLT)** and **OU=Exchange Administrative Group (FYDIBOHF23SPDLT)** public folders have replicated to the Exchange 2010 Client Access/Mailbox server, you must remove the replicas for these public folders on the Exchange 2003 server.

  5. **Modify the LegacyExchangeDN parameter**

     Modify the *LegacyExchangeDN* parameter on all mail-enabled objects in the Exchange 2003 organization that reference the remote Exchange 2013 organization. Change the existing organizational unit (OU) value for the mail-enabled object to **External (FYDIBOHF25SPDLT)**. For example, **LegacyExchangeDN=/o=First Organization/ou=External (FYDIBOHF25SPDLT)/cn=Recipients/cn=User Name**.

     To modify mail-enabled objects in the Exchange 2003 organization, you can use the [Active Directory Service Interfaces Editor (ADSI Edit) tool](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc773354(v=ws.10)).
