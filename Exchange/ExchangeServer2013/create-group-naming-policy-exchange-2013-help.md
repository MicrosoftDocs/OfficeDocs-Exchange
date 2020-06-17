---
title: 'Create a distribution group naming policy: Exchange 2013 Help'
TOCTitle: Create a distribution group naming policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: b2ffb654-345d-4be1-be8e-83d28901373e
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a distribution group naming policy in Exchange 2013

_**Applies to:** Exchange Server 2013_

A group naming policy lets you standardize and manage the names of distribution groups created by users in your organization. You can require a specific prefix and suffix be added to the name for a distribution group when it's created, and you can block specific words from being used. This helps you minimize the use of inappropriate words in group names.

A group naming policy:

- Enforces a consistent naming strategy for groups created by users.

- Identifies distribution groups in the shared address book.

- Suggests the function or membership of the group.

- Identifies the type of users who are likely members of the group.

- Identifies the geographic region the group is used in.

- Blocks inappropriate words in group names.

How does a group naming policy work? When a user creates a group, they specify a name in the Display Name field. After the group is created, Microsoft Exchange applies the group naming policy by adding any prefix or suffix that you've defined in the group naming policy. The full name is displayed in the distribution groups list in the Exchange admin center (EAC), the shared address book, and the To:, Cc:, and From: fields in email messages. If a user tries to use a word that you've blocked, they get an error message when they try to save the new group and are asked to remove the blocked word and save the group again.

Here are some examples of a group naming policy. In each, **\<Group Name\>** is a descriptive name provided by the person who creates the group. Exchange adds the prefixes and suffixes defined by the policy to the display name when the group is created.

- Text strings, with underscore characters, used for a single prefix (DG) and suffix (Users):

  DG_\<Group Name\>_Users

- Multiple prefixes (DG and Contoso) and one suffix (Users), using text strings:

  DG_Contoso_\<Group Name\>_Users

- An attribute (Department) used for the prefix:

  Department_\<Group Name\>

  For example, say that your school populates the Department attribute for faculty members. Here's an example of a group name created by a faculty member in the Psychology department:

  **Psychology\_Cognitive201**

  In this example, the underscore character (\_) is provided as the only text string in a second prefix to separate the department name from the group name.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution Groups" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- The maximum length for a group name is 64 characters. This includes the combined number of characters in the prefix, the group name provided by the user, and the suffix.

- The group naming policy is applied only to groups created by users. When you or other administrators use the EAC to create distribution groups, the group naming policy is ignored and not applied to the group name.

- Group names are created without spacing. We recommend that you use an underscore character (_) or some other placeholder between text strings, attributes, and the group name.

- You can use Windows PowerShell to override the group naming policy when you create and edit a distribution group. For more information, see [Override the distribution group naming policy](override-group-naming-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to create a group naming policy

1. In the EAC, select **Groups** \> **More** ![More Options Icon](images/ITPro_EAC_MoreOptionsIcon.gif) \> **Configure group naming policy**.

2. Under **Group Naming Policy**, configure the prefix by selecting either **Attribute** or **Text** in the pull-down menu.

   - **Attribute** Select the attribute and then click **OK**.

   - **Text** Type the text string and click **OK**.

     Notice that the text string that you typed or the attribute you selected is displayed as a hyperlink. Click the hyperlink to change the text string or attribute.

3. Click **Add** to add additional prefixes.

4. For the suffix, in the pull-down menu, select either **Attribute** or **Text**, and configure the suffix.

5. Click **Add** to add additional suffixes.

   After you add a prefix or suffix, notice that a preview of the group naming policy is displayed.

6. To delete a prefix or suffix from the policy, click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

7. Click **Blocked Words** to add or remove blocked words.

   - To add a word to the list, type the word to block and click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

   - To remove a word from the list, select it and click **Remove**.

   - To edit an existing blocked word, select it and click **Edit**.

8. When you are finished, click **Save**.

## How do you know this worked?

To verify that you've successfully created a group naming policy, do the following:

- In the EAC, select **Groups** \> **More** \> **Configure group naming policy**.

  On the **Group naming policy** page, the group naming policy that you defined is displayed under **Preview of policy**.

- In Windows PowerShell, run the following command to display the group naming policy.

  ```powershell
  Get-OrganizationConfig | Format-List DistributionGroupNamingPolicy
  ```
