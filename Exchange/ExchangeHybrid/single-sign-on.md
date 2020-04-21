---
title: "Single sign-on with hybrid deployments"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: 050606f9-718d-4a1f-b7a6-50b08c6e9e07
ms.reviewer: 
description: "Single sign-on enables users to access both the on-premises and Office 365 organizations with a single user name and password. It provides users with a familiar sign-on experience and can allow administrators to easily control account policies for Exchange Online organization mailboxes by using on-premises Active Directory management tools. While you don't have to configure a hybrid deployment with single sign-on enabled, we strongly recommend that you do. Without single sign-on, users will need to remember two different sets of credentials, one for your on-premises organization, and one for Office 365. Here are a few other advantages to single sign-on:"
---

# Single sign-on with hybrid deployments

 Single sign-on enables users to access both the on-premises and Office 365 organizations with a single user name and password. It provides users with a familiar sign-on experience and can allow administrators to easily control account policies for Exchange Online organization mailboxes by using on-premises Active Directory management tools. While you don't have to configure a hybrid deployment with single sign-on enabled, we strongly recommend that you do. Without single sign-on, users will need to remember two different sets of credentials, one for your on-premises organization, and one for Office 365. Here are a few other advantages to single sign-on:

- **Exchange Online Archiving**: When single sign-on is deployed, on-premises Outlook users are prompted for their credentials when accessing archived content in the Exchange Online organization for the first time. However, users can then temporarily avoid future credential prompting by choosing "save password" and then will only be prompted for credentials again when their on-premises account password is changed. If single sign-on isn't deployed in Exchange organizations and Exchange Online Archiving is enabled, the on-premises user principal name (UPN) must match their Exchange Online account and users will always be prompted for their on-premises credentials when accessing their archive.

- **Policy control**: You can control account policies through Active Directory, which gives you the ability to manage password policies, workstation restrictions, lock-out controls, and more, without having to perform additional tasks in your Office 365 organization.

- **Reduced support calls**: Forgotten passwords are a common source of support calls in all companies. If users have fewer passwords to remember, they are less likely to forget them.

You have three options when deploying single sign-on: password hash synchronization, pass-through authentication and federation, for example, Active Directory Federation Services (AD FS). All options are implemented by Azure Active Directory Connect. We strongly recommend using the password hash synchronization method unless you have a specific need that requires federation. Password hash synchronization provides many of the same benefits of federation without the complexity of its deployment.

To learn more about each option, see [Choose the right authentication method for your Azure Active Directory hybrid identity solution](https://docs.microsoft.com/azure/active-directory/hybrid/choose-ad-authn).
