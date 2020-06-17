---
localization_priority: Normal
description: Admins can learn about mailbox plans and how view, modify, and set the default mailbox plan in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 
ms.reviewer: 
f1.keywords:
- NOCSH
title: Mailbox plans in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Mailbox plans in Exchange Online

A mailbox plan is a template that automatically configures mailbox properties in Exchange Online. Mailbox plans correspond to Office 365 license types. When you assign a license to a new user, the corresponding mailbox plan is used to configure the settings on the new mailbox that's created. If you change the license that's assigned to an existing user, the settings in the mailbox plan that's associated with the new license are applied to the user's existing mailbox.

The following table describes the mailbox plans that you're likely to see in Exchange Online.

|**Subscription or license**|**Mailbox plan display name**|
|:-----|:-----|
|Exchange Online Kiosk|ExchangeOnlineDeskless|
|Office 365 Enterprise E1 <br/><br/> Exchange Online Plan 1|ExchangeOnline|
|Office 365 Enterprise E3 <br/><br/> Office 365 Enterprise E5 <br/><br/> Exchange Online Plan 2|ExchangeOnlineEnterprise|
|Microsoft 365 Business Basic |ExchangeOnlineEssentials|

**Notes**:

- The availability of a mailbox plan in your organization is determined by your selection when you enroll in Office 365. A subscription might contain multiple mailbox plans. A mailbox plan might not be available to you based on your subscription or the age of your organization.

- The name value of the mailbox plan is appended with `-<GUID>` (for example, `ExchangeOnlineEnterprise-44107b46-a8c4-4573-a7ba-bb004fde4d58`).

For every mailbox plan (returned by the **Get-MailboxPlan** cmdlet), there's a corresponding Client Access services (CAS) mailbox plan (returned by the **Get-CasMailboxPlan** cmdlet). The names and display names of the mailbox plans and CAS mailbox plans are identical, and the relationship between them is unbreakable (both the mailbox plan and the corresponding CAS mailbox plan are assigned to the mailbox when you license the user; you can't assign just the mailbox plan or just the CAS mailbox plan separately).

The _modifiable_ settings that are available in mailbox plans by using the **Set-MailboxPlan** cmdlet are described in the following table:

|**Setting**|**Default value**|**Description**|
|:-----|:-----|:-----|
|_IssueWarningQuota_|Varies by license.|The user receives a warning message when their mailbox reaches the specified size. <br/><br/> For more information, see [Capacity alerts](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#capacity-alerts).|
|_MaxReceiveSize_|Varies by license.|The maximum total message size that can be received by the mailbox. This value is roughly 33% larger than the actual message size to account for Base64 encoding. <br/><br/> For more information, see [Message limits across Office 365 options](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#message-limits-across-office-365-options).|
|_MaxSendSize_|Varies by license.|The maximum total message size that can be sent from the mailbox. This value is roughly 33% larger than the actual message size to account for Base64 encoding. <br/><br/> For more information, see [Message limits across Office 365 options](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#message-limits-across-office-365-options).|
|_ProhibitSendQuota_|Varies by license.|The user receives a warning message and they can't send messages when their mailbox reaches the specified size (which must be greater than the _IssueWarningQuota_ value). <br/><br/> For more information, see [Capacity alerts](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#capacity-alerts).|
|_ProhibitSendReceiveQuota_|Varies by license.|The user receives a warning message and they can't send or receive messages when their mailbox reaches the specified size (which must be greater than the _ProhibitSendQuota_ value). <br/><br/> For more information, see [Capacity alerts](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#capacity-alerts).|
|_RetainDeletedItemsFor_|`14.00:00:00` (14 days)|Depending on your subscription, you can change this value up to 30 days. For more information, see [Change how long permanently deleted items are kept for an Exchange Online mailbox](change-deleted-item-retention.md).|
|_RetentionPolicy_|Default MRM Policy|For more information, see [Retention tags and retention policies in Exchange Online](../../security-and-compliance/messaging-records-management/retention-tags-and-policies.md).|
|_RoleAssignmentPolicy_|Default Role Assignment Policy|Grants users permissions to their own mailbox and distribution groups. For more information, see [Role assignment policies](../../permissions-exo/permissions-exo.md#role-assignment-policies).|

The _modifiable_ settings that are available in CAS mailbox plans by using the **Set-CasMailboxPlan** cmdlet are described in the following table:

|**Setting**|**Default value**|**Description**|
|:-----|:-----|:-----|
|_ActiveSyncEnabled_|True|Enables or disables Exchange ActiveSync (EAS) access to the mailbox.|
|_ImapEnabled_|Varies by license.|Enables or disables IMAP4 access to the mailbox.|
|_OwaMailboxPolicy_|OwaMailboxPolicy-Default|Configures the user's settings in Outlook on the web (formerly known as Outlook Web App). For more information about Outlook on the web mailbox policies, see [Outlook on the web mailbox policies in Exchange Online](../../clients-and-mobile-in-exchange-online/outlook-on-the-web/outlook-web-app-mailbox-policies.md).|
|_PopEnabled_|True|Enables or disables POP3 access to the mailbox.|

Modifying the settings of a mailbox plan won't update the settings of an existing mailbox that's already has the mailbox plan applied. To modify these settings on a existing mailbox, you can:

- Modify the corresponding mailbox settings directly in the Exchange admin center (EAC) or in Exchange Online PowerShell (the **Set-Mailbox** and **Set-CasMailbox** cmdlets).

- Assign a different license to the user. The mailbox plan that corresponds to the new license will be applied to the existing mailbox (the settings in the mailbox plan will be applied to the existing mailbox).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox settings" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to view mailbox plans

These examples return a summary list of all mailbox plans:

```PowerShell
Get-MailboxPlan
```

```PowerShell
Get-CasMailboxPlan
```

These examples return the modifiable property values in all mailbox plans:

```PowerShell
Get-MailboxPlan | Format-List DisplayName,IsDefault,Max*Size,IssueWarningQuota,Prohibit*Quota,RetainDeletedItemsFor,RetentionPolicy,RoleAssignmentPolicy
```

```PowerShell
Get-CasMailboxPlan | Format-List DisplayName,ActiveSyncEnabled,ImapEnabled,PopEnabled,OwaMailboxPolicy
```

These examples return detailed information for the mailbox plan named ExchangeOnlineEnterprise.

```PowerShell
Get-MailboxPlan -Identity ExchangeOnlineEnterprise | Format-List
```

```PowerShell
Get-CasMailboxPlan -Identity ExchangeOnlineEnterprise | Format-List
```

This example returns the mailbox plan that's assigned to the user named Suk-Jae Yoo.

```PowerShell
Get-Mailbox -Identity "Suk-Jae Yoo" | Format-List MailboxPlan
```

To return all mailboxes that had a specific mailbox plan applied:

1. Run the following command to find the distinguished name of the mailbox plan:

   ```PowerShell
   Get-MailboxPlan | Format-List DisplayName,DistinguishedName
   ```

2. Use the following syntax to return the mailboxes that have the mailbox plan assigned:

   ```PowerShell
   Get-Mailbox -ResultSize unlimited -Filter "MailboxPlan -eq '<MailboxPlanDistinguishedName>'"
   ```

   This example returns the mailboxes that have the ExchangeOnline mailbox plan applied.

   ```PowerShell
   Get-Mailbox -ResultSize unlimited -Filter "MailboxPlan -eq 'CN=ExchangeOnline-93f46670-2ae7-4591-baa4-ee153e090945,OU=constoso.onmicrosoft.com,OU=Microsoft Exchange Hosted Organizations,DC=NAMPR22B009,DC=PROD,DC=OUTLOOK,DC=COM'"
   ```

For detailed syntax and parameter information, see [Get-MailboxPlan](https://docs.microsoft.com/powershell/module/exchange/get-mailboxplan) and [Get-CasMailboxPlan](https://docs.microsoft.com/powershell/module/exchange/get-casmailboxplan).

## Use Exchange Online PowerShell to specify the default mailbox plan

The default mailbox plan is used as the default template for new mailboxes that you create without a license (because the license specifies the mailbox plan).

To specify the default mailbox plan, use the following syntax:

```PowerShell
Set-MailboxPlan -Identity <MailboxPlanIdentity> -IsDefault
```

This example specifies the ExchangeOnline mailbox plan as the default.

```PowerShell
Set-MailboxPlan -Identity ExchangeOnline -IsDefault
```

For detailed syntax and parameter information, see [Set-MailboxPlan](https://docs.microsoft.com/powershell/module/exchange/set-mailboxplan).

### How do you know this worked?

To verify that you've successfully specified the default mailbox plan, use any of the following steps:

- In Exchange Online PowerShell, run the following command to verify the property values:

   ```PowerShell
   Get-MailboxPlan | Format-Table DisplayName,IsDefault -Auto
   ```

- Create a new mailbox without assigning a license as described in [Create user mailboxes in Exchange Online](../create-user-mailboxes.md). Replace \<MailboxIdentity\> with the name, alias, account name, or email address of the mailbox, and run the following command in Exchange Online PowerShell to verify the **MailboxPlan** property value:

   ```PowerShell
   Get-Mailbox -Identity <MailboxIdentity> | Format-List MailboxPlan
   ```

## Use Exchange Online PowerShell to modify mailbox plans

To modify a mailbox plan, use the following syntax:

```PowerShell
Set-MailboxPlan -Identity <MailboxPlanIdentity> [-MaxReceiveSize <Size>] [-MaxSendSize <Size>] [-IssueWarningQuota <Size>] [-ProhibitSendQuota <Size>] [-ProhibitSendReceiveQuota <Size>] [-RetainDeletedItemsFor <TimeSpan>] [-RetentionPolicy <RetentionPolicyIdentity>] [-RoleAssignmentPolicy <RoleAssignmentPolicyIdentity>]
```

```PowerShell
Set-CASMailboxPlan -Identity <MailboxPlanIdentity> [-ActiveSyncEnabled <$true | $false>] [-ImapEnabled <$true | $false>] [-PopEnabled <$true | $false>] [-OwaMailboxPolicy <PolicyIdentity>]
```

This example modifies the mailbox plan named ExchangeOnlineEnterprise to use the retention policy named Contoso Retention Policy.

```PowerShell
Set-MailboxPlan -Identity -RetentionPolicy "Contoso Retention Policy"
```

This example disables Exchange ActiveSync, POP3, and IMAP4 access to mailboxes in all CAS mailbox plans.

```PowerShell
Get-CASMailboxPlan | Set-CASMailboxPlan -ActiveSyncEnabled $false -ImapEnabled $false -PopEnabled $false
```

For detailed syntax and parameter information, see [Set-MailboxPlan](https://docs.microsoft.com/powershell/module/exchange/set-mailboxplan) and [Set-CasMailboxPlan](https://docs.microsoft.com/powershell/module/exchange/set-casmailboxplan).

### How do you know this worked?

To verify that you've successfully modified a mailbox plan, use any of the following steps:

- In Exchange Online PowerShell, run the following commands to verify the property values:

   ```PowerShell
   Get-MailboxPlan | Format-List DisplayName,IsDefault,Max*Size,IssueWarningQuota,Prohibit*Quota,RetainDeletedItemsFor,RetentionPolicy,RoleAssignmentPolicy
   ```

   ```PowerShell
   Get-CasMailboxPlan | Format-List DisplayName,ActiveSyncEnabled,ImapEnabled,PopEnabled,OwaMailboxPolicy
   ```

- Using the license that corresponds to the modified mailbox plan, do one of the following steps:

   - Create a new mailbox and assign the license as described in [Create user mailboxes in Exchange Online](../create-user-mailboxes.md).

   - Assign the license to an existing mailbox user who currently has a different license (therefore, mailbox plan) assigned.

   Replace \<MailboxIdentity\> with the name, alias, account name, or email address of the mailbox, and run the following commands in Exchange Online PowerShell to verify the property values:

   ```PowerShell
   Get-Mailbox -Identity "<MailboxIdentity>" | Format-List MailboxPlan,Max*Size,IssueWarningQuota,Prohibit*Quota,RetainDeletedItemsFor,RetentionPolicy,RoleAssignmentPolicy
   ```

   ```PowerShell
   Get-CasMailbox -Identity "<MailboxIdentity>" | Format-List ActiveSyncEnabled,ImapEnabled,PopEnabled,OwaMailboxPolicy
   ```
