---
title: 'Configure the VoIP security setting: Exchange 2013 Help'
TOCTitle: Configure the VoIP security setting
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: b5335654-c766-4f3f-883c-f31263e1d9c1
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the VoIP security setting in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable Voice over IP (VoIP) security for a Unified Messaging (UM) dial plan. By default, when a UM dial plan is created, it will use Unsecured mode or no encryption. Exchange servers can answer calls for single or multiple UM dial plans and can answer calls for dial plans that have different VoIP security settings.

When you configure a UM dial plan to use Session Initiation Protocol (SIP) secured or Secured mode, the Exchange servers that answer calls for the UM dial plan will encrypt the SIP signaling traffic (for SIP secured mode) or both the Realtime Transport Protocol (RTP) media channels and the SIP signaling traffic (for Secured mode).

> [!IMPORTANT]
> For on-premises and hybrid deployments, when you configure the SipTCPListeningPort, SipTLSListeningPort, or the UMStartUpMode on a Client Access server running the Microsoft Exchange Unified Messaging Call Router service or a Mailbox server running the Microsoft Exchange Unified Messaging service, you will need to configure the Windows Firewall rules correctly to allow SIP and RTP network traffic.

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Server](um-dial-plan-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure VoIP security on a UM dial plan

1. In the EAC, navigate to **Unified Messaging** \> **UM Dial Plans**, select the UM dial plan on which you want to change the VoIP security, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, click **Configure**.

3. In **General**, under **VoIP security mode**, select one of the following options:

   - **SIP secured**

   - **Unsecured** (default)

   - **Secured**

4. Click **Save**.

## Use the Shell to configure VoIP security on a UM dial plan

This example configures a UM dial plan named `MySecureDialPlan` to encrypt both SIP and RTP traffic.

```powershell
Set-UMDialPlan -identity MySecureDialPlan -VoIPSecurity Secured
```

This example configures a UM dial plan named `MySecureDialPlan` to encrypt SIP but not encrypt RTP traffic.

```powershell
Set-UMDialPlan -identity MySecureDialPlan -VoIPSecurity SIPsecured
```

This example configures a UM dial plan named `MySecureDialPlan` to not encrypt SIP and RTP traffic.

```powershell
Set-UMDialPlan -identity MySecureDialPlan -VoIPSecurity Unsecured
```
