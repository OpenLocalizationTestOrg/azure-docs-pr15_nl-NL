<properties 
    pageTitle="Wat zijn de logica Apps?" 
    description="Meer informatie over het App-Service logica Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Wat zijn de logica Apps?

Logica Apps om er zelf een toe om te vereenvoudigen en implementeren van scalable integraties en werkstromen in de cloud. De ontwerper van een visuele als u wilt modelleren en uw automatiseren als een reeks stappen bekend als een werkstroom voor het krijgen.  Zijn er [veel verbindingslijnen](../connectors/apis-list.md) in de cloud en on-premises snel integreren over services en protocollen.  Een app logica begint met een trigger (like 'wanneer u een account wordt toegevoegd aan Dynamics CRM') en nadat starten met het aantal combinaties acties, conversies en voorwaarde logica beginnen kunt.

Dit zijn de voordelen van het gebruik van de logica Apps:  

- Wat tijd bespaart met het ontwerpen van complexe processen met eenvoudig kunnen begrijpen ontwerphulpmiddelen
- Patronen en werkstromen naadloos implementeren, dat anders is moeilijk te implementeren in code
- Snel aan de slag van sjablonen
- Het aanpassen van uw app logica met uw eigen aangepaste API's, code en acties
- Verbinding maken en verschillende systemen synchroniseren via on-premises en de cloud
- Mensen uit de BizTalk-server, API Management Azure-functies en Azure Service Bus bouwen met ondersteuning voor eersteklas-integratie

Logica Apps is een volledig beheerde iPaaS (integratie Platform als een Service) zodat ontwikkelaars niet hoeft te hosten, schaalbaarheid, beschikbaarheid en beheerbaarheid samenstellen.  Logica Apps worden automatisch uitgebreid naar voldoen aan de vraag.

![Stroom app designer](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Zoals vermeld, met logica Apps, kunt u bedrijfsprocessen automatiseren. Hier volgen enkele voorbeelden:  
 
* Bestanden die zijn geüpload naar een FTP-server op te slaan Azure verplaatsen
* Proces- en doorsturen orders via on-premises implementatie en cloud systemen
* Alle tweets over een bepaald onderwerp bewaken, alsjeblieft analyseren en waarschuwingen en taken voor items die moeten worden opvolgen maken.

Scenario's zoals deze kunnen worden geconfigureerd vanaf de visuele ontwerpfunctie en zonder het schrijven van één regel met code. Krijg gestart [samenstellen van uw app logica nu][create].  Nadat geschreven - kan een app logica [snel geïmplementeerd en geconfigureerd](app-service-logic-create-deploy-template.md) zijn op meerdere omgevingen en regio's.

## <a name="why-logic-apps"></a>Waarom logica Apps?

Logica Apps levert de enterprise-integratie ruimte snelheid en schaalbaarheid.  Het gebruiksgemak van de ontwerpfunctie voor groot aantal beschikbare triggers en acties en krachtige beheerprogramma's zorg centraal opslaan van uw API's eenvoudiger dan ooit.  Zoals bedrijven naar digitalization verplaatsen, wordt er logica Apps kunt u oudere en geavanceerde systemen met elkaar verbinden.

Daarnaast kunnen met onze [Enterprise integratie Account] [ biztalk] kunt u de schaal om de integratie van scenario's met de macht van een [XML-berichtenservice]vervallen[xml], [handelsdag partner management][tpm], en nog veel meer.

- **Eenvoudig te gebruiken ontwerphulpmiddelen** - logica Apps kan zijn ontworpen end-to-end in de browser of met Visual Studio tools. Beginnen met een trigger - van een eenvoudige plan wanneer een probleem met de GitHub wordt gemaakt. Vervolgens goedkeuringen door een willekeurig aantal acties met de uitgebreide galerie met verbindingslijnen.

- **Verbinding-API's eenvoudig** -Even samenstelling taken die gemakkelijk zijn te beschrijven zijn moeilijk te implementeren in code. Logica Apps kunt u gemakkelijk ongelijke systemen. Uw marketing-oplossing voor uw on-premises cloud verbinding wilt maken systeem facturering? Wilt u centraal messaging tussen API's en -systemen met een Enterprise Service Bus? Logica apps zijn de snelste, meest betrouwbare manier om aan te geven van oplossingen voor deze problemen.

- **Aan de slag snel van sjablonen** - om u aan de slag te helpen we een [galerie met sjablonen] hebt opgegeven[ templates] waarmee u kunt u snel enkele veelgebruikte oplossingen. Geavanceerde uit B2B oplossingen naar eenvoudige SaaS-connectiviteit en zelfs een paar die alleen 'voor een goede' - de galerie is de snelste manier om aan de slag met de kracht van logica Apps.

- **Uitbreiden warm-in** - niet wordt weergegeven de verbindingslijn die u nodig hebt? Logica Apps is ontworpen voor gebruik met uw eigen API's en code; uw eigen API-app als u wilt gebruiken als een aangepaste verbindingslijn of inbellen bij een [Azure-functie](https://functions.azure.com) codefragmenten code op aanvraag uitvoeren, kunt u eenvoudig maken. 

- **Reële integratie paardenkracht** - eenvoudig starten en uit te breiden naar wens. Logica Apps kunt gemakkelijk de kracht van BizTalk, Microsoft industrie toonaangevende integratie oplossing om in te schakelen integratie professionals om de oplossingen die zij nodig hebben te maken. Meer informatie over het [Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logica App concepten

Hier volgen enkele van de belangrijkste onderdelen waaruit de logica Apps-ervaring. 

- **Werkstroom** - logica Apps biedt een grafische manier om uw bedrijfsprocessen als een reeks stappen of een werkstroom voor het model.
- **Verbindingslijnen beheerde** - uw apps logica moeten toegang hebben tot gegevens en -services. Beheerde verbindingslijnen worden specifiek om te helpen wanneer u verbinding maakt met en werkt met uw gegevens gemaakt. Zie de lijst met verbindingslijnen nu beschikbaar in de [verbindingslijnen beheerde][managedapis].
- **Triggers** - sommige beheerde verbindingslijnen kan ook fungeren als een trigger. Een trigger wordt een nieuw exemplaar van een werkstroom die is gebaseerd op een bepaalde gebeurtenis, zoals de ontvangst van een e-mailbericht of een wijziging in uw account Azure Storage gestart.
-  **Acties** - elke stap nadat de trigger in een werkstroom een actie wordt genoemd. Elke actie wordt meestal koppelt aan een bewerking op uw beheerde verbindingslijn of aangepaste API-apps.
- **Enterprise Integration Pack** - bevat de mogelijkheden van BizTalk voor meer geavanceerde integratie scenario's logica Apps. BizTalk is Microsofts industrie toonaangevende integratieplatform. De verbindingslijnen Enterprise Integration Pack kunnen u eenvoudig opnemen validatie, transformatie en meer in naar uw werkstromen logica-App.

## <a name="getting-started"></a>Aan de slag  

- Als u wilt beginnen met logica Apps, volgt u het [maken van een App logica] [ create] zelfstudie.  
- [Algemene voorbeelden en scenario's weergeven](app-service-logic-examples-and-scenarios.md)
- [U kunt bedrijfsprocessen automatiseren met logica Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Meer informatie over het integreren van uw systemen met logica Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
