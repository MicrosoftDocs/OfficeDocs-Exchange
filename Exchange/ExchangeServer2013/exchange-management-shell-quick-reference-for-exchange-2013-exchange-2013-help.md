---
title: 'Exchange Management Shell quick reference for Exchange 2013'
TOCTitle: Exchange Management Shell quick reference for Exchange 2013
ms:assetid: 3ea4a105-a93c-48ba-96ce-6170125354e1
ms:mtpsurl: https://technet.microsoft.com/library/JJ619302(v=EXCHG.150)
ms:contentKeyID: 49352789
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange Management Shell quick reference for Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic describes the most frequently used cmdlets available in the release to manufacturing (RTM) and later versions of Microsoft Exchange Server 2013 and provides examples of their use.

> [!NOTE]
> More content will be added about other areas of Exchange 2013 soon.

For more information about the Exchange Management Shell in Exchange 2013 and all the available cmdlets, see the following topics:

- [Using PowerShell with Exchange 2013 (Exchange Management Shell)](https://docs.microsoft.com/powershell/exchange/exchange-management-shell)

- [Exchange PowerShell](https://docs.microsoft.com/powershell/exchange/)

## What would you like to learn about?

## Common cmdlet actions

The following verbs are supported by most cmdlets and are associated with a specific action.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>New</p></td>
<td><p>The New verb creates an instance of something, such as a new configuration setting, a new database, or a new SMTP connector.</p></td>
</tr>
<tr class="even">
<td><p>Remove</p></td>
<td><p>The Remove verb removes an instance of something, such as a mailbox or transport rule.</p>
<p>All Remove cmdlets support the <em>WhatIf</em> and <em>Confirm</em> parameters. For more information about these parameters, see Important Parameters.</p></td>
</tr>
<tr class="odd">
<td><p>Enable</p></td>
<td><p>The Enable verb enables a setting or mail-enables a recipient.</p></td>
</tr>
<tr class="even">
<td><p>Disable</p></td>
<td><p>The Disable verb disables an enabled setting or mail-disables a recipient.</p>
<p>All Disable tasks also support the <em>WhatIf</em> and <em>Confirm</em> parameters. For more information about these parameters, see Important Parameters.</p></td>
</tr>
<tr class="odd">
<td><p>Set</p></td>
<td><p>The Set verb modifies specific settings of an object, such as the alias of a contact or the deleted item retention of a mailbox database.</p></td>
</tr>
<tr class="even">
<td><p>Get</p></td>
<td><p>The Get verb queries a specific object or a subset of a type of object, such as a specific mailbox, all mailbox users, or mailbox users in a domain.</p></td>
</tr>
</tbody>
</table>

## Important parameters

The following parameters help you control how your commands run and indicate exactly what a command will do before it affects data.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Identity</p></td>
<td><p>The <em>Identity</em> parameter identifies the unique object for the task. It's typically used with Enable, Disable, Remove, Set, and Get cmdlets. <em>Identity</em> is also a positional parameter, which means that you don't have to specify <em>Identity</em> when you specify the parameter's value on the command line.</p>
<p>For example, <em>Get-Mailbox -Identity user1</em> queries for the mailbox of <em>user1</em>. <em>Get-Mailbox user1</em> is equivalent to <em>Get-Mailbox -Identity user1</em>.</p></td>
</tr>
<tr class="even">
<td><p>WhatIf</p></td>
<td><p>The <em>WhatIf</em> parameter instructs the cmdlet to simulate the actions that it would take on the object. By using the <em>WhatIf</em> parameter, you can view what changes would occur without actually applying any of the changes. The default value is <em>$true</em>.</p></td>
</tr>
<tr class="odd">
<td><p>Confirm</p></td>
<td><p>The <em>Confirm</em> parameter causes the cmdlet to pause processing and requires the administrator to acknowledge what the cmdlet will do before processing continues. The default value is <em>$true</em>.</p></td>
</tr>
<tr class="even">
<td><p>Validate</p></td>
<td><p>The <em>Validate</em> parameter causes the cmdlet to check that all prerequisites for running the operation are satisfied and that the operation will complete successfully.</p></td>
</tr>
</tbody>
</table>

## Tips and tricks

The following commands are associated with various tasks that you can use when administering Exchange 2013.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Get-Command</p></td>
<td><p>This cmdlet retrieves all tasks that can be executed in Exchange 2013.</p></td>
</tr>
<tr class="even">
<td><p>Get-Command *<em>keyword</em>*</p></td>
<td><p>This cmdlet retrieves tasks that have <em>keyword</em> in the cmdlet.</p></td>
</tr>
<tr class="odd">
<td><p>Get-<em>task</em> | Get-Member</p></td>
<td><p>This cmdlet retrieves all properties and methods of <em>task</em>.</p></td>
</tr>
<tr class="even">
<td><p>Get-<em>task</em> | Format-List</p></td>
<td><p>This cmdlet displays the output of the query in a formatted list. You can pipe the output of any Get cmdlet to Format-List to view the whole set of properties that exist on the object returned by that command, or you can specify individual properties that you want to view, separated by commas, as in the following example: <em>Get-Mailbox *john* | Format-List alias,*quota</em></p></td>
</tr>
<tr class="odd">
<td><p>Help <em>task</em></p></td>
<td><p>This cmdlet retrieves Exchange Management Shell help information for any task in Exchange 2013, as in the following example: <em>Help Get-Mailbox</em></p></td>
</tr>
<tr class="even">
<td><p>Get-<em>task</em> | Format-List &gt; <em>file.txt</em></p></td>
<td><p>This cmdlet exports the output of <em>task</em> to a text file: <em>file.txt</em></p></td>
</tr>
</tbody>
</table>

## Permissions

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Get-RoleGroupMember &quot;<em>Organization Management</em>&quot;</p></td>
<td><p>This command retrieves the members of the <em>Organization Management</em> management role group.</p></td>
</tr>
<tr class="even">
<td><p>Get-ManagementRoleAssignment -Role &quot;<em>Mail Recipient Creation</em>&quot; -GetEffectiveUsers</p></td>
<td><p>This command retrieves a list of all the users who are granted permissions provided by the <em>Mail Recipient Creation</em> management role. This includes users who are members of role groups or universal security groups (USGs) that are assigned the Mail Recipient Creation role. This doesn't include users who are members of linked role groups in another forest.</p></td>
</tr>
<tr class="odd">
<td><p>Get-ManagementRoleAssignment -RoleAssignee <em>Administrator</em> | Get-ManagementRole | Get-ManagementRoleEntry</p></td>
<td><p>This command retrieves a list of cmdlets that the user <em>Administrator</em> can run.</p></td>
</tr>
<tr class="even">
<td><p>ForEach ($RoleEntry in Get-ManagementRoleEntry *\<em>Remove-Mailbox</em> -parameters Identity) {Get-ManagementRoleAssignment -Role $RoleEntry.Role -GetEffectiveUsers -Delegating $False | Where-Object {$_.EffectiveUserName -Ne &quot;All Group Members&quot;} | FL Role, EffectiveUserName, AssignmentChain}</p></td>
<td><p>This command retrieves a list of all the users who can run the <em>Remove-Mailbox</em> cmdlet.</p></td>
</tr>
<tr class="odd">
<td><p>Get-ManagementRoleAssignment -WritableRecipient <em>kima</em> -GetEffectiveUsers | FT RoleAssigneeName, EffectiveUserName, Role, AssignmentChain</p></td>
<td><p>This command retrieves a list of all users who can modify the mailbox of <em>kima</em>.</p></td>
</tr>
<tr class="even">
<td><p>New-ManagementScope &quot;<em>Seattle Users</em>&quot; -RecipientRestrictionFilter "<em>City</em> -Eq '<em>Seattle</em>'"</p>
<p>New-RoleGroup &quot;<em>Seattle Admins</em>&quot; -Roles &quot;<em>Mail Recipients</em>&quot;, &quot;<em>Mail Recipient Creation</em>&quot;, &quot;<em>Mailbox Import Export</em>&quot;, -CustomRecipientWriteScope &quot;<em>Seattle Users</em>&quot;</p></td>
<td><p>This command creates a new management scope and management role group to enable members of the role group to manage recipients in Seattle.</p>
<p>First, the <em>Seattle Users</em> management scope is created, which matches only recipients who have <em>Seattle</em> in the <em>City</em> attribute on their user object.</p>
<p>Then, a new role group called <em>Seattle Admins</em> is created and the <em>Mail Recipients</em>, <em>Mail Recipient Creation</em>, and <em>Mailbox Import Export</em> roles are assigned. The role group is scoped so that its members can manage only users who match the <em>Seattle Users</em> recipient filter scope.</p></td>
</tr>
<tr class="odd">
<td><p>New-ManagementScope &quot;<em>Vancouver Servers</em>&quot; -ServerRestrictionFilter "<em>ServerSite</em> -Eq '<em>Vancouver</em>'"</p>
<p>$RoleGroup = Get-RoleGroup &quot;<em>Server Management</em>&quot;</p>
<p>New-RoleGroup &quot;<em>Vancouver Server Management</em>&quot; -Roles $RoleGroup.Roles -CustomConfigWriteScope &quot;<em>Vancouver Servers</em>&quot;</p></td>
<td><p>This command creates a new management scope and copies an existing role group to enable members of the new role group to manage only servers in the Vancouver Active Directory site.</p>
<p>First, the <em>Vancouver Servers</em> management scope is created, which matches only servers that are located in the <em>Vancouver</em> Active Directory site. The Active Directory site is stored in the <em>ServerSite</em> attribute on the server objects.</p>
<p>Then, a new role group called <em>Vancouver Server Management</em> is created that's a copy of the <em>Server Management</em> role group. This new role group, however, is scoped to allow its members to manage only servers that match the <em>Vancouver Servers</em> configuration filter scope.</p></td>
</tr>
<tr class="even">
<td><p>Add-RoleGroupMember &quot;<em>Organization Management</em>&quot; -Member <em>davids</em></p></td>
<td><p>This command adds the user <em>davids</em> to the <em>Organization Management</em> role group.</p></td>
</tr>
<tr class="odd">
<td><p>Get-ManagementRoleAssignment -Role &quot;<em>Mail Recipient Creation</em>&quot; -RoleAssignee &quot;<em>Seattle Admins</em>&quot; | Remove-ManagementRoleAssignment</p></td>
<td><p>This command removes the <em>Mail Recipient Creation</em> role from the <em>Seattle Admins</em> role group. This command is useful because you don't need to know the name of the management role assignment that assigns the role to the role group.</p></td>
</tr>
</tbody>
</table>

## Remote Shell

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri http://<em>ExServer.contoso.com</em>/PowerShell/ -Authentication Kerberos</p>
<p>Import-PSSession $Session</p></td>
<td><p>These commands open a new remote Shell session between a local domain-joined computer and a remote Exchange 2013 server with the FQDN <em>ExServer.contoso.com</em>. Use this command if you want to administer a remote Exchange 2013 server and only have the Windows Management Framework, which includes the Windows PowerShell command-line interface, installed on your local computer. This command uses your current logon credentials to authenticate against the remote Exchange 2013 server.</p></td>
</tr>
<tr class="even">
<td><p>$UserCredential = Get-Credential</p>
<p>$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri http://<em>ExServer.contoso.com</em>/PowerShell/ -Authentication Kerberos -Credential $UserCredential</p>
<p>Import-PSSession $Session</p></td>
<td><p>These commands open a new remote Shell session between a local domain-joined computer and a remote Exchange 2013 server with the FQDN <em>ExServer.contoso.com</em>. Use this command if you want to administer a remote Exchange 2013 server and only have the Windows Management Framework, which includes Windows PowerShell, installed on your local computer. This command uses credentials you specify explicitly to authenticate against the remote Exchange 2013 server.</p></td>
</tr>
<tr class="odd">
<td><p>Remove-PSSession $Session</p></td>
<td><p>This command closes the remote Shell session between a local computer and the remote Exchange 2013 server.</p></td>
</tr>
<tr class="even">
<td><p>Import-RecipientDataProperty -Identity &quot;Tony Smith&quot; -SpokenName -FileData <em>([Byte[]]$(Get-Content -Path &quot;M:\AudioFiles\TonySmith.wma&quot; -Encoding Byte -ReadCount 0))</em></p></td>
<td><p>This command shows an example of the syntax, shown in italics, required to import a file into a remote Exchange 2013 server using the FileData parameter on a cmdlet. The syntax encapsulates the data contained in the <em>M:\AudioFiles\TonySmith.wma</em> file and streams the data to the FileData property on the Import-RecipientDataProperty cmdlet.</p>
<p>The FileData parameter accepts data from a file on your local computer using this syntax on most cmdlets.</p></td>
</tr>
<tr class="odd">
<td><p>Export-RecipientDataProperty -Identity tony@contoso.com -SpokenName <em>| ForEach {$_.FileData | Add-Content C:\tonysmith.wma -Encoding Byte}</em></p></td>
<td><p>This command shows an example of the syntax, shown in italics, required to export a file from a remote Exchange 2013 server. The syntax encapsulates the data stored in the FileData property on the object returned by the cmdlet and then streams the data to your local computer. The data is then stored in the <em>C:\tonysmith.wma</em> file.</p>
<p>Most cmdlets that output objects with a FileData property use this syntax to export data to a file on your local computer.</p></td>
</tr>
</tbody>
</table>
