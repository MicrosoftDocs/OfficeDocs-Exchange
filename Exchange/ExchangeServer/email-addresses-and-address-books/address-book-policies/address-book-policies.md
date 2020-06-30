---
localization_priority: Normal
description: 'Summary: Learn how to use address book policies (ABPs) to create separate virtual organizations with a segmented global address list in Exchange Server.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d0a916a1-e3ed-49ae-b116-a559be0dcce6
ms.reviewer: 
title: Address book policies in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Address book policies in Exchange Server

Address book policies (ABPs) lets administrators segment users into specific groups to provide customized views of the organization's global address list (GAL). The goal of an ABP is to provide a simpler mechanism for GAL segmentation (also known as *GAL segregation*) in on-premises organizations that require multiple GALs.

An ABP contains these elements:

- One GAL. For more information about GALs, see [Global address lists](../address-lists/address-lists.md#global-address-lists).

- One offline address book (OAB). For more information about OABs, see [Offline address books in Exchange Server](../../email-addresses-and-address-books/offline-address-books/offline-address-books.md).

- One room list. Note that this room list is a custom address list that specifies rooms (contains the filter `RecipientDisplayType -eq 'ConferenceRoomMailbox'`). It's not a room finder that you create with the _RoomList_ switch on the **New-DistributionGroup** or **Set-DistributionGroup** cmdlet. For more information, see [Create and manage room mailboxes](../../recipients/room-mailboxes.md).

- One or more address lists. For more information about address lists, see [Custom address lists](../address-lists/address-lists.md#custom-address-lists).

For procedures involving ABPs, see [Procedures for address book policies in Exchange Server](abp-procedures.md).

 **Notes**:

- ABPs create only a virtual separation of users from a directory perspective, not a legal separation.

- Implementing an ABP is a multi-step process that requires planning. For more information, see [Scenario: Deploying address book policies in Exchange Server](abp-scenarios.md).

## How ABPs work

The following diagram shows how ABPs work. The user is assigned Address Book Policy A that contains a subset of address lists that are available in the organization. When the ABP is created and assigned to the user, the ABP becomes the scope of the address lists that the user is able to view.

![Overview of Address Book Policies](../../media/ITPro_Mailbox_ABPOverall.gif)

APBs take effect when a user connects to the Client Access (frontend) services on a Mailbox server. If you change an ABP, the updated APB takes effect when a user restarts or reconnects their client app, or you restart the Mailbox server (specifically, the Microsoft Exchange RPC Client Access service in the backend services).

### Address Book Policy Routing agent

In an Exchange organization that doesn't use ABPs, the following things occur when a user creates an email message in Outlook or Outlook on the web and sends the message to another recipient in the organization:

1. The email address resolves to the user's display name. For example, if you type sardor@contoso.com in the **To** field, the SMTP email address resolves to **Sarah Dorsey**.

2. After the name resolves, you can view the recipient's contact card by double-clicking on the user's name. The contact card shows the recipient's contact information, such as office and phone number.

If you're using ABPs, and you don't want the users in the ABPs to view each other's potentially private information, you can turn on the Address Book Policy Routing agent. The ABP Routing agent is a Transport agent that controls how recipients are resolved in your organization. When the ABP Routing agent is installed and configured, users that are assigned to different GALs by different ABPs can't view each other's contact cards (they appear as external recipients to each other).

For details about how to turn on the ABP Routing agent, see [Use the Exchange Management Shell to install and configure the Address Book Policy Routing Agent](abp-procedures.md#use-the-exchange-management-shell-to-install-and-configure-the-address-book-policy-routing-agent).

## ABP example

In the following diagram, Fabrikam and Tailspin Toys share the same Exchange organization and the same CEO. The CEO is the only employee common to both companies.

![Two Companies One CEO](../../media/ITPro_.png)

The suggested configuration includes three ABPs:

- One ABP is assigned to Fabrikam employees. The GAL and address lists in the ABP include Fabrikam employees and the CEO.

- One ABP is assigned to Tailspin Toys employees. The GAL and address lists in the ABP include Tailspin Toys employees and the CEO.

- One ABP is assigned to only the CEO. The (default) GAL and address lists in the ABP include all employees (Fabrikam, Tailspin Toys, and the CEO).

Based on this configuration, the ABPs help to enforce these requirements:

- The users in Tailspin Toys can only see Tailspin Toys employees and the CEO when they browse the GAL.

- The users in Fabrikam can only see Fabrikam employees and the CEO when they browse the GAL.

- The CEO can see all Fabrikam and Tailspin Toys employees when she browses the GAL.

- Users who view the CEO's group membership can see only groups that belong to their company. They can't see groups that belong to the other company.

## ABPs for Entourage and Outlook for Mac users

ABPs won't function for Entourage and Outlook for Mac users who connect to their mailboxes from inside the corporate network, because Entourage and Outlook for Mac connect directly to a global catalog server to query Active Directory (which bypasses the ABPs). However, Entourage and Outlook for Mac clients that connect to their mailboxes from outside the corporate networks can use an OAB or Exchange Web Services (EWS), which allows them to search the GAL based on the assigned ABP. To learn more about administering Outlook for Mac 2011, see [Planning for Outlook for Mac 2011](https://docs.microsoft.com/previous-versions/office/office-for-mac-2011/jj984221(v=office.14)).
