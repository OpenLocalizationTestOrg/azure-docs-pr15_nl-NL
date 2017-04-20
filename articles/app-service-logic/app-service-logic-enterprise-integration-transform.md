<properties 
    pageTitle="Overzicht van Enterprise-integratie Pack | App-Service van Microsoft Azure | Microsoft Azure" 
    description="Gebruik de functies van Enterprise Integration Pack om het proces en integratie scenario's voor bedrijven met Microsoft Azure App-service inschakelen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise-integratie met XML-transformaties

## <a name="overview"></a>Overzicht
De Enterprise-integratie transformeren verbindingslijn worden gegevens van de ene indeling geconverteerd naar een andere indeling. U wellicht bijvoorbeeld een binnenkomend bericht met de huidige datum in de indeling YearMonthDay. U kunt een transformatie opmaak van de datum in de MonthDayYear-indeling wijzigen.

## <a name="what-does-a-transform-do"></a>Wat doet een transformatie?
Een transformeren, dat wil ook bekend als een kaart zeggen, bestaat uit een bron XML-schema (de invoer) en een doel XML-schema (de uitvoer). U kunt verschillende ingebouwde functies gebruiken om te bewerken of te bepalen welke gegevens, inclusief tekenreeks te springen, voorwaardelijke toewijzingen, rekenkundige expressies, datum tijd formatters en zelfs herhalen constructies.

## <a name="how-to-create-a-transform"></a>Het maken van een transformatie?
U kunt een transformeren/kaart maken met behulp van de Visual Studio [Enterprise integratie SDK](https://aka.ms/vsmapsandschemas). Wanneer u klaar bent met maken en te testen van de transformatie, kunt u de transformatie uploaden naar uw account integratie. 

## <a name="how-to-use-a-transform"></a>Het gebruik van een transformatie
Nadat u de transformatie naar uw account integratie uploaden, kunt u deze om een logica-toepassing te maken. De logica-app wordt uw transformaties vervolgens uitgevoerd wanneer de app logica wordt geactiveerd (en dat is invoer inhoud die moet worden omgezet).

**Hier volgen de stappen voor het gebruik van een transformatie**:

### <a name="prerequisites"></a>Vereisten voor 
Klik in het voorbeeld moet u:  

-  [Een container Azure functies maken] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Een container Azure functies maken")  
-  [Toevoegen, een functie aan de container Azure-functies] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Deze sjabloon maakt u een webhook op basis van C# azure functie met transformeren mogelijkheden gebruiken in de logica apps integratie scenario 's")    
-  Maak een account integratie en een kaart toevoegen aan deze  

>[AZURE.TIP] Noteer de de naam van de container Azure-functies en de functie Azure, moet u deze in de volgende stap.  

Nu u hebt afgehandeld van de vereisten, is het tijd om uw logica-toepassing te maken:  

1. Maak een logica app en [koppel deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md "leren hoe u kunt een account integratie aan een app logica koppelen") met de kaart.
2. Een **aanvraag - wanneer een HTTP-aanvraag wordt ontvangen** -trigger toevoegen aan uw logica-app  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. De actie **Transformeren XML-** door eerst te selecteren **een actie toevoegen** toevoegen   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. De word *Transformeren* invoeren in het zoekvak om te filteren van alle acties met de sjabloon die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Selecteer de actie **XML transformeren**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Selecteer de **Functie CONTAINER** met de functie die u wilt gebruiken. Dit is de naam van de functies van Azure-container die u eerder in deze stappen hebt gemaakt.
7. Selecteer de **functie** die u wilt gebruiken. Dit is de naam van de Azure-functie die u eerder hebt gemaakt.
8. De XML- **inhoud** die u wordt transformeren toevoegen. Houd er rekening mee dat u een XML-gegevens die u in het HTTP-verzoek als de **inhoud ontvangt**kunt gebruiken. In dit voorbeeld, selecteert u de hoofdtekst van het HTTP-verzoek die de logica app geactiveerd.
9. Selecteer de naam van de **MAP** die u gebruiken wilt voor het uitvoeren van de transformatie. De plattegrond moet al in uw account integratie. In een eerdere stap heeft u al uw logica app toegang aan uw account voor integratie met de kaart.
10. Sla uw werk  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

U bent nu klaar instellen van uw map. In een echte wereld-toepassing wilt u mogelijk de getransformeerd gegevens opslaan in een LOB-toepassing zoals SalesForce. U kunt eenvoudig als een actie aan de uitvoer van de transformatie verzenden naar Salesforce. 

U kunt nu de transformatie testen door een aanvraag naar het HTTP-eindpunt.  

## <a name="features-and-use-cases"></a>Functies en gebruik dozen

- De transformatie die is gemaakt in een map kan zijn eenvoudig zijn, zoals een naam en het adres van het ene document kopiëren naar een andere. Of, kunt u meer complexe transformaties de bewerkingen out-van-het-box-map maken.  
- Verschillende bewerkingen van de kaart of functies zijn gemakkelijk beschikbaar is, met inbegrip van tekenreeksen, datum time-functies, enzovoort.  
- U kunt een kopie directe gegevens tussen de schema's kunt doen. In de toewijzing is opgenomen in de SDK, is dit net zo eenvoudig als het tekenen van een lijn die de elementen in het schema van de gegevensbron met hun collega's in het schema bestemming verbindt.  
- Bij het maken van een kaart, weergave u een grafische van de kaart, waarin alle relaties en koppelingen die u maakt weergeven.
- Gebruik de functie Test kaart om toe te voegen een XML-voorbeeldbericht. U kunt met een eenvoudige klik testen van de map die u hebt gemaakt en verschijnt de gegenereerde uitvoer.  
- Bestaande kaarten uploaden  
- Biedt ondersteuning voor de XML-indeling.


## <a name="learn-more"></a>Meer informatie
- [Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")  
- [Meer informatie over kaarten] (./app-service-logic-enterprise-integration-maps.md "Algemene informatie over de integratie van de enterprise-kaarten")  
 