<properties
    pageTitle="Overzicht van de logica Apps verbindingslijnen | Microsoft Azure"
    description="Overzicht van verbindingslijnen die kunnen worden gebruikt in een app logica"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Verbindingslijnen gebruiken in een logica-app

Verbindingslijnen snel toegang bieden tot gebeurtenissen, gegevens en acties verkoopeenheden services, producten protocollen en platforms.  De volledige lijst met verbindingslijnen die ondersteuning biedt voor logica Apps kan [worden gevonden hier](apis-list.md).  Verbindingslijnen kunnen worden gebruikt als een trigger of een actie in een app logica en kan vragen om een geconfigureerde *verbinding* gebruiken (bijvoorbeeld: een Twitter-account toegang krijgen tot of namens u posten machtigen).

## <a name="basics"></a>Basisbeginselen

Verbindingslijnen zijn gehoste services die u toegang als onderdeel van een app logica tot hebt kunt integreren met andere services zoals Dynamics, Azure, Salesforce, [en meer](apis-list.md).  Ze zijn geïmplementeerd en beheerd door Microsoft, zodat u kunt uw werkstromen integratie met schaal, doorvoer en beveiliging die u hebt gemaakt om ervoor te zorgen maken.  U kunt een verbindingslijn toevoegen aan een app logica door te zoeken en selecteren van een verbindingslijn actie of trigger onder **Microsoft weergeven beheerde API's**.

![Menu van de actie voor het selecteren van trigger][1]

Elke verbindingslijn actie of trigger heeft de set met eigenschappen te configureren.  U kunt klikt u op de knoppen info voor meer informatie over de actie of verwijzen naar de documentatie [voor meer informatie](apis-list.md).

Als u integreren met een service of API dat niet wordt vermeld wilt, maar een verbindingslijn, u kunt ook logica apps via een [aangepaste verbindingslijn](../app-service-logic/app-service-logic-create-api-app.md) uitbreiden of alleen rechtstreeks naar de service via een protocol zoals HTTP bellen.

## <a name="triggers"></a>Triggers

Sommige verbindingslijnen hebben trigger, wat betekent dat een gebeurtenis van deze verbindingslijn wordt een app logica gestart en wordt doorgegeven in alle gegevens als onderdeel van de trigger.  Een trigger is altijd de eerste stap in een app logica.  Populaire triggers zijn bewerkingen, zoals:
 
 * Terugkeerpatroon - per uur uitvoeren
 * Wanneer een HTTP-aanvraag wordt ontvangen
 * Wanneer een item wordt toegevoegd aan een wachtrij
 * Wanneer een e-mailbericht wordt ontvangen
 
Sommige triggers het chatgesprek dat een gebeurtenis vindt plaats via een melding naar de logica-app wordt uitgevoerd en andere moet u een terugkeerpatroon die is geconfigureerd op hoe vaak de logica-app de service voor een gebeurtenis (maximaal elke 15 seconden) moeten worden gecontroleerd.  

Wanneer een gebeurtenis wordt ontvangen, de voeren logica-app wordt uitgevoerd en de acties in de werkstroom wordt gestart.  Ook is mogelijk voor toegang tot gegevens vanaf de trigger overal in de werkstroom (bijvoorbeeld de trigger 'op een nieuwe tweet' wordt binnenkomen de tweet de uitvoeren).

## <a name="actions"></a>Acties

De meeste verbindingslijnen hebben een of meer acties die kunnen worden uitgevoerd als onderdeel van de werkstroom.  Acties zijn de stappen die hebben plaatsgevonden nadat het uitvoeren van een trigger is gestart.  Als u wilt toevoegen van een actie Klik op de knop **Nieuwe stap** en zoek naar de verbindingslijn u wilt gebruiken.  Nadat geselecteerd (en als u na het configureren van alle [verbindingen](#connections) die nodig zijn) ziet u de actie-kaart die u kunt configureren.  U kunt gegevens uit de vorige stappen selecteren door te klikken op een van de tokens voor uitvoer, of in een andere configuratie Voer zo nodig.

![De actie van een verbindingslijn configureren][2]

## <a name="connections"></a>Verbindingen

De meeste verbindingslijnen moeten u een *verbinding* configureren voordat u de verbindingslijn kunt gebruiken.  Een *verbinding* is een login of verbinding configuratie die nodig zijn voor toegang tot de verbindingslijn.  Betekent dat zich aanmeldt bij de service (zoals Office 365, Salesforce of GitHub) waar uw toegangstoken kan worden gecodeerd en veilig worden opgeslagen in een Azure geheime archief voor verbindingslijnen die OAuth gebruiken, een verbinding maken.  Andere verbindingslijnen (zoals FTP en SQL) vereisen een verbinding met de configuratie zoals serveradres, gebruikersnaam en wachtwoord.  Deze configuratie verbindingsgegevens zijn ook gecodeerd en veilig opgeslagen.  Verbindingen kunnen toegang tot de service voor zo lang maken als de service kunt.  We kunt blijven het toegangstoken voor onbepaalde tijd vernieuwen voor Azure Active Directory OAuth-verbindingen (zoals Office 365 en Dynamics).  Andere services kunnen limieten op hoe lang we een token kunt gebruiken zonder deze worden vernieuwd plaatsen.  Bepaalde acties bijgehouden, zoals het wijzigen van een wachtwoord, worden in het algemeen alle access-tokens ongeldig.  

Verbindingen kunnen worden bekeken en beheerd in Azure wordt aangegeven door te klikken op **Bladeren** en **API verbindingen**te selecteren.  Uit de bron API verbindingen kunt u weergeven, bewerken, bijwerken of opnieuw Autoriseer alle verbindingen die u hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Informatie over algemene gebruik en voorbeelden van logica-apps](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Aan de slag met enterprise-Integratietriggers en acties](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png