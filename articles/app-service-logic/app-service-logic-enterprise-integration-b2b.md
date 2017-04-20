<properties 
    pageTitle="B2B oplossingen te maken met Enterprise Integration Pack | App-Service van Microsoft Azure | Microsoft Azure" 
    description="Meer informatie over het ontvangen van gegevens met behulp van de functies B2B van het taalpakket Enterprise-integratie" 
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

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Meer informatie over het ontvangen van gegevens met behulp van de functies B2B van het taalpakket Enterprise-integratie#

## <a name="overview"></a>Overzicht ##

In dit document maakt deel uit van het taalpakket logica Apps Enterprise-integratie. Lees dan het overzicht voor meer informatie over de [mogelijkheden van het taalpakket Enterprise-integratie](./app-service-logic-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Vereisten voor ##

Gebruik van de AS2 en X12 acties moet u een Account van de integratie Enterprise

[Het maken van een Account van de integratie Enterprise](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Het gebruik van de logica Apps B2B verbindingslijnen ##

Zodra u hebt een integratie-account gemaakt en toegevoegd partners en overeenkomsten ernaar u gereed bent om het maken van een App logica implementeert die een werkstroom voor bedrijven (B2B).

In dit walkthru ziet u het gebruik van de AS2 en X12 acties om een bedrijven logica toepassing dat gegevens van een partner werkbladen ontvangt te maken.

1. Maak een nieuwe logica app en [koppel deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md).  
2. Een **aanvraag - wanneer een HTTP-aanvraag wordt ontvangen** -trigger toevoegen aan uw logica-app  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. De actie **AS2 decoderen** door eerst te selecteren **een actie toevoegen** toevoegen  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Voer de word- **as2** in het zoekvak om te filteren van alle acties met de sjabloon die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Selecteer de actie **AS2 - decoderen AS2 bericht**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Zoals u ziet, de **hoofdtekst** die u gaat toevoegen als invoer. In dit voorbeeld, selecteert u de hoofdtekst van het HTTP-verzoek die de logica app geactiveerd. U kunt ook een expressie voor het invoeren van de koppen in het veld**KOPTEKSTEN** invoeren:

    @triggerOutputs()['headers']

8. De **kop** die zijn vereist voor AS2 toevoegen. Deze worden weergegeven in de koptekst van de HTTP-verzoek. In dit voorbeeld, selecteert u de koppen van de HTTP-aanvraag die de logica app geactiveerd.
9. Nu de decoderen X12 berichtactie opnieuw door **toevoegen** te selecteren een actie toevoegen  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Voer de word- **x12** in het zoekvak om te filteren van alle acties met de sjabloon die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Selecteer de **X12-decoderen X12 bericht** actie toe te voegen aan de logica-app  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. U moet nu opgeven van de invoer voor deze actie die de uitvoer van de bovenstaande AS2-actie. De inhoud van de werkelijke bericht zich in een object JSON en base64 gecodeerd. Daarom moet u een expressie opgeven, zoals de invoer zodat u de volgende expressie opgeven in het **X12 PLATTE bestand bericht aan decoderen** invoer veld  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Deze stap wordt de X12 gegevens ontvangen van de werkbladen partner en de gegevens uit een aantal items in een object JSON decoderen. Om te laten de partner kent van de ontvangst van de gegevens kunt u terug een antwoord met het AS2 bericht voor bestemming melding (MDN) in een actie HTTP-antwoord verzenden  
14. De actie **antwoord** toevoegen door **een actie toevoegen** te selecteren   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Voer het word- **antwoord** in het zoekvak om te filteren van alle acties met de sjabloon die u wilt gebruiken  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Selecteer de actie **antwoord** toe te voegen  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Het veld van de **hoofdtekst** antwoord instellen met behulp van de volgende expressie voor toegang tot de MDN uit de resultaten van de actie **decoderen X12 bericht**  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Sla uw werk  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

U bent nu klaar instellen van uw app B2B logica. In een echte wereld-toepassing, wilt u mogelijk de gecodeerde X12 opslaan gegevens in een LOB-toepassing of gegevens archief. U kunt eenvoudig verdere acties wilt werkwijze of schrijven van aangepaste API's om te verbinden met uw eigen LOB-toepassingen en deze API's gebruiken in uw app logica toevoegen.

## <a name="features-and-use-cases"></a>Functies en gebruik dozen ##

- De AS2 en X12 decoderen en coderen van acties kunt u gegevens uit ontvangen en gegevens verzendt naar handelsdag partners gebruikt industriële standaard protocollen logica apps gebruiken  
- U kunt AS2 en X12 met of zonder elkaar gegevens uitwisselen met werkbladen partners zoals vereist
- De acties B2B vergemakkelijkt partners en overeenkomsten maken in het Account dat integratie en deze gebruiken in een app logica  
- Door uw app logica met andere acties uit te breiden kunt u verzenden en ontvangen van gegevens naar en vanuit andere toepassingen en services zoals SalesForce  

## <a name="learn-more"></a>Meer informatie ##

[Meer informatie over het taalpakket Enterprise-integratie](./app-service-logic-enterprise-integration-overview.md)  