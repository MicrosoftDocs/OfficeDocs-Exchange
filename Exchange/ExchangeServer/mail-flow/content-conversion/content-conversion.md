---
localization_priority: Normal
description: 'Summary: Learn about content conversion (message formatting and encoding options) in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bc367eb3-0306-4da9-9a84-4341caef77af
ms.reviewer:
title: Content conversion
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Content conversion

 *Content conversion* is the process of correctly formatting a message for each recipient. The decision to perform content conversion on a message depends on the destination and format of the message. They types of content conversion that occur in Exchange 2016 and Exchange 2019 are unchanged from Exchange 2013:

- **Message conversion for external recipients**: This type of content conversion includes the Transport Neutral Encapsulation Format (TNEF) conversion options and message encoding options for external recipients. Messages sent to recipients inside the Exchange organization don't require this type of content conversion. This type of content conversion is handled by the categorizer in the Transport service on a Mailbox server. Categorization on each message happens after a newly arrived message is put in the Submission queue. In addition to recipient resolution and routing resolution, content conversion is performed on the message before the message is put in a delivery queue. If a single message contains multiple recipients, the categorizer determines the appropriate encoding for each message recipient. Content conversion tracing doesn't capture any content conversion failures that the categorizer encounters as it converts messages sent to external recipients.

- **MAPI conversion for internal recipients**: his type of content conversion is handled by the Mailbox Transport service. The Mailbox Transport service exists on Mailbox servers to transmit messages between mailbox databases on the local server, and the Transport service on Mailbox servers. Specifically, the Mailbox Transport Submission service transmits messages from the sender's Outbox to the Transport service on a Mailbox server. The Mailbox Transport Delivery service transmits messages from the Transport service on a Mailbox server to the recipient's Inbox. The Mailbox Transport Submission service converts all outgoing messages from MAPI and the Mailbox Transport Delivery service converts all incoming messages to MAPI. Content conversion tracing captures these MAPI conversion failures. For more information, see [Managing Content Conversion Tracing](https://docs.microsoft.com/exchange/content-conversion-tracing-exchange-2013-help).

## Exchange and Outlook message formats
<a name="Exchange"> </a>

The following list describes the basic message formats available in Exchange and Outlook:

- **Plain text**: A plain text message uses only US-ASCII text as described in RFC 5322. The message can't contain different fonts or other text formatting. The following two formats can be used for a plain text message:

  - The message headers and the message body are composed of US-ASCII text. Attachments must be encoded by using *Uuencode*. Uuencode represents Unix-to-Unix encoding and defines an encoding algorithm to store binary attachments in the body of an email message by using US-ASCII text characters.

  - The message is MIME-encoded with a **Content-Type** value of `text/plain`, and a **Content-Transfer-Encoding** value of `7bit` for the text parts of a multipart message. Any message attachments are encoded by using Quoted-printable or Base64 encoding. By default, when you compose and send a plain text message in Outlook, the message is MIME-encoded with a **Content-Type** value of `text/plain`.

- **HTML**: An HTML message supports text formatting, background images, tables, bullet points, and other graphical elements. By definition, an HTML-formatted message must be MIME-encoded to preserve these formatting elements.

- **Rich text format (RTF)**: RTF supports text formatting and other graphical elements. RTF is synonymous with TNEF (TNEF and RTF can be used interchangeably). The rich text message format is completely different from the rich text document format that's available in Word.

- **TNEF**: The Transport Neutral Encapsulation Format is a Microsoft-specific format for encapsulating MAPI message properties. A TNEF message contains a plain text version of the message and an attachment that packages the original formatted version of the message. Typically, this attachment is named Winmail.dat. The Winmail.dat attachment includes the following information:

  - Original formatted version of the message (for example, fonts, text sizes, and text colors)

  - OLE objects (for example, embedded pictures or embedded Office documents)

  - Special Outlook features (for example, custom forms, voting buttons, or meeting requests)

  - Regular message attachments that were in the original message

    The resulting plain text message can be represented in the following formats:

  - RFC 5322-compliant message composed of only US-ASCII text with a Winmail.dat attachment encoded in Uuencode

  - Multipart MIME-encoded message that has a Winmail.dat attachment

    Outlook and other email clients that fully understand TNEF process the Winmail.dat attachment and display the original message content without ever displaying the Winmail.dat attachment. Email clients that don't understand TNEF may present TNEF messages in any of the following ways:

  - The plain text version of the message is displayed, and the message contains an attachment named Winmail.dat, Win.dat, or some other generic name such as Att _nnnnn_.dat or Att _nnnnn_.eml where the _nnnnn_ placeholder represents a random number.

  - The plain text version of the message is displayed. The TNEF attachment is ignored or removed. The result is a plain text message.

  - Messaging servers that understand TNEF can be configured to remove TNEF attachments from incoming messages. The result is a plain text message. Moreover, some email clients may not understand TNEF, but recognize and ignore TNEF attachments. The result is a plain text message.

    There are third-party utilities that can help convert Winmail.dat attachments.

    TNEF is understood by all versions of Exchange since Exchange Server version 5.5.

- **Summary Transport Neutral Encapsulation Format (STNEF)**: STNEF is equivalent to TNEF. However, STNEF messages are encoded differently than TNEF messages. Specifically, STNEF messages are always MIME-encoded, and always have the **Content-Transfer-Encoding** value `Binary`. Therefore, there's no plain text representation of the message, and there's no distinct Winmail.dat attachment contained in the body of the message. The whole message is represented by using only binary data. Messages that have a **Content-Transfer-Encoding** value of **Binary** can only be transferred between messaging servers that support and advertise the **BINARYMIME** and **CHUNKING** SMTP extensions as defined in RFC 3030. The messages are always transferred between messaging servers by using the **BDAT** command, instead of the standard **DATA** command.

    STNEF is understood by all versions of Exchange since Exchange 2000. STNEF is automatically used for all messages transferred between Exchange servers in the organization since native mode Exchange Server 2003.

    Exchange never sends STNEF messages to external recipients. Only TNEF messages can be sent to recipients outside the Exchange organization.

## Content conversion options for external recipients
<a name="External"> </a>

The content conversion options that you can set in an Exchange organization for external recipients can be described in the following categories:

- **TNEF conversion options**: These conversion options specify whether TNEF should be preserved or removed from messages that leave the Exchange organization.

- **Message encoding options**: These options specify message encoding options, such as MIME and non-MIME character sets, message encoding, and attachment formats.

These conversion and encoding options are independent of one another. For example, whether TNEF messages can leave the Exchange organization isn't related to the MIME encoding settings or plain text encoding settings of those messages.

You can specify the content conversion at various levels of the Exchange organization as described in the following list:

- **Remote domain settings**: Remote domains define the settings for outgoing message transfers between the Exchange organization and external domains.. Even if you don't create remote domain entries for specific domains, there's a predefined remote domain named Default that applies to all remote address spaces (\*). For more information about remote domains, see [Remote Domains](https://docs.microsoft.com/exchange/remote-domains-exchange-2013-help).

- **Mail user and mail contact settings**: Mail users and mail contacts are similar because both have external email addresses and contain information about people outside the Exchange organization. The main difference is mail users have accounts that they can use to log on to Active Directory and access resources in the organization. For more information, see [Recipients](../../recipients/recipients.md).

- **Outlook settings**: You can set these message formatting and encoding options in Outlook:

  - **Message format**: You can set the default message format for all messages. You can override the default message format as you compose a specific message.

  - **Internet message format**: You can control whether TNEF messages are sent to remote recipients or whether they are first converted to a more compatible format. You can also specify various message encoding options for messages sent to remote recipients. These settings don't apply to messages sent to recipients in the Exchange organization.

  - **Internet recipient message format (Outlook 2010 or earlier)**: You can control whether TNEF messages are sent to specific contacts in your Contacts folder. These conversion options aren't available for recipients in the Exchange organization.

  - **Internet recipient message encoding options (Outlook 2010 or earlier)**: You can control the MIME or plain text encoding options for specific contacts in your Contacts folder. These conversion options aren't available for recipients in the Exchange organization.

  - **International options**: You can control the character sets used in messages.

    For more information about these settings, see [TNEF conversion options](tnef-conversion.md) and [Message encoding options in Exchange Server](message-encoding.md).

## Understanding the structure of email messages
<a name="Understanding"> </a>

To better understand the content conversion options for external recipients, you need to understand the structure of email messages. An SMTP message is based on plain 7-bit US-ASCII text to compose and send email messages. A standard SMTP message consists of the following elements:

- **Message envelope**: The message envelope is defined in RFC 5321. The message envelope contains information required to transmit and deliver the message. Recipients never see the message envelope, because it's generated by the message transmission process and isn't actually part of the message contents.

- **Message contents**: The message contents are defined in RFC 5322. The message contents consist of the following elements:

  - **Message header**: The message header is a collection of header fields. Header fields consist of a field name, followed by a colon (:) character, followed by a field body, and ended by a carriage return/line feed (CR/LF) character combination.

    A field name must be composed of printable US-ASCII text characters except the colon (:) character. Specifically, ASCII characters that have values from 33 through 57 and 59 through 126 are permitted.

    A field body may be composed of any US-ASCII characters, except for the carriage return (CR) character and the line feed (LF) character. However, a field body may contain the CR/LF character combination when used in *header folding*. Header folding is the separation of a single header field body into multiple lines as described in section 2.2.3 of RFC 5322. Other field body syntax requirements are described in sections 3 and 4 of RFC 5322.

  - **Message body**: The message body is a collection of lines of US-ASCII text characters that appears after the message header. The message header and the message body are separated by a blank line that ends with the CR/LF character combination. The message body is optional. Any line of text in the message body must be less than 998 characters. The CR and LF characters can only appear together to indicate the end of a line.

When SMTP messages contain elements that aren't plain US-ASCII text, the message must be encoded to preserve those elements. The MIME standard defines a method of encoding content in messages that isn't text. MIME allows for text in other character sets, attachments without text, multipart message bodies, and header fields in other character sets. MIME is defined in RFC 2045, RFC 2046, RFC 2047, RFC 4288, RFC 4289, and RFC 2049. MIME defines a collection of header fields that specifies additional message attributes. The following sections describe some important MIME header fields.

### MIME-Version header field

 **Default value**: `1.0`

This header field is the first MIME header field that appears in a MIME-formatted message. This header field appears after the other standard RFC 5322 header fields, but before any other MIME header fields. MIME-aware email clients use this header field to identify a MIME-encoded message. When this header field is absent, MIME-aware email clients identify the message as plain text.

### Content-Type header field

 **Default value**: `text/plain`

This header field identifies the media type of the message content as described in RFC 2046. A media type consists of:

- **A type**:

  - Types that begin with `x-` aren't standard. The Internet Assigned Numbers Authority (IANA) maintains a list of registered media types. For more information, see [MIME Media Types](https://www.iana.org/assignments/media-types/).

  - The *multipart* media type allows for multiple message parts in the same message by using sections defined by different media types. Some **Content-Type** field values include `text/plain`, `text/html`, `multipart/mixed`, and `multipart/alternative`.

- **A subtype**: Subtypes that begin with `vnd.` are vendor-specific.

- **One or more optional parameters**: For example, a `charset=` parameter that defines the MIME character encoding.

### Content-Transfer-Encoding header field

 **Default value**: `7bit`

This header field can describe the following information about a message:

- The encoding algorithm used to transform any non-US-ASCII text or binary data that exists in the message body.

- An indicator that describes the current condition of the message body.

There can be multiple values of the **Content-Transfer-Encoding** header field in a MIME message. When the **Content-Transfer-Encoding** header field appears in the message header, it applies to the whole body of the message. When the **Content-Transfer-Encoding** header field appears in one of the parts of a multipart message, it applies only to that part of the message.

When an encoding algorithm is applied to the message body data, the message body data is transformed into plain US-ASCII text. This transformation allows the message to travel through older messaging servers that only support messages in US-ASCII text. The **Content-Transfer-Encoding** header field values that indicate an encoding algorithm was used on the message body are:

- `Quoted-printable`: Uses printable US-ASCII characters to encode the message body data. If the original message text is mostly US-ASCII text, Quoted-printable encoding gives somewhat readable and compact results. All printable US-ASCII text characters except the equal sign (=) character can be represented without encoding.

- `Base64`: Based primarily on the privacy-enhanced mail (PEM) standard defined in RFC 4648. Base64 encoding uses the 64-character alphabet encoding algorithm and output padding characters defined by PEM to encode the message body data. A Base64 encoded message is typically 33 percent larger than the original message. Base64 encoding creates a predictable increase in message size and is optimal for binary data and non-US-ASCII text.

Typically, you won't see multiple encoding algorithms used in the same message.

When no encoding algorithm has been used on the message body, the **Content-Transfer-Encoding** header field merely identifies the current condition of the message body data. The **Content-Transfer-Encoding** header field values that indicate that no encoding algorithms were used on the message body are:

- `7bit`: Indicates that the message body data is already in the RFC 5322 format. Specifically, this means that the following conditions must be true:

  - All lines of text must be less than 998 characters long.

  - All characters must be US-ASCII text that have character values from 1 through 127.

  - The CR and LF characters can only be used together to indicate the end of a line of text.

    The whole message body may be 7-bit, or part of the message body in a multipart message may be 7-bit. If the multipart message contains other parts that have any binary data or non-US-ASCII text, that part of the message must be encoded using the Quoted-printable or Base64 encoding algorithms.

    Messages that have 7-bit bodies can travel between messaging servers by using the standard DATA command.

- `8bit`: Indicates that the message body data contains non-US-ASCII characters. Specifically, this means that the following conditions must be true:

  - All lines of text must be less than 998 characters long.

  - One or more characters in the message body have values larger than 127.

  - The CR and LF characters can only be used together to indicate the end of a line of text.

    The whole message body may be 8-bit, or part of the message body in a multipart message may be 8-bit. If the multipart message contains other parts that have binary data, that part of the message must be encoded using the Quoted-printable or Base64 encoding algorithms.

    Messages that have 8-bit bodies can only travel between messaging servers that support the **8BITMIME** SMTP extension as defined in RFC 6152, such as Exchange 2000 Server or later. Specifically, this means that the following conditions must be true:

  - The **8BITMIME** keyword must be advertised in the server's EHLO response.

  - Messages are still transferred by using the SMTP standard **DATA** command. However, the `BODY=8BITMIME` parameter must be added to the end of the **MAIL FROM** command.

- `Binary`: Indicates that the message body contains non-US-ASCII text or binary data. Specifically, this means that the following conditions are true:

  - Any sequence of characters is allowed.

  - There is no line length limitation.

  - Binary message elements don't require encoding.

    Messages that have binary bodies can only travel between messaging servers that support the **BINARYMIME** SMTP extension as defined in RFC 3030, such as Exchange 2000 Server or later. Specifically, this means that the following conditions must be true:

  - The **BINARYMIME** keyword must be advertised in the server's EHLO response.

  - The **BINARYMIME** SMTP extension can only be used with the **CHUNKING** SMTP extension. *Chunking* enables large message bodies to be sent in multiple, smaller chunks. Chunking is also defined in RFC 3030. The **CHUNKING** keyword must also be advertised in the server's EHLO response.

  - Messages are transferred using the **BDAT** command instead of the standard **DATA** command.

  - The `BODY=BINARYMIME` parameter must be added to the end of the **MAIL FROM** command when the message has a message body.

The values `7bit`, `8bit`, and `Binary` never exist together in the same multipart message (the values are mutually exclusive). The `Quoted-printable` or `Base64` values may appear in a 7-bit or 8-bit multipart message body, but never in a binary message body. If a multipart message body contains different parts composed of 7-bit and 8-bit content, the whole message is classified as 8-bit. If a multipart message body contains different parts composed of 7-bit, 8-bit, and binary content, the whole message is classified as binary.

### Content-Disposition header field

 **Default value**: `Attachment`

This header field instructs a MIME-enabled email client on how it should display an attached file, and is described in RFC 2183. Valid values are:

- `Inline`: The attachment is displayed in the message body.

- `Attachment`: The attached file appears as a regular attachment separate from the message body. Other parameters are also with this values (for example, `Filename`, `Creation-date`, and `Size`).
