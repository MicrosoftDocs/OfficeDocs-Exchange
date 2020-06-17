---
title: Overview of Document Fingerprinting in Exchange
TOCTitle: Document Fingerprinting
ms:assetid: 1e0c579c-26e0-462a-a1b0-d7506dfe05fa
ms:mtpsurl: https://technet.microsoft.com/library/Dn635176(v=EXCHG.150)
ms:contentKeyID: 61218308
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Document Fingerprinting

_**Applies to:** Exchange Server 2013_

Information workers in your organization handle many kinds of sensitive information during a typical day. *Document Fingerprinting* makes it easier for you to protect this information by identifying standard forms that are used throughout your organization. This topic describes the concepts behind Document Fingerprinting. If you'd like to learn how to create a document fingerprint, see [Protect form data with document fingerprinting](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/protect-data-with-fingerprinting).

## Basic scenario for Document Fingerprinting

Document Fingerprinting is a Data Loss Prevention (DLP) feature that converts a standard form into a sensitive information type, which you can use to define transport rules and DLP policies. For example, you can create a document fingerprint based on a blank patent template and then create a DLP policy that detects and blocks all outgoing patent templates with sensitive content filled in. Optionally, you can set up [Policy Tips](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/policy-tips) to notify senders that they might be sending sensitive information, and the sender should verify that the recipients are qualified to receive the patents. This process works with any text-based forms used in your organization. Additional examples of forms that you can upload include:

  - Government forms

  - Health Insurance Portability and Accountability Act (HIPAA) compliance forms

  - Employee information forms for Human Resources departments

  - Custom forms created specifically for your organization

Ideally, your organization already has an established business practice of using certain forms to transmit sensitive information. After you upload an empty form to be converted to a document fingerprint and set up a corresponding policy, the DLP agent will detect any documents in outbound mail that match that fingerprint.

## How Document Fingerprinting works

You've probably already guessed that documents don't have actual fingerprints, but the name helps explain the feature. In the same way that a person's fingerprints have unique patterns, documents have unique word patterns. When you upload a file, the DLP agent identifies the unique word pattern in the document, creates a document fingerprint based on that pattern, and uses that document fingerprint to detect outbound documents containing the same pattern. That's why uploading a form or template creates the most effective type of document fingerprint. Everyone who fills out a form uses the same original set of words and then adds his or her own words to the document. As long as the outbound document isn't password protected and contains all the text from the original form, the DLP agent can determine if the document matches the document fingerprint.

The following example shows what happens if you create a document fingerprint based on a patent template, but you can use any form as a basis for creating a document fingerprint.

**Example of a patent document matching a document fingerprint of a patent template**

![A patent document matching a document fingerprint.](images/Dn635176.9c952770-2cd4-4f62-9735-6d073344be7f(EXCHG.150).png "A patent document matching a document fingerprint.")

The patent template contains the blank fields "Patent title," "Inventors," and "Description" and descriptions for each of those fields, that's the word pattern. When you upload the original patent template, it's in one of the supported file types and in plain text. The DLP agent uses an algorithm to convert this word pattern into a document fingerprint, which is a small Unicode XML file containing a unique hash value representing the original text, and the fingerprint is saved as a data classification in Active Directory. (As a security measure, the original document itself isn't stored on the service; only the hash value is stored, and the original document can't be reconstructed from the hash value.) The patent fingerprint then becomes a sensitive information type that you can associate with a DLP policy. After you associate the fingerprint with a DLP policy, the DLP agent detects any outbound emails containing documents that match the patent fingerprint and deals with them according to your organization's policy. For example, you might want to set up a DLP policy that prevents regular employees from sending outgoing messages containing patents. The DLP agent will use the patent fingerprint to detect patents and block those emails. Alternatively, you might want to let your legal department to be able to send patents to other organizations because it has a business need for doing so. You can allow specific departments to send sensitive information by creating exceptions for those departments in your DLP policy, or you can allow them to override a policy tip with a business justification. For more detailed information about creating DLP policy rules and exceptions, see [DLP procedures](dlp-procedures-exchange-2013-help.md), and to learn more about setting up policy tips that users can override, see [Policy Tips in Exchange 2013](policy-tips-exchange-2013-help.md).

## Supported file types

Document Fingerprinting supports the same file types that are supported in transport rules. For a list of supported file types, see [Supported file types for transport rule content inspection](use-transport-rules-to-inspect-message-attachments-exchange-2013-help.md#supported-file-types-for-transport-rule-content-inspection). One quick note about file types: neither transport rules nor Document Fingerprinting supports the .dotx file type, which can be confusing because that's a template file in Word. When you see the word "template" in this and other Document Fingerprinting topics, it refers to a document that you have established as a standard form, not the template file type.

## Limitations of document fingerprinting

The Document Fingerprinting DLP agent won't detect sensitive information in the following cases:

  - Password protected files

  - Files that contain only images

  - Documents that don't contain all the text from the original form used to create the document fingerprint

## For more information

[Protect form data with document fingerprinting](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/protect-data-with-fingerprinting)

[Integrating sensitive information rules with transport rules](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/integrate-sensitive-information-rules)

[DLP procedures](dlp-procedures-exchange-2013-help.md)
