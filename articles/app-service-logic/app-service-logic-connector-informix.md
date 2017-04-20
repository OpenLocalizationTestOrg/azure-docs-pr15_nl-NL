<properties
   pageTitle="De verbindingslijn Informix met Microsoft Azure App Service | Microsoft Azure"
   description="Het gebruik van de verbindingslijn Informix met logica app triggers en acties"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="informix-connector"></a>Informix-connector
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

Microsoft-connector voor Informix is een API-app voor toepassingen via Azure App Service verbinden resources die zijn opgeslagen in een IBM Informix-database. Connector bevat een Microsoft-Client verbinding maken met externe computers voor Informix-server via een netwerkverbinding TCP/IP, met inbegrip van Azure hybride verbindingen naar on-premises implementatie Informix-servers met behulp van de Azure Service Bus Relay. Connector ondersteunt de volgende databasebewerkingen:

- Rijen selecteren met lezen
- Peiling te lezen rijen met Selecteer tellen gevolgd door selecteren
- Één rij of meerdere (bulksgewijs) rijen invoegen met toevoegen
- Één rij of meerdere (bulksgewijs) rijen met UPDATE wijzigen
- Verwijderen van één rij of meerdere (bulksgewijs) rijen verwijderen gebruiken
- Alleen rijen met Selecteer CURSOR gevolgd door UPDATE waar huidige van CURSOR wijzigen
- Alleen rijen met Selecteer CURSOR gevolgd door UPDATE waar huidige van CURSOR verwijderen
- Procedure uitvoeren met invoer- en uitvoerbereik parameters, als resultaat, resultaatset, oproep gebruiken
- Aangepaste opdrachten en samengestelde bewerkingen met selecteren, invoegen, UPDATE, DELETE

## <a name="triggers-and-actions"></a>Triggers en acties
Connector ondersteunt de volgende logica app triggers en acties:

Triggers | Acties
--- | ---
<ul><li>Gegevens van de peiling</li></ul> | <ul><li>Bulksgewijs invoegen</li><li>Invoegen</li><li>Bulksgewijs bijwerken</li><li>Update</li><li>Bellen</li><li>Bulksgewijs verwijderen</li><li>Verwijderen</li><li>Selecteer</li><li>Voorwaardelijke bijwerken</li><li>Posten naar EntitySet</li><li>Voorwaardelijke verwijderen</li><li>Selecteer één entiteit</li><li>Verwijderen</li><li>Upsert naar EntitySet</li><li>Aangepaste opdrachten</li><li>Samengestelde bewerkingen</li></ul>


## <a name="create-the-informix-connector"></a>De verbindingslijn Informix maken
U kunt een verbindingslijn binnen een app logica of van Azure Marketplace, zoals definiëren in het volgende voorbeeld:  

1. Selecteer in de Azure startboard **Marketplace**.
2. **Informix** Typ in het vak **Alles zoeken** in het blad **Alles** en klik vervolgens op de enter-toets.
3. Ga in de zoekopdracht alles deelvenster resultaten, selecteer **Informix verbindingslijn**.
4. Selecteer in het blad Informix verbindingslijn beschrijving, **maken**.
5. Voer in het blad Informix verbindingslijn pakket, de naam (bijvoorbeeld "InformixConnectorNewOrders"), App-Service plannen en andere eigenschappen.
6. Selecteer **Package settings**en voer de volgende pakketinstellingen.

    Naam | Vereist |  Beschrijving
--- | --- | ---
ConnectionString | Ja | De verbindingsreeks Informix-Client (bijvoorbeeld ' netwerkadres = servernaam; Netwerk poort = 9089; Gebruikers-ID = gebruikersnaam; Wachtwoord = wachtwoord; initiële catalogus = nwind; standaard Schema informix = ").
Tabellen | Ja | Door komma's gescheiden lijst van tabel, weergeven en alias namen die zijn vereist voor OData-bewerkingen en swagger documentatie met voorbeelden (bijvoorbeeld "NEWORDERS") wilt genereren.
Procedures | Ja | Door komma's gescheiden lijst met namen van procedure en, functie (bijvoorbeeld "SPORDERID").
Lokale | Nee | On-premises implementatie met Azure Service Bus Relay implementeren.
ServiceBusConnectionString | Nee | Azure-Service Bus Relay-verbindingsreeks.
PollToCheckData | Nee | Selecteer aantal instructie voor gebruik met een app-trigger logica (bijvoorbeeld "SELECT tellen (\*) uit NEWORDERS waar VERZENDDATUM IS NULL ').
PollToReadData | Nee | SELECT-instructie voor gebruik met een app-trigger logica (bijvoorbeeld ' Selecteer \* uit NEWORDERS waar VERZENDDATUM null-waarden voor UPDATE IS ').
PollToAlterData | Nee | BIJWERKEN of verwijderen-instructie voor gebruik met een app-trigger logica (bijvoorbeeld ' UPDATE NEWORDERS instellen VERZENDDATUM = huidige datum waar huidige van &lt;CURSOR&gt;").

7. Selecteer **OK**en selecteer vervolgens **maken**.
8. Als alles compleet is, is de Package Settings er ongeveer als volgt uit:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Logica app met Informix verbindingslijn actie moet worden gegevens toevoegen ##
U kunt een logische app actie als u gegevens toevoegt aan een Informix-tabel met een API invoegen of Post aan entiteit OData bewerking wilt definiëren. Bijvoorbeeld, kunt u invoegen een nieuwe record in de volgorde klant, door het verwerken van een invoegen van de SQL-instructie ten opzichte van een tabel die is gedefinieerd met behulp van een id-kolom, waarde voor de identiteit of de rijen aan de app logica retourneren (ORDID van uiteindelijke tabel selecteren (invoegen in NEWORDERS (KLANTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) waarden (?,?,?,?,?,?))).

> [AZURE.TIP] Informix verbinding "*posten naar EntitySet*" retourneert de waarde van de kolom identiteit en '*API invoegen*' retourneert rijen beïnvloed

1. Selecteer in de Azure startboard **+** (plusteken), **Web + Mobile**, waarna **logica-app**.
2. Voer de naam (bijvoorbeeld "NewOrdersInformix"), App-Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard, de logica app die u zojuist hebt gemaakt, **Instellingen**en klik vervolgens **Triggers en acties**.
4. Selecteer in het blad Triggers en acties de optie **maken** binnen de app logica sjablonen.
5. In het deelvenster API-Apps, selecteer **Terugkeerpatroon**, stel een frequentie en interval en klik vervolgens **vinkje**.
6. Selecteer **Informix verbindingslijn**, uitvouwen van de lijst bewerkingen om te selecteren **in NEWORDER invoegen**in het deelvenster API-Apps.
7. Voer de volgende waarden in de parameterlijst uitvouwen:  

    Naam | Waarde
--- | --- 
KLANTID | 10042
EXPEDITEUR | 10000
VERZENDNAAM | Fikse K Kountry Store 
SHIPADDR | 12 orkest terras
VERZENDPLAATS | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Selecteer het **vinkje** om op te slaan de actie-instellingen en klik op **Opslaan**.
9. De instellingen ziet er als volgt uit:  
![][3]  
10. Selecteer het item wordt vermeld van eerste (meest recente uitvoeren) in de lijst **alle wordt uitgevoerd** onder **bewerkingen**. 
11. Selecteer de **actie** item **informixconnectorneworders**in het blad **logica app uitvoeren** .
12. Selecteer de **Koppeling van de invoer**in het blad **logica app actie** . Informix verbindingslijn gebruikmaakt van de invoer verwerkingstijd van een met parameters invoeginstructie.
13. Selecteer de **Uitvoer van koppeling**in het blad **logica app actie** . De invoer ziet er als volgt uit:  
![][4]

#### <a name="what-you-need-to-know"></a>Wat u moet weten

- Connector afgekapt Informix tabelnamen wanneer zodat er logica app actienamen. De bewerking **NEWORDERS invoegt** is bijvoorbeeld afgekapt tot **NEWORDER invoegt**.
- Logica app verwerkt na het opslaan van de app logica **Triggers en acties**, de bewerking. Er zijn mogelijk een vertraging van een aantal seconden (bijvoorbeeld 3 tot en met 5 seconden) voordat logica app de bewerking verwerkt. (Optioneel) u kunt op **Nu uitvoeren** om te verwerken van de bewerking.
- Informix verbindingslijn definieert leden van de EntitySet met kenmerken, inclusief of het lid bij een Informix-kolom met een standaard hoort- of gegenereerd kolommen (bijvoorbeeld identiteit). Logica weergegeven een rood sterretje naast de naam EntitySet lid-ID, om aan te geven Informix-kolommen waarvoor de waarden. U moet een waarde niet opgeven voor het lid ORDID, dat met Informix-id-kolom overeenkomt. U kunt waarden invoeren voor andere optionele leden (ITEMS, ORDDATE, REQDATE, EXPEDITEUR, VRACHTKOSTEN, SHIPCTRY), die met Informix kolommen met standaardwaarden overeenkomen. 
- Informix verbindingslijn geeft bij logica app-het antwoord op het bericht naar EntitySet waarin de waarden voor de identiteitskolommen, die wordt afgeleid van de DRDA SQLDARD (SQL gegevens gebied antwoordgegevens) op de vooraf gedefinieerde invoegen van de SQL-instructie. Informix server retourneert geen de ingevoegde waarden voor kolommen met standaardwaarden.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Logica app met Informix verbindingslijn actie bulksgewijs gegevens toe te voegen ##
U kunt een logische app actie als u wilt gegevens toevoegen aan een Informix-tabel met een bewerking API bulksgewijs invoegen definiëren. Zoals u kunt invoegen twee nieuwe klant volgorde records, door het verwerken van een invoegen van de SQL-instructie met een matrix van rijwaarden ten opzichte van een tabel die is gedefinieerd met behulp van een id-kolom retourneren de rijen aan de logica-app (ORDID van uiteindelijke tabel selecteren (invoegen in NEWORDERS (KLANTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) waarden (?,?,?,?,?,?))).

1. Selecteer in de Azure startboard **+** (plusteken), **Web + Mobile**, waarna **logica-app**.
2. Voer de naam (bijvoorbeeld "NewOrdersBulkInformix"), App-Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard, de logica app die u zojuist hebt gemaakt, **Instellingen**en klik vervolgens **Triggers en acties**.
4. Selecteer in het blad Triggers en acties de optie **maken** binnen de app logica sjablonen.
5. In het deelvenster API-Apps, selecteer **Terugkeerpatroon**, stel een frequentie en interval en klik vervolgens **vinkje**.
6. Selecteer **Informix verbindingslijn**, uitvouwen van de lijst bewerkingen om te selecteren **Bulksgewijs nieuw invoegt**in het deelvenster API-Apps.
7. Voer de waarde in **rijen** als een matrix. Bijvoorbeeld, kopieer en plak de volgende:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
    ```
        
8. Selecteer het **vinkje** om op te slaan de actie-instellingen en klik op **Opslaan**. De instellingen ziet er als volgt uit:  
![][6]

9. Klik in de lijst **alle wordt uitgevoerd** onder **bewerkingen**klikt u op het item wordt vermeld van eerste (meest recente uitvoeren).
10. Klik in het blad **logica app uitvoeren** op de **actie** -item.
11. Klik in het blad **logica app actie** op de **Koppeling van de invoer**. De uitvoer ziet er als volgt uit:  
[][7]
12. Klik op de **Koppeling van de uitvoer van**het blad **logica app actie** . De uitvoer ziet er als volgt uit:  
![][8]

#### <a name="what-you-need-to-know"></a>Wat u moet weten

- Connector afgekapt Informix tabelnamen wanneer zodat er logica app actienamen. De bewerking **Bulksgewijs invoegen in NEWORDERS** wordt bijvoorbeeld afgekapt tot het **Bulksgewijs nieuw invoegt**.
- Informix-database is mogelijk hoofdlettergevoelig aan tabel- en kolomnamen. De bulksgewijs invoegen bewerking matrix kolomnamen moet bijvoorbeeld worden opgegeven in kleine letters ("KlantId") in plaats van hoofdletters ("KLANTID").
- Door het weglaten van id-kolommen (bijvoorbeeld ORDID), nullable kolommen (bijvoorbeeld VERZENDDATUM) en kolommen met standaardwaarden (bijvoorbeeld ORDDATE, REQDATE, EXPEDITEUR, VRACHTKOSTEN, SHIPCTRY), genereert Informix-database waarden.
- Door op te geven 'vandaag' en 'morgen', Informix verbindingslijn genereert 'Huidige datum' en 'Huidige datum en 1 dag' functies (bijvoorbeeld REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Logica app met Informix verbindingslijn trigger om te lezen, wijzigen of verwijderen van gegevens ##
U kunt een trigger logica app als u wilt controleren en gegevens lezen van een Informix-tabel met een samengestelde API peiling gegevens-bewerking definiëren. Bijvoorbeeld, vindt u een of meer nieuwe klant volgorde records, de records terugkeren naar de logica-app. Het pakket/app Informix-verbindingsinstellingen ziet er als volgt uit:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | Selecteer tellen (\*) uit NEWORDERS waar VERZENDDATUM NULL is
PollToReadData | Selecteer \* uit NEWORDERS waar VERZENDDATUM NULL voor UPDATE is
PollToAlterData | <no value specified>


U kunt ook een trigger logica app als u wilt controleren, lezen en wijzigen van gegevens in een Informix-tabel met een samengestelde API peiling gegevens-bewerking definiëren. Bijvoorbeeld, vindt u een of meer nieuwe klant volgorde records, de rijwaarden van de, de geselecteerde (vóór update) records terugkeren naar de app logica bijwerken. Het pakket/app Informix-verbindingsinstellingen ziet er als volgt uit:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | Selecteer tellen (\*) uit NEWORDERS waar VERZENDDATUM NULL is
PollToReadData | Selecteer \* uit NEWORDERS waar VERZENDDATUM NULL voor UPDATE is
PollToAlterData | UPDATE NEWORDERS SET VERZENDDATUM = huidige datum waar huidige van &lt;CURSOR&gt;


Bovendien kunt u een app-trigger logica poll uitvoeren onder, lezen en gegevens verwijderen uit een Informix-tabel met een samengestelde API peiling gegevens-bewerking definiëren. Bijvoorbeeld, vindt u een of meer nieuwe klant volgorde records, de rijen, de geselecteerde (vóór verwijderen) records terugkeren naar de app logica verwijderen. Het pakket/app Informix-verbindingsinstellingen ziet er als volgt uit:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | Selecteer tellen (\*) uit NEWORDERS waar VERZENDDATUM NULL is
PollToReadData | Selecteer \* uit NEWORDERS waar VERZENDDATUM NULL voor UPDATE is
PollToAlterData | DELETE NEWORDERS waar huidige van &lt;CURSOR&gt;

In dit voorbeeld wordt logica app poll uitvoeren onder, lezen, bijwerken en klik vervolgens gegevens in de tabel Informix opnieuw te lezen.

1. Selecteer in de Azure startboard **+** (plusteken), **Web + Mobile**, waarna **logica-app**.
2. Voer de naam (bijvoorbeeld "ShipOrdersInformix"), App-Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard, de logica app die u zojuist hebt gemaakt, **Instellingen**en klik vervolgens **Triggers en acties**.
4. Selecteer in het blad Triggers en acties de optie **maken** binnen de app logica sjablonen.
5. Selecteer **Informix verbindingslijn**, een frequentie interval en klik vervolgens **vinkje**instellen in het deelvenster API-Apps.
6. Selecteer **Informix verbindingslijn**in het deelvenster API-Apps, de lijst bewerkingen als u wilt selecteren **uit NEWORDERS**uitvouwen.
7. Selecteer het **vinkje** om op te slaan de actie-instellingen en klik op **Opslaan**. De instellingen ziet er als volgt uit:  
![][10]
8. Klik om te sluiten van het blad **Triggers en acties** en klik vervolgens op om te sluiten van het blad **Instellingen** .
9. Klik in de lijst **alle wordt uitgevoerd** onder **bewerkingen**klikt u op het item wordt vermeld van eerste (meest recente uitvoeren).
10. Klik in het blad **logica app uitvoeren** op de **actie** -item.
11. Klik op de **Koppeling van de uitvoer van**het blad **logica app actie** . De uitvoer ziet er als volgt uit:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Logica app met Informix verbindingslijn actie om gegevens te verwijderen ##
U kunt een logische app actie om gegevens te verwijderen uit een Informix-tabel met een API verwijderen of Post aan entiteit OData bewerking definiëren. Bijvoorbeeld, kunt u invoegen een nieuwe record in de volgorde klant, door het verwerken van een invoegen van de SQL-instructie ten opzichte van een tabel die is gedefinieerd met behulp van een id-kolom, waarde voor de identiteit of de rijen aan de app logica retourneren (ORDID van uiteindelijke tabel selecteren (invoegen in NEWORDERS (KLANTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) waarden (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Logica app Informix verbindingslijn gebruiken om te verwijderen van gegevens maken ##
U kunt een nieuwe app van de logica van binnen de Azure Marketplace maakt en gebruikt u de verbindingslijn Informix als een actie klantorders verwijderen. Bijvoorbeeld, kunt u de Informix verbindingslijn voorwaardelijke verwijderbewerking verwerkingstijd van een verwijderen van de SQL-instructie (verwijderen uit NEWORDERS waar ORDID > = 10000).

1. Klik in het menu hub van het bord Azure **starten** op **+** (plusteken), klik op **Web + Mobile**, en klik vervolgens op **logica-app**. 
2. Typ een **naam**, bijvoorbeeld **RemoveOrdersInformix**in het blad **logica maken-app** .
3. Selecteer of waarden voor de andere instellingen (bijvoorbeeld serviceplan, resourcegroep) definiëren.
4. De instellingen moeten er als volgt uitzien. Klik op **maken**:  
![][12]
5. Klik in het blad **Instellingen** op **Triggers en acties**.
6. Klik in het blad **Triggers en acties** in de lijst **logica app sjablonen** op **maken op basis van maken**.
7. Klik in het blad **Triggers en acties** in het deelvenster **API-Apps** in de resourcegroep op **Terugkeerpatroon**.
8. Klik op het item **Terugkeerpatroon** op het ontwerpoppervlak logica app, **frequentie** en **Interval**, bijvoorbeeld **dagen** en **1**, instellen en klik op het **vinkje** om op te slaan, de instellingen voor het item terugkeerpatroon.
9. Klik in het blad **Triggers en acties** in het deelvenster **API-Apps** in de resourcegroep op **Informix verbindingslijn**.
10. Op het ontwerpoppervlak logica voor app, klik op het **Informix verbindingslijn** actie-item, klik op de weglatingstekens (**...**) om uit te vouwen van de lijst bewerkingen en klik vervolgens op **voorwaardelijke verwijderen uit N**.
11. Typ op het item Informix verbindingslijn actie **ordid ge 10000** voor een **expressie die een subset van gegevens worden geïdentificeerd**.
12. Klik op het **vinkje** om op te slaan de actie-instellingen en klik vervolgens op **Opslaan**. De instellingen ziet er als volgt uit:  
![][13]
13. Klik om te sluiten van het blad **Triggers en acties** en klik vervolgens op om te sluiten van het blad **Instellingen** .
14. Klik in de lijst **alle wordt uitgevoerd** onder **bewerkingen**klikt u op het item wordt vermeld van eerste (meest recente uitvoeren).
15. Klik in het blad **logica app uitvoeren** op de **actie** -item.
16. Klik op de **Koppeling van de uitvoer van**het blad **logica app actie** . De uitvoer ziet er als volgt uit:  
![][14]

**Notitie:** Logica app designer tabelnamen van de afgekapt. De bewerking **voorwaardelijke verwijderen uit NEWORDERS** wordt bijvoorbeeld afgekapt tot **voorwaardelijke verwijderen uit N**.


> [AZURE.TIP] Gebruik de volgende SQL-instructies om de voorbeeldtabel en opgeslagen procedures te maken. 

U kunt de voorbeeldtabel NEWORDERS de hand van de volgende Informix SQL DDL-instructies kunt maken:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


U kunt de steekproef SPORDERID opgeslagen procedure met de volgende Informix DDL-instructie maken:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Hybride configuratie (optioneel)

> [AZURE.NOTE] Deze stap is vereist als u werkt met een DB2 verbindingslijn on-premises implementatie achter de firewall.

App-Service gebruikt de hybride Configuration Manager veilig verbinden met uw on-premises-systeem. Als u verbindingslijn een on-premises implementatie IBM DB2-Server voor Windows gebruikt, is de hybride Connection Manager is vereist.

Zie [werken met de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Doe meer met de verbindingslijn
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een logica-app. Zie [Wat zijn de logica apps?](app-service-logic-what-are-logic-apps.md).

Maak de API-Apps met REST API's. Zie [verbindingslijnen en API Apps verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=529766).

U kunt ook prestaties statistieken besturingselement beveiliging en aan de verbindingslijn bekijken. Zie [beheren en controleren van de ingebouwde API-Apps en verbindingslijnen](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


