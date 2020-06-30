---
localization_priority: Normal
description: 'Summary: How to create a resource mailbox called a room mailbox, a room list, and how to change room mailbox properties.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f70752ad-fce0-4e14-8428-fc5ac63f6c54
ms.reviewer:
title: Create and manage room mailboxes
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create and manage room mailboxes

A room mailbox is a resource mailbox that's assigned to a physical location, such as a conference room, an auditorium, or a training room. With room mailboxes, users can easily reserve these rooms by including room mailboxes in their meeting requests. When they do this, the room mailbox uses options you can configure to decide whether the invite should be accepted or denied.

To create a room mailbox, you need to be an administrator who's a member of either the Organization Management or Recipient Management role groups.

If you want to grant someone access to a room mailbox so they can directly manage its calendar (for example, an assistant who needs to make room for an executive meeting), you can do so using the instructions in [Manage permissions for recipients](mailbox-permissions.md). After a user's been granted permissions to access a room mailbox, they can open the mailbox using the instructions in [Open and use a shared mailbox in Outlook for Windows](https://support.microsoft.com/office/d94a8e9e-21f1-4240-808b-de9c9c088afd).

**IMPORTANT** Room mailboxes should never be set as the organizer of a meeting, nor should room mailboxes be accessed directly by users in order to make changes to a meeting. Rooms should only be added to meetings in the Attendee or Location fields. Otherwise you will override the Resource Booking Assistant (RBA), which manages and processes all calendar items sent to the room mailbox, and unexpected errors may occur. If your organization has one or more users who need to manage a room and its mailbox, then assign users to the room as resource delegates for the room mailbox, as described later in this article. When a delegate is assigned, all items sent to the room's mailbox will be directed to the booking delegate, who can then accept or decline from their own Inbox. If your organization wants to use a room mailbox like a team calendar, consider using Exchange's shared calendar features.

If you want to learn about the types of recipients that are available in Exchange Server, check out [Recipients](recipients.md). For info about another type of resource mailbox, check out [Manage equipment mailboxes](equipment-mailboxes.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Server](../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Recipient Provisioning Permissions" section in the [Recipients Permissions](../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

- If you're using room or equipment mailboxes in Microsoft 365 or Office 365, see [Room and equipment mailboxes(https://docs.microsoft.com/microsoft-365/admin/manage/room-and-equipment-mailboxes) for more information.

> [!IMPORTANT]
> If you're running Exchange 2013 in a hybrid scenario, make sure you create the room mailboxes in the appropriate place. Create your room mailboxes for your on-premises organization on-premises, and room mailboxes for Exchange Online should be created in the cloud. > Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create a room mailbox

1. In the Exchange admin center, navigate to **Recipients** \> **Resources**.

2. To create a room mailbox, click **New** ![Add icon](../media/ITPro_EAC_AddIcon.png) \> **Room mailbox**.

3. Use the options on the page to specify the settings for the new resource mailbox.

   - **Room name**: Use this box to type a name for the room mailbox. This is the name that's listed in the resource mailbox list in the Exchange admin center and in your organization's address book. This name is required and it can't exceed 64 characters.

    > [!TIP]
    > Although there are other fields that describe the details of the room (for example, Location and Capacity) consider summarizing the most important details in the room name using a consistent naming convention. Why? So users can easily see the details when they select the room from the address book in the meeting request.

   - **Alias**: A room mailbox has an email address so it can receive booking requests. The email address consists of an alias on the left side of the @ symbol, which must be unique in the forest, and your domain name on the right. The alias is required.

   - **Location**, **Phone**, **Capacity**: You can use these fields to enter details about the room. However, as explained earlier, you can include some or all of this information in the room name so users can see it.

4. When you're finished, click **Save** to create the room mailbox.

After you've created a room mailbox, you can [Change how a room mailbox handles meeting requests](#change-how-a-room-mailbox-handles-meeting-requests) (including whether it responds automatically or someone needs to decide what to do). By default, it'll automatically accept or decline requests depending on whether the requests conflict with any existing meetings on its calendar. It'll also allow meetings that repeat, and allow meetings up to 180 days from the current date (and decline any requests beyond that) that are up to 24 hours in duration. If you want to change other options, head down to [Change other room mailbox properties](#change-other-room-mailbox-properties).

For information on how to create a room mailbox using the Exchange Management shell, see Examples 2 and 3 in [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox).

## Change how a room mailbox handles meeting requests

1. In the Exchange admin center, navigate to **Recipients** \> **Resources**.

2. In the list of resource mailboxes, click the room mailbox that you want to change the properties for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. On the room mailbox properties page, click **Booking Delegates** (allow automatic responses or not) or **Booking Options** (allow repeating meetings, decline meetings that are scheduled too far out, etc).

Use the **Booking Delegates** section to view or change how the room mailbox handles meeting requests and to define who can accept or decline booking requests if it isn't done automatically.

- Select one of the following options to handle meeting requests.

  - **Accept or decline booking requests automatically**: If this is selected, the meeting request will automatically be declined if there's a scheduling conflict with an existing reservation, or if the booking request violates the scheduling limits of the resource, for example, the reservation duration is too long.

  - **Select delegates who can accept or decline booking requests**: If this is selected, one of the people you added to the **Delegates** list below will be responsible for accepting or declining meeting requests that are sent to the room mailbox. If you assign more than one resource delegate, only one of them has to act on a specific meeting request.

Use the **Booking Options** section to view or change the settings for the booking policy that defines when the room can be scheduled, how long it can be reserved, and how far in advance it can be reserved.

- **Allow repeating meetings**: This setting allows or prevents repeating meetings for the room.

- **Allow scheduling only during working hours**: This setting accepts or declines meeting requests that aren't during the working hours defined for the room, which are, by default, 8:00 A.M. to 5:00 P.M. Monday through Friday. You can configure the working hours of the room mailbox either by logging into the mailbox using Outlook on the web and going to the **Options** \> **Calendar** \> **Calendar appearance** page, or by using [Set-MailboxCalendarConfiguration](https://docs.microsoft.com/powershell/module/exchange/set-mailboxcalendarconfiguration).

- **Always decline if the end date is beyond this limit**: This setting controls the behavior of repeating meetings that extend beyond the date specified by the maximum booking lead time setting.

  - If you enable this setting, a repeating booking request is automatically declined if the bookings start on or before the date specified by the value in the **Maximum booking lead time** box, and they extend beyond the specified date. This is the default setting.

  - If you disable this setting, a repeating booking request is automatically accepted if booking requests start on or before the date specified by the value in the **Maximum booking lead time** box, and they extend beyond the specified date. However, the number of bookings is reduced so bookings won't occur after the specified date.

- **Maximum booking lead time (days)**: This setting specifies the maximum number of days in advance that the room can be booked. Valid input is an integer between 0 and 1080. The default value is 180 days.

- **Maximum duration (hours)**: This setting specifies the maximum duration that the room can be reserved in a booking request. The default value is 24 hours.

    For repeating booking requests, the maximum booking duration applies to the length of each instance of the repeating booking request.

There's also a box on this page that you can use to write a message that will be sent to users who send booking requests to reserve the room.

## Change resource scheduling settings

An Admin or user with full access to the resource mailbox can make changes to the resource scheduling settings.

1. Log in to [Outlook Web App](https://outlook.office365.com) and click on **Your name** in the top right corner.

2. Click **Open another mailbox**. Locate the meeting room resource you want and click **Open**.

3. Go to **settings** and click **Calendar**.

4. Navigate to **Resource scheduling**.

5. Configure the Scheduling Options and Scheduling Permissions as needed. (See the descriptions of all options in the following two sections for details.)

6. Click **Save** after you have finished making your changes.

## Scheduling Options

- **Automatically process meeting requests and cancellations**

Enables or disables all options below as well as the options under Scheduling Permissions. If not checked, the owner must manage every request manually. By default, this is not checked.

- **Disable reminders**

Enables or disables reminders for events in this calendar. This setting applies only to the resource; the organizer and attendees will still receive reminders if they have elected to do so.

- **Maximum number of days in advance resources can be booked**

Limits how far in advance an event can be scheduled. The default is 180 days.

- **Always decline if the end date is beyond this limit**

Requests beyond the maximum number of days specified will be automatically declined. Valid values are between 0 (today) and 1080 (about three years in the future).

- **Limit meeting duration and Maximum allowed minutes**

Limits the amount of time for which a room can be scheduled within a single day. Unchecking the box will mean a meeting has no limit. Checking the box allows for a limit between 0 to 1440 minutes.

- **Allow scheduling only during working hours**

If checked, an event can only be scheduled during the hours specified under Calendar Work Week in the Calendar tab. Events outside of working hours will be automatically declined.

- **Allow repeating meetings**

Allows booking of the resource room at a regular interval. The event can be set to repeat over a specified duration of time (also called recurring).

- **Allow conflicts**

Allow or prevent conflicting meeting requests (double booking). If repeating meetings are allowed as well, this setting will only apply to repeating meetings. When the resource is invited, it will need to be entered into the Attendees field as opposed to being chosen with the Add Rooms button.

- **Allow up to this number of individual conflicts**

This setting specifies the maximum number of conflicts that are allowed for new repeating meeting requests. When set to 0, a recurring event will fail to schedule if one or more conflicting appointments already appear. When set to a number greater than 0, a recurring event is allowed the specified number of conflicts before being denied.

- **Allow up to this percentage of individual conflicts**

This setting specifies the maximum percentage of meeting conflicts that are allowed for new repeating meeting requests. This is similar to specifying a number of individual conflicts (explained above), but in this case, a recurring event is allowed the specified percentage of conflicts before being denied.

## Scheduling Permissions

- **These users can schedule automatically if the resource is available**

By default, everyone can schedule this resource without the manual approval of the resource owner. If **Select users and groups** is selected, only the users and groups specified can schedule automatically. All other users or groups will receive a decline message. If **Select users and groups** is selected but no users or groups are specified, this option will be ignored.

- **These users can submit a request for owner approval if the resource is available**

If everyone is selected, then all requests must receive manual approval by the resource owner. If **Select users and groups** is selected, only the specified users and groups require manual approval by the resource owner. **Select users and groups** is selected and left blank by default so that all requests are approved automatically.

- **These users can schedule automatically if the resource is available and can submit a request for owner approval if the resource is unavailable**

When everyone is selected (the default setting), any request during an open time frame will be automatically approved. If the room is booked at the requested time, a form is submitted to the resource owner for manual approval. If **Select users and groups** is selected, only those specified will have the option to have the request manually approved; all others will have a conflicting request denied without the option of manual approval by the resource owner.

## Change other room mailbox properties

After you create a room mailbox, you can make changes and set additional properties by using the Exchange admin center or the Exchange Management Shell.

### Use the Exchange admin center to change room mailbox properties

1. In the Exchange admin center, navigate to **Recipients** \> **Resources**.

2. In the list of resource mailboxes, click the room mailbox that you want to change the properties for, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. On the room mailbox properties page, click one of the following sections to view or change properties (for booking options, see [Change how a room mailbox handles meeting requests](#change-how-a-room-mailbox-handles-meeting-requests).

   - [General](#general)

   - [Contact Information](#contact-information)

   - [Email Address](#email-address)

   - [MailTip](#mailtip)

#### General

Use the **General** section to view or change basic information about the resource.

- **Room name**: This name appears in the resource mailbox list in the Exchange admin center and in your organization's address book. It can't exceed 64 characters if you change it.

- **Email address**: This read-only box displays the email address for the room mailbox. You can change it in the [Email Address](#email-address) section.

- **Capacity**: Use this box to enter the maximum number of people who can safely occupy the room.

Click **More options** to view or change these additional properties:

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the account for the room mailbox. You have to use Active Directory Users and Computers to move the account to a different OU.

- **Mailbox database**: This read-only box displays the name of the mailbox database that hosts the room mailbox. Use the **Migration** page in the Exchange admin center to move the mailbox to a different database.

- **Alias** Use this box to change the alias for the room mailbox.

- **Hide from address lists**: Select this check box to prevent the room mailbox from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send booking messages to the room mailbox by using the email address.

- **Department**: Use this box to specify a department name that the room is associated with. You can use this property to create recipient conditions for dynamic distribution groups and address lists.

- **Company**: Use this box to specify a company that the room is associated with, if applicable. Like the Department property, you can use this property to create recipient conditions for dynamic distribution groups and address lists.

- **Address book policy**: Use this option to specify an address book policy (ABP) for the room mailbox. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. To learn more, see [Address book policies in Exchange Server](../email-addresses-and-address-books/address-book-policies/address-book-policies.md).

   In the drop-down list, select the policy that you want associated with this mailbox.

- **Custom attributes**: This section displays the custom attributes defined for the room mailbox. To specify custom attribute values, click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png). You can specify up to 15 custom attributes for the recipient.

#### Contact Information

Use the **Contact Information** section to view or change the contact information for the room. The information on this page is displayed in the address book.

> [!TIP]
> You can use the **State/Province** box to create recipient conditions for dynamic distribution groups, email address policies, or address lists.

#### Email Address

Use the **Email Address** section to view or change the email addresses associated with the room mailbox. This includes the mailbox's primary SMTP address and any associated proxy addresses. The primary SMTP address (also known as the *reply address*) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**: Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this button and then type the new SMTP address in the **Email address** box.

  - **EUM**: An EUM (Exchange Unified Messaging) address is used by the Microsoft Exchange Unified Messaging service in Exchange 2016 to locate UM-enabled recipients within an Exchange organization. EUM addresses consist of the extension number and the UM dial plan for the UM-enabled user. Click this button and type the extension number in the **Address/Extension** box. Then click **Browse** and select a dial plan for the mailbox. (**Note**: Unified Messaging is not available in Exchange 2019.)

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **Email address** box.

    **Notes**:

    - With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

    - When you add a new email address, you have the option to make it the primary SMTP address.

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization.

#### MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues before they send a booking request to the room mailbox. A MailTip is text that's displayed in the InfoBar when this recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
>MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Use the Exchange Management Shell to change room mailbox properties

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

Here are some examples of using the Exchange Management Shell to change room mailbox properties.

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

## Create a room list

If you're planning to have more to have hundreds of rooms, use multiple room lists to help you organize your rooms. If your company has several buildings with rooms that can be booked for meetings, it might help to create room lists for each building. Room lists are specially marked distribution groups that you can use the same way you use distribution groups. However, you can only create room lists using the Exchange Management Shell.

> [!NOTE]
> Although there is no hard limit to the number of rooms you can have in a Room List, the maximum number of rooms that can be returned in request for a Room List is 100. A possible workaround would be to further break down your rooms into smaller lists.

### Use the Exchange Management Shell to create a room list

This example creates a room list for building 32.

```PowerShell
New-DistributionGroup -Name "Building 32 Conference Rooms" -OrganizationalUnit "contoso.com/rooms" -RoomList
```

### Use the Exchange Management Shell to add a room to a room list

This example adds confroom3223 to the building 32 room list.

```PowerShell
Add-DistributionGroupMember -Identity "Building 32 Conference Rooms" -Member confroom3223@contoso.com
```

### Use the Exchange Management Shell to convert a distribution group to a room list

You may already have created distribution groups in the past that contain your conference rooms. You don't need to recreate them; we can convert them quickly into a room list.

This example converts the distribution group, building 34 conference rooms, to a room list.

```PowerShell
Set-DistributionGroup -Identity "Building 34 Conference Rooms" -RoomList
```

## How do you know this worked?

To verify that you've successfully changed properties for a room mailbox, do the following:

- In the Exchange admin center, select the mailbox and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected mailbox.

- In the Exchange Management Shell, use the **Get-Mailbox** cmdlet to verify the changes. One advantage of using the Exchange Management Shell is that you can view multiple properties for multiple mailboxes. In the example above where booking requests could be scheduled only during working hours and have a maximum duration of 9 hours, run the following command to verify the new values.

  ```PowerShell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'RoomMailbox'" | Get-CalendarProcessing | Format-List Identity,ScheduleOnlyDuringWorkHours,MaximumDurationInMinutes
  ```
