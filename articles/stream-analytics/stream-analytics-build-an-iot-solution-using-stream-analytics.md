<properties
    pageTitle="Een oplossing IoT maken met behulp van de Stream Analytics | Microsoft Azure"
    description="Introductie zelfstudie voor de Stream Analytics IoT oplossing van een stencil-scenario"
    keywords="IOT oplossing, venster functies"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Een oplossing IoT maken met behulp van de Stream Analytics

## <a name="introduction"></a>Inleiding

In deze zelfstudie leert u hoe u inzicht krijgen in realtime vinden in uw gegevens met Azure Stream Analytics. Ontwikkelaars combineren eenvoudig streams van gegevens, zoals de klik-streams, logboeken en gebeurtenissen die door apparaat gegenereerd, met historische records of referentiegegevens bedrijven inzichten afleiden. Als een service van de stream volledig beheerde, realtime berekening die wordt gehost in Microsoft Azure, biedt Azure Stream Analytics ingebouwde tolerantie, lage latentie en schaalbaarheid om u omhoog en worden uitgevoerd in minuten.

Na het voltooien van deze zelfstudie, wordt u zichtbaar mogen zijn:

-   Vertrouwd raken met de portal Azure Stream Analytics.
-   Configureer en implementeren van een streaming taak.
-   Kunt verwoorden echte problemen en ze oplossen door de querytaal Stream Analytics.
-   Ontwikkel streaming oplossingen voor uw klanten met behulp van de Stream Analytics probleemloos.
-   Gebruik de controle en registratie ervaring oplossen van problemen.

## <a name="prerequisites"></a>Vereisten voor

U moet de volgende vereisten voor het voltooien van deze zelfstudie:

-   De nieuwste versie van [Azure PowerShell](../powershell-install-configure.md)
-   Visual Studio-2015 of de gratis [Visual Studio-Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   Een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Beheerdersbevoegdheden op de computer
-   Download van [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) van het Microsoft Download Center
-   Optioneel: Broncode voor het genereren van de gebeurtenis TollApp in [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scenario Inleiding: "Hallo, betaalde!"


Een station betaalde is een algemene verschijnsel. Tijdens deze veel autowegen, bruggen en tunnels kant van de wereld zitten. Elk station betaalde heeft meerdere betaalde stands. U stopt bij handmatige stands, om te betalen het betaalde naar een attendant. Een sensor boven aan elke booth gescand bij geautomatiseerde stands, een RFID-kaart die wordt aangebracht op de voorruit van uw voertuig terwijl u de stand betaalde doorgeven. Het is makkelijk de overgang door middel van deze stations betaalde als de stroom van een gebeurtenis waarover interessante bewerkingen kunnen worden uitgevoerd.

![Afbeelding van auto's op betaalde stands](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Binnenkomende gegevens

Deze zelfstudie werkt met twee stromen van gegevens. Sensoren geïnstalleerd in het begin en eind van de stations betaalde produceren de eerste stream. De tweede stream is een statische opzoekgegevensset registratiegegevens voertuig met.

### <a name="entry-data-stream"></a>Gegevensstroom-fragment

De gegevensstroom vermelding bevat informatie over auto's, zoals ze betaalde stations invoeren.


| TollID | EntryTime               | LicensePlate | De staat | Tabelmaakquery   | Model   | VehicleType | VehicleWeight | Betaalde | Tag       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 2014-09-10 12:01:00.000 | JNB 7001     | ZA    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 2014-09-10 12:02:00.000 | YXZ 1001     | ZA    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 2014-09-10 12:02:00.000 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 2014-09-10 12:03:00.000 | XYZ 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 2014-09-10 12:03:00.000 | BNJ 1007     | ZA    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 2014-09-10 12:05:00.000 | COB 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Hier volgt een korte beschrijving van de kolommen:

| Kolom | Beschrijving |
|--------------|--------------------------------------------------------------------|
| TollID       | Het betaalde booth-ID, waarmee deze identificeert op een betaalde stand            |
| EntryTime    | De datum en tijd van de invoer van het voertuig naar de stand betaalde in UTC |
| LicensePlate | De sofi-nummer van het voertuig                            |
| De staat        | Een staat in de Verenigde Staten                                           |
| Tabelmaakquery         | De fabrikant van de auto                                 |
| Model        | Het getal model van de auto                                 |
| VehicleType  | 1 voor personenvoertuigen of 2 voor commerciële voertuigen       |
| WeightType   | Voertuig gewicht in ton; 0 voor personenvoertuigen                   |
| Betaalde         | De waarde betaalde in USD                                              |
| Tag          | De e-code op de auto waarmee betaling. lege waarde waar de te betalen handmatig werden geëffend |


### <a name="exit-data-stream"></a>Gegevensstroom afsluiten

De gegevensstroom afsluiten bevat informatie over auto's het station betaalde verlaten.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | COB 1007         |

Hier volgt een korte beschrijving van de kolommen:


| Kolom | Beschrijving |
|--------------|-----------------------------------------------------------------|
| TollID       | Het betaalde booth-ID, waarmee deze identificeert op een betaalde stand         |
| ExitTime     | De datum en tijd van afsluiten van het voertuig van betaalde booth in UTC |
| LicensePlate | De sofi-nummer van het voertuig                         |

### <a name="commercial-vehicle-registration-data"></a>Registratiegegevens commercieel voertuig

De zelfstudie gebruikt een momentopname van een commercieel voertuig registratie-database.


| LicensePlate | Registratie-id | Verlopen |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| A 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Hier volgt een korte beschrijving van de kolommen:


| Kolom | Beschrijving |
|--------------|-----------------------------------------------------------------|
| LicensePlate | De sofi-nummer van het voertuig                         |
| Registratie-id     | Van het voertuig registratie-ID |
| Verlopen | De registratiestatus van het voertuig: 0 als voertuigregistratie actief is, 1 als de registratie is verlopen                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Stel de omgeving van Azure Stream analysegegevens

Als u wilt deze zelfstudie hebt voltooid, moet u een Microsoft Azure-abonnement. Microsoft biedt gratis proefversie voor Microsoft Azure-services.

Als u niet een Azure-account hebt, kunt u [verzoek om een gratis proefversie](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Om u te registreren voor een gratis proefversie, moet u een mobiel apparaat waarop u SMS-berichten en een geldige creditcard kunt ontvangen.

Zorg ervoor dat u de stappen in het gedeelte "Uw Azure-account opschonen" aan het einde van dit artikel zodat u kunt het beste gebruik van uw 200 euro vrij te geven Azure creditcard.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Azure resources die zijn vereist voor de zelfstudie inrichten

Deze zelfstudie is vereist twee gebeurtenis hubs *invoer* en *Afsluiten* streams van gegevens te ontvangen. Azure SQL-Database Hiermee kunt u de resultaten van de Stream Analytics-taken. Azure opslag verwijzingsgegevens over een voertuig registraties opgeslagen.

U kunt het script Setup.ps1 in de map TollApp op GitHub maken van alle vereiste resources. In het belang van de tijd, is het raadzaam dat u het uitvoert. Als u meer informatie over het configureren van deze resources in de portal van Azure wilt, raadpleegt u naar de bijlage "Configureert Zelfstudievideo resources in Azure portal".

Download en sla de map met ondersteunende [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) en bestanden.

Open een **Microsoft Azure PowerShell** -venster _als een beheerder_. Als u nog niet Azure PowerShell hebt, volgt u de instructies in [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) om het te installeren.

Omdat Windows wordt automatisch geblokkeerd .ps1 dll-bestanden, en .exe, moet u voor het instellen van het beleid kan worden uitgevoerd voordat u het script uitvoeren. Controleer of dat het Azure PowerShell-venster wordt uitgevoerd _als een beheerder_. **Set-uitvoeringsbeleid onbeperkte**worden uitgevoerd. Wanneer u wordt gevraagd, typt u **Y**.

![Schermafbeelding van "Set-uitvoeringsbeleid onbeperkte" venster met Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Voer **Get-ExecutionPolicy** om ervoor te zorgen dat de opdracht heeft gewerkt.

![Schermafbeelding van "Get-ExecutionPolicy" met in Azure PowerShell-venster](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Ga naar de map met de scripts en genereren-toepassing.

![Schermafbeelding van 'cd .\TollApp\TollApp"met in het venster Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Type **.\\ Setup.ps1** als u wilt uw Azure-account hebt ingesteld, maken en configureren van alle vereiste middelen en begin met het genereren van gebeurtenissen. Het script hervat willekeurig een gebied uw resources maken. Als u wilt een gebied expliciet opgeeft, kunt u doorgeven de **-locatie** parameter zoals in het volgende voorbeeld:

**. \\Setup.ps1-locatie "Central US"**

![Schermafbeelding van de Azure aanmeldingspagina](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Het script opent de pagina **Aanmelden** voor Microsoft Azure. Voer de referenties van uw account.

> [AZURE.NOTE] Als uw account toegang tot meerdere abonnementen heeft, wordt u gevraagd de naam van het abonnement dat u wilt gebruiken voor de zelfstudie invoeren.

Het script kan enkele minuten duren om uit te voeren. Nadat deze is voltooid, wordt de uitvoer er als de volgende schermafbeelding.

![Schermafbeelding van de uitvoer van het script in het venster Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

U ziet ook een ander venster dat lijkt op de volgende schermafbeelding. Deze toepassing verstuurt gebeurtenissen naar Azure gebeurtenis Hubs, die is vereist voor het uitvoeren van de zelfstudie. Ja, niet de toepassing stoppen of sluit u dit venster totdat u klaar bent met de zelfstudie.

![Schermafbeelding van 'Verzenden gebeurtenis hub-gegevens'](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

U moet mogelijk om alle gemaakte resources in Azure-portal nu weer te geven. Ga naar <https://manage.windowsazure.com>en meld u aan met referenties van uw account.

### <a name="azure-event-hubs"></a>Azure gebeurtenis Hubs

Klik op **SERVICE-BUS** aan de linkerkant van de Azure-portal als u wilt zien van de gebeurtenis hubs die het script in de vorige sectie hebt gemaakt.

![Service Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Alle beschikbare naamruimten ziet u uw abonnement. Klik op de fase die met *tolldata begint* (tolldata4637388511 in ons voorbeeld). Klik op het tabblad **GEBEURTENIS HUBS** .

![Tabblad van de gebeurtenis Hubs in de portal van Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

U ziet twee gebeurtenis hubs benoemd *item* en *Afsluiten* in deze naamruimte hebt gemaakt.

![Schermafbeelding van "invoer" en "afsluiten" gebeurtenis hubs in Azure-portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure opslag container

1. Klik op **opslagruimte** aan de linkerkant van de Azure-portal als u wilt zien van de container Azure Storage die wordt gebruikt in de zelfstudie.

    ![Opslag menu-item](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Klik op de computer die beginnen met *tolldata* (tolldata4637388511 in ons voorbeeld). Klik op het tabblad **CONTAINERS** om te zien van de gemaakte container.

    ![Containers tabblad in de portal van Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Klik op de container **tolldata** als u wilt zien van de geüploade JSON-bestand dat een voertuig registratiegegevens heeft.

    ![Schermafbeelding van het bestand registration.json in de container](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Azure SQL-Database

1. Klik op **SQL-DATABASES** aan de linkerkant van de Azure-portal als u wilt zien van de SQL-database die wordt gebruikt in deze zelfstudie.

    ![SQL-Databases](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Klik op **tolldatadb**.

    ![Schermafbeelding van de gemaakte SQL-database](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. De naam van de server zonder het poortnummer kopiëren (*servernaam*. database.windows.net, bijvoorbeeld).

## <a name="connect-to-the-database-from-visual-studio"></a>Verbinding maken met de database van Visual Studio

Gebruik Visual Studio voor toegang tot queryresultaten in de Uitvoerdatabase.

Verbinding maken met de SQL-database (de bestemming) van Visual Studio:

1. Open Visual Studio, en klik op **Hulpmiddelen voor** > **verbinding maken met de Database**.

2. Als wordt gevraagd, klikt u op **Microsoft SQL Server** als gegevensbron.

    ![Dialoogvenster voor gegevensbron wijzigen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. Plak in het veld **servernaam** de naam die u hebt gekopieerd in de vorige sectie van de Azure-portal (dat wil zeggen *servernaam*. database.windows.net).

4. Klik op **Gebruik SQL Server-verificatie**.

5. **Tolladmin** invoeren in het veld **gebruikersnaam in te voeren** en **123toll!** in het veld **wachtwoord** .

6. **Selecteer of typ de databasenaam van een**op en selecteer **TollDataDB** als de database.

    ![Dialoogvenster verbinding toevoegen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Klik op **OK**.

8. Open de Verkenner Server.

    ![Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Zie vier tabellen in de database TollDataDB.

    ![Tabellen in de database TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Gebeurtenis genereren: TollApp voorbeeld van project

De PowerShell-script worden automatisch gestart gebeurtenissen verzenden met behulp van de toepassing van TollApp voorbeeld. U hoeft niet te eventuele extra stappen uitvoeren.

Als u geïnteresseerd in uitvoering details bent, kunt u de broncode van de toepassing TollApp vinden in GitHub [Voorbeelden/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Schermafbeelding van de steekproef code weergegeven in Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Een taak Stream analyses maken

1. Klik in de portal Azure open Stream Analytics en klikt u op **Nieuw** in de linkerbenedenhoek van de pagina een nieuwe analytics-taak maken.

    ![De knop Nieuw](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Klik op **snelle maken**. Selecteer de dezelfde regio waar uw andere resources die zijn gemaakt door het script.

3. Selecteer **Nieuwe opslag-ACCOUNT maken** en geef deze een unieke naam voor de instelling **Regionale MONITORING opslag-ACCOUNT** . Azure Stream Analytics wordt dit account gebruiken om op te slaan controlegegevens voor alle toekomstige projecten.

4. Klik op **Taak voor STREAM analyses maken** onder aan de pagina.

    ![De optie Stream Analytics taak maken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Invoerbronnen definiëren

1. Klik op de gemaakte analytics-taak in de portal.

    ![Schermafbeelding van de analytics-taak in de portal](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Klik op het tabblad **invoer** als u wilt de brongegevens te definiëren.

    ![Het tabblad invoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Klik op **een INVOERTAAL toevoegen**.

    ![Het toevoegen van een optie invoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Klik op **GEGEVENSSTROOM** op de eerste pagina.

    ![De optie gegevensstroom](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Klik op **GEBEURTENIS HUB** op de tweede pagina van de wizard.

    ![De optie gebeurtenis Hub](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Voer **EntryStream** als **invoer ALIAS**.

7. Klik op de **Gebeurtenis Hub** vervolgkeuzelijst en selecteer de fase die met 'TollData' (bijvoorbeeld TollData9518658221 begint).

8. Selecteer **item** als de naam voor de gebeurtenis hub en **alle** als de naam van de gebeurtenis hub-beleid.

    Uw instellingen eruitziet:

    ![Gebeurtenis-hub-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Selecteer op de volgende pagina **JSON** voor **GEBEURTENIS SERIALISATIE-indeling** en **UTF8** voor **codering**.

    ![Serialisatie-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Klik op **OK** onder aan de pagina om de wizard te voltooien.

    ![Schermafbeelding van EntryStream invoer in de portal van Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Nu dat u de vermelding stream hebt gemaakt, wordt u Volg dezelfde stappen als u wilt maken van de stream afsluiten. Klik op de derde pagina van de wizard, zorg ervoor dat u waarden als invoeren op de volgende schermafbeelding.

    ![Instellingen voor de stream afsluiten](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    U hebt twee invoer streams gedefinieerd:

    ![Invoer-streams gedefinieerd in de portal van Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Vervolgens gaat toevoegen u invoer van de verwijzing gegevens voor het bestand blob met Auto registratiegegevens.

11. **INVOERTAAL toevoegen**op en klik vervolgens op **REFERENTIEGEGEVENS**.

    !["Toevoegen een invoer" opties met de gegevens van de verwijzing is geselecteerd](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Selecteer op de volgende pagina, het opslag-account die met **tolldata begint**. De containernaam van de moet **tolldata**en de naam van de blob onder **Pad PATROON** moet **registration.json**. Deze bestandsnaam is hoofdlettergevoelig en moet kleine letters.

    ![Blog-opslag-instellingen](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Selecteer de waarden zoals weergegeven in de volgende schermafbeelding op de volgende pagina en klik vervolgens op **OK** om de wizard te voltooien.

    ![Selectie van JSON voor "Even serialiseringsindeling" en UTF8 voor "Codering"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Nu zijn alle invoer gedefinieerd.

![Schermafbeelding van de drie gedefinieerde invoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Uitvoer definiëren

1. Klik op het tabblad **uitvoer** en klik op **Een uitvoer toevoegen**.

    ![De uitvoer tabblad en de optie "Toevoegen een uitvoer"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Klik op **SQL-Database**.

3. Selecteer de naam van de server die is gebruikt in de sectie 'Verbinden aan Database uit Visual Studio' in dit artikel. De naam van de database is **TollDataDB**.

4. **Tolladmin** invoeren in het veld **gebruikersnaam** **123toll!** in het veld voor **wachtwoord** en **TollDataRefJoin** in **het tabelveld** .

    ![Instellingen van de SQL-Database](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics-query

Het tabblad **QUERY** bevat een SQL-query waarmee de binnenkomende gegevens worden omgezet.

![Een query die is toegevoegd aan het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Deze zelfstudie probeert om te beantwoorden van verschillende zakelijke vragen die betrekking hebben op betaald gegevens en constructies Stream gebruiksanalyses query's die kunnen worden gebruikt in Azure Stream Analytics een relevante antwoord te geven.

Voordat u uw eerste taak van de Stream analyses, laten we een paar scenario's en de querysyntaxis van de verkennen.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Inleiding tot de querytaal Azure Stream Analytics
-----------------------------------------------------

Stel dat u moet het aantal voertuigen die een booth betaalde invoert. Omdat dit een continue stroom van gebeurtenissen, moet u een 'tijdsperiode."definiëren Laten we wijzigen de vraag om te worden "hoeveel voertuigen Voer een booth betaalde elke drie minuten?". Dit is gewoonlijk de telling gewor genoemd.

Bekijk de Azure Stream Analytics-query die vindt u antwoorden op deze vraag:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Zoals u ziet, geeft Azure Stream Analytics een querytaal die lijkt op SQL en voegt een paar extensies om op te geven van de tijd aspecten van de query wordt gebruikt.

Lees meer over [Tijd te beheren](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructies gebruikt in de query uit MSDN voor meer informatie.

## <a name="testing-azure-stream-analytics-queries"></a>Testen van Azure Stream gebruiksanalyses query 's

Nu u uw eerste Azure Stream Analytics-query hebt geschreven, is het tijd om deze te testen met behulp van de steekproef gegevensbestanden in uw map TollApp in het volgende pad:

**.. \\TollApp\\TollApp\\gegevens**

Deze map bevat de volgende bestanden:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Vraag 1: Aantal voertuigen een booth betaalde invoeren

1. Open de Azure-portal en Ga naar uw gemaakte Azure Stream Analytics-taak. Klik op het tabblad **QUERY** en plak de query uit de vorige sectie.

    ![Query geplakt in het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Klik op de knop **testen** om te valideren deze query ten opzichte van voorbeeldgegevens. In het dialoogvenster dat wordt geopend, gaat u naar Entry.json, een bestand met voorbeeldgegevens uit de stroom van de gebeurtenis **EntryTime** .

    ![Schermafbeelding van het bestand Entry.json](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Controleren of de uitvoer van de query zoals verwacht:

    ![Resultaten van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Vraag 2: De totale tijd voor elk auto te bladeren door de stand betaalde rapporteren

De gemiddelde tijd die nodig is voor een auto door het betaalde kunt u inzicht in de efficiëntie van het proces en de gebruikerservaring.

Als u wilt de totale tijd hebt gevonden, moet u deelnemen aan de stream EntryTime met de stream ExitTime. U kunt de streams op kolommen TollId en LicencePlate wordt deelnemen. De operator **deelnemen** , moet u opgeven tijdelijke eenheidsprofiel waarmee het aanvaardbaar tijdverschil tussen de gekoppelde gebeurtenissen beschreven. U kunt **DATEDIFF** -functie opgeven dat gebeurtenissen niet meer dan 15 minuten van elkaar moeten zijn. U wordt ook de **DATEDIFF** -functie om af te sluiten en -tijd van de invoer te berekenen van de werkelijke tijd waarop een auto in het station betaalde besteedt toepassen. Houd rekening met het verschil tussen het gebruik van **DATEDIFF** wanneer dit wordt gebruikt in plaats van een voorwaarde **deelnemen aan** een **SELECT** -instructie.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Als u wilt deze query testen, de bijwerkquery op het tabblad **QUERY** van uw taak uit te voeren:

    ![Bijgewerkte query op het tabblad Query](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Klik op **testen** en invoer voorbeeldbestanden voor EntryTime en ExitTime opgeven.

    ![Schermafbeelding van de geselecteerde bestanden van de invoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Schakel het selectievakje in de query testen en weergeven van de uitvoer:

    ![Uitvoer van de test](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Vraag 3: Alle commercieel voertuigen met vervallen registratie rapporteren

Azure Stream Analytics kunt deelnemen met tijdelijke gegevensstromen via statische momentopnamen van gegevens. Voorbeelden van deze mogelijkheid, gebruikt u het volgende Voorbeeldvraag.

Als u een commercieel voertuig is geregistreerd bij het bedrijf betaalde, wordt doorgegeven via de stand betaalde zonder het worden gestopt voor controle. U kunt commercieel voertuigregistratie opzoektabel alle commercieel voertuigen die zijn verlopen registraties identificeren.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Als u wilt een query testen met behulp van referentiegegevens, moet u een invoerbron voor de verwijzingsgegevens, dat u al hebt gedaan definiëren.

1. Als u wilt deze query testen, plak de query in het tabblad **QUERY** op **testen**en de twee invoerbronnen opgeven:

    ![Schermafbeelding van de geselecteerde bestanden van de invoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. De uitvoer van de query weergeven:

    ![Schermafbeelding van de queryuitvoer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Start de Stream Analytics-taak


Nu is het tijd om te stoppen met de configuratie en start de taak. Opslaan de query uit vraag 3, zullen die uitvoer die overeenkomt met het schema van de tabel van de uitvoer **TollDataRefJoin** produceren.

Ga naar de taak **DASHBOARD**, en klik op **START**.

![Schermafbeelding van de knop Start in de Project-dashboard](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

In het dialoogvenster dat wordt geopend, door de **Uitvoer starten** -tijd naar **Aangepaste tijd**te wijzigen. Wijzig het uur in één uur voor de huidige tijd. Deze wijziging zorgt ervoor dat alle gebeurtenissen in de hub gebeurtenis nadat u bent begonnen te genereren van de gebeurtenissen die aan het begin van de zelfstudie worden verwerkt. Klik nu op het vinkje om te starten van de taak.

![Selectie van aangepaste tijd](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

De taak begint, kan een paar minuten duren. Klik op de pagina op het hoogste niveau kunt u de status bekijken voor Stream Analytics.

![Schermafbeelding van de status van de taak](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Resultaten in Visual Studio

1. Open Visual Studio Server Explorer en met de rechtermuisknop op de tabel **TollDataRefJoin** .
2. Klik op **Gegevens in een tabel weergeven** om de uitvoer van uw project te bekijken.

    ![Selectie van "tabelgegevens weergeven' in Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Schaal van Azure Stream Analytics taken

Azure Stream Analytics is bedoeld om elastically schaal, zodat deze kan omgaan met een groot aantal gegevens. De query Azure Stream Analytics kunt een **PARTITION BY** -component gebruiken om te zien van het systeem dat deze stap wordt schaal af. **PartitionId** is een speciale kolom die in het systeem worden toegevoegd zodat deze overeenkomen met de ID op van de invoer (gebeurtenis hub).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. De huidige taak stoppen, bijwerken van de query in het tabblad **QUERY** op en open het tabblad **schaal** .

    **STREAMING eenheden** definiëren de hoeveelheid rekenkracht dat de taak kan ontvangen.

2. Verplaats de schuifregelaar tot en met 6.

    ![Schermafbeelding van het selecteren van 6 streaming eenheden](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Ga naar het tabblad **uitvoer van** en wijzig de naam van de SQL-tabel in **TollDataTumblingCountPartitioned**.

Als u de taak begint nu Azure Stream analyses kunt werk verdelen over meer berekeningscluster informatiebronnen en hogere doorvoersnelheid bereiken. Houd er rekening mee dat de toepassing TollApp ook gebeurtenissen partitioneren door TollId verzendt.

## <a name="monitor"></a>Monitor

Het tabblad **Beeldscherm** bevat statistieken over de actieve taak.

![Schermafbeelding van statistieken over het uitvoeren van taken](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

U kunt **Logboeken aan de bewerking** openen vanaf het tabblad **DASHBOARD** .

![De optie "Logboeken aan de bewerking"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Schermafbeelding van logboeken aan de bewerking waar u de status van taken kunt zien](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Als u wilt meer informatie over een bepaalde gebeurtenis wordt weergegeven, klikt u op de gebeurtenis en klik vervolgens op de knop **DETAILS** .

![Schermafbeelding van de details van een geselecteerde gebeurtenis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Sluiten

Deze zelfstudie kennis u met de Azure Stream Analytics-service. Deze gedemonstreerd hoe invoer en uitvoer voor de Stream Analytics-taak configureren. Met het scenario betaalde gegevens, beschreven de zelfstudie voorkomende problemen die ontstaan in de ruimte van gegevens in beweging en hoe ze kunnen worden opgelost met eenvoudige SQL-achtige-query's in Azure Stream Analytics. De zelfstudie beschreven SQL-extensie constructies voor het werken met tijdelijke gegevens. Zich gegevensstromen deelnemen aan, hoe u de gegevensstroom met statische referentiegegevens verrijken en hoe u de schaal van een query om te bereiken sneller worden verwerkt.

Hoewel deze zelfstudie bestaat uit een goede inleiding, is het niet voltooid door middel van. U vindt meer query patronen met de SAQL-taal voorbeelden van de [Query voor algemene Stream Analytics gebruikspatronen](stream-analytics-stream-analytics-query-patterns.md).
Raadpleeg de [onlinedocumentatie](https://azure.microsoft.com/documentation/services/stream-analytics/) voor meer informatie over Azure Stream Analytics.

## <a name="clean-up-your-azure-account"></a>Uw Azure-account opschonen

1. De Stream Analytics-taak in de portal van Azure stoppen.

    Het script Setup.ps1 twee gebeurtenis hubs en een SQL-database gemaakt. De volgende instructies helpen u aan het einde van de zelfstudie resources opruimen.

2. Typ in een venster PowerShell **.\\ CleanUp.ps1** om het script verwijderen van een resources die worden gebruikt in deze zelfstudie te starten.

    > [AZURE.NOTE] Resources worden aangeduid met de naam. Zorg ervoor dat u elk item zorgvuldig controleren voordat u bevestigt verwijderen.

    ![Schermafbeelding van het opruimproces](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
