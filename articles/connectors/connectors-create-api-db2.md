<properties
    pageTitle="De verbindingslijn DB2 in uw Apps logica toevoegen | Microsoft Azure"
    description="Overzicht van DB2 verbindingslijn met REST API parameters"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Aan de slag met de DB2-connector
Microsoft-connector voor DB2 is verbonden logica Apps resources die zijn opgeslagen in een IBM DB2-database. Deze connector bevat een Microsoft-client om te communiceren met externe DB2 servercomputers via een TCP/IP-netwerk. Dit omvat cloud-databases, zoals IBM Bluemix dashDB of IBM DB2 voor Windows Azure virtualization, met en het on-premises databases die gebruikmaken van de on-premises implementatie data gateway. Bekijk de [lijst met ondersteunde](connectors-create-api-db2.md#supported-db2-platforms-and-versions) van IBM DB2-platforms en verschillende versies (in dit onderwerp).

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps algemene beschikbaarheid (GA). 

De verbindingslijn DB2 ondersteunt de volgende databasebewerkingen:

- Lijst-databasetabellen
- Een rij selecteren met lezen
- Alle rijen selecteren met lezen
- Eén rij invoegen met toevoegen
- ALTER één rij met bijwerken
- Verwijderen van één rij met verwijderen

In dit onderwerp ziet u hoe u de verbindingslijn gebruiken in een app logica aan proces databasebewerkingen.

Zie meer informatie over logica Apps, [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Beschikbare acties
De verbindingslijn DB2 ondersteunt de volgende bewerkingen uit logica app:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Lijst met tabellen
Maken van een app logica voor elke bewerking bestaat uit veel stappen uitgevoerd via de portal van Microsoft Azure.

U kunt een actie binnen de app logica toevoegen aan lijst met tabellen in een DB2-database. Hiermee geeft u de actie voor de verbindingslijn plaatsen, zoals een DB2-schema-instructie is verwerken `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Een logica-app maken
1.  Selecteer de **Azure starten bord** **+** (plusteken), **Web + Mobile**, waarna **Logica-App**.
2.  Voer de **naam**, zoals `Db2getTables`, **abonnement**, **resourcegroep**, **locatie**en **App-Service plannen**. Selecteer **vastmaken aan dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer**, **Lege LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst **triggers** **Terugkeerpatroon**. 
3.  In het **Terugkeerpatroon** -trigger, selecteer **bewerken**, **frequentie** -omlaag om **dag**te selecteren en stel vervolgens het **Interval** **7**typt.  
4.  Selecteer het vak **+ nieuwe stap** en selecteer vervolgens **een actie toevoegen**.
5.  Typ in de lijst **Acties** `db2` vak bewerken in het **Zoeken naar meer acties** en selecteer **DB2 - tabellen (Preview) opvragen**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  Schakel **het selectievakje** om in te schakelen **Verbinden via on-premises implementatie gegevensgateway**in het deelvenster **DB2 - tabellen opvragen** configuratie. U ziet dat de instellingen voor het wijzigen van de cloud in on-premises implementatie.
    - Typ de waarde voor de **Server**, in de vorm van poortnummer adres of alias dubbele punt. Typ bijvoorbeeld `ibmserver01:50000`.
    - Typ de waarde voor de **Database**. Typ bijvoorbeeld `nwind`.
    - Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **eenvoudige**.
    - Typ de waarde voor de **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
    - Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
    - Selecteer de waarde voor de **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
7. Selecteer **maken**en selecteer vervolgens **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Selecteer in het blad **Db2getTables** , in de lijst **alle wordt uitgevoerd** onder **Overzicht**, het item wordt vermeld van eerste (meest recente uitvoeren).
9.  Selecteer **Details uitvoeren**in het blad **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_tables**. Zie de waarde voor **Status**, die **is voltooid**moet zijn. Selecteer de **ingevoerde koppeling** om weer te geven van de invoer. Selecteer de **koppeling uitvoer**en de uitvoer van; te bekijken welke moet een lijst met tabellen bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>De verbindingen maken
Deze connector ondersteunt verbindingen naar databases die worden gehost on-premises implementatie en klik in de cloud met de volgende eigenschappen van verbinding. 

Eigenschap | Beschrijving
--- | ---
Server | Vereist. Een tekenreekswaarde die vertegenwoordigt een TCP/IP-adres of de alias in IPv4 of IPv6-indeling gevolgd (dubbele punt is scheidingsteken) door een TCP/IP-poortnummer accepteert. 
database | Vereist. Een tekenreekswaarde die een DRDA relationele Database naam (RDBNAM vertegenwoordigt) accepteert. DB2 voor z/OS accepteert een 16-byte-tekenreeks (database, is bekend als een IBM DB2 voor z/OS locatie). DB2 voor i5/OS accepteert een tekenreeks 18 bytes (database is bekend als een IBM DB2 voor i relationele database). DB2 voor LUW accepteert een tekenreeks 8 bytes.
verificatie | Optioneel. Een item lijstwaarde, Basic of Windows (kerberos) accepteert. 
gebruikersnaam | Vereist. Een tekenreekswaarde accepteert. DB2 voor z/OS accepteert een tekenreeks 8 bytes. DB2 voor ik een tekenreeks met 10-byte geaccepteerd. DB2 voor Linux of UNIX accepteert een tekenreeks 8 bytes. DB2 voor Windows accepteert een tekenreeks 30-byte.
wachtwoord | Vereist. Een tekenreekswaarde accepteert.
gateway | Vereist. Een lijst item waarde, dat staat voor de on-premises implementatie data gateway gedefinieerd tot logica Apps binnen de opslaggroep accepteert.  

## <a name="create-the-on-premises-gateway-connection"></a>De on-premises implementatie gateway-verbinding maken
Deze connector toegang tot een on-premises implementatie DB2-database met de gateway on-premises implementatie. Zie gateway-onderwerpen voor meer informatie. 

1. Schakel **het selectievakje** om in te schakelen **Verbinden via gateway**in het deelvenster **Gateways** configuratie. U ziet dat de instellingen voor het wijzigen van de cloud in on-premises implementatie.
2. Typ de waarde voor de **Server**, in de vorm van poortnummer adres of alias dubbele punt. Typ bijvoorbeeld `ibmserver01:50000`.
3. Typ de waarde voor de **Database**. Typ bijvoorbeeld `nwind`.
4. Selecteer de waarde voor **verificatie**. Selecteer bijvoorbeeld **eenvoudige**.
5. Typ de waarde voor de **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
6. Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
7. Selecteer de waarde voor de **Gateway**. Selecteer bijvoorbeeld **datagateway01**.
8. Selecteer **maken** om door te gaan. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>De verbinding met de cloud maken
Deze connector hebben toegang tot een cloud DB2-database. 

1. Klik in het deelvenster **Gateways** configuratie uitgeschakeld laat het **selectievakje** (niet is geklikt) **Verbinden via gateway**. 
2. Typ de waarde voor **de naam van de verbinding**. Typ bijvoorbeeld `hisdemo2`.
3. Typ de waarde voor **servernaam DB2**, in de vorm van poortnummer adres of alias dubbele punt. Typ bijvoorbeeld `hisdemo2.cloudapp.net:50000`.
3. Typ de waarde voor **de naam van de DB2-database**. Typ bijvoorbeeld `nwind`.
4. Typ de waarde voor de **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
5. Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
6. Selecteer **maken** om door te gaan. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Alle rijen selecteren met ophalen
U kunt een logische app actie als u wilt ophalen van alle rijen in een tabel DB2 definiëren. Hiermee wordt aangegeven dat de verbindingslijn verwerkingstijd van een DB2 SELECT-instructie, zoals wanneer `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Een logica-app maken
1.  Selecteer de **Azure starten bord** **+** (plusteken), **Web + Mobile**, waarna **Logica-App**.
2.  Voer de **naam**, zoals `Db2getRows`, **abonnement**, **resourcegroep**, **locatie**en **App-Service plannen**. Selecteer **vastmaken aan dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer**, **Lege LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst **triggers** **Terugkeerpatroon**. 
3.  In het **Terugkeerpatroon** -trigger, selecteert u **bewerken**, selecteert u **de frequentie** -omlaag om **dag**te selecteren en selecteer vervolgens **Interval** **7**typt. 
4.  Selecteer het vak **+ nieuwe stap** en selecteer vervolgens **een actie toevoegen**.
5.  Typ in de lijst **Acties** `db2` vak bewerken in het **Zoeken naar meer acties** en selecteer **DB2 - rijen (Preview) ophalen**.
6. Selecteer in de actie **rijen (Preview) ophalen** **verbinding wijzigen**.
7. Selecteer **Nieuw**in het deelvenster **verbindingen** configuratie. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. Klik in het deelvenster **Gateways** configuratie uitgeschakeld laat het **selectievakje** (niet is geklikt) **Verbinden via gateway**.
    - Typ de waarde voor **de naam van de verbinding**. Typ bijvoorbeeld `HISDEMO2`.
    - Typ de waarde voor **servernaam DB2**, in de vorm van poortnummer adres of alias dubbele punt. Typ bijvoorbeeld `HISDEMO2.cloudapp.net:50000`.
    - Typ de waarde voor **de naam van de DB2-database**. Typ bijvoorbeeld `NWIND`.
    - Typ de waarde voor de **gebruikersnaam**. Typ bijvoorbeeld `db2admin`.
    - Typ de waarde voor het **wachtwoord**. Typ bijvoorbeeld `Password1`.
9. Selecteer **maken** om door te gaan.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer vervolgens **gebied**.
11. (Optioneel) Selecteer **Geavanceerde opties** om op te geven queryopties.
12. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Selecteer in het blad **Db2getRows** , in de lijst **alle wordt uitgevoerd** onder **Overzicht**, het item wordt vermeld van eerste (meest recente uitvoeren).
14. Selecteer **Details uitvoeren**in het blad **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor **Status**, die **is voltooid**moet zijn. Selecteer de **ingevoerde koppeling** om weer te geven van de invoer. Selecteer de **koppeling uitvoer**en de uitvoer van; te bekijken waarin een lijst van rijen moet bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Eén rij invoegen met toevoegen
U kunt een logische app actie als u wilt toevoegen van een rij in een tabel DB2 definiëren. Hiermee geeft u deze actie in voor de verbindingslijn verwerkingstijd van een instructie DB2 invoegen, zoals `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Een logica-app maken
1.  Selecteer de **Azure starten bord** **+** (plusteken), **Web + Mobile**, waarna **Logica-App**.
2.  Voer de **naam**, zoals `Db2insertRow`, **abonnement**, **resourcegroep**, **locatie**en **App-Service plannen**. Selecteer **vastmaken aan dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer**, **Lege LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst **triggers** **Terugkeerpatroon**. 
3.  In het **Terugkeerpatroon** -trigger, selecteert u **bewerken**, selecteert u **de frequentie** -omlaag om **dag**te selecteren en selecteer vervolgens **Interval** **7**typt. 
4.  Selecteer het vak **+ nieuwe stap** en selecteer vervolgens **een actie toevoegen**.
5.  In de lijst **Acties** **db2** Typ in het vak **Zoeken naar meer acties** bewerken en selecteer vervolgens **DB2 - invoegrij (Preview)**.
6. Selecteer in de actie **rijen (Preview) ophalen** **verbinding wijzigen**. 
7. Selecteer een verbinding in het deelvenster van de configuratie **verbindingen** . Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer vervolgens **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` voor **AREAID**, typt u `Area 99999`, en typ `102` voor **REGIONID**. 
10. Selecteer **Opslaan**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Selecteer in het blad **Db2insertRow** , in de lijst **alle wordt uitgevoerd** onder **Overzicht**, het item wordt vermeld van eerste (meest recente uitvoeren).
12. Selecteer **Details uitvoeren**in het blad **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor **Status**, die **is voltooid**moet zijn. Selecteer de **ingevoerde koppeling** om weer te geven van de invoer. Selecteer de **koppeling uitvoer**en de uitvoer van; te bekijken waarop de nieuwe rij moet bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Een rij selecteren met ophalen
U kunt een logische app actie als u wilt ophalen van één rij in een tabel DB2 definiëren. Hiermee geeft u deze actie in voor de verbindingslijn verwerkingstijd van een instructie DB2 selecteren waar, zoals `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logica-app maken
1.  Selecteer de **Azure starten bord** **+** (plusteken), **Web + Mobile**, waarna **Logica-App**.
2.  Voer de **naam** (bijvoorbeeld "**Db2getRow**"), **abonnement** **resourcegroep**, **locatie**en **App-Service plannen**. Selecteer **vastmaken aan dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer**, **Lege LogicApp** in de lijst met **sjablonen** . 
2.  Selecteer in de lijst **triggers** **Terugkeerpatroon**. 
3.  In het **Terugkeerpatroon** -trigger, selecteert u **bewerken**, selecteert u **de frequentie** -omlaag om **dag**te selecteren en selecteer vervolgens **Interval** **7**typt. 
4.  Selecteer het vak **+ nieuwe stap** en selecteer vervolgens **een actie toevoegen**.
5.  In de lijst **Acties** **db2** Typ in het vak **Zoeken naar meer acties** bewerken en selecteer vervolgens **DB2 - rijen (Preview) ophalen**.
6. Selecteer in de actie **rijen (Preview) ophalen** **verbinding wijzigen**. 
7. Klik in het deelvenster **verbindingen** configuraties een bestaande verbinding te selecteren. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer vervolgens **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` voor **AREAID**. 
10. (Optioneel) Selecteer **Geavanceerde opties** om op te geven queryopties.
11. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Selecteer in het blad **Db2getRow** , in de lijst **alle wordt uitgevoerd** onder **Overzicht**, het item wordt vermeld van eerste (meest recente uitvoeren).
13. Selecteer **Details uitvoeren**in het blad **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor **Status**, die **is voltooid**moet zijn. Selecteer de **ingevoerde koppeling** om weer te geven van de invoer. Selecteer de **koppeling uitvoer**en de uitvoer van; te bekijken welke rij moet bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Één rij met UPDATE wijzigen
U kunt een logische app actie als u wilt wijzigen van een rij in een tabel DB2 definiëren. Hiermee geeft u deze actie in voor de verbindingslijn verwerkingstijd van een DB2-UPDATE-instructie, zoals `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Een logica-app maken
1.  Selecteer de **Azure starten bord** **+** (plusteken), **Web + Mobile**, waarna **Logica-App**.
2.  Voer de **naam**, zoals `Db2updateRow`, **abonnement**, **resourcegroep**, **locatie**en **App-Service plannen**. Selecteer **vastmaken aan dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer**, **Lege LogicApp** in de lijst met **sjablonen** .
2.  Selecteer in de lijst **triggers** **Terugkeerpatroon**. 
3.  In het **Terugkeerpatroon** -trigger, selecteert u **bewerken**, selecteert u **de frequentie** -omlaag om **dag**te selecteren en selecteer vervolgens **Interval** **7**typt. 
4.  Selecteer het vak **+ nieuwe stap** en selecteer vervolgens **een actie toevoegen**.
5.  In de lijst **Acties** **db2** Typ in het vak **Zoeken naar meer acties** bewerken en selecteer vervolgens **DB2 - Update rij (Preview)**.
6. Selecteer in de actie **rijen (Preview) ophalen** **verbinding wijzigen**. 
7. Selecteer in het deelvenster **verbindingen** configuraties aan een bestaande verbinding selecteren. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer vervolgens **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` voor **AREAID**, typt u `Updated 99999`, en typ `102` voor **REGIONID**. 
10. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Selecteer in het blad **Db2updateRow** , in de lijst **alle wordt uitgevoerd** onder **Overzicht**, het item wordt vermeld van eerste (meest recente uitvoeren).
12. Selecteer **Details uitvoeren**in het blad **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor **Status**, die **is voltooid**moet zijn. Selecteer de **ingevoerde koppeling** om weer te geven van de invoer. Selecteer de **koppeling uitvoer**en de uitvoer van; te bekijken waarop de nieuwe rij moet bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Verwijderen van één rij met verwijderen
U kunt een logische app actie als u wilt verwijderen van één rij in een tabel DB2 definiëren. Hiermee geeft u deze actie in voor de verbindingslijn verwerkingstijd van een instructie DB2 verwijderen, zoals `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Een logica-app maken
1.  Selecteer de **Azure starten bord** **+** (plusteken), **Web + Mobile**, waarna **Logica-App**.
2.  Voer de **naam**, zoals `Db2deleteRow`, **abonnement**, **resourcegroep**, **locatie**en **App-Service plannen**. Selecteer **vastmaken aan dashboard**en selecteer **maken**.

### <a name="add-a-trigger-and-action"></a>Een trigger en een actie toevoegen
1.  Selecteer in de **Logica Apps Designer**, **Lege LogicApp** in de lijst met **sjablonen** . 
2.  Selecteer in de lijst **triggers** **Terugkeerpatroon**. 
3.  In het **Terugkeerpatroon** -trigger, selecteert u **bewerken**, selecteert u **de frequentie** -omlaag om **dag**te selecteren en selecteer vervolgens **Interval** **7**typt. 
4.  Selecteer het vak **+ nieuwe stap** en selecteer vervolgens **een actie toevoegen**.
5.  In de lijst **Acties** **db2** selecteren in het vak **Zoeken naar meer acties** bewerken en selecteer vervolgens **DB2 - rij (Preview) verwijderen**.
6. Selecteer in de actie **rijen (Preview) ophalen** **verbinding wijzigen**. 
7. Klik in het deelvenster **verbindingen** configuraties een bestaande verbinding te selecteren. Selecteer bijvoorbeeld **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Selecteer de **pijl-omlaag**in de lijst **naam van de tabel** en selecteer vervolgens **gebied**.
9. Voer waarden in voor alle vereiste kolommen (Zie rood sterretje). Typ bijvoorbeeld `99999` voor **AREAID**. 
10. Selecteer **Opslaan**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Selecteer in het blad **Db2deleteRow** , in de lijst **alle wordt uitgevoerd** onder **Overzicht**, het item wordt vermeld van eerste (meest recente uitvoeren).
12. Selecteer **Details uitvoeren**in het blad **logica app uitvoeren** . Selecteer in de lijst **actie** **Get_rows**. Zie de waarde voor **Status**, die **is voltooid**moet zijn. Selecteer de **ingevoerde koppeling** om weer te geven van de invoer. Selecteer de **koppeling uitvoer**en de uitvoer van; te bekijken waarin de verwijderde rij moet bevatten.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Technische Details

## <a name="actions"></a>Acties
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een app logica. De verbindingslijn DB2-database bevat de volgende bewerkingen uit. 

|Actie|Beschrijving|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Eén rij opgehaald uit een DB2-tabel|
|[GetRows](connectors-create-api-db2.md#get-rows)|Rijen opgehaald uit een DB2-tabel|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Een nieuwe rij invoegen in een tabel DB2|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Hiermee verwijdert u een rij uit een DB2-tabel|
|[GetTables](connectors-create-api-db2.md#get-tables)|Tabellen opgehaald uit een DB2-database|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Een bestaande rij in een tabel DB2 wordt bijgewerkt|

### <a name="action-details"></a>Actiedetails

In deze sectie, de specifieke informatie over elke actie, inclusief eventuele verplicht of optioneel eigenschappen voor de invoer en alle bijbehorende uitvoer die is gekoppeld aan de verbindingslijn te bekijken.

#### <a name="get-row"></a>Rij ophalen 
Haalt één rij vanuit een DB2-tabel.  

| Naam van eigenschap| Weergavenaam |Beschrijving|
| ---|---|---|
|tabel * | Tabelnaam |Naam van DB2-tabel|
|ID * | Rij-id |Unieke id van de rij om op te halen|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
Item

| Naam van eigenschap | Gegevenstype |
|---|---|
|ItemInternalId|tekenreeks|


#### <a name="get-rows"></a>Rijen ophalen 
Rijen opgehaald uit een DB2-tabel.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Tabelnaam|Naam van DB2-tabel|
|$skip|Overslaan tellen|Aantal vermeldingen om over te slaan (standaard = 0)|
|$top|Maximale Get tellen|Maximum aantal vermeldingen om op te halen (standaard = 256)|
|$filter|Query filteren|Een query voor het filter van ODATA om het aantal items te beperken|
|$orderby|Sorteren op|Een query ODATA orderBy om aan te geven van de volgorde van items|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
ItemsList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|


#### <a name="insert-row"></a>Rij invoegen 
Hiermee voegt u een nieuwe rij in een tabel DB2.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Tabelnaam|Naam van DB2-tabel|
|item *|Rij|Rij invoegen in de opgegeven tabel in DB2|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
Item

| Naam van eigenschap | Gegevenstype |
|---|---|
|ItemInternalId|tekenreeks|


#### <a name="delete-row"></a>Rij verwijderen 
Hiermee verwijdert u een rij uit een tabel DB2.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Tabelnaam|Naam van DB2-tabel|
|ID *|Rij-id|Unieke id van de rij verwijderen|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer
Geen.

#### <a name="get-tables"></a>Tabellen opvragen 
Tabellen opgehaald uit een DB2-database.  

Er zijn geen parameters voor dit gesprek. 

##### <a name="output-details"></a>Gegevens voor uitvoer 
TablesList

| Naam van eigenschap | Gegevenstype |
|---|---|
|waarde|matrix|

#### <a name="update-row"></a>Rij bijwerken 
Een bestaande rij in een tabel DB2 worden bijgewerkt.  

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|tabel *|Tabelnaam|Naam van DB2-tabel|
|ID *|Rij-id|Unieke id van de rij bijwerken|
|item *|Rij|Rij met de bijgewerkte waarden|

Een sterretje (*) betekent dat de eigenschap is vereist.

##### <a name="output-details"></a>Gegevens voor uitvoer  
Item

| Naam van eigenschap | Gegevenstype |
|---|---|
|ItemInternalId|tekenreeks|


### <a name="http-responses"></a>HTTP-antwoorden

Wanneer u oproepen naar de verschillende acties, krijgt u mogelijk bepaalde antwoorden. De volgende tabel bevat een overzicht van de antwoorden en de bijbehorende beschrijvingen:  

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="supported-db2-platforms-and-versions"></a>Ondersteunde platforms voor DB2 en versies
Deze connector ondersteunt de volgende IBM DB2-platforms en versies, evenals IBM DB2 compatibele producten (bijvoorbeeld IBM Bluemix dashDB) die ondersteuning bieden voor Distributed relationele Database architectuur (DRDA) SQL Access Manager (SQLAM) versie 10 en 11:

- IBM DB2 voor z/OS 11.1
- IBM DB2 voor z/OS 10,1
- IBM DB2 voor i 7.3
- IBM DB2 voor i 7.2
- IBM DB2 voor i 7.1
- IBM DB2 voor LUW 11
- IBM DB2 voor LUW 10.5


## <a name="next-steps"></a>Volgende stappen

[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md). Verken de andere beschikbare connectors in logica-Apps op onze [lijst API's](apis-list.md).

