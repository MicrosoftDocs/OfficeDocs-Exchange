---
title: 'Configure UM to work with Lync Server: Exchange 2013 Help'
TOCTitle: Configure UM to work with Lync Server
ms:assetid: 29bdddbf-75d5-4c92-988e-c8506ecc7a1c
ms:mtpsurl: https://technet.microsoft.com/library/JJ966276(v=EXCHG.150)
ms:contentKeyID: 51439478
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure UM to work with Lync Server in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you're integrating Microsoft Lync Server with Exchange Unified Messaging (UM), you have to run the ExchUcUtil.ps1 script in the Shell. The ExchUcUtil.ps1 script does the following:

- Creates a UM IP gateway for each Lync Server pool.

    > [!IMPORTANT]
    > The ExchUcUtil.ps1 script creates one or more UM IP gateways. You must disable outgoing calls on all UM IP gateways except one gateway that the script created. This includes disabling outgoing calls on UM IP gateways that were created before you ran the script. To disable outgoing calls on a UM IP gateway, see <A href="disable-outgoing-calls-on-https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateways">Disable outgoing calls on UM IP gateways</A>.

- Creates a UM hunt group for each UM IP gateway. The pilot identifier of each hunt group specifies the UM SIP URI dial plan used by the Lync Server Front End pool or Standard Edition server that's associated with the UM IP gateway.

- Grants Lync Server permission to read Active Directory UM container objects such as UM dial plans, auto attendants, UM IP gateways, and UM hunt groups.

> [!IMPORTANT]
> Each UM forest must be configured to trust the forest in which Lync Server 2013 is deployed, and the forest in which Lync Server 2013 is deployed must be configured to trust each UM forest. If Exchange UM is installed in multiple forests, the Exchange Server integration steps must be performed for each UM forest or you'll have to specify the Lync Server domain. For example, <EM>ExchUcUtil.ps1 -Forest:&lt;lync-domain-controller-fqdn&gt;</EM>.

For additional management tasks related to integrating Lync Server and Unified Messaging, see [Deploying Exchange 2013 UM and Lync Server overview](deploying-exchange-2013-um-and-lync-server-overview-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to run the ExchUcUtil.ps1 script

Run the ExchUcUtil.ps1 script on any Exchange server in your organization that's in the same topology as Microsoft Lync Server. You can run the script from a Mailbox server using the Shell or you can run the script using Remote Windows PowerShell on a Client Access server. If you run the script on a Client Access server in your organization, the Client Access server will proxy the Remote Windows PowerShell session to a Mailbox server in the organization.

> [!IMPORTANT]
> The ExchUcUtil.ps1 script creates one or more UM IP gateways. You must disable outgoing calls on all UM IP gateways except one gateway that the script created. This includes disabling outgoing calls on UM IP gateways that were created before you ran the script. To disable outgoing calls on a UM IP gateway, see <A href="disable-outgoing-calls-on-https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateways">Disable outgoing calls on UM IP gateways</A>. <br/><br/> You must have the permissions of the Exchange Organization Management role or be a member of the Exchange Organization Administrators security group to run the script.

1. Open the Exchange Management Shell.

2. At the `C:\Windows\System32` prompt, type **cd "\<drive letter\>\\Program Files\\Microsoft\\Exchange Server\\V15\\Scripts\>.ExchUcUtil.ps1"**, and then press Enter.

## How do you know this worked?

To verify that the ExchUcUtul.ps1 script completed successfully, do the following:

- Use the **Get-UMIPGateway** cmdlet or the EAC to view the new UM IP gateway or gateways that were created.

- Use the **Get-UMHuntGroup** cmdlet or the EAC to view the new UM hunt group or groups that were created.
