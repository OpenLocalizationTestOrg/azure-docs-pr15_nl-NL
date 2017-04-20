<properties
   pageTitle="Gebruik van de SQL-Connector in logica Apps | Microsoft Azure-App-Service"
   description="Het maken en configureren van de SQL-verbindingslijn of API-app en deze gebruiken in een app logica in Azure App-Service"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Aan de slag met de Microsoft SQL-Connector en deze toevoegen aan uw logica-App
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie. Klik op [SQL Azure-API](../connectors/connectors-create-api-sqlazure.md)voor de schema Azure SQL-2015-08-01-preview-versie.

Verbinding maken met een on-premises SQL Server- of een Azure SQL-Database voor het maken en uw gegevens of gegevens wijzigen. Verbindingslijnen kunnen worden gebruikt in logica Apps ophalen, proces, of als onderdeel van een 'werkstroom' push-gegevens. Wanneer u de SQL-Connector in uw werkstroom gebruikt, kunt u verschillende scenario's kunt bereiken. U kunt bijvoorbeeld het volgende doen:

- Laten zien een gedeelte van de gegevens die in uw SQL-database via een web of mobiele toepassing.
- Gegevens in een tabel met de SQL-database voor de opslag invoegen. U kunt bijvoorbeeld werknemerrecords invoeren, verkooporders bijwerken en enzovoort.
- Gegevens ophalen uit SQL en in een bedrijfsproces te gebruiken. U kunt bijvoorbeeld klantrecords ophalen en deze klantrecords in SalesForce hebt opgeslagen.

U kunt de SQL-verbindingslijn toevoegen aan uw zakelijke werkstroom en proces gegevens als onderdeel van deze werkstroom binnen een logica-App. 

## <a name="triggers-and-actions"></a>Triggers en acties
*Triggers* zijn gebeurtenissen die plaatsvinden. Bijvoorbeeld wordt wanneer een order is bijgewerkt of wanneer een nieuwe klant toegevoegd. Een *actie* is het resultaat van de trigger. Bijvoorbeeld wanneer een order wordt bijgewerkt, stuurt u een melding naar de verkoper. Of, wanneer een nieuwe klant is toegevoegd, stuurt u een welkomstbericht naar de nieuwe klant.

De SQL-Connector kunnen worden gebruikt als een trigger of een actie in een logica app en ondersteunt in JSON en XML-indelingen. Voor elke tabel die is opgenomen in uw pakket instellingen (hierover verderop in dit onderwerp), er is een reeks acties JSON en een reeks XML-acties.

De SQL-Connector heeft de volgende Triggers en acties die beschikbaar zijn:

Triggers | Acties
--- | ---
Gegevens van de peiling | <ul><li>In tabel invoegen</li><li>Inhoudsopgave bijwerken</li><li>Selecteer in de tabel</li><li>Uit tabel verwijderen</li><li>Opgeslagen Procedure aanroepen</li></ul>

## <a name="create-the-sql-connector"></a>De SQL-Connector maken

Een verbindingslijn binnen een app logica kan worden gemaakt of rechtstreeks van Azure Marketplace worden gemaakt. Een verbindingslijn van Marketplace maken:  

1. Selecteer in de Azure startboard **Marketplace**.
2. Zoeken naar 'SQL-Connector', selecteert u deze en selecteer **maken**.
3. Voer de naam van de App-Service plannen en andere eigenschappen.
4. Voer de volgende pakketinstellingen:

    Naam | Vereist |  Beschrijving
--- | --- | ---
De naam van server | Ja | Voer de naam van de SQL Server. Voer bijvoorbeeld *SQL Server/sqlexpress* of *SQLserver.mydomain.com*.
Poort | Nee | Standaard is 1433.
Gebruikersnaam in te voeren | Ja | Typ de naam van een gebruiker die zich kan aanmelden in de SQL-Server. Als u verbinding maakt met een lokale SQL Server, voert u SQL-verificatie referenties.
Wachtwoord | Ja | Voer de gebruikersnaam.
Databasenaam | Ja | Voer in de database die u verbinding maakt. U kunt bijvoorbeeld *klanten* of *dbo/orders*invoeren.
On-Premises | Ja | Standaard is ONWAAR. Voer ONWAAR als verbinding maakt met een Azure SQL-database. Typ True als verbinding maakt met een lokale SQL Server.
Service Bus verbindingsreeks | Nee | Als u verbinding met on-premises implementatie maakt, voert u de verbindingsreeks van de Service Bus-relay.<br/><br/>[Gebruik van de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md)<br/>[Service-Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/)
Partner-servernaam | Nee | Als de primaire-server niet beschikbaar is, kunt u een partnerserver invoeren als een alternatieve of back-server.
Tabellen | Nee | Lijst met de databasetabellen die kunnen worden bijgewerkt door de verbindingslijn. Voer bijvoorbeeld *OrdersTable* of *EmployeeTable*. Als geen tabellen zijn ingevoerd, kunnen alle tabellen kunnen worden gebruikt. Geldige tabellen en/of opgeslagen Procedures moet deze connector gebruiken als een actie.
Opgeslagen Procedures | Nee | Voer een bestaande opgeslagen procedure die kan worden aangeroepen door de verbindingslijn. Voer bijvoorbeeld *sp_IsEmployeeEligible* of *sp_CalculateOrderDiscount*. Geldige tabellen en/of opgeslagen Procedures moet deze connector gebruiken als een actie.
Beschikbare gegevensquery | Voor trigger ondersteuning | SQL-instructie om te bepalen of geen gegevens beschikbaar is voor een SQL Server-databasetabel polling. Dit moet een getal dat staat voor het aantal rijen van de beschikbare gegevens retourneren. Voorbeeld: Selecteer COUNT(*) in tabelnaam.
Peiling gegevensquery | Voor trigger ondersteuning | De SQL-instructie naar poll uitvoeren onder de tabel met SQL Server-database. U kunt een willekeurig aantal SQL-instructies gescheiden door een puntkomma invoeren. Deze instructie is uitgevoerd transactioneel en alleen vastgelegd wanneer de gegevens veilig worden opgeslagen in uw app logica. Voorbeeld: Selecteer *in tabelnaam; VERWIJDEREN uit tabelnaam. <br/>**Note**<br/>Een peiling-instructie die een lus vermijdt door te verwijderen, verplaatsen of het bijwerken van geselecteerde gegevens om ervoor te zorgen dat dezelfde gegevens opnieuw wordt niet gecontroleerd, moet u opgeven.

5. Als alles compleet is, is de Package Settings er ongeveer als volgt uit:  
![][1]  

6. Selecteer **maken**. 


## <a name="use-the-connector-as-a-trigger"></a>De verbindingslijn als een Trigger gebruiken
Bekijk een eenvoudige logica app waarmee gegevens uit een SQL-tabel worden opgevraagd, voegt de gegevens in een andere tabel en de gegevens worden bijgewerkt.

Als u wilt gebruiken de SQL-connector als een trigger, voer de waarden in **Beschikbaar gegevensquery** en **Gegevensquery peiling** . **Gegevens beschikbaar Query** wordt uitgevoerd op de planning die u invoert, en bepaalt als er geen gegevens beschikbaar is. Aangezien deze query alleen een scalaire getal retourneert, kan deze kan worden afgestemd en geoptimaliseerd voor veelgebruikte worden uitgevoerd.

**Peiling gegevensquery** wordt alleen worden uitgevoerd wanneer de gegevens beschikbaar Query wordt aangegeven dat er gegevens beschikbaar is. Deze instructie wordt uitgevoerd binnen een transactie en is alleen vastgelegd wanneer de opgehaalde gegevens blijvend in de werkstroom opgeslagen worden. Het is belangrijk om te voorkomen oneindig dezelfde gegevens opnieuw te extraheren. De transacties aard van deze uitvoering kan worden gebruikt om te verwijderen of de gegevens zodat deze niet wordt de volgende keer dat gegevens wordt gevraagd verzameld bijwerken.

> [AZURE.NOTE] Het schema die het resultaat van deze instructie bevat de beschikbare eigenschappen in de verbindingslijn. Alle kolommen moeten worden genoemd.

#### <a name="data-available-query-example"></a>Voorbeeld van gegevens beschikbaar Query

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Voorbeeld van de Query gegevens peiling

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>De Trigger toevoegen
1. Wanneer maakt of een app logica bewerken, selecteert u de SQL-verbindingslijn die u hebt gemaakt als de trigger. Hier ziet u de beschikbare triggers: **Peiling-gegevens (JSON)** en **Peiling-gegevens (XML)**:  
![][5]

2. Selecteer de trigger **Peiling-gegevens (JSON)** , voert u de frequentie en klik op de ✓:  
![][6]

3. De trigger wordt nu weergegeven zoals deze is geconfigureerd in de logica-app. De output(s) van de trigger worden weergegeven en kan worden gebruikt als invoer in de volgende acties:  
![][7]

## <a name="use-the-connector-as-an-action"></a>De verbindingslijn gebruiken als een actie
Met onze eenvoudige logica app scenario welke gegevens uit een SQL-tabel probeert, voegt de gegevens in een andere tabel en werkt u de gegevens.

Als u wilt gebruiken de SQL-Connector als een actie, voer de naam van de tabellen en/of opgeslagen Procedures die u hebt ingevoerd wanneer u de SQL-Connector gemaakt:

1. Na de trigger (of kies de optie 'dit logica handmatig uitvoeren'), de SQL-verbindingslijn die u hebt gemaakt in de galerie toevoegen. Selecteer een van de acties invoegen, zoals *Invoegen in TempEmployeeDetails (JSON)*:  
![][8]

2. Voer de invoerwaarden van de record die moet worden ingevoegd en klik op de ✓:  
![][9]

3. Selecteer de dezelfde SQL-verbindingslijn die u hebt gemaakt in de galerie. Als een actie, selecteert u de actie bijwerken op dezelfde tabel, zoals *Update EmployeeDetails*:  
![][11]

4. Voer de invoerwaarden voor de bijwerkactie en klik op de ✓:  
![][12]

U kunt de logica app testen door een nieuwe record toe te voegen in de tabel die moet worden worden doorzocht.

## <a name="what-you-can-and-cannot-do"></a>Wat u wel en niet kunt doen

SQL-Query | Ondersteund | Niet ondersteund
--- | --- | ---
Waar component | <ul><li>Operatoren: En, of, =, <>, <, <, >, > = en zoals</li><li>Meerdere sub voorwaarden kunnen worden gecombineerd '(' en')'</li><li>Letterlijke, Datetime (tussen enkele aanhalingstekens), getallen (mogen alleen numerieke tekens)</li><li>Strikt moet een expressie binaire indeling, zoals ((operand operator operand) en/of (operand operator operand)) *</li></ul> | <ul><li>Operatoren: Tussen, IN</li><li>Alle ingebouwde functies, zoals ADD(), MAX() NOW(), POWER() en dergelijke</li><li>Rekenkundige operatoren zoals *, -, +, enzovoort</li><li>Samenvoegingen van tekenreeksen met +.</li><li>Alle Joins</li><li>IS NULL en IS niet Null</li><li>Getallen met niet-numerieke tekens, zoals hexadecimale getallen</li></ul>
Velden (in selectiequery) | <ul><li>Geldige kolomnamen gescheiden door komma's. Geen tabel naam voorvoegsels toegestaan (de verbindingslijn werkt op één tabel tegelijk).</li><li>Namen kunnen worden voorafgegaan door ' [' en '] "</li></ul> | <ul><li>Trefwoorden zoals de BOVENKANT, DISTINCT, enzovoort</li><li>Aliasing, zoals straat, plaats + Zip als adres</li><li>Alle ingebouwde functies, zoals ADD(), MAX() NOW(), POWER() en dergelijke</li><li>Rekenkundige operatoren, zoals *, -, +, enzovoort</li><li>Samenvoegingen van tekenreeksen met +</li></ul>

#### <a name="tips"></a>Tips

- Voor geavanceerde query's, wordt u aangeraden een opgeslagen procedure en uitvoeren met de procedure uitvoeren die zijn opgeslagen API maken.
- Wanneer u een inner-query's gebruikt, kunt u deze binnen opgeslagen procedures gebruiken.
- Voor deelname aan meerdere voorwaarden, kunt u de 'en' en 'Of' operatoren.

## <a name="hybrid-configuration-optional"></a>Hybride configuratie (optioneel)

> [AZURE.NOTE] Deze stap is vereist als u werkt met een lokale SQL Server achter de firewall.

App-Service gebruikt de hybride Configuration Manager veilig verbinden met uw on-premises-systeem. Als u verbindingslijn wordt gebruikt een lokale SQL Server bent, is de hybride Connection Manager is vereist.

> [AZURE.NOTE] Als u aan de slag met Azure logica Apps wilt voordat u zich registreert voor een Azure-account, gaat u naar [Logica-App proberen](https://tryappservice.azure.com/?appservice=logic), waar u direct een tijdelijk starter logica-app in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

Zie [werken met de hybride Connection Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Doe meer met de verbindingslijn
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een logica-App. Zie [Wat zijn de logica Apps?](app-service-logic-what-are-logic-apps.md).

De verwijzing Swagger REST API op [verbindingslijnen en API Apps verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=529766)bekijken.

U kunt ook prestaties statistieken besturingselement beveiliging en aan de verbindingslijn bekijken. Zie [beheren en controleren van de ingebouwde API-Apps en verbindingslijnen](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
