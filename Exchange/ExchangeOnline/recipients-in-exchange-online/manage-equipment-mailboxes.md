---
localization_priority: Normal
description: An equipment mailbox is a resource mailbox assigned to a resource that's not location specific, such as a portable computer, projector, microphone, or a company car. After an administrator creates an equipment mailbox, users can easily reserve the piece of equipment by including the corresponding equipment mailbox in a meeting request. You can use the EAC and Exchange Online PowerShell to create an equipment mailbox or change equipment mailbox properties. For more information, see Recipients.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e5f58b3a-83e1-4742-8846-85103a44ee18
ms.reviewer: 
f1.keywords:
- NOCSH
title: Manage equipment mailboxes
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage equipment mailboxes

An equipment mailbox is a resource mailbox assigned to a resource that's not location specific, such as a portable computer, projector, microphone, or a company car. After an administrator creates an equipment mailbox, users can easily reserve the piece of equipment by including the corresponding equipment mailbox in a meeting request. You can use the EAC and Exchange Online PowerShell to create an equipment mailbox or change equipment mailbox properties. For more information, see [Recipients in Exchange Online](recipients-in-exchange-online.md).

For information about another type of resource mailbox, a room mailbox, see [Create and manage room mailboxes](manage-room-mailboxes.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create an equipment mailbox

### Use the EAC to create an equipment mailbox

1. In the EAC, navigate to **Recipients** \> **Resources**.

2. To create an equipment mailbox, click **New** \> **Equipment mailbox**. To create a room mailbox, click **New** \> **Room mailbox**.

3. Use the options on the page to specify the settings for the new resource mailbox.

   - **\* Equipment name**: Use this box to type a name for the equipment mailbox. This is the name that's listed in the resource mailbox list in the EAC and in your organization's address book. This name is required and it can't exceed 64 characters.

   > [!TIP]
   > Although there are other fields that describe the details of the room, for example, Capacity, consider summarizing the most important details in the equipment name using a consistent naming convention. Why? So users can easily see the details when they select the equipment from the address book in a meeting request.

   - **\* Email address**: An equipment mailbox has an email address so it can receive booking requests. The email address consists of an alias on the left side of the @ symbol, which must be unique in the forest, and your domain name on the right. The email address is required.

4. When you're finished, click **Save** to create the equipment mailbox.

Once you've created your equipment mailbox, you can edit your equipment mailbox to update info about booking options, MailTips and delegates. Check out the Change equipment mailbox properties section below to change room mailbox properties

### Use Exchange Online PowerShell to create an equipment mailbox

This example creates an equipment mailbox with the following configuration:

- The equipment mailbox resides on Mailbox Database 1.

- The equipment's name is MotorVehicle2 and the name will display in the GAL as Motor Vehicle 2.

- The email address is MotorVehicle2@contoso.com.

- The mailbox is in the Equipment organizational unit.

- The _Equipment_ parameter specifies that this mailbox will be created as an equipment mailbox.

```PowerShell
New-Mailbox -Database "Mailbox Database 1" -Name MotorVehicle2 -OrganizationalUnit Equipment -DisplayName "Motor Vehicle 2" -Equipment
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox).

### How do you know this worked?

To verify that you've successfully created a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Resources**. The new user mailbox is displayed in the mailbox list. Under **Mailbox Type**, the type is **Equipment**.

- In Exchange Online PowerShell, run the following command to display information about the new equipment mailbox.

  ```PowerShell
  Get-Mailbox <Name> | Format-List Name,RecipientTypeDetails,PrimarySmtpAddress
  ```

## Change equipment mailbox properties

After you create an equipment mailbox, you can make changes and set additional properties by using the EAC or Exchange Online PowerShell.

### Use the EAC to change equipment mailbox properties

1. In the EAC, navigate to **Recipients** \> **Resources**.

2. In the list of resource mailboxes, click the equipment mailbox that you want to change the properties for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. On the equipment mailbox properties page, click one of the following sections to view or change properties.

#### General

Use the **General** section to view or change basic information about the resource.

- **\* Equipment name**: This name appears in the resource mailbox list in the EAC and in your organization's address book. It can't exceed 64 characters if you change it.

- **\* Email address**: This read-only box displays the email address for the equipment mailbox. You can change it in the [Email Address](#email-address) section.

- **Capacity**: Use this box to enter the maximum number of people who can use this resource, if applicable, For example, if the equipment mailbox corresponds to a compact car, you could enter **4**.

Click **More options** to view or change these additional properties:

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the account for the equipment mailbox. You have to use Active Directory Users and Computers to move the account to a different OU.

- **Mailbox database**: This read-only box displays the name of the mailbox database that hosts the equipment mailbox. Use the **Migration** page in the EAC to move the mailbox to a different database.

- **\* Alias**: Use this box to change the alias for the equipment mailbox.

- **Hide from address lists**: Select this check box to prevent equipment mailbox from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send booking messages to the equipment mailbox by using the email address.

- **Department**: Use this box to specify a department name that the resource is associated with. You can use this property to create recipient conditions for dynamic distribution groups and address lists.

- **Company**: Use this box to specify a company that the resource is associated with. Like the Department property, you can use this property to create recipient conditions for dynamic distribution groups and address lists.

- **Address book policy**: Use this option to specify an address book policy (ABP) for the resource. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. To learn more, see [Address book policies](../address-books/address-book-policies/address-book-policies.md).

  In the drop-down list, select the policy that you want associated with this mailbox.

- **Custom attributes**: This section displays the custom attributes defined for the equipment mailbox. To specify custom attribute values, click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif). You can specify up to 15 custom attributes for the recipient.

#### Delegates

Use this section to view or change how the equipment mailbox handles reservation requests and to define who can accept or decline booking requests if it isn't done automatically.

- **Booking requests**: Select one of the following options to handle booking requests.

  - **Accept or decline booking requests automatically**: A valid meeting request automatically reserves the resource. If there's a scheduling conflict with an existing reservation, or if the booking request violates the scheduling limits of the resource, for example, the reservation duration is too long, the meeting request is automatically declined.

  - **Select delegates who can accept or decline booking requests**: Resource delegates are responsible for accepting or declining meeting requests that are sent to the equipment mailbox. If you assign more than one resource delegate, only one of them has to act on a specific meeting request.

- **Delegates**: If you selected the option requiring that booking requests be sent to delegates, the specified delegates are listed. Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) or **Remove** ![Remove icon](../media/ITPro_EAC_RemoveIcon.gif) to add or remove delegates from this list.

#### Booking Options

Use the **Booking Options** section to view or change the settings for the booking policy that defines when the resource can be scheduled, how long it can be reserved, and how far in advance it can be reserved.

- **Allow repeating meetings**: This setting allows or prevents repeating meetings for the resource. By default, this setting is enabled, so repeating meetings are allowed.

- **Allow scheduling only during working hours**: This setting accepts or declines meeting requests that aren't during the working hours defined for the resource. By default, this setting is disabled, so meeting requests are allowed outside the working hours.By default, working hours are 8:00 A.M. to 5:00 P.M. Monday through Friday. You can configure the working hours of the equipment mailbox in the Appearance section on the Calendar page.

- **Always decline if the end date is beyond this limit**: This setting controls the behavior of repeating meetings that extend beyond the date specified by the maximum booking lead time setting.

  - If you enable this setting, a repeating booking request is automatically declined if the bookings start on or before the date specified by the value in the **Maximum booking lead time** box, and they extend beyond the specified date. This is the default setting.

  - If you disable this setting, a repeating booking request is automatically accepted if the booking requests start on or before the date specified by the value in the **Maximum booking lead time** box, and they extend beyond the specified date. However, the number of bookings is reduced so bookings won't occur after the specified date.

- **Maximum booking lead time (days)**: This setting specifies the maximum number of days in advance that the resource can be booked. Valid input is an integer between 0 and 1080. The default value is 180 days.

- **Maximum duration (hours)**: This setting specifies the maximum duration that the resource can be reserved in a booking request. The default value is 24 hours.

  For repeating booking requests, the maximum booking duration applies to the length of each instance of the repeating booking request.

There is also a box on this page that you can use to write a message that will be sent to users who send meeting requests to reserve the resource.

#### Contact Information

Use the **Contact Information** section to view or change the contact information for the resource. The information on this page is displayed in the address book.

> [!TIP]
> You can use the **State/Province** box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

#### Email Address

Use the **Email Address** section to view or change the email addresses associated with the equipment mailbox. This includes the mailbox's primary SMTP address and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type. <br/><br/> When you add a new email address, you have the option to make it the primary SMTP address.

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization.

#### MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues before they send a booking request to the equipment mailbox. A MailTip is text that's displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Use Exchange Online PowerShell to change equipment mailbox properties

Use the following sets of cmdlets to view and change equipment mailbox properties: **Get-Mailbox** and **Set-Mailbox** cmdlets to view and change general properties and email addresses for equipment mailboxes. Use the **Get-CalendarProcessing** and **Set-CalendarProcessing** cmdlets to view and change delegates and booking options.

- **Get-User** and **Set-User**: Use these cmdlets to view and set general properties such as department and company names.

- **Get-Mailbox** and **Set-Mailbox**: Use these cmdlets to view and set mailbox properties, such as email addresses and the mailbox database.

- **Get-CalendarProcessing** and **Set-CalendarProcessing**: Use these cmdlets to view and set booking options and delegates.

For information about these cmdlets, see the following topics:

- [Get-User](https://docs.microsoft.com/powershell/module/exchange/get-user)

- [Set-User](https://docs.microsoft.com/powershell/module/exchange/set-user)

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

- [Get-CalendarProcessing](https://docs.microsoft.com/powershell/module/exchange/get-calendarprocessing)

- [Set-CalendarProcessing](https://docs.microsoft.com/powershell/module/exchange/set-calendarprocessing)

Here are some examples of using Exchange Online PowerShell to change equipment mailbox properties.

This example changes the display name and primary SMTP address (called the default reply address) for the MotorPool 1 equipment mailbox. The previous reply address is kept as a proxy address.

```PowerShell
Set-Mailbox "MotorPool 1" -DisplayName "Motor Pool 1 - Compact" -EmailAddresses SMTP:MP1.compact@contoso.com,smtp:MP.1@contoso.com
```

This example configures equipment mailboxes to allow booking requests to be scheduled only during working hours.

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'EquipmentMailbox'" | Set-CalendarProcessing -ScheduleOnlyDuringWorkHours $true
```

This example uses the **Get-User** cmdlet to find all equipment mailboxes in the Audio Visual department, and then uses the **Set-CalendarProcessing** cmdlet to send booking requests to a delegate named Ann Beebe to accept or decline.

```PowerShell
Get-User -ResultSize unlimited -Filter "(RecipientTypeDetails -eq 'EquipmentMailbox') -and (Department -eq 'Audio Visual')" | Set-CalendarProcessing -AllBookInPolicy $false -AllRequestInPolicy $true -ResourceDelegates "Ann Beebe"
```

### How do you know this worked?

To verify that you've successfully changed properties for an equipment mailbox, do the following:

- In the EAC, select the mailbox and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected mailbox.

- In Exchange Online PowerShell, use the **Get-Mailbox** cmdlet to verify the changes. One advantage of using Exchange Online PowerShell is that you can view multiple properties for multiple mailboxes. In the example above where booking requests could be scheduled only during working hours, run the following command to verify the new value.

  ```PowerShell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'EquipmentMailbox'" | Get-CalendarProcessing | Format-List Identity,ScheduleOnlyDuringWorkHours
  ```
