<properties
    pageTitle="Aanbevolen procedures voor het Azure App-Service"
    description="Informatie over aanbevolen procedures en probleemoplossing voor Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Aanbevolen procedures voor het Azure App-Service

In dit artikel bevat een overzicht van de aanbevolen procedures voor het gebruik van [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Onderbrengen
Wanneer Azure resources voor het opstellen van een oplossing zoals een web-app en een database bevinden zich in verschillende regio's de effecten kunnen onder andere de volgende:

*  Verbeterde latentie in de communicatie tussen resources
*  Monetaire kosten voor uitgaande gegevens worden doorverbonden cross-regio zoals vermeld op de [pagina met Azure prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

Onderbrengen in hetzelfde gebied meest geschikt is voor Azure resources voor het opstellen van een oplossing zoals een web-app en een database of opslag-account voor het opslaan van inhoud of gegevens. Bij het maken van resources, die moet u controleren of zijn ze in de dezelfde Azure regio tenzij u specifieke zakelijke hebt of ontwerpen reden niet wilt laten. U kunt een App Service-app verplaatsen naar hetzelfde gebied, als de database door gebruikmaken van de [App Service klonen functie](app-service-web-app-cloning-portal.md) momenteel beschikbaar is voor apps Premium App-Service plannen.   

## <a name="memoryresources"></a>Wanneer de apps meer geheugen dan verwacht gebruiken
Als er meer geheugen dan verwacht, zoals aangegeven via de cmdlets voor controle wordt gebruikt door een app of service aanbevelingen Houd rekening met de [functie App Service automatisch retoucheren](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Een van de opties voor de functie automatisch retoucheren duurt aangepaste acties op basis van een drempelwaarde geheugen. Acties verdeeld over de verschillende e-mailmeldingen naar onderzoek via vastgelegd aan het plaatse risicobeperking door het werkproces opnieuw. Automatisch retoucheren kan worden geconfigureerd via web.config en via een beschrijvende gebruikersinterface, zoals beschreven op in dit blogbericht voor de [App-ondersteuning Site extensie](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Wanneer de apps meer CPU dan verwacht gebruiken
CPU pieken wanneer ziet u dat een app verbruikt meer CPU dan verwacht of ervaringen worden herhaald, zoals aangegeven via de cmdlets voor controle of service aanbevelingen overwegen schaalbaarheid van of schalen het App-abonnement. Als uw toepassing statefull is, is schaalbaarheid van de enige optie terwijl als de toepassing is stateless, schaal out u meer flexibiliteit en hoger schaal potentieel krijgt. 

U kunt deze video bekijken voor meer informatie over "statefull" vs "stateless" toepassingen: [plannen van een Scalable End-to-End met meerdere niveaus-toepassing op Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Lees voor meer informatie over App schalen en autoscaling serviceopties: [schaal een Web-App in Azure App-Service](web-sites-scale.md).  

## <a name="socketresources"></a>Wanneer de socket resources voorraad
Een algemene reden voor allemaal uitgaande TCP-verbindingen is het gebruik van clientbibliotheken die niet zijn geïmplementeerd TCP-verbindingen opnieuw te gebruiken of in het geval van een hoger niveau protocol zoals HTTP - permanente niet wordt gebruikt. Raadpleeg de documentatie voor elk van de bibliotheken waarnaar wordt verwezen door de apps in uw App-Service plannen om ervoor te zorgen ze zijn geconfigureerd of worden ontvangen in uw code voor efficiënte hergebruik van uitgaande verbindingen. Ook Volg de instructies voor de bibliotheek-documentatie van BEGINLETTERS maken en release of opruimen om te voorkomen lekkende verbindingen. Terwijl deze clientbibliotheken onderzoeken in voortgang van invloed zijn mogelijk worden beperkt door schalen naar meerdere exemplaren.  

## <a name="appbackup"></a>Wanneer uw app back-up maken wordt gestart verbroken
De twee meest voorkomende redenen waarom app back-up mislukt zijn: ongeldige Opslaginstellingen en ongeldige databaseconfiguratie. Deze fouten optreden meestal wanneer er wijzigingen in de opslagruimte of database resources, of wijzigingen in voor het openen van deze resources (bijvoorbeeld referenties bijgewerkt voor de database die is geselecteerd in de instellingen van de back-ups). Back-ups meestal worden uitgevoerd op een planning en toegang tot opslag (voor het uitvoeren van de back-upbestanden) en databases vereisen (voor het kopiëren en lezen inhoud in de back-up moet worden opgenomen). Het resultaat van de toegankelijkheid van een van deze resources zou consistent back-up is mislukt. 

Wanneer de back-fouten opgetreden, Controleer de meest recente resultaten als u wilt weten welke soort storing komt. In het geval van opslag access mislukt, Controleer en de opslaginstellingen gebruikt in de back-configuratie bijwerken. Als de database toegang mislukt, Controleer en bijwerken van uw verbindingen tekenreeksen als onderdeel van de app-instellingen. gaat u verder met het bijwerken van uw back-configuratie als u wilt opnemen correct de vereiste databases. Zie de documentatie [Back-up van een WebApp in Azure App-Service](web-sites-backup.md) voor meer informatie over het app-back-up.

## <a name="nodejs"></a>Wanneer er nieuwe Node.js apps zijn geïmplementeerd in Azure App-Service
Azure App Service standaardconfiguratie voor Node.js apps is bedoeld voor het beste aansluiten op de behoeften van de meest voorkomende apps. Als de configuratie voor uw app Node.js zou komen gepersonaliseerde optimaliseren om de prestaties verbeteren of Resourcegebruik voor CPU/geheugen/netwerk resources optimaliseren, kunt u onze aanbevolen procedures en stappen voor probleemoplossing kan bekijken. In dit artikel documentatie worden de instellingen iisnode u wellicht moet configureren voor uw app Node.js, worden de verschillende scenario's beschreven of problemen die uw app kan worden tegenoverliggende en ziet u hoe u deze problemen op te lossen: [Aanbevolen procedures en gids voor probleemoplossing voor knooppunt toepassingen op Azure App-Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


