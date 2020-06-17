---
title: 'Configure circular logging for a mailbox database: Exchange 2013 Help'
TOCTitle: Configure circular logging for a mailbox database
ms:assetid: 29cbd7cd-382b-4e0d-8368-2e49e75df2fc
ms:mtpsurl: https://technet.microsoft.com/library/Dn756374(v=EXCHG.150)
ms:contentKeyID: 62524835
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure circular logging for a mailbox database

_**Applies to:** Exchange Server 2013_

When you enable circular logging for a mailbox database, the type of circular logging you get depends on whether or not the mailbox database is replicated using continuous replication:

- If the mailbox database is not replicated, it will use JET circular logging. In this case, enabling or disabling JET circular logging will require a dismount and mount of the database.

- If the mailbox database is replicated, it will use continuous replication circular logging (CRCL). In this case, enabling or disabling CRCL takes effect dynamically; there is no need to dismount and re-mount the database.

For more information about circular logging and CRCL, see [Exchange Native Data Protection](backup-restore-and-disaster-recovery-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox Database Permissions" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

## Use the EAC to configure circular logging for a database

1. In the EAC, go to **Servers** \> **databases**.

2. Select the mailbox database you want to configure and click ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. Check or uncheck the **Enable circular logging** checkbox, and then click **save**.

4. If a dismount and mount operation are required, a warning message will appear. Click **OK** to close the warning message.

    1. To dismount the database, click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon"), and then click **Dismount**. Click **yes** when the warning message appears.

    2. To mount the database, click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon"), and then click **Mount**. Click **yes** when the warning message appears.

## Use the Shell to configure circular logging for a database

This example enables circular logging for database DB1.

```powershell
Set-MailboxDatabase DB1 -CircularLoggingEnabled $True
```

This example disables circular logging for database DB1.

```powershell
Set-MailboxDatabase DB1 -CircularLoggingEnabled $False
```

See [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxDatabase) for other mailbox database parameters you can configure.
