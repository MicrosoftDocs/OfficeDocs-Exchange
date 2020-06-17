---
localization_priority: Normal
description: Admins can learn how to increase or decrease the space that's available to store Inbox rules in mailboxes in an Exchange Online organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3f01edde-1cdc-4891-ad9d-7d01582664e9
ms.reviewer: 
title: Modify the space used by Inbox rules in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Modify the space used by Inbox rules in Exchange Online

Inbox rules in Outlook on the web (formerly known as Outlook Web App) and Outlook are limited to 256 KB total for all rules. Each rule you create will take up space in your mailbox. The actual amount of space a rule uses depends on several factors, such as how long the name is and how many conditions you've applied. When you reach the 256 KB limit, you'll be warned that you can't create any more rules or that you can't update a rule. You can't increase the amount of space that's allocated to store Inbox rules in Exchange Online, but you can decrease it to suit your business needs.

**Notes**:

- The valid range for the Inbox rules quota is 32 KB to 256 KB.

- There isn't a maximum number of rules that users can create.

- The quota for Inbox rules applies only to _enabled_ rules. There's no restriction on the number of _disabled_ rules that a mailbox can have. However, the _total size_ of rules that are enabled or active in the mailbox can't exceed the quota value

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes or less.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox settings" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- You can only use Exchange Online PowerShell to perform the procedure in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to increase the limit for Inbox rules

There are three basic methods you can use to modify the rules quota for a mailbox:

- **Individual mailboxes**: Use the following syntax:

    ```PowerShell
    Set-Mailbox -Identity <MailboxIdentity> -RulesQuota "<32 KB to 256 KB>"
    ```

    This example decreases the rules quota to 200 KB for the user douglas@contoso.com.

    ```PowerShell
    Set-Mailbox -Identity douglas@contoso.com -RulesQuota "200 KB"
    ```

- **Filter mailboxes by attributes**: This method requires that the mailboxes all share a unique filterable attribute. For example:

    - Title, Department, or address information for user accounts as seen by the **Get-User** cmdlet.

    - CustomAttribute1 through CustomAttribute15 for mailboxes by as seen the **Get-Mailbox** cmdlet.

    The syntax uses the following two commands (one to identify the mailboxes, and the other to apply the rules quota to the mailboxes):

    ```PowerShell
    $<VariableName> = <Get-User | Get-Mailbox> -ResultSize unlimited -Filter <Filter>
    ```

    ```PowerShell
    $<VariableName> | foreach {Set-Mailbox -Identity $_.MicrosoftOnlineServicesID -RulesQuota "<32 KB to 256 KB>"}
    ```

    This example decreases the rules quota to 32 KB to all mailboxes whose **Title** attribute contains "Vendor" or "Contractor".

    ```PowerShell
    $V = Get-User -ResultSize unlimited -Filter "(RecipientType -eq 'UserMailbox') -and (Title -like '*Vendor*' -or Title -like '*Contractor*')"
    ```

    ```PowerShell
    $V | foreach {Set-Mailbox -Identity $_.MicrosoftOnlineServicesID -RulesQuota "32 KB"}
    ```

- **Use a list of specific mailboxes**: This method requires a text file to identify the mailboxes. Values that don't contain spaces (for example, the user account) work best. The text file must contain one user account on each line like this:

  > akol@contoso.com <br/> tjohnston@contoso.com <br/> kakers@contoso.com

  The syntax uses the following two commands (one to identify the user accounts, and the other to apply the rules quota to those users):

  ```PowerShell
  $<VariableName> = Get-Content "<text file>"
  ```

  ```PowerShell
    $<VariableName> | foreach {Set-Mailbox -Identity $_ RulesQuota "<32 KB to 256 KB>"}
  ```

  This example decreases the rules quota to 150 KB to the mailboxes specified in the file C:\My Documents\Junior Managers.txt.

  ```PowerShell
  $Jr = Get-Content "C:\My Documents\Junior Managers.txt"
  ```

  ```PowerShell
  $Jr | foreach {Set-Mailbox -Identity $_ -RulesQuota "150 KB"}
  ```

## How do you know this worked?

To verify that you've modified the Inbox rules quota on a mailbox, use any of the following steps in Exchange Online PowerShell:

- Replace \<MailboxIdentity\> with the name, alias, email address, or account name of the mailbox, and run the following command to verify the value of the **RulesQuota** property:

    ```PowerShell
    Get-Mailbox -Identity "<MailboxIdentity>" | Format-List RulesQuota
    ```

- Run the following command to verify the value of the **RulesQuota** property for all mailboxes:

    ```PowerShell
    Get-Mailbox -ResultSize unlimited | Format-Table Name,RulesQuota -Auto
    ```

## What else do I need to know?

- Inbox rules are run from top to bottom in the order in which they appear in the **Rules** window. To change the order of rules, click the rule you want to move, and then click the up or down arrow to move the rule to the position you want in the list.

- When you create a forwarding rule, you can add more than one address to forward to. The number of addresses you can forward to may be limited, depending on the settings for your account. If you add more addresses than are allowed, your forwarding rule won't work. If you create a forwarding rule with more than one address, test it to be sure it works.
