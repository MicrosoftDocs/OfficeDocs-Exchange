---
localization_priority: Normal
description: "Summary: Learn about how Exchange ActiveSync lets enabled mobile devices access a user's Microsoft Exchange mailbox data any time, anywhere."
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 93a949e7-b3ef-43ea-ae0c-6698826fc8d2
ms.reviewer: 
title: Mobile devices
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Mobile devices

There are many different mobile phones and devices enabled for Exchange ActiveSync. These include Android phones and tablets, as well as the Apple iPhone, iPod, and iPad.

Both phone and non-phone mobile devices support Exchange ActiveSync, and in most Exchange ActiveSync documentation, we use the term *mobile device*. Unless the feature or features we're discussing require a cellular telephone signal, such as SMS message notification, the term mobile device applies to both mobile phones and other mobile devices such as tablets.

## What Exchange ActiveSync does

Exchange ActiveSync is a communications protocol that enables over-the-air mobile access to email messages, scheduling data, contacts, and tasks. Exchange ActiveSync is available on third-party phones that are enabled for Exchange ActiveSync.

Exchange ActiveSync offers Direct Push technology. Direct Push uses an encrypted HTTPS connection that's established and maintained between the mobile device and the server to push new email messages and other Exchange data to the phone.

To use Direct Push with Microsoft Exchange Server 2013, your users must have a mobile device that's designed to support Direct Push.

### Exchange ActiveSync features

Exchange ActiveSync provides access to many different features that enable you to enforce security policies on mobile devices. By using Exchange 2013, you can configure multiple mobile device mailbox policies and control which mobile devices can synchronize with your Exchange server. Exchange ActiveSync enables you to send a remote device wipe command that wipes all data from a mobile device in case that mobile device is lost or stolen. Users can also initiate a remote device wipe from Outlook Web App.

Exchange ActiveSync lets users generate a recovery password. This recovery password is saved on the mobile device and is used when a user forgets their password. The user generates the recovery password at the same time that they generate the device password or PIN. This recovery password can be used to unlock the mobile device. Immediately after this recovery password is used, the user will be required to create a new PIN.

## POP3 and IMAP4 limitations

If your mobile device doesn't support Exchange ActiveSync, or you don't need the rich feature set that Exchange ActiveSync provides, you can use POP3 or IMAP4 to access your email on your mobile device. For more information about POP3 and IMAP4 access to your mailbox, see [POP3 and IMAP4 in Exchange Server](../../clients/pop3-and-imap4/pop3-and-imap4.md).
