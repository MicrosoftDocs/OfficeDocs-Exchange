---
localization_priority: Normal
description: 'Summary: Learn about user workload management and throttling in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 276740c4-bdb7-49f1-9470-ae6f2bfd65aa
ms.reviewer:
title: User workload management in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# User workload management in Exchange Server

User workload management allows you to control how Exchange system resources are consumed by users. This feature was available in Exchange 2010 (known as *user throttling*), and was expanded to its current level in Exchange 2013.

A *workload* is a feature, protocol, or service that's been explicitly defined to manage system resources on Exchange servers. Each workload consumes system resources on the Exchange server (for example CPU, memory, network, and disk bandwidth). Examples of workloads include Outlook on the web (formerly known as Outlook Web App), Exchange ActiveSync, mailbox migration, and mailbox assistants.

## Control the user consumption of Exchange system resources

By default, the user workload settings allow users to increase their resource consumption for brief periods without experiencing a reduction in bandwidth. Because you can limit user access to resources, there are fewer instances of large resource consumers being locked out. You can further budget user resource consumption by setting a recharge rate for users. The important concepts for user workload management are describe in this list:

- **Burst allowances**: Allows users perform short periods of increased resource consumption without experiencing any throttling.

- **Recharge rate**: Uses a budget system to manage user resource consumption, and specifies the rate at which the user's budget is charged (how much the budget grows by) during the budget time. For example, if the budget time is one hour, a recharge rate value of 600,000 milliseconds indicates that resource budgets for users are recharged at the rate of ten minutes of usage per hour.

- **Traffic shaping (microdelays)**: Works by delaying the user for short periods of time when their resource usage reaches the configured limit over a specific time interval. This delay occurs for very short periods of time (users generally don't notice the delay), and well before the resource consumption causes a significant impact the Exchange server's performance. Traffic shaping preserves the availability of the Exchange server without blocking user productivity, has less user impact than a user lockout, and significantly reduces the chance of a user lockout.

- **Maximum usage**: Temporarily blocks a user who reaches a maximum user resource threshold (the user consumes an unusually high amount of resources over a short time interval). Users who are temporarily blocked from resource usage are unblocked as soon as their resource usage budget allows it (as their budgets are recharged).

You manage user workload settings with these cmdlets in the Exchange Management Shell:

- **View, create, remove, and modify user workload settings**: [Get-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/get-throttlingpolicy), [New-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/new-throttlingpolicy), [Remove-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-throttlingpolicy) and [Set-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/set-throttlingpolicy).

- **Assign user workload settings to users or computers**: [Get-ThrottlingPolicyAssociation](https://docs.microsoft.com/powershell/module/exchange/get-throttlingpolicyassociation) and [Set-ThrottlingPolicyAssociation](https://docs.microsoft.com/powershell/module/exchange/set-throttlingpolicyassociation)

## Scopes in user workload settings

By default, there's one throttling policy named `GlobalThrottlingPolicy`. This policy has the scope value Global, which means it applies to all users in the organization. Typically, the settings in the default throttling policy are adequate for users in most Exchange organizations. Instead of customizing the default throttling policy, you can create custom throttling policies that have different settings that the default policy. The scopes that are available in custom throttling policies are:

- **Organization**: The throttling settings apply to *all* users in the organization.

- **Regular**: The throttling settings that apply only to *specific* users in the organization.

The order of precedence for throttling polices are:

1. Throttling policies with the scope value Regular are applied to users before Organization policies and the default throttling policy.

2. Throttling policies with the scope value Organization are applied to users before the default throttling policy.

3. The default throttling policy is applied last, or exclusively to users who don't have Regular or Organization policies assigned to them.

If you create custom throttling policies, the settings should be different than the default throttling policy, and you should plan for the difference in settings from Regular policies to Organization policies to the default policy (for example, least restrictive to most restrictive, or vice-versa).

> [!NOTE]
> We strongly recommend that you don't modify the default throttling policy, because changes to the default policy could be overwritten by future Exchange updates. Instead, you should create custom throttling policies that contain customized settings.

## User throttling in Exchange 2010 coexistence environments

Users with mailboxes on Exchange 2016 servers are throttled using Exchange 2016 throttling features, even if you install Exchange 2016 in an Exchange 2010 organization. This list describes the important considerations for throttling in coexistence environments:

- Exchange 2010 mailboxes remain throttled by Exchange 2010 throttling features when users access their mailboxes through Exchange 2010 Client Access servers.

- When you install Exchange 2016 in an Exchange 2010 organization, Exchange 2016 setup might try to carry some of the Exchange 2010 throttling settings forward. However, the throttling functionality is so different that the effects of any legacy throttling settings will generally not alter how throttling works in Exchange 2016.
