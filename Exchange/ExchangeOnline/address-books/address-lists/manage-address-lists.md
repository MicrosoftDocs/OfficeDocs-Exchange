---
localization_priority: Normal
description: Admins can learn how to create, view the membership of, update, modify, remove, and hide recipients from address lists in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: cac74760-7bd1-482c-8d43-b0165e988ec0
ms.reviewer:
title: Manage address lists in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Manage address lists in Exchange Online

An address list is a collection of mail-enabled recipient objects in Exchange Online. Address lists are based on recipient filters. For more information about address lists, see [Address lists in Exchange Online](address-lists.md).

For additional management tasks related to manage address lists, see [Address list procedures in Exchange Online](address-list-procedures.md).

Looking for the Exchange Server version of this topic? See [Create address lists](https://docs.microsoft.com/Exchange/email-addresses-and-address-books/address-lists/address-list-procedures#create-address-lists).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can only use Exchange Online PowerShell to perform virtually all of the procedures in this topic (everything except hiding recipients from address lists). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to create address lists

You can create address lists with or without recipient filters. For details about recipient filters, see [Recipient filters for address lists in Exchange Online PowerShell](use-recipient-filters-to-create-an-address-list.md).

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


This example creates the address list named Oregon and Washington Users by using the _RecipientFilter_ parameter and includes recipients that are mailbox users and have **StateOrProvince** set to `Washington` or `Oregon`.

```PowerShell
New-AddressList -Name "Oregon and Washington" -RecipientFilter "((RecipientType -eq 'UserMailbox') -and ((StateOrProvince -eq 'Washington') -or (StateOrProvince -eq 'Oregon')))"
```

This example creates the child address list Building 34 Meeting Rooms in the All Rooms parent container, using built-in conditions.

```PowerShell
New-AddressList -Name "Building 34 Meeting Rooms" -Container "\All Rooms" -IncludedRecipients Resources -ConditionalCustomAttribute1 "Building 34"
```

For detailed syntax and parameter information, see [New-AddressList](https://docs.microsoft.com/powershell/module/exchange/new-addresslist).

### How do you know this worked?

To verify that you've successfully created an address list, replace _\<AddressListIdentity\>_ with the path\name of the address list, and run the following command in Exchange Online Powershell to verify the property values:

```PowerShell
Get-AddressList -Identity "<AddressListIdentity>" | Format-List Name,RecipientFilterType,RecipientFilter,IncludedRecipients,Conditional*
```

## Use Exchange Online Powershell to view members of address lists

Technically, this procedure returns *all* recipients (including hidden recipients) that match the recipient filters for the address list. The recipients that are actually visible in the address list have the **HiddenFromAddressListsEnabled** property value `False`.

To view the members of an address list, use the following syntax:

```PowerShell
$<VariableName> = Get-AddressList -Identity <AddressListIdentity>; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $<VariableName>.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled
```

This example returns the members of the address list named Southeast Offices.

```PowerShell
$AL = Get-AddressList -Identity "Southeast Offices"; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $AL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled
```

This example exports the results to the file C:\My Documents\Southeast Offices Export.csv.

```PowerShell
$AL = Get-AddressList -Identity "Southeast Offices"; Get-Recipient -ResultSize unlimited -RecipientPreviewFilter $AL.RecipientFilter | select Name,PrimarySmtpAddress,HiddenFromAddressListsEnabled | Export-Csv -NoTypeInformation -Path "C:\My Documents\Southeast Offices Export.csv"
```

## Use Exchange Online PowerShell to update address lists

The **Update-AddressList** cmdlet (or **Update-GlobalAddressList**) isn't available in Exchange Online PowerShell. If recipients that should appear an address list do not, you need to change the required property value for those users to a temporary value, and then back to the value that's required by the address list. You can update the user property values in the Exchange admin center (EAC) or Exchange Online PowerShell, but it's quicker to do bulk operations in PowerShell.

For example, suppose the address list named Oregon and Washington Users uses the filter `"((RecipientType -eq 'UserMailbox') -and ((StateOrProvince -eq 'Washington') -or (StateOrProvince -eq 'Oregon')))"`, but the address list doesn't include everyone whose **StateOrProvince** property values are set correctly. To update the address list, perform the following steps:

1. Use the query from the address list to find all users that should be in the address list. For example:

   ```PowerShell
   $Before = Get-User -Filter "((RecipientType -eq 'UserMailbox') -and ((StateOrProvince -eq 'Oregon') -or (StateOrProvince -eq 'Washington')))" -ResultSize Unlimited
   ```

2. Change the required property to a temporary value. For example, change the **StateOrProvince** values from `Oregon` to `OR`, and `Washington` to `WA`:

   ```PowerShell
   $Before | where {$_.StateOrProvince -eq 'Oregon'} | foreach {Set-User $_.Identity -StateOrProvince OR}
   ```

   ```PowerShell
   $Before | where {$_.StateOrProvince -eq 'Washington'} | foreach {Set-User $_.Identity -StateOrProvince WA}
   ```

3. Find those same users again by using the temporary property values. For example:

   ```PowerShell
   $After = Get-User -Filter "((RecipientType -eq 'UserMailbox') -and ((StateOrProvince -eq 'OR') -or (StateOrProvince -eq 'WA')))" -ResultSize Unlimited
   ```

4. Change the temporary value back to the required value. For example, change the **StateOrProvince** values from `OR` to `Oregon`, and `WA` to `Washington`:

   ```PowerShell
   $After | where {$_.StateOrProvince -eq 'OR'} | foreach {Set-User $_.Identity -StateOrProvince Oregon}
   ```

   ```PowerShell
   $After | where {$_.StateOrProvince -eq 'WA'} | foreach {Set-User $_.Identity -StateOrProvince Washington}
   ```

**Notes:**

- Title, department and address properties require the **Get-User** and **Set-User** cmdlets. CustomAttribute1 through CustomAttribute15 properties require the **Get-Mailbox** and **Set-Mailbox** cmdlets. For more information about what properties are available on which cmdlet, see the following topics:

   - [Set-User](https://docs.microsoft.com/powershell/module/exchange/set-user)

   - [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

- If a only small number of users don't appear in the address list, you can modify the required property value for each user. For example:

   1. Set a temporary property value for the user:

      ```PowerShell
      Set-User -Identity <UserIdentity> -StateOrProvince WA
      ```

   2. Change the temporary value back to the required value:

      ```PowerShell
      Set-User -Identity <Identity> -StateOrProvince Washington
      ```

### How do you know this worked?

To verify that you've successfully updated an address list, replace _\<AddressListIdentity\>_ with the name of the address list, and run the following command in Exchange Online PowerShell to verify the **RecipientFilterApplied** property value:

```PowerShell
Get-AddressList -Identity <AddressListIdentity> | Format-Table Name,RecipientFilterApplied -Auto
```

## Use Exchange Online PowerShell to modify address lists

The same basic settings are available as when you created the address list. For more information, see the [Use Exchange Online PowerShell to create address lists](#use-exchange-online-powershell-to-create-address-lists) section in this topic.

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

### How do you know this worked?

To verify that you've successfully modified an address list, replace _\<AddressListIdentity\>_ with the path\name of the address list, and run the following command in Exchange Online Powershell to verify the property values:

```PowerShell
Get-AddressList -Identity "<AddressListIdentity>" | Format-List Name,RecipientFilterType,RecipientFilter,IncludedRecipients,Conditional*
```

## Use Exchange Online PowerShell to delete address lists

To remove an address list, use the following syntax:

```PowerShell
Remove-AddressList -Identity "<AddressListName>"
```

This example removes the address list Sales Department, which doesn't contain child address lists.

```PowerShell
Remove-AddressList -Identity "Sales Department"
```

For detailed syntax and parameter information, see [Remove-AddressList](https://docs.microsoft.com/powershell/module/exchange/remove-addresslist).

### How do you know this worked?

To verify that you've successfully removed an address list, run the following command in Exchange Online Powershell to verify that the address list isn't listed:

```PowerShell
Get-AddressList
```

## Hide recipients from address lists

Hiding a recipient from address lists doesn't prevent the recipient from receiving email messages; it prevents users from finding the recipient in address lists. The recipient is hidden from **all** address lists and GALs (effectively, they're exceptions to the recipient filters in all address lists). If you want to selectively include the recipient in some address lists but not others, you need to adjust the recipient filters in the address lists to include or exclude the recipient.

### Use the EAC to hide recipients from address lists

To open the EAC, see [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

You can't use the EAC to hide Microsoft 365 groups from address lists.

1. In the EAC, go to one of the following locations based on the recipient type:

   - **Recipients** \> **Mailboxes**: User mailboxes.

   - **Recipients** \> **Groups**: Distribution groups, mail-enabled security groups, and dynamic distribution groups.

   - **Recipients** \> **Resources**: Room and equipment mailboxes.

   - **Recipients** \> **Contacts**: Mail users and mail contacts.

   - **Recipients** \> **Shared**: Shared mailboxes.

   - **Public folders** \> **Public folders**: Mail-enabled public folders.

2. Select the recipient that you want to hide from address lists, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

3. The recipient properties window opens. What you do next depends on the recipient type:

   - **Mailboxes**, **Contacts**, and **Shared**: On the **General** tab, select **Hide from address list**.

   - **Groups**: On the **General** tab, select **Hide this group from address lists**.

   - **Resources**: On the **General** tab, click **More options**, and then select **Hide from address lists**.

   - **Public folders**: On the **General mail properties** tab, select **Hide from Exchange address list**.

   When you're finished, click **Save**.

### Use Exchange Online PowerShell to hide recipients from address lists

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

- `UnifiedGroup`

This example hides the distribution group named Internal Affairs from address lists.

```PowerShell
Set-DistributionGroup -Identity "Internal Affairs" -HiddenFromAddressListsEnabled $true
```

This example hides the mailbox michelle@contoso.com from address lists.

```PowerShell
Set-Mailbox -Identity michelle@contoso.com -HiddenFromAddressListsEnabled $true
```

 **Note**: To make the recipient visible in address lists again, use the value `$false` for the _HiddenFromAddressListsEnabled_ parameter.

### How do you know this worked?

You can verify that you've successfully hidden a recipient from address lists by using any of the following procedures:

- In the EAC, select the recipient, click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) and verify the hide from address lists setting is selected.

- In Exchange Online PowerShell, run the following command and verify the recipient is listed:

   ```PowerShell
   Get-Recipient -ResultSize unlimited -Filter 'HiddenFromAddressListsEnabled -eq $true'
   ```

- Open the GAL in Outlook or Outlook on the web (formerly known as Outlook Web App), and verify the recipient isn't visible.
