---
title: 'Create an address list: Exchange 2013 Help'
TOCTitle: Create an address list
ms:assetid: e86ba1b7-c41c-4050-bc29-13996cf53c59
ms:mtpsurl: https://technet.microsoft.com/library/Bb125036(v=EXCHG.150)
ms:contentKeyID: 49289446
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.Mailbox.NewAddressListWizardForm.AddressListIntroductionPage
---

# Create an address list

_**Applies to:** Exchange Server 2013_

Address lists are a collection of recipient and other Active Directory objects. Each address list can contain one or more types of objects (for example, users, contacts, groups, public folders, conferencing, and other resources). Address lists also provide a mechanism to partition mail-enabled objects in Active Directory for the benefit of specific groups of users.

For other management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to create an address list

1. Navigate to **Organization** \> **Address lists**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In **Address List**, type a name and specify the types of recipients to include in the list.

3. By default, Exchange creates address lists that contain all members of your organization. To create a unique custom address list, click **Add a rule**.

    > [!IMPORTANT]
    > If you don't add a rule, you'll create an address list that's redundant with one of the default address lists.

4. In the list, select a filtering option (for example, **Custom attribute 1**).

5. In **Specify words or phrases**, type words or phrases to filter by, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"), and then click **OK**.

    You can continue to add several phrases or words by repeating Step 4. The filter is a Boolean **OR** statement. For example, you can create a filter that will apply the address list to users whose Custom 1 attribute equals **Oregon**, **Idaho**, or **Washington**.

6. (Optional) Click **Add a rule** again to add additional filters. Additional filters create a Boolean **And** statement. The more filters you add, the fewer number of users the address list will apply to.

7. Click **Preview recipients the address lists includes** to see the recipients that this address list is going to apply to.

8. Click **Save**.

9. You'll get a warning that the address list won't be applied until you update it. Depending on the size of your organization and the filters that you added to the address list, some address lists can contain thousands or tens of thousands of recipients. Updating address lists can impact your resources, so you may want to update the address during off-peak hours.

    For details about updating an address list, see [Update an address list](update-an-address-list-exchange-2013-help.md).

## Use the Shell to create an address list

This example creates the address list MyAddressList by using the *RecipientFilter* parameter and includes recipients that are mailbox users and have `StateOrProvince` set to `Washington` or `Oregon`.

```powershell
New-AddressList -Name MyAddressList -RecipientFilter "((RecipientType -eq 'UserMailbox') -and ((StateOrProvince -eq 'Washington') -or (StateOrProvince -eq 'Oregon')))"
```

This example creates the child address list Building 34 Meeting Rooms in the All Rooms parent container, using built-in conditions.

```powershell
New-AddressList -Name "Building 34 Meeting Rooms" -Container "\All Rooms" -IncludedRecipients Resources -ConditionalCustomAttribute1 "Building 34"
```

For detailed syntax and parameter information, see [New-AddressList](https://docs.microsoft.com/powershell/module/exchange/New-AddressList).
