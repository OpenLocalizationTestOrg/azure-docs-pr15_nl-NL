<properties
    pageTitle="Stream Analytics: Realtime fraude detectie | Microsoft Azure"
    description="Informatie over het maken van een oplossing van de detectie realtime fraude met Stream Analytics. Een gebeurtenis hub voor realtime gebeurtenis verwerking gebruiken."
    keywords="afwijking detectie, detectie van fraude, realtime afwijking detectie"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Aan de slag met Azure Stream Analytics: realtime fraude detectie

Informatie over het maken van een end-to-end-oplossing voor realtime fraude detectie met Azure Stream Analytics. Gebeurtenissen brengen naar Azure gebeurtenis Hubs, schrijven Stream Analytics-query's voor aggregatie of waarschuwingen en de resultaten verzenden naar een sink uitvoer naar inzicht krijgen over gegevens met realtime verwerking. Realtime afwijking detectie voor telecommunicatie wordt uitgelegd, maar de voorbeeld-techniek gelijkmatig is geschikt voor andere soorten fraude detectie zoals creditcard of identiteit diefstal-scenario's.

Stream Analytics is een volledig beheerde service waarmee met lage latentie, ten zeerste beschikbaar, scalable, complexe gebeurtenis verwerking via streaming van gegevens in de cloud. Zie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)voor meer informatie.


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telecommunicatie en SIM detectie van fraude in realtime

Een bedrijf telecommunicatie heeft een grote hoeveelheid gegevens voor binnenkomende oproepen. Het bedrijf heeft de volgende handelingen uit de gegevens nodig:

* Gegevens beperken tot een beheerbare bedrag en inzichten over het gebruik van de klant verkrijgen na verloop van tijd en tussen geografische regio's.
* Detecteren in realtime SIM fraude (meerdere oproepen van dezelfde identiteit rond tegelijk, maar in geografisch verschillende locaties) zodat het bedrijf eenvoudig reageren kan op de hoogte te stellen klanten of service afgesloten.

Canonieke Internet van dingen (IoT)-scenario's hebben een ton van telemetrielogboek of gegevens van sensoren. Klanten wilt af van de gegevens of waarschuwingen ontvangen over afwijkingen in realtime.

## <a name="prerequisites"></a>Vereisten voor

- [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) downloaden van het Microsoft Download Center
- Optioneel: Broncode van de gebeurtenis genereren van [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Azure gebeurtenis Hubs invoer- en consumenten groep maken

De toepassing van de steekproef worden gebeurtenissen genereren en deze op een exemplaar van de gebeurtenis Hubs voor realtime verwerking push. Service Bus gebeurtenis Hubs zijn de voorkeursmethode voor gebeurtenis opname van Stream analysegegevens. U kunt meer informatie over de gebeurtenis Hubs in [Azure Service Bus documentatie](/documentation/services/service-bus/).

De hub van een gebeurtenis maken:

1.  Klik in de [portal van Azure](https://manage.windowsazure.com/)op **Nieuw** > **APP SERVICES** > **SERVICE BUS** > **GEBEURTENIS HUB** > **Snelle maken**. Geef een naam, regio en nieuwe of bestaande naamruimte maken van een nieuwe gebeurtenis hub.  
2.  Als een goede gewoonte, moet elke taak Stream Analytics lezen uit een eenmalige gebeurtenis hub consumenten-groep. We begeleiden u bij het maken van een groep consumenten later. [Meer informatie over groepen consumenten](https://msdn.microsoft.com/library/azure/dn836025.aspx). Als u wilt een groep consumenten hebt gemaakt, gaat u de zojuist gemaakte gebeurtenis-hub, klik op het tabblad **Consumenten groepen** , klikt u op **maken** vanaf de onderkant van de pagina en geef een naam voor uw groep consumenten.
3.  Als u wilt verlenen van toegang tot de gebeurtenis-hub, moeten we een gedeelde-beleid maken. Klik op het tabblad **configureren** van uw hub-gebeurtenis.
4.  Klik onder **Gedeeld-beleid**maken van nieuw beleid met machtigingen **beheren** .

    ![Gedeeld Clienttoegangsbeleid waar u een beleid met machtigingen beheren maken kunt.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Klik op **Opslaan** onder aan de pagina.
6.  Ga naar het **Dashboard**op onder aan de pagina **VERBINDINGSGEGEVENS** en vervolgens kopiëren en opslaan van de verbindingsgegevens.

## <a name="configure-and-start-the-event-generator-application"></a>Configureren en start u de gebeurtenis genereren-toepassing

We hebben een clienttoepassing die steekproef binnenkomende oproep metagegevens genereren en deze naar de gebeurtenis Hubs push opgegeven. Gebruik de volgende stappen voor het instellen van deze toepassing.  

1.  Download het [bestand TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)en pak dit naar een map.

    > [AZURE.NOTE] Windows blokkeert mogelijk het gedownloade zip-bestand. Met de rechtermuisknop op het bestand en selecteer **Eigenschappen**. Als u het bericht ziet 'dit bestand is afkomstig van een andere computer en mogelijk geblokkeerd ter bescherming van deze computer', schakelt u het **deblokkeren** en klik op toepassen op het zip-bestand.

2.  Vervang de waarden Microsoft.ServiceBus.ConnectionString en EventHubName in telcodatagen.exe.config door uw gebeurtenis hub-verbindingsreeks en de naam.

    De verbindingsreeks die u hebt gekopieerd in de portal van Azure plaatst de naam van de verbinding aan het einde. Zorg ervoor dat u wilt verwijderen "; EntityPath =<value>"van de ' sleutel toevoegen met =" veld.

3.  Start de toepassing. Het gebruik is als volgt:

    telcodatagen.exe [#NumCDRsPerHour] [SIM kaart fraude kans] [#DurationHours]

Het volgende voorbeeld wordt 1000 gebeurtenissen met een kans 20 procent van fraude genereren in de loop van de twee uur.

    telcodatagen.exe 1000 .2 2

Hier ziet u records naar de hub gebeurtenis is verzonden. Sommige sleutelvelden die we in deze toepassing van de detectie realtime fraude gebruiken worden hier gedefinieerd:

| Record | Definitie |
| ------------- | ------------- |
| CallrecTime | Begintijd van tijdstempel voor het gesprek. |
| SwitchNum | Telefoon schakeloptie gebruikt om verbinding maken met het gesprek. |
| CallingNum | Het telefoonnummer van de beller. |
| CallingIMSI | Internationale mobiele abonnee identiteit (IMSI).  Unieke id van de beller. |
| CalledNum | Het telefoonnummer van de ontvanger. |
| CalledIMSI | Internationale mobiele abonnee identiteit (IMSI).  Unieke id van de ontvanger. |


## <a name="create-a-stream-analytics-job"></a>Een taak Stream analyses maken
Nu dat we een reeks telecommunicatie gebeurtenissen hebben, kunnen we een taak Stream Analytics instellen zodat u kunt deze gebeurtenissen in realtime analyseren.

### <a name="provision-a-stream-analytics-job"></a>Een taak Stream Analytics inrichten

1.  Klik op **Nieuw**in de portal Azure > **GEGEVENSSERVICES** > **STREAM ANALYTICS** > **Snelle maken**.
2.  Opgeven van de volgende waarden en klik vervolgens op **STREAM ANALYTICS-taak maken**:

    * **TAAKNAAM**: Voer een taaknaam.

    * **Regio**: Selecteer het gebied waar u wilt uitvoeren van de taak. Houd rekening met de taak en de hub gebeurtenis plaatsen in dezelfde regio om ervoor te zorgen betere prestaties en om ervoor te zorgen dat u niet betaalt om gegevens te brengen tussen regio's.

    * **Opslag-ACCOUNT**: Kies het Azure opslag-account dat u gebruiken wilt voor de opslag van controlegegevens voor alle Stream Analytics-taken die in dit gebied worden uitgevoerd. U hebt de optie voor het kiezen van een bestaand opslag-account of een nieuw account te maken.

3.  Klik op **STREAM ANALYTICS** in het linkerdeelvenster voor een overzicht van de Stream Analytics-taken.

    ![Pictogram voor stream Analytics-service](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    De nieuwe taak wordt weergegeven met de status van **gemaakt**. Zoals u ziet dat de knop **START** vanaf de onderkant van de pagina is uitgeschakeld. Moet u de taak invoer, uitvoer en query voordat u kunt de taak.

### <a name="specify-job-input"></a>Taak invoer opgeven
1.  In uw project Stream Analytics **INVOERITEMS** boven aan de pagina op en klik vervolgens op **Invoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u bij de verschillende stappen voor het instellen van uw invoer.
2.  **GEGEVENSSTROOM**op en klik vervolgens op de juiste knop.
3.  **HUB voor de GEBEURTENIS**op en klik vervolgens op de juiste knop.
4.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **Invoer-ALIAS**: Voer een beschrijvende naam, zoals *CallStream*, voor deze taak. Houd er rekening mee dat u deze naam in de query later gebruikt.

    * **GEBEURTENIS-HUB**: als de gebeurtenis hub die u hebt gemaakt, hetzelfde als de taak Stream Analytics-abonnement is, selecteert u de naamruimte waarop de gebeurtenis hub zich bevindt.

        Als uw hub gebeurtenis zich in een ander abonnement, selecteer **Gebruik gebeurtenis Hub uit een ander abonnement**en voert u de gegevens voor **SERVICE BUS NAAMRUIMTE**, **De naam van de GEBEURTENIS-HUB**, **GEBEURTENIS HUB BELEIDSNAAM**, **GEBEURTENIS HUB BELEIDSSLEUTEL**en **Aantal van GEBEURTENIS HUB PARTITIES**handmatig.

    * **De naam van de HUB GEBEURTENIS**: Selecteer de naam van de hub gebeurtenis.

    * **De naam van een GEBEURTENIS HUB-beleid**: Selecteer de gebeurtenis hub-beleid dat u eerder in deze zelfstudie hebt gemaakt.

    * **GEBEURTENIS HUB consumenten groep**: Typ de naam van de groep voor consumenten die u eerder in deze zelfstudie hebt gemaakt.

5.  Klik op de juiste knop.
6.  Geef de volgende waarden:

    * **GEBEURTENIS SERIALISATIEFUNCTIE OPMAKEN**: JSON
    * **Codering**: UTF8
7.  Klik op de knop **controleren** en deze bron en om te bevestigen dat Stream Analytics verbinding met de hub gebeurtenis maken kunt.

### <a name="specify-job-query"></a>Taak-query opgeven

Stream Analytics ondersteunt een eenvoudige, declaratieve query-model dat transformaties voor realtime verwerking wordt beschreven. Meer informatie over de taal, raadpleegt u [Query Naslaggids voor Azure Stream Analytics](https://msdn.microsoft.com/library/dn834998.aspx). Deze zelfstudie kunt u de auteur en testen van meerdere query's via uw realtime gegevensstroom gesprek.

#### <a name="optional-sample-input-data"></a>Optioneel: Invoer voorbeeldgegevens
Valideren van uw query op gegevens van de werkelijke hoeveelheid werk, kunt u de functie **VOORBEELDGEGEVENS** gebeurtenissen extraheren van de stream en maak een. JSON-bestand van de gebeurtenissen voor het testen.  De volgende stappen hoe u dit wilt doen. We hebben ook een voorbeeldbestand [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) voor testdoeleinden opgegeven.

1.  Selecteer de gebeurtenis hub invoer en klik op **VOORBEELDGEGEVENS** onder aan de pagina.
2.  In het dialoogvenster dat wordt geopend, Geef een **BEGINTIJD** om te beginnen met het verzamelen van gegevens en een **duur** van de hoeveelheid extra gegevens om te gebruiken.
3.  Klik op de knop **controleren** om te beginnen steekproeven gegevens van de invoer.  Dit kan een paar minuten duren voor het gegevensbestand moet worden geproduceerd.  Wanneer het proces is voltooid, klikt u op **DETAILS**, de gegenereerde downloaden. JSON bestand en op te slaan.

    ![Download en sla verwerkte gegevens in een JSON-bestand](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Pass Through-query

Als u archiveren van elke gebeurtenis wilt, kunt u een Pass Through-query gebruiken om te lezen van alle velden in de nettolading van de gebeurtenis of het bericht. Als u wilt starten, doet u een eenvoudige Pass Through-query die projecten alle velden in een gebeurtenis.

1.  Klik op **QUERY** vanaf de bovenkant van de pagina van de taak Stream Analytics.
2.  Voeg de volgende naar de code-editor:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Zorg ervoor dat de naam van de invoerbron overeenkomt met de naam van de invoer die u eerder hebt opgegeven.

3.  Klik op **testen** onder de query-editor.
4.  Geef een testbestand. Gebruik een die u hebt gemaakt met behulp van de voorgaande stappen, of [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Klik op de knop **controleren** en de resultaten weergegeven onder de definitie van de query weer te geven.

    ![Definitie van queryresultaten](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Kolom raming

Nu verlagen we de geretourneerde velden naar een kleinere set.

1.  De query in de editor om te wijzigen:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Uitvoer in query-editor.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Aantal inkomende oproepen per regio: gewor-venster met aggregatie

Als u wilt vergelijken het aantal binnenkomende oproepen per regio, gebruiken we een [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) om het aantal binnenkomende gesprekken die zijn gegroepeerd op **SwitchNum** elke vijf seconden.

1.  De query in de editor om te wijzigen:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Het trefwoord **Tijdstempel door** deze query gebruikt om op te geven van een tijdstempelveld in de nettolading moet worden gebruikt in de tijdelijke berekening. Als u dit veld is niet opgeeft, de bewerking windowing uitgevoerd met behulp van de tijd die elke gebeurtenis in de hub gebeurtenis ontvangen. Zie ["Ontvangst van tijd tegenover toepassing Time" in de Stream Analytics Query Naslaggids](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Houd er rekening mee dat u toegang hebben tot een tijdstempel op voor het einde van elke venster met behulp van de eigenschap **System.Timestamp** .

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Queryresultaten voor Timestand door](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM fraude detectie met Self-joins

Als u wilt identificeren frauduleuze gebruik, zullen we voor oproepen die afkomstig van dezelfde gebruiker maar op verschillende locaties in minder dan 5 seconden zijn.  We [deelnemen aan](https://msdn.microsoft.com/library/azure/dn835026.aspx) de stroom van gesprek gebeurtenissen met zichzelf om deze zaken te controleren.

1.  De query in de editor om te wijzigen:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Queryresultaten van een join](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Uitvoer sink maken

Nu dat we de stroom van een gebeurtenis, een gebeurtenis hub invoer voor gebeurtenissen en van een query wilt uitvoeren van een transformatie via de stream, nemen hebt gedefinieerd is de laatste stap het definiëren van een sink uitvoer voor de taak. We wordt gebeurtenissen voor frauduleuze gedrag schrijven met Azure-blobopslag.

Gebruik de volgende stappen uit om te maken van een container voor Blob storage als u er nog geen hebt.

1.  Gebruik van een bestaand opslag-account of maak een nieuw account voor de opslag door te klikken op **Nieuw > GEGEVENSSERVICES > opslag > snelle maken**, en volg de instructies.
2.  Selecteer het account opslagruimte op **CONTAINERS** boven aan de pagina en klik vervolgens op **toevoegen**.
3.  Geef een **naam** voor de container en de **toegang** ingesteld op **Openbare Blob**.

## <a name="specify-job-output"></a>Taak-uitvoer opgeven

1.  In uw project Stream Analytics **uitvoer** boven aan de pagina op en klik vervolgens op **Uitvoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u bij de verschillende stappen voor het instellen van uw uitvoer.
2.  **BLOB STORAGE**op en klik vervolgens op de juiste knop.
3.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **UITVOERALIAS**: Voer een beschrijvende naam voor de uitvoer van deze taak.
    * **Abonnement**: als de blobopslag die u hebt gemaakt in hetzelfde als de taak Stream Analytics-abonnement is, klikt u op **Gebruik opslag-Account vanaf huidige abonnement**. Als uw opslag in een ander abonnement, klikt u op **Gebruik opslag Account uit een ander abonnement**en gegevens voor **Opslag-ACCOUNT**, **Opslag ACCOUNTSLEUTEL**en **CONTAINER**handmatig invoeren.
    * **Opslag-ACCOUNT**: Selecteer de naam van het account opslag.
    * **CONTAINER**: Selecteer de naam van de container.
    * **Bestandsnaam VOORVOEGSEL**: Typ een Bestandsvoorvoegsel gebruiken bij het schrijven van blob uitvoer.

4.  Klik op de juiste knop.
5.  Geef de volgende waarden:

    * **GEBEURTENIS SERIALISATIEFUNCTIE OPMAKEN**: JSON
    * **Codering**: UTF8

6.  Klik op de knop **controleren** en deze bron en om te bevestigen dat Stream Analytics succes verbinding met het account opslag maken kunt.

## <a name="start-job-for-real-time-processing"></a>Taak voor realtime verwerking starten

Omdat een taak invoer, query, en uitvoer alle zijn opgegeven, zijn wij de Stream Analytics-taak voor realtime fraude detectie gaan.

1.  Klik op de taak **DASHBOARD**, op **starten** onder aan de pagina.
2.  In het dialoogvenster dat wordt geopend, klikt u op **taak BEGINTIJD**en klik vervolgens op de knop **controleren** op de onderkant van het dialoogvenster. De taakstatus wordt gewijzigd in **starten** en kort **uitgevoerd**wordt gewijzigd.

## <a name="view-fraud-detection-output"></a>Weergave fraude detectie uitvoer

Gebruik een hulpmiddel zoals [Azure opslag Explorer](http://storageexplorer.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) frauduleuze gebeurtenissen weergeven terwijl ze naar uw uitvoer realtime worden geschreven.  

![Detectie van fraude: frauduleuze gebeurtenissen bekeken in realtime](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Ondersteuning
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
