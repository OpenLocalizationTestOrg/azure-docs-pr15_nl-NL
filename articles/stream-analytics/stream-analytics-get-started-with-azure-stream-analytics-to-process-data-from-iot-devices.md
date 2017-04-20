<properties
    pageTitle="Aan de slag met Azure Stream Analytics om gegevens te verwerken vanaf IoT apparaten. | Microsoft Azure"
    description="IoT sensor labels en gegevens gegevensstromen met stream analyses en realtime gegevens verwerking"
    keywords="IOT de oplossing aan de slag met iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Aan de slag met Azure Stream Analytics om gegevens te verwerken vanaf IoT apparaten

In deze zelfstudie leert u hoe stream-verwerking logica voor het verzamelen van gegevens op Internet van dingen (IoT)-apparaten maken. We een echte, Internet van dingen (IoT) use-case gebruiken om te laten zien hoe u uw oplossing snel en economisch kunt maken.

## <a name="prerequisites"></a>Vereisten voor

-   [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Query en gegevens voorbeeldbestanden downloadbare uit [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scenario

Contoso, dat wil een bedrijf in de ruimte industriële automatisering zeggen, heeft de manufacturing-proces volledig geautomatiseerd. Machine in dit bedrijf heeft sensoren die kunnen streams van gegevens in realtime uitzenden. In dit scenario wil een productiemanager floor hebt realtime inzichten van de sensorgegevens om te zoeken naar patronen en deze acties uitvoeren. We gebruiken de Stream Analytics Query taal (SAQL) via de sensorgegevens interessante patronen uit de binnenkomende stroom van gegevens te zoeken.

Hier wordt gegevens vanaf een apparaat Texas Instruments sensor tag gegenereerd.

![Texas Instruments sensor tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

De nettolading van de gegevens zich in de indeling van JSON en ziet er als volgt uit:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

U kunt in een scenario voor echte honderden die sensoren gebeurtenissen genereren als een gegevensstroom hebben. Een gateway-apparaat zou ideale geval code als u wilt deze gebeurtenissen push [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/) of [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/)uitvoeren. Uw werk Stream Analytics zou nemen deze gebeurtenissen van gebeurtenis Hubs en realtime gebruiksanalyses query's uitvoeren op de streams. Vervolgens kan u de resultaten verzenden naar een van de [uitvoer van ondersteund](stream-analytics-define-outputs.md).

Voor gebruiksgemak vindt u in deze ophalen handleiding een voorbeeld van gegevensbestand, die is opgenomen vanaf reële sensor tag apparaten. U kunt query's uitvoeren op de voorbeeldgegevens en de resultaten weer te geven. In de volgende zelfstudies, u leert hoe u uw werk verbinden met invoer en uitvoer en implementeren ze naar de Azure-service.

## <a name="create-a-stream-analytics-job"></a>Een taak Stream analyses maken

1. In de [Azure-portal](http://portal.azure.com), klik op het plusteken en typ **STREAM ANALYTICS** in het tekstvenster naar rechts. Selecteer **Stream Analytics-taak** in de lijst met resultaten.

    ![Een nieuwe taak van de Stream analyses maken](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Voer een unieke taaknaam en controleer of het abonnement dat de juiste voor uw taak. Vervolgens een nieuwe resourcegroep maken of Selecteer een bestaande van uw abonnement.

3. Selecteer een locatie voor uw taak. Voor de snelheid van verwerking en wilt verkleinen van de kosten in gegevens wordt doorverbinden dezelfde locatie selecteren als de resourcegroep en de beoogde opslag-account aanbevolen.

    ![Details van een nieuwe Stream Analytics-taak maken](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] U moet dit account opslag slechts één keer per regio maken. Deze opslag wordt gedeeld in alle Stream Analytics-projecten die zijn gemaakt in die regio.

4. Schakel het selectievakje in als u wilt uw taak in uw dashboard te plaatsen en klik op **maken**.

    ![taak wordt gemaakt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Ziet u een 'implementatie gestart...' weergegeven in de bovenste rechts van het browservenster. Snel wordt deze gewijzigd in een voltooide venster zoals hieronder wordt weergegeven.

    ![taak wordt gemaakt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Een query Azure Stream analyses maken

Nadat uw taak is gemaakt heeft deze afstemmen met deze openen en een query maken. U kunt eenvoudig toegang tot uw taak door te klikken op de tegel voor deze.

![Taak-tegel](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Klik in het vak **QUERY** om te gaan naar de Query-Editor in het deelvenster **Topologie van de taak** . De **QUERY** -editor, kunt u een T-SQL-query waarmee de transformatie via de gegevens van de binnenkomende gebeurtenis in te voeren.

![In het queryvak](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Query: Uw onbewerkte gegevens archiveren

De eenvoudigste van query is een Pass Through-query die alle ingevoerde gegevens naar de aangewezen uitvoer worden gearchiveerd. Het voorbeeld van gegevensbestand naar een locatie op uw computer downloaden vanuit [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) . 

1. Plak de query uit het bestand PassThrough.txt. 

    ![De invoer gegevensstroom testen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Klik op de drie puntjes naast uw invoer en schakel in **Voorbeeldgegevens uit bestand uploaden** .

    ![De invoer gegevensstroom testen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Een deelvenster aan de rechterkant wordt geopend als een resultaat, in deze selecteert u het gegevensbestand HelloWorldASA-InputStream.json van uw gedownloade locatie en klik op **OK** onderaan in het deelvenster.

    ![De invoer gegevensstroom testen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Klik op het tandwiel **testen** in het bovenste gedeelte van het venster links en verwerken van uw testquery ten opzichte van de steekproef gegevensset. Een resultatenvenster wordt geopend onder uw query de verwerking is voltooid.

    ![Testresultaten](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Query: De gegevens filteren op basis van een voorwaarde

Laten we eens proberen om de resultaten op basis van een voorwaarde te filteren. Willen we resultaten voor de gebeurtenissen die afkomstig zijn uit "sensorA." De query is in het bestand Filtering.txt.

![Een gegevensstroom filteren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Houd er rekening mee dat de hoofdlettergevoelige query een tekenreekswaarde verschilt. Klik op het tandwiel **Test** opnieuw om de query uitvoert. De query moet 389 rijen uit 1860 gebeurtenissen worden geretourneerd.

![Tweede uitvoer resultaten van de query testen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Query: Een melding voor het starten van een werkstroom voor bedrijven

Laten we onze query meer gedetailleerde laten. Voor elk type sensor willen we gemiddelde temperatuur per 30 seconden venster bewaken en resultaten weer te geven alleen als de gemiddelde temperatuur hoger dan 100 graden is. We schrijven van de volgende query en klik vervolgens op **testen** om de resultaten te zien. De query is in het bestand ThresholdAlerting.txt.

![30 seconden filterquery](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

U ziet nu resultaten met alleen 245 rijen en de namen van sensoren waar het gemiddelde gematigde klimaatzone is groter dan 100. Deze query de stroom van gebeurtenissen gegroepeerd op **dspl**, dat wil de naam van de sensor over een **Venster Tumbling** van 30 seconden zeggen. Tijdelijke query's moeten worden vermeld hoe we tijd aan de voortgang wilt. Met behulp van de **Tijdstempel BY** -component, hebben we de kolom **OUTPUTTIME** tijden om aan te koppelen alle tijdelijke berekeningen opgegeven. Lees de MSDN-artikelen over [Tijd te beheren](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing functies](https://msdn.microsoft.com/library/azure/dn835019.aspx)voor gedetailleerde informatie.

### <a name="query-detect-absence-of-events"></a>Query: Detecteren gebrek aan gebeurtenissen

Hoe kunnen we een query voor een gebrek aan invoer gebeurtenissen schrijven? Zoek de laatste keer dat we dat een sensor gegevens verzonden en vervolgens niet heeft verzonden gebeurtenissen voor de volgende minuut. De query is in het bestand AbsenseOfEvent.txt.

![Gebrek aan gebeurtenissen detecteren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

We gebruiken hier een **LEFT OUTER** join aan de dezelfde gegevensstroom (self join). Voor een **INNER** join, wordt een resultaat gegeven alleen wanneer een overeenkomst wordt gevonden  Voor een **LEFT OUTER** join, als een gebeurtenis vanaf de linkerkant van de join definieert niet-gerelateerde records is, een rij met Null-waarden voor alle kolommen van de rechterkant geretourneerd. Deze techniek is bijzonder nuttig zijn om te zoeken ontbreken van gebeurtenissen. Zie de documentatie van onze MSDN voor meer informatie over het [deelnemen aan](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Sluiten

Het doel van deze zelfstudie is om te laten zien hoe u verschillende querytaal voor Stream Analytics-query's schrijft en de resultaten bekijken in de browser. Echter dit is alleen aan de slag. U kunt nog veel meer met Stream analyses uitvoeren. Stream Analytics ondersteunt een groot aantal ingangen en Hiermee kunt u kunt zelfs functies gebruiken in Azure Machine Learning zodat u deze een robuuste hulpmiddel voor het analyseren van gegevensstromen. U kunt beginnen met meer over Stream Analytics verkennen met behulp van onze [zal het leren van map](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Lees het artikel over [algemene query patronen](./stream-analytics-stream-analytics-query-patterns.md)voor meer informatie over het schrijven van query's.
