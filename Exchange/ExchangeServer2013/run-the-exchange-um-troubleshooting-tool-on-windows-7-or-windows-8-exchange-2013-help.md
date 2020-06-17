---
title: 'Run the Exchange UM Troubleshooting Tool on Windows 7 or Windows 8: Exchange 2013 Help'
TOCTitle: Run the Exchange UM Troubleshooting Tool on Windows 7 or Windows 8
ms:assetid: 98d6869d-ee4a-4088-849d-ef75b0f5d932
ms:mtpsurl: https://technet.microsoft.com/library/Ff851872(v=EXCHG.150)
ms:contentKeyID: 55129212
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Run the Exchange UM Troubleshooting Tool on Windows 7 or Windows 8

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

The Microsoft Exchange 2010 UM Troubleshooting Tool is an Exchange Management Shell cmdlet named **Test-ExchangeUMCallFlow**. You can use the cmdlet to diagnose configuration errors specific to call answering scenarios and to test whether voice mail is functioning correctly in both on-premises and cross-premises Microsoft Exchange Server 2010 Service Pack 1 (SP1) or later UM deployments. You can use this cmdlet in deployments with Microsoft Office Microsoft Lync Server 2010 or later or in UM deployments with Vo IP gateways, IP PBXs or session border controllers (SBCs).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM server" or "UM services" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Make sure your Exchange 2010 or Exchange 2013 organization meets the following requirements:

  - A UM dial plan has been created. For detailed steps, see [Create a UM dial plan](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan).

  - A UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy).

  - A UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway).

  - An Exchange 2010 UM server has been added to UM dial plan. If you are using Exchange 2013 with Lync Server, add all Client Access and Mailbox servers to the SIP URI dial plans. For detailed steps, see [Add a UM Server to a Dial Plan](https://go.microsoft.com/fwlink/p/?linkid=313051) or [Add Mailbox and Client Access servers to a SIP URI dial plan](add-mailbox-and-client-access-servers-to-a-sip-uri-dial-plan-exchange-2013-help.md).

- If you're running the UM Troubleshooting Tool on a local UM server with Exchange 2010 SP1 or later or on an Exchange 2013 Mailbox server, you may not have to install all the prerequisites listed below. They may have already been installed along with the UM server role. However, if you're installing the UM Troubleshooting Tool on a 64-bit computer other than a server that is running the UM server role, you will need to install the following components:

  - Microsoft .NET Framework 3.5 Service Pack 1 (SP1). See [Microsoft .NET Framework 3.5 Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=152380).

  - If the tool will be run on a Windows Vista or Windows Server 2008 computer, see [Microsoft .NET Framework 3.5 Family Update for Windows Vista x64, and Windows Server 2008 x64](https://go.microsoft.com/fwlink/p/?linkid=178998).

  - Windows Remote Management (WinRM) 2.0 and Windows PowerShell V2 (Windows6.0-KB968930.msu). See Microsoft Knowledge Base article 968930, [Windows Management Framework Core package (Windows PowerShell 2.0 and WinRM 2.0)](https://support.microsoft.com/help/968930).

  - Microsoft Unified Communications Managed API 2.0 Core Runtime (UcmaRuntimeWebDownloadX64.msi).

- Download and install the UM Troubleshooting Tool.

  - Download [Unified Messaging Troubleshooting Tool](https://go.microsoft.com/fwlink/p/?linkid=182625) from the Microsoft Download Center.

  - Install the tool. For details, see [Install the Exchange UM Troubleshooting Tool](install-the-exchange-um-troubleshooting-tool-exchange-2013-help.md).

    > [!IMPORTANT]
    > If you will be using the UM Troubleshooting Tool in `SIPClient` mode, there are several other Office Communications Server 2007 R2 or Microsoft Lync Server requirements and prerequisites that must be met. For more information, see <A href="https://go.microsoft.com/fwlink/p/?linkid=311961">Checklist: Deploy Office Communications Server 2007 R2 and Exchange 2010 Unified Messaging</A>.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Run the UM Troubleshooting Tool on Windows Vista, Windows 7, or Windows 8

1. Click **Start** \> **All Programs** \> **Accessories** \> **Windows PowerShell**.

2. Right-click **Windows PowerShell**, and from the pop-up menu select **Run as administrator**.

3. At the Windows PowerShell command prompt, go to the folder where the UM Troubleshooting Tool was installed and run the following.

   ```powershell
   C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -psconsolefile .\Microsoft.Exchange.UM.TroubleshootingToolsnapin.psc1 -noexit -command ". '.\Microsoft.Exchange.UM.TroubleshootingTool.ps1' "
   ```

4. If you're running the UM Troubleshooting Tool on Windows Vista, Windows 7, or Windows 8, at the Windows PowerShell command prompt, run the following.

   ```powershell
   Set-ExecutionPolicy RemoteSigned
   ```

5. From the **Start** menu, open the **Microsoft Exchange 2010 UM Troubleshooting Tool**.

6. In the **Microsoft Exchange 2010 UM Troubleshooting Tool** window, at the prompt, type the following and press Enter.

   ```powershell
   $cred=Get-Credential
   ```

7. In the **Windows PowerShell Credential Request** window, type the domain\\username and password, and then click **OK**.

8. In the **Microsoft Exchange 2010 UM Troubleshooting Tool** window, specify the necessary cmdlet parameters to test for call flow. For example:

   ```powershell
   Test-ExchangeUMCallFlow -Mode SIPClient -CallingParty tonysmith@contoso.com - CalledParty jamiestark@contoso.com NextHop ocsfe.contoso.com -Credential $cred
   ```
