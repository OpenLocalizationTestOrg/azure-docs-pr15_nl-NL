<properties
   pageTitle="De verbindingslijn DB2 met Microsoft Azure App Service | Microsoft Azure"
   description="Het gebruik van de verbindingslijn DB2 met logica app triggers en acties"
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

# <a name="db2-connector"></a>DB2-connector
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

Microsoft-connector voor DB2 is een API-app voor toepassingen via Azure App Service verbinden resources die zijn opgeslagen in een IBM DB2-database. Connector bevat een Microsoft-Client verbinding maken met externe DB2 servercomputers via een netwerkverbinding TCP/IP, met inbegrip van Azure hybride verbindingen naar on-premises implementatie DB2 servers met behulp van de Azure Service Bus Relay verbindingslijn ondersteunt de volgende databasebewerkingen:

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


## <a name="create-the-db2-connector"></a>De verbindingslijn DB2 maken
U kunt een verbindingslijn binnen een app logica of van Azure Marketplace, zoals definiëren in het volgende voorbeeld:  

1. Selecteer in de Azure startboard **Marketplace**.
2. Typ **db2** in het vak **Alles zoeken** in het blad **Alles** en klik vervolgens op de enter-toets.
3. Ga in de zoekopdracht alles deelvenster resultaten, selecteer **DB2 verbindingslijn**.
4. Selecteer in het blad DB2 verbindingslijn beschrijving, **maken**.
5. Voer in het blad DB2 verbindingslijn pakket, de naam (bijvoorbeeld "Db2ConnectorNewOrders"), App-Service plannen en andere eigenschappen.
6. Selecteer **Package settings**en voer de volgende pakketinstellingen:  

    Naam | Vereist |  Beschrijving
--- | --- | ---
ConnectionString | Ja | De verbindingsreeks DB2-Client (bijvoorbeeld ' netwerkadres = servernaam; Netwerk poort = 50000; Gebruikers-ID = gebruikersnaam; Wachtwoord = wachtwoord; initiële catalogus = steekproef; Siteverzameling als pakket inpakken = NWIND; standaard Schema NWIND = ").
Tabellen | Ja | Door komma's gescheiden lijst van tabel, weergeven en alias namen die zijn vereist voor OData-bewerkingen en swagger documentatie met voorbeelden (bijvoorbeeld "*NEWORDERS*") wilt genereren.
Procedures | Ja | Door komma's gescheiden lijst met namen van procedure en, functie (bijvoorbeeld "SPORDERID").
Lokale | Nee | On-premises implementatie met Azure Service Bus Relay implementeren.
ServiceBusConnectionString | Nee | Azure-Service Bus Relay-verbindingsreeks.
PollToCheckData | Nee | Selecteer aantal instructie voor gebruik met een app-trigger logica (bijvoorbeeld "SELECT tellen (\*) uit NEWORDERS waar VERZENDDATUM IS NULL ').
PollToReadData | Nee | SELECT-instructie voor gebruik met een app-trigger logica (bijvoorbeeld ' Selecteer \* uit NEWORDERS waar VERZENDDATUM null-waarden voor UPDATE IS ').
PollToAlterData | Nee | BIJWERKEN of verwijderen-instructie voor gebruik met een app-trigger logica (bijvoorbeeld ' UPDATE NEWORDERS instellen VERZENDDATUM = huidige datum waar huidige van &lt;CURSOR&gt;").

7. Selecteer **OK**en selecteer vervolgens **maken**.
8. Als alles compleet is, is de Package Settings er ongeveer als volgt uit:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logica app met DB2 verbindingslijn actie moet worden gegevens toevoegen ##
U kunt een logische app actie als u gegevens toevoegt aan een DB2-tabel met een API invoegen of Post aan entiteit OData bewerking wilt definiëren. Bijvoorbeeld: u kunt invoegen een nieuwe record in de volgorde klant, door het verwerken van een invoegen van de SQL-instructie ten opzichte van een tabel die is gedefinieerd met behulp van een id-kolom retourneren de identiteitwaarde of de rijen aan de logica-app (ORDID van uiteindelijke tabel selecteren (invoegen in NWIND. NEWORDERS (KLANTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) WAARDEN (?,?,?,?,?,?))).

> [AZURE.TIP] DB2 verbinding "*posten naar EntitySet*" retourneert de waarde van de kolom identiteit en '*API invoegen*' retourneert rijen beïnvloed

1. Selecteer in de Azure startboard **+** (plusteken), **Web + Mobile**, waarna **logica-app**.
2. Voer de naam (bijvoorbeeld "NewOrdersDb2"), App-Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard, de logica app die u zojuist hebt gemaakt, **Instellingen**en klik vervolgens **Triggers en acties**.
4. Selecteer in het blad Triggers en acties de optie **maken** binnen de app logica sjablonen.
5. In het deelvenster API-Apps, selecteer **Terugkeerpatroon**, stel een frequentie en interval en klik vervolgens **vinkje**.
6. Selecteer **DB2 verbindingslijn**, vouw uit de lijst bewerkingen om te selecteren **in NEWORDER invoegen**in het deelvenster API-Apps.
7. Voer de volgende waarden in de parameterlijst uitvouwen:  

    Naam | Waarde
--- | --- 
KLANTID | 10042
VERZENDNAAM | Fikse K Kountry Store 
SHIPADDR | 12 orkest terras
VERZENDPLAATS | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Selecteer het **vinkje** om op te slaan de actie-instellingen en klik op **Opslaan**.
9. De instellingen ziet er als volgt uit:  
![][3]

10. Selecteer het item wordt vermeld van eerste (meest recente uitvoeren) in de lijst **alle wordt uitgevoerd** onder **bewerkingen**. 
11. Selecteer de **actie** item **db2connectorneworders**in het blad **logica app uitvoeren** .
12. Selecteer de **Koppeling van de invoer**in het blad **logica app actie** . De invoer DB2 verbindingslijn gebruikt verwerkingstijd van een met parameters invoeginstructie.
13. Selecteer de **Uitvoer van koppeling**in het blad **logica app actie** . De invoer ziet er als volgt uit:  
![][4]

#### <a name="what-you-need-to-know"></a>Wat u moet weten

- Connector afgekapt DB2 tabelnamen wanneer zodat er logica app actienamen. De bewerking **NEWORDERS invoegt** is bijvoorbeeld afgekapt tot **NEWORDER invoegt**.
- Logica app verwerkt na het opslaan van de app logica **Triggers en acties**, de bewerking. Er zijn mogelijk een vertraging van een aantal seconden (bijvoorbeeld 3 tot en met 5 seconden) voordat logica app de bewerking verwerkt. (Optioneel) u kunt op **Nu uitvoeren** om te verwerken van de bewerking.
- DB2 verbindingslijn definieert leden van de EntitySet met kenmerken, inclusief of het lid bij een DB2-kolom met een standaard hoort- of gegenereerd kolommen (bijvoorbeeld identiteit). Logica weergegeven een rood sterretje naast de naam EntitySet lid-ID, om aan te geven DB2-kolommen waarvoor de waarden. U moet een waarde niet opgeven voor het lid ORDID, dat met DB2 id-kolom overeenkomt. U kunt waarden invoeren voor andere optionele leden (ITEMS, ORDDATE, REQDATE, EXPEDITEUR, VRACHTKOSTEN, SHIPCTRY), die met DB2 kolommen met standaardwaarden overeenkomen. 
- DB2 verbindingslijn geeft bij logica app-het antwoord op het bericht naar EntitySet waarin de waarden voor de identiteitskolommen, die wordt afgeleid van de DRDA SQLDARD (SQL gegevens gebied antwoordgegevens) op de vooraf gedefinieerde invoegen van de SQL-instructie. DB2 server retourneert geen de ingevoegde waarden voor kolommen met standaardwaarden.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logica app met DB2 verbindingslijn actie moet worden gegevens bulksgewijs toevoegen ##
U kunt een logische app actie als u wilt gegevens toevoegen aan een DB2-tabel met een bewerking API bulksgewijs invoegen definiëren. Bijvoorbeeld: u kunt invoegen twee nieuwe klant volgorde records, door het verwerken van een invoegen van de SQL-instructie met een matrix van rijwaarden ten opzichte van een tabel die is gedefinieerd met behulp van een id-kolom retourneren de rijen aan de logica-app (ORDID van uiteindelijke tabel selecteren (invoegen in NWIND. NEWORDERS (KLANTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) WAARDEN (?,?,?,?,?,?))).

1. Selecteer in de Azure startboard **+** (plusteken), **Web + Mobile**, waarna **logica-app**.
2. Voer de naam (bijvoorbeeld "NewOrdersBulkDb2"), App-Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard, de logica app die u zojuist hebt gemaakt, **Instellingen**en klik vervolgens **Triggers en acties**.
4. Selecteer in het blad Triggers en acties de optie **maken** binnen de app logica sjablonen.
5. In het deelvenster API-Apps, selecteer **Terugkeerpatroon**, stel een frequentie en interval en klik vervolgens **vinkje**.
6. Selecteer **DB2 verbindingslijn**, uitvouwen van de lijst bewerkingen om te selecteren **Bulksgewijs nieuw invoegt**in het deelvenster API-Apps.
7. Voer de waarde in **rijen** als een matrix. Bijvoorbeeld, kopieer en plak de volgende:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
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

- Connector afgekapt DB2 tabelnamen wanneer zodat er logica app actienamen. De bewerking **Bulksgewijs invoegen in NEWORDERS** wordt bijvoorbeeld afgekapt tot het **Bulksgewijs nieuw invoegt**.
- Door het weglaten van id-kolommen (bijvoorbeeld ORDID), nullable kolommen (bijvoorbeeld VERZENDDATUM) en kolommen met standaardwaarden (bijvoorbeeld ORDDATE, REQDATE, EXPEDITEUR, VRACHTKOSTEN, SHIPCTRY), genereert DB2-database waarden.
- Door op te geven 'vandaag' en 'morgen', DB2 verbindingslijn genereert 'Huidige datum' en 'Huidige datum en 1 dag' functies (bijvoorbeeld REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logica app met DB2 verbindingslijn trigger om te lezen, wijzigen of verwijderen van gegevens ##
U kunt een app-trigger logica om te controleren en lezen van gegevens uit een DB2-tabel met een samengestelde API peiling gegevens-bewerking definiëren. Bijvoorbeeld, vindt u een of meer nieuwe klant volgorde records, de records terugkeren naar de logica-app. Het pakket/app DB2-verbindingsinstellingen ziet er als volgt uit:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | Selecteer tellen (\*) uit NEWORDERS waar VERZENDDATUM NULL is
PollToReadData | Selecteer \* uit NEWORDERS waar VERZENDDATUM NULL voor UPDATE is
PollToAlterData | <no value specified>


U kunt ook een trigger logica app als u wilt controleren, lezen en wijzigen van gegevens in een DB2-tabel met een samengestelde API peiling gegevens-bewerking definiëren. Bijvoorbeeld, vindt u een of meer nieuwe klant volgorde records, de rijwaarden van de, de geselecteerde (vóór update) records terugkeren naar de app logica bijwerken. Het pakket/app DB2-verbindingsinstellingen ziet er als volgt uit:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | Selecteer tellen (\*) uit NEWORDERS waar VERZENDDATUM NULL is
PollToReadData | Selecteer \* uit NEWORDERS waar VERZENDDATUM NULL voor UPDATE is
PollToAlterData | UPDATE NEWORDERS SET VERZENDDATUM = huidige datum waar huidige van &lt;CURSOR&gt;


Bovendien kunt u een app-trigger logica poll uitvoeren onder, lezen en gegevens verwijderen uit een DB2-tabel met een samengestelde API peiling gegevens-bewerking definiëren. Bijvoorbeeld, vindt u een of meer nieuwe klant volgorde records, de rijen, de geselecteerde (vóór verwijderen) records terugkeren naar de app logica verwijderen. Het pakket/app DB2-verbindingsinstellingen ziet er als volgt uit:

    App-instelling | Waarde
--- | --- | ---
PollToCheckData | Selecteer tellen (\*) uit NEWORDERS waar VERZENDDATUM NULL is
PollToReadData | Selecteer \* uit NEWORDERS waar VERZENDDATUM NULL voor UPDATE is
PollToAlterData | DELETE NEWORDERS waar huidige van &lt;CURSOR&gt;

In dit voorbeeld wordt logica app poll uitvoeren onder, lezen, bijwerken en klik vervolgens gegevens in de tabel DB2 opnieuw te lezen.

1. Selecteer in de Azure startboard **+** (plusteken), **Web + Mobile**, waarna **logica-app**.
2. Voer de naam (bijvoorbeeld "ShipOrdersDb2"), App-Service plannen, andere eigenschappen en selecteer **maken**.
3. Selecteer in de Azure startboard, de logica app die u zojuist hebt gemaakt, **Instellingen**en klik vervolgens **Triggers en acties**.
4. Selecteer in het blad Triggers en acties de optie **maken** binnen de app logica sjablonen.
5. Selecteer **DB2 verbindingslijn**, een frequentie interval en klik vervolgens **vinkje**instellen in het deelvenster API-Apps.
6. In het deelvenster API-Apps, selecteer **DB2 verbindingslijn**, de lijst bewerkingen als u wilt selecteren **uit NEWORDERS**uitvouwen.
7. Selecteer het **vinkje** om op te slaan de actie-instellingen en klik op **Opslaan**. De instellingen ziet er als volgt uit:  
![][10]  
8. Klik om te sluiten van het blad **Triggers en acties** en klik vervolgens op om te sluiten van het blad **Instellingen** .
9. Klik in de lijst **alle wordt uitgevoerd** onder **bewerkingen**klikt u op het item wordt vermeld van eerste (meest recente uitvoeren).
10. Klik in het blad **logica app uitvoeren** op de **actie** -item.
11. Klik op de **Koppeling van de uitvoer van**het blad **logica app actie** . De uitvoer ziet er als volgt uit:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logica app met DB2 verbindingslijn actie om gegevens te verwijderen ##
U kunt een logische app actie om gegevens te verwijderen uit een DB2-tabel met een API verwijderen of Post aan entiteit OData bewerking definiëren. Bijvoorbeeld: u kunt invoegen een nieuwe record in de volgorde klant, door het verwerken van een invoegen van de SQL-instructie ten opzichte van een tabel die is gedefinieerd met behulp van een id-kolom retourneren de identiteitwaarde of de rijen aan de logica-app (ORDID van uiteindelijke tabel selecteren (invoegen in NWIND. NEWORDERS (KLANTID, VERZENDNAAM, SHIPADDR, VERZENDPLAATS, SHIPREG, SHIPZIP) WAARDEN (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Logica app DB2 verbindingslijn gebruiken om te verwijderen van gegevens maken ##
U kunt een nieuwe app van de logica van binnen de Azure Marketplace maakt en gebruikt u de verbindingslijn DB2 als een actie klantorders verwijderen. Bijvoorbeeld, kunt u de DB2 verbindingslijn voorwaardelijke verwijderbewerking verwerkingstijd van een verwijderen van de SQL-instructie (verwijderen uit NEWORDERS waar ORDID > = 10000).

1. Klik in het menu hub van het bord Azure **starten** op **+** (plusteken), klik op **Web + Mobile**, en klik vervolgens op **logica-app**. 
2. Typ een **naam**, bijvoorbeeld **RemoveOrdersDb2**in het blad **logica maken-app** .
3. Selecteer of waarden voor de andere instellingen (bijvoorbeeld serviceplan, resourcegroep) definiëren.
4. De instellingen moeten er als volgt uitzien. Klik op **maken**:  
![][12]  
5. Klik in het blad **Instellingen** op **Triggers en acties**.
6. Klik in het blad **Triggers en acties** in de lijst **logica app sjablonen** op **maken op basis van maken**.
7. Klik in het blad **Triggers en acties** in het deelvenster **API-Apps** in de resourcegroep op **Terugkeerpatroon**.
8. Klik op het item **Terugkeerpatroon** op het ontwerpoppervlak logica app, **frequentie** en **Interval**, bijvoorbeeld **dagen** en **1**, instellen en klik op het **vinkje** om op te slaan, de instellingen voor het item terugkeerpatroon.
9. Klik in het blad **Triggers en acties** in het deelvenster **API-Apps** in de resourcegroep op **DB2 verbindingslijn**.
10. Op het ontwerpoppervlak logica voor app, klik op het **DB2 verbindingslijn** actie-item, klik op de weglatingstekens (**...**) om uit te vouwen van de lijst bewerkingen en klik vervolgens op **voorwaardelijke verwijderen uit N**.
11. Typ op het item DB2 verbindingslijn actie **ORDID ge 10000** voor een **expressie die een subset van gegevens worden geïdentificeerd**.
12. Klik op het **vinkje** om op te slaan de actie-instellingen en klik vervolgens op **Opslaan**. De instellingen ziet er als volgt uit:  
![][13]  
13. Klik om te sluiten van het blad **Triggers en acties** en klik vervolgens op om te sluiten van het blad **Instellingen** .
14. Klik in de lijst **alle wordt uitgevoerd** onder **bewerkingen**klikt u op het item wordt vermeld van eerste (meest recente uitvoeren).
15. Klik in het blad **logica app uitvoeren** op de **actie** -item.
16. Klik op de **Koppeling van de uitvoer van**het blad **logica app actie** . De uitvoer ziet er als volgt uit:  
![][14]

**Notitie:** Logica app designer tabelnamen van de afgekapt. De bewerking **voorwaardelijke verwijderen uit NEWORDERS** wordt bijvoorbeeld afgekapt **voorwaardelijke**verwijderen uit N.


> [AZURE.TIP] Gebruik de volgende SQL-instructies om de voorbeeldtabel en opgeslagen procedures te maken. 

U kunt de voorbeeldtabel NEWORDERS de hand van de volgende DB2 SQL DDL-instructies kunt maken:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



U kunt de steekproef SPOERID opgeslagen procedure met de volgende DB2 DDL-instructie maken:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hybride configuratie (optioneel)

> [AZURE.NOTE] Deze stap is vereist als u werkt met een DB2 verbindingslijn on-premises implementatie achter de firewall.

App-Service gebruikt de hybride Configuration Manager veilig verbinden met uw on-premises-systeem. Als u verbindingslijn een on-premises implementatie IBM DB2-Server voor Windows gebruikt, is de hybride Connection Manager is vereist.

Zie [werken met de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Doe meer met de verbindingslijn
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een logica-app. Zie [Wat zijn de logica apps?](app-service-logic-what-are-logic-apps.md).

Maak de API-Apps met REST API's. Zie [verbindingslijnen en API Apps verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=529766).

U kunt ook prestaties statistieken besturingselement beveiliging en aan de verbindingslijn bekijken. Zie [beheren en controleren van de ingebouwde API-Apps en verbindingslijnen](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

