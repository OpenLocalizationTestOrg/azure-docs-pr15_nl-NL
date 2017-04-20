<properties
    pageTitle="Voorbeelden voor het algemene gebruikspatronen in Stream Analytics query | Microsoft Azure"
    description="Algemene Azure Stream Analytics Query patronen "
    keywords="Voorbeelden van de query"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Voorbeelden voor algemene Stream Analytics gebruikspatronen query

## <a name="introduction"></a>Inleiding

Query's in Azure Stream Analytics worden uitgedrukt in een SQL-achtige querytaal, die wordt beschreven in de Naslaggids [Stream Analytics Query taal](https://msdn.microsoft.com/library/azure/dn834998.aspx) .  In dit artikel vindt u een overzicht van oplossingen voor verschillende algemene query patronen op basis van de echte wereld scenario's.  Dit is een werk in uitvoering en blijft moeten worden bijgewerkt met nieuwe patronen op doorlopend.

## <a name="query-example-data-type-conversions"></a>Voorbeeld van de query: gegevenstypeconversies
**Beschrijving**: definiëren van de soorten de eigenschappen op de invoer stream.
Bijvoorbeeld auto gewicht komt voor de invoer stroom als tekenreeksen en moet worden geconverteerd naar INT om uit te voeren deze totaliseren.

**Invoer**:

| Tabelmaakquery | Tijd | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | '1000' |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Uitvoer**:

| Tabelmaakquery | Gewicht |
| --- | --- |
| Honda | 3000 |

**Oplossing**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Uitleg**: een CAST-instructie op het veld gewicht gebruiken om op te geven van het type (Zie de lijst met ondersteunde gegevenstypen [hier](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Voorbeeld van de query: gebruik vind ik leuk/niet wilt patroon overeenkomende
**Beschrijving**: controleren of de waarde van een veld op de gebeurtenis overeenkomt met een bepaald patroon bijvoorbeeld afzender overschrijven die beginnen met A en eindigen met 9

**Invoer**:

| Tabelmaakquery | LicensePlate | Tijd |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| Tabelmaakquery | LicensePlate | Tijd |
| --- | --- | --- |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Oplossing**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Uitleg**: gebruik van de LIKE-instructie om te controleren dat de waarde van het LicensePlate veld begint met A en vervolgens een willekeurige tekenreeks nul of meer tekens heeft en met 9 eindigt. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Voorbeeld van de query: logica voor verschillende zaken/waarden (hoofdletters/kleine letters-instructies) opgeven
**Beschrijving**: bieden verschillende berekening voor een veld op basis van bepaalde criteria.
Bijvoorbeeld: Geef een beschrijving van de tekenreeks voor hoeveel auto's van de dezelfde maken met een speciale hoofdletters/kleine letters worden doorgegeven voor 1.

**Invoer**:

| Tabelmaakquery | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| CarsPassed | Tijd |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01-01T00:00:10.0000000Z |

**Oplossing**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Uitleg**: de hoofdletters/kleine letters component kan wij een verschillende berekenen op basis van bepaalde criteria (in onze geval het aantal van auto's in het venster statistische).

## <a name="query-example-send-data-to-multiple-outputs"></a>Voorbeeld van de query: gegevens verzenden naar meerdere uitvoer
**Beschrijving**: gegevens verzenden naar meerdere uitvoer doelen van één taak.
Bijvoorbeeld analyseren van gegevens voor de melding voor een drempelwaarde gebaseerde en alle gebeurtenissen met blob storage archiveren

**Invoer**:

| Tabelmaakquery | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Tabelmaakquery | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Tabelmaakquery | Tijd | Tellen |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Oplossing**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Uitleg**: het in-component worden vermeld Stream Analytics welke van de uitvoer van bij het wegschrijven van de gegevens van deze instructie.
De eerste query is een Pass Through-query van de gegevens die we naar een uitvoer dat we ArchiveOutput naam hebt ontvangen.
De tweede query bevat enkele eenvoudige aggregatie en filteren en de resultaten verzenden naar een volgende waarschuwingsmethoden systeem.
*Opmerking*: U kunt de resultaten van CTE's (dat wil zeggen met instructies) ook opnieuw gebruiken in meerdere uitvoer instructies: dit is het voordeel van het openen van minder lezers naar de invoerbron.
Bijvoorbeeld: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Voorbeeld van de query: unieke waarden tellen
**Beschrijving**: het aantal unieke waarden die worden weergegeven in de stream binnen een tijdvenster tellen.
Het aantal unieke tabelmaakquery van auto's is bijvoorbeeld gepasseerd van de stand betaalde in een 2 tweede venster?

**Invoer**:

| Tabelmaakquery | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Uitvoer:**

| Tellen | Tijd |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Oplossing:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Uitleg:** Doen we een eerste aggregatie om unieke maken met hun aantal via het venster.
Dan doen we een aggregatie van hoeveel maken we alle unieke waarden in een venster krijgen de dezelfde tijdstempel en vervolgens het tweede aggregatie-venster moet worden minimale om samen te voegen niet 2 windows uit de eerste stap hebt u – gegeven.

## <a name="query-example-determine-if-a-value-has-changed"></a>Voorbeeld van de query: bepalen of een waarde is gewijzigd#
**Beschrijving**: Bekijk een vorige waarde om te bepalen of deze anders uit dan de huidige waarde is bijvoorbeeld de vorige auto onderweg betaalde hetzelfde als de huidige auto maken?

**Invoer**:

| Tabelmaakquery | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Uitvoer**:

| Tabelmaakquery | Tijd |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Oplossing**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Uitleg**: gebruik VERTRAGINGS om te kijken naar de invoer streamen één gebeurtenis terug en haal de waarde aanbrengen. Vervolgens Vergelijk deze met het aanbrengen op de huidige gebeurtenis en uitvoer van de gebeurtenis als deze verschillen.

## <a name="query-example-find-first-event-in-a-window"></a>Voorbeeld van de query: de eerste gebeurtenis zoeken in een venster
**Beschrijving**: eerste auto zoeken in elke interval 10 minuten?

**Invoer**:

| LicensePlate | Tabelmaakquery | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Tabelmaakquery | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Oplossing**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Nu we het probleem en zoek eerste auto van bepaalde wijzigen in elk interval 10 minuten.

| LicensePlate | Tabelmaakquery | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Oplossing**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Voorbeeld van de query: zoeken naar laatste gebeurtenis in een venster
**Beschrijving**: laatste auto zoeken in elke interval 10 minuten.

**Invoer**:

| LicensePlate | Tabelmaakquery | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Tabelmaakquery | Tijd |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Oplossing**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Uitleg**: Er zijn twee stappen in de query – de eerste fase meest recente tijdstempel vindt u in windows 10 minuten. De tweede stap verbindt resultaten van de eerste query met de oorspronkelijke stream gebeurtenissen die voldoen aan laatste tijdstempels in elk venster zoeken. 

## <a name="query-example-detect-the-absence-of-events"></a>Voorbeeld van de query: gebrek aan gebeurtenissen detecteren
**Beschrijving**: controleren of een stroom heeft geen waarde die overeenkomt met een bepaalde criteria.
Bijvoorbeeld 2 opeenvolgende auto's uit het hetzelfde merk hebt onderweg betaalde binnen 90 seconden?

**Invoer**:

| Tabelmaakquery | LicensePlate | Tijd |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | DEFINITION-987 | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI-345 | 2015-01-01T00:00:04.0000000Z |

**Uitvoer**:

| Tabelmaakquery | Tijd | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA-999 | ABC-123 | 2015-01-01T00:00:01.0000000Z |

**Oplossing**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Uitleg**: gebruik VERTRAGINGS om te kijken naar de invoer streamen één gebeurtenis terug en haal de waarde aanbrengen. Vervolgens Vergelijk deze met het aanbrengen op de huidige gebeurtenis en uitvoer van de gebeurtenis als ze hetzelfde zijn en VERTRAGINGS toegang tot gegevens over de vorige auto kunt gebruiken.

## <a name="query-example-detect-duration-between-events"></a>Voorbeeld van de query: duur tussen gebeurtenissen detecteren
**Beschrijving**: zoek de duur van een bepaalde gebeurtenis. Bijvoorbeeld, gegeven een clickstream web bepalen tijd die aan een functie besteed.

**Invoer**:  
  
| Gebruiker | Functie | Gebeurtenis | Tijd |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Starten | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Einde | 2015-01-01T00:00:08.0000000Z |
  
**Uitvoer**:  
  
| Gebruiker | Functie | Duur |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Oplossing**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Uitleg**: laatste functie gebruiken om op te halen laatste tijdwaarde wanneer gebeurtenistype is 'Start'. Houd er rekening mee dat laatste functie PARTITION door [gebruiker] gebruikt om aan te geven dat resultaat wordt berekend per unieke gebruiker.  De query heeft een 1 uur maximum drempelwaarde voor tijdverschil tussen gebeurtenissen 'Start' en 'Stop', maar kan worden geconfigureerd als nodig (LIMIET DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Voorbeeld van de query: duur van een voorwaarde detecteren
**Beschrijving**: out hoelang een voorwaarde is opgetreden voor zoeken.
Stel dat een fout waardoor er alle auto's met een onjuiste gewicht (meer dan 20.000 pond) – we wilt berekenen van de duur van de bug.

**Invoer**:

| Tabelmaakquery | Tijd | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Uitvoer**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Oplossing**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Uitleg**: gebruik VERTRAGINGS bekijken van de invoer stream 24 uur en zoek naar exemplaren waar StartFault en StopFault worden overbrugd door gewicht < 20000.

## <a name="query-example-fill-missing-values"></a>Voorbeeld van de query: ontbrekende waarden invullen
**Beschrijving**: produceren voor de stroom van gebeurtenissen met ontbrekende waarden, een reeks gebeurtenissen met regelmatige tussenpozen.
Genereer gebeurtenis bijvoorbeeld elke vijf seconden die het meest recent zichtbaar gegevenspunt wordt rapporteren.

**Invoer**:

| t | waarde |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

**Uitvoer (eerste 10 rijen)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Oplossing**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Uitleg**: deze query gebeurtenissen elke 5 seconde wordt genereren en de laatste gebeurtenis die is ontvangen voordat u gaat uitvoeren. [Venster Hopping] (https://msdn.microsoft.com/library/dn835041.aspx "Hopping venster - Azure Stream Analytics") duur bepaalt hoe ver terug de query ziet er als u wilt zoeken naar de meest recente gebeurtenis (300 seconden in dit voorbeeld).


## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
