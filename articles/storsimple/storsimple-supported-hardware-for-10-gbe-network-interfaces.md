<properties 
   pageTitle="Hardware voor StorSimple 10 GbE interfaces | Microsoft Azure"
   description="Beschrijving van de ondersteunde kleine vormgeving pluggable (SFP) plaatsen, kabels en schakelopties voor de 10 GbE netwerk-interfaces op uw apparaat StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Hardware ondersteund voor de 10 GbE netwerkinterfaces op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

In dit artikel vindt u informatie over aanvullende hardware die met uw Microsoft Azure StorSimple-apparaat werkt.

## <a name="list-of-devices-tested-by-microsoft"></a>Lijst met apparaten getest door Microsoft

Microsoft heeft de volgende kleine vormgeving pluggable (SFP) plaatsen, kabels en schakelopties om ervoor te zorgen dat ze optimaal met apparaten werken getest. (In de volgende tabellen wordt bijgewerkt als nieuwe hardware wordt getest.)

### <a name="sfp-transceivers"></a>SFP + plaatsen

|Tabelmaakquery|Model|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kabels

|S. Nee. |Tabelmaakquery|Model|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp-Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Schakelopties

|S. Nee.|Tabelmaakquery|Model|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-FA|

## <a name="list-of-devices-tested-in-the-field"></a>Lijst met apparaten getest in het veld

Deze sectie bevat de lijst met apparaten die zijn is geïmplementeerd in het veld StorSimple klanten. Deze zijn niet getest door Microsoft, maar kunnen werken met uw apparaat StorSimple.
 
| Parameter                         | Waarde                                    |
|-----------------------------------|------------------------------------------|
| Schakeloptie maken                     | Juniper                                  |
| Schakeloptie model                    | ex4550-32F                               |
| De versie van de schakeloptie-besturingssysteem | JunOS 12.3R9.4                           |
| Blade model                     | Poorten ingebouwde PIC (0)                    |
| Ontvanger maken                  | Juniper                                  |
| Ontvanger model               | Onderdeelnummer 740-021308 <br></br> Onderdeelnummer 740-030658                   |
| Ontvanger firmwareversie    | Rev 01 versie 0,0 (gerapporteerd)            |
| Kabel model                     | Dubbelzijdig meestal LC/LC 50/125µ, OM3, LSZH |
| StorSimple model                | 8600                                     |
| StorSimple softwareversie     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Lijst met apparaten getest door OEM-provider (Mellanox)  

Mellanox heeft de volgende kleine vormgeving pluggable (SFP) plaatsen, kabels en schakelopties om ervoor te zorgen dat ze optimaal met Mellanox netwerk via interfaces zoals de 10 GbE netwerkinterfaces op uw apparaat StorSimple werken getest.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabels en modules worden ondersteund door Mellanox 

De volgende tabel bevat de kabels en modules worden ondersteund door Mellanox. Deze zijn niet getest door Microsoft, maar kunnen werken met uw apparaat StorSimple.

| S. Nee. | Snelheid | Model                 | Beschrijving                                            | Tabelmaakquery                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | passieve koperen kabel SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | passieve koperen kabel SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | passieve koperen kabel SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | passieve koperen kabel SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + kabel                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + kabel                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + kabel                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + tot SFP + 1m Direct bijvoegen koperen-kabel             | HP                    |
| 9.     | 10 GbE| 455883-B21 HP BLc     | 10Gb SR SFP + Opt                                       | HP                    |
| 10.    | 10 GbE| 455886-B21 HP BLc     | 10Gb LR SFP + Opt                                       | HP                    |
| 11.    |10 GbE | 487649-B21 HP BLc     | SFP + 0,5 m 10GbE koperen kabel                           | HP                    |
| 12.    |10 GbE | 487652-B21 HP BLc     | SFP + 1m 10GbE koperen kabel                             | HP                    |
| 13.    |10 GbE | 487655-B21 HP BLc     | SFP + 3m 10GbE koperen kabel                             | HP                    |
| 14.    |10 GbE | 487658-B21 HP BLc     | SFP + 7m 10GbE koperen kabel                             | HP                    |
| 15.    |10 GbE | 537963-B21 HP BLc     | SFP + 5m 10GbE koperen kabel                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m C-reeks passieve Koperen SFP + kabel                  | HP                    |
| 17.    |10 GbE | AP785A HP             | 5m C-reeks passieve Koperen SFP + kabel                  | HP                    |
| 18.    |10 GbE | AP818A HP             | 1m B-reeks actieve Koperen SFP + kabel                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B-reeks actieve Koperen SFP + kabel                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR ontvanger                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR ontvanger                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC-kabel                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC-kabel                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + 0.65 m DAC-kabel                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + 1, 2 m DAC-kabel                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m Pa-kabel                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A-QSA Mellanox | QSFP aan SFP + Adapter                                   | Mellanox technologieën |
| 28.    | 10 GbE| MC2309124-006 Mt      | Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 24awg 7 m   | Mellanox technologieën |
| 29.    | 10 GbE| MC2309124-007 Mt      | Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 24awg 7 m   | Mellanox technologieën |
| 30.    | 10 GbE| MC2309130-003 Mt      | Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 30awg 3 m   | Mellanox technologieën |
| 31.    | 10 GbE| MC2309130-00A Mt      | Passieve koperen kabel 1 x SFP+ naar QSFP 10 Gb/s 30awg 0,5 m | Mellanox technologieën |
| 32.    | 10 GbE| MC3309124-005 Mt      | Passieve koperen kabel 1 x SFP+ 10 Gb/s 24awg 5 m           | Mellanox technologieën |
| 33.    | 10 GbE| MC3309124-007 Mt      | Passieve koperen kabel 1 x SFP+ 10 Gb/s 24awg 7 m           | Mellanox technologieën |
| 34.    | 10 GbE| MC3309130-003 Mt      | Passieve koperen kabel 1 x SFP+ 10 Gb/s 30awg 3 m           | Mellanox technologieën |
| 35.    | 10 GbE| MC3309130-00A Mt      | Passieve koperen kabel 1 x SFP+ 10 Gb/s 30awg 0,5 m         | Mellanox technologieën |

### <a name="switches-supported-by-mellanox"></a>Schakelopties worden ondersteund door Mellanox 

De volgende tabel bevat de schakelopties die worden ondersteund door Mellanox. Deze zijn niet getest door Microsoft, maar kunnen werken met uw apparaat StorSimple.

| S. Nee. | Snelheid | Model | Beschrijving                                                         | Tabelmaakquery              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733-B21  | HP ProCurve 6120XG 10GbE Ethernet Blade Switch                      | HP    |
| 2.     | 10GbE | 538113-B21  | HP 10GbE Pass Through-query-Module (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex systeem stof EN4093 10 Gigabit Scalable schakeloptie Module | IBM   |
| 4.     | 1GbE  | 3020        | Cisco exemplaar 3020 1GbE veranderen blade                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco exemplaar 3020 X 1GbE veranderen blade                              | Cisco |
| 6.     | 1GbE  | 438030-B21  | HP 1GbE veranderen module - GbE2c Layer 2/3 Ethernet Blade Switch       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE veranderen blade                              | HP    |

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over StorSimple hardwareonderdelen en status](storsimple-monitor-hardware-status.md).
