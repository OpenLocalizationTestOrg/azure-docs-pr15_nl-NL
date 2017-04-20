<properties
    pageTitle="De verbindingslijn bestand gebruiken in logica apps | Microsoft Azure-App-Service"
    description="Het maken en configureren van de verbindingslijn van bestand of de API-app en deze gebruiken in een app logica in Azure App-Service"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Aan de slag met de verbindingslijn bestand en deze toevoegen aan uw logica-app
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

Verbinding maken met een bestandssysteem als u wilt uploaden, downloaden en meer tot uw bestanden op een hostcomputer. Logica apps kunnen activeren op basis van een groot aantal gegevensbronnen en verbindingslijnen aanbieding voor het ophalen en gegevens verwerken. U kunt de verbindingslijn bestand toevoegen aan uw zakelijke werkstroom en proces gegevens als onderdeel van deze werkstroom binnen een logica-app. 

De verbindingslijn bestand wordt de hybride Connection Manager gebruikt voor hybride connectiviteit met het bestandssysteem host.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Een verbindingslijn bestand voor de logica-app maken ##
Als u wilt de verbindingslijn bestand gebruiken, moet u eerst een exemplaar van het bestand verbindingslijn API-app maakt. Dit u kunt doen als volgt:

1.  Open de Azure Marketplace met + nieuwe optie aan de linkerkant van de Azure-Portal.
2.  Zoeken naar "bestand verbindingslijn".
3.  Selecteer **Bestand verbindingslijn (preview)** in de zoekresultaten.
4.  Selecteer de knop **maken**
5.  Configureer de verbindingslijn bestand als volgt:  
![][1]

    - **Naam** - geven een naam voor de verbindingslijn bestand
    - **Package Settings**
        - **Hoofdmap** - pad naar de hoofdmap-map op uw hostcomputer opgeven. Bv. D:\FileConnectorTest
        - **Service Bus verbindingsreeks** - Geef de verbindingsreeks van een Service-Bus. Zorg ervoor dat de service bus naamruimte van het type standaard is en geen eenvoudige om te staan voor gebruik van de Service Bus relais.  Service Bus Relay wordt gebruikt om te koppelen aan de hybride Connection Manager.
    - **App-serviceplan** - selecteren of maken van een App-serviceplan
    - **Prijzen van laag** - kiest u een laag prijzen voor de verbindingslijn
    - **Resourcegroep** - Selecteer of maak een resourcegroep waarin de verbindingslijn zich moet bevinden
    - **Abonnement** - Kies een abonnement dat u deze connector worden gemaakt wilt in
    - **Locatie** - Kies de geografische locatie waar u de verbindingslijn worden geïmplementeerd zou nu

4. Klik op maken. Een nieuw bestand verbindingslijn wordt gemaakt

## <a name="configure-hybrid-connection-manager"></a>Hybride Connection Manager configureren ##
Nadat het exemplaar API-App is gemaakt, blader naar de dashboard.  Dit kan gebeuren door te klikken op Bladeren > API Apps > selecteert u de verbindingslijn bestand API-App.  Hier kunt moet de hybride Connection Manager worden geconfigureerd.
Zie [gebruik van de hybride Connection Manager]voor meer informatie over het configureren en probleemoplossing van de hybride Connection Manager.

## <a name="using-the-file-connector-in-your-logic-app"></a>De verbindingslijn bestand gebruiken in uw logica-app ##
Nadat uw API-app is gemaakt, kunt u nu de verbindingslijn bestand gebruiken als een actie voor de logica-app. Hiervoor moet u:

1.  Een nieuwe logica-app maakt en kiest u dezelfde resourcegroep waarvoor de verbindingslijn bestand. Volg de instructies naar [een nieuwe logica-app maken].

2.  Open "Triggers en acties" binnen de gemaakte logica-app naar de logica apps Designer te openen en uw mailstromen configureren.

3.  De verbindingslijn bestand weergegeven in de sectie 'API Apps in deze resourcegroep' in de galerie aan de rechterkant.

4.  U kunt het bestand verbindingslijn API-app neerzetten in de editor door te klikken op de verbindingslijn"bestand". bestand verbindingslijn beschrijft één trigger en 4 acties:  
![][5]

6.  Elke één van deze beschrijft bepaalde eigenschappen. De onderstaande afbeelding bevat de eigenschappen voor de trigger en krijgt bestand actie:  
![][6]

7. Nadat deze zijn geconfigureerd, kan de Trigger en een actie worden gebruikt in uw flow. Andere acties kunnen op dezelfde manier als u ook worden geconfigureerd.

> [AZURE.NOTE] De trigger bestand wordt het bestand verwijderen nadat deze met succes uit de map wordt gelezen.

## <a name="file-connector-rest-apis"></a>Bestand verbindingslijn REST API 's ##
Als u wilt gebruiken de verbindingslijn buiten een logica-app, kunnen de REST API's die worden aangeboden door de verbindingslijn worden gebruikt. U kunt deze API definities bladeren met-Api App > Weergave -> bestand verbindingslijn. Klik nu op de lens API definitie onder de sectie Overzicht om te bekijken van alle de API's die worden aangeboden door deze connector:  
![][7]

Details van de API's vindt u op [bestand verbindingslijn API definitie].

## <a name="do-more-with-your-connector"></a>Doe meer met de verbindingslijn
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een logica-app. Zie [Wat zijn de logica apps?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Als u aan de slag met Azure logica apps wilt voordat u zich registreert voor een Azure-account, gaat u naar [probeert logica app](https://tryappservice.azure.com/?appservice=logic), waar u direct een tijdelijk starter logica-app in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

De verwijzing Swagger REST API op [verbindingslijnen en API Apps verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=529766)bekijken.

U kunt ook prestaties statistieken besturingselement beveiliging en aan de verbindingslijn bekijken. Zie [beheren en controleren van de ingebouwde API-Apps en verbindingslijn](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Een nieuwe logica-app maken]: app-service-logic-create-a-logic-app.md
[Bestand verbindingslijn API definitie]: https://msdn.microsoft.com/library/dn936296.aspx
[Gebruik van de hybride Connection Manager]: app-service-logic-hybrid-connection-manager.md
