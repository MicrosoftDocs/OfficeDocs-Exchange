---
title: 'Install and configure the Address Book Policy Routing agent'
TOCTitle: Install and configure the Address Book Policy Routing agent
ms:assetid: 20e8a43d-4508-4388-a2c9-aa3073593cc2
ms:mtpsurl: https://technet.microsoft.com/library/JJ907308(v=EXCHG.150)
ms:contentKeyID: 50639771
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Install and configure the Address Book Policy Routing agent

_**Applies to:** Exchange Server 2013_

The Address Book Policy Routing agent is a Transport agent that runs on the Mailbox server that controls how recipients are resolved in your organization. When the ABP Routing agent is installed and configured, users that are assigned to different GALs appear as external recipients in that they can't view external recipients' contact cards.

For additional management tasks related to ABPs, see [Address book policy procedures](address-book-policy-procedures-exchange-2013-help.md).

Looking for the Exchange Online version of this topic? See [Turn on address book policy routing](https://docs.microsoft.com/Exchange/address-books/address-book-policies/turn-on-address-book-policy-routing).

## What do you need to know before you begin?

- Estimated time to complete this task: 15 minutes.

- After the ABP Routing agent is installed and configured, it may take up to 30 minutes for email in the organization to be evaluated by agent.

- You can't use the EAC to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Install the ABP Routing agent

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport Agents" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

Install the ABP Routing agent by running the following command. This is the exact command and syntax you'll need to use.

```powershell
Install-TransportAgent -Name "ABP Routing Agent" -TransportAgentFactory "Microsoft.Exchange.Transport.Agent.AddressBookPolicyRoutingAgent.AddressBookPolicyRoutingAgentFactory" -AssemblyPath $env:ExchangeInstallPath\TransportRoles\agents\AddressBookPolicyRoutingAgent\Microsoft.Exchange.Transport.Agent.AddressBookPolicyRoutingAgent.dll
```

You'll get a warning that the Transport service needs to be restarted for your changes to take effect, but perform Step 2 first so you only have to restart the Transport service once.

For detailed syntax and parameter information, see [Install-TransportAgent](https://docs.microsoft.com/powershell/module/exchange/Install-TransportAgent).

## Step 2: Enable the Transport Routing agent

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport Agents" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

After the ABP Routing agent is installed, you need to enable it by running the following command:

```powershell
Enable-TransportAgent "ABP Routing Agent"
```

For detailed syntax and parameter information, see [Enable-TransportAgent](https://docs.microsoft.com/powershell/module/exchange/Enable-TransportAgent).

## Step 3: Restart the Transport service and verify the ABP Routing agent is installed and enabled

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport Agents" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

1. Restart the Transport service by running the following command.

    ```powershell
    Restart-Service MSExchangeTransport
    ```

2. After the service has restarted, verify that the ABP Routing agent is installed and enabled by running the following cmdlet.

    ```powershell
    Get-TransportAgent
    ```

    If the ABP Routing agent is listed, the agent has been correctly installed.

For detailed syntax and parameter information, see [Get-TransportAgent](https://docs.microsoft.com/powershell/module/exchange/Get-TransportAgent).

## Step 4: Enable the ABP Routing agent

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport configuration" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

The final step in this process is to enable ABP routing for the organization. Run the following command.

```powershell
Set-TransportConfig -AddressBookPolicyRoutingEnabled $true
```

For detailed syntax and parameter information, see [Set-TransportConfig](https://docs.microsoft.com/powershell/module/exchange/Set-TransportConfig).
