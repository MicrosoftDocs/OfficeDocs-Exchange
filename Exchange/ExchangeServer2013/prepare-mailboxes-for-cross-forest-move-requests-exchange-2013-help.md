---
title: 'Prepare mailboxes for cross-forest move requests: Exchange 2013 Help'
TOCTitle: Prepare mailboxes for cross-forest move requests
ms:assetid: fdbed4fc-a77e-40d5-a211-863b05d74784
ms:mtpsurl: https://technet.microsoft.com/library/Ee633491(v=EXCHG.150)
ms:contentKeyID: 49360516
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Prepare mailboxes for cross-forest move requests

_**Applies to:** Exchange Server 2013_

**Summary:** Learn about preparing mailboxes for cross-forest moves in Exchange 2013.

Microsoft Exchange 2013 supports mailbox moves and migrations using the Exchange Management Shell, specifically the **New-MoveRequest** and **New-MigrationBatch** cmdlets. You can also move the mailbox via the Exchange admin center (EAC). Note that you can move Exchange 2010 and Exchange 2013 mailboxes to an Exchange 2013 forest.

To move a mailbox from an Exchange forest to an Exchange 2013 forest, the Exchange 2013 target forest must contain a valid mail-enabled user with a specified set of Active Directory attributes. If there is at least one Exchange 2013 Client Access server deployed in the forest, the forest is considered an Exchange 2013 forest.

To prepare for the mailbox move, you must create mail-enabled users with the required attributes in the target forest. Here are two recommended approaches to creating mail-enable users with the necessary attributes:

  - If you deployed Microsoft Identity Lifecycle Manager (ILM) for cross-forest global address list (GAL) synchronization, the recommended approach to creating the mail-enabled user is to use Service Pack 1 (SP1) for ILM 2007 Feature Pack 1 (FP1). We've created sample code that you can use to learn how to customize ILM to synchronize the source mailbox user and target mail user.

    For more information, including how to download the sample code, see [Prepare mailboxes for cross-forest moves using sample code](prepare-mailboxes-for-cross-forest-moves-using-sample-code-exchange-2013-help.md).

  - If you created the target mail user using an Active Directory tool other than ILM/MIIS, use the **Update-Recipient** cmdlet with the *Identity* parameter to run the Address List service to generate the **LegacyExchangeDN** for the target mail user. We have created a sample Windows PowerShell script that reads from and writes to Active Directory and calls the **Update-Recipient** cmdlet.

    For more information about using the sample script, see [Prepare mailboxes for cross-forest moves using the Prepare-MoveRequest.ps1 script in the Shell](prepare-mailboxes-for-cross-forest-moves-using-the-prepare-moverequest-ps1-script-in-the-shell-exchange-2013-help.md).

After creating the target mail user, you can then run the **New-MoveRequest** or the **New-MigrationBatch** cmdlets to move the mailbox to the target Exchange 2013 forest.

For more information about remote move requests, see the following topics:

  - [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch)

  - [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest)

For more information about remote mailbox moves and remote legacy moves, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md).

The remainder of this topic describes the Active Directory mail user attributes that are required for a mailbox move. These attributes are configured for you when you use either the code or the script to prepare for the mailbox move. However, you can manually copy these attributes using an Active Directory editor.

## Active Directory user attributes required for a mailbox move

To support a remote mailbox move, the mail user object in the target Exchange 2013 forest must have the Active Directory attributes that are described in this section:

  - Mandatory attributes

  - Optional attributes

  - Linked attributes

  - Linked user attributes

  - Resource mailbox attributes

  - Additional attributes

## Mandatory attributes

The following table lists the minimum set of attributes that need to be configured in ILM on the target mail user for the **New-MoveRequest** cmdlet to function correctly.

### Mail user's attributes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Mail user's Active Directory attributes</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>displayName</strong></p></td>
<td><p>Copy the corresponding attribute of the source mailbox or generate a new value.</p></td>
</tr>
<tr class="even">
<td><p><strong>Mail</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>mailNickname</strong></p></td>
<td><p>Copy the corresponding attribute of the source mailbox or generate a new value.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchArchiveGUID and msExchArchiveName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox. Attributes are only available if the source mailbox is Exchange 2010.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchMailboxGUID</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchRecipientDisplayType</strong></p></td>
<td><p>-2147483642 (decimal) //equivalent to 0x80000006 (hex).</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchRecipientTypeDetails</strong></p></td>
<td><p>128 (decimal) /0x80 (hex).</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchUserCulture</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchVersion</strong></p></td>
<td><p>44220983382016 (decimal).</p></td>
</tr>
<tr class="even">
<td><p><strong>cn</strong></p></td>
<td><p>Copy the corresponding attribute of the source mailbox or generate a new value.</p></td>
</tr>
<tr class="odd">
<td><p><strong>proxyAddresses</strong></p></td>
<td><p>Copy source mailbox's <strong>proxyAddresses</strong> attribute. Additionally, copy source mailbox's <strong>LegacyExchangeDN</strong> as an X500 address in the <strong>proxyAddresses</strong> attribute of the target mail user.</p>

> [!NOTE]
> The <STRONG>proxyAddresses</STRONG> of the source mailbox user must contain an SMTP address that matches the authoritative domain of the target forest. This allows the <STRONG>New-MoveRequest</STRONG> cmdlet to correctly select the <STRONG>targetAddress</STRONG> of the source mail-enabled user (converted from the source mailbox user after the mailbox move request is complete) to ensure that mail routing is still functional.

</td>
</tr>
<tr class="even">
<td><p><strong>sAMAccountName</strong></p></td>
<td><p>Copy the corresponding attribute of the source mailbox or generate a new value.</p>
<p>Ensure that the value is unique within the target forest domain that the target mail user belongs to.</p></td>
</tr>
<tr class="odd">
<td><p><strong>targetAddress</strong></p></td>
<td><p>Set to an SMTP address in the <strong>proxyAddresses</strong> attribute of the source mailbox.</p>
<p>This SMTP address must belong to the authoritative domain of the source forest.</p></td>
</tr>
<tr class="even">
<td><p><strong>userAccountControl</strong></p></td>
<td><p>Constant: 514 //equivalent to 0x202, ACCOUNTDISABLE | NORMAL_ACCOUNT.</p></td>
</tr>
<tr class="odd">
<td><p><strong>userPrincipalName</strong></p></td>
<td><p>Copy the corresponding attribute of the source mailbox or generate a new value. Because the mail user is logon disabled, this <strong>userPrincipalName</strong> isn't used.</p></td>
</tr>
</tbody>
</table>

## Optional attributes

It isn't mandatory that the following attributes are configured for the **New-MoveRequest** cmdlet to function correctly; however, synchronizing them provides a better end-to-end user experience after moving the mailbox. Because the GAL in the target forest displays this target mail user, you should set the following GAL-related attributes.

### GAL-related attributes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Mail user's Active Directory attributes</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>c</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>co</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>countryCode</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>company</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>department</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>facsimileTelephoneNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>givenName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>homePhone</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>info</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>initials</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>l</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>mobile</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchAssistantName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchHideFromAddressLists</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>otherHomePhone</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>otherTelephone</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>pager</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>physicalDeliveryOfficeName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>postalCode</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>sn</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>st</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>streetAddress</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>telephoneAssistant</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>telephoneNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>title</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
</tbody>
</table>

## Linked attributes

A linked attribute is an Active Directory attribute that references other Active Directory objects in the local forest. You can't directly copy the linked attribute values from a mailbox in the source forest to a mail user in the target forest. First, you must find the Active Directory objects in the source forest that the source mailbox attribute refers to. Then, you must find the corresponding Active Directory objects in the target forest for the above-mentioned Active Directory object in the source forest. And finally, set the target mail user's attribute to refer to the Active Directory objects in the target forest.

### Linked attributes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Mail User's Active Directory attributes</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>altRecipient</strong></p></td>
<td><p>Correspond to the source mailbox's <strong>altRecipient</strong> attribute.</p></td>
</tr>
<tr class="even">
<td><p><strong>deliverAndRedirect</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox. This attribute is a Boolean value that should be set along with <strong>altRecipient</strong>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Manager</strong> (and its backlinks)</p></td>
<td><p>Correspond to the source mailbox's manager attribute.</p></td>
</tr>
<tr class="even">
<td><p><strong>MemberOf</strong> (backlinks)</p></td>
<td><p>This is the backlink of group member attribute.</p></td>
</tr>
<tr class="odd">
<td><p><strong>publicDelegates</strong> (and its backlinks)</p></td>
<td><p>Correspond to the source mailbox's <strong>publicDelegates</strong> attribute.</p></td>
</tr>
</tbody>
</table>

## Linked user attributes

If you want to move a mailbox to an Exchange 2013 resource forest, the mailbox in the resource forest is considered a *linked mailbox*. In this scenario, you need to create a linked mail user in the (target) resource forest. To create a linked mail user, you need to set the attributes shown in the following table.

### Linked mail user attributes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Mail user's Active Directory attributes</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>msExchMasterAccountHistory</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchMasterAccountSid</strong></p></td>
<td><p>If the source mailbox has <strong>msExchMasterAccountSid</strong>, copy it. Otherwise, copy the source mailbox's <strong>objectSid</strong>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchRecipientDisplayType</strong></p></td>
<td><p>Constant:-1073741818 (decimal) //equivalent to *unsigned* 0xC0000006.</p></td>
</tr>
</tbody>
</table>

> [!NOTE]
> A linked mailbox can only be created if there's forest trust between the source forest and target forest.

If the source object is disabled and the **msExchMasterAccountSid** attribute is set to self (resource mailbox, shared mailbox), don't stamp anything on the target user.

If the source object is disabled and the **msExchMasterAccountSid** attribute isn't set, the mailbox is invalid.

If the source object is enabled and the **msExchMasterAccountSid** attribute is set, the mailbox is invalid.

## Resource mailbox attributes

If you want to move a resource mailbox to an Exchange 2013 forest, you need to set the attributes shown in the following table on the target mail user.

### Resource mailbox attributes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Mail user's Active Directory attributes</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>msExchRecipientDisplayType</strong></p></td>
<td><p>If the source mailbox is a conference room:</p>
<ul>
<li><p><strong>Constant</strong>   -2147481850 (decimal) //equivalent to *unsigned* 0x80000706.</p></li>
</ul>
<p>If the source mailbox is an equipment mailbox:</p>
<ul>
<li><p><strong>Constant</strong>   -2147481594 (decimal) //equivalent to *unsigned* 0x80000806.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>msExchResourceCapacity</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchResourceDisplay</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchResourceMetaData</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchResourceSearchProperties</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
</tbody>
</table>

## Additional attributes

In Exchange 2007, the **Move-Mailbox** cmdlet also copied the attributes shown in the following table when moving a mailbox. You can optionally copy these attribute if required by your organization.

### Resource mailbox attributes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Mail User's Active Directory attributes</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>comment</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>deletedItemFlags</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>delivContLength</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>departmentNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>description</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>division</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>employeeID</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>employeeNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>employeeType</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>extensionAttribute1-15</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>homePostalAddress</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>internationalISDNNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>ipPhone</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>language</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>lmPwdHistory</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>localeID</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>mAPIRecipient</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>middleName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msDS-PhoneticCompanyName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msDS-PhoneticDepartment</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msDS-PhoneticDisplayName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msDS-PhoneticFirstName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msDS-PhoneticLastName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchBlockedSendersHash</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchELCExpirySuspensionEnd</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchELCExpirySuspensionStart</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchELCMailboxFlags</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchExternalOOFOptions</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchMessageHygieneFlags</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchMessageHygieneSCLDeleteThreshold</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchMessageHygieneSCLJunkThreshold</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchMessageHygieneSCLQuarantineThreshold</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchMessageHygieneSCLRejectThreshold</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchMDBRulesQuota</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchPoliciesExcluded</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchSafeRecipientsHash</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>msExchSafeSendersHash</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>msExchUMSpokenName</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>otherFacsimileTelephoneNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>otherIpPhone</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>otherMobile</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>otherPager</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>preferredDeliveryMethod</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>personalPager</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>personalTitle</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>photo</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>pOPCharacterSet</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>pOPContentFormat</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>postalAddress</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>postOfficeBox</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>primaryInternationalISDNNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>primaryTelexNumber</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>showInAdvancedViewOnly</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>street</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>terminalServer</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>textEncodedORAddress</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>thumbnailLogo</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>thumbnailPhoto</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>url</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>userCert</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>userCertificate</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><strong>userSMIMECertificate</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>wWWHomePage</strong></p></td>
<td><p>Directly copy the corresponding attribute of the source mailbox.</p></td>
</tr>
</tbody>
</table>
