---
title: 'Exchange workload management: Exchange 2013 Help'
TOCTitle: Exchange workload management
ms:assetid: 276740c4-bdb7-49f1-9470-ae6f2bfd65aa
ms:mtpsurl: https://technet.microsoft.com/library/JJ150503(v=EXCHG.150)
ms:contentKeyID: 47559972
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange workload management

_**Applies to:** Exchange Server 2013_

An Exchange workload is an Exchange Server feature, protocol, or service that's been explicitly defined for the purposes of Exchange system resource management. Each Exchange workload consumes system resources such as CPU, mailbox database operations, or Active Directory requests to run user requests or background work. Examples of Exchange workloads include Outlook Web App, Exchange ActiveSync, mailbox migration, and mailbox assistants.

You manage Exchange workloads by controlling how resources are consumed by individual users (sometimes called user throttling in Exchange 2010). Controlling how Exchange system resources are consumed by individual users was possible in Exchange Server 2010, and this capability has been expanded for Exchange Server 2013.

> [!NOTE]
> Managing workloads by monitoring the health of system resources on the Exchange servers in your organization should only be done under the direction of Microsoft Customer Service and Support.

## Managing workloads by controlling how resources are consumed by individual users

Throttling functionality is enhanced in Exchange 2013. The enhanced functionality helps make sure that excessive resource consumption by individual users doesn't adversely affect server performance or the user experience.

By default, user throttling in Exchange 2013 allows users to increase resource consumption for short periods without experiencing any reduction in bandwidth. Also, the complete lockout of users who use a very large amount of resources will be infrequent, or never occur. Rather than completely blocking a user from performing operations, throttling occurs and processes are delayed for short periods of time (think of them as "microdelays"), occurring before they cause a significant impact on a server.

**Feature Highlights**

Here are some highlights of the way Exchange controls how resources are consumed by individual users in Exchange 2013:

  - **Burst allowances**: Burst allowances let your users perform short periods of increased resource consumption without experiencing any throttling.

  - **Recharge rate**: Recharge rate manages your users' resource consumption by using a resource budget system. You can set the rate at which your users' resource budgets are recharged. For example, a value of 600,000 (in milliseconds) implies that users' budgets get recharged at a rate of ten minutes of usage per hour.

  - **Traffic shaping**: When a user's resource usage reaches the configured limit over a period of time, that user is delayed for very short periods of time well in advance of causing a significant impact on a server. Users generally don't notice these "microdelays." This process enables Exchange 2013 to efficiently shape traffic without blocking users from being productive. Traffic shaping has less impact on your users than early lockout, and it significantly reduces the chance that a lockout will occur.

  - **Maximum usage**: In rare circumstances, a user may consume a very high amount of resources over a short period of time. As a precaution, a user who reaches a maximum usage threshold may be temporarily blocked from using resources. Users who are temporarily blocked from resource usage are unblocked as soon as their usage budgets are recharged.

For a list of cmdlets you can use to control how resources are consumed by individual users, see "Cmdlets to control how resources are used by individual users" later in this topic.

**Exchange 2013 throttling functionality and deployment considerations**

Whether you perform a clean installation of Exchange 2013 or install Exchange 2013 into a coexistence environment that includes Exchange 2010 computers, all users with mailboxes on computers running Exchange 2013 are throttled using the new Exchange 2013 throttling functionality. However, Exchange 2010 mailboxes will remain throttled by Exchange 2010 throttling functionality when they access their mailboxes through Exchange 2010 Client Access servers.

When Exchange 2013 is installed into a coexistence environment, the Exchange 2013 installation process may try to carry forward some of the throttling settings that you had set in your Exchange 2010 configuration. However, the Exchange 2013 throttling functionality is so different that the impact of any legacy throttling settings will generally not impact how throttling works in Exchange 2013.

**Managing throttling policies by using scopes**

Similar to Exchange 2010, there's a single default throttling policy in Exchange 2013. In Exchange 2013, the default throttling policy is named **GlobalThrottlingPolicy**. This policy has the **Global** scope. The other available user throttling scopes are **Organization** and **Regular**. Due to the introduction of scope assignment for Exchange 2013 user throttling policies, you manage throttling policies differently than in Exchange 2010. The **GlobalThrottlingPolicy** defines the baseline default throttling settings for every new and existing user in your organization unless you have customized throttling policies for your organization. In many typical Exchange deployment scenarios, the **GlobalThrottlingPolicy** will be adequate to manage your users.

We strongly recommend that you don't customize throttling settings by modifying the **GlobalThrottlingPolicy**. Instead, you should create additional throttling policies. Creating additional throttling policies will help you better manage your workloads. It will also prevent any modifications to throttling policy settings from being overwritten by future Exchange 2013 updates.

To customize throttling settings that apply to all users in your organization, create a new throttling policy with the scope assignment **Organization**. In new Organization-scope policies, you should only set the throttling settings that are different from those in the **GlobalThrottlingPolicy**. To customize throttling settings that apply only to specific users in your organization, create a new throttling policy with the scope assignment **Regular**. In new Regular-scope policies, you should only set the throttling settings that are different from those in the **GlobalThrottlingPolicy** and any other organization policies. This will help you to inherit the rest of the policy settings from the **GlobalThrottlingPolicy** and let you benefit from any updates to throttling policies that are added in future Exchange updates.

## Managing workload throttling settings

You manage Exchange workload throttling settings by using the Exchange Management Shell.

**Cmdlets to control how resources are used by individual users**

You manage throttling settings with the following cmdlets, which were introduced in Exchange 2010:

Manage throttling policies

  - [Get-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-ThrottlingPolicy)

  - [New-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/New-ThrottlingPolicy)

  - [Remove-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/Remove-ThrottlingPolicy)

  - [Set-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-ThrottlingPolicy)

Assign throttling policies

  - [Get-ThrottlingPolicyAssociation](https://docs.microsoft.com/powershell/module/exchange/Get-ThrottlingPolicyAssociation)

  - [Set-ThrottlingPolicyAssociation](https://docs.microsoft.com/powershell/module/exchange/Set-ThrottlingPolicyAssociation)

> [!NOTE]
> The <STRONG>&#42;-ResourcePolicy</STRONG>, <STRONG>&#42;-WorkloadManagementPolicy</STRONG> and <STRONG>&#42;-WorkloadPolicy</STRONG> system workload management cmdlets have been deprecated. System workload management settings should be customized only under the direction of Microsoft Customer Service and Support.
