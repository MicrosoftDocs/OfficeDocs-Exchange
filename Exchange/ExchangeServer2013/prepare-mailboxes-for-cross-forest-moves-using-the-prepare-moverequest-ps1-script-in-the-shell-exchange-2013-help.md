---
title: 'Prepare mailboxes for cross-forest moves using the Prepare-MoveRequest.ps1 script in the Shell'
TOCTitle: Prepare mailboxes for cross-forest moves using the Prepare-MoveRequest.ps1 script in the Shell
ms:assetid: 2cea59fb-69b7-4a2f-833f-de4d93cf1810
ms:mtpsurl: https://technet.microsoft.com/library/Ee861103(v=EXCHG.150)
ms:contentKeyID: 49360509
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Prepare mailboxes for cross-forest moves using the Prepare-MoveRequest.ps1 script in the Shell

_**Applies to:** Exchange Server 2013_

**Summary:** Learn how to manage cross-forest mailbox moves and migrations in Exchange 2013 by using the Prepare-MoveRequest.ps1 script in the Exchange Management Shell.

Microsoft Exchange 2013 supports mailbox moves and migrations using the **New-MoveRequest** and **New-MigrationBatch** cmdlets. You can also move the mailbox via the Exchange admin center (EAC). You can move an Exchange 2010 or Exchange 2013 mailbox from a source Exchange forest to a target Exchange 2013 forest.

To run the **New-MoveRequest** and **New-MigrationBatch** cmdlets, a mail user must exist in the target Exchange forest, and the mail user must have a minimum set of required Active Directory attributes.

The sample Windows PowerShell script described in this topic supports this task by synchronizing mailbox users from an Exchange 2013 source forest to Exchange 2013 target forests as mail-enabled users. The script copies the Active Directory attributes of the mailbox users in the source forest to the target forest, and then uses the **Update-Recipient** cmdlet to turn the target objects into mail-enabled users.

For more information about using and writing scripts, see [Scripting with the Exchange Management Shell](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts). For more information about preparing for cross-forest moves, see [Prepare mailboxes for cross-forest move requests](prepare-mailboxes-for-cross-forest-move-requests-exchange-2013-help.md).

Looking for other management tasks related to remote move requests? Check out [Manage on-premises moves](manage-on-premises-moves-exchange-2013-help.md).

## What do you need to know before you begin?

- Locate the script in the following location: Program Files\\Microsoft\\Exchange Server\\V15\\Scripts

- To run the sample script, you need the following:

  - An Exchange source forest, where the mailbox currently resides. This can be an Exchange 2010 or Exchange 2013 mailbox.

  - A target forest with Exchange 2013 installed, where the mailbox will be moved to.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Prepare-MoveRequest.ps1 script to prepare mailboxes for cross-forest moves

Run the script from the Shell on a server role running Exchange 2013 in the target Exchange 2013 forest. The script copies the mailbox attributes from the source forest.

To assign a specific authentication credential for the remote forest domain controller, you must first run the Windows PowerShell **Get-Credential** cmdlet and store the user input in a temporary variable. When you run the **Get-Credential** cmdlet, the cmdlet asks for the user name and password of the account used during authentication with the remote forest domain controller. You can then use the temporary variable in the Prepare-MoveRequest.ps1 script. For more information about the **Get-Credential** cmdlet, see [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential).

> [!NOTE]
> Make sure that you use two separate credentials for the local forest and the remote forest when calling this script.

1. Run the following commands to get the local forest and remote forest credentials.

   ```powershell
   $LocalCredentials = Get-Credential
   $RemoteCredentials = Get-Credential
   ```

2. Run the following commands to pass the credential information to the *LocalForestCredential* and *RemoteForestCredential* parameters in the Prepare-MoveRequest.ps1 script.

   ```powershell
   Prepare-MoveRequest.ps1 -Identity JohnSmith@Fabrikan.com -RemoteForestDomainController DC001.Fabrikam.com -RemoteForestCredential $RemoteCredentials -LocalForestDomainController DC001.Contoso.com -LocalForestCredential $LocalCredentials
   ```

## Parameter set of the script

The following table describes the parameter set for the script.

### Parameter set of the Prepare-MoveRequest.ps1 script

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Required</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Identity</em></p></td>
<td><p>Required</p></td>
<td><p>The <em>Identity</em> parameter uniquely identifies a mailbox in the source forest. Identity can be any of the following:</p>
<ul>
<li><p>Common name (CN)</p></li>
<li><p>Alias</p></li>
<li><p><strong>proxyAddress</strong> property</p></li>
<li><p><strong>objectGuid</strong> property</p></li>
<li><p><strong>DisplayName</strong> property</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><em>RemoteForestCredential</em></p></td>
<td><p>Required</p></td>
<td><p>The <em>RemoteForestCredential</em> parameter specifies the administrator who has permissions to copy data from the source forest Active Directory.</p></td>
</tr>
<tr class="odd">
<td><p><em>RemoteForestDomainController</em></p></td>
<td><p>Required</p></td>
<td><p>The <em>RemoteForestDomainController</em> parameter specifies a domain controller in the source forest where the mailbox resides.</p></td>
</tr>
<tr class="even">
<td><p><em>DisableEmailAddressPolicy</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>DisableEmailAddressPolicy</em> parameter specifies whether the Email Address Policy (EAP) should be disabled when creating a <strong>MailUser</strong> object in the target forest.</p>
<p>When you specify this parameter, the EAP in the target forest won't be applied.</p>

> [!NOTE]
> When you specify this parameter, the <STRONG>MailUser</STRONG> object won't have e-mail address mapping in the local forest domain stamped. This is usually stamped by the EAP.

</td>
</tr>
<tr class="odd">
<td><p><em>LinkedMailUser</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>LinkedMailUser</em> switch specifies whether to create a linked MailUser in the local forest for the mailbox user in the remote forest.</p>
<p>If the switch is provided, the script creates a target <strong>MailUser</strong> object linked to the source mailbox. If the switch is omitted, the script creates a regular target <strong>MailUser</strong> object.</p></td>
</tr>
<tr class="even">
<td><p><em>LocalForestCredential</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>LocalForestCredential</em> parameter specifies the administrator with permissions to write data to the target forest Active Directory.</p>
<p>We recommend that you explicitly specify this parameter to avoid Active Directory permission issues.</p>
<p>If the remote forest and the local forest have a trusted relationship configured, don't use a user account from the remote forest as the local forest credential, even though the remote user account may have permission to modify Active Directory in the local forest.</p></td>
</tr>
<tr class="odd">
<td><p><em>LocalForestDomainController</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>LocalForestDomainController</em> parameter specifies a domain controller in the target forest where the mail-enabled user will be created.</p>
<p>We recommend that you specify this parameter to avoid possible domain controller replication delay issues in the local forest that could occur if a random domain controller is selected.</p></td>
</tr>
<tr class="even">
<td><p><em>MailboxDeliveryDomain</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>MailboxDeliveryDomain</em> parameter specifies an authoritative domain of the source forest so that the script can select the correct source mailbox user's <strong>proxyAddress</strong> property as the target mail-enabled user's <strong>targetAddress</strong> property.</p>
<p>By default, the primary SMTP address of the source mailbox user is set as the <strong>targetAddress</strong> property of the target mail-enabled user.</p></td>
</tr>
<tr class="odd">
<td><p><em>OverWriteLocalObject</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>OverWriteLocalObject</em> parameter is used for users created by the Active Directory Migration Tool. The properties are copied from the existing mail contact to the newly created mail user. However, after this copy, the script also copies the properties from the source forest user to the newly created mail user.</p></td>
</tr>
<tr class="even">
<td><p><em>TargetMailUserOU</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>TargetMailuserOU</em> parameter specifies the organizational unit (OU) under which the target mail-enabled user will be created.</p></td>
</tr>
<tr class="odd">
<td><p><em>UseLocalObject</em></p></td>
<td><p>Optional</p></td>
<td><p>The <em>UseLocalObject</em> parameter specifies whether to convert the existing local object to the required target mail-enabled user if the script detects an object in the local forest that conflicts with the to-be-created mail-enabled user.</p></td>
</tr>
</tbody>
</table>

## Examples

This section contains several examples of how you can use the Prepare-MoveRequest.ps1 script.

## Example: Single linked mail-enabled user

This example provisions a single linked mail-enabled user in the local forest, when there is forest trust between the remote forest and local forest.

1. Run the following commands to get the local forest and remote forest credentials.

   ```powershell
   $LocalCredentials = Get-Credential
   $RemoteCredentials = Get-Credential
   ```

2. Run the following command to pass the credential information to the *LocalForestCredential* and *RemoteForestCredential* parameters in the Prepare-MoveRequest.ps1 script.

   ```powershell
   Prepare-MoveRequest.ps1 -Identity JamesAlvord@Contoso.com -RemoteForestDomainController DC001.Fabrikam.com -RemoteForestCredential $RemoteCredentials -LocalForestDomainController DC001.Contoso.com -LocalForestCredential $LocalCredentials -LinkedMailUser
   ```

## Example: Pipelining

This example supports pipelining if you supply a list of mailbox identities.

1. Run the following command.

   ```powershell
   $UserCredentials = Get-Credential
   ```

2. Run the following command to pass the credential information to the *RemoteForestCredential* parameter in the Prepare-MoveRequest.ps1 script.

   ```powershell
   "IanP@Contoso.com","JoeAn@Contoso.com" | Prepare-MoveRequest.ps1 -RemoteForestDomainController DC001.Fabrikam.com -RemoteForestCredential $UserCredentials
   ```

## Example: Use a .csv file to bulk-create mail-enabled users

You can generate a .csv file containing a list of mailbox identities from the source forest, which allows you to pipe the content of this file into the script to bulk-create the target mail-enabled users.

For example, the content of the .csv file can be:

```console
Identity
Ian@contoso.com
John@contoso.com
Cindy@contoso.com
```

This example calls a .csv file to bulk create the target mail-enabled users.

1. Run the following command to get the remote forest credentials.

   ```powershell
   $UserCredentials = Get-Credential
   ```

2. Run the following command to pass the credential information to the *RemoteForestCredential* parameter in the Prepare-MoveRequest.ps1 script.

   ```powershell
   Import-Csv Test.csv | Prepare-MoveRequest.ps1 -RemoteForestDomainController DC001.Fabrikam.com -RemoteForestCredential $UserCredentials
   ```

## Script behavior per target object

This section describes how the script performs in relation to several scenarios for target objects.

## Duplicate target mail-enabled object

When the script attempts to create a target mail-enabled user from the source mailbox user, and it detects a duplicate local mail-enabled object, it uses the following logic:

- If the source mailbox user's **masterAccountSid** attribute equals any target object's **objectSid** or **masterAccountSid** attribute:

  - If the target object isn't mail-enabled, the script returns an error because the script doesn't support converting an object that isn't mail-enabled to a mail-enabled user.

  - If the target object is mail-enabled, the target object is a duplicate.

- If an address in the source mailbox user's **proxyAddress** properties (smtp/x500 only) equals an address in a target object's **proxyAddress** properties (smtp/x500 only), the target object is a duplicate.

The script prompts the user about the duplicate objects.

If the target mail-enabled object is a mail-enabled user or contact, which is most likely created by a cross-forest (Identity Lifecycle Management 2007 Service Pack 1-based) global address list (GAL) synchronization deployment, the user can run the script again with the *UseLocalObject* parameter to use the target mail-enabled object for mailbox migration.

## Mail-enabled user

If the target object is a mail-enabled user, the script copies the following attributes from the source mailbox user to the target mail-enabled user:

- **msExchMailboxGUID**

- **msExchArchiveGUID**

- **msExchArchiveName**

If the *LinkedMailUser* parameter is set, the script copies the source **objectSid**/**masterAccountSid** attribute.

## Mail-enabled contact

If the target object is a mail-enabled contact, the script deletes the existing contact and copies all its attributes to a new mail-enabled user. The script also copies the following attributes from the source mailbox user:

- **msExchMailboxGUID**

- **msExchArchiveGUID**

- **msExchArchiveName**

- **sAMAccountName**

- **userAccountControl** (set to 514 //equivalent to 0x202, ACCOUNTDISABLE | NORMAL\_ACCOUNT)

- **userPrincipalName**

If the *LinkedMailUser* parameter is set, the script copies the source **objectSid**/**masterAccountSid** attribute.

## LegacyExchangeDN attribute

When the **Update-Recipient** cmdlet is called to convert the target object into a mail-enabled user, a new **LegacyExchangeDN** attribute is generated for the target mail-enabled user. The script copies the **LegacyExchangeDN** attribute of the target mail-enabled user as an x500 address to the **proxyAddress** properties of the source mailbox user.
