---
localization_priority: Normal
description: You can apply an Outlook on the web mailbox policy to one or more mailboxes or remove one using either the EAC or Exchange Online PowerShell.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 51d8e269-b0d5-4bc7-9b3d-0460871e54fa
ms.reviewer: 
title: Apply or remove an Outlook on the web mailbox policy on a mailbox in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Apply or remove an Outlook on the web mailbox policy on a mailbox in Exchange Online

Assigning an Outlook on the web mailbox policy to a mailbox controls the Outlook on the web (formerly known as Outlook Web App) experience for the user. You can apply Outlook on the web mailbox policies to one or more mailboxes or remove the policy assignments in the Exchange admin center (EAC) or Exchange Online PowerShell.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook on the web mailbox policies" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Apply Outlook on the web mailbox policies to mailboxes

### Use the EAC to apply an Outlook on the web mailbox policy to a mailbox

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. Do one of the following steps:

   - Select a mailbox and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

     1. In the properties of the mailbox window that opens, click **Mailbox features**.

     2. In the **Email connectivity** section under **Outlook on the web: Enabled**, click **View details**.

     3. In the **Outlook Web App mailbox policy** policy window that opens, click **Browse** to find and select the policy to apply, and then click **OK** when you're finished. By default, the default policy named **OwaMailboxPolicy-Default** is applied.

     4. When you're finished, click **Save** multiple times.

   - Select multiple mailboxes.

     1. In the Details pane, find **Outlook on the web** and click **Assign a policy**.

     2. In the bulk assign window that opens, click **Browse** to find and select the policy to apply, and then click **OK** when you're finished.

     3. When you're finished, click **Save**.

### Use Exchange Online PowerShell to apply an Outlook on the web mailbox policy to a mailbox

There are three basic methods you can use to apply an Outlook on the web mailbox policy to mailboxes:

- **Individual mailboxes**: Use the following syntax:

    ```PowerShell
    Set-CasMailbox -Identity <MailboxIdentity> -OwaMailboxPolicy "<Policy Name>"
    ```

    This example applies the Outlook on the web mailbox policy named Sales Associates to tony@contoso.com.

    ```PowerShell
    Set-CASMailbox -Identity tony@contoso.com -OwaMailboxPolicy "Sales Associates"
    ```

- **Filter mailboxes by attributes**: This method requires that the mailboxes all share a unique filterable attribute. For example:

  - Title, Department, or address information for user accounts as seen by the **Get-User** cmdlet.

  - CustomAttribute1 through CustomAttribute15 for mailboxes by as seen the **Get-Mailbox** cmdlet.

  The syntax uses the following two commands (one to identify the mailboxes, and the other to apply the policy to the mailboxes):

  ```PowerShell
  $<VariableName> = <Get-User | Get-Mailbox> -ResultSize unlimited -Filter <Filter>
  ```

  ```PowerShell
  $<VariableName> | foreach {Set-CasMailbox -Identity $_.MicrosoftOnlineServicesID -OwaMailboxPolicy "<Policy Name>"}
  ```

  This example assigns the policy named Managers and Executives to all mailboxes whose **Title** attribute contains "Manager" or "Executive".

  ```PowerShell
  $Mgmt = Get-User -ResultSize unlimited -Filter "(RecipientType -eq 'UserMailbox') -and (Title -like '*Manager*' -or Title -like '*Executive*')"
  ```

  ```PowerShell
  $Mgmt | foreach {Set-CasMailbox -Identity $_.MicrosoftOnlineServicesID -OwaMailboxPolicy "Managers and Executives"}
  ```

- **Use a list of specific mailboxes**: This method requires a text file to identify the mailboxes. Values that don't contain spaces (for example, the user account) work best. The text file must contain one user account on each line like this:

  > akol@contoso.com <br/> ljohnston@contoso.com <br/> kakers@contoso.com

  The syntax uses the following two commands (one to identify the user accounts, and the other to apply the policy to those users):

  ```PowerShell
  $<VariableName> = Get-Content "<text file>"
  ```

  ```PowerShell
  $<VariableName> | foreach {Set-CasMailbox -Identity $_ -OwaMailboxPolicy "<Policy Name>"}
  ```

  This example assigns the policy named Managers and Executives to the mailboxes specified in the file C:\My Documents\Management.txt.

  ```PowerShell
  $Mgrs = Get-Content "C:\My Documents\Management.txt"
  ```

  ```PowerShell
  $Mgrs | foreach {Set-CasMailbox -Identity $_ -OwaMailboxPolicy "Managers and Executives"}
  ```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

### How do you know this worked?

To verify that you've applied an Outlook on the web mailbox policy to a mailbox, use any of the following steps:

- In the EAC, go to **Recipients** \> **Mailboxes** and select the mailbox. In the Details pane, go to **Email Connectivity**, click **View details**, and verify the name of the policy in the **Outlook Web App mailbox policy** window that appears.

- In the EAC, go to **Recipients** \> **Mailboxes**, select the mailbox, and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the properties of the mailbox window that opens, click **Mailbox features**. In the **Email connectivity** section under **Outlook on the web: Enabled**, click **View details**, and verify the name of the policy in the **Outlook Web App mailbox policy** window that appears.

- In Exchange Online PowerShell, replace \<MailboxIdentity\> with the name, alias, email address, or account name of the mailbox, and run the following command to verify the value of the **OwaMailboxPolicy** property:

  ```PowerShell
  Get-CasMailbox -Identity "<MailboxIdentity>" | Format-List OwaMailboxPolicy
  ```

- In Exchange Online PowerShell, run the following command to verify the value of the **OwaMailboxPolicy** property for all mailboxes:

  ```PowerShell
  Get-CasMailbox -ResultSize unlimited | Format-Table Name,OwaMailboxPolicy -Auto
  ```

## Remove an Outlook on the web mailbox policy assignments from mailboxes

### Use the EAC to remove an Outlook on the web mailbox policy assignment from a mailbox

1. In the EAC, go to **Recipients** \> **Mailboxes**, and select the mailbox that you want to modify.

2. Scroll down in the details pane to **Email Connectivity** and click **View details**.

   If a mailbox policy has been assigned, click **Clear** **X** to remove the policy assignment from the mailbox.

3. When you're finished, click **Save** to save.

### Use Exchange Online PowerShell to remove an Outlook on the web mailbox policy assignment from a mailbox

To remove the policy assignment from the mailbox, use the following syntax:

```PowerShell
Set-CasMailbox -Identity "<MailboxIdentity>" -OwaMailboxPolicy $null
```

This example removes the Outlook on the web mailbox policy from mailbox of the user tony@contoso.com.

```PowerShell
Set-CASMailbox -Identity tony@contoso.com -OwaMailboxPolicy $null
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

### How do you know this worked?

To verify that you've removed an Outlook on the web mailbox policy assignment from a mailbox, use any of the following steps:

- In the EAC, go to **Recipients** \> **Mailboxes** and select the mailbox. In the Details pane, go to **Email Connectivity**, click **View details**, and verify the policy is blank in the **Outlook Web App mailbox policy** window that appears.

- In the EAC, go to **Recipients** \> **Mailboxes**. In the properties of the mailbox window that opens, click **Mailbox features**. In the **Email connectivity** section under **Outlook on the web: Enabled**, click **View details**, and verify the policy is blank in the **Outlook Web App mailbox policy** window that appears.

- In Exchange Online PowerShell, replace \<MailboxIdentity\> with the name, alias, email address, or account name of the mailbox, and run the following command to verify the value of the **OwaMailboxPolicy** property:

  ```PowerShell
  Get-CasMailbox -Identity "<MailboxIdentity>" | Format-List OwaMailboxPolicy
  ```

- In Exchange Online PowerShell, run the following command to verify the value of the **OwaMailboxPolicy** property:

  ```PowerShell
  Get-CasMailbox -ResultSize unlimited | Format-Table Name,OwaMailboxPolicy -Auto
  ```
