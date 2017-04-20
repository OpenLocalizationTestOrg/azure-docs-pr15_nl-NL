<properties
   pageTitle="Status van Azure Resourceoverzicht | Microsoft Azure"
   description="Overzicht van Azure Resource systeemstatus"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Status van Azure Resourceoverzicht

Azure status van de Resource is een service die beschrijft de status van afzonderlijke Azure resources en bevat sneller richtlijnen voor problemen. In een cloud-omgeving waarin het niet mogelijk te rechtstreeks toegang tot servers of infrastructuur elementen, is het doel voor de Resource gezondheid verkleinen van de tijd die klanten besteden aan het oplossen van problemen met name verkleinen van de tijd wordt gebruikt als de hoofdmap van het probleem in de toepassing bepaalt of als dit wordt veroorzaakt door een gebeurtenis in het Azure platform vaststellen.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Wat wordt beschouwd als een Resource en hoe doet status van de resource wordt bepaald of de resource al dan niet correct is? 
Een resource is een gebruikersexemplaar die is gemaakt van een resourcetype is verstrekt door een service, bijvoorbeeld: een virtuele machine, een Web-app of een SQL-database. 

Status van de resource, is afhankelijk van signalen dat door de resource en/of de service om te bepalen of een resource al dan niet correct is. Het is belangrijk dat momenteel Resource systeemstatus alleen accounts voor de status van een specifieke resource Typ en andere elementen die aan de algemene status bijdragen kunnen geen rekening opvalt. Bijvoorbeeld bij het rapporteren van dat de status van een virtuele machine, alleen het berekeningscluster gedeelte van de infrastructuur wordt beschouwd als, dat wil zeggen problemen in het netwerk wordt niet weergegeven in de status van de Resource, tenzij er een servicestoring aangegeven wordt in dat geval deze worden opgehaald door de banner boven aan het blad. Meer informatie over het servicestoring wordt verderop in dit artikel aangeboden. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Status van de Resource op welke manier verschilt van dashboard voor servicestatus:?

De informatie die is verstrekt door de status van de Resource is uitgebreider dan wat wordt geleverd door het dashboard voor servicestatus:. Hoewel SHD communiceert gebeurtenissen die van invloed zijn op de beschikbaarheid van een service in een gebied, Resource-status beschrijft informatie die relevant zijn voor een specifieke bron, bijvoorbeeld gebeurtenissen die van invloed zijn op de beschikbaarheid van een virtuele machine, een web-app of een SQL-database, zal worden blootgesteld. Bijvoorbeeld als een knooppunt onverwacht opnieuw is opgestart, moeten klanten waarvan virtuele machines zijn uitgevoerd op dat knooppunt krijgen van de reden waarom hun VM niet beschikbaar voor een bepaalde periode is.   

## <a name="how-to-access-resource-health"></a>Het openen van Resource systeemstatus
Er zijn voor de services beschikbaar via de status van de Resource, 2 manieren voor toegang tot de status van de Resource.

### <a name="azure-portal"></a>Azure-Portal
Het blad Resource servicestatus in de Azure-Portal vindt u gedetailleerde informatie over de status van de resource, evenals aanbevolen acties die, afhankelijk van de huidige status van de resource variëren. Deze blade biedt de beste ervaring query's naar de status van de Resource, zoals dit bestand is toegang tot andere bronnen in de portal. Zoals eerder de set aanbevolen acties in het blad van de servicestatus Resource zijn afhankelijk van de huidige status:

* Orde resources: aangezien er geen probleem dat van invloed op de status van de resource zijn kan is opgetreden, de acties die zijn gericht op helpt het proces voor probleemoplossing. Bijvoorbeeld, biedt deze directe toegang tot het blad probleemoplossing, waarin biedt hulp bij het oplossen van de meest voorkomende problemen klanten nominale.
* Beschadigde resource: voor problemen die worden veroorzaakt door Azure, het blad acties Microsoft duurt (of heeft die u hebt gemaakt) worden weergegeven voor het herstellen van de resource. Voor problemen die worden veroorzaakt door gebruiker gestarte acties, de blade hebben een lijst met acties klanten kunt uitvoeren zodat het probleem en herstellen van de resource.  

Zodra u hebt aangemeld bij de Portal Azure, zijn er twee manieren voor toegang tot het blad van de servicestatus Resource: 

###<a name="open-the-resource-blade"></a>Open het blad Resource
Open het blad Resource voor een bepaalde bron. Klik op het blad instellingen dat wordt geopend naast het blad voor de Resource, op Resource systeemstatus openen van het blad van de servicestatus Resource. 

![Resource systeemstatus blade](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Help en ondersteuning blade
Open het blad Help en ondersteuning door te klikken op het vraagteken in de rechterbovenhoek en vervolgens Help + ondersteuning te selecteren. 

**De bovenste navigatiebalk**

![Help + ondersteuning](./media/resource-health-overview/HelpAndSupport.png)

Het abonnement blad van Resource servicestatus waarin een met alle bronnen in uw abonnement lijst wordt te klikken op de tegel worden geopend. Naast elke resource, moet u er een pictogram die aangeeft dat de servicestatus is. Klikken op elke resource, wordt het Resource systeemstatus blad geopend.

**Resource-systeemstatus-tegel**

![Resource-systeemstatus-tegel](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Wat betekent de status van mijn Resource status?
Zijn er 4 verschillende systeemstatus statussen die u voor de resource ziet mogelijk.

### <a name="available"></a>Beschikbaar
De service heeft sprake is van problemen niet gevonden in het platform die kan worden die invloed hebben op de beschikbaarheid van de resource. Hiermee wordt aangegeven door een pictogram voor een groen vinkje. 

![Resource is beschikbaar](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niet beschikbaar

In dit geval heeft de service een lopend probleem gevonden in het platform die is die invloed hebben op de beschikbaarheid van deze resource, bijvoorbeeld het knooppunt waarop de VM is uitgevoerd onverwacht opnieuw worden gestart. Hiermee wordt aangegeven door een rode waarschuwingspictogram. Meer informatie over het probleem is opgegeven in het middelste gedeelte van het blad, waaronder: 

1.  Welke acties Microsoft duurt het herstellen van de resource 
2.  Een gedetailleerde tijdlijn van het probleem, inclusief de tijd van de verwachte resolutie
3.  Een lijst met aanbevolen acties voor gebruikers 

![Resource is niet beschikbaar](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Niet beschikbaar – klant gestart
De resource is niet beschikbaar vanwege een aanvraag klant zoals een resource stoppen of opnieuw aanvragen. Hiermee wordt aangegeven door een pictogram voor een blauwe informatieve. 

![Resource is niet beschikbaar vanwege de gebruiker een gestarte actie](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Onbekend
De service heeft niet ontvangen voor informatie over deze informatiebron voor meer dan 5 minuten. Hiermee wordt aangegeven door een pictogram voor een grijze vraagteken. 

Het is belangrijk dat dit is niet een definitieve aanduiding dat er iets fout gegaan aan een resource, is zodat klanten moeten deze aanbevelingen volgt:

* Als de resource wordt uitgevoerd, zoals verwacht, maar de status is ingesteld op Onbekend in de status van de Resource, wordt er geen problemen zijn en u kunt de status van de resource bijwerken naar orde na een paar minuten verwachten.
* Als er problemen met het openen van de resource en de status is ingesteld op Onbekend in de status van de Resource, dit kan moeten wordt vroege aangegeven kan er extra onderzoeken en een actie-items worden uitgevoerd totdat de status wordt bijgewerkt in orde of beschadigd

![Status van de resource is onbekend](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Service die invloed hebben op gebeurtenissen
Als u de resource kan worden beïnvloed door een lopend Service die invloed hebben op gebeurtenis, wordt een banner boven aan het blad van de servicestatus Resource worden weergegeven. Klikken op de banner, wordt het blad gebeurtenissen controleren, die wordt weergegeven als u meer informatie over de storing geopend.

![Status van de resource kan worden beïnvloed door een wil](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Wat moet ik weten over de status van de Resource?

### <a name="signal-latency"></a>Signaal latentie
De signalen die feed servicestatus van de Resource, mogelijk maximaal 15 min vertraagd, waardoor verschillen tussen de huidige status van de resource en de beschikbaarheid van de werkelijke. Het is belangrijk dit in gedachten moet houden terwijl deze onnodige tijd onderzoek mogelijke problemen worden voorkomen. 

### <a name="special-case-for-sql"></a>Speciale hoofdletters/kleine letters voor SQL 
Status van de resource de status van de SQL-database, niet op de SQL server-rapporten. Hoewel deze route gaan een meer natuurlijke systeemstatus-afbeelding biedt, is vereist dat meerdere onderdelen en -services in aanmerking worden genomen om te bepalen de status van de database. Het huidige signaal, is afhankelijk van aanmeldingen bij de database, wat betekent dat voor databases die normale aanmeldingen ontvangt (waaronder onder andere ontvangen van query-aanvragen voor uitvoering) de status status wordt regelmatig weergegeven. Als de database niet voor een periode van 10 minuten of langer is geopend, wordt deze verplaatst naar de onbekende status. Dit betekent niet dat de database is niet beschikbaar, alleen dat geen signaal is gegenereerd omdat geen aanmeldingen zijn uitgevoerd. Verbinding maken met de database en uitvoeren van een query wordt de signalen die nodig zijn om te bepalen en bijwerken van de status van de database verzenden.

## <a name="feedback"></a>Feedback
We hebben altijd een geopende feedback en suggesties! Stuur ons uw [suggesties](https://feedback.azure.com/forums/266794-support-feedback). Bovendien kunt u met ons deelnemen via een [Twitter-](https://twitter.com/azuresupport) of de [MSDN-forums](https://social.msdn.microsoft.com/Forums/azure).
