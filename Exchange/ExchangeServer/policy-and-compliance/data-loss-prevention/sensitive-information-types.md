---
localization_priority: Normal
description: 'Summary: Learn about the sensitive information types you can use when setting up DLP policies in your Exchange Server organization.'
ms.topic: reference
author: chrisda
ms.author: chrisda
ms.assetid: 98b81f9c-87bb-4905-8e53-04621c3ae74d
ms.date: 7/8/2018
ms.reviewer: 
title: Sensitive information types in Exchange Server
ms.collection: exchange-server
audience: ITPro
keywords:
- data loss prevention in office 365,Data loss prevetion in Exchange,DLP,sensitive information types
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Sensitive information types in Exchange Server

 Data loss prevention (DLP) includes 80 sensitive information types that are ready for you to use in your DLP policies. This topic lists all of these sensitive information types and shows what a DLP policy looks for when it detects each type. A sensitive information type is defined by a pattern that can be identified by a regular expression or a function. In addition, corroborative evidence such as keywords and checksums can be used to identify a sensitive information type. Confidence level and proximity are also used in the evaluation process.

## ABA Routing Number

 **Format**: 9 digits which may be in a formatted or unformatted pattern.

 **Pattern**:

Formatted:

- Four digits beginning with 0, 1, 2, 3, 6, 7, or 8

- A hyphen

- Four digits

- A hyphen

- A digit

Unformatted: 9 consecutive digits beginning with 0, 1, 2, 3, 6, 7, or 8

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_aba_routing` finds content that matches the pattern.

- A keyword from `Keyword_ABA_Routing` is found.

```
<!-- ABA Routing Number -->
<Entity id="cb353f78-2b72-4c3c-8827-92ebe4f69fdf" patternsProximity="300" recommendedConfidence="75">
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_aba_routing" />
        <Match idRef="Keyword_ABA_Routing" />
      </Pattern>
 </Entity>
```

 **Keywords**:

|**Keyword_ABA_Routing**|
|:-----|
|aba  <br/> aba #  <br/> aba routing #  <br/> aba routing number  <br/> aba#  <br/> abarouting#  <br/> aba number  <br/> abaroutingnumber  <br/> american bank association routing #  <br/> american bank association routing number  <br/> americanbankassociationrouting#  <br/> americanbankassociationroutingnumber  <br/> bank routing number  <br/> bankrouting#  <br/> bankroutingnumber  <br/> routing transit number  <br/> RTN|

## Argentina National Identity (DNI) Number

 **Format**: Eight digits separated by periods

 **Pattern**: Eight digits:

- Two digits

- A period

- Three digits

- A period

- Three digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_argentina_national_id` finds content that matches the pattern.

- A keyword from `Keyword_argentina_national_id` is found.

```
<!-- Argentina National Identity (DNI) Number -->
<Entity id="eefbb00e-8282-433c-8620-8f1da3bffdb2" recommendedConfidence="75" patternsProximity="300">
   <Pattern confidenceLevel="75">
      <IdMatch idRef="Regex_argentina_national_id"/>
      <Match idRef="Keyword_argentina_national_id"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_argentina_national_id**|
|:-----|
|Argentina National Identity number  <br/> Identity  <br/> Identification National Identity Card  <br/> DNI  <br/> NIC National Registry of Persons  <br/> Documento Nacional de Identidad  <br/> Registro Nacional de las Personas  <br/> Identidad  <br/> Identificación|

## Australia Bank Account Number

 **Format**: 6-10 digits with or without a bank state branch number

 **Pattern**: Account number is 6-10 digits. Australia bank state branch number:

- Three digits

- A hyphen

- Three digits

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_australia_bank_account_number` finds content that matches the pattern..

- A keyword from `Keyword_australia_bank_account_number` is found.

- The regular expression `Regex_australia_bank_account_number_bsb` finds content that matches the pattern.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_australia_bank_account_number` finds content that matches the pattern..

- A keyword from `Keyword_australia_bank_account_number` is found.

```
<!-- Australia Bank Account Number -->
<Entity id="74a54de9-2a30-4aa0-a8aa-3d9327fc07c7" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Regex_australia_bank_account_number" />
        <Match idRef="Keyword_australia_bank_account_number" />
        <Match idRef="Regex_australia_bank_account_number_bsb" />
  </Pattern>
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_australia_bank_account_number" />
        <Match idRef="Keyword_australia_bank_account_number" />
  </Pattern>
 </Entity>
```

 **Keywords**:

|**Keyword_australia_bank_account_number**|
|:-----|
|swift bank code  <br/> correspondent bank  <br/> base currency  <br/> usa account  <br/> holder address  <br/> bank address  <br/> information account  <br/> fund transfers  <br/> bank charges  <br/> bank details  <br/> banking information  <br/> full names  <br/> iaea|

## Australia Driver's License Number

 **Format**: Nine letters and digits

 **Pattern**: Nine letters and digits:

- Two digits or letters (not case sensitive)

- Two digits

- Five digits or letters (not case sensitive)

  OR

- 1-2 optional letters (not case sensitive)

- 4-9 digits

  OR

- Nine digits or letters (not case sensitive)

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_australia_drivers_license_number` finds content that matches the pattern.

- A keyword from `Keyword_australia_drivers_license_number` is found.

- No keyword from `Keyword_australia_drivers_license_number_exclusions` is found.

```
<!-- Australia Drivers License Number -->
<Entity id="1cbbc8f5-9216-4392-9eb5-5ac2298d1356" patternsProximity="300" recommendedConfidence="75">
   <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_australia_drivers_license_number" />
        <Match idRef="Keyword_australia_drivers_license_number" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_australia_drivers_license_number_exclusions" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_australia_drivers_license_number**|**Keyword_australia_drivers_license_number_exclusions**|
|:-----|:-----|
|international driving permits  <br/> australian automobile association  <br/> sydney nsw  <br/> international driving permit  <br/> DriverLicence  <br/> DriverLicences  <br/> Driver Lic  <br/> Driver Licence  <br/> Driver Licences  <br/> DriversLic  <br/> DriversLicence  <br/> DriversLicences  <br/> Drivers Lic  <br/> Drivers Lics  <br/> Drivers Licence  <br/> Drivers Licences  <br/> Driver'Lic  <br/> Driver'Lics  <br/> Driver'Licence  <br/> Driver'Licences  <br/> Driver' Lic  <br/> Driver' Lics  <br/> Driver' Licence  <br/> Driver' Licences  <br/> Driver'sLic  <br/> Driver'sLics  <br/> Driver'sLicence  <br/> Driver'sLicences  <br/> Driver's Lic  <br/> Driver's Lics  <br/> Driver's Licence  <br/> Driver's Licences  <br/> DriverLic#  <br/> DriverLics#  <br/> DriverLicence#  <br/> DriverLicences#  <br/> Driver Lic#  <br/> Driver Lics#  <br/> Driver Licence#  <br/> Driver Licences#  <br/> DriversLic#  <br/> DriversLics#  <br/> DriversLicence#  <br/> DriversLicences#  <br/> Drivers Lic#  <br/> Drivers Lics#  <br/> Drivers Licence#  <br/> Drivers Licences#  <br/> Driver'Lic#  <br/> Driver'Lics#  <br/> Driver'Licence#  <br/> Driver'Licences#  <br/> Driver' Lic#  <br/> Driver' Lics#  <br/> Driver' Licence#  <br/> Driver' Licences#  <br/> Driver'sLic#  <br/> Driver'sLics#  <br/> Driver'sLicence#  <br/> Driver'sLicences#  <br/> Driver's Lic#  <br/> Driver's Lics#  <br/> Driver's Licence#  <br/> Driver's Licences#|aaa  <br/> DriverLicense  <br/> DriverLicenses  <br/> Driver License  <br/> Driver Licenses  <br/> DriversLicense  <br/> DriversLicenses  <br/> Drivers License  <br/> Drivers Licenses  <br/> Driver'License  <br/> Driver'Licenses  <br/> Driver' License  <br/> Driver' Licenses  <br/> Driver'sLicense  <br/> Driver'sLicenses  <br/> Driver's License  <br/> Driver's Licenses  <br/> DriverLicense#  <br/> DriverLicenses#  <br/> Driver License#  <br/> Driver Licenses#  <br/> DriversLicense#  <br/> DriversLicenses#  <br/> Drivers License#  <br/> Drivers Licenses#  <br/> Driver'License#  <br/> Driver'Licenses#  <br/> Driver' License#  <br/> Driver' Licenses#  <br/> Driver'sLicense#  <br/> Driver'sLicenses#  <br/> Driver's License#  <br/> Driver's Licenses#|

## Australia Medical Account Number

 **Format**: 10-11 digits

 **Pattern**: 10-11 digits:

- First digit is in the range 2-6

- Ninth digit is a check digit

- Tenth digit is the issue digit

- Eleventh digit (optional) is the individual number

 **Checksum**: Yes

 **Definition**:

A DLP policy is 95% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_australian_medical_account_number` finds content that matches the pattern.

- A keyword from `Keyword_Australia_Medical_Account_Number` is found.

- The checksum passes.

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_australian_medical_account_number` finds content that matches the pattern.

- The checksum passes.

```
<!-- Australia Medical Account Number -->
<Entity id="104a99a0-3d3b-4542-a40d-ab0b9e1efe63" recommendedConfidence="85" patternsProximity="300">
    <Pattern confidenceLevel="95">
     <IdMatch idRef="Func_australian_medical_account_number"/>
     <Any minMatches="1">
     <Match idRef="Keyword_Australia_Medical_Account_Number"/>
     </Any>
  </Pattern>
<Pattern confidenceLevel="85">
     <IdMatch idRef="Func_australian_medical_account_number"/>
     <Any minMatches="0" maxMatches="0">
  <Match idRef="Keyword_Australia_Medical_Account_Number"/>
     </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_Australia_Medical_Account_Number**|
|:-----|
|bank account details  <br/> medicare payments  <br/> mortgage account  <br/> bank payments  <br/> information branch  <br/> credit card loan  <br/> department of human services  <br/> local service  <br/> medicare|

## Australia Passport Number

 **Format**: A letter followed by seven digits

 **Pattern**: A letter (not case sensitive) followed by seven digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_australia_passport_number` finds content that matches the pattern.

- A keyword from `Keyword_passport` or `Keyword_australia_passport_number` is found.

```
<!-- Australia Passport Number -->
<Entity id="29869db6-602d-4853-ab93-3484f905df50" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_australia_passport_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_passport" />
          <Match idRef="Keyword_australia_passport_number" />
        </Any>
   </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_passport**|**Keyword_australia_passport_number**|
|:-----|:-----|
|Passport Number  <br/> Passport No  <br/> Passport #  <br/> Passport#  <br/> PassportID  <br/> Passportno  <br/> passportnumber  <br/> パスポート  <br/> パスポート番号  <br/> パスポートのNum  <br/> パスポート ＃  <br/> Numéro de passeport  <br/> Passeport n °  <br/> Passeport Non  <br/> Passeport #  <br/> Passeport#  <br/> PasseportNon  <br/> Passeportn °|passport  <br/> passport details  <br/> immigration and citizenship  <br/> commonwealth of australia  <br/> department of immigration  <br/> residential address  <br/> department of immigration and citizenship  <br/> visa  <br/> national identity card  <br/> passport number  <br/> travel document  <br/> issuing authority|

## Australia Tax File Number

 **Format**: 8-9 digits

 **Pattern**: 8-9 digits typically presented with spaces as follows:

- Three digits

- An optional space

- Three digits

- An optional space

- 2-3 digits where the last digit is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 95% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_australian_tax_file_number` finds content that matches the pattern.

- A keyword from `Keyword_Australia_Tax_File_Number` is found.

- No keyword from `Keyword_number_exclusions` is found.

- The checksum passes.

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_australian_tax_file_number` finds content that matches the pattern.

- No keyword from `Keyword_Australia_Tax_File_Number` or `Keyword_number_exclusions` is found.

- The checksum passes.

```
<!-- Australia Tax File Number -->
<Entity id="e29bc95f-ff70-4a37-aa01-04d17360a4c5" patternsProximity="300" recommendedConfidence="85">
    <Pattern confidenceLevel="95">
        <IdMatch idRef="Func_australian_tax_file_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_Australia_Tax_File_Number" />
        </Any>
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_number_exclusions" />
        </Any>
  </Pattern>
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_australian_tax_file_number" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_Australia_Tax_File_Number" />
          <Match idRef="Keyword_number_exclusions" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_Australia_Tax_File_Number**|**Keyword_number_exclusions**|
|:-----|:-----|
|australian business number  <br/> marginal tax rate  <br/> medicare levy  <br/> portfolio number  <br/> service veterans  <br/> withholding tax  <br/> individual tax return  <br/> tax file number|00000000  <br/> 11111111  <br/> 22222222  <br/> 33333333  <br/> 44444444  <br/> 55555555  <br/> 66666666  <br/> 77777777  <br/> 88888888  <br/> 99999999  <br/> 000000000  <br/> 111111111  <br/> 222222222  <br/> 333333333  <br/> 444444444  <br/> 555555555  <br/> 666666666  <br/> 777777777  <br/> 888888888  <br/> 999999999  <br/> 0000000000  <br/> 1111111111  <br/> 2222222222  <br/> 3333333333  <br/> 4444444444  <br/> 5555555555  <br/> 6666666666  <br/> 7777777777  <br/> 8888888888  <br/> 9999999999|

## Belgium National Number

 **Format**: 11 digits plus delimiters

 **Pattern**: 11 digits plus delimiters:

- Six digits and two periods in the format YY.MM.DD for date of birth

- A hyphen

- Three sequential digits (odd for males, even for females)

- A period

- Two digits that are a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_belgium_national_number` finds content that matches the pattern.

- A keyword from `Keyword_belgium_national_number` is found.

- The checksum passes.

```
<!-- Belgium National Number -->
  <Entity id="fb969c9e-0fd1-4b18-8091-a2123c5e6a54" recommendedConfidence="75" patternsProximity="300">
   <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_belgium_national_number"/>
     <Match idRef="Keyword_belgium_national_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_belgium_national_number**|
|:-----|
|Identity  <br/> Registration  <br/> Identification  <br/> ID  <br/> Identiteitskaart  <br/> Registratie nummer  <br/> Identificatie nummer  <br/> Identiteit  <br/> Registratie  <br/> Identificatie  <br/> Carte d'identité  <br/> numéro d'immatriculation  <br/> numéro d'identification  <br/> identité  <br/> inscription  <br/> Identifikation  <br/> Identifizierung  <br/> Identifikationsnummer  <br/> Personalausweis  <br/> Registrierung  <br/> Registrationsnummer|

## Brazil Legal Entity Number (CNPJ)

 **Format**: 14 digits that include a registration number, branch number, and check digits, plus delimiters

 **Pattern**: 14 digits, plus delimiters:

- Two digits

- A period

- Three digits

- A period

- Three digits (these first eight digits are the registration number)

- A forward slash

- Four-digit branch number

- A hyphen

- Two digits which are check digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_brazil_cnpj` finds content that matches the pattern.

- A keyword from `Keyword_brazil_cnpj` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_brazil_cnpj` finds content that matches the pattern.

- The checksum passes.

```
<!-- Brazil Legal Entity Number (CNPJ) -->
<Entity id="9b58b5cd-5e90-4df6-b34f-1ebcc88ceae4" recommendedConfidence="85" patternsProximity="300">
   <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_brazil_cnpj"/>
     <Match idRef="Keyword_brazil_cnpj"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_brazil_cnpj"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_brazil_cnpj**|
|:-----|
|CNPJ  <br/> CNPJ/MF  <br/> CNPJ-MF  <br/> National Registry of Legal Entities  <br/> Taxpayers Registry  <br/> Legal entity  <br/> Legal entities  <br/> Registration Status  <br/> Business  <br/> Company  <br/> CNPJ  <br/> Cadastro Nacional da Pessoa Jurídica  <br/> Cadastro Geral de Contribuintes  <br/> CGC  <br/> Pessoa jurídica  <br/> Pessoas jurídicas  <br/> Situação cadastral  <br/> Inscrição  <br/> Empresa|

## Brazil CPF Number

 **Format**: 11 digits that include a check digit and can be formatted or unformatted

 **Pattern**:

Formatted:

- Three digits

- A period

- Three digits

- A period

- Three digits

- A hyphen

- Two digits which are check digits

Unformatted: 11 digits where the last two digits are check digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_brazil_cpf` finds content that matches the pattern.

- A keyword from `Keyword_brazil_cpf` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_brazil_cpf` finds content that matches the pattern.

- The checksum passes.

```
<!-- Brazil CPF Number -->
<Entity id="78e09124-f2c3-4656-b32a-c1a132cd2711" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_brazil_cpf"/>
     <Match idRef="Keyword_brazil_cpf"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_brazil_cpf"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_brazil_cpf**|
|:-----|
|CPF  <br/> Identification  <br/> Registration  <br/> Revenue  <br/> Cadastro de Pessoas Físicas  <br/> Imposto  <br/> Identificação  <br/> Inscrição  <br/> Receita|

## Brazil National ID Card (RG)

 **Format**:

- Registro Geral (old format): Nine digits plus delimiters

- Registro de Identidade (RIC) (new format): 11 digits plus a hyphen

 **Pattern**:

Registro Geral (old format):

- Two digits

- A period

- Three digits

- A period

- Three digits

- A hyphen

- One digit which is a check digit

Registro de Identidade (RIC) (new format)

- 10 digits

- A hyphen

- One digit which is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_brazil_rg` finds content that matches the pattern.

- A keyword from `Keyword_brazil_rg` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_brazil_rg` finds content that matches the pattern.

- The checksum passes.

```
<!-- Brazil National ID Card (RG) -->
<Entity id="486de900-db70-41b3-a886-abdf25af119c" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_brazil_rg"/>
     <Match idRef="Keyword_brazil_rg"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_brazil_rg"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_brazil_rg**|
|:-----|
|National ID  <br/> Registration  <br/> Cédula de identidade  <br/> Registro Geral  <br/> RG  <br/> Registro de Identidade  <br/> RIC  <br/> Número de registo  <br/> Registro|

## Canada Bank Account Number

 **Format**: Seven or twelve digits

 **Pattern**: A Canada Bank Account Number is seven or twelve digits. A Canada bank account transit number is:

- Five digits

- A hyphen

- Three digits

    OR

- A zero "0"

- Eight digits

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_canada_bank_account_number` finds content that matches the pattern.

- A keyword from `Keyword_canada_bank_account_number` is found.

- The regular expression `Regex_canada_bank_account_transit_number` finds content that matches the pattern.

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_canada_bank_account_number` finds content that matches the pattern.

- A keyword from `Keyword_canada_bank_account_number` is found.

```
<!-- Canada Bank Account Number -->
<Entity id="552e814c-cb50-4d94-bbaa-bb1d1ffb34de" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Regex_canada_bank_account_number" />
        <Match idRef="Keyword_canada_bank_account_number" />
        <Match idRef="Regex_canada_bank_account_transit_number" />
   </Pattern>
   <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_canada_bank_account_number" />
        <Match idRef="Keyword_canada_bank_account_number" />
   </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_canada_bank_account_number**|
|:-----|
|canada savings bonds  <br/> canada revenue agency  <br/> canadian financial institution  <br/> direct deposit form  <br/> canadian citizen  <br/> legal representative  <br/> notary public  <br/> commissioner for oaths  <br/> child care benefit  <br/> universal child care  <br/> canada child tax benefit  <br/> income tax benefit  <br/> harmonized sales tax  <br/> social insurance number  <br/> income tax refund  <br/> child tax benefit  <br/> territorial payments  <br/> institution number  <br/> deposit request  <br/> banking information  <br/> direct deposit|

## Canada Driver's License Number

 **Format**: Varies by province

 **Pattern**: Various patterns covering Alberta, British Columbia, Manitoba, New Brunswick, Newfoundland/Labrador, Nova Scotia, Ontario, Prince Edward Island, Quebec, and Saskatchewan

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_[province_name]_drivers_license_number` finds content that matches the pattern.

- A keyword from `Keyword_[province_name]_drivers_license_name` is found.

- A keyword from `Keyword_canada_drivers_license` is found.

```
<!-- Canada Driver's License Number -->
    <Entity id="37186abb-8e48-4800-ad3c-e3d1610b3db0" patternsProximity="300" recommendedConfidence="75">
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_alberta_drivers_license_number" />
        <Match idRef="Keyword_alberta_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_british_columbia_drivers_license_number" />
        <Match idRef="Keyword_british_columbia_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_manitoba_drivers_license_number" />
        <Match idRef="Keyword_manitoba_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_new_brunswick_drivers_license_number" />
        <Match idRef="Keyword_new_brunswick_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_newfoundland_labrador_drivers_license_number" />
        <Match idRef="Keyword_newfoundland_labrador_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_nova_scotia_drivers_license_number" />
        <Match idRef="Keyword_nova_scotia_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_ontario_drivers_license_number" />
        <Match idRef="Keyword_ontario_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_prince_edward_island_drivers_license_number" />
        <Match idRef="Keyword_prince_edward_island_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_quebec_drivers_license_number" />
        <Match idRef="Keyword_quebec_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
      <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_saskatchewan_drivers_license_number" />
        <Match idRef="Keyword_saskatchewan_drivers_license_name" />
        <Match idRef="Keyword_canada_drivers_license" />
      </Pattern>
    </Entity>
```

 **Keywords**:

|**Keyword_[province_name]_drivers_license_name**|**Keyword_canada_drivers_license**|
|:-----|:-----|
|The province abbreviation, for example AB  <br/> The province name, for example Alberta|DL  <br/> DLS  <br/> CDL  <br/> CDLS  <br/> DriverLic  <br/> DriverLics  <br/> DriverLicense  <br/> DriverLicenses  <br/> DriverLicence  <br/> DriverLicences  <br/> Driver Lic  <br/> Driver Lics  <br/> Driver License  <br/> Driver Licenses  <br/> Driver Licence  <br/> Driver Licences  <br/> DriversLic  <br/> DriversLics  <br/> DriversLicence  <br/> DriversLicences  <br/> DriversLicense  <br/> DriversLicenses  <br/> Drivers Lic  <br/> Drivers Lics  <br/> Drivers License  <br/> Drivers Licenses  <br/> Drivers Licence  <br/> Drivers Licences  <br/> Driver'Lic  <br/> Driver'Lics  <br/> Driver'License  <br/> Driver'Licenses  <br/> Driver'Licence  <br/> Driver'Licences  <br/> Driver' Lic  <br/> Driver' Lics  <br/> Driver' License  <br/> Driver' Licenses  <br/> Driver' Licence  <br/> Driver' Licences  <br/> Driver'sLic  <br/> Driver'sLics  <br/> Driver'sLicense  <br/> Driver'sLicenses  <br/> Driver'sLicence  <br/> Driver'sLicences  <br/> Driver's Lic  <br/> Driver's Lics  <br/> Driver's License  <br/> Driver's Licenses  <br/> Driver's Licence  <br/> Driver's Licences  <br/> Permis de Conduire  <br/> id  <br/> ids  <br/> idcard number  <br/> idcard numbers  <br/> idcard #  <br/> idcard #s  <br/> idcard card  <br/> idcard cards  <br/> idcard  <br/> identification number  <br/> identification numbers  <br/> identification #  <br/> identification #s  <br/> identification card  <br/> identification cards  <br/> identification  <br/> DL#  <br/> DLS#  <br/> CDL#  <br/> CDLS#  <br/> DriverLic#  <br/> DriverLics#  <br/> DriverLicense#  <br/> DriverLicenses#  <br/> DriverLicence#  <br/> DriverLicences#  <br/> Driver Lic#  <br/> Driver Lics#  <br/> Driver License#  <br/> Driver Licenses#  <br/> Driver License#  <br/> Driver Licences#  <br/> DriversLic#  <br/> DriversLics#  <br/> DriversLicense#  <br/> DriversLicenses#  <br/> DriversLicence#  <br/> DriversLicences#  <br/> Drivers Lic#  <br/> Drivers Lics#  <br/> Drivers License#  <br/> Drivers Licenses#  <br/> Drivers Licence#  <br/> Drivers Licences#  <br/> Driver'Lic#  <br/> Driver'Lics#  <br/> Driver'License#  <br/> Driver'Licenses#  <br/> Driver'Licence#  <br/> Driver'Licences#  <br/> Driver' Lic#  <br/> Driver' Lics#  <br/> Driver' License#  <br/> Driver' Licenses#  <br/> Driver' Licence#  <br/> Driver' Licences#  <br/> Driver'sLic#  <br/> Driver'sLics#  <br/> Driver'sLicense#  <br/> Driver'sLicenses#  <br/> Driver'sLicence#  <br/> Driver'sLicences#  <br/> Driver's Lic#  <br/> Driver's Lics#  <br/> Driver's License#  <br/> Driver's Licenses#  <br/> Driver's Licence#  <br/> Driver's Licences#  <br/> Permis de Conduire#  <br/> id#  <br/> ids#  <br/> idcard card#  <br/> idcard cards#  <br/> idcard#  <br/> identification card#  <br/> identification cards#  <br/> identification#|

## Canada Health Service Number

 **Format**: 10 digits

 **Pattern**: 10 digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_canada_health_service_number` finds content that matches the pattern.

- A keyword from `Keyword_canada_health_service_number` is found.

```
<!-- Canada Health Service Number -->
<Entity id="59c0bf39-7fab-482c-af25-00faa4384c94" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_canada_health_service_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_canada_health_service_number" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_canada_health_service_number**|
|:-----|
|personal health number  <br/> patient information  <br/> health services  <br/> speciality services  <br/> automobile accident  <br/> patient hospital  <br/> psychiatrist  <br/> workers compensation  <br/> disability|

## Canada Passport Number

 **Format**: Two uppercase letters followed by six digits

 **Pattern**: Two uppercase letters followed by six digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_canada_passport_number` finds content that matches the pattern.

- A keyword from `Keyword_canada_passport_number` or `Keyword_passport` is found.

```
<!-- Canada Passport Number -->
<Entity id="14d0db8b-498a-43ed-9fca-f6097ae687eb" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_canada_passport_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_canada_passport_number" />
          <Match idRef="Keyword_passport" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_canada_passport_number**|**Keyword_passport**|
|:-----|:-----|
|canadian citizenship  <br/> canadian passport  <br/> passport application  <br/> passport photos  <br/> certified translator  <br/> canadian citizens  <br/> processing times  <br/> renewal application|Passport Number  <br/> Passport No  <br/> Passport #  <br/> Passport#  <br/> PassportID  <br/> Passportno  <br/> passportnumber  <br/> パスポート  <br/> パスポート番号  <br/> パスポートのNum  <br/> パスポート＃  <br/> Numéro de passeport  <br/> Passeport n °  <br/> Passeport Non  <br/> Passeport #  <br/> Passeport#  <br/> PasseportNon  <br/> Passeportn °|

## Canada Personal Health Identification Number (PHIN)

 **Format**: Nine digits

 **Pattern**: Nine digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_canada_phin` finds content that matches the pattern.

- At least two keywords from `Keyword_canada_phin` or `Keyword_canada_provinces` are found..

```
<!-- Canada PHIN -->
<Entity id="722e12ac-c89a-4ec8-a1b7-fea3469f89db" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_canada_phin" />
        <Any minMatches="2">
          <Match idRef="Keyword_canada_phin" />
          <Match idRef="Keyword_canada_provinces" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_canada_phin**|**Keyword_canada_provinces**|
|:-----|:-----|
|social insurance number  <br/> health information act  <br/> income tax information  <br/> manitoba health  <br/> health registration  <br/> prescription purchases  <br/> benefit eligibility  <br/> personal health  <br/> power of attorney  <br/> registration number  <br/> personal health number  <br/> practitioner referral  <br/> wellness professional  <br/> patient referral  <br/> health and wellness|Nunavut  <br/> Quebec  <br/> Northwest Territories  <br/> Ontario  <br/> British Columbia  <br/> Alberta  <br/> Saskatchewan  <br/> Manitoba  <br/> Yukon  <br/> Newfoundland and Labrador  <br/> New Brunswick  <br/> Nova Scotia  <br/> Prince Edward Island  <br/> Canada|

## Canada Social Insurance Number

 **Format**: Nine digits with optional hyphens or spaces

 **Pattern**:

Formatted:

- Three digits

- A hyphen or space

- Three digits

- A hyphen or space

- Three digits

Unformatted: Nine digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_canadian_sin` finds content that matches the pattern.

- At least two of any combination of the following:

  - A keyword from `Keyword_sin` is found.

  - A keyword from `Keyword_sin_collaborative` is found.

  - The function `Func_eu_date` finds a date in the right date format.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_unformatted_canadian_sin` finds content that matches the pattern.

- A keyword from `Keyword_sin` is found.

- The checksum passes.

```
<!-- Canada Social Insurance Number -->
<Entity id="a2f29c85-ecb8-4514-a610-364790c0773e" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_canadian_sin" />
        <Any minMatches="2">
          <Match idRef="Keyword_sin" />
          <Match idRef="Keyword_sin_collaborative" />
          <Match idRef="Func_eu_date" />
        </Any>
  </Pattern>
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_unformatted_canadian_sin" />
        <Match idRef="Keyword_sin" />
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_sin**|**Keyword_sin_collaborative**|
|:-----|:-----|
|sin  <br/> social insurance  <br/> numero d'assurance sociale  <br/> sins  <br/> ssn  <br/> ssns  <br/> social security  <br/> numero d'assurance social  <br/> national identification number  <br/> national id  <br/> sin#  <br/> soc ins  <br/> social ins|driver's license  <br/> drivers license  <br/> driver's licence  <br/> drivers licence  <br/> DOB  <br/> Birthdate  <br/> Birthday  <br/> Date of Birth|

## Chile Identity Card Number

 **Format**: 7-8 digits plus delimiters a check digit or letter

 **Pattern**: 7-8 digits plus delimiters:

- 1-2 digits

- A period

- Three digits

- A period

- Three digits

- A dash

- One digit or letter (not case sensitive) which is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_chile_id_card` finds content that matches the pattern.

- A keyword from `Keyword_chile_id_card` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_chile_id_card` finds content that matches the pattern.

- The checksum passes.

```
<!-- Chile Identity Card Number -->
<Entity id="4e979794-49a0-407e-a0b9-2c536937b925" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_chile_id_card"/>
     <Match idRef="Keyword_chile_id_card"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_chile_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_chile_id_card**|
|:-----|
|National Identification Number  <br/> Identity card  <br/> ID  <br/> Identification  <br/> Rol Único Nacional  <br/> RUN  <br/> Rol Único Tributario  <br/> RUT  <br/> Cédula de Identidad  <br/> Número De Identificación Nacional  <br/> Tarjeta de identificación  <br/> Identificación|

## China Resident Identity Card (PRC) Number

 **Format**: 18 digits

 **Pattern**: 18 digits:

- Six digits which are an address code

- Eight digits in the form YYYYMMDD which are the date of birth

- Three digits which are an order code

- One digit which is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_china_resident_id` finds content that matches the pattern.

- A keyword from `Keyword_china_resident_id` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_china_resident_id` finds content that matches the pattern.

- The checksum passes.

```
<!-- China Resident Identity Card (PRC) Number -->
<Entity id="c92daa86-2d16-4871-901f-816b3f554fc1" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_china_resident_id"/>
     <Match idRef="Keyword_china_resident_id"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_china_resident_id"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_china_resident_id**|
|:-----|
|Resident Identity Card  <br/> PRC  <br/> National Identification Card  <br/> 身份证  <br/> 居民 身份证  <br/> 居民身份证  <br/> 鉴定  <br/> 身分證  <br/> 居民 身份證  <br/> 鑑定|

## Credit Card Number

 **Format**: 14 digits which can be formatted or unformatted (dddddddddddddd) and must pass the Luhn test.

 **Pattern**: Very complex and robust pattern that detects cards from all major brands worldwide, including Visa, MasterCard, Discover Card, JCB, American Express, gift cards, and diner cards.

 **Checksum**: Yes, the Luhn checksum

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_credit_card` finds content that matches the pattern.

- One of the following is true:

  - A keyword from `Keyword_cc_verification` is found.

  - A keyword from `Keyword_cc_name` is found.

  - The function `Func_expiration_date` finds a date in the right date format.

- The checksum passes.

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_credit_card` finds content that matches the pattern.

- The checksum passes.

```
<!-- Credit Card Number -->
<Entity id="50842eb7-edc8-4019-85dd-5a5c1f2bb085" patternsProximity="300" recommendedConfidence="85">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_credit_card" />
        <Any minMatches="1">
          <Match idRef="Keyword_cc_verification" />
          <Match idRef="Keyword_cc_name" />
          <Match idRef="Func_expiration_date" />
        </Any>
  </Pattern>
  <Pattern confidenceLevel="65">
        <IdMatch idRef="Func_credit_card" />
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_cc_verification**|**Keyword_cc_name**|
|:-----|:-----|
|card verification  <br/> card identification number  <br/> cvn  <br/> cid  <br/> cvc2  <br/> cvv2  <br/> pin block  <br/> security code  <br/> security number  <br/> security no  <br/> issue number  <br/> issue no  <br/> cryptogramme  <br/> numéro de sécurité  <br/> numero de securite  <br/> kreditkartenprüfnummer  <br/> kreditkartenprufnummer  <br/> prüfziffer  <br/> prufziffer  <br/> sicherheits Kode  <br/> sicherheitscode  <br/> sicherheitsnummer  <br/> verfalldatum  <br/> codice di verifica  <br/> cod. sicurezza  <br/> cod sicurezza  <br/> n autorizzazione  <br/> código  <br/> codigo  <br/> cod. seg  <br/> cod seg  <br/> código de segurança  <br/> codigo de seguranca  <br/> codigo de segurança  <br/> código de seguranca  <br/> cód. segurança  <br/> cod. seguranca cod. segurança  <br/> cód. seguranca  <br/> cód segurança  <br/> cod seguranca cod segurança  <br/> cód seguranca  <br/> número de verificação  <br/> numero de verificacao  <br/> ablauf  <br/> gültig bis  <br/> gültigkeitsdatum  <br/> gultig bis  <br/> gultigkeitsdatum  <br/> scadenza  <br/> data scad  <br/> fecha de expiracion  <br/> fecha de venc  <br/> vencimiento  <br/> válido hasta  <br/> valido hasta  <br/> vto  <br/> data de expiração  <br/> data de expiracao  <br/> data em que expira  <br/> validade  <br/> valor  <br/> vencimento  <br/> Venc|amex  <br/> american express  <br/> americanexpress  <br/> Visa  <br/> mastercard  <br/> master card  <br/> mc  <br/> mastercards  <br/> master cards  <br/> diner's Club  <br/> diners club  <br/> dinersclub  <br/> discover card  <br/> discovercard  <br/> discover cards  <br/> JCB  <br/> japanese card bureau  <br/> carte blanche  <br/> carteblanche  <br/> credit card  <br/> cc#  <br/> cc#:  <br/> expiration date  <br/> exp date  <br/> expiry date  <br/> date d'expiration  <br/> date d'exp  <br/> date expiration  <br/> bank card  <br/> bankcard  <br/> card number  <br/> card num  <br/> cardnumber  <br/> cardnumbers  <br/> card numbers  <br/> creditcard  <br/> credit cards  <br/> creditcards  <br/> ccn  <br/> card holder  <br/> cardholder  <br/> card holders  <br/> cardholders  <br/> check card  <br/> checkcard  <br/> check cards  <br/> checkcards  <br/> debit card  <br/> debitcard  <br/> debit cards  <br/> debitcards  <br/> atm card  <br/> atmcard  <br/> atm cards  <br/> atmcards  <br/> enroute  <br/> en route  <br/> card type  <br/> carte bancaire  <br/> carte de crédit  <br/> carte de credit  <br/> numéro de carte  <br/> numero de carte  <br/> nº de la carte  <br/> nº de carte  <br/> kreditkarte  <br/> karte  <br/> karteninhaber  <br/> karteninhabers  <br/> kreditkarteninhaber  <br/> kreditkarteninstitut  <br/> kreditkartentyp  <br/> eigentümername  <br/> kartennr  <br/> kartennummer  <br/> kreditkartennummer  <br/> kreditkarten-nummer  <br/> carta di credito  <br/> carta credito  <br/> n. carta  <br/> n carta  <br/> nr. carta  <br/> nr carta  <br/> numero carta  <br/> numero della carta  <br/> numero di carta  <br/> tarjeta credito  <br/> tarjeta de credito  <br/> tarjeta crédito  <br/> tarjeta de crédito  <br/> tarjeta de atm  <br/> tarjeta atm  <br/> tarjeta debito  <br/> tarjeta de debito  <br/> tarjeta débito  <br/> tarjeta de débito  <br/> nº de tarjeta  <br/> no. de tarjeta  <br/> no de tarjeta  <br/> numero de tarjeta  <br/> número de tarjeta  <br/> tarjeta no  <br/> tarjetahabiente  <br/> cartão de crédito  <br/> cartão de credito  <br/> cartao de crédito  <br/> cartao de credito  <br/> cartão de débito  <br/> cartao de débito  <br/> cartão de debito  <br/> cartao de debito  <br/> débito automático  <br/> debito automatico  <br/> número do cartão  <br/> numero do cartão  <br/> número do cartao  <br/> numero do cartao  <br/> número de cartão  <br/> numero de cartão  <br/> número de cartao  <br/> numero de cartao  <br/> nº do cartão  <br/> nº do cartao  <br/> nº. do cartão  <br/> no do cartão  <br/> no do cartao  <br/> no. do cartão  <br/> no. do cartao|

## Croatia Identity Card Number

 **Format**: Nine digits

 **Pattern**: Nine consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_croatia_id_card` finds content that matches the pattern.

- A keyword from `Keyword_croatia_id_card` is found.

```
<!--Croatia Identity Card Number-->
<Entity id="ff12f884-c20a-4189-b185-34c8e7258d47" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_croatia_id_card"/>
     <Match idRef="Keyword_croatia_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_croatia_id_card**|
|:-----|
|Croatian identity card  <br/> Osobna iskaznica|

## Croatia Personal Identification (OIB) Number

 **Format**: 10 digits

 **Pattern**: 10 digits:

- Six digits in the form DDMMYY which are the date of birth

- Four digits where the final digit is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_croatia_oib_number` finds content that matches the pattern.

- A keyword from `Keyword_croatia_oib_number` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_croatia_oib_number` finds content that matches the pattern.

- The checksum passes.

```
<!-- Croatia Personal Identification (OIB) Number -->
<Entity id="31983b6d-db95-4eb2-a630-b44bd091968d" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_croatia_oib_number"/>
     <Match idRef="Keyword_croatia_oib_number"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_croatia_oib_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_croatia_oib_number**|
|:-----|
|Personal Identification Number  <br/> Osobni identifikacijski broj  <br/> OIB|

## Czech National Identity Card Number

 **Format**: 10 digits containing a forward slash

 **Pattern**: 10 digits:

- Six digits which are the date of birth

- A forward slash

- Four digits where the final digit is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_czech_id_card` finds content that matches the pattern.

- A keyword from `Keyword_czech_id_card` is found.

- The checksum passes.

```
<!-- Czech National Identity Card Number -->
<Entity id="60c0725a-4eb6-455b-9dda-05d8a7396497" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_czech_id_card"/>
     <Match idRef="Keyword_czech_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_czech_id_card**|
|:-----|
|Czech national identity card  <br/> Občanský průka|

## Denmark Personal Identification Number

 **Format**: 10 digits containing a hyphen

 **Pattern**: 10 digits:

- Six digits in the format DDMMYY which are the date of birth

- A hyphen

- Four digits where the final digit is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_denmark_id` finds content that matches the pattern.

- A keyword from `Keyword_denmark_id` is found.

- The checksum passes.

```
<!-- Denmark Personal Identification Number -->
<Entity id="6c4f2fef-56e1-4c00-8093-88d7a01cf460" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_denmark_id"/>
     <Match idRef="Keyword_denmark_id"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_denmark_id**|
|:-----|
|Personal Identification Number  <br/> CPR  <br/> Det Centrale Personregister  <br/> Personnummer|

## Drug Enforcement Agency (DEA) Number

 **Format**: Two letters followed by seven digits

 **Pattern**: Pattern must include all of the following:

- One letter (not case sensitive) from this set of possible letters: abcdefghjklmnprstux, which is a registrant code

- One letter (not case sensitive), which is the first letter of the registrant's last name

- Seven digits, the last of which is the check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_dea_number` finds content that matches the pattern.

- The checksum passes.

```
<!-- DEA Number -->
<Entity id="9a5445ad-406e-43eb-8bd7-cac17ab6d0e4" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_dea_number"/>
  </Pattern>
</Entity>
```

 **Keywords**: None

## EU Debit Card Number

 **Format**: 16 digits

 **Pattern**: Very complex and robust pattern

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_eu_debit_card` finds content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_eu_debit_card` is found.

  - A keyword from `Keyword_card_terms_dict` is found.

  - A keyword from `Keyword_card_security_terms_dict` is found.

  - A keyword from `Keyword_card_expiration_terms_dict` is found.

  - The function `Func_eu_date1` finds a date in the right date format.

  - The function `Func_eu_date2` finds a date in the right date format.

- The checksum passes.

```
<!-- EU Debit Card Number -->
<Entity id="0e9b3178-9678-47dd-a509-37222ca96b42" patternsProximity="300" recommendedConfidence="85">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_eu_debit_card" />
        <Any minMatches="1">
          <Match idRef="Keyword_eu_debit_card" />
          <Match idRef="Keyword_card_terms_dict" />
          <Match idRef="Keyword_card_security_terms_dict" />
          <Match idRef="Keyword_card_expiration_terms_dict" />
          <Match idRef="Func_expiration_date" />
          <Match idRef="Func_eu_date" />
          <Match idRef="Func_eu_date1" />
          <Match idRef="Func_eu_date2" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_eu_debit_card**|**Keyword_card_terms_dict**|**Keyword_card_security_terms_dict**|**Keyword_card_expiration_terms_dict**|
|:-----|:-----|:-----|:-----|
|account number  <br/> card number  <br/> card no.  <br/> security number  <br/> cc#|acct nbr  <br/> acct num  <br/> acct no  <br/> american express  <br/> americanexpress  <br/> americano espresso  <br/> amex  <br/> atm card  <br/> atm cards  <br/> atm kaart  <br/> atmcard  <br/> atmcards  <br/> atmkaart  <br/> atmkaarten  <br/> bancontact  <br/> bank card  <br/> bankkaart  <br/> card holder  <br/> card holders  <br/> card num  <br/> card number  <br/> card numbers  <br/> card type  <br/> cardano numerico  <br/> cardholder  <br/> cardholders  <br/> cardnumber  <br/> cardnumbers  <br/> carta bianca  <br/> carta credito  <br/> carta di credito  <br/> cartao de credito  <br/> cartao de crédito  <br/> cartao de debito  <br/> cartao de débito  <br/> carte bancaire  <br/> carte blanche  <br/> carte bleue  <br/> carte de credit  <br/> carte de crédit  <br/> carte di credito  <br/> carteblanche  <br/> cartão de credito  <br/> cartão de crédito  <br/> cartão de debito  <br/> cartão de débito  <br/> cb  <br/> ccn  <br/> check card  <br/> check cards  <br/> checkcard  <br/> checkcards  <br/> chequekaart  <br/> cirrus  <br/> cirrus-edc-maestro  <br/> controlekaart  <br/> controlekaarten  <br/> credit card  <br/> credit cards  <br/> creditcard  <br/> creditcards  <br/> debetkaart  <br/> debetkaarten  <br/> debit card  <br/> debit cards  <br/> debitcard  <br/> debitcards  <br/> debito automatico  <br/> diners club  <br/> dinersclub  <br/> discover  <br/> discover card  <br/> discover cards  <br/> discovercard  <br/> discovercards  <br/> débito automático  <br/> edc  <br/> eigentümername  <br/> european debit card  <br/> hoofdkaart  <br/> hoofdkaarten  <br/> in viaggio  <br/> japanese card bureau  <br/> japanse kaartdienst  <br/> jcb  <br/> kaart  <br/> kaart num  <br/> kaartaantal  <br/> kaartaantallen  <br/> kaarthouder  <br/> kaarthouders  <br/> karte  <br/> karteninhaber  <br/> karteninhabers  <br/> kartennr  <br/> kartennummer  <br/> kreditkarte  <br/> kreditkarten-nummer  <br/> kreditkarteninhaber  <br/> kreditkarteninstitut  <br/> kreditkartennummer  <br/> kreditkartentyp  <br/> maestro  <br/> master card  <br/> master cards  <br/> mastercard  <br/> mastercards  <br/> mc  <br/> mister cash  <br/> n carta  <br/> n. carta  <br/> no de tarjeta  <br/> no do cartao  <br/> no do cartão  <br/> no. de tarjeta  <br/> no. do cartao  <br/> no. do cartão  <br/> nr carta  <br/> nr. carta  <br/> numeri di scheda  <br/> numero carta  <br/> numero de cartao  <br/> numero de carte  <br/> numero de cartão  <br/> numero de tarjeta  <br/> numero della carta  <br/> numero di carta  <br/> numero di scheda  <br/> numero do cartao  <br/> numero do cartão  <br/> numéro de carte  <br/> nº carta  <br/> nº de carte  <br/> nº de la carte  <br/> nº de tarjeta  <br/> nº do cartao  <br/> nº do cartão  <br/> nº. do cartão  <br/> número de cartao  <br/> número de cartão  <br/> número de tarjeta  <br/> número do cartao  <br/> scheda dell'assegno  <br/> scheda dell'atmosfera  <br/> scheda dell'atmosfera  <br/> scheda della banca  <br/> scheda di controllo  <br/> scheda di debito  <br/> scheda matrice  <br/> schede dell'atmosfera  <br/> schede di controllo  <br/> schede di debito  <br/> schede matrici  <br/> scoprono la scheda  <br/> scoprono le schede  <br/> solo  <br/> supporti di scheda  <br/> supporto di scheda  <br/> switch  <br/> tarjeta atm  <br/> tarjeta credito  <br/> tarjeta de atm  <br/> tarjeta de credito  <br/> tarjeta de debito  <br/> tarjeta debito  <br/> tarjeta no  <br/> tarjetahabiente  <br/> tipo della scheda  <br/> ufficio giapponese della  <br/> scheda  <br/> v pay  <br/> v-pay  <br/> visa  <br/> visa plus  <br/> visa electron  <br/> visto  <br/> visum  <br/> vpay|card identification number  <br/> card verification  <br/> cardi la verifica  <br/> cid  <br/> cod seg  <br/> cod seguranca  <br/> cod segurança  <br/> cod sicurezza  <br/> cod. seg  <br/> cod. seguranca  <br/> cod. segurança  <br/> cod. sicurezza  <br/> codice di sicurezza  <br/> codice di verifica  <br/> codigo  <br/> codigo de seguranca  <br/> codigo de segurança  <br/> crittogramma  <br/> cryptogram  <br/> cryptogramme  <br/> cv2  <br/> cvc  <br/> cvc2  <br/> cvn  <br/> cvv  <br/> cvv2  <br/> cód seguranca  <br/> cód segurança  <br/> cód. seguranca  <br/> cód. segurança  <br/> código  <br/> código de seguranca  <br/> código de segurança  <br/> de kaart controle  <br/> geeft nr uit  <br/> issue no  <br/> issue number  <br/> kaartidentificatienummer  <br/> kreditkartenprufnummer  <br/> kreditkartenprüfnummer  <br/> kwestieaantal  <br/> no. dell'edizione  <br/> no. di sicurezza  <br/> numero de securite  <br/> numero de verificacao  <br/> numero dell'edizione  <br/> numero di identificazione della  <br/> scheda  <br/> numero di sicurezza  <br/> numero van veiligheid  <br/> numéro de sécurité  <br/> nº autorizzazione  <br/> número de verificação  <br/> perno il blocco  <br/> pin block  <br/> prufziffer  <br/> prüfziffer  <br/> security code  <br/> security no  <br/> security number  <br/> sicherheits kode  <br/> sicherheitscode  <br/> sicherheitsnummer  <br/> speldblok  <br/> veiligheid nr  <br/> veiligheidsaantal  <br/> veiligheidscode  <br/> veiligheidsnummer  <br/> verfalldatum|ablauf  <br/> data de expiracao  <br/> data de expiração  <br/> data del exp  <br/> data di exp  <br/> data di scadenza  <br/> data em que expira  <br/> data scad  <br/> data scadenza  <br/> date de validité  <br/> datum afloop  <br/> datum van exp  <br/> de afloop  <br/> espira  <br/> espira  <br/> exp date  <br/> exp datum  <br/> expiration  <br/> expire  <br/> expires  <br/> expiry  <br/> fecha de expiracion  <br/> fecha de venc  <br/> gultig bis  <br/> gultigkeitsdatum  <br/> gültig bis  <br/> gültigkeitsdatum  <br/> la scadenza  <br/> scadenza  <br/> valable  <br/> validade  <br/> valido hasta  <br/> valor  <br/> venc  <br/> vencimento  <br/> vencimiento  <br/> verloopt  <br/> vervaldag  <br/> vervaldatum  <br/> vto  <br/> válido hasta|

## Finland National ID

 **Format**: Six digits plus a character indicating a century plus three digits plus a check digit

 **Pattern**: Pattern must include all of the following:

- Six digits in the format DDMMYY which are a date of birth

- Century marker (either '-', '+' or 'a')

- Three-digit personal identification number

- A digit or letter (case insensitive) which is a check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_finnish_national_id` finds content that matches the pattern.

- A keyword from `Keyword_finnish_national_id` is found.

- The checksum passes.

```
<!-- Finnish National ID-->
<Entity id="338FD995-4CB5-4F87-AD35-79BD1DD926C1" patternsProximity="300" recommendedConfidence="85">
  <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_finnish_national_id" />
          <Match idRef="Keyword_finnish_national_id" />
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_finnish_national_id**|
|:-----|
|Sosiaaliturvatunnus  <br/> SOTU Henkilötunnus HETU  <br/> Personbeteckning  <br/> Personnummer|

## Finland Passport Number

 **Format**: Combination of nine letters and digits

 **Pattern**: Combination of nine letters and digits:

- Two letters (not case sensitive)

- Seven digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_finland_passport_number` finds content that matches the pattern.

- A keyword from `Keyword_finland_passport_number` is found.

```
<!-- Finland Passport Number -->
<Entity id="d1685ac3-1d3a-40f8-8198-32ef5669c7a5" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_finland_passport_number"/>
     <Match idRef="Keyword_finland_passport_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_finland_passport_number**|
|:-----|
|Passport  <br/> Passi|

## France Driver's License Number

 **Format**: 12 digits

 **Pattern**: 12 digits with validation to discount similar patterns such as French telephone numbers

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters::

- The function `Func_french_drivers_license` finds content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_french_drivers_license` is found.

  - The function `Func_eu_date` finds a date in the right date format.

```
<!-- France Driver's License Number -->
<Entity id="18e55a36-a01b-4b0f-943d-dc10282a1824" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_french_drivers_license" />
        <Any minMatches="1">
          <Match idRef="Keyword_french_drivers_license" />
          <Match idRef="Func_eu_date" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_french_drivers_license**|
|:-----|
|drivers licence  <br/> drivers license  <br/> driving licence  <br/> driving license  <br/> permis de conduire  <br/> licence number  <br/> license number  <br/> licence numbers  <br/> license numbers|

## France National ID Card (CNI)

 **Format**: 12 digits

 **Pattern**: 12 digits

 **Checksum**: No

 **Definition**:

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters: The regular expression `Regex_france_cni` finds content that matches the pattern.

```
<!-- France CNI -->
<Entity id="f741ac74-1bc0-4665-b69b-f0c7f927c0c4" patternsProximity="300" recommendedConfidence="65">
  <Pattern confidenceLevel="65">
        <IdMatch idRef="Regex_france_cni" />
  </Pattern>
</Entity>
```

 **Keywords**: None

## France Passport Number

 **Format**: Nine digits and letters

 **Pattern**: Nine digits and letters:

- Two digits

- Two letters (not case sensitive)

- Five digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_fr_passport` finds content that matches the pattern.

- A keyword from `Keyword_passport` is found..

```
<!-- France Passport Number -->
<Entity id="3008b884-8c8c-4cd8-a289-99f34fc7ff5d" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_fr_passport" />
        <Match idRef="Keyword_passport" />
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_passport**|
|:-----|
|Passport Number  <br/> Passport No  <br/> Passport #  <br/> Passport#  <br/> PassportID  <br/> Passportno  <br/> passportnumber  <br/> パスポート  <br/> パスポート番号  <br/> パスポートのNum  <br/> パスポート ＃  <br/> Numéro de passeport  <br/> Passeport n °  <br/> Passeport Non  <br/> Passeport #  <br/> Passeport#  <br/> PasseportNon  <br/> Passeportn °|

## France Social Security Number (INSEE)

 **Format**: 15 digits

 **Pattern**:

Must match one of two patterns:

- 13 digits followed by a space followed by two digits, or

- 15 consecutive digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 95% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_french_insee` or `Func_fr_insee` finds content that matches the pattern.

- A keyword from `Keyword_fr_insee` is found.

- The checksum passes.

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_french_insee` or `Func_fr_insee` finds content that matches the pattern.

- No keyword from `Keyword_fr_insee` is found.

- The checksum passes.

```
<!-- France INSEE -->
<Entity id="71f62b97-efe0-4aa1-aa49-e14de253619d" patternsProximity="300" recommendedConfidence="85">
  <Pattern confidenceLevel="95">
        <IdMatch idRef="Func_french_insee" />
        <Match idRef="Func_fr_insee" />
        <Any minMatches="1">
          <Match idRef="Keyword_fr_insee" />
        </Any>
  </Pattern>
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_french_insee" />
        <Match idRef="Func_fr_insee" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_fr_insee" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_fr_insee**|
|:-----|
|insee  <br/> securité sociale  <br/> securite sociale  <br/> national id  <br/> national identification  <br/> numéro d'identité  <br/> no d'identité  <br/> no. d'identité  <br/> numero d'identite  <br/> no d'identite  <br/> no. d'identite  <br/> social security number  <br/> social security code  <br/> social insurance number  <br/> le numéro d'identification nationale  <br/> d'identité nationale  <br/> numéro de sécurité sociale  <br/> le code de la sécurité sociale  <br/> numéro d'assurance sociale  <br/> numéro de sécu  <br/> code sécu|

## German Driver's License Number

 **Format**: Combination of 11 digits and letters

 **Pattern**: 11 digits and letters (not case sensitive):

- A digit or letter

- Two digits

- Six digits or letters

- A digit

- A digit or letter

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_german_drivers_license` finds content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_german_drivers_license_number` is found.

  - A keyword from `Keyword_german_drivers_license_collaborative` is found.

  - A keyword from `Keyword_german_drivers_license` is found.

- The checksum passes.

```
<!-- German Driver's License Number -->
<Entity id="91da9335-1edb-45b7-a95f-5fe41a16c63c" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_german_drivers_license" />
        <Any minMatches="1">
          <Match idRef="Keyword_german_drivers_license_number" />
          <Match idRef="Keyword_german_drivers_license_collaborative" />
          <Match idRef="Keyword_german_drivers_license" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_german_drivers_license_number**|**Keyword_german_drivers_license_collaborative**|**Keyword_german_drivers_license**|
|:-----|:-----|:-----|
|Führerschein  <br/> Fuhrerschein  <br/> Fuehrerschein  <br/> Führerscheinnummer  <br/> Fuhrerscheinnummer  <br/> Fuehrerscheinnummer  <br/> Führerschein-  <br/> Fuhrerschein-  <br/> Fuehrerschein-  <br/> FührerscheinnummerNr  <br/> FuhrerscheinnummerNr  <br/> FuehrerscheinnummerNr  <br/> FührerscheinnummerKlasse  <br/> FuhrerscheinnummerKlasse  <br/> FuehrerscheinnummerKlasse  <br/> Führerschein- Nr  <br/> Fuhrerschein- Nr  <br/> Fuehrerschein- Nr  <br/> Führerschein- Klasse  <br/> Fuhrerschein- Klasse  <br/> Fuehrerschein- Klasse  <br/> FührerscheinnummerNr  <br/> FuhrerscheinnummerNr  <br/> FuehrerscheinnummerNr  <br/> FührerscheinnummerKlasse  <br/> FuhrerscheinnummerKlasse  <br/> FuehrerscheinnummerKlasse  <br/> Führerschein- Nr  <br/> Fuhrerschein- Nr  <br/> Fuehrerschein- Nr  <br/> Führerschein- Klasse  <br/> Fuhrerschein- Klasse  <br/> Fuehrerschein- Klasse  <br/> DL  <br/> DLS  <br/> Driv Lic  <br/> Driv Licen  <br/> Driv License  <br/> Driv Licenses  <br/> Driv Licence  <br/> Driv Licences  <br/> Driv Lic  <br/> Driver Licen  <br/> Driver License  <br/> Driver Licenses  <br/> Driver Licence  <br/> Driver Licences  <br/> Drivers Lic  <br/> Drivers Licen  <br/> Drivers License  <br/> Drivers Licenses  <br/> Drivers Licence  <br/> Drivers Licences  <br/> Driver's Lic  <br/> Driver's Licen  <br/> Driver's License  <br/> Driver's Licenses  <br/> Driver's Licence  <br/> Driver's Licences  <br/> Driving Lic  <br/> Driving Licen  <br/> Driving License  <br/> Driving Licenses  <br/> Driving Licence  <br/> Driving Licences|Nr-Führerschein  <br/> Nr-Fuhrerschein  <br/> Nr-Fuehrerschein  <br/> No-Führerschein  <br/> No-Fuhrerschein  <br/> No-Fuehrerschein  <br/> N-Führerschein  <br/> N-Fuhrerschein  <br/> N-Fuehrerschein  <br/> Nr-Führerschein  <br/> Nr-Fuhrerschein  <br/> Nr-Fuehrerschein  <br/> No-Führerschein  <br/> No-Fuhrerschein  <br/> No-Fuehrerschein  <br/> N-Führerschein  <br/> N-Fuhrerschein  <br/> N-Fuehrerschein|ausstellungsdatum  <br/> ausstellungsort  <br/> ausstellende behöde  <br/> ausstellende behorde  <br/> ausstellende behoerde|

## German Identity Card Number

 **Format**:

- Since 1 November 2010: Nine letters and digits

- From 1 April 1987 until 31 October 2010: 10 digits

 **Pattern**:

Since 1 November 2010:

- One letter (not case sensitive)

- Eight digits

From 1 April 1987 until 31 October 2010: 10 digits

 **Checksum**: No

 **Definition**:

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_germany_id_card` finds content that matches the pattern.

- A keyword from `Keyword_germany_id_card` is found.

```
<!-- Germany Identity Card Number -->
<Entity id="e577372f-c42e-47a0-9d85-bebed1c237d4" recommendedConfidence="65" patternsProximity="300">
  <Pattern confidenceLevel="65">
     <IdMatch idRef="Regex_germany_id_card"/>
     <Match idRef="Keyword_germany_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_germany_id_card**|
|:-----|
|Identity Card  <br/> ID  <br/> Identification  <br/> Personalausweis  <br/> Identifizierungsnummer  <br/> Ausweis  <br/> Identifikation|

## German Passport Number

 **Format**: 10 digits or letters

 **Pattern**: Pattern must include all of the following:

- First character is a digit or a letter from this set (C, F, G, H, J, K)

- Three digits

- Five digits or letters from this set (C, -H, J-N, P, R, T, V-Z)

- A digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_german_passport` finds content that matches the pattern.

- A keyword from any of the five keyword lists is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_german_passport_data` finds content that matches the pattern.

- A keyword from any of the five keyword lists is found.

- The checksum passes.

```
<!-- German Passport Number -->
<Entity id="2e3da144-d42b-47ed-b123-fbf78604e52c" patternsProximity="300" recommendedConfidence="75">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_german_passport" />
        <Any minMatches="1">
          <Match idRef="Keyword_german_passport" />
          <Match idRef="Keyword_german_passport_collaborative" />
          <Match idRef="Keyword_german_passport_number" />
          <Match idRef="Keyword_german_passport1" />
          <Match idRef="Keyword_german_passport2" />
        </Any>
  </Pattern>
  <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_german_passport_data" />
        <Any minMatches="1">
          <Match idRef="Keyword_german_passport" />
          <Match idRef="Keyword_german_passport_collaborative" />
          <Match idRef="Keyword_german_passport_number" />
          <Match idRef="Keyword_german_passport1" />
          <Match idRef="Keyword_german_passport2" />
        </Any>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_german_passport**|**Keyword_german_passport_collaborative**|**Keyword_german_passport_number**|**Keyword_german_passport1**|**Keyword_german_passport2**|
|:-----|:-----|:-----|:-----|:-----|
|reisepass  <br/> reisepasse  <br/> reisepassnummer  <br/> passport  <br/> passports|geburtsdatum  <br/> ausstellungsdatum  <br/> ausstellungsort|No-Reisepass  <br/> Nr-Reisepass|Reisepass-Nr|bnationalit.t|

## Greece National ID Card

 **Format**: Combination of 7-8 letters and numbers plus a dash

 **Pattern**:

Seven letters and numbers (old format):

- One letter (any letter of the Greek alphabet)

- A dash

- Six digits

Eight letters and numbers (new format):

- Two letters whose uppercase character occurs in both the Greek and Latin alphabets (ABEZHIKMNOPTYX)

- A dash

- Six digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_greece_id_card` finds content that matches the pattern.

- A keyword from `Keyword_greece_id_card` is found.

```
<!-- Greece National ID Card -->
<Entity id="82568215-1da1-46d3-874a-d2294d81b5ac" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Regex_greece_id_card"/>
     <Match idRef="Keyword_greece_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_greece_id_card**|
|:-----|
|Greek identity Card  <br/> Tautotita  <br/> Δελτίο αστυνομικής ταυτότητας  <br/> Ταυτότητα|

## Hong Kong Identity Card (HKID) Number

 **Format**: Combination of 8-9 letters and numbers plus optional parentheses around the final character

 **Pattern**: Combination of 8-9 letters:

- 1-2 letters (not case sensitive)

- Six digits

- The final character (any digit or the letter A), which is the check digit and is optionally enclosed in parentheses.

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_hong_kong_id_card` finds content that matches the pattern.

- A keyword from `Keyword_hong_kong_id_card` is found.

- The checksum passes.

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_hong_kong_id_card` finds content that matches the pattern.

- The checksum passes.

```
<!-- Hong Kong Identity Card (HKID) number -->
<Entity id="e63c28a7-ad29-4c17-a41a-3d2a0b70fd9c" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_hong_kong_id_card"/>
     <Match idRef="Keyword_hong_kong_id_card"/>
  </Pattern>
  <Pattern confidenceLevel="65">
     <IdMatch idRef="Func_hong_kong_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_hong_kong_id_card**|
|:-----|
|Hong Kong Identity Card  <br/> HKID  <br/> ID card  <br/> 香港身份證  <br/> 香港永久性居民身份證|

## India Permanent Account Number

 **Format**: 10 letters or digits

 **Pattern**: 10 letters or digits:

- Five letters (not case sensitive)

- Four digits

- A letter which is an alphabetic check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_india_permanent_account_number` finds content that matches the pattern.

- A keyword from `Keyword_india_permanent_account_number` is found.

- The checksum passes.

```
<!-- India Permanent Account Number -->
<Entity id="2602bfee-9bb0-47a5-a7a6-2bf3053e2804" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Regex_india_permanent_account_number"/>
     <Match idRef="Keyword_india_permanent_account_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_india_permanent_account_number**|
|:-----|
|Permanent Account Number  <br/> PAN|

## India Unique Identification (Aadhaar) Number

 **Format**: 12 digits containing optional spaces or dashes

 **Pattern**: 12 digits:

- Four digits

- An optional space or dash

- Four digits

- An optional space or dash

- The final digit which is the check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_india_aadhaar` finds content that matches the pattern.

- A keyword from `Keyword_india_aadhar` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_india_aadhaar` finds content that matches the pattern.

- The checksum passes.

```
<!-- India Unique Identification (Aadhaar) number -->
<Entity id="1ca46b29-76f5-4f46-9383-cfa15e91048f" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_india_aadhaar"/>
     <Match idRef="Keyword_india_aadhar"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_india_aadhaar"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_india_aadhar**|
|:-----|
|Aadhar  <br/> Aadhaar  <br/> UID  <br/> आधार|

## Indonesia Identity Card (KTP) Number

 **Format**: 16 digits containing optional periods

 **Pattern**: 16 digits:

- Two-digit province code

- A period (optional)

- Two-digit regency or city code

- Two-digit subdistrict code

- A period (optional)

- Six digits in the format DDMMYY which are the date of birth

- A period (optional)

- Four digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_indonesia_id_card` finds content that matches the pattern.

- A keyword from `Keyword_indonesia_id_card` is found.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters: The regular expression `Regex_indonesia_id_card` finds content that matches the pattern.

```
<!-- Indonesia Identity Card (KTP) Number -->
<Entity id="da68fdb0-f383-4981-8c86-82689d3b7d55" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Regex_indonesia_id_card"/>
     <Match idRef="Keyword_indonesia_id_card"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_indonesia_id_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_indonesia_id_card**|
|:-----|
|KTP  <br/> Kartu Tanda Penduduk  <br/> Nomor Induk Kependudukan|

## International Banking Account Number (IBAN)

 **Format**: Country code (two letters) plus check digits (two digits) plus bban number (up to 30 characters)

 **Pattern**:

Pattern must include all of the following:

- Two-letter country code

- Two check digits (followed by an optional space)

- 1-7 groups of four letters or digits (can be separated by spaces)

- 1-3 letters or digits

The format for each country is slightly different. The IBAN sensitive information type covers these 60 countries: ad, ae, al, at, az, ba, be, bg, bh, ch, cr, cy, cz, de, dk, do, ee, es, fi, fo, fr, gb, ge, gi, gl, gr, hr, hu, ie, il, is, it, kw, kz, lb, li, lt, lu, lv, mc, md, me, mk, mr, mt, mu, nl, no, pl, pt, ro, rs, sa, se, si, sk, sm, tn, tr, vg

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_iban` finds content that matches the pattern.

- The checksum passes.

```
<Entity id="e7dc4711-11b7-4cb0-b88b-2c394a771f0e" patternsProximity="300" recommendedConfidence="85">
  <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_iban" />
  </Pattern>
</Entity>
```

 **Keywords**: None

## IP Address

 **Format**: IPv4 or IPv6 address

 **Pattern**:

- IPv4: Complex pattern which accounts for formatted (periods) and unformatted (no periods) versions of the IPv4 addresses.

- IPv6: Complex pattern which accounts for formatted IPv6 numbers (which include colons).

 **Checksum**: No

 **Definition**:

For IPv4, a DLP policy is 95% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_ipv4_address` finds content that matches the pattern.

- A keyword from `Keyword_ipaddress` is found.

For IPv6, a DLP policy is 95% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_ipv6_address` finds content that matches the pattern.

- No keyword from `Keyword_ipaddress` is found.

For IPv4, a DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_ipv4_address` finds content that matches the pattern.

- No keyword from `Keyword_ipaddress` is found.

For IPv6, a DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_ipv6_address` finds content that matches the pattern.

- No keyword from `Keyword_ipaddress` is found.

```
<Entity id="1daa4ad5-e2dd-4ca4-a788-54722c09efb2" patternsProximity="300" recommendedConfidence="85">
    <Pattern confidenceLevel="95">
        <IdMatch idRef="Regex_ipv4_address" />
        <Any minMatches="1">
          <Match idRef="Keyword_ipaddress" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="95">
        <IdMatch idRef="Regex_ipv6_address" />
        <Any minMatches="1">
          <Match idRef="Keyword_ipaddress" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Regex_ipv4_address" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_ipaddress" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Regex_ipv6_address" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_ipaddress" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_ipaddress**|
|:-----|
|ip address  <br/> internet protocol  <br/> IP-כתובת ה|

## Ireland Personal Public Service (PPS) Number

 **Format**:

- New format (1 Jan 2013 and later): Seven digits followed by two letters

- Old format (31 Dec 2012 and earlier): Seven digits followed by 1-2 letters

 **Pattern**:

New format (1 Jan 2013 and later)

- Seven digits

- A letter (not case sensitive) which is an alphabetic check digit

- The letter "A" or "H" (not case sensitive)

Old format (31 Dec 2012 and earlier)

- Seven digits

- 1-2 letters (not case sensitive)

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_ireland_pps` finds content that matches the pattern.

- One of the following is true:

  - A keyword from `Keyword_ireland_pps` is found.

  - The function `Func_eu_date` finds a date in the right date format.

- The checksum passes.

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_ireland_pps` finds content that matches the pattern.

- The checksum passes.

```
<!-- Ireland Personal Public Service (PPS) Number -->
<Entity id="1cdb674d-c19a-4fcf-9f4b-7f56cc87345a" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_ireland_pps"/>
     <Any minMatches="1">
  <Match idRef="Keyword_ireland_pps"/>
  <Match idRef="Func_eu_date"/>
     </Any>
  </Pattern>
  <Pattern confidenceLevel="65">
     <IdMatch idRef="Func_ireland_pps"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_ireland_pps**|
|:-----|
|Personal Public Service Number  <br/> PPS Number  <br/> PPS Num  <br/> PPS No.  <br/> PPS #  <br/> PPS#  <br/> PPSN  <br/> Public Services Card  <br/> Uimhir Phearsanta Seirbhíse Poiblí  <br/> Uimh. PSP  <br/> PSP|

## Israel Bank Account Number

 **Format**: 13 digits

 **Pattern**:

Formatted:

- Two digits

- A dash

- Three digits

- A dash

- Eight digits

Unformatted: 13 consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_israel_bank_account_number` finds content that matches the pattern.

- A keyword from `Keyword_israel_bank_account_number` is found.

```
<!-- Israel Bank Account Number -->
<Entity id="7d08b2ff-a0b9-437f-957c-aeddbf9b2b25" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_israel_bank_account_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_israel_bank_account_number" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_israel_bank_account_number**|
|:-----|
|Bank Account Number  <br/> Bank Account  <br/> Account Number  <br/> מספר חשבון בנק|

## Israel National ID

 **Format**: Nine digits

 **Pattern**: Nine consecutive digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_israeli_national_id_number` finds content that matches the pattern.

- A keyword from `Keyword_Israel_National_ID` is found.

- The checksum passes.

```
<!-- Israel National ID Number -->
<Entity id="e05881f5-1db1-418c-89aa-a3ac5c5277ee" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_israeli_national_id_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_Israel_National_ID" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_Israel_National_ID**|
|:-----|
|מספר זהות  <br/> National ID Number|

## Italy Driver's License Number

 **Format**: A combination of 10 letters and digits

 **Pattern**: A combination of 10 letters and digits:

- One letter (not case sensitive)

- The letter "A" or "V" (not case sensitive)

- Seven letters (not case sensitive), digits, or the underscore character

- One letter (not case sensitive)

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_italy_drivers_license_number` finds content that matches the pattern.

- A keyword from `Keyword_italy_drivers_license_number` is found.

```
<!-- Italy Driver's license Number -->
<Entity id="97d6244f-9157-41bd-8e0c-9d669a5c4d71" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_italy_drivers_license_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_italy_drivers_license_number" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_italy_drivers_license_number**|
|:-----|
|numero di patente di guida  <br/> patente di guida|

## Japan Bank Account Number

 **Format**: Seven or eight digits

 **Pattern**:

Bank account number: Seven or eight digits

Bank account branch code:

- Four digits

- A space or dash (optional)

- Three digits

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_bank_account` finds content that matches the pattern.

- A keyword from `Keyword_jp_bank_account` is found.

- One of the following is true:

  - The function `Func_jp_bank_account_branch_code` finds content that matches the pattern.

  - A keyword from `Keyword_jp_bank_branch_code` is found.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_bank_account` finds content that matches the pattern.

- A keyword from `Keyword_jp_bank_account` is found.

```
<!-- Japan Bank Account Number -->
<Entity id="d354f95b-96ee-4b80-80bc-4377312b55bc" patternsProximity="300" recommendedConfidence="75">
  <Version minEngineVersion="15.01.0131.000">
    <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_jp_bank_account" />
          <Match idRef="Keyword_jp_bank_account" />
          <Any minMatches="1">
            <Match idRef="Func_jp_bank_account_branch_code" />
            <Match idRef="Keyword_jp_bank_branch_code" />
          </Any>
      </Pattern>
  </Version>
     <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_jp_bank_account" />
        <Match idRef="Keyword_jp_bank_account" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_jp_bank_account**|**Keyword_jp_bank_branch_code**|
|:-----|:-----|
|Checking Account Number  <br/> Checking Account  <br/> Checking Account #  <br/> Checking Acct Number  <br/> Checking Acct #  <br/> Checking Acct No.  <br/> Checking Account No.  <br/> Bank Account Number  <br/> Bank Account  <br/> Bank Account #  <br/> Bank Acct Number  <br/> Bank Acct #  <br/> Bank Acct No.  <br/> Bank Account No.  <br/> Savings Account Number  <br/> Savings Account  <br/> Savings Account #  <br/> Savings Acct Number  <br/> Savings Acct #  <br/> Savings Acct No.  <br/> Savings Account No.  <br/> Debit Account Number  <br/> Debit Account  <br/> Debit Account #  <br/> Debit Acct Number  <br/> Debit Acct #  <br/> Debit Acct No.  <br/> Debit Account No.  <br/> 口座番号を当座預金口座の確認  <br/> ＃アカウントの確認、勘定番号の確認  <br/> ＃勘定の確認  <br/> 勘定番号の確認  <br/> 口座番号の確認  <br/> 銀行口座番号  <br/> 銀行口座  <br/> 銀行口座＃  <br/> 銀行の勘定番号  <br/> 銀行のacct＃  <br/> 銀行の勘定いいえ  <br/> 銀行口座番号  <br/> 普通預金口座番号  <br/> 預金口座  <br/> 貯蓄口座＃  <br/> 貯蓄勘定の数  <br/> 貯蓄勘定＃  <br/> 貯蓄勘定番号  <br/> 普通預金口座番号  <br/> 引き落とし口座番号  <br/> 口座番号  <br/> 口座番号＃  <br/> デビットのacct番号  <br/> デビット勘定＃  <br/> デビットACCTの番号  <br/> デビット口座番号|Otemachi|

## Japan Driver's License Number

 **Format**: 12 digits

 **Pattern**: 12 consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_drivers_license_number` finds content that matches the pattern.

- A keyword from `Keyword_jp_drivers_license_number` is found.

```
<!-- Japan Driver's License Number -->
<Entity id="c6011143-d087-451c-8313-7f6d4aed2270" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_jp_drivers_license_number" />
        <Match idRef ="Keyword_jp_drivers_license_number" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_jp_drivers_license_number**|
|:-----|
|driver license  <br/> drivers license  <br/> driver's license  <br/> drivers licenses  <br/> driver's licenses  <br/> driver licenses  <br/> dl#  <br/> dls#  <br/> lic#  <br/> lics#  <br/> 運転免許証  <br/> 運転免許  <br/> 免許証  <br/> 免許  <br/> 運転免許証番号  <br/> 運転免許番号  <br/> 免許証番号  <br/> 免許番号  <br/> 運転免許証ナンバー  <br/> 運転免許ナンバー  <br/> 免許証ナンバー  <br/> 運転免許証No.  <br/> 運転免許No.  <br/> 免許証No.  <br/> 免許No.  <br/> 運転免許証#  <br/> 運転免許#  <br/> 免許証#  <br/> 免許#|

## Japan Passport Number

 **Format**: Two letters followed by seven digits

 **Pattern**: Two letters (not case sensitive) followed by seven digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_passport` finds content that matches the pattern.

- A keyword from `Keyword_jp_passport` is found.

```
<!-- Japan Passport Number -->
<Entity id="75177310-1a09-4613-bf6d-833aae3743f8" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_jp_passport" />
        <Match idRef="Keyword_jp_passport" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_jp_passport**|
|:-----|
|パスポート  <br/> パスポート番号  <br/> パスポートのNum  <br/> パスポート＃|

## Japan Resident Registration Number

 **Format**: 11 digits

 **Pattern**: 11 consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_resident_registration_number` finds content that matches the pattern.

- A keyword from `Keyword_jp_resident_registration_number` is found.

```
<!-- Japan Resident Registration Number -->
<Entity id="01c1209b-6389-4faf-a5f8-3f7e13899652" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_jp_resident_registration_number" />
        <Match idRef ="Keyword_jp_resident_registration_number" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_jp_resident_registration_number**|
|:-----|
|Resident Registration Number  <br/> Resident Register Number  <br/> Residents Basic Registry Number  <br/> Resident Registration No.  <br/> Resident Register No.  <br/> Residents Basic Registry No.  <br/> Basic Resident Register No.  <br/> 住民登録番号、登録番号をレジデント  <br/> 住民基本登録番号、登録番号  <br/> 住民基本レジストリ番号を常駐  <br/> 登録番号を常駐住民基本台帳登録番号|

## Japan Social Insurance Number (SIN)

 **Format**: 7-12 digits

 **Pattern**: 7-12 digits:

- Four digits

- A hyphen (optional)

- Six digits

  OR

- 7-12 consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_sin` finds content that matches the pattern.

- A keyword from `Keyword_jp_sin` is found.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_jp_sin_pre_1997` finds content that matches the pattern.

- A keyword from `Keyword_jp_sin` is found.

```
<!-- Japan Social Insurance Number -->
<Entity id="c840e719-0896-45bb-84fd-1ed5c95e45ff" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_jp_sin" />
        <Match idRef="Keyword_jp_sin" />
    </Pattern>
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_jp_sin_pre_1997" />
        <Match idRef="Keyword_jp_sin" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_jp_sin**|
|:-----|
|Social Insurance No.  <br/> Social Insurance Num  <br/> Social Insurance Number  <br/> 社会保険のテンキー  <br/> 社会保険番号|

## Malaysia ID Card Number

 **Format**: 12 digits containing optional hyphens

 **Pattern**: 12 digits:

- Six digits in the format YYMMDD which are the date of birth

- A dash (optional)

- Two-letter place-of-birth code

- A dash (optional)

- Three random digits

- One-digit gender code

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_malaysia_id_card_number` finds content that matches the pattern.

- A keyword from `Keyword_malaysia_id_card_number` is found.

```
<!-- Malaysia ID Card Number -->
</Entity>
      <Entity id="7f0e921c-9677-435b-aba2-bb8f1013c749" patternsProximity="300" recommendedConfidence="85">
        <Pattern confidenceLevel="85">
            <IdMatch idRef="Regex_malaysia_id_card_number" />
            <Match idRef="Keyword_malaysia_id_card_number" />
        </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_malaysia_id_card_number**|
|:-----|
|MyKad  <br/> Identity Card  <br/> ID Card  <br/> Identification Card  <br/> Digital Application Card  <br/> Kad Akuan Diri  <br/> Kad Aplikasi Digital|

## Netherlands Citizen's Service (BSN) Number

 **Format**: 8-9 digits containing optional spaces

 **Pattern**: 8-9 digits:

- Three digits

- A space (optional)

- Three digits

- A space (optional)

- 2-3 digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_netherlands_bsn` finds content that matches the pattern.

- A keyword from `Keyword_netherlands_bsn` is found.

- The function `Func_eu_date` finds a date in the right date format.

- The checksum passes.

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_netherlands_bsn` finds content that matches the pattern.

- The checksum passes.

```
<!-- Netherlands Citizen's Service (BSN) Number -->
<Entity id="c5f54253-ef7e-44f6-a578-440ed67e946d" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_netherlands_bsn"/>
     <Match idRef="Keyword_netherlands_bsn"/>
     <Match idRef="Func_eu_date"/>
  </Pattern>
  <Pattern confidenceLevel="65">
     <IdMatch idRef="Func_netherlands_bsn"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_netherlands_bsn**|
|:-----|
|Citizen service number  <br/> BSN  <br/> Burgerservicenummer  <br/> Sofinummer  <br/> Persoonsgebonden nummer  <br/> Persoonsnummer|

## New Zealand Ministry of Health Number

 **Format**: Three letters, a space (optional), and four digits

 **Pattern**: Three letters (not case sensitive) a space (optional) four digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_new_zealand_ministry_of_health_number` finds content that matches the pattern.

- A keyword from `Keyword_nz_terms` is found.

- The checksum passes.

```
<!-- New Zealand Health Number -->
<Entity id="2b71c1c8-d14e-4430-82dc-fd1ed6bf05c7" patternsProximity="300" recommendedConfidence="85">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_new_zealand_ministry_of_health_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_nz_terms" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_nz_terms**|
|:-----|
|NHI  <br/> New Zealand  <br/> Health  <br/> treatment|

## Norway Identification Number

 **Format**: 11 digits

 **Pattern**: 11 digits:

- Six digits in the format DDMMYY which are the date of birth

- Three-digit individual number

- Two check digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_norway_id_number` finds content that matches the pattern.

- A keyword from `Keyword_norway_id_number` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_norway_id_numbe` finds content that matches the pattern.

- The checksum passes.

```
<!-- Norway Identification Number -->
<Entity id="d4c8a798-e9f2-4bd3-9652-500d24080fc3" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_norway_id_number"/>
     <Match idRef="Keyword_norway_id_number"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_norway_id_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_norway_id_number**|
|:-----|
|Personal identification number  <br/> Norwegian ID Number  <br/> ID Number  <br/> Identification  <br/> Personnummer  <br/> Fødselsnummer|

## Philippines Unified Multi-Purpose ID Number

 **Format**: 12 digits separated by hyphens

 **Pattern**: 12 digits:

- Four digits

- A hyphen

- Seven digits

- A hyphen

- One digit

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_philippines_unified_id` finds content that matches the pattern.

- A keyword from `Keyword_philippines_id` is found.

```
<!-- Philippines Unified Multi-Purpose ID number -->
<Entity id="019b39dd-8c25-4765-91a3-d9c6baf3c3b3" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_philippines_unified_id"/>
     <Match idRef="Keyword_philippines_id"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_philippines_id**|
|:-----|
|Unified Multi-Purpose ID  <br/> UMID  <br/> Identity Card  <br/> Pinag-isang Multi-Layunin ID|

## Poland Identity Card

 **Format**: Three letters and six digits

 **Pattern**: Three letters (not case sensitive) followed by six digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_polish_national_id` finds content that matches the pattern.

- A keyword from `Keyword_polish_national_id_passport_number` is found.

- The checksum passes.

```
<!-- Poland Identity Card-->
<Entity id="25E64989-ED5D-40CA-A939-6C14183BB7BF" patternsProximity="300" recommendedConfidence="85">
      <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_polish_national_id" />
          <Match idRef="Keyword_polish_national_id_passport_number" />
      </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_polish_national_id_passport_number**|
|:-----|
|Nazwa i nr dowodu tożsamości  <br/> Dowód Tożsamości  <br/> dow. os.|

## Poland National ID (PESEL)

 **Format**: 11 digits

 **Pattern**: 11 consecutive digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_pesel_identification_number` finds content that matches the pattern.

- A keyword from `Keyword_pesel_identification_number` is found.

- The checksum passes.

```
<!-- Poland National ID (PESEL) -->
<Entity id="E3AAF206-4297-412F-9E06-BA8487E22456" patternsProximity="300" recommendedConfidence="85">
      <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_pesel_identification_number" />
          <Match idRef="Keyword_pesel_identification_number" />
      </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_pesel_identification_number**|
|:-----|
|Nr PESEL  <br/> PESEL|

## Poland Passport

 **Format**: Two letters and seven digits

 **Pattern**: Two letters (not case sensitive) followed by seven digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_polish_passport_number` finds content that matches the pattern.

- A keyword from `Keyword_polish_national_id_passport_number` is found.

- The checksum passes.

```
<!-- Poland Passport Number -->
<Entity id="03937FB5-D2B6-4487-B61F-0F8BFF7C3517" patternsProximity="300" recommendedConfidence="85">
      <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_polish_passport_number" />
          <Match idRef="Keyword_polish_national_id_passport_number" />
      </Pattern>
</Entity>
</Version>
```

 **Keywords**:

|**Keyword_polish_national_id_passport_number**|
|:-----|
|Nazwa i nr dowodu tożsamości  <br/> Dowód Tożsamości  <br/> dow. os.|

## Portugal Citizen Card Number

 **Format**: Eight digits

 **Pattern**: Eight digits

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_portugal_citizen_card` finds content that matches the pattern.

- A keyword from `Keyword_portugal_citizen_card` is found.

```
<!-- Portugal Citizen Card Number -->
<Entity id="91a7ece2-add4-4986-9a15-c84544d81ecd" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Regex_portugal_citizen_card"/>
     <Match idRef="Keyword_portugal_citizen_card"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_portugal_citizen_card**|
|:-----|
|Citizen Card  <br/> National ID Card  <br/> CC  <br/> Cartão de Cidadão  <br/> Bilhete de Identidade|

## Saudi Arabia National ID

 **Format**: 10 digits

 **Pattern**: 10 consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_saudi_arabia_national_id` finds content that matches the pattern.

- A keyword from `Keyword_saudi_arabia_national_id` is found.

```
<!-- Saudi Arabia National ID -->
<Entity id="8c5a0ba8-404a-41a3-8871-746aa21ee6c0" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_saudi_arabia_national_id" />
        <Any minMatches="1">
          <Match idRef="Keyword_saudi_arabia_national_id" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_saudi_arabia_national_id**|
|:-----|
|Identification Card  <br/> I card number  <br/> ID number  <br/> الوطنية الهوية بطاقة رقم|

## Singapore National Registration Identity Card (NRIC) Number

 **Format**: Nine letters and digits

 **Pattern**: Nine letters and digits:

- The letter "F", "G", "S", or "T" (not case sensitive)

- Seven digits

- An alphabetic check digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_singapore_nric` finds content that matches the pattern.

- A keyword from `Keyword_singapore_nric` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_singapore_nric` finds content that matches the pattern.

- The checksum passes.

```
<!-- Singapore National Registration Identity Card (NRIC) Number -->
<Entity id="cead390a-dd83-4856-9751-fb6dc98c34da" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Regex_singapore_nric"/>
     <Match idRef="Keyword_singapore_nric"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_singapore_nric"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_singapore_nric**|
|:-----|
|National Registration Identity Card  <br/> Identity Card Number  <br/> NRIC  <br/> IC  <br/> Foreign Identification Number  <br/> FIN  <br/> 身份证  <br/> 身份證|

## South Africa Identification Number

 **Format**: 13 digits that may contain spaces

 **Pattern**: 13 digits:

- Six digits in the format YYMMDD which are the date of birth

- Four digits

- A single-digit citizenship indicator

- The digit "8" or "9"

- One digit which is a checksum digit

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_south_africa_identification_number` finds content that matches the pattern.

- A keyword from `Keyword_south_africa_identification_number` is found.

- The checksum passes.

```
<!-- South Africa Identification Number -->
<Entity id="e2adf7cb-8ea6-4048-a2ed-d89eb65f2780" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_south_africa_identification_number"/>
     <Match idRef="Keyword_south_africa_identification_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_south_africa_identification_number**|
|:-----|
|Identity card  <br/> ID  <br/> Identification|

## South Korea Resident Registration Number

 **Format**: 13 digits containing a hyphen

 **Pattern**: 13 digits:

- Six digits in the format YYMMDD which are the date of birth

- A hyphen

- One digit determined by the century and gender

- Four-digit region-of-birth code

- One digit used to differentiate people for whom the preceding numbers are identical

- A check digit.

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_south_korea_resident_number` finds content that matches the pattern.

- A keyword from `Keyword_south_korea_resident_number` is found.

- The checksum passes.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_south_korea_resident_number` finds content that matches the pattern.

- The checksum passes.

```
<!-- South Korea Resident Registration Number -->
<Entity id="5b802e18-ba80-44c4-bc83-bf2ad36ae36a" recommendedConfidence="85" patternsProximity="300">
  <Pattern confidenceLevel="85">
     <IdMatch idRef="Func_south_korea_resident_number"/>
     <Match idRef="Keyword_south_korea_resident_number"/>
  </Pattern>
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Func_south_korea_resident_number"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_south_korea_resident_number**|
|:-----|
|National ID card  <br/> Citizen's Registration Number  <br/> Jumin deungnok beonho  <br/> RRN  <br/> 주민등록번호|

## Spain Social Security Number (SSN)

 **Format**: 11-12 digits

 **Pattern**: 11-12 digits:

- Two digits

- A forward slash (optional)

- 7-8 digits

- A forward slash (optional)

- Two digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_spanish_social_security_number` finds content that matches the pattern.

- The checksum passes.

```
<!-- Spain SSN -->
<Entity id="5df987c0-8eae-4bce-ace7-b316347f3070" patternsProximity="300" recommendedConfidence="85">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_spanish_social_security_number" />
    </Pattern>
</Entity>
```

 **Keywords**: None

## Sweden National ID

 **Format**: 10 or 12 digits and an optional delimiter

 **Pattern**: 10 or 12 digits and an optional delimiter:

- 2-4 digits (optional)

- Six digits in date format YYMMDD

- Delimiter of "-" or "+" (optional), plus

- Four digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_swedish_national_identifier` finds content that matches the pattern.

- The checksum passes.

```
<!-- Sweden National ID -->
<Entity id="f69aaf40-79be-4fac-8f05-fd1910d272c8" patternsProximity="300" recommendedConfidence="85">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_swedish_national_identifier" />
    </Pattern>
</Entity>
```

 **Keywords**: None

## Sweden Passport Number

 **Format**: Eight digits

 **Pattern**: Eight consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_sweden_passport_number` finds content that matches the pattern.

- One of the following is true:

  - A keyword from `Keyword_passport` is found.

  - A keyword from `Keyword_sweden_passport` is found.

```
<!-- Sweden Passport Number -->
<Entity id="ba4e7456-55a9-4d89-9140-c33673553526" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_sweden_passport_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_passport" />
          <Match idRef="Keyword_sweden_passport" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_sweden_passport**|**Keyword_passport**|
|:-----|:-----|
|visa requirements  <br/> Alien Registration Card  <br/> Schengen visas  <br/> Schengen visa  <br/> Visa Processing  <br/> Visa Type  <br/> Single Entry  <br/> Multiple Entry  <br/> G3 Processing Fees|Passport Number  <br/> Passport No  <br/> Passport #  <br/> Passport#  <br/> PassportID  <br/> Passportno  <br/> passportnumber  <br/> パスポート  <br/> パスポート番号  <br/> パスポートのNum  <br/> パスポート＃  <br/> Numéro de passeport  <br/> Passeport n °  <br/> Passeport Non  <br/> Passeport #  <br/> Passeport#  <br/> PasseportNon  <br/> Passeportn °|

## SWIFT Code

 **Format**: Four letters followed by 5-31 letters or digits

 **Pattern**: Four letters followed by 5-31 letters or digits:

- Four-letter bank code (not case sensitive)

- An optional space

- 4-28 letters or digits (the Basic Bank Account Number (BBAN))

- An optional space

- 1-3 letters or digits (remainder of the BBAN)

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_swift` finds content that matches the pattern.

- A keyword from `Keyword_swift` is found.

```
<Entity id="cb2ab58c-9cb8-4c81-baf8-a4e106791df4" patternsProximity="300" recommendedConfidence="75">
<Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_swift" />
        <Match idRef="Keyword_swift" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_swift**|
|:-----|
|international organization for standardization 9362  <br/> iso 9362  <br/> iso9362  <br/> swift#  <br/> swiftcode  <br/> swiftnumber  <br/> swiftroutingnumber  <br/> swift code  <br/> swift number #  <br/> swift routing number  <br/> bic number  <br/> bic code  <br/> bic #  <br/> bic#  <br/> bank identifier code  <br/> 標準化9362  <br/> 迅速＃  <br/> SWIFTコード  <br/> SWIFT番号  <br/> 迅速なルーティング番号  <br/> BIC番号  <br/> BICコード  <br/> 銀行識別コードのための国際組織  <br/> Organisation internationale de normalisation 9362  <br/> rapide #  <br/> code SWIFT  <br/> le numéro de swift  <br/> swift numéro d'acheminement  <br/> le numéro BIC  <br/> # BIC  <br/> code identificateur de banque|

## Taiwan National ID

 **Format**: One letter (in English) followed by nine digits

 **Pattern**: One letter (in English) followed by nine digits:

- One letter (in English, not case sensitive)

- The digit "1" or "2"

- Eight digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_taiwanese_national_id` finds content that matches the pattern.

- A keyword from `Keyword_taiwanese_national_id` is found.

- The checksum passes.

```
<!-- Taiwanese National ID -->
<Entity id="4C7BFC34-8DD1-421D-8FB7-6C6182C2AF03" patternsProximity="300" recommendedConfidence="85">
      <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_taiwanese_national_id" />
          <Match idRef="Keyword_taiwanese_national_id" />
      </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_taiwanese_national_id**|
|:-----|
|身份證字號  <br/> 身份證  <br/> 身份證號碼  <br/> 身份證號  <br/> 身分證字號  <br/> 身分證  <br/> 身分證號碼  <br/> 身份證號  <br/> 身分證統一編號  <br/> 國民身分證統一編號  <br/> 簽名  <br/> 蓋章  <br/> 簽名或蓋章  <br/> 簽章|

## Taiwan Passport Number

 **Format**:

- Biometric passport number: Nine digits

- Non-biometric passport number: Nine digits

 **Pattern**:

- Biometric passport number

  - The digit "3"

  - Eight digits

- Non-biometric passport number: Nine digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_taiwan_passport` finds content that matches the pattern.

- A keyword from `Keyword_taiwan_passport` is found.

```
<!-- Taiwan Passport Number -->
<Entity id="e7251cb4-4c2c-41df-963e-924eb3dae04a" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_taiwan_passport"/>
     <Match idRef="Keyword_taiwan_passport"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_taiwan_passport**|
|:-----|
|ROC passport number  <br/> Passport number  <br/> Passport no  <br/> Passport Num  <br/> Passport #  <br/> 护照  <br/> 中華民國護照  <br/> Zhōnghuá Mínguó hùzhào|

## Taiwan Resident Certificate (ARC/TARC) Number

 **Format**: 10 letters and digits

 **Pattern**: 10 letters and digits:

- Two letters (not case sensitive)

- Eight digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_taiwan_resident_certificate` finds content that matches the pattern.

- A keyword from `Keyword_taiwan_resident_certificate` is found.

```
<!-- Taiwan Resident Certificate (ARC/TARC) -->
<Entity id="48269fec-05ea-46ea-b326-f5623a58c6e9" recommendedConfidence="75" patternsProximity="300">
  <Pattern confidenceLevel="75">
     <IdMatch idRef="Regex_taiwan_resident_certificate"/>
     <Match idRef="Keyword_taiwan_resident_certificate"/>
  </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_taiwan_resident_certificate**|
|:-----|
|Resident Certificate  <br/> Resident Cert  <br/> Resident Cert.  <br/> Identification card  <br/> Alien Resident Certificate  <br/> ARC  <br/> Taiwan Area Resident Certificate  <br/> TARC  <br/> 居留證  <br/> 外僑居留證  <br/> 台灣地區居留證|

## U.K. Driver's License Number

 **Format**: Combination of 18 letters and digits in the specified format

 **Pattern**: 18 letters and digits:

- Five letters (not case sensitive) or the digit "9" in place of a letter

- One digit

- Five digits in the date format DDMMY for date of birth

- Two letters (not case sensitive) or the digit "9" in place of a letter

- Five digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_uk_drivers_license` finds content that matches the pattern.

- A keyword from `Keyword_uk_drivers_license` is found.

- The checksum passes.

```
<!-- U.K. Driver's License Number -->
<Entity id="f93de4be-d94c-40df-a8be-461738047551" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_uk_drivers_license" />
        <Match idRef="Keyword_uk_drivers_license" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_uk_drivers_license**|
|:-----|
|DVLA  <br/> light vans  <br/> quadbikes  <br/> motor cars  <br/> 125cc  <br/> sidecar  <br/> tricycles  <br/> motorcycles  <br/> photocard licence  <br/> learner drivers  <br/> licence holder  <br/> licence holders  <br/> driving licences  <br/> driving licence  <br/> dual control car|

## U.K. Electoral Roll Number

 **Format**: Two letters followed by 1-4 digits

 **Pattern**: Two letters (not case sensitive) followed by 1-4 numbers

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_uk_electoral` finds content that matches the pattern.

- A keyword from `Keyword_uk_electoral` is found.

```
<!-- U.K. Electoral Number -->
<Entity id="a3eea206-dc0c-4f06-9e22-aa1be3059963" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_uk_electoral" />
        <Any minMatches="1">
          <Match idRef="Keyword_uk_electoral" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_uk_electoral**|
|:-----|
|council nomination  <br/> nomination form  <br/> electoral register  <br/> electoral roll|

## U.K. National Health Service Number

 **Format**: 10-17 digits separated by spaces

 **Pattern**: 10-17 digits:

- Either 3 or 10 digits

- A space

- Three digits

- A space

- Four digits

 **Checksum**: Yes

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_uk_nhs_number` finds content that matches the pattern.

- One of the following is true:

  - A keyword from `Keyword_uk_nhs_number` is found.

  - A keyword from `Keyword_uk_nhs_number1` is found.

  - A keyword from `Keyword_uk_nhs_number_dob` is found.

- The checksum passes.

```
<!-- U.K. NHS Number -->
<Entity id="3192014e-2a16-44e9-aa69-4b20375c9a78" patternsProximity="300" recommendedConfidence="85">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_uk_nhs_number" />
        <Any minMatches="1">
          <Match idRef="Keyword_uk_nhs_number" />
          <Match idRef="Keyword_uk_nhs_number1" />
          <Match idRef="Keyword_uk_nhs_number_dob" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_uk_nhs_number**|**Keyword_uk_nhs_number1**|**Keyword_uk_nhs_number_dob**|
|:-----|:-----|:-----|
|national health service  <br/> nhs  <br/> health services authority  <br/> health authority|patient id  <br/> patient identification  <br/> patient no  <br/> patient number|GP  <br/> DOB  <br/> D.O.B  <br/> Date of Birth  <br/> Birth Date|

## U.K. National Insurance Number (NINO)

 **Format**: Nine letters and digits, with each pair of letters and digits optionally separated by spaces or dashes

 **Pattern**: Nine letters and digits, with each pair of letters and digits optionally separated by spaces or dashes:

- Two letters (not case sensitive), neither of which can be D, F, I, Q, U, or V. Additionally, the second letter can't be O. The following combinations are also not allowed: BG, GB, KN, NK, NT, TN, and ZZ.

- Six digits

- A space or dash (optional)

- Two digits

- A space or dash (optional)

- Two digits

- A space or dash (optional)

- Two digits

- One letter that can be A, B, C, D; or one space.

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_uk_nino` finds content that matches the pattern.

- A keyword from `Keyword_uk_nino` is found.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_uk_nino` finds content that matches the pattern.

- No keyword from `Keyword_uk_nino` is found.

```
<!-- U.K. NINO -->
<Entity id="16c07343-c26f-49d2-a987-3daf717e94cc" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_uk_nino" />
        <Any minMatches="1">
          <Match idRef="Keyword_uk_nino" />
        </Any>
    </Pattern>
     <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_uk_nino" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_uk_nino" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_uk_nino**|
|:-----|
|national insurance number  <br/> national insurance contributions  <br/> protection act  <br/> insurance  <br/> social security number  <br/> insurance application  <br/> medical application  <br/> social insurance  <br/> medical attention  <br/> social security  <br/> great britain  <br/> insurance|

## U.S. / U.K. Passport Number

 **Format**: Nine digits

 **Pattern**: Nine consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_usa_uk_passport` finds content that matches the pattern.

- A keyword from `Keyword_passport` is found.

```
<Entity id="178ec42a-18b4-47cc-85c7-d62c92fd67f8" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_usa_uk_passport" />
        <Match idRef="Keyword_passport" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_passport**|
|:-----|
|Passport Number  <br/> Passport No  <br/> Passport #  <br/> Passport#  <br/> PassportID  <br/> Passportno  <br/> passportnumber  <br/> パスポート  <br/> パスポート番号  <br/> パスポートのNum  <br/> パスポート＃  <br/> Numéro de passeport  <br/> Passeport n °  <br/> Passeport Non  <br/> Passeport #  <br/> Passeport#  <br/> PasseportNon  <br/> Passeportn °|

## U.S. Bank Account Number

 **Format**: 4-17 digits

 **Pattern**: 4-17 consecutive digits

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The regular expression `Regex_usa_bank_account_number` finds content that matches the pattern.

- A keyword from `Keyword_usa_Bank_Account` is found.

```
<!-- U.S. Bank Account Number -->
<Entity id="a2ce32a8-f935-4bb6-8e96-2a5157672e2c" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Regex_usa_bank_account_number" />
        <Match idRef="Keyword_usa_Bank_Account" />
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_usa_Bank_Account**|
|:-----|
|Checking Account Number  <br/> Checking Account  <br/> Checking Account #  <br/> Checking Acct Number  <br/> Checking Acct #  <br/> Checking Acct No.  <br/> Checking Account No.  <br/> Bank Account Number  <br/> Bank Account #  <br/> Bank Acct Number  <br/> Bank Acct #  <br/> Bank Acct No.  <br/> Bank Account No.  <br/> Savings Account Number  <br/> Savings Account.  <br/> Savings Account #  <br/> Savings Acct Number  <br/> Savings Acct #  <br/> Savings Acct No.  <br/> Savings Account No.  <br/> Debit Account Number  <br/> Debit Account  <br/> Debit Account #  <br/> Debit Acct Number  <br/> Debit Acct #  <br/> Debit Acct No.  <br/> Debit Account No.|

## U.S. Driver's License Number

 **Format**: Depends on the state

 **Pattern**: Depends on the state -- for example, New York:

- Nine digits formatted like ddd ddd ddd will match

- Nine digits like ddddddddd will not match.

 **Checksum**: No

 **Definition**:

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_new_york_drivers_license_number` finds content that matches the pattern.

- A keyword from `Keyword_[state_name]_drivers_license_name` is found.

- A keyword from `Keyword_us_drivers_license` is found.

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_new_york_drivers_license_number` finds content that matches the pattern.

- A keyword from `Keyword_[state_name]_drivers_license_name` is found.

- A keyword from `Keyword_us_drivers_license_abbreviations` is found.

- No keyword from `Keyword_us_drivers_license` is found.

```
<Pattern confidenceLevel="75">
        <IdMatch idRef="Func_new_york_drivers_license_number" />
        <Match idRef="Keyword_new_york_drivers_license_name" />
        <Match idRef="Keyword_us_drivers_license" />
    </Pattern>
    <Pattern confidenceLevel="65">
        <IdMatch idRef="Func_new_york_drivers_license_number" />
        <Match idRef="Keyword_new_york_drivers_license_name" />
        <Match idRef="Keyword_us_drivers_license_abbreviations" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Keyword_us_drivers_license" />
        </Any>
    </Pattern>
```

 **Keywords**:

|**Keyword_us_drivers_license_abbreviations**|**Keyword_us_drivers_license**|**Keyword_[state_name]_drivers_license_name**|
|:-----|:-----|:-----|
|DL  <br/> DLS  <br/> CDL  <br/> CDLS  <br/> ID  <br/> IDs  <br/> DL#  <br/> DLS#  <br/> CDL#  <br/> CDLS#  <br/> ID#  <br/> IDs#  <br/> ID number  <br/> ID numbers  <br/> LIC  <br/> LIC#|DriverLic  <br/> DriverLics  <br/> DriverLicense  <br/> DriverLicenses  <br/> Driver Lic  <br/> Driver Lics  <br/> Driver License  <br/> Driver Licenses  <br/> DriversLic  <br/> DriversLics  <br/> DriversLicense  <br/> DriversLicenses  <br/> Drivers Lic  <br/> Drivers Lics  <br/> Drivers License  <br/> Drivers Licenses  <br/> Driver'Lic  <br/> Driver'Lics  <br/> Driver'License  <br/> Driver'Licenses  <br/> Driver' Lic  <br/> Driver' Lics  <br/> Driver' License  <br/> Driver' Licenses  <br/> Driver'sLic  <br/> Driver'sLics  <br/> Driver'sLicense  <br/> Driver'sLicenses  <br/> Driver's Lic  <br/> Driver's Lics  <br/> Driver's License  <br/> Driver's Licenses  <br/> identification number  <br/> identification numbers  <br/> identification #  <br/> id card  <br/> id cards  <br/> identification card  <br/> identification cards  <br/> DriverLic#  <br/> DriverLics#  <br/> DriverLicense#  <br/> DriverLicenses#  <br/> Driver Lic#  <br/> Driver Lics#  <br/> Driver License#  <br/> Driver Licenses#  <br/> DriversLic#  <br/> DriversLics#  <br/> DriversLicense#  <br/> DriversLicenses#  <br/> Drivers Lic#  <br/> Drivers Lics#  <br/> Drivers License#  <br/> Drivers Licenses#  <br/> Driver'Lic#  <br/> Driver'Lics#  <br/> Driver'License#  <br/> Driver'Licenses#  <br/> Driver' Lic#  <br/> Driver' Lics#  <br/> Driver' License#  <br/> Driver' Licenses#  <br/> Driver'sLic#  <br/> Driver'sLics#  <br/> Driver'sLicense#  <br/> Driver'sLicenses#  <br/> Driver's Lic#  <br/> Driver's Lics#  <br/> Driver's License#  <br/> Driver's Licenses#  <br/> id card#  <br/> id cards#  <br/> identification card#  <br/> identification cards#|State abbreviation (for example, "NY")  <br/> State name (for example, "New York")|

## U.S. Individual Taxpayer Identification Number (ITIN)

 **Format**: Nine digits that start with a "9" and contain a "7" or "8" as the fourth digit, optionally formatted with spaces or dashes

 **Pattern**:

Formatted:

- The digit "9"

- Two digits

- A space or dash

- A "7" or "8"

- A digit

- A space, or dash

- Four digits

Unformatted:

- The digit "9"

- Two digits

- A "7" or "8"

- Five digits

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_formatted_itin` finds content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_itin` is found.

  - The function `Func_us_address` finds an address in the right date format.

  - The function `Func_us_date` finds a date in the right date format.

  - A keyword from `Keyword_itin_collaborative` is found.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_unformatted_itin` finds content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_itin_collaborative` is found.

  - The function `Func_us_address` finds an address in the right date format.

  - The function `Func_us_date` finds a date in the right date format.

```
<!-- U.S. Individual Taxpayer Identification Number (ITIN) -->
<Entity id="e55e2a32-f92d-4985-a35d-a0b269eb687b" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_formatted_itin" />
        <Any minMatches="1">
          <Match idRef="Keyword_itin" />
          <Match idRef="Func_us_address" />
          <Match idRef="Func_us_date" />
          <Match idRef="Keyword_itin_collaborative" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_unformatted_itin" />
        <Match idRef="Keyword_itin" />
        <Any minMatches="1">
          <Match idRef="Keyword_itin_collaborative" />
          <Match idRef="Func_us_address" />
          <Match idRef="Func_us_date" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_itin**|**Keyword_itin_collaborative**|
|:-----|:-----|
|taxpayer  <br/> tax id  <br/> tax identification  <br/> itin  <br/> ssn  <br/> tin  <br/> social security  <br/> tax payer  <br/> itins  <br/> taxid  <br/> individual taxpayer|License  <br/> DL  <br/> DOB  <br/> Birthdate  <br/> Birthday  <br/> Date of Birth|

## U.S. Social Security Number (SSN)

 **Format**: 9 digits, which may be in a formatted or unformatted pattern

> [!NOTE]
> If issued before mid-2011, an SSN has strong formatting where certain parts of the number must fall within certain ranges to be valid (but there's no checksum).

 **Pattern**: Four functions look for SSNs in four different patterns:

- `Func_ssn` finds SSNs with pre-2011 strong formatting that are formatted with dashes or spaces (ddd-dd-dddd OR ddd dd dddd)

- `Func_unformatted_ssn` finds SSNs with pre-2011 strong formatting that are unformatted as nine consecutive digits (ddddddddd)

- `Func_randomized_formatted_ssn` finds post-2011 SSNs that are formatted with dashes or spaces (ddd-dd-dddd OR ddd dd dddd)

- `Func_randomized_unformatted_ssn` finds post-2011 SSNs that are unformatted as nine consecutive digits (ddddddddd)

 **Checksum**: No

 **Definition**:

A DLP policy is 85% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_ssn` finds content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_ssn` is found.

  - The function `Func_us_date` finds a date in the right date format.

  - The function `Func_us_address` finds an address in the right date format.

A DLP policy is 75% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_unformatted_ssn` finds content that matches the pattern.

- A keyword from `Keyword_ssn` is found.

- At least one of the following is true:

  - The function `Func_us_date` finds a date in the right date format.

  - The function `Func_us_address` finds an address in the right date format.

A DLP policy is 65% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_randomized_formatted_ssn` finds content that matches the pattern.

- The function `Func_ssn` does not find content that matches the pattern.

- At least one of the following is true:

  - A keyword from `Keyword_ssn` is found.

  - The function `Func_us_date` finds a date in the right date format.

  - The function `Func_us_address` finds an address in the right date format.

A DLP policy is 55% confident that it's detected this type of sensitive information if, within a proximity of 300 characters:

- The function `Func_randomized_unformatted_ssn` finds content that matches the pattern.

- A keyword from `Keyword_ssn` is found.

- The function `Func_unformatted_ssn` does not find content that matches the pattern.

- At least one of the following is true:

  - The function `Func_us_date` finds a date in the right date format.

  - The function `Func_us_address` finds an address in the right date format.

```
<!-- U.S. Social Security Number (SSN) -->
<Entity id="a44669fe-0d48-453d-a9b1-2cc83f2cba77" patternsProximity="300" recommendedConfidence="75">
    <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_ssn" />
        <Any minMatches="1">
          <Match idRef="Keyword_ssn" />
          <Match idRef="Func_us_date" />
          <Match idRef="Func_us_address" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="75">
        <IdMatch idRef="Func_unformatted_ssn" />
        <Match idRef="Keyword_ssn" />
        <Any minMatches="1">
          <Match idRef="Func_us_date" />
          <Match idRef="Func_us_address" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="65">
        <IdMatch idRef="Func_randomized_formatted_ssn" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Func_ssn" />
        </Any>
        <Any minMatches="1">
          <Match idRef="Keyword_ssn" />
          <Match idRef="Func_us_date" />
          <Match idRef="Func_us_address" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="55">
        <IdMatch idRef="Func_randomized_unformatted_ssn" />
        <Match idRef="Keyword_ssn" />
        <Any minMatches="0" maxMatches="0">
          <Match idRef="Func_unformatted_ssn" />
        </Any>
        <Any minMatches="1">
          <Match idRef="Func_us_date" />
          <Match idRef="Func_us_address" />
        </Any>
    </Pattern>
</Entity>
```

 **Keywords**:

|**Keyword_ssn**|
|:-----|
|Social Security  <br/> Social Security#  <br/> Soc Sec  <br/> SSN  <br/> SSNS  <br/> SSN#  <br/> SS#  <br/> SSID|
