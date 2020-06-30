---
title: 'Set credentials to use with Exchange UM Troubleshooting Tool: Exchange 2013 Help'
TOCTitle: Set the credentials to use with the Exchange UM Troubleshooting Tool
ms:assetid: 542b7718-9345-40cc-bcb2-e307e70a1fa2
ms:mtpsurl: https://technet.microsoft.com/library/Ff630916(v=EXCHG.150)
ms:contentKeyID: 55129210
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the credentials to use with the Exchange UM Troubleshooting Tool

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

The Microsoft Exchange 2010 UM Troubleshooting Tool is an Exchange Management Shell cmdlet named **Test-ExchangeUMCallFlow**. You can use the cmdlet to diagnose configuration errors specific to call answering scenarios and to test whether voice mail is functioning correctly in both on-premises and cross-premises Microsoft Exchange Server 2010 Service Pack 1 (SP1) or later UM deployments. You can use this cmdlet in deployments with Microsoft Office Microsoft Lync Server 2010 or later or in UM deployments with Vo IP gateways, IP PBXs or session border controllers (SBCs).

By default, when you're running the UM Troubleshooting Tool, it uses the credentials that are used when you log on to the computer. The credentials used are those that are specified for the calling party. You must set or specify the credentials to be used when you're running the UM Troubleshooting Tool in `SIPClient` mode. However, you don't need to set the credentials when running the UM Troubleshooting Tool in `Gateway` mode.

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM server" or "UM services" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Make sure your Exchange 2010 or Exchange 2013 organization meets the following requirements:

  - A UM dial plan has been created. For detailed steps, see [Create a UM dial plan](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan).

  - A UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy).

  - A UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway).

  - An Exchange 2010 UM server has been added to the UM dial plan. If you're using Exchange 2013 with Lync Server, add all Client Access and Mailbox servers to the SIP URI dial plans. For detailed steps, see [Add a UM Server to a Dial Plan](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/aa996399(v=exchg.141)) or [Add Mailbox and Client Access servers to a SIP URI dial plan](add-mailbox-and-client-access-servers-to-a-sip-uri-dial-plan-exchange-2013-help.md).

- Install the UM Troubleshooting Tool. For detailed steps, see [Install the Exchange UM Troubleshooting Tool](install-the-exchange-um-troubleshooting-tool-exchange-2013-help.md).

    > [!IMPORTANT]
    > If you will be using the UM Troubleshooting Tool in <CODE>SIPClient</CODE> mode, there are several other Office Communications Server 2007 R2 or Microsoft Lync Server requirements and prerequisites. For more information, see <A href="https://docs.microsoft.com/previous-versions/office/exchange-server-2010/dd638120(v=exchg.141)">Checklist: Deploy Office Communications Server 2007 R2 and Exchange 2010 Unified Messaging</A> or <A href="checklist-integrate-exchange-2013-um-with-lync-server-exchange-2013-help.md">Checklist: Integrate Exchange 2013 UM with Lync Server</A>.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Set the credentials to use with the UM Troubleshooting Tool

1. From the **Start** menu, open the **Microsoft Exchange 2010 UM Troubleshooting Tool**.

2. In the **Microsoft Exchange 2010 UM Troubleshooting Tool** window, at the prompt, type the following and press Enter.

   ```powershell
   $cred=Get-Credential
   ```

3. In the **Windows PowerShell Credential Request** window, type the domain\\username and password, and then click **OK**.

4. In the **Microsoft Exchange 2010 UM Troubleshooting Tool** window, specify the necessary cmdlet parameters to test for call flow. For example:

   ```powershell
   Test-ExchangeUMCallFlow -Mode SIPClient -CallingParty tonysmith@contoso.com - CalledParty jamiestark@contoso.com NextHop ocsfe.contoso.com -Credential $cred
   ```
