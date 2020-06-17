---
title: 'Create a custom management scope for In-Place eDiscovery searches: Exchange 2013 Help'
TOCTitle: Create a custom management scope for In-Place eDiscovery searches
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 1543aefe-3709-402c-b9cd-c11fe898aad1
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a custom management scope for In-Place eDiscovery searches in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use a custom management scope to let specific people or groups use In-Place eDiscovery to search a subset of mailboxes in your Exchange 2013 organization. For example, you might want to let a discovery manager search only the mailboxes of users in a specific location or department. You can do this by creating a custom management scope. This custom management scope uses a recipient filter to control which mailboxes can be searched. Recipient filter scopes use filters to target specific recipients based on recipient type or other recipient properties.

For In-Place eDiscovery, the only property on a user mailbox that you can use to create a recipient filter for a custom scope is distribution group membership (the actual property name is _MemberOfGroup_). If you use other properties, such as _CustomAttributeN_, _Department_, or _PostalCode_, the search fails when it's run by a member of the role group that's assigned the custom scope.

To learn more about management scopes, see:

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

- [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- As previously stated, you can only use group membership as the recipient filter to create a custom recipient filter scope that is intended to be used for eDiscovery. Any other recipient properties can't be used to create a custom scope for eDiscovery searches. Note that membership in a dynamic distribution group can't be used either.

- Perform steps 1 through 3 to let a discovery manager export the search results for an eDiscovery search that uses a custom management scope.

- If your discovery manager doesn't need to preview the search results, you can skip step 4.

- If your discovery manager doesn't need to copy the search results, you can skip step 5.

## Step 1: Organize users into distribution groups for eDiscovery

To search a subset of mailboxes in your organization or to narrow the scope of source mailboxes that a discovery manager can search, you'll need to group the subset of mailboxes into one or more distribution groups. When you create a custom management scope in step 2, you'll use these distribution groups as the recipient filter to create a custom management scope. This allows a discovery manager to search only the mailboxes of the users who are members of a specified group.

You might be able to use existing distribution groups for eDiscovery purposes, or you can create new ones. See the [More information](#more-information) section at the end of this topic for tips on how to create distribution groups that can be used to scope eDiscovery searches.

## Step 2: Create a custom management scope

Now you'll create a custom management scope that's defined by the membership of a distribution group (using the _MemberOfGroup_ recipient filter). When this scope is applied to a role group used for eDiscovery, members of the role group can search the mailboxes of users who are members of the distribution group that was used to create the custom management scope.

This procedure uses Exchange Management Shell commands to create a custom scope named Ottawa Users eDiscovery Scope. It specifies the distribution group named Ottawa Users for the recipient filter of the custom scope.

1. Run this command to get and save the properties of the Ottawa Users group to a variable, which is used in the next command.

   ```powershell
   $DG = Get-DistributionGroup -Identity "Ottawa Users"
   ```

2. Run this command to create a custom management scope based on the membership of the Ottawa Users distribution group.

   ```powershell
   New-ManagementScope "Ottawa Users eDiscovery Scope" -RecipientRestrictionFilter "MemberOfGroup -eq '$($DG.DistinguishedName)'"
   ```

    The distinguished name of the distribution group, which is contained in the variable **$DG**, is used to create the recipient filter for the new management scope.

## Step 3: Create a management role group

In this step, you create a new management role group and assign the custom scope that you created in step 2. Add the Legal Hold and Mailbox Search roles so that role group members can perform In-Place eDiscovery searches and place mailboxes on In-Place Hold or Litigation Hold. You can also add members to this role group so they can search the mailboxes of the members of the distribution group used to create the custom scope in step 2.

In the following examples, the Ottawa Users eDiscovery Managers security group will be added as members this role group. You can use either the Shell or the EAC for this step.

### Use the Shell to create a management role group

Run this command to create a new role group that uses the custom scope created in step 2. The command also adds the Legal Hold and Mailbox Search roles, and adds the Ottawa Users eDiscovery Managers security group as members of the new role group.

```powershell
New-RoleGroup "Ottawa Discovery Management" -Roles "Mailbox Search","Legal Hold" -CustomRecipientWriteScope "Ottawa Users eDiscovery Scope" -Members "Ottawa Users eDiscovery Managers"
```

### Use the EAC to create a management role group

1. In the EAC, go to **Permissions** \> **Admin roles**, and then click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

2. In **New role group**, provide the following information:

   - **Name**: Provide a descriptive name for the new role group. For this example, you'd use Ottawa Discovery Management.

   - **Write scope**: Select the custom management scope that you created in step 2. This scope will be applied to the new role group.

   - **Roles**: Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif), and add the **Legal Hold** and **Mailbox Search** roles to the new role group.

   - **Members**: Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif), and select the users, security group, or role groups that you want add as members of the new role group. For this example, the members of the **Ottawa Users eDiscovery Managers** security group will be able to search only the mailboxes of users who are members of the **Ottawa Users** distribution group.

3. Click **Save** to create the role group.

   Here's an example of what the **New role group** window will look like when you're done.

   ![Create a new role group for a custom scope](images/TA_MRM_eDiscoveryCustomRoleGroup.gif)

## (Optional) Step 4: Add discovery managers as members of the distribution group used to create the custom management scope

You only need to perform this step if you want to let a discovery manager preview eDiscovery search results.

Run this command to add the Ottawa Users eDiscovery Managers security group as a member of the Ottawa Users distribution group.

```powershell
Add-DistributionGroupMember -Identity "Ottawa Users" -Member "Ottawa Users eDiscovery Managers"
```

You can also use the EAC to add members to a distribution group. For more information, see [Create and manage distribution groups](manage-distribution-groups-exchange-2013-help.md).

## (Optional) Step 5: Add a discovery mailbox as a member of the distribution group used to create the custom management scope

You only need to perform this step if you want to let a discovery manager copy eDiscovery search results.

Run this command to add a discovery mailbox named Ottawa Discovery Mailbox as a member of the Ottawa Users distribution group.

```powershell
Add-DistributionGroupMember -Identity "Ottawa Users" -Member "Ottawa Discovery Mailbox"
```

> [!NOTE]
> To open a discovery mailbox and view the search results, discovery managers must be assigned Full Access permissions for the discovery mailbox. For more information, see [Create a discovery mailbox](create-a-discovery-mailbox-exchange-2013-help.md).

## How do you know this worked?

Here are some ways to verify if you've successfully implemented custom management scopes for eDiscovery. When you verify, be sure that the user running the eDiscovery searches is a member of the role group that uses the custom management scope.

- Create an eDiscovery search, and select the distribution group that was used to create the custom management scope as the source of mailboxes to be searched. All mailboxes should be successfully searched.

- Create an eDiscovery search, and search the mailboxes of any users who aren't members of the distribution group that was used to create the custom management scope. The search should fail because the discovery manager can only search mailboxes for users who are members of the distribution group that was used to create the custom management scope. In this case, an error such as "Unable to search mailbox \< _name of mailbox_\> because the current user does not have permissions to access the mailbox" will be returned.

- Create an eDiscovery search, and search the mailboxes of users who are members of the distribution group that was used to create the custom management scope. In the same search, include the mailboxes of users who aren't members. The search should partially succeed. The mailboxes of members of the distribution group used to create the custom management scope should be successfully searched. The search of mailboxes for users who aren't members of the group should fail.

## More information

- Because distribution groups are used in this scenario to scope eDiscovery searches and not for message delivery, consider the following when you create and configure distribution groups for eDiscovery:

  - Create distribution groups with a closed membership so that members can be added to or removed from the group only by the group owners. If you're creating the group in the Shell, use the syntax `MemberJoinRestriction closed` and `MemberDepartRestriction closed`.

  - Enable group moderation so that any message sent to the group is first sent to the group moderators who can approve or reject the message accordingly. If you're creating the group in the Shell, use the syntax `ModerationEnabled $true`. If you're using the EAC, you can enable moderation after the group is created.

  - Hide the distribution group from the organization's shared address book. Use the EAC or the **Set-DistributionGroup** cmdlet after the group is created. If you're using the Shell, use the syntax `HiddenFromAddressListsEnabled $true`.

    In the following example, the first command creates a distribution group with closed membership and moderation enabled. The second command hides the group from the shared address book.

    ```powershell
    New-DistributionGroup -Name "Vancouver Users eDiscovery Scope" -Alias VancouverUserseDiscovery -MemberJoinRestriction closed -MemberDepartRestriction closed -ModerationEnabled $true
    ```

    ```powershell
    Set-DistributionGroup "Vancouver Users eDiscovery Scope" -HiddenFromAddressListsEnabled $true
    ```

    For more information about creating and managing distribution groups, see [Create and manage distribution groups](manage-distribution-groups-exchange-2013-help.md).

- Though you can use only distribution group membership as the recipient filter for a custom management scope used for eDiscovery, you can use other recipient properties to add users to that distribution group. Here are some examples of using the **Get-Mailbox** and **Get-Recipient** cmdlets to return a specific group of users based on common user or mailbox attributes.

  ```powershell
  Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'Department -eq "HR"'
  ```

  ```powershell
  Get-Mailbox -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'CustomAttribute15 -eq "VancouverSubsidiary"'
  ```

   ```powershell
  Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'PostalCode -eq "98052"'
   ```

   ```powershell
  Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'StateOrProvince -eq "WA"'
   ```

  ```powershell
  Get-Mailbox -RecipientTypeDetails UserMailbox -ResultSize unlimited -OrganizationalUnit "namsr01a002.sdf.exchangelabs.com/Microsoft Exchange Hosted Organizations/contoso.onmicrosoft.com"
  ```

- You can then use the examples from the previous bullet to create a variable that can be used with the **Add-DistributionGroupMember** cmdlet to add a group of users to a distribution group. In the following example, the first command creates a variable that contains all user mailboxes that have the value **Vancouver** for the _Department_ property in their user account. The second command adds these users to the Vancouver Users distribution group.

  ```powershell
  $members = Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'Department -eq "Vancouver"'
  ```

  ```powershell
  $members | ForEach {Add-DistributionGroupMember "Ottawa Users" -Member $_.Name}
  ```

- You can use the **Add-RoleGroupMember** cmdlet to add a member to an existing role group that's used to scope eDiscovery searches. For example, the following command adds the user admin@ottawa.contoso.com to the Ottawa Discovery Management role group.

  ```powershell
  Add-RoleGroupMember "Vancouver Discovery Management" -Member paralegal@vancouver.contoso.com
  ```

  You can also use the EAC to add members to a role group. For more information, see the "Add members to a role group" section in [Manage Role Group Members](https://technet.microsoft.com/library/c064729d-7cda-47fc-b105-acf4b300d430.aspx).
