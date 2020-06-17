---
title: 'Configuration notes for supported VoIP gateways, IP PBXs, and PBXs: Exchange 2013 Help'
TOCTitle: Configuration notes for supported VoIP gateways, IP PBXs, and PBXs
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer: 
mtps_version: v=EXCHG.150
---

# Configuration notes for supported VoIP gateways, IP PBXs, and PBXs in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

This page provides links to configuration notes that have been created and tested by Microsoft or a VoIP gateway partner. When Microsoft or a partner deploys Unified Messaging with a new VoIP gateway and PBX or IP PBX configuration, the prerequisites and configuration settings are documented. This information is used to create a configuration note.

Each PBX configuration note contains information about how to deploy Unified Messaging with a specific telephony configuration, and includes the manufacturer, model, and firmware version for the VoIP gateways, IP PBXs, or PBXs. In addition, each PBX configuration note includes other information, such as:

- Contributors in authoring the configuration note.

- Detailed prerequisites, including the following:

  - Features that have to be enabled or disabled on the PBX.

  - Specialized hardware that has to be installed.

  - Whether a VoIP gateway is required.

  - Features that must be present on the VoIP gateway, if one is needed.

  - Specific cabling requirements between an IP gateway and a PBX.

  - A list of Unified Messaging features that may not be available with a given telephony configuration.

To find out more about the Microsoft Unified Communications Open Interoperability Program for enterprise telephony infrastructure, including finding qualified SIP PSTN gateways and IP PBXs and the process telephony infrastructure vendors can use to join and participate in the program, see [Microsoft Unified Communications Open Interoperability Program](https://docs.microsoft.com/SkypeForBusiness/lync-cert/qualified-lync-apps).

## VoIP gateway, IP PBX, and PBX configuration notes

Microsoft is working with VoIP gateway partners, AudioCodes and Dialogic, to add to the list of PBXs that are tested. Because we are currently testing many combinations of telephony components, this topic is updated frequently. Please check back if you can't locate the appropriate configuration note for your deployment.

### Aastra
<a name="aastra"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Aastra MD110 (formerly Ericsson MD110)|MX1 TSW R2A (aka BC13)|Analog - Serial MD110|Dialogic|DMG1008LSW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/)|
| Aastra MD110 (formerly Ericsson MD110)|MX1 TSW R2A (aka BC13)|E1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/)|
| Aastra MX-ONE|4.0|Direct SIP Connection|N.A.|N.A.|Aastra|[Download](http://www.aastra.com/cps/rde/aareddownload?file_id=4384-14746-_P06_XML&dsproject=aastra&mtype=pdf)|

### Alcatel
<a name="alcatel"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|OmniPCX 4400|R4.2-d2.304-4-h-il-c6s2|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Avaya
<a name="avaya"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Aura|Communication Manager 5.2.1 with SP 5  <br/> Session Manager 5.2.|Direct SIP Connection|N.A.|N.A.|Avaya|[Download](https://support.avaya.com/downloads/)|
|CS 2100|CS 2100 SE13|Direct SIP Connection|N.A.|N.A.|Avaya|[Download](https://support.avaya.com/css/P8/documents/100149819)|
|Definity G3|R009i.05.122.4|Digital Set Emulation (DNI7434)|Dialogic|DMG1008DNIW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|Definity G3|R013i.01.1.628.7|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Definity G3|R013i.01.1.628.7|T1 CAS - In-Band DTMF|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Definity G3|R013i.01.1.628.7|T1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Definity G3|R013i.01.1.628.7|E1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Merlin Magix|Release 1.5 v.6.0|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|S8300|G3xV11 Communication Manager 1.3|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|S8300|R013x.01.2.632.1|T1 CAS - In-Band DTMF|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|S8300|R013x.01.2.632.1|E1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|S8500|Communication Manager 3.0 (R013x00.1.346.0)|E1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|S8500|Communication Manager 3.0 (R013x00.1.346.0)|T1 CAS - In-Band DTMF|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|S8500|Communication Manager 3.0 (R013x00.1.346.0)|T1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|S8700|R011x.02.0.110.4|E1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Cisco
<a name="cisco"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Cisco Call Manager 4.x|4.x|IP-to-IP|AudioCodes|AudioCodes|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Cisco Call Manager 5.1|5.1.0.9921-12|Direct SIP Connection|N.A.|N.A.|Microsoft|[Download](https://go.microsoft.com/fwlink/p/?linkId=225083)|
|Cisco Unified Communications Manager 6.0 and 6.1|6.x|Direct SIP Connection|N.A.|N.A.|Microsoft|[Download](https://go.microsoft.com/fwlink/p/?linkId=225083)|
|Cisco Unified Communications Manager 7.0|7.0.2.20000-5|Direct SIP Connection|N.A.|N.A.|Microsoft|[Download](https://go.microsoft.com/fwlink/p/?linkId=196361)|
|Cisco Unified Communications Manager 8.0|8.0.3.20000-5|Direct SIP Connection|N.A.|N.A.|Microsoft|[Download](https://go.microsoft.com/fwlink/p/?linkId=213007)|

### Inter-Tel
<a name="inter-tel"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|5000|Inter-Tel 5000 v2.1|T1 CAS - In-Band DTMF|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Axxess|Axxess V9.0|T1 CAS - In-Band DTMF|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Intecom
<a name="Intecom"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|PointSpan M6880|40PS3.5.K.2|T1 CAS - SMDI|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Mitel
<a name="mitel"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|3300|5.1.4.8|E1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|3300|5.1.4.8|T1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|SX2000|5.0.24|Digital Set Emulation (DNISS430)|Dialogic|DMG1008MTLDNIW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|3300|7|T1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### NEC
<a name="nec"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Electra Elite 192|SP034V4.5|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|NEAX2400IMX|version 7400|T1 CAS - serial MCI|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|NEAX2400IMX & IPX|version 7400|Digital Set Emulation (DNIDtermIII)|Dialogic|DMG1008DNIW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|NEAX2400IPX|Ver. R18.06.24.000|T1 CAS - serial MCI|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|NEAX2400IPX|Ver. R18.06.24.000|Analog - serial MCI|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|NEAX2400IPX|Ver.17 Rel.03.46.001|T1 Q.SIG - serial MCI|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|

### NeXspan
<a name="nexspan"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|S|RMS1 version R1.3 E1TA|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Nortel
<a name="nortel"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|CS1000|3.0 & 4.5|E1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Meridian 81C|4.5|E1 Q.SIG|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Meridian 81C|4.5|T1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Option11c|Release 25|Digital Set Emulation (DNI2616)|Dialogic|DMG1008DNIW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|Option11c|Release 25|T1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|Option11c|Release 25|E1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|CS-1000M (Succession)|Release 25.40|E1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|

### Panasonic
<a name="panasonic"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|KX-TDA200|001-001|Analog - In-Band DTMF|AudioCodes|Mediant 1000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|KX-TDA200|3|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|KX-TES824|2.0.2|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Rolm
<a name="rolm"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|9751|9005|Digital Set Emulation (DNIRP400)|Dialogic|DMG1008RLMDNIW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|

### ShoreTel
<a name="shoretel"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|IP Telephony System|6.1|Analog - SMDI|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|IP Telephony System|7.5|Analog - SMDI|AudioCodes|Mediant 1000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Siemens
<a name="siemens"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|HiCom 150E|Rel. 2.2|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|HiCom 300|SA300-V3.05|BRI QSIG|Dialogic|DMG3000|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|HiCom 300|9006.4SMR3|Digital Set Emulation (DNIOptiset)|Dialogic|DMG1008DNIW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|HiCom 300|9006.4SMR3|T1 CAS - In-Band DTMF|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|HiPath 3550|Rel. 3|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|HiPath 4000|Ver 3.0 SMR5 SMP4|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|HiPath 4000|SA300-V3.05|BRI QSIG|Dialogic|DMG3000|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|HiPath 4000|Ver 3.0 SMR5 SMP4|T1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|HiPath 4000|Version 2.0 SMR9 SMP0|Analog - In-Band DTMF|Dialogic|DMG1008LSW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|HiPath 4000|Version 2.0 SMR9 SMP0|T1 Q.SIG|Dialogic|DMG2030DTIQ|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|

### Sonus
<a name="sonus"> </a>

|**VoIP gateway model**|**VoIP gateway software release**|**Supported protocols**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|
|SBC 1000/2000|2.2.1 or later| TDM Signaling (ISDN): AT&T 4ESS/5ESS, Nortel DMS- 100, Euro ISDN (ETSI 300-102), QSIG, NTT InsNet (Japan), ANSI National ISDN-2 (NI-2)  <br/>  TDM Signaling (CAS): T1 CAS (E&M, Loop start); E1 CAS (R2)|Sonus|[Download](http://www.sonus.net/sites/default/files/sonussbc1k2kconfigo365.pdf)|

### Tadiran
<a name="tadiran"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Coral Flexicom|14.67.49|Analog - In-Band DTMF|AudioCodes|MP 11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral Flexicom|14.67.49|BRI QSIG|AudioCodes|Mediant  <br/> 1000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral Flexicom|14.67.49|E1 CAS - In-Band DTMF|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral Flexicom|14.67.49|E1 Q.SIG|AudioCodes|Mediant 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral IPX|14.67.49|Analog - In-Band DTMF|AudioCodes|MP-11x FXO|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral IPX|14.67.49|BRI QSIG|AudioCodes|Mediant 1000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral IPX|14.67.49|E1 CAS - In-Band DTMF|AudioCodes|Mediant 2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|
|Coral IPX|14.67.49|E1 QSIG|AudioCodes|Mediant  <br/> 1000/2000|AudioCodes|[Download](https://www.audiocodes.com/solutions/microsoft-unified-messaging)|

### Toshiba
<a name="toshiba"> </a>

|**PBX model**|**PBX software release**|**Protocol**|**Gateway vendor**|**Gateway model**|**Configuration author**|**Configuration file download**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|CTX|AR1ME021.00|Analog - SMDI|Dialogic|DMG1008LSW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
|CTX|AR1ME021.00|Analog - In-Band DTMF|Dialogic|DMG1008LSW|Dialogic|[Download](https://www.dialogic.com/support/helpweb/mg/integration-exchange-2013-help.md)|
