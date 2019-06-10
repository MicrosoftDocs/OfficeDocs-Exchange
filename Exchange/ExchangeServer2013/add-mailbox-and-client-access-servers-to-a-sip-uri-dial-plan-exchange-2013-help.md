---
title: 'Add Mailbox and Client Access servers to a SIP URI dial plan: Exchange 2013 Help'
TOCTitle: Add Mailbox and Client Access servers to a SIP URI dial plan
ms:assetid: 17fed308-ff0d-4e61-b9f9-e6680b6eccaa
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Aa996399(v=EXCHG.150)
ms:contentKeyID: 51439477
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Add Mailbox and Client Access servers to a SIP URI dial plan in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can add Client Access and Mailbox servers to SIP URI dial plans. Client Access and Mailbox servers can't be associated with Telephone Extension or E.164 dial plans, but the servers will answer all incoming calls.

If you're deploying Microsoft Lync Server, to enable outbound calling to work correctly, you must manually add all Client Access and Mailbox servers to all SIP URI dial plans that you've created for Lync Server.

For additional management tasks related to UM dial plans, see [UM dial plan procedures](um-dial-plan-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a SIP URI dial plan has been created. For detailed steps, see [Create a UM dial plan in Exchange Server](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to add a Mailbox server to a SIP URI dial plan

1. In the EAC, navigate to **Servers** \> **Servers**.

2. In the list view, select the Mailbox server you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the **Exchange Server** page, click **Unified Messaging**.

4. Under **UM Service settings** \> **Associated dial plans**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

5. In the **Select a UM Dial Plan** window, select the SIP URI dial plan, click **Add**, click **OK**, and then click **Save**.

## Use the Shell to add a Mailbox server to a SIP URI dial plan

This example adds the Mailbox server named `MyMailboxServer` to a SIP URI dial plan named `MySIPDialPlan` and prevents it from accepting new calls. It also sets the startup mode to Dual mode, which enables the Mailbox server to accept TCP and TLS requests.

```powershell
Set-UMService -Identity MyMailboxServer -DialPlans MySIPDialPlan -Status Disabled -UMStartupMode Dual
```

This example adds the Mailbox server named `MyMailboxServer` to two SIP dial plans, named `MySIPDialPlan` and `MySIPDialPlan2`, and sets the following:

- Allows both IPv4 and IPv6 addresses.

- Sets the maximum number of incoming calls to 50.

- Configures the SIP access service for Lync Server.

```powershell
Set-UMService -Identity MyMailboxServer -DialPlans MySIPDialPlan, MySIPDialPlan2 -IPAddressFamily Any -MaxCallsAllowed 50 -SipAccessService northamerica.lyncpoolna.contoso.com
```

## Use the EAC to add a Client Access server to a SIP URI dial plan

1. In the EAC, navigate to **Servers** \> **Servers**.

2. In the list view, select the Client Access server you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the **Exchange Server** page, click **Unified Messaging**.

4. Under **UM Call Router settings** \> **Associated dial plans**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

5. In the **Select a UM Dial Plan** window, select the SIP URI dial plan, click **Add**, click **OK**, and then click **Save**.

## Use the Shell to add a Client Access server to a SIP URI dial plan

This example adds the Client Access server named `MyClientAccessServer` to a SIP URI dial plan named `MySIPDialPlan`. It also sets the startup mode to Dual mode, which enables the Client Access server to accept TCP and TLS requests.

```powershell
Set-UMCallRouterSettings -DialPlans MySIPDialPlan -Server MyClientAccessServer -UMStartupMode Dual
```

This example adds the Client Access server named `MyClientAccessServer` to two SIP dial plans, named `MySIPDialPlan` and `MySIPDialPlan2`, and allows the server to use both IPv4 and IPv6 addresses.

```powershell
Set-UMCallRouterSettings -DialPlans MySIPDialPlan, MySIPDialPlan2 -IPAddressFamily Any -Server MyClientAccessServer
```
