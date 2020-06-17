---
localization_priority: Normal
description: 'Summary: Learn about preparing mailboxes for cross-forest moves in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: fdbed4fc-a77e-40d5-a211-863b05d74784
ms.reviewer: 
title: Prepare mailboxes for cross-forest move requests
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Prepare mailboxes for cross-forest move requests

Mailbox moves and mailbox migrations in Exchange 2016 and Exchange 2019 from one forest to another require that you prepare the destination forest, which is made easier by Exchange tools and cmdlets. Exchange 2016 supports mailbox moves and migrations using the Exchange Management Shell, specifically the **New-MoveRequest** and **New-MigrationBatch** cmdlets. You can also move the mailbox in the Exchange admin center (EAC).

To move an Exchange mailbox from a source forest to the target Exchange 2016 or Exchange 2019 target forest, the target forest needs to contain a valid mail user (also known as a mail-enabled user) with a specified set of Active Directory attributes.

- In Exchange 2016, you can move an Exchange 2010, Exchange 2013, or Exchange 2016 mailbox from a source Exchange forest to a target Exchange 2016 forest. If there's at least one Exchange 2016 Mailbox server in the target forest, the forest is considered an Exchange 2016 forest.

- In Exchange 2019, you can move an Exchange 2013, Exchange 2016, or Exchange 2019 mailbox from a source Exchange forest to a target Exchange 2019 forest. If there's at least one Exchange 2019 Mailbox server in the target forest, the forest is considered an Exchange 2019 forest.

To prepare for the mailbox move, you need to create mail users (also known as mail-enabled users) with the required Active Directory attributes in the target forest. There are two recommended approaches for creating mail users with the necessary attributes:

- If you deployed Identity Lifecycle Manager (ILM) for cross-forest global address list (GAL) synchronization, we recommend that you use Microsoft Identity Manager 2016 Service Pack 1. We've created sample code that you can use to learn how to customize ILM to synchronize the source mailbox user and target mail user.

    For more information, including how to download the sample code, see [Prepare mailboxes for cross-forest moves using sample code](https://docs.microsoft.com/exchange/prepare-mailboxes-for-cross-forest-moves-using-sample-code-exchange-2013-help).

- If you created the target mail user using an Active Directory tool other than ILM or Microsoft Identity Integration Server (MIIS), use the **Update-Recipient** cmdlet with the _Identity_ parameter to generate the **LegacyExchangeDN** attribute for the target mail user. We've created a sample PowerShell script that reads from and writes to Active Directory and calls the **Update-Recipient** cmdlet.

    For more information about using the sample script, see [Prepare mailboxes for cross-forest moves using the Exchange Management Shell](prep-mailboxes-for-cross-forest-moves-in-powershell.md).

After creating the target mail user, you can then run the **New-MoveRequest** or the **New-MigrationBatch** cmdlets to move the mailbox to the target Exchange 2016 or Exchange 2019 forest.

For more information about remote move requests, see the following topics:

- [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/new-migrationbatch)

- [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/new-moverequest)

The remainder of this topic describes the mail user Active Directory attributes that are required for a mailbox move. These attributes are configured for you when you use either the code or the script to prepare for the mailbox move. However, you can manually copy these attributes using an Active Directory editor.

## Active Directory user attributes required for a mailbox move

To support a remote mailbox move, the mail user object in the target Exchange forest must have the Active Directory attributes that are described in this section:

- Mandatory attributes

- Optional attributes

- Linked attributes

- Linked user attributes

- Resource mailbox attributes

- Additional attributes

### Mandatory attributes

The following table lists the minimum set of attributes that need to be configured in ILM on the target mail user for the **New-MoveRequest** cmdlet to function correctly.

**Mail user attributes**

|**Active Directory attribute**|**Action**|
|:-----|:-----|
|**displayName**|Copy the corresponding attribute of the source mailbox or generate a new value.|
|**Mail**|Directly copy the corresponding attribute of the source mailbox.|
|**mailNickname**|Copy the corresponding attribute of the source mailbox or generate a new value.|
|**msExchArchiveGUID and msExchArchiveName**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMailboxGUID**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchRecipientDisplayType**|-2147483642 decimal (equivalent to 0x80000006 hex).|
|**msExchRecipientTypeDetails**|128 decimal (0x80 hex).|
|**msExchUserCulture**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchVersion**|44220983382016 (decimal).|
|**cn**|Copy the corresponding attribute of the source mailbox or generate a new value.|
|**proxyAddresses**|Copy source mailbox's **proxyAddresses** attribute. Additionally, copy source mailbox's **LegacyExchangeDN** as an X500 address in the **proxyAddresses** attribute of the target mail user.  <br/> **Note**: The **proxyAddresses** of the source mailbox user must contain an SMTP address that matches the authoritative domain of the target forest. This allows the **New-MoveRequest** cmdlet to correctly select the **targetAddress** of the source mail-enabled user (converted from the source mailbox user after the mailbox move request is complete) to ensure that mail routing is still functional.|
|**sAMAccountName**|Copy the corresponding attribute of the source mailbox or generate a new value.  <br/> Ensure that the value is unique within the target forest domain that the target mail user belongs to.|
|**targetAddress**|Set to an SMTP address in the **proxyAddresses** attribute of the source mailbox.  <br/> This SMTP address must belong to the authoritative domain of the source forest.|
|**userAccountControl**|Constant: 514 (equivalent to 0x202, ACCOUNTDISABLE \| NORMAL_ACCOUNT).|
|**userPrincipalName**|Copy the corresponding attribute of the source mailbox or generate a new value. Because the mail user is logon disabled, this **userPrincipalName** isn't used.|

### Optional attributes

The following attributes aren't required for the **New-MoveRequest** cmdlet to function correctly; however, synchronizing them provides a better end-to-end user experience after moving the mailbox. Because the GAL in the target forest displays this target mail user, you should set the following GAL-related attributes.

**GAL-related attributes**

|**Mail user's Active Directory attribute**|**Action**|
|:-----|:-----|
|**c**|Directly copy the corresponding attribute of the source mailbox.|
|**co**|Directly copy the corresponding attribute of the source mailbox.|
|**countryCode**|Directly copy the corresponding attribute of the source mailbox.|
|**company**|Directly copy the corresponding attribute of the source mailbox.|
|**department**|Directly copy the corresponding attribute of the source mailbox.|
|**facsimileTelephoneNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**givenName**|Directly copy the corresponding attribute of the source mailbox.|
|**homePhone**|Directly copy the corresponding attribute of the source mailbox.|
|**info**|Directly copy the corresponding attribute of the source mailbox.|
|**initials**|Directly copy the corresponding attribute of the source mailbox.|
|**l**|Directly copy the corresponding attribute of the source mailbox.|
|**mobile**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchAssistantName**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchHideFromAddressLists**|Directly copy the corresponding attribute of the source mailbox.|
|**otherHomePhone**|Directly copy the corresponding attribute of the source mailbox.|
|**otherTelephone**|Directly copy the corresponding attribute of the source mailbox.|
|**pager**|Directly copy the corresponding attribute of the source mailbox.|
|**physicalDeliveryOfficeName**|Directly copy the corresponding attribute of the source mailbox.|
|**postalCode**|Directly copy the corresponding attribute of the source mailbox.|
|**sn**|Directly copy the corresponding attribute of the source mailbox.|
|**st**|Directly copy the corresponding attribute of the source mailbox.|
|**streetAddress**|Directly copy the corresponding attribute of the source mailbox.|
|**telephoneAssistant**|Directly copy the corresponding attribute of the source mailbox.|
|**telephoneNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**title**|Directly copy the corresponding attribute of the source mailbox.|

### Linked attributes

A _linked attribute_ is an Active Directory attribute that references other Active Directory objects in the local forest. You can't directly copy the linked attribute values from a mailbox in the source forest to a mail user in the target forest. Instead, you do the following steps:

1. Find the Active Directory objects in the source forest that the source mailbox attribute refers to.

2. Find the corresponding Active Directory objects in the target forest.

3. Set the target mail user's attribute to refer to the Active Directory objects in the target forest.

**Linked attributes**

|**Mail user's Active Directory attribute**|**Action**|
|:-----|:-----|
|**altRecipient**|Correspond to the source mailbox's **altRecipient** attribute.|
|**deliverAndRedirect**|Directly copy the corresponding attribute of the source mailbox. This attribute is a Boolean value that should be set along with **altRecipient**.|
|**Manager** (and its backlinks)|Correspond to the source mailbox's manager attribute.|
|**MemberOf** (backlinks)|This is the backlink of group member attribute.|
|**publicDelegates** (and its backlinks)|Correspond to the source mailbox's **publicDelegates** attribute.|

### Linked user attributes

If you want to move a mailbox to an Exchange resource forest, the mailbox in the resource forest is considered a _linked mailbox_. In this scenario, you need to create a linked mail user in the (target) resource forest. To create a linked mail user, you need to set the attributes shown in the following table.

**Linked mail user attributes**

|**Active Directory attribute**|**Action**|
|:-----|:-----|
|**msExchMasterAccountHistory**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMasterAccountSid**|If the source mailbox has **msExchMasterAccountSid**, copy it. Otherwise, copy the source mailbox's **objectSid**.|
|**msExchRecipientDisplayType**|Constant:-1073741818 decimal (equivalent to `*unsigned* 0xC0000006`).|

> [!NOTE]
> A linked mailbox can only be created if there's a forest trust between the source forest and target forest.

If the source object is disabled and the **msExchMasterAccountSid** attribute is set to self (resource mailbox, shared mailbox), don't stamp anything on the target user.

If the source object is disabled and the **msExchMasterAccountSid** attribute isn't set, the mailbox is invalid.

If the source object is enabled and the **msExchMasterAccountSid** attribute is set, the mailbox is invalid.

### Resource mailbox attributes

If you want to move a resource mailbox to an Exchange forest, you need to set the attributes shown in the following table on the target mail user.

**Resource mailbox attributes**

|**Mail user's Active Directory attribute**|**Action**|
|:-----|:-----|
|**msExchRecipientDisplayType**|If the source mailbox is a conference room: Constant: -2147481850 decimal (equivalent to `*unsigned* 0x80000706`).  <br/> If the source mailbox is an equipment mailbox: Constant: -2147481594 decimal (equivalent to `*unsigned* 0x80000806`).|
|**msExchResourceCapacity**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchResourceDisplay**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchResourceMetaData**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchResourceSearchProperties**|Directly copy the corresponding attribute of the source mailbox.|

### Additional attributes

**Resource mailbox attributes**

|**Mail User's Active Directory attributes**|**Description**|
|:-----|:-----|
|**comment**|Directly copy the corresponding attribute of the source mailbox.|
|**deletedItemFlags**|Directly copy the corresponding attribute of the source mailbox.|
|**delivContLength**|Directly copy the corresponding attribute of the source mailbox.|
|**departmentNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**description**|Directly copy the corresponding attribute of the source mailbox.|
|**division**|Directly copy the corresponding attribute of the source mailbox.|
|**employeeID**|Directly copy the corresponding attribute of the source mailbox.|
|**employeeNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**employeeType**|Directly copy the corresponding attribute of the source mailbox.|
|**extensionAttribute1-15**|Directly copy the corresponding attribute of the source mailbox.|
|**homePostalAddress**|Directly copy the corresponding attribute of the source mailbox.|
|**internationalISDNNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**ipPhone**|Directly copy the corresponding attribute of the source mailbox.|
|**language**|Directly copy the corresponding attribute of the source mailbox.|
|**lmPwdHistory**|Directly copy the corresponding attribute of the source mailbox.|
|**localeID**|Directly copy the corresponding attribute of the source mailbox.|
|**mAPIRecipient**|Directly copy the corresponding attribute of the source mailbox.|
|**middleName**|Directly copy the corresponding attribute of the source mailbox.|
|**msDS-PhoneticCompanyName**|Directly copy the corresponding attribute of the source mailbox.|
|**msDS-PhoneticDepartment**|Directly copy the corresponding attribute of the source mailbox.|
|**msDS-PhoneticDisplayName**|Directly copy the corresponding attribute of the source mailbox.|
|**msDS-PhoneticFirstName**|Directly copy the corresponding attribute of the source mailbox.|
|**msDS-PhoneticLastName**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchBlockedSendersHash**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchELCExpirySuspensionEnd**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchELCExpirySuspensionStart**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchELCMailboxFlags**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchExternalOOFOptions**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMessageHygieneFlags**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMessageHygieneSCLDeleteThreshold**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMessageHygieneSCLJunkThreshold**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMessageHygieneSCLQuarantineThreshold**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMessageHygieneSCLRejectThreshold**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchMDBRulesQuota**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchPoliciesExcluded**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchSafeRecipientsHash**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchSafeSendersHash**|Directly copy the corresponding attribute of the source mailbox.|
|**msExchUMSpokenName**|Directly copy the corresponding attribute of the source mailbox.|
|**otherFacsimileTelephoneNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**otherIpPhone**|Directly copy the corresponding attribute of the source mailbox.|
|**otherMobile**|Directly copy the corresponding attribute of the source mailbox.|
|**otherPager**|Directly copy the corresponding attribute of the source mailbox.|
|**preferredDeliveryMethod**|Directly copy the corresponding attribute of the source mailbox.|
|**personalPager**|Directly copy the corresponding attribute of the source mailbox.|
|**personalTitle**|Directly copy the corresponding attribute of the source mailbox.|
|**photo**|Directly copy the corresponding attribute of the source mailbox.|
|**pOPCharacterSet**|Directly copy the corresponding attribute of the source mailbox.|
|**pOPContentFormat**|Directly copy the corresponding attribute of the source mailbox.|
|**postalAddress**|Directly copy the corresponding attribute of the source mailbox.|
|**postOfficeBox**|Directly copy the corresponding attribute of the source mailbox.|
|**primaryInternationalISDNNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**primaryTelexNumber**|Directly copy the corresponding attribute of the source mailbox.|
|**showInAdvancedViewOnly**|Directly copy the corresponding attribute of the source mailbox.|
|**street**|Directly copy the corresponding attribute of the source mailbox.|
|**terminalServer**|Directly copy the corresponding attribute of the source mailbox.|
|**textEncodedORAddress**|Directly copy the corresponding attribute of the source mailbox.|
|**thumbnailLogo**|Directly copy the corresponding attribute of the source mailbox.|
|**thumbnailPhoto**|Directly copy the corresponding attribute of the source mailbox.|
|**url**|Directly copy the corresponding attribute of the source mailbox.|
|**userCert**|Directly copy the corresponding attribute of the source mailbox.|
|**userCertificate**|Directly copy the corresponding attribute of the source mailbox.|
|**userSMIMECertificate**|Directly copy the corresponding attribute of the source mailbox.|
|**wWWHomePage**|Directly copy the corresponding attribute of the source mailbox.|
