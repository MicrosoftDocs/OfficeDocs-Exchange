---
title: 'DLP policy templates supplied in Exchange: Exchange 2013 Help'
TOCTitle: DLP policy templates supplied in Exchange
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 7e1917ab-1920-4a52-97d1-7dfe2add6198
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# DLP policy templates supplied in Exchange 2013

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, you can use data loss prevention (DLP) policy templates as a starting point for building DLP policies that help you meet your specific regulatory and business policy needs. You can modify the templates to meet the specific needs of your organization.

> [!CAUTION]
> You should enable your DLP policies in test mode before running them in your production environment. During such tests, it is recommended that you configure sample user mailboxes and send test messages that invoke your test policies in order to confirm the results. > Use of these policies does not ensure compliance with any regulation. After your testing is complete, make the necessary configuration changes in Exchange so the transmission of information complies with your organization's policies. For example, you might need to configure TLS with known business partners or add more restrictive transport rule actions, such as adding rights protection to messages that contain a certain type of data.

## Templates available for DLP

The following table lists the DLP policy templates in Exchange. To learn more about customizing these templates to create DLP policies, see [Manage DLP policies](manage-dlp-policies-exchange-2013-help.md).

|**Template**|**Description**|
|:-----|:-----|
|Australia Financial Data|Helps detect the presence of information commonly considered to be financial data in Australia, including credit cards, and SWIFT codes.|
|Australia Health Records Act (HRIP Act)|Helps detect the presence of information commonly considered to be subject to the Health Records and Information Privacy (HRIP) act in Australia, like medical account number and tax file number.|
|Australia Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in Australia, like tax file number and driver's license.|
|Australia Privacy Act|Helps detect the presence of information commonly considered to be subject to the privacy act in Australia, like driver's license and passport number.|
|Canada Financial Data|Helps detect the presence of information commonly considered to be financial data in Canada, including bank account numbers and credit cards.|
|Canada Health Information Act (HIA)|Helps detect the presence of information subject to Canada Health Information Act (HIA) for Alberta, including data like passport numbers and health information.|
|Canada Personal Health Act (PHIPA) - Ontario|Helps detect the presence of information subject to Canada Personal Health Information Protection Act (PHIPA) for Ontario, including data like passport numbers and health information.|
|Canada Personal Health Information Act (PHIA) - Manitoba|Helps detect the presence of information subject to Canada Personal Health Information Act (PHIA) for Manitoba, including data like health information.|
|Canada Personal Information Protection Act (PIPA)|Helps detect the presence of information subject to Canada Personal Information Protection Act (PIPA) for British Columbia, including data like passport numbers and health information.|
|Canada Personal Information Protection Act (PIPEDA)|Helps detect the presence of information subject to Canada Personal Information Protection and Electronic Documents Act (PIPEDA), including data like passport numbers and health information.|
|Canada Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in Canada, like health ID number and social insurance number.|
|France Data Protection Act|Helps detect the presence of information commonly considered to be subject to the Data Protection Act in France, like the health insurance card number.|
|France Financial Data|Helps detect the presence of information commonly considered to be financial information in France, including information like credit card, account information, and debit card numbers.|
|France Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in France, including information like passport numbers.|
|Germany Financial Data|Helps detect the presence of information commonly considered to be financial data in Germany like EU debit card numbers.|
|Germany Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in Germany, including information like driver's license and passport numbers.|
|Israel Financial Data|Helps detect the presence of information commonly considered to be financial data in Israel, including bank account numbers and SWIFT codes.|
|Israel Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in Israel, like national ID number.|
|Israel Protection of Privacy|Helps detect the presence of information commonly considered to be subject to the Protection of Privacy in Israel, including information like bank account numbers or national ID.|
|Japan Financial Data|Helps detect the presence of information commonly considered to be financial information in Japan, including information like credit card, account information, and debit card numbers.|
|Japan Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in Japan, including information like driver's license and passport numbers.|
|Japan Protection of Personal Information|Helps detect the presence of information subject to Japan Protection of Personal Information, including data like resident registration numbers.|
|PCI Data Security Standard (PCI DSS)|Helps detect the presence of information subject to PCI Data Security Standard (PCI DSS), including information like credit card or debit card numbers.|
|Saudi Arabia - Anti-Cyber Crime Law|Helps detect the presence of information commonly considered to be subject to the Anti-Cyber Crime Law in Saudi Arabia, including international bank account numbers and SWIFT codes.|
|Saudi Arabia Financial Data|Helps detect the presence of information commonly considered to be financial data in Saudi Arabia, including international bank account numbers and SWIFT codes.|
|Saudi Arabia Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in Saudi Arabia, like national ID number.|
|U.K. Access to Medical Reports Act|Helps detect the presence of information subject to United Kingdom Access to Medical Reports Act, including data like National Health Service numbers.|
|U.K. Data Protection Act|Helps detect the presence of information subject to United Kingdom Data Protection Act, including data like national insurance numbers.|
|U.K. Financial Data|Helps detect the presence of information commonly considered to be financial information in United Kingdom, including information like credit card, account information, and debit card numbers.|
|U.K. Personal Information Online Code of Practice (PIOCP)|Helps detect the presence of information subject to United Kingdom Personal Information Online Code of Practice, including data like health information.|
|U.K. Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in United Kingdom, including information like driver's license and passport numbers.|
|U.K. Privacy and Electronic Communications Regulations|Helps detect the presence of information subject to United Kingdom Privacy and Electronic Communications Regulations, including data like financial information.|
|U.S. Federal Trade Commission (FTC) Consumer Rules|Helps detect the presence of information subject to U.S. Federal Trade Commission (FTC) Consumer Rules, including data like credit card numbers.|
|U.S. Financial Data|Helps detect the presence of information commonly considered to be financial information in United States, including information like credit card, account information, and debit card numbers.|
|U.S. Gramm-Leach-Bliley Act (GLBA)|Helps detect the presence of information subject to Gramm-Leach-Bliley Act (GLBA), including information like social security numbers or credit card numbers.|
|U.S. Health Insurance Act (HIPAA)|Helps detect the presence of information subject to United States Health Insurance Portability and Accountability Act (HIPAA),including data like social security numbers and health information.|
|U.S. Patriot Act|Helps detect the presence of information commonly subject to U.S. Patriot Act, including information like credit card numbers or tax identification numbers.|
|U.S. Personally Identifiable Information (PII) Data|Helps detect the presence of information commonly considered to be personally identifiable information (PII) in the United States, including information like social security numbers or driver's license numbers.|
|U.S. State Breach Notification Laws|Helps detect the presence of information subject to U.S. State Breach Notification Laws, including data like social security and credit card numbers.|
|U.S. State Social Security Number Confidentiality Laws|Helps detect the presence of information subject to U.S. State Social Security Number Confidentiality Laws, including data like social security numbers.|

## For more information

[Data loss prevention](data-loss-prevention-exchange-2013-help.md)

[Create a DLP policy from a template](create-dlp-policy-from-template-exchange-2013-help.md)
