<properties
    pageTitle="Leer hoe u coderen of decoderen platte bestanden met de Enterprise-integratie Inpakken en logica apps | App-Service van Microsoft Azure | Microsoft Azure"
    description="Gebruik de functies van Enterprise Integration Pack en logica apps om coderen of decoderen platte bestanden"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Enterprise-integratie met platte bestanden

## <a name="overview"></a>Overzicht

U wilt coderen van XML-inhoud voordat u het hebt verzonden naar een partner bedrijven in een scenario voor bedrijven-to-business (B2B). In een logica-app die zijn aangebracht door de logica Apps-functie van de Azure App-Service, kunt u het platte bestand codering verbindingslijn u dit wilt doen. De logica-app die u maakt kunt krijgen de XML-inhoud van een verscheidenheid aan bronnen, inclusief uit een HTTP-verzoek-trigger, uit een andere toepassing of zelfs niet vanuit een van de vele [verbindingslijnen](../connectors/apis-list.md). Zie voor meer informatie over logica apps de [logica apps documentatie](./app-service-logic-what-are-logic-apps.md "meer informatie over de logica apps").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Het maken van het platte bestand codering verbindingslijn

Volg deze stappen om een plat bestand codering connector aan uw app logica toevoegen.

1. Maak een logica-app en [koppel deze aan uw account integratie](./app-service-logic-enterprise-integration-accounts.md "leren hoe u kunt een account integratie aan een app logica koppelen"). Dit account bevat het schema dat u gebruiken wilt voor het coderen van de XML-gegevens.  
2. Een **aanvraag - wanneer een HTTP-aanvraag wordt ontvangen** -trigger toevoegen aan uw logica-app.  
![Schermafbeelding van trigger selecteren](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Het platte bestand codering actie, als volgt toevoegen:

    een. Selecteer het **plus** -teken.

    b. Selecteer de koppeling **toevoegen een actie** (wordt weergegeven nadat u het plusteken (+) hebt geselecteerd).

    c. Voer *plat* als u wilt filteren van alle acties met de sjabloon die u wilt gebruiken in het zoekvak.

    d. Selecteer de optie voor het **Platte bestand-codering** in de lijst.   
![Schermafbeelding van plat bestandscodering optie](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. Selecteer het tekstvak **inhoud** in het dialoogvenster **Plat bestand-codering** .  
![Schermafbeelding van inhoud tekstvak](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Selecteer de tag hoofdtekst als de inhoud die u wilt coderen. De tag hoofdtekst vult de inhoud veld.     
![Schermafbeelding van de hoofdtekst tag](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Selecteer de keuzelijst **Schema** en kiest u het schema dat u gebruiken wilt voor het coderen van de invoer inhoud.    
![Schermafbeelding van de naam van de Schema keuzelijst met invoervak](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Sla uw werk.   
![Schermafbeelding van opslaan-pictogram](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Nu bent u klaar met de plat bestand codering verbindingslijn instellen. In een echte wereld-toepassing wilt u mogelijk de gecodeerde gegevens opslaan in een LOB-toepassing, zoals Salesforce. Of u kunt verzenden dat gecodeerde gegevens naar een handelsdag partner. U kunt eenvoudig een actie als u wilt verzenden de uitvoer van de tekstcodering actie Salesforce of uw werkbladen partner met behulp van een van de andere opgegeven verbindingslijnen toevoegen.

Nu kunt u de verbindingslijn testen door een verzoek om te bellen naar het eindpunt HTTP, en de XML-inhoud op te nemen in de hoofdtekst van het verzoek.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Het maken van het platte bestand decoderen verbindingslijn

>[AZURE.NOTE] Als u wilt deze stappen hebt voltooid, moet u beschikken over een schemabestand al is geüpload naar u integratie-account.

1. Een **aanvraag - wanneer een HTTP-aanvraag wordt ontvangen** -trigger toevoegen aan uw logica-app.  
![Schermafbeelding van trigger selecteren](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Het platte bestand decoderen actie, als volgt toevoegen:

    een. Selecteer het **plus** -teken.

    b. Selecteer de koppeling **toevoegen een actie** (wordt weergegeven nadat u het plusteken (+) hebt geselecteerd).

    c. Voer *plat* als u wilt filteren van alle acties met de sjabloon die u wilt gebruiken in het zoekvak.

    d. Selecteer de optie **Plat bestand decoderen** in de lijst.   
![Schermafbeelding van plat bestand decoderen optie](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Selecteer **het inhoudsbesturingselement** . Dit resulteert in een lijst met de inhoud van de eerdere stappen die u als de inhoud gebruiken kunt decoderen. U ziet dat de *hoofdtekst* van de binnenkomende HTTP-aanvraag beschikbaar is voor worden gebruikt als de inhoud decoderen. U kunt ook de inhoud moet decoderen rechtstreeks in het besturingselement **inhoud** invoeren.     
- Selecteer de tag *hoofdtekst* . Zoals u ziet dat de tag hoofdtekst bevindt zich nu in **het inhoudsbesturingselement** .
- Selecteer de naam van het schema dat u gebruiken wilt voor de inhoud decoderen. De volgende schermafbeelding ziet u dat *OrderFile* naam van het geselecteerde schema is. De naam van deze schema had eerder geüpload naar de integratie-account.

 ![Schermafbeelding van plat bestand decoderen dialoogvenster](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Sla uw werk.  
![Schermafbeelding van opslaan-pictogram](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Nu bent u klaar met het platte bestand decoderen verbindingslijn instellen. In een echte wereld-toepassing wilt u mogelijk de gecodeerde gegevens opslaan in een LOB-toepassing zoals Salesforce. U kunt eenvoudig een actie om de uitvoer van de decoderen actie verzenden naar Salesforce toevoegen.

Nu kunt u de verbindingslijn testen door een verzoek om te bellen naar het eindpunt HTTP, en met inbegrip van de XML-inhoud die u wilt decoderen in de hoofdtekst van het verzoek.  

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack").  
