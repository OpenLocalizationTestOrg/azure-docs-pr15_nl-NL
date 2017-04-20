<properties 
   pageTitle="Technische specificaties StorSimple | Microsoft Azure"
   description="Beschrijft de technische specificaties en wettelijke normen naleving informatie voor de onderdelen van de hardware StorSimple."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Specificaties en naleving voor het StorSimple-apparaat

## <a name="overview"></a>Overzicht

De hardwareonderdelen van uw apparaat Microsoft Azure StorSimple voldoen aan de technische specificaties en wettelijke normen in dit artikel beschreven. De technische specificaties beschrijven de Power en koeling Modules (PCMs), schijfstations, opslagcapaciteit en bijlagen. De informatie over naleving behandelt zaken als internationale normen, veiligheid en uitstoot en kabel.


## <a name="power-and-cooling-module-specifications"></a>Specificaties voor Power en koeling Module  

Het apparaat StorSimple heeft twee 100-240V dubbele Waaieren, SBB-compatibele Power koeling Modules (PCMs). Hierdoor wordt een redundante configuratie. Als een PCM mislukt, blijft het apparaat werken normaal op de andere PCM totdat de mislukte module wordt vervangen.  

De ruimte EBOD gebruikt een 580 W PCM en primaire insluiting een 764 W PCM. De volgende tabellen bevatten de technische specificaties die is gekoppeld aan de PCMs.

| Specificatie           | 580 W PCM (EBOD)                                    | 764 W PCM (primair)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maximale vermogen    | 580 W                                               | 764                                                |
| Frequentie               | 50/60 Hz                                            | 50/60 Hz                                           |
| Spanning bereikselectie | Automatisch die variëren: 90-264 V AC, 47/63 Hz               | Automatisch die variëren: 90-264 V AC, 47/63 Hz               |
| Maximale stroomoverbelasting huidige  | 20 A                                                | 20 A                                               |
| Power factor correctie | > nominale Ingangsspanning 95%                          | > nominale Ingangsspanning 95%                         |
| Harmonische               | Voldoet aan EN61000-3-2                                   | Voldoet aan EN61000-3-2                                  |
| Uitvoer                  | 5 v stand-by spanning @ 2.0 A                          | 5 v stand-by spanning @ 2.7 A                         |
|                         | + 5 v @ 42 A                                          | + 5 v @ 40 A                                         |
|                         | + 12 v @ 38 A                                         | + 12 v @ 38 A                                        |
| Direct pluggable           |  Ja                                                | Ja                                                |
| Schakelopties en LED 's       | Schakeloptie AC aan/uit- en vier statusindicator LED 's     | Schakeloptie AC aan/uit- en zes statusindicator LED 's     |
| Insluiting koeling       | Ene koelventilatoren met variabele ventilator snelheid besturingselement  | Ene koelventilatoren met variabele ventilator snelheid besturingselement |

 
## <a name="power-consumption-statistics"></a>Power verbruiksstatistieken  

De volgende tabel bevat de normale power verbruik-gegevens (werkelijke waarden kunnen afwijken van de gepubliceerde) voor de verschillende modellen StorSimple apparaat. 
 
 Voorwaarden | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Ventilatoren traag, stations inactief | 1,45 A  |0.31 kW | 1057.76 BTU/uur | 3.19 A | 0.34 kW | 1160.13 BTU/uur 
 Ventilatoren traag, stations openen | 1,54 A | 0,33 kW | 1126.01 BTU/uur | 3.27 A | 0.36 kW | 1228.37 BTU/uur 
 Snelle, stations voor ventilatoren is niet actief geweest, twee PSUs mogelijk gemaakt | 2.14 A | 0.49 kW  | 1671.95 BTU/uur | 4,99 A | 0.54 kW | 1842.56 BTU/uur 
 Ventilatoren snel, niet-actieve stations, één voeding mogelijk gemaakt door een niet-actieve | 2.05 A | 0.48 kW | 1637.83 BTU/uur | 4,58 A | 0,50 kW | 1706.07 BTU/uur 
 Ventilatoren snel, stations openen kunnen twee PSUs mogelijk gemaakt | 2, 26 G A | 0,51 kW | 1740.19 BTU/uur | 4,95 A | 0.54 kW | 1842.56 BTU/uur 
 Ventilatoren snel, stations openen, één voeding mogelijk gemaakt een niet-actieve | 2.14 A |0.49 kW | 1671.95 BTU/uur | 4.81 A  | 0.53 kW | 1808.44 BTU/uur 

## <a name="disk-drive-specifications"></a>Specificaties voor schijf  

Uw apparaat StorSimple ondersteunt maximaal 12 3,5-inch formulier seriële bijgevoegd SCSI (SA's) factor schijfstations. De werkelijke stations is een combinatie van solid-state stations (SSD) of vaste schijven (harde schijven), afhankelijk van de configuratie van het product. De 12 schijf sleuven bevinden zich in een 3 bij 4-configuratie vóór de ruimte. De ruimte EBOD kan voor extra opslagruimte voor een andere 12 schijfstations. Hierna ziet u altijd harde schijven.  

## <a name="storage-specifications"></a>Specificaties voor opslag
De apparaten StorSimple hebben een combinatie van de harde schijf en solid-state stations voor zowel het 8100 en 8600. De totale best capaciteit voor de 8100 en 8600 respectievelijk ongeveer 15 TB en 38 TB. De volgende tabel worden de details van SSD, harde schijf en cloud capaciteit in de context van de capaciteit van de oplossing StorSimple.

| Apparaatmodel / capaciteit                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Aantal vaste schijven (harde schijven)              |   8                                                     |  19                                                     |
| Aantal solid-state stations (SSD)            |   4                                                     |  5                                                      |
| Eén capaciteit van de harde schijf                            |   4 TB                                                  |  4 TB                                                   |
| Eén SSD capaciteit                            |   400 GB                                                |  800 GB                                                 |
| Vrije capaciteit                                 |   4 TB                                                  | 4 TB                                                    |
| Best capaciteit van de harde schijf                            | 14 TB                                                   | 36 TB                                                   |
| Best SSD capaciteit                            | 800 GB                                                  | 2 TB                                                    |
| Totale best capaciteit *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maximale oplossing capaciteit (inclusief de cloud)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *De totale best capaciteit bevat de capaciteit die beschikbaar zijn voor gegevens, metagegevens en buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Insluiting dimensies en dikte specificaties  

De volgende tabellen staan de verschillende insluiting specificaties voor dimensies en dikte.  

### <a name="enclosure-dimensions"></a>Insluiting afmetingen
De volgende tabel bevat de afmetingen van de ruimte in millimeters en inches.

|Insluiting |Millimeters |Inch |
|----------|------------|-------| 
| Hoogte |87.9 | 3,46 |
| Breedte over flens koppelen | 483 | 19.02 |
| Breedte in de hoofdtekst van insluiting | 443 | 17.44 |
| Diepteas uit flens voorste koppelen aan het uiteinde van Insluiting hoofdtekst | 577 | 22.72 |
| Diepteas via bewerkingen Configuratiescherm naar verst uiteinde insluiting | 630.5 | 24.82 |
| Diepteas uit koppelen flens naar verst uiteinde insluiting |   603 | 23.74 |

### <a name="enclosure-weight"></a>Insluiting gewicht  

Afhankelijk van de configuratie, een volledig geladen primaire insluiting wegen van 21 naar 33 kg en is vereist twee personen afgehandeld. 
 
| Insluiting | Gewicht |
|-----------|--------| 
| Maximaal gewicht (afhankelijk van de configuratie) |30 kg – 33 kg |
| Leeg (geen stations voorzien) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Specificaties voor insluiting-omgeving  

In deze sectie bevat de specificaties die betrekking hebben op de omgeving insluiting. De temperatuur, vochtigheid, hoogte, gebruik, trillingen, afdrukstand, veiligheid en elektromagnetische compatibiliteit (EMC) worden opgenomen in deze categorie.  

### <a name="temperature-and-humidity"></a>Temperatuur en vochtigheid

| Insluiting        | Temperatuurbereik  | Relatieve omgevingsvochtigheid | Maximale wet bol   |
|------------------|----------------------------|---------------------------|--------------------|
| Operationele      | 5° C - 35° C (41° F - 95° F)    | 20% - 80% niet-condenserend- | 28° C (82° F)        |
| Niet-operationele  | -40 TEMPERATUUR - 70° C (40° F - 158° F) | 5-100% niet-condenserend  | 29° C (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luchtstroom, hoogte, gebruik, trillingen, afdrukstand, veiligheid en EMC
 
| Insluiting          | Operationele specificaties                                                |
|--------------------|---------------------------------------------------------------------------| 
| Luchtstroom            | Systeem luchtstroom is voor naar achteren. Systeem moet worden gebruikt met een low-pressure, achterzijde uitlaatgas installatie. Vorige druk die door rek deuren en belemmering mag niet meer dan 5 Pascal (0,5 mm water toelichtingen). |
| Hoogte, operationele  | meter-30 toe aan 3045 meter (voet-100 naar 10.000 voet) met maximale temperatuur ongedaan door 5 ° C boven 7000 voet worden geclassificeerd. |
| Hoogte, niet-operationele  | -305 meter naar 12.192 meter (-1,000 voet naar 40.000 voet) |
| Gebruik, operationele  | 5g 10 ms ½ sinus |
| Gebruik, niet-operationele  | 10 ms ½ sinus van 30g |
| Trillingen, operationele  | 0.21g RMS 5 tot 500 Hz willekeurig |
| Trillingen, niet-operationele  | 1.04g RMS 2-200 Hz willekeurig |
| Trillingen, verplaatsing  | 3g 2-200 Hz sinus |
| Afdrukstand en koppelen  | 19-inch rek mountains (2 EIA eenheden) |
| Rackrails  | Als u wilt aanpassen aan de minimale 700 mm (31.50 inch) diepteas rekken compatibel is met IEC 297 |
| Veiligheid en goedkeuringen  |   CE en UL nl 61000-3, IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Internationale normen naleving
Uw Microsoft Azure StorSimple apparaat voldoet aan de volgende internationale normen:  

- CE - NL 60950-1  
- CB-rapport naar IEC 60950-1  
- UL en cUL UL 60950-1  

## <a name="safety-compliance"></a>Veiligheid naleving  

Uw Microsoft Azure StorSimple-apparaat voldoet aan de volgende veiligheid classificaties:  

- Goedkeuring van systeem product: UL, cUL, CE  
- Naleving van de veiligheid: UL 60950, IEC 60950, nl 60950  

## <a name="emc-compliance"></a>EMC naleving 

Uw Microsoft Azure StorSimple-apparaat voldoet aan de volgende EMC classificaties.  

### <a name="emissions"></a>Uitstoot

Het apparaat is compatibel met EMC voor uitgevoerde en uitgezonden uitstoot niveaus.  

- Uitgevoerde uitstoot beperken niveaus: CFR 47 deel 15 ter Class A EN55022 Class A CISPR Class A  
- Uitgezonden uitstoot beperken niveaus: CFR 47 deel 15 ter Class A EN55022 Class A CISPR Class A   

### <a name="harmonics-and-flicker"></a>Harmonische en Flickr  

Het apparaat voldoet aan EN61000-3-2-3.  

### <a name="immunity-limit-levels"></a>Limiet immuniteitsgrenzen  
Het apparaat voldoet aan EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power kabel is naleving
  
De plug en de vergadering van de kabel is voltooid power moeten voldoen aan de standaarden geschikt te maken voor het land waarin het apparaat wordt gebruikt, en ze veiligheid goedkeuringen die toegestaan in dat land zijn moeten hebben. De volgende tabellen bevatten normen voor de VS en Europa.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC power kabel - VS (moet NRTL vermeld zijn)

| Onderdeel       | Specificatie                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Type kabel is       | AVP of SVT, 18 AWG minimum, 3 geleider 2.0 meter maximale duur |
| Sluit apparaat            | NEMA 5-15P basiskennis-type bijlage plug geclassificeerd 120 V, 10 A; of IEC 320 C14, 250 V, 10 A |
| Socket          | IEC 320 C-13, 250 V, 10 A                                         |

### <a name="ac-power-cords---europe"></a>AC power kabel - Europa

| Onderdeel       | Specificatie                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Type kabel is       | Geharmoniseerde, H05-VVF-3G1.0                                         |
| Socket          | IEC 320 C-13, 250 V, 10 A                                         |

## <a name="supported-network-cables"></a>Ondersteunde netwerkkabels  

Voor de 10 GbE netwerkinterfaces, gegevens 2 en 3 voor gegevens, raadpleegt u de [lijst met ondersteunde netwerkkabels en modules](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te implementeren van een apparaat StorSimple in uw datacenter. Zie [uw on-premises implementatie-apparaat](storsimple-deployment-walkthrough-u2.md)gebruiken voor meer informatie.  
