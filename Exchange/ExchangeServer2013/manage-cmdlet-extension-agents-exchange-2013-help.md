---
title: 'Manage cmdlet extension agents: Exchange 2013 Help'
TOCTitle: Manage cmdlet extension agents
ms:assetid: 9141b3cb-ad13-4415-be2f-aa89f91445f5
ms:mtpsurl: https://technet.microsoft.com/library/Dd298143(v=EXCHG.150)
ms:contentKeyID: 50117645
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage cmdlet extension agents

_**Applies to:** Exchange Server 2013_

This topic shows you how to enable, disable, view, and change the priority of cmdlet extension agents in Microsoft Exchange Server 2013. For more information about cmdlet extension agents in Exchange 2013, see [Cmdlet extension agents](cmdlet-extension-agents-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Cmdlet extension agents" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- Before you enable the `Scripting Agent`, you must verify that it's configured correctly. For more information about the `Scripting Agent`, see [Cmdlet extension agents](cmdlet-extension-agents-exchange-2013-help.md).

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Enable a cmdlet extension agent

When you enable a cmdlet extension agent in Exchange 2013, the agent is run on every server running Exchange 2013 in the organization. When an agent is enabled, it's made available to cmdlets, which can then use the agent to perform additional operations.

> [!WARNING]
> Before you enable an agent, be sure that you're aware of how the agent works and what impact the agent will have on your organization.

This example enables a cmdlet extension agent by using the **Enable-CmdletExtensionAgent** cmdlet. You must specify the name of the agent you want to enable when you run the cmdlet. Before you enable the `Scripting Agent`, you need to make sure that you've deployed the `ScriptingAgentConfig.xml` configuration file to all the servers in your organization. If you don't deploy the configuration file first and you enable the `Scripting ``Agent`, all non-**Get** cmdlets fail when they're run. This example enables the `Scripting Agent`.

```powershell
Enable-CmdletExtensionAgent "Scripting Agent"
```

For detailed syntax and parameter information, see [Enable-CmdletExtensionAgent](https://docs.microsoft.com/powershell/module/exchange/Enable-CmdletExtensionAgent).

## Disable a cmdlet extension agent

When you disable a cmdlet extension agent in Exchange 2013, the agent is disabled on every server running Exchange 2013 in the organization. When an agent is disabled, it's not made available to cmdlets. Cmdlets can no longer use the agent to perform additional operations.

> [!WARNING]
> Before you disable an agent, be sure that you're aware of how the agent works and what impact disabling the agent will have on your organization.

To disable a cmdlet extension agent, use the **Disable-CmdletExtensionAgent** cmdlet. Specify the name of the agent you want to disable when you run the cmdlet. This example disables the `Scripting Agent`.

```powershell
Disable-CmdletExtensionAgent "Scripting Agent"
```

For detailed syntax and parameter information, see [Disable-CmdletExtensionAgent](https://docs.microsoft.com/powershell/module/exchange/Disable-CmdletExtensionAgent).

## View existing cmdlet extension agents

Viewing cmdlet extension agents enables you to see which agents are run first and which agents are enabled in an Exchange 2013 organization. For more information about pipelining and the **Format-Table** cmdlet, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

This example gets the details of a specific cmdlet extension agent by using the **Get-CmdletExtensionAgent** cmdlet. In this example, the details of the `Mailbox Permissions Agent` are returned.

```powershell
Get-CmdletExtensionAgent "Mailbox Permissions Agent"
```

This example gets multiple cmdlet extension agents by using the **Get-CmdletExtensionAgent** cmdlet, and then pipes the output to the **Format-Table** cmdlet. This example displays a list of all of the cmdlet extension agents in the organization, and by using the **Format-Table** cmdlet, the **Name**, **Enabled**, and **Priority** properties of each agent are displayed in a table.

```powershell
Get-CmdletExtensionAgent | Format-Table Name, Enabled, Priority
```

For detailed syntax and parameter information, see [Get-CmdletExtensionAgent](https://docs.microsoft.com/powershell/module/exchange/Get-CmdletExtensionAgent).

## Change the priority of a cmdlet extension agent

The ability to change the priority of a cmdlet extension agent in Exchange 2013 is useful when you want a certain agent to be called by a cmdlet before another agent. This is especially useful if you create a custom script that's run in the `Scripting Agent`, and you want that script to take precedence over a built-in agent. For more information about the `Scripting Agent`, see [Cmdlet extension agents](cmdlet-extension-agents-exchange-2013-help.md).

> [!WARNING]
> Changing the priority or replacing the functionality of a built-in agent is an advanced operation. Be sure that you completely understand the changes you're making.

Agents are ordered from zero to the maximum number of agents. The closer to zero the agent is, the higher the priority of the agent. Agents with a higher priority are called first. For more information about agent priorities, see [Cmdlet extension agents](cmdlet-extension-agents-exchange-2013-help.md).

This example changes the priority of a cmdlet extension agent by using the **Set-CmdletExtensionAgent** cmdlet. In this example, the priority of the `Scripting Agent` is changed to 3.

```powershell
Set-CmdletExtensionAgent "Scripting Agent" -Priority 3
```

For detailed syntax and parameter information, see [Set-CmdletExtensionAgent](https://docs.microsoft.com/powershell/module/exchange/Set-CmdletExtensionAgent).
