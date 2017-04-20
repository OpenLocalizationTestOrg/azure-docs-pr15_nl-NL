<properties 
    pageTitle="Overzicht van XML-validatie in de Enterprise-integratie Pack | App-Service van Microsoft Azure | Microsoft Azure" 
    description="Informatie over de werking van gegevensvalidatie in de Enterprise-integratie Inpakken en logica-apps" 
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

# <a name="enterprise-integration-with-xml-validation"></a>Enterprise-integratie met XML-validatie

## <a name="overview"></a>Overzicht
Vaak in B2B scenario's moeten de partners bij een overeenkomst voor het valideren van berichten die deze tussen elkaar uitwisselen zijn geldig voordat verwerking van de gegevens kunt. In de Enterprise-integratie Pack, kunt u de verbindingslijn XML-validatie gebruiken voor het valideren van documenten tegen een vooraf gedefinieerde schema.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Het valideren van een document met de XML-validatie verbindingslijn
1. Maak een logica app en [koppel deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md "leren hoe u kunt een account integratie aan een app logica koppelen") die het schema dat u gebruiken wilt voor het valideren van de XML-gegevens bevat.
2. Een **aanvraag - wanneer een HTTP-aanvraag wordt ontvangen** -trigger toevoegen aan uw logica-app  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. De actie **XML-validatie** door eerst te selecteren **een actie toevoegen** toevoegen  
4. *Xml* invoeren in het zoekvak om te filteren van alle acties met de sjabloon die u wilt gebruiken 
5. **XML-validatie** selecteren     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Selecteer het tekstvak **inhoud**  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Selecteer de tag hoofdtekst als de inhoud die wordt gevalideerd.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Selecteer de **Naam SCHEMA** keuzelijst met invoervak en kies het schema dat u gebruiken wilt voor het valideren van de invoer *inhoud* bovenstaande     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Sla uw werk  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Nu bent u klaar met de verbindingslijn validatie instellen. In een echte wereld-toepassing wilt u mogelijk de gevalideerde gegevens opslaan in een LOB-toepassing zoals SalesForce. U kunt eenvoudig een actie om de uitvoer van de validatie verzenden naar Salesforce toevoegen. 

Nu kunt u de actie Validatieregels testen door een aanvraag naar het eindpunt HTTP.  

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")   