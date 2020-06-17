---
title: 'Cmdlets: Exchange 2013 Help'
TOCTitle: Cmdlets
ms:assetid: 1d741dea-1eb8-4909-850f-63d4efaa1a32
ms:mtpsurl: https://technet.microsoft.com/library/Aa996589(v=EXCHG.150)
ms:contentKeyID: 49352361
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Cmdlets

_**Applies to:** Exchange Server 2013_

A *cmdlet*, pronounced "command-let", is the smallest unit of functionality in the Exchange Management Shell. Cmdlets resemble built-in commands in other shells, for example, the `dir` command found in `cmd.exe`. Like these familiar commands, cmdlets can be called directly from the command line in the Shell and run under the context of the Shell, not as a separate process.

> [!NOTE]
> Since Microsoft Exchange Server 2007, there have been changes to how Exchange 2013 uses cmdlets internally due to the use of Windows PowerShell remoting functionality. These changes have little to no impact on how you need to use cmdlets, but they may offer additional flexibility in how you manage your Exchange servers.

Cmdlets are usually designed around repetitive administrative tasks, and, in the Shell, several hundred cmdlets are provided for Exchange-specific management tasks. These cmdlets are available in addition to the non-Exchange system cmdlets included in the basic Windows PowerShell shell design. For information about how to open the Exchange Management Shell, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

All cmdlets in the Shell are presented in verb-noun pairs. The verb-noun pair is always separated by a hyphen (-) without spaces, and the cmdlet nouns are always singular. Verbs refer to the action that the cmdlet takes. Nouns refer to the object on which the cmdlet takes action. For example, in the **Get-SystemMessage** cmdlet, the verb is **Get**, and the noun is **SystemMessage**. All Shell cmdlets that manage a specific feature share the same noun. The following table provides examples of some verbs available in the Shell.

> [!NOTE]
> By default, if the verb is omitted, the Shell assumes the <STRONG>Get</STRONG> verb. For example, when you call <STRONG>Mailbox</STRONG>, you retrieve the same results as when you call <STRONG>Get-Mailbox</STRONG>.

### Examples of verbs in the Exchange Management Shell

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Verb</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Disable</strong></p></td>
<td><p><strong>Disable</strong> cmdlets set the <code>Enabled</code> status of the specified Exchange object to <code>$False</code>. This prevents the object from processing data even though the object exists.</p></td>
</tr>
<tr class="even">
<td><p><strong>Enable</strong></p></td>
<td><p><strong>Enable</strong> cmdlets set the Enabled status of the specified Exchange object to <code>$True</code>. This enables the object to process data.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Get</strong></p></td>
<td><p><strong>Get</strong> cmdlets retrieve information about a specific Exchange object.</p>

> [!NOTE]
> Most <STRONG>Get</STRONG> cmdlets only return summary information when you run them. To tell the <STRONG>Get</STRONG> cmdlet to return verbose information when you run a command, pipe the command to the <STRONG>Format-List</STRONG> cmdlet. For more information about the <STRONG>Format-List</STRONG> command, see <A href="working-with-command-output-exchange-2013-help.md">Working with command output</A>. For more information about pipelining, see <A href="https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines">Pipelining</A>.

</td>
</tr>
<tr class="even">
<td><p><strong>Install</strong></p></td>
<td><p><strong>Install</strong> cmdlets install a new object or feature on an Exchange server.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Move</strong></p></td>
<td><p><strong>Move</strong> cmdlets relocate the specified Exchange object from one container or server to another.</p></td>
</tr>
<tr class="even">
<td><p><strong>New</strong></p></td>
<td><p><strong>New</strong> cmdlets create new Exchange object.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Remove</strong></p></td>
<td><p><strong>Remove</strong> cmdlets delete the specified Exchange object.</p></td>
</tr>
<tr class="even">
<td><p><strong>Set</strong></p></td>
<td><p><strong>Set</strong> cmdlets modify the properties of an existing Exchange object.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Test</strong></p></td>
<td><p><strong>Test</strong> cmdlets test specific Exchange components and provide log files that you can examine.</p></td>
</tr>
<tr class="even">
<td><p><strong>Uninstall</strong></p></td>
<td><p><strong>Uninstall</strong> cmdlets remove an object or feature from an Exchange server.</p></td>
</tr>
</tbody>
</table>

The following list of cmdlets is an example of a complete cmdlet set. This cmdlet set is used to manage the delivery status notification (DSN) message and mailbox quota message features of Exchange 2013:

  - **Get-SystemMessage**

  - **New-SystemMessage**

  - **Remove-SystemMessage**

  - **Set-SystemMessage**
