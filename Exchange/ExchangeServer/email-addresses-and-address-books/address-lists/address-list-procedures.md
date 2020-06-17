---
description: 'Summary: Learn the tasks that Exchange Server 2016 and Exchange Server 2019 administrators need to know to manage address lists and global address lists (GAL).'
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: 236e8530-62dd-4c43-8a5d-8465623252e6
ms.collection: exchange-server
ms.reviewer:
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
title: Procedures for address lists in Exchange Server

---

# Procedures for address lists in Exchange Server

Address lists and global address lists (GALs) are collections of mail-enabled recipient objects from Active Directory. You can create or modify GALs, and update using the tools available in the Exchange admin center (EAC) and the Exchange Management Shell. For more information, see [Address lists in Exchange Server](address-lists.md).

These are the address list and GAL procedures that you'll find in this topic:

- **Global address list procedures**

  - [Use the Exchange Management Shell to update global address lists](#use-the-exchange-management-shell-to-update-global-address-lists)

  - [Use the Exchange Management Shell to view members of global address lists](#use-the-exchange-management-shell-to-view-members-of-global-address-lists)

  - [Use the Exchange Management Shell to create global address lists](#use-the-exchange-management-shell-to-create-global-address-lists)

  - [Use the Exchange Management Shell to modify global address lists](#use-the-exchange-management-shell-to-modify-global-address-lists)

  - [Use the Exchange Management Shell to remove global address lists](#use-the-exchange-management-shell-to-remove-global-address-lists)

- **Address list procedures**

  - [Update address lists](#update-address-lists)

  - [View the members of address lists](#view-the-members-of-address-lists)

  - [Create address lists](#create-address-lists)

  - [Modify address lists](#modify-address-lists)

  - [Use the Exchange Management Shell to move address lists](#use-the-exchange-management-shell-to-move-address-lists)

  - [Remove address lists](#remove-address-lists)

  - [Hide recipients from address lists](#hide-recipients-from-address-lists)

[Recipient filters in the EAC](#recipient-filters-in-the-eac)

[Recipient filters in the Exchange Management Shell](#recipient-filters-in-the-exchange-management-shell)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](../../permissions/feature-permissions/address-book-permissions.md) topic.

- You can do some of the procedures in this topic by using the EAC. For more information about the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). Some procedures require the Exchange Management Shell. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Global address list procedures

All procedures for modifying or updating a GAL require the Exchange Management Shell.

### Use the Exchange Management Shell to update global address lists

After you create or modify a GAL, you need to update its membership. Updating a GAL only starts the update process. It may take several hours for the GAL update to be completed.

To update a GAL, use the following syntax:

```PowerShell
Update-GlobalAddressList -Identity <GALIdentity>
```

This example updates the GAL named Contoso GAL.

```PowerShell
Update-AddressList -Identity "Contoso GAL"
```

This example updates all GALs in the organization that require updates.

```PowerShell
Get-GlobalAddressList | where {$_.RecipientFilterApplied -eq $false} | Update-GlobalAddressList
```

For detailed syntax and parameter information, see [Update-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/update-globaladdresslist).

#### How do you know this worked?

To verify that you've successfully updated the GAL, replace _\<GALIdentity\>_ with the name of the address list, and run the following command to verify that the **RecipientFilterApplied** property value is present:

```PowerShell
Get-AddressList -Identity <GALIdentity> | Format-Table -Auto Name,RecipientFilterApplied
```

### Use the Exchange Management Shell to view members of global address lists

- Technically, this procedure returns *all* recipients (including hidden recipients) that match the recipient filters for the GAL. The recipients that are actually visible in the GAL have the **HiddenFromAddressListsEnabled** property value `False`.

- If the GAL isn't up to date (the **RecipientFilterApplied** property has the value `False`), you should update the GAL before you view the members. For more information, see the previous section.

To view the members of a GAL, use the following syntax:

```PowerShell
$GAL = Get-GlobalAddressList -Identity <GALIdentity>; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $GAL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled
```

This example returns the members of the GAL named Humongous Insurance.

```PowerShell
$GAL = Get-GlobalAddressList -Identity "Humongous Insurance"; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $GAL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled
```

This example exports the results to the file C:\My Documents\Humongous Insurance Export.csv.

```PowerShell
$GAL = Get-GlobalAddressList -Identity "Humongous Insurance"; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $GAL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled | Export-Csv -NoTypeInformation -Path "C:\My Documents\Humongous Insurance Export.csv"
```

### Use the Exchange Management Shell to create global address lists

For more information about the requirements and implications of having multiple GALs in your organization, see [Global address lists](address-lists.md#global-address-lists).

For details about recipient filters in the Exchange Management Shell, see the [Recipient filters in the Exchange Management Shell](#recipient-filters-in-the-exchange-management-shell) section in this topic.

To create a GAL, use the following syntax:

```PowerShell
New-GlobalAddressList -Name "<GAL Name>" [<Precanned recipient filter | Custom recipient filter>]
```

This example creates a GAL with a precanned recipient filter:

- **Name**: Contoso GAL

- **Precanned recipient filter**: All recipient types where the **Company** value is Contoso.

```PowerShell
New-GlobalAddressList -Name "Contoso GAL" -IncludedRecipients AllRecipients -ConditionalCompany Contoso
```

This example creates a GAL with a custom recipient filter:

- **Name**: Agency A GAL

- **Custom recipient filter**: All recipient types where the CustomAttribute15 property contains the value AgencyA.

```PowerShell
New-GlobalAddressList -Name "Agency A GAL" -RecipientFilter "CustomAttribute15 -like '*AgencyA*'"
```

For detailed syntax and parameter information, see [New-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/new-globaladdresslist).

#### How do you know this worked?

To verify that you've successfully created a GAL, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, select the address list, and click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) to view the details.

- In the Exchange Management Shell, replace _\<GAL Name\>_ with the name of the GAL, and run the following command to verify the property values:

   ```PowerShell
   Get-GlobalAddressList -Identity "<GAL Name>" | Format-List Name,RecipientFilterType,RecipientContainer,RecipientFilter,IncludedRecipients,Conditional*
   ```

### Use the Exchange Management Shell to modify global address lists

- The same settings are available as when you created the GAL. For more information, see the previous section.

- After you modify the GAL, you need to update its membership. For more information, see the [Use the Exchange Management Shell to update global address lists](#use-the-exchange-management-shell-to-update-global-address-lists) section in this topic.

- You can't replace a custom recipient filter with a precanned recipient filter or vice-versa in an existing GAL.

To modify a GAL, use the following syntax:

```PowerShell
Set-GlobalAddressList -Identity <GALIdentity>] [-Name <Name>] [<Precanned recipient filter | Custom recipient filter>] [-RecipientContainer <OrganizationalUnit>]
```

When you modify the _Conditional_ parameter values, you can use the following syntax to add or remove values without affecting other existing values: `@{Add="<Value1>","<Value2>"...; Remove="<Value1>","<Value2>"...}`.

This example modifies the existing GAL named Contoso GAL by adding the **Company** value Fabrikam to the precanned recipient filter.

```PowerShell
Set-GlobalAddressList -Identity "Contoso GAL" -ConditionalCompany @{Add="Fabrikam"}
```

For detailed syntax and parameter information, see [Set-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/set-globaladdresslist).

#### How do you know this worked?

To verify that you've successfully modified a GAL, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, select the address list, and click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) to view the details.

- In the Exchange Management Shell, replace _\<GAL Name\>_ with the name of the GAL, and run the following command to verify the property values:

  ```PowerShell
  Get-GlobalAddressList -Identity "<GAL Name>" | Format-List Name,RecipientFilterType,RecipientContainer,RecipientFilter,IncludedRecipients,Conditional*
  ```

### Use the Exchange Management Shell to remove global address lists

- You can't remove the GAL named Default Offline Address Book, which is the GAL that's automatically created by Exchange, and the only GAL that has the **IsDefaultGlobalAddressList** property value `True`.

- You can't remove a GAL that's defined in an offline address book (OAB). To modify the address lists that are defined in an OAB, see [Use the Exchange Management Shell to add and remove address lists from offline address books](../offline-address-books/oab-procedures.md#use-the-exchange-management-shell-to-add-and-remove-address-lists-from-offline-address-books).

To remove a GAL, use the following syntax:

```PowerShell
Remove-GlobalAddressList -Identity <GALIdentity>
```

This example removes the address list named Agency A GAL.

```PowerShell
Remove-GlobalAddressList -Identity "Agency A GAL"
```

For detailed syntax and parameter information, see [Remove-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/remove-globaladdresslist).

#### How do you know this worked?

To verify that you've successfully removed a GAL, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, and verify that the GAL is no longer listed.

- In the Exchange Management Shell, run the following command to verify that the GAL isn't listed:

  ```PowerShell
  Get-GlobalAddressList
  ```

## Address list procedures

### Update address lists

After you create or modify an address list in the EAC or the Exchange Management Shell, you need to update the membership of the address list.

- If the address list contains more than 3000 recipients, we recommend that you use the Exchange Management Shell to update the address list. Updating the membership of the address list will take a long time, and will prevent you from using the EAC session until the address list is fully updated.

- If the address list contains fewer than 3000 recipients, it's OK to use the EAC.

#### Use the EAC to update address lists

1. In the EAC, go to **Organization** \> **Address lists**, and select the address list that you want to update.

   - If the address list needs to be updated, you'll see a **Not up to date** section with an **Update** link in the details pane. Click **Update**.

   - If the address list is already up to date, you'll see **This address list is up to date** in the details pane.

2. After you click **Update**, a warning message that appears. Click **Yes** to update the address list by using the EAC. A progress bar allows you to monitor the update process. When the update is complete, click **Close**.

#### Use the Exchange Management Shell to update address lists

To update an address list, use the following syntax:

```PowerShell
Update-AddressList -Identity [<AddressListIdentity>]
```

This example updates the address list named Northwest Executives.

```PowerShell
Update-AddressList -Identity "Northwest Executives"
```

This example updates the address list named Sales that's located under the address list named North America.

```PowerShell
Update-AddressList "North America\Sales"
```

This example updates all address lists in the organization that require updates.

```PowerShell
Get-AddressList | where {$_.RecipientFilterApplied -eq $false} | Update-AddressList
```

For detailed syntax and parameter information, see [Update-AddressList](https://docs.microsoft.com/powershell/module/exchange/update-addresslist).

#### How do you know this worked?

To verify that you've successfully updated an address list, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, select the address list, and verify that you see **This address list is up to date** (instead of **Not up to date** with an **Update** link) in the details pane.

- In the Exchange Management Shell, replace _\<AddressListIdentity\>_ with the name of the address list, and run the following command to verify the **RecipientFilterApplied** property value:

  ```PowerShell
  Get-AddressList -Identity <AddressListIdentity> | Format-Table -Auto Name,RecipientFilterApplied
  ```

### View the members of address lists

If the address list isn't up to date, you should update the address list before you view the members. For more information, see the previous section.

#### Use the EAC to view the members of address lists

1. In the EAC, go to **Organization** \> **Address lists**, and select the address list, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

2. Click **Preview recipients the address list includes**.

#### Use the Exchange Management Shell to view members of address lists

- Technically, this procedure returns *all* recipients (including hidden recipients) that match the recipient filters for the address list. The recipients that are actually visible in the address list have the **HiddenFromAddressListsEnabled** property value `False`.

To view the members of an address list, use the following syntax:

```PowerShell
$AL = Get-AddressList -Identity <AddressListIdentity>; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $AL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled
```

This example returns the members of the address list named Southeast Offices.

```PowerShell
$AL = Get-AddressList -Identity "Southeast Offices"; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $AL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled
```

This example exports the results to the file C:\My Documents\Southeast Offices Export.csv.

```PowerShell
$AL = Get-AddressList -Identity "Southeast Offices"; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $AL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled | Export-Csv -NoTypeInformation -Path "C:\My Documents\Southeast Offices Export.csv"
```

### Create address lists

You can create address lists by using the EAC or the Exchange Management Shell. In the EAC, when you create an address list, you're required to include a recipient filter that's based on the recipient type (specific types or all recipients). In the Exchange Management Shell, you aren't required to include a recipient filter that's based on recipient type.

#### Use the EAC to create address lists

1. In the EAC, go to **Organization** \> **Address lists**, and then click **New** (![Add icon](../../media/ITPro_EAC_AddIcon.png)).

2. In the **Address list** windows that opens, configure the following settings:

   - **Name**: Enter a unique, descriptive name for the address list.

   - **Address list path**: You can create the address list in the root (" **\**", also known as All Address Lists), or you can create the address list under an existing address list. To create the address list under an existing address list, click **Browse**, select the address list in the picker window, and then click **OK**.

   - For details about the recipient filters and preview options that are available here, see the [Recipient filters in the EAC](#recipient-filters-in-the-eac)  section in this topic.

3. When you're finished, click **Save**. You'll receive a warning message that tells you to click **Update** in the details pane to update the membership of the address list. For more information, see the [Update address lists](#update-address-lists) section in this topic.

#### Use the Exchange Management Shell to create address lists

 You can create address lists with or without recipient filters. For details about recipient filters in the Exchange Management Shell, see the [Recipient filters in the Exchange Management Shell](#recipient-filters-in-the-exchange-management-shell) section in this topic.

To create an address list, use the following syntax:

```PowerShell
New-AddressList -Name "<Address List Name>" [-Container <ExistingAddressListPath>] [<Precanned recipient filter | Custom recipient filter>] [-RecipientContainer <OrganizationalUnit>]
```

This example creates an address list with a precanned recipient filter:

- **Name**: Southeast Offices

- **Location**: Under the root (" `\`", also known as All Address Lists) because we didn't use the _Container_ parameter, and the default value is " `\`".

- **Precanned recipient filter**: All users with mailboxes where the **State or province** value is GA, AL, or LA (Georgia, Alabama, or Louisiana).

```PowerShell
New-AddressList -Name "Southeast Offices" -IncludedRecipients MailboxUsers -ConditionalStateorProvince "GA","AL","LA"
```

This example creates an address list with a custom recipient filter:

- **Name**: Northwest Executives

- **Location**: Under the existing address list named North America.

- **Custom recipient filter**: All users with mailboxes where the **Title** value contains Director or Manager, and the **State or province** value is WA, OR, or ID (Washington, Oregon, or Idaho).

```PowerShell
New-AddressList -Name "Northwest Executives" -Container "\North America"-RecipientFilter "(RecipientType -eq 'UserMailbox') -and (Title -like '*Director*' -or Title -like '*Manager*') -and (StateOrProvince -eq 'WA' -or StateOrProvince -eq 'OR' -or StateOrProvince -eq 'ID')"
```

For detailed syntax and parameter information, see [New-AddressList](https://docs.microsoft.com/powershell/module/exchange/new-addresslist).

#### How do you know this worked?

To verify that you've successfully created an address list, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, select the address list, and click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) to view the details.

- In the Exchange Management Shell, replace _[\<AddressListPath\>_\] _\<AddressListName\>_ with the name and (optionally) location of the address list, and run the following command to verify the property values:

  ```PowerShell
  Get-AddressList -Identity "[<AddressListPath>\]<AddressListName>" | Format-List Name,RecipientFilterType,RecipientContainer,RecipientFilter,IncludedRecipients,Conditional*
  ```

### Modify address lists

- If you created an address list with no recipient filters or a custom recipient filter in the Exchange Management Shell, you can't modify the address list in the EAC. You need to use the Exchange Management Shell.

- After you modify an address list, you need to update its membership. For more information, see the [Update address lists](#update-address-lists) section in this topic.

- You can't replace a custom recipient filter with a precanned recipient filter or vice-versa in an existing address list.

- You can change the location of an address list by using the **Move-AddressList** cmdlet in the Exchange Management Shell. For more information, see the [Use the Exchange Management Shell to move address lists](#use-the-exchange-management-shell-to-move-address-lists) section in this topic.

#### Modify address lists in the EAC

1. In the EAC, go to **Organization** \> **Address lists**, select the address list, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

2. In **Address list** windows that opens, configure the following settings:

   - **Display name**: Enter a unique, descriptive name for the address list.

   - For details about the recipient filters and preview options that are available here, see the [Recipient filters in the EAC](#recipient-filters-in-the-eac)  section in this topic.

3. When you're finished, click **Save**. You'll receive a warning message that tells you to click **Update** in the details pane to update the membership of the address list. For more information, see the [Update address lists](#update-address-lists) section in this topic.

#### Modify address lists in the Exchange Management Shell

- The same basic settings are available as when you created the address list. For more information, see the [Use the Exchange Management Shell to create address lists](#use-the-exchange-management-shell-to-create-address-lists) section in this topic.

- You can't use this procedure to move an address list. For more information, see the [Use the Exchange Management Shell to move address lists](#use-the-exchange-management-shell-to-move-address-lists) section in this topic.

To modify an existing address list, use the following syntax:

```PowerShell
Set-AddressList -Identity <AddressListIdentity> [-Name <Name>] [<Precanned recipient filter | Custom recipient filter>] [-RecipientContainer <OrganizationalUnit>]
```

When you modify the _Conditional_ parameter values, you can use the following syntax to add or remove values without affecting other existing values: `@{Add="<Value1>","<Value2>"...; Remove="<Value1>","<Value2>"...}`.

This example modifies the existing address list named Southeast Offices by adding the **State or province** value TX (Texas) to the precanned recipient filter.

```PowerShell
Set-AddressList -Identity "Southeast Offices" -ConditionalStateOrProvince @{Add="TX"}
```

For detailed syntax and parameter information, see [Set-AddressList](https://docs.microsoft.com/powershell/module/exchange/set-addresslist).

#### How do you know this worked?

To verify that you've successfully modified an address list, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, select the address list, and click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) to view the details.

- In the Exchange Management Shell, replace _\<AddressListIdentity\>_ with the path\name of the address list, and run the following command to verify the property values:

  ```PowerShell
  Get-AddressList -Identity "<AddressListIdentity>" | Format-List Name,RecipientFilterType,RecipientContainer,RecipientFilter,IncludedRecipients,Conditional*
  ```

### Use the Exchange Management Shell to move address lists

You can select the location of an address list when you create an address list in the EAC or the Exchange Management Shell. But, you can only move an existing address list by using the **Move-AddressList** cmdlet in the Exchange Management Shell. If the source address list contains child address lists under it, the address list hierarchy is moved to the target location that you specify.

To move an address list, use the following syntax:

```PowerShell
Move-AddressList -Identity "<AddressListIdentity>" -Target "<AddressListIdentity or \>"
```

This example moves the address list named Southeast Offices from the root (" `\`", also known as All Address Lists) to the address list named North America.

```PowerShell
Move-AddressList -Identity "Southeast Offices" -Target "North America"
```

For detailed syntax and parameter information, see [Move-AddressList](https://docs.microsoft.com/powershell/module/exchange/move-addresslist).

#### How do you know this worked?

To verify that you've successfully modified an address list, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, select the address list, and click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) to view the details.

- In the Exchange Management Shell, replace _\<AddressListIdentity\>_ with the path\name of the address list, and run the following command to verify the property values:

  ```PowerShell
  Get-AddressList -Identity "<AddressListIdentity>" | Format-List Name,RecipientFilterType,RecipientContainer,RecipientFilter,IncludedRecipients,Conditional*
  ```

### Remove address lists

If the address list contains more than 3000 recipients, we recommend that you use the Exchange Management Shell to remove the address list. Removing the address list will take a long time, and will prevent you from using the EAC session until the address list is fully removed. If the address list contains less than 3000 recipients, it's OK to use the EAC to remove the address list.

- You can't remove an address list that's defined in an offline address book (OAB). To modify the address lists that are defined in an OAB, see [Use the Exchange Management Shell to add and remove address lists from offline address books](../offline-address-books/oab-procedures.md#use-the-exchange-management-shell-to-add-and-remove-address-lists-from-offline-address-books).

- You can't remove an address list that contains child address lists (you'll receive an error). You first need to do one of the following steps:

  - Use the EAC to remove the parent and all child address lists at the same time.

  - Use the Exchange Management Shell to move all child address lists to another location by using the **Move-AddressList** cmdlet.

#### Use the EAC to remove address lists

1. In the EAC, go to **Organization** \> **Address lists**.

2. Select the address list or lists that you want to remove, and then click **Remove** (![Delete icon](../../media/ITPro_EAC_DeleteIcon.png)). You can select multiple address lists by pressing the CTRL key while selecting each list.

3. Click **Yes** in the warning message that appears. A progress bar allows you to monitor the removal process. When the removal is complete, click **Close**.

#### Use the Exchange Management Shell to remove address lists

To remove an address list, use the following syntax:

```PowerShell
Remove-AddressList -Identity "[<AddressListPath>\]<AddressListName>" [-Recursive]
```

This example removes the address list named Southeast Offices and all its children from under the North America address list.

```PowerShell
Remove-AddressList -Identity "North America\Southeast Offices" -Recursive
```

For detailed syntax and parameter information, see [Remove-AddressList](https://docs.microsoft.com/powershell/module/exchange/remove-addresslist).

#### How do you know this worked?

To verify that you've successfully removed an address list, use either of the following procedures:

- In the EAC, go to **Organization** \> **Address lists**, and verify that the address list is no longer listed.

- In the Exchange Management Shell, run the following command to verify that the address list isn't listed:

  ```PowerShell
  Get-AddressList
  ```

## Hide recipients from address lists

Hiding a recipient from address lists doesn't prevent the recipient from receiving email messages; it prevents users from finding the recipient in address lists. The recipient is hidden from **all** address lists and GALs (effectively, they're exceptions to the recipient filters in all address lists). If you want to selectively include the recipient in some address lists but not others, you need to adjust the recipient filters in the address lists to include or exclude the recipient.

Hiding a mailbox from address lists also prevents Outlook from finding the mailbox in GAL when you create a new profile, or add an additional mailbox to an existing profile. To add the hidden mailbox in Outlook, you can temporarily make the mailbox visible in address lists, configure Outlook, and then hide the mailbox from address lists again.

### Use the EAC to hide recipients from address lists

1. In the EAC, go to one of the following locations based on the recipient type:

   - **Recipients** \> **Mailboxes**: User mailboxes, linked mailboxes, and remote mailboxes.

   - **Recipients** \> **Groups**: Distribution groups, mail-enabled security groups, and dynamic distribution groups.

   - **Recipients** \> **Resources**: Room and equipment mailboxes.

   - **Recipients** \> **Contacts**: Mail users and mail contacts.

   - **Recipients** \> **Shared**: Shared mailboxes.

   - **Public folders** \> **Public folders**: Mail-enabled public folders.

2. Select the recipient that you want to hide from address lists, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

3. The recipient properties window opens. What you do next depends on the recipient type:

   - **Mailboxes, Contacts and Shared**: On the **General** tab, select **Hide from address lists**.

   - **Groups**: On the **General** tab, select **Hide this group from address lists**.

   - **Resources**: On the **General** tab, click **More options**, and then select **Hide from address lists**.

   - **Public folders**: On the **General mail properties** tab, select **Hide from Exchange address list**.

   When you're finished, click **Save**.

### Use the Exchange Management Shell to hide recipients from address lists

To hide a recipient from address lists, use the following syntax:

```PowerShell
Set-<RecipientType> -Identity <RecipientIdentity> -HiddenFromAddressListsEnabled $true
```

 _\<RecipientType\>_ is one of these values:

- `DistributionGroup`

- `DynamicDistributionGroup`

- `Mailbox`

- `MailContact`

- `MailPublicFolder`

- `MailUser`

- `RemoteMailbox`

This example hides the distribution group named Internal Affairs from address lists.

```PowerShell
Set-DistributionGroup -Identity "Internal Affairs" -HiddenFromAddressListsEnabled $true
```

This example hides the mailbox michelle@contoso.com from address lists.

```PowerShell
Set-Mailbox -Identity michelle@contoso.com -HiddenFromAddressListsEnabled $true
```

**Notes**:

- To make the recipient visible in address lists again, use the value `$false` for the _HiddenFromAddressListsEnabled_ parameter.

- By default, arbitration mailboxes and public folder mailboxes are hidden from address lists. If you use the **Set-Mailbox** cmdlet to change this or any other setting for arbitration or public folder mailboxes, you need to include the _Arbitration_ or _PublicFolder_ switches.

### How do you know this worked?

You can verify that you've successfully hidden a recipient from address lists by using any of the following procedures:

- In the EAC, select the recipient, click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) and verify the hide from address lists setting is selected.

- In the Exchange Management Shell, run the following command and verify the recipient is listed:

  ```PowerShell
  Get-Recipient -ResultSize unlimited -Filter "HiddenFromAddressListsEnabled -eq `$true"
  ```

- Open the GAL in Outlook or Outlook on the web (formerly known as Outlook Web App), and verify the recipient isn't visible.

## Recipient filters in the EAC

When you create or modify address lists in the EAC, the following recipient filter settings are available:

- **Types of recipients to include**

  - **All recipients**

    Or

  - **Only the following recipient types**: Select one or more of the following values:

  - **Users with Exchange mailboxes**

  - **Mail users with external email addresses**

  - **Resource mailboxes**

  - **Mail contacts with external email addresses**

  - **Mail-enabled groups**

- **Create rules to further define the recipients**

1. Click **Add rule** and select one of the recipient properties from the drop down list:

   - **Recipient container** (container or organization unit)

   - **State or province**

   - **Company**

   - **Department**

   - Custom attribute 1 to 15

2. Enter a value for the property you selected:

   - If you selected **Recipient container**, a **Select an organizational unit** dialog box appears that allows you to select the container or OU in Active Directory.

   - For other recipient properties, a **Specify words or phrases** dialog appears that allows you to add, edit and remove text values.

   - Property values require an exact match. Wildcards and partial matches aren't supported. For example, the value "Sales" doesn't match "Sales and Marketing".

   - Multiple values of the same property use the **or** operator. For example, "Department equals Sales or Department equals Marketing"

3. After you've selected a property and value, click **Add rule**.

4. Repeat the previous steps to configure more filters. Note that multiple properties use the **and** operator. For example, "Department equals Sales and Company equals Contoso".

   **Preview recipients the address list includes**: When you click this setting, a **Preview** dialog appears that shows you the recipients that are identified by the filters you configured.

## Recipient filters in the Exchange Management Shell

In the Exchange Management Shell, you can specify **precanned recipient filters**, or **custom recipient filters**, but not both at the same time.

- **Precanned recipient filters**

  - Uses the required _IncludedRecipient_ parameter with the `AllRecipients` value *or* one or more of the following values: `MailboxUsers`, `MailContacts`, `MailGroups`, `MailUsers`, or `Resources`. You can specify multiple values separated by commas.

  - You can also use any of the optional _Conditional_ filter parameters: _ConditionalCompany_, _ConditionalCustomAttribute[1to15]_, _ConditionalDepartment_, and _ConditionalStateOrProvince_.

    You specify multiple values for a _Conditional_ parameter by using the syntax `"<Value1>","<Value2>"...`. Multiple values of the same property implies the **or** operator. For example, "Department equals Sales or Marketing or Finance".

- **Custom recipient filters**: Uses the required _RecipientFilter_ parameter with an OPATH filter.

   - The basic OPATH filter syntax is `"<Property1> -<Operator> '<Value1>' <Property2> -<Operator> '<Value2>'..."`.

  - Double quotation marks `" "` are required around the whole OPATH filter. Although the filter is a string (not a system block), you can also use braces `{ }`, but only if the filter doesn't contain variables that require expansion..

  - Hyphens (`-`) are required before all operators. Here are some of the most frequently used operators:

  - `and`, `or`, and `not`.

  - `eq` and `ne` (equals and does not equal; not case-sensitive).

  - `lt` and `gt` (less than and greater than).

  - `like` and `notlike` (string contains and does not contain; requires at least one wildcard in the string. For example, `"Department -like 'Sales*'"`.

  - Use parentheses to group `<Property> -<Operator> '<Value>'` statements together in complex filters. For example, `"(Department -like 'Sales*' -or Department -like 'Marketing*') -and (Company -eq 'Contoso' -or Company -eq 'Fabrikam')"`. Exchange stores the filter in the **RecipientFilter** property with each individual statement enclosed in parentheses, but you don't need to enter them that way.

  - For more information, see [Additional OPATH syntax information](https://docs.microsoft.com/powershell/exchange/recipient-filters#additional-opath-syntax-information).

  - After you use the **New-AddressList** cmdlet to create an address list that uses custom recipient filters, you can't modify the address list in the EAC. You need to use the **Set-AddressList** cmdlet with the _RecipientFilter_ parameter in the Exchange Management Shell.

 **Note**: The _RecipientContainer_ (organizational unit) recipient filter parameter is available to both precanned recipient filters and custom recipient filters.
