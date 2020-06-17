---
localization_priority: Normal
description: Learn how to create and modify room mailboxes in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f70752ad-fce0-4e14-8428-fc5ac63f6c54
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create and manage room mailboxes
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create and manage room mailboxes

A room mailbox is a resource mailbox that's assigned to a physical location, such as a conference room, an auditorium, or a training room. After an administrator creates room mailboxes, users can easily reserve rooms by including room mailboxes in meeting requests. For more details, check out [Recipients in Exchange Online](recipients-in-exchange-online.md).

For info about another type of resource mailbox, check out [Manage equipment mailboxes](manage-equipment-mailboxes.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

> [!IMPORTANT]
> If you're running Exchange Server in a hybrid scenario, make sure you create the room mailboxes in the appropriate place. Create your room mailboxes for your on-premises organization on-premises, and room mailboxes for Exchange Online side should be created in the cloud.

### Create a room mailbox

#### Use the Exchange admin center to create a room mailbox

1. In the Exchange admin center, navigate to **Recipients** \> **Resources**.

2. To create a room mailbox, click **New** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) \> **Room mailbox**.

3. Use the options on the page to specify the settings for the new resource mailbox.

   - **\* Room name**: Use this box to type a name for the room mailbox. This is the name that's listed in the resource mailbox list in the Exchange admin center and in your organization's address book. This name is required and it can't exceed 64 characters.

    > [!TIP]
    > Although there are other fields that describe the details of the room, for example, Location and Capacity, consider summarizing the most important details in the room name using a consistent naming convention. Why? So users can easily see the details when they select the room from the address book in the meeting request.

   - **\* Email address**: A room mailbox has an email address so it can receive booking requests. The email address consists of an alias on the left side of the @ symbol, which must be unique in the forest, and your domain name on the right. The email address is required.

   - **Location**, **Phone**, **Capacity**: You can use these fields to enter details about the room. However, as explained earlier, you can include some or all of this information in the room name so users can see it.

4. When you're finished, click **Save** to create the room mailbox.

Once you've created your room mailbox, you can edit your room mailbox to update info about booking options, MailTips and mailbox delegation. Check out the Use the Exchange admin center section below to change room mailbox properties.

#### Use Exchange Online PowerShell to create a room mailbox

This example creates a room mailbox with the following configuration:

- The mailbox's name is ConfRoom1. This name will also be used to create the room's email address.

- The display name in the Exchange admin center and the address book will be Conference Room 1.

- The _Room_ switch specifies that this mailbox will be created as a room mailbox.

```PowerShell
New-Mailbox -Name ConfRoom1 -DisplayName "Conference Room 1" -Room
```

For detailed syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox).

#### How do you know this worked?

You can make sure you've created the room mailbox correctly a couple of different ways:

- In the Exchange admin center, navigate to **Recipients** \> **Resources**. The new room mailbox is displayed in the mailbox list. Under **Mailbox Type**, the type is **Room**.

- In Exchange Online PowerShell, run the following command to display information about the new room mailbox.

  ```PowerShell
  Get-Mailbox <Name> | Format-List Name,RecipientTypeDetails,PrimarySmtpAddress
  ```

### Create a room list

If you're planning to have more than a hundred rooms, or already have more than a hundred rooms created, use a room list to help you organize your rooms. If your company has several buildings with rooms that can be booked for meetings, it might help to create room lists for each building. Room lists are specially marked distribution groups that you can use the same way you use distribution groups. However, you can only create room lists using Exchange Online PowerShell.

#### Use Exchange Online PowerShell to create a room list

This example creates a room list for building 32.

```PowerShell
New-DistributionGroup -Name "Building 32 Conference Rooms" -OrganizationalUnit "contoso.com/rooms" -RoomList
```

#### Use Exchange Online PowerShell to add a room to a room list

This example adds confroom3223 to the building 32 room list.

```PowerShell
Add-DistributionGroupMember -Identity "Building 32 Conference Rooms" -Member confroom3223@contoso.com
```

#### Use Exchange Online PowerShell to convert a distribution group to a room list

You may already have created distribution groups in the past that contain your conference rooms. You don't need to recreate them; we can convert them quickly into a room list.

This example converts the distribution group, building 34 conference rooms, to a room list.

```PowerShell
Set-DistributionGroup -Identity "Building 34 Conference Rooms" -RoomList
```

### Change room mailbox properties

After you create a room mailbox, you can make changes and set additional properties by using the Exchange admin center or Exchange Online PowerShell.

#### Use the Exchange admin center to change room mailbox properties

1. In the Exchange admin center, navigate to **Recipients** \> **Resources**.

2. In the list of resource mailboxes, click the room mailbox that you want to change the properties for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. On the room mailbox properties page, click one of the following sections to view or change properties.

#### General

Use the **General** section to view or change basic information about the resource.

- **\* Room name**: This name appears in the resource mailbox list in the Exchange admin center and in your organization's address book. It can't exceed 64 characters if you change it.

- **\* Email address**: This read-only box displays the email address for the room mailbox. You can change it in the [Email Address](#email-address) section.

- **Capacity**: Use this box to enter the maximum number of people who can safely occupy the room.

Click **More options** to view or change these additional properties:

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the account for the room mailbox. You have to use Active Directory Users and Computers to move the account to a different OU.

- **Mailbox database**: This read-only box displays the name of the mailbox database that hosts the room mailbox. Use the **Migration** page in the Exchange admin center to move the mailbox to a different database.

- **\* Alias**: Use this box to change the alias for the room mailbox.

- **Hide from address lists**: Select this check box to prevent the room mailbox from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send booking messages to the room mailbox by using the email address.

- **Department**: Use this box to specify a department name that the room is associated with. You can use this property to create recipient conditions for dynamic distribution groups and address lists.

- **Company**: Use this box to specify a company that the room is associated with, if applicable. Like the Department property, you can use this property to create recipient conditions for dynamic distribution groups and address lists.

- **Address book policy**: Use this option to specify an address book policy (ABP) for the room mailbox. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. To learn more, see [Address book policies](../address-books/address-book-policies/address-book-policies.md).

    In the drop-down list, select the policy that you want associated with this mailbox.

- **Custom attributes**: This section displays the custom attributes defined for the room mailbox. To specify custom attribute values, click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif). You can specify up to 15 custom attributes for the recipient.

#### Delegates

Use this section to view or change how the room mailbox handles reservation requests and to define who can accept or decline booking requests if it isn't done automatically.

- **Booking requests**: Select one of the following options to handle booking requests.

  - **Accept or decline booking requests automatically**: A valid meeting request automatically reserves the room. If there's a scheduling conflict with an existing reservation, or if the booking request violates the scheduling limits of the resource, for example, the reservation duration is too long, the meeting request is automatically declined.

  - **Select delegates who can accept or decline booking requests**: Resource delegates are responsible for accepting or declining meeting requests that are sent to the room mailbox. If you assign more than one resource delegate, only one of them has to act on a specific meeting request.

- **Delegates**: If you selected the option requiring that booking requests be sent to delegates, the specified delegates are listed. Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) or **Remove** ![Remove icon](../media/ITPro_EAC_RemoveIcon.gif) to add or remove delegates from this list.

#### Booking Options

Use the **Booking Options** section to view or change the settings for the booking policy that defines when the room can be scheduled, how long it can be reserved, and how far in advance it can be reserved.

- **Allow repeating meetings**: This setting allows or prevents repeating meetings for the room. By default, this setting is enabled, so repeating meetings are allowed.

- **Allow scheduling only during working hours**: This setting accepts or declines meeting requests that aren't during the working hours defined for the room. By default, this setting is disabled, so meeting requests are allowed outside the working hours. By default, working hours are 8:00 A.M. to 5:00 P.M. Monday through Friday. You can configure the working hours of the room mailbox in the Appearance section on the Calendar page.

- **Always decline if the end date is beyond this limit**: This setting controls the behavior of repeating meetings that extend beyond the date specified by the maximum booking lead time setting.

  - If you enable this setting, a repeating booking request is automatically declined if the bookings start on or before the date specified by the value in the **Maximum booking lead time** box, and they extend beyond the specified date. This is the default setting.

  - If you disable this setting, a repeating booking request is automatically accepted if booking requests start on or before the date specified by the value in the **Maximum booking lead time** box, and they extend beyond the specified date. However, the number of bookings is reduced so bookings won't occur after the specified date.

- **Maximum booking lead time (days)**: This setting specifies the maximum number of days in advance that the room can be booked. Valid input is an integer between 0 and 1080. The default value is 180 days.

- **Maximum duration (hours)**: This setting specifies the maximum duration that the room can be reserved in a booking request. The default value is 24 hours.

    For repeating booking requests, the maximum booking duration applies to the length of Exchange admin center instance of the repeating booking request.

There's also a box on this page that you can use to write a message that will be sent to users who send booking requests to reserve the room.

#### Contact Information

Use the **Contact Information** section to view or change the contact information for the room. The information on this page is displayed in the address book.

> [!TIP]
> You can use the **State/Province** box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

#### Email Address

Use the **Email Address** section to view or change the email addresses associated with the room mailbox. This includes the mailbox's primary SMTP address and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type. <br/><br/> When you add a new email address, you have the option to make it the primary SMTP address.

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization.

#### MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues before they send a booking request to the room mailbox. A MailTip is text that's displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

#### Use Exchange Online PowerShell to change room mailbox properties

Use the following sets of cmdlets to view and change room mailbox properties: **Get-Mailbox** and **Set-Mailbox** cmdlets to view and change general properties and email addresses for room mailboxes. Use the **Get-CalendarProcessing** and **Set-CalendarProcessing** cmdlets to view and change delegates and booking options.

- **Get-User** and **Set-User**: Use these cmdlets to view and set general properties such as location, department, and company names.

- **Get-Mailbox** and **Set-Mailbox**: Use these cmdlets to view and set mailbox properties, such as email addresses and the mailbox database.

- **Get-CalendarProcessing** and **Set-CalendarProcessing**: Use these cmdlets to view and set booking options and delegates.

For information about these cmdlets, see the following topics:

- [Get-User](https://docs.microsoft.com/powershell/module/exchange/get-user)

- [Set-User](https://docs.microsoft.com/powershell/module/exchange/set-user)

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

- [Get-CalendarProcessing](https://docs.microsoft.com/powershell/module/exchange/get-calendarprocessing)

- [Set-CalendarProcessing](https://docs.microsoft.com/powershell/module/exchange/set-calendarprocessing)

Here are some examples of using Exchange Online PowerShell to change room mailbox properties.

This example changes the display name, the primary SMTP address (called the default reply address), and the room capacity. Also, the previous reply address is kept as a proxy address.

```PowerShell
Set-Mailbox "Conf Room 123" -DisplayName "Conf Room 31/123 (12)" -EmailAddresses SMTP:Rm33.123@contoso.com,smtp:rm123@contoso.com -ResourceCapacity 12
```

This example configures room mailboxes to allow booking requests to be scheduled only during working hours and sets a maximum duration of 9 hours.

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'RoomMailbox'" | Set-CalendarProcessing -ScheduleOnlyDuringWorkHours $true -MaximumDurationInMinutes 540
```

This example uses the **Get-User** cmdlet to find all room mailboxes that correspond to private conference rooms, and then uses the **Set-CalendarProcessing** cmdlet to send booking requests to a delegate named Robin Wood to accept or decline.

```PowerShell
Get-User -ResultSize unlimited -Filter "(RecipientTypeDetails -eq 'RoomMailbox') -and (DisplayName -like 'Private*')" | Set-CalendarProcessing -AllBookInPolicy $false -AllRequestInPolicy $true -ResourceDelegates "Robin Wood"
```

#### How do you know this worked?

To verify that you've successfully changed properties for a room mailbox, do the following:

- In the Exchange admin center, select the mailbox and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected mailbox.

- In Exchange Online PowerShell, use the **Get-Mailbox** cmdlet to verify the changes. One advantage of using Exchange Online PowerShell is that you can view multiple properties for multiple mailboxes. In the example above where booking requests could be scheduled only during working hours and have a maximum duration of 9 hours, run the following command to verify the new values.

  ```PowerShell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'RoomMailbox'" | Get-CalendarProcessing | Format-List Identity,ScheduleOnlyDuringWorkHours,MaximumDurationInMinutes
  ```

For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
