<properties
    pageTitle="Lijst met beschikbare verbindingslijnen en API Apps | Microsoft Azure-App-Service"
    description="Lees meer over de verbindingslijnen en API-Apps in Azure App-Service"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Lijst met verbindingslijnen en API-Apps gebruiken in uw Apps logica
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie. Zie [Nieuwe verbindingslijnen lijst](../connectors/apis-list.md)voor de logica Apps algemene beschikbaarheid (GA)-versie.

Meer informatie over alle beschikbare connectors en API-Apps die Microsoft gebruik binnen uw Apps logica heeft gemaakt.

Zie [Azure App serviceprijzen](https://azure.microsoft.com/pricing/details/app-service/)voor prijzen informatie en een lijst met wat opgenomen in elke servicelaag is.

> [AZURE.NOTE] Ga naar [Logica-App proberen](https://tryappservice.azure.com/?appservice=logic)om te beginnen met logica Apps voordat u zich registreert voor een Azure-account. U kunt direct een tijdelijk starter logica-app maken in de App-Service. Geen creditcards vereist; geen verplichtingen.

## <a name="core-connectors"></a>Core verbindingslijnen
De volgende tabel bevat alle beschikbare connectors en API-Apps die door Microsoft is gemaakt die beschikbaar zijn als Core verbindingslijnen:

Naam | Beschrijving
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Bing gebruiken om tekst te vertalen in een andere taal.
[HTTP](app-service-logic-connector-http.md) | De HTTP luisteraar ervan af wordt geopend een eindpunt die fungeert als een HTTP-server en luistert naar binnenkomende HTTP of HTTPS-opdrachten. De HTTP-actie hoeft het niet een API-App en wordt ondersteund in logica-Apps.
[Toegestane vertraging](app-service-logic-connector-slack.md) | Verbinding maken met toegestane vertraging en berichten in tekort kanalen posten.


## <a name="enterprise-integration-connectors"></a>Enterprise-integratie verbindingslijnen
De volgende tabel bevat alle beschikbare verbindingslijnen en API-Apps die beschikbaar zijn als de Enterprise-integratie verbindingslijnen Microsoft heeft gemaakt:

Naam  | Beschrijving
------------- | -------------
[BizTalk regels](app-service-logic-use-biztalk-rules.md) | Regels voor BizTalk gebruiken om te definiëren en bepalen van de bedrijfslogica binnen een organisatie. Bedrijfsbeleid kunnen worden bijgewerkt zonder compileren of zonder de bijbehorende toepassingen opnieuw te distribueren.
[BizTalk XPath Extractor](app-service-logic-xpath-extract.md) | Moeten worden opgezocht en haalt gegevens uit XML-inhoud op basis van een XPath die u kiest.
[DB2-Connector](app-service-logic-connector-db2.md) | Hiermee maakt u verbinding een IBM DB2-database on-premises implementatie en klik op een Azure virtuele machines waarop u een Windows-besturingssysteem wordt uitgevoerd. Web API- en OData-API-bewerkingen kunt worden toewijzen aan opdrachten Informix Structured Query Language. <br/><br/>Geen triggers. Acties opnemen tabel selecteren, invoegen, update, delete en aangepaste-instructie<br/><br/>Deze connector bevat ook de Microsoft-Client voor DRDA verbinding maken met een Informix-server via een TCP/IP-netwerk.
[Bestand](app-service-logic-connector-file.md) | Met deze connector, u kunt verbinding maken met het bestandssysteem on-premises implementatie of het netwerk en het volledige ander Bestandstaken, zoals uploaden, verwijderen, vermelding bestanden en meer.
[Informix](app-service-logic-connector-informix.md) | Maakt verbinding met een IBM Informix-database, on-premises implementatie en klik op een Azure virtuele machines waarop een Windows-besturingssysteem wordt uitgevoerd. Web API- en OData-API-bewerkingen kunt worden toewijzen aan opdrachten Informix Structured Query Language.<br/><br/>Geen triggers. Acties bevatten tabel selecteren, invoegen, update, delete en aangepaste-instructie.<br/><br/>Wanneer u een on-premises implementatie gebruikt, kunnen VPN of Azure ExpressRoute kan worden gebruikt. Deze connector bevat ook een Microsoft-Client voor DRDA verbinding maken met een Informix-server via een TCP/IP-netwerk.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Maakt verbinding met on-premises SQL Server- of een Azure SQL-Database. U kunt maken, bijwerken, ophalen en verwijderen van items in een tabel met SQL-database.
MQ | Maakt verbinding naar IBM WebSphere MQ Server versie 8 on-premises implementatie en klik op een Azure virtuele machines waarop een Windows-besturingssysteem wordt uitgevoerd. Wanneer u een on-premises implementatie gebruikt, kunnen VPN of Azure ExpressRoute kan worden gebruikt. De verbindingslijn bevat ook de Microsoft-Client voor MQ.<br/><br/>Geen triggers. Geen acties.<br/><br/>**Opmerking** Momenteel kunnen niet worden gebruikt met logica Apps.

## <a name="connectors-as-triggers"></a>Verbindingslijnen als Triggers
Verschillende verbindingslijnen bieden triggers voor logica-Apps. Deze triggers zijn van de twee typen:

1. Peiling Triggers: Deze triggers poll uitvoeren onder uw service met een opgegeven frequentie wilt controleren op nieuwe gegevens. Wanneer u nieuwe gegevens beschikbaar is, wordt een nieuw exemplaar van uw App logica uitgevoerd met de gegevens die als invoer. Om te voorkomen dat dezelfde gegevens wordt verbruikt meerdere keren, de trigger mogelijk opschoning gegevens dat is gelezen en doorgegeven aan de logica-App. Voorbeelden van dergelijke verbindingslijnen zijn bestand en SQL Azure-opslag.
2. Push-Triggers: Deze triggers luisteren voor gegevens op een eindpunt of voor een gebeurtenis plaatsvindt. Kies vervolgens een nieuw exemplaar van een App logica activeren. Voorbeelden van dergelijke verbindingslijnen zijn HTTP luisteraar ervan af en Twitter.

## <a name="connectors-as-actions"></a>Verbindingslijnen als acties
Verbindingslijnen kunnen ook worden gebruikt als acties binnen uw App logica. Acties zijn handig voor het opzoeken van gegevens binnen de logica App die u kunt vervolgens in de uitvoering gebruikt. Bijvoorbeeld, moet u zoeken naar gegevens uit een SQL-database voor meer informatie over een klant tijdens het verwerken van een order. Of u wilt schrijven, bijwerken of verwijderen van gegevens in een bestemming. U kunt dit doen met de acties die is verstrekt door de verbindingslijnen. Acties toewijzen aan bewerkingen in API-Apps (zoals gedefinieerd door de metagegevens Swagger).

## <a name="create-your-own-connectors-and-api-apps"></a>Uw eigen verbindingslijnen en API-Apps maken
[Verbindingslijnen en API Apps verwijzing](http://aka.ms/appservicesconnectorreference)  
[Azure App Service API app-triggers](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logica App verwijzing](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Meer informatie over de verbindingslijnen en API-Apps
[Wat zijn de verbindingslijnen en BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md)  
[Gebruik van de hybride Connection Manager in Azure App-Service](app-service-logic-hybrid-connection-manager.md)  
[Beheren en controleren van de ingebouwde API-Apps en verbindingslijnen](app-service-logic-monitor-your-connectors.md)
