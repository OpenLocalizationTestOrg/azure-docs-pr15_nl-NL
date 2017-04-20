<properties
   pageTitle="Met de HTTP luisteraar ervan af en de verbindingslijn in logica Apps | Microsoft Azure-App-Service "
   description="Het maken en configureren van de luisteraar ervan af HTTP en HTTP actie verbindingslijn of API-app en deze gebruiken in een app logica in Azure App-Service"
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
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Aan de slag met de luisteraar ervan af HTTP en HTTP actie en deze toevoegen aan uw logica-App

> [AZURE.NOTE]We zijn ondersteuning voor deze connector beëindigd omdat de functionaliteit wordt nu geleverd al dan niet standaard als **handmatige trigger** bij het maken van nieuwe logica apps.  We raden u al uw logica apps die van deze verbindingslijn gebruikmaken.  
> Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

Sluit rechtstreeks aan bronnen HTTP luisteren naar HTTP aanvragen en configureren van HTTP-webaanvragen. Er zijn enkele scenario's waarin u werken met directe HTTP-verbindingen moet mogelijk, waaronder:

1.  Een logica App die ondersteuning biedt voor een Internet of een mobiele gebruiker interactieve front-end ontwikkelen.
2.  Voor het ophalen en gegevens uit een webservice die geen een automatisch niet vak verbindingslijn verwerken.
3.  Acties wilt uitvoeren die al weergegeven als een webservice, maar niet beschikbaar als een app-API worden.

Er zijn twee opties voor deze scenario's:

1. **HTTP luisteraar ervan af**: deze connector fungeert als een trigger en luistert naar HTTP-aanvragen op een geconfigureerde eindpunt. Wanneer een oproep op het geconfigureerde eindpunt ontvangt, een nieuw exemplaar van de stroom activeert en de gegevens die zijn ontvangen in de aanvraag voor de stroom voor verwerking worden doorgegeven. Dit kan ook worden geconfigureerd automatisch beantwoord de binnenkomende aanvraag wanneer de stroom heeft gestart, of u kunt een antwoord op basis van de uitvoering stroom maken en een antwoord wordt verzonden naar de beller.
2. **HTTP-actie**: Hiermee kunt u een web-verzoek om alle beschikbare eindpunt configureren op internet, krijgt weer een reactie en maakt het beschikbaar voor extra acties in de stroom in beslag neemt.

Logica apps kunnen activeren op basis van een groot aantal gegevensbronnen en verbindingslijnen aanbieding voor het ophalen en gegevens als onderdeel van de stroom van proces. U kunt de HTTP-verbindingslijn toevoegen aan uw zakelijke werkstroom en proces gegevens als onderdeel van deze werkstroom binnen een logica-App. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Een HTTP luisteraar ervan af voor de logica-App maken
Een verbindingslijn binnen een app logica kan worden gemaakt of rechtstreeks van Azure Marketplace worden gemaakt. Een verbindingslijn van Marketplace maken:  

1. Selecteer in de Azure startboard **Marketplace**.
2. Zoeken naar 'HTTP', selecteert u HTTP luisteraar ervan af en selecteer **maken**.
3.  Als volgt te werk om de HTTP luisteraar ervan af te configureren:  
![][1]

4.  Wanneer u de pakketinstellingen instelt, ziet u de volgende optie in het automatisch beantwoorden of moet u van de luisteraar ervan af om een expliciete antwoord wordt verzonden. Stelt u dit op **Onwaar** om uw eigen antwoord te sturen:  
![][2]

5.  Klik op **OK** om te maken.
6.  Nadat het exemplaar van de app API is gemaakt, opent u de instellingen wilt configureren van de beveiliging. De HTTP luisteraar ervan af ondersteunt momenteel basisverificatie. U kunt dit configureren met de optie beveiliging bij het openen van de HTTP luisteraar ervan af:  
![][3]
  
    **Bekend probleem** *De beveiliging instellingen weergeven "Geen" Als de standaardwaarde, maar deze ongedefinieerd is. Moet u de instelling wijzigen naar Basic en terug naar geen voordat u deze om ervoor te zorgen dat de HTTP luisteraar ervan af correct is geconfigureerd.*  

7. Stel ten slotte de beveiligingsinstellingen van de API-App op openbare (anoniem) toe te staan dat externe klanten voor toegang tot het eindpunt. Deze instelling is beschikbaar onder ' alle instellingen > Toepassingsinstellingen "van de HTTP luisteraar ervan af API-App:![][10]

Wanneer u dat hebt gedaan, kunt u nu een app logica voor het gebruik van de HTTP luisteraar ervan af.

## <a name="using-the-http-listener-in-your-logic-app"></a>Gebruik van de HTTP luisteraar ervan af in uw App logica
Nadat uw API-app is gemaakt, kunt u nu de HTTP luisteraar ervan af als een trigger gebruiken voor uw App logica. Hiervoor moet u:

4.  Maak een nieuwe logica-App.
5.  Open "Triggers en acties" om de logica Apps Designer te openen en uw mailstromen configureren. De HTTP luisteraar ervan af wordt weergegeven in de galerie. Selecteer deze.
6.  Nu kunt u de HTTP-methode en de relatieve URL waarop u de luisteraar ervan af zo activeren dat de stroom vereist instellen:  
![][4]  
![][5]

7.  Als u de volledige URI, dubbelklikt u op de HTTP luisteraar ervan af om te bekijken van de configuratie-instellingen en kopieer de URL voor de "Host" van de API-app:  
![][6]
8.  Nu kunt u de gegevens die zijn ontvangen in het vergaderverzoek HTTP in andere acties in de stroom als volgt:  
![][7]  
![][8]
9.  Ten slotte, als u wilt een antwoord verzenden, voeg luisteraar ervan af met een ander HTTP en selecteert u de actie HTTP-antwoord verzenden. Stel de ID aanvragen op aanvraag-id uit de HTTP luisteraar ervan af opgehaald en de hoofdtekst van het antwoord en HTTP-status die u wilt retourneren terug te vullen:  
![][9]

## <a name="using-the-http-action"></a>Gebruik van de HTTP-actie
De actie HTTP wordt ondersteund door logica Apps en hoeft het niet een API-app moet worden gemaakt eerste als u wilt kunnen gebruiken. U kunt een actie HTTP invoegen op een willekeurige plaats in uw App logica en kies de URI, koppen en hoofdtekst voor het gesprek.
De actie HTTP ondersteunt meerdere opties voor client naast beveiliging. Zie de [Beveiligingsopties kant](../scheduler/scheduler-outbound-authentication.md).

De uitvoer van de actie HTTP is kop- en de tekst die kan worden gebruikt verder lager in de stroom vergelijkbaar met hoe uitvoer van andere acties en verbindingslijnen wordt verbruikt.

## <a name="do-more-with-your-connector"></a>Doe meer met de verbindingslijn
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke werkstroom met een logica-App. Zie [Wat zijn de logica Apps?](app-service-logic-what-are-logic-apps.md).

De verwijzing Swagger REST API op [verbindingslijnen en API Apps verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=529766)bekijken.

U kunt ook prestaties statistieken besturingselement beveiliging en aan de verbindingslijn bekijken. Zie [beheren en controleren van de ingebouwde API-Apps en verbindingslijnen](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Als u aan de slag met Azure logica Apps wilt voordat u zich registreert voor een Azure-account, gaat u naar [De logica-App proberen](https://tryappservice.azure.com/?appservice=logic). U kunt direct een tijdelijk starter logica-app maken in de App-Service. Geen creditcards vereist; geen verplichtingen.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
