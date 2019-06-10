---
title: 'Configure Exchange to accept mail for multiple authoritative domains'
TOCTitle: Configure Exchange to accept mail for multiple authoritative domains
ms:assetid: 11801f73-4934-4025-a1c1-3935dada7e9b
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Aa996314(v=EXCHG.150)
ms:contentKeyID: 50874000
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure Exchange to accept mail for multiple authoritative domains

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, it's easy to add multiple authoritative domains to your organization. However, after you add the authoritative domain, you need to decide how to use the authoritative domain in your organization. For example:

- If you want to add an email address in the new authoritative domain for recipients in your organization, do you want to replace the existing primary (reply to) address for the recipients, or add the new email address as a proxy (secondary) address?

- Do you want the email address in the new authoritative domain to apply to all recipients, and all recipient types? Or do you want the new email address to apply to specific types of recipients, or specific recipients based on their user properties, for example, only users in the Finance department?

The following examples are scenarios in which your Exchange organization may have to receive and process email for more than one authoritative SMTP domain:

- You are changing your SMTP domain name, but have to continue to accept email for the old domain name for a time, in case customers send email messages to the previous email addresses. You can set the new email address as the primary (reply to) address. This means that the new address will be the default email address displayed on all email messages sent by the recipient. You can set the old email address as a secondary address. This will enable the recipient to continue to receive email sent to the old email address.

- You want to provision different email addresses for business units within your organization. For example, if the contoso.com Active Directory forest contains subdomains for the subsidiaries Tailspin Toys and Fourth Coffee, you may want to assign the SMTP domain names contoso.com, tailspintoys.com, and fourthcoffee.com to the recipients in those respective business units.

- You provide email hosting services and have to accept email for more than one SMTP domain name.

## What do you need to know before you begin?

- Estimated time to complete this task: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Accepted domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic and the "Email address policies" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- If you have a subscribed Edge Transport server in your perimeter network, you configure accepted domains on a Mailbox server in your Exchange organization. The accepted domains configuration is replicated to the Edge Transport server during EdgeSync synchronization. For more information, see [Edge Subscriptions](edge-subscriptions-exchange-2013-help.md).

- When you create an accepted domain, you can use a wildcard character (\*) in the address space to indicate that all subdomains of the SMTP address space are also accepted by the Exchange organization. For example, to configure contoso.com and all its subdomains as accepted domains, enter **\*.contoso.com** as the SMTP address space. However, if the subdomain names will be used in an email address policy, each subdomain must have an explicit accepted domain entry.

- An MX record in public DNS is required for each SMTP domain for which you accept email from the Internet. Each MX record should resolve to the Internet-facing server that receives email for your organization.

- You need to configure Send connectors and Receive connectors so your Exchange organization can send email to and receive email from the Internet. The configuration of the Internet Send connectors and Receive connectors is determined by your Exchange topology. For more information about configuring connectors, see [Connectors](connectors-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## How do you do this?

## Step 1: Create an authoritative domain

## Use the Exchange Administration Center to create an authoritative domain

1. In the EAC, navigate to **Mail flow** \> **Accepted domains**, and click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **Name** field, enter the display name for the accepted domain. Each accepted domain for your organization must have a unique display name. This may be different than the accepted domain. For example, the domain contoso.com could have a display name of Contoso Local Accepted Domain.

3. In the **Accepted domain** field, specify an SMTP namespace for which your organization accepts email messages. For example, contoso.com.

4. Select **Authoritative domain**.

5. Click **Save**.

## Use the Shell to create an authoritative domain

To create a new authoritative domain, use the following syntax.

```powershell
New-AcceptedDomain -Name "<Unique Name>" -DomainName <SMTP domain> -DomainType Authoritative
```

For example, to create a new authoritative domain named "Fourth Coffee subsidiary" for the domain fourthcoffee.com, run the following command:

```powershell
New-AcceptedDomain -Name "Fourth Coffee subsidiary" -DomainName fourthcoffee.com -DomainType Authoritative
```

## How do you know this step worked?

To verify that you have successfully created an authoritative domain, do one of the following:

- In the EAC, navigate to **Mail flow** \> **Accepted domains**. Verify the accepted domain you created is displayed, and the **Domain Type** value is **Authoritative**.

- In the Shell, run the command **Get-AcceptedDomain**. Verify the domain you created is displayed, and the **DomainType** value is `Authoritative`.

## Step 2: Configure an email address policy for the authoritative domain

To use the authoritative accepted domain you created, you need to configure an email address policy for the authoritative domain that meets the objectives of your scenario. For example, you may need to create a new email address policy, or modify an existing policy. You may elect to replace the primary email address for some or all of your recipients, and you can elect to keep or remove the old primary email address. Two example scenarios are presented in this section.

## Change the existing primary email address

To change the primary (reply to) email address assigned to recipients and keep the old primary email address as a proxy (secondary) email address, follow these steps:

## Use the EAC to change the existing primary email address

1. In the EAC, navigate to **Mail flow** \> **Email address policies**. Select the email address policy you want to modify, and click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **Email Address Policy** page, click the **Email address format** tab. In the **Email address format** section, click **Add**![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. On the **Email Address Format** page that appears, make the following selections:

   - **Select an accepted domain**: Click the drop-down list, and select the new authoritative domain.

   - Select **Make this format the reply email address**.

    When you are finished, click **Save**.

4. On the **Email Address Policy** page, click **Save** to save your changes to the policy.

5. You'll get a warning that the email address policy won't be applied until you update it. After it's created, select it, and then, in the details pane, click **Apply**.

## Use the Shell to change the existing primary email address

In the Shell, you use two separate commands: one command to modify the existing email address policy, and another command to apply the updated email address policy to the recipients in your organization.

To change the existing primary email address, and keep the old primary email address as a proxy address, run the following command:

```powershell
Set-EmailAddressPolicy <EmailAddressPolicyIdentity> -EnabledEmailAddressTemplates SMTP:<NewPrimaryEmailAddress>,smtp:<OldPrimaryEmailAddress>
```

For example, suppose the email address policy in your organization uses the email addresses format *useralias*`@contoso.com`. This example changes the domain of primary (reply to) address in the email address policy named "Default Policy" to `@fourthcoffee.com`, and keeps the old primary reply address in the `@contoso.com` domain as a proxy (secondary) address.

```powershell
Set-EmailAddressPolicy "Default Policy" -EnabledEmailAddressTemplates SMTP:@fourthcoffee.com,smtp:@contoso.com
```

> [!NOTE]
> The <CODE>SMTP</CODE> qualifier in uppercase letters specifies the primary (reply to) address. The <CODE>smtp</CODE> qualifier in lowercase letters specifies a proxy (secondary) address.

To apply the updated email address policy to recipients, use the following syntax.

```powershell
Update-EmailAddressPolicy <EamilAddressPolicyIdentity>
```

For example, to apply the updated email address policy named "Default Policy", run the following command:

```powershell
Update-EmailAddressPolicy "Default Policy"
```

## Replace the existing primary email address for a filtered set of recipients

You can't modify the default email address policy to apply to a filtered set of recipients. You need to create a new email address policy, or modify an existing custom email address policy. The examples in this section create a new email address policy. In these examples, the primary (reply to) address in the new accepted domain replaces the old primary address for the specified recipients without keeping the old primary address as a proxy (secondary) email address. Therefore, the affected recipients can no longer receive email at their old primary email address.

Also, email address policies that apply to specific users should have a higher priority (indicated by a lower integer value) than other email address policies, including the default policy, so the specific policy is applied first. Because two policies can't have the same priority value, you may first need lower the priority of your organization's default email address policy.

## Use the EAC to replace the existing primary email address for a filtered set of recipients

To create additional email addresses that will be used as the primary email address for a filtered set of recipients, follow these steps.

1. In the EAC, navigate to **Mail flow** \> **Email address policies**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. On the **Email Address Policy** page, complete the following fields:

    1. **Policy name**: Enter a unique, descriptive name.

    2. **Email address format**: Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). On the **Email Address Format** page that appears, make the following selections:

       - **Select an accepted domain**: Click the drop-down list, and select the new authoritative domain.

       - **Email address format**: Select the appropriate email address format for your organization.

       - Select **Make this format the reply email address**.

        When you are finished, click **Save**.

3. **Run this policy in this sequence with other policies**: Typically, policies that apply to specific users should have a higher priority (indicated by a lower integer value) than other email address policies, including the default policy.

4. **Specify the types of recipients this email address will apply to**: Select the recipient types to which you want the email address policy applied.

5. **Create rules to further define the recipients that this email address policy applies to**: Click **Add a rule** to restrict the recipients that this policy will apply to. This creates a Boolean **And** statement. Repeat this step as many times as necessary.

   > [!WARNING]
   > If you apply too many rules, it's possible to restrict the email address policy to the point that it doesn't contain any users.

6. Click **Preview recipients the policy applies to** to view the recipients that policy will apply to.

7. Click **Save** to save your changes and create the policy.

8. You'll get a warning that the email address policy won't be applied until you update it. After it's created, select it, and then, in the details pane, click **Apply**.

## Use the Shell to replace the existing primary email address for a filtered set of recipients

To replace the primary email address for a filtered set of recipients, use the following command:

```powershell
New-EmailAddressPolicy -Name <Policy Name> -Priority <Integer> -IncludedRecipients <RecipientTypes> <Conditional Recipient Properties> -EnabledEmailAddressTemplates SMTP:@<NewPrimaryEmailAddress>
```

This example creates an email address policy named "Fourth Coffee Recipients", assigns that policy to mailbox users in the Fourth Coffee department, and sets the highest priority for that email address policy so the policy is applied first. Note that the old primary email address isn't preserved for these recipients, so they can't receive email at their old primary email address.

```powershell
New-EmailAddressPolicy -Name "Fourth Coffee Recipients" -Priority 1 -IncludedRecipients MailboxUsers -ConditionalDepartment "Fourth Coffee" -EnabledEmailAddressTemplates SMTP:@fourthcoffee.com
```

To apply the new email address policy to the affected recipients, run the following command:.

```powershell
Update-EmailAddressPolicy "Fourth Coffee Recipients"
```

## How do you know this step worked?

To verify that you have successfully configured an email address policy for the authoritative domain, do one of the following:

- In the EAC, navigate to **Mail flow** \> **Email address policies**. Verify the policies are applied in the correct order. Also, select any new or updated policies, and in the details pane, verify the email address format, included recipients, and if the policy has been applied,

- In the Shell, run the commands **Get-EmailAddressPolicy** and `Get-EmailAddressPolicy "<Policy Name>"| Format-List` to verify the details of the policies.

## How do you know this task worked?

To verify that you have configured Exchange to accept mail for multiple authoritative domains, do the following:

1. Send test messages to an affected recipient from a mailbox outside your Exchange organization. Verify the email addresses that successfully accept mail.

2. Send test messages from an affected recipient mailbox to an external recipient, and verify the From address of the message.
