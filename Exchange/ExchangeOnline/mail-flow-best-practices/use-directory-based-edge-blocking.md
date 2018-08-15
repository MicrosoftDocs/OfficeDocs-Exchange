---
title: "Use Directory Based Edge Blocking to reject messages sent to invalid recipients"
ms.author: sirkkuw
author: Sirkkuw
manager: scotv
ms.date: 12/9/2016
ms.audience: ITPro
ms.topic: article
ms.service: exchange-online
localization_priority: Normal
ms.assetid: ca7b7416-92ed-40ad-abdb-695be46ea2e4
description: "The Directory Based Edge Blocking (DBEB) feature in Exchange Online and Exchange Online Protection (EOP) lets you reject messages for invalid recipients at the service network perimeter. DBEB lets admins add mail-enabled recipients to Office 365 and block all messages sent to email addresses that aren't present in Office 365."
---

# Use Directory Based Edge Blocking to reject messages sent to invalid recipients

The Directory Based Edge Blocking (DBEB) feature in Exchange Online and Exchange Online Protection (EOP) lets you reject messages for invalid recipients at the service network perimeter. DBEB lets admins add mail-enabled recipients to Office 365 and block all messages sent to email addresses that aren't present in Office 365.
  
If a message is sent to a valid email address present in Office 365, the message continues through the rest of the service filtering layers (anti-malware, anti-spam, transport rules). If the address is not present, the service blocks the message before filtering even occurs, and a non-delivery report (NDR) is sent to the sender informing them that their message was not delivered. The contents of the NDR will be similar to the following: '550 5.4.1 [\< *nosuchuser*  \>@\<  *recipient_domain*  \>]: Recipient address rejected: Access denied'. 

<Please add this as a note>: For a hybrid configuration, the recipient's SMTP domain must have the DNS MX record pointing to O365/EOP.
  
## Configure DBEB

The steps for configuring DBEB are as follows:
  
1. **Ensure that your accepted domain to Set to Internal relay**: 
    
1. In the EAC, go to **Mail flow** \> **Accepted domains**.
    
2. Select the domain and click **Edit**.
    
3. Ensure that the domain type is set to **Internal relay**. If it's set to **Authoritative**, change it to **Internal relay** and click **Save**. 
    
2. **Add valid users to Office 365**. You can do this in one of the following ways: 
    
  - **Directory synchronization.** Add valid users to Office 365 by synchronizing from your on-premises Active Directory environment to [Azure Active Directory](https://technet.microsoft.com/en-us/library/hh967611.aspx) in the cloud. For more information about how to set up directory synchronization, see "Use directory synchronization to manage recipients" in [Manage Mail Users in EOP](http://technet.microsoft.com/library/4bfaf2ab-e633-4227-8bde-effefb41a3db.aspx). 
    
  - **Add users via remote Windows PowerShell.** For more information about how add users in this manner, see "Use remote Windows PowerShell to manage mail users" in [Manage Mail Users in EOP](http://technet.microsoft.com/library/4bfaf2ab-e633-4227-8bde-effefb41a3db.aspx) or [Manage mail users](../recipients-in-exchange-online/manage-mail-users.md) (for Exchange Online customers). 
    
  - **Add users directly in the Exchange admin center (EAC).** For more information about how add users in this manner, see "Use the EAC to manage mail users" in [Manage Mail Users in EOP](http://technet.microsoft.com/library/4bfaf2ab-e633-4227-8bde-effefb41a3db.aspx) or [Manage mail users](../recipients-in-exchange-online/manage-mail-users.md) (for Exchange Online customers). 
    
3. **Set your accepted domain to Authoritative**: 
    
1. In the EAC, go to **Mail flow** \> **Accepted domains**.
    
2. Select the domain and click **Edit**.
    
3. Set the domain type to **Authoritative**.
    
    > [!NOTE]
    > Until all of your valid users have been added to Office 365 and replicated through the system you should leave the domain type configured as **Internal relay**. Once the domain type has been changed to **Authoritative**, DBEB is designed to allow any SMTP address that has been added to the service (except for mail-enabled public folders). There might be infrequent instances where recipient addresses that do not exist in your Office 365 organization are allowed to relay through the service. 
  
4. Click **Save** to save your changes, and confirm that you want to enable Directory Based Edge Blocking. 
    

