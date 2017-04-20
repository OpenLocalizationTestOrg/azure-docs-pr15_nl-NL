<properties 
    pageTitle="Een app integreren met een Azure Virtual Network" 
    description="Ziet u hoe u een app in Azure App Service verbinden met een nieuw of bestaand Azure virtual network" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uw app integreren met een Azure Virtual Network #

Dit document beschrijving van de integratiefunctie van Azure App Service virtueel netwerk en ziet u hoe u deze instelt met apps in [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714).  Als u niet bekend bent met Azure virtuele netwerken (VNETs), is dit een mogelijkheid waarmee u veel van uw Azure resources in een niet-internet routeable netwerk dat u toegang tot plaatsen.  Deze netwerken kunnen vervolgens worden verbonden met uw aan premises-netwerken met een verscheidenheid aan VPN technologieën.  Voor meer informatie over Azure virtuele netwerken beginnen met de gegevens hier: [Azure virtuele netwerk overzicht][VNETOverview].  

De App Azure Service heeft twee formulieren.  

1. De meerdere tenant-mailsystemen die ondersteuning bieden voor de volledige reeks prijzen van abonnementen
1. De functie van de App Service omgeving (ASE) premium die in uw VNET.  

In dit document gaat tot en met VNET integratie en niet de omgeving van App-Service.  Als u wilt meer informatie over de functie ASE en start u de gegevens hier: [App-omgeving inleiding][ASEintro].

Integratie met VNET geeft uw web app-toegang tot bronnen in het virtuele netwerk, maar geen privé toegang verleent tot uw web-app van het virtuele netwerk.  Toegang tot een persoonlijke site is alleen beschikbaar met een ASE geconfigureerd met een interne laden verdeling (ILB).  Voor meer informatie over het gebruik van een ASE ILB begint met het artikel hier: [maken en gebruiken van een ASE ILB][ILBASE]. 

Toegang via uw web-app aan een database of een webservices worden uitgevoerd op een virtuele machine in uw Azure virtuele netwerk is het inschakelen van een gebruikelijk waar u VNET integratie wilt gebruiken.  Met VNET integratie hoeft u niet te laten zien van een openbare eindpunt voor toepassingen op uw VM maar kan de particuliere geen internet-geschikt adressen in plaats hiervan te gebruiken.  

De functie Integratie met VNET:

- vereist een standaard- of Premium prijzen van abonnement 
- werken met Classic(V1) of Resource Manager(V2) VNET 
- ondersteuning biedt voor alle TCP- en UDP
- identiteitsprogramma werkt met het Web, Mobile en API-apps
- kan een app verbinding maken met slechts 1 VNET per keer
- kunnen maximaal 5 VNETs om te worden geïntegreerd met in een App-Service plannen 
- kunt u de dezelfde VNET moet worden gebruikt door meerdere apps in een App-Service plannen
- een SLA 99,9% vanwege een afhankelijkheid van de Gateway VNET ondersteunt

Er zijn enkele dingen die VNET integratie biedt geen ondersteuning voor het waaronder:

- een station koppelen
- AD-integratie 
- NetBios
- toegang tot een persoonlijke site

### <a name="getting-started"></a>Aan de slag ###
Hier zijn enkele dingen in gedachten moet houden voordat u verbinding maakt van uw web-app met een virtueel netwerk:

- Integratie van VNET werkt alleen met apps in een **standaard** - of **Premium** prijzen van abonnement.  Als u de functie inschakelen en klik vervolgens uw App serviceplan een niet-ondersteunde prijzen plan schalen verliest uw apps hun verbindingen naar de VNETs persoon gebruikt.  
- Als uw doel virtuele netwerk al bestaat, moet dit punt-naar-site VPN met een dynamische routeren gateway ingeschakeld voordat deze kan niet worden verbonden met een app.  U kunt niet punt-naar-site Virtual Private Network (VPN) inschakelen als de gateway is geconfigureerd met statische routering.
- De VNET moet zich in hetzelfde abonnement als uw App Service Plan(ASP).  
- De apps die zijn geïntegreerd met een VNET wordt gebruikt in de DNS-records die voor deze VNET is opgegeven.
- Al dan niet standaard stuurt de geïntegreerde apps alleen verkeer naar uw VNET op basis van de routes die zijn gedefinieerd in uw VNET.  


## <a name="enabling-vnet-integration"></a>VNET-integratie inschakelen ##

In dit document is gericht hoofdzakelijk over het gebruik van de Azure-Portal voor VNET integratie.  Als u wilt inschakelen VNET integratie met uw app via PowerShell, volgt u de aanwijzingen hier: [verbinding maken met uw app bij het netwerk van uw virtuele via PowerShell][IntPowershell].

Hebt u de optie uw app verbinden met een nieuw of bestaand virtueel netwerk.  Als u een nieuw netwerk als onderdeel van uw integratie Klik naast het maken van de VNET maken, een dynamische routeren gateway worden vooraf geconfigureerde voor u en wijs Site VPN worden ingeschakeld.  

>[AZURE.NOTE] Configuratie van de integratie van een nieuwe virtuele netwerken kan enkele minuten duren.  

Inschakelen VNET integratie openen uw app-instellingen en selecteer vervolgens netwerken.  De gebruikersinterface dat wordt geopend biedt drie netwerken keuzemogelijkheden.  Deze handleiding wordt alleen naar gaan VNET integratie echter hybride verbindingen en App serviceomgevingen worden verderop in dit document besproken.  

Als uw app niet in de juiste plan prijzen is kunt de gebruikersinterface moment u de schaal van uw abonnement op een hoger prijzen abonnement van uw keuze aanpassen.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>VNET-integratie met een bestaande VNET inschakelen###
De gebruikersinterface van de integratie VNET kunt u kiezen uit een lijst met uw VNETs.  De klassieke VNETs wordt aangegeven dat ze bijvoorbeeld met het woord 'Klassieke' haakjes naast de naam van de VNET zijn.  De lijst is gesorteerd dat de VNETs resourcemanager eerst worden vermeld.  In de onderstaande afbeelding ziet u dat er slechts één VNET kan worden geselecteerd.  Er zijn verschillende redenen dat een VNET lichter gekleurd inclusief wordt worden weergegeven:

- de VNET zich in een ander abonnement dat uw account toegang tot heeft
- de VNET heeft geen punt naar Site ingeschakeld
- de VNET heeft geen een dynamische routeren gateway


![][2]

Als u wilt inschakelen integratie Klik op de VNET die u wilt integreren met.  Nadat u de VNET, uw app automatisch door te gaan om de wijzigingen zijn doorgevoerd.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Wijs Site in een klassieke VNET inschakelen #####
Als uw VNET geen een gateway heeft noch punt naar Site heeft hebt u instellen dat van de eerste.  Als u wilt deze stap herhalen voor een klassiek VNET, gaat u naar de [Portal van Azure] [ AzurePortal] en de lijst met virtuele Networks(classic) weer.  Vanaf hier Klik op het netwerk dat u wilt integreren met en klikt u op het grote vak onder Essentials VPN-verbindingen genoemd.  Hier kunt kunt u uw punt om site-VPN en zelfs hebben het maken van een gateway maken.  Nadat u de komma naar site gateway maken ervaring dient ongeveer 30 minuten doorlopen voordat de werkmap gereed is.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Wijs Site in een VNET resourcemanager inschakelen #####

Een VNET resourcemanager configureren met een gateway en verwijzen naar de Site die u nodig hebt om PowerShell te gebruiken zoals beschreven hier [een punt-naar-Site-verbinding met een virtueel netwerk via PowerShell configureren][V2VNETP2S].  De gebruikersinterface voor het uitvoeren van deze functie is nog niet beschikbaar. 

### <a name="creating-a-pre-configured-vnet"></a>Een vooraf geconfigureerde VNET maken ###
Als u een nieuwe VNET die is geconfigureerd met een gateway en punt-naar-Site maken wilt, klikt u vervolgens heeft de App-Service netwerkproblemen UI de mogelijkheid dat te doen maar alleen voor een resourcemanager VNET.  Als u wilt een klassieke VNET maken met een gateway en punt-naar-Site moet u dit handmatig doen via de gebruikersinterface van netwerken. 

Als u wilt maken van een Resource Manager VNET via de gebruikersinterface van de integratie VNET, gewoon Selecteer **Nieuw virtueel netwerk maken** en geef de:

- Virtuele netwerknaam
- Virtual Network Adresblok
- De subnetnaam van de
- Subnet Adresblok
- Gateway-Adresblok
- Adresblok punt-naar-Site

Als u wilt dat deze VNET verbinding maken met een van uw andere netwerk vervolgens Vermijd het IP-adresruimte in die met deze netwerken overlapt te kiezen.  

>[AZURE.NOTE] Resourcemanager VNET maken met een gateway duurt ongeveer 30 minuten en momenteel wordt niet integreren in de VNET uw app.  Nadat uw VNET is gemaakt met de gateway moet u keert u terug naar uw app VNET integratie UI en selecteert u uw nieuwe VNET.

![][3]

Azure VNETs worden normaal in adressen privé netwerk gemaakt.  Functie wordt een verkeer naar uw VNET voor deze IP-adresbereiken routeert al dan niet standaard de VNET-integratie.  De privé IP-adresbereiken zijn:

- 10.0.0.0/8 - het resultaat is hetzelfde als 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 - het resultaat is hetzelfde als 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 - het resultaat is hetzelfde als 192.168.0.0 - 192.168.255.255
 
De adresruimte VNET moet worden opgegeven in de CIDR notatie weer.  Als u niet bekend met CIDR notatie bent, is een methode voor het adresblokken met een IP-adres en een geheel getal dat staat voor het netwerkmasker opgeven. Een beknopt overzicht, kunt u dat 10.1.0.0/24 256 adressen en 10.1.0.0/25 zou 128 adressen.  Een IPv4-adres met een /32 zou slechts 1 adres.  

Als u de DNS-server-gegevens Stel hier vervolgens die wordt ingesteld voor uw VNET.  U kunt deze informatie van de gebruikerservaring VNET bewerken nadat de VNET is gemaakt.

Wanneer u een klassieke VNET gebruik van de gebruikersinterface van de integratie VNET maakt, wordt er een VNET gemaakt in dezelfde resourcegroep als uw app. 

## <a name="how-the-system-works"></a>De werking van het systeem ##
Deze functie automatisch achter de samengesteld boven aan de punt-naar-Site VPN-technologie uw app verbinden met uw VNET.   Apps in Azure App Service hebben een meerdere tenant systeemarchitectuur waarin inrichting van een app rechtstreeks in een VNET uitsluit als met virtuele machines is voltooid.  Door te maken op een punt-naar-site technologie beperken we netwerktoegang om alleen de virtuele machine hostingprovider van de app te.  Toegang tot het netwerk is verder beperkt op die app-hosts zodat uw apps alleen toegang hebt tot de netwerken die u ze om toegang te configureren.  

![][4]
 
Als u een DNS-server nog niet hebt geconfigureerd met uw netwerk virtuele moet u IP-adressen te gebruiken.  Vergeet niet dat de belangrijkste voordelen van deze functie is dat u kunt de particuliere adressen binnen het netwerk van uw persoonlijke gebruiken bij het gebruik van IP-adressen.  Als u uw app gebruik van openbare IP-voor een van uw VMs adressen u niet gebruikt de functie VNET integratie en communiceren via internet instellen.


##<a name="managing-the-vnet-integrations"></a>De integraties VNET beheren##

De mogelijkheid om te maken en verbreken naar een VNET is op het niveau van een app.  Bewerkingen die betrekking hebben op de VNET-integratie tussen meerdere apps zijn op een niveau ASP.  U kunt de details op uw VNET krijgen van de gebruikersinterface die wordt weergegeven op het niveau van de app.  De meeste dezelfde informatie wordt ook weergegeven op het niveau van ASP.  

![][5]

Via de pagina Status van netwerk-functie kunt u zien als uw app is verbonden met uw VNET.  Als uw VNET gateway is niet beschikbaar voor wilt welke reden dan ook vervolgens dit weergeven als niet-verbonden.  

De informatie die u hebt nu beschikbaar zijn voor u in de gebruikersinterface van de integratie niveau VNET is hetzelfde als de detailgegevens die u van de ASP krijgen-app.  Hier vindt u deze items:

- De naam van de VNET - deze koppeling wordt geopend de gebruikersinterface van het netwerk
- Locatie - dit geeft de locatie van uw VNET.  Is het mogelijk te integreren met een VNET in een andere locatie.
- Certificaatstatus - zijn gebruikt voor het beveiligen van de VPN-verbinding tussen de app en de VNET certificaten.  Dit weerspiegelt een toets om ervoor te zorgen dat ze zijn gesynchroniseerd.
- Status van gateway - uw gateways moet omlaag voor welke reden dan ook vervolgens uw app geen toegang tot bronnen in de VNET.  
- VNET-adresruimte - dit is de ruimte IP-adres voor uw VNET.  
- Wijs Site-adresruimte - dit is de site IP-adres ruimte voor uw VNET.  Uw app wordt communicatie als die afkomstig zijn uit een van de IP-adressen in deze adresruimte weergegeven.  
- Site op site-adresruimte - kunt u Site naar Site VPN's aan uw VNET verbinding te maken met uw aan premises resources of andere VNETs.  U mocht dat hebt geconfigureerd en vervolgens de IP-bereiken die worden gedefinieerd met behulp van de VPN-verbinding hier leert hebben.
- DNS-Servers - hebt u DNS-Servers geconfigureerd met uw VNET en vervolgens worden deze hier weergegeven.
- IP-adressen doorgestuurd naar de VNET - er zijn een lijst met IP-adressen dat uw VNET routering heeft voor gedefinieerd.  De adressen die wordt hier weergegeven.  

De bewerkingen die u in de app-weergave van uw VNET-integratie uitvoeren kunt is uw app met de VNET deze momenteel is verbonden met verbreken.  Als u wilt doen op dit verbinding verbreken gewoon aan de bovenkant.  Deze actie in ongewijzigd uw VNET blijven.  De VNET en de configuratie, met inbegrip van de gateways blijft ongewijzigd.  Als u vervolgens wilt verwijderen van uw VNET moet u eerst de informatiebronnen in met inbegrip van de gateways verwijderen.  

De App-Service plannen weergave heeft een aantal extra bewerkingen.  Het is ook vanuit anders dan de app.  Te bereiken openen de gebruikersinterface ASP netwerkproblemen gewoon uw ASP UI en schuif omlaag.  Er is een UI-element functie netwerkstatus genoemd.  Sommige secundaire details rond integratie met de VNET geeft.  Klikken op deze gebruikersinterface, wordt de gebruikersinterface van netwerk functie Status geopend.  Als u op "Klik hier klikt voor het beheren van" wordt u UI waarin de integraties VNET in deze ASP geopend.

![][6]

De locatie van de ASP is het handig om te onthouden wanneer de locaties van de VNETs u met integreert kijken.  Wanneer de VNET bevindt zich op een andere locatie ziet u veel meer waarschijnlijk latentieproblemen.  

De VNETs geïntegreerd met is een herinnering voor hoeveel VNETs uw apps zijn geïntegreerd met in deze ASP en hoeveel u kunt hebben.  

Als u wilt zien extra details op elke VNET, klik op de VNET waarin u geïnteresseerd bent.  Naast de details die zijn hierboven ziet u ook een lijst van de apps in deze ASP dat deze VNET gebruikt.  

Er zijn twee primaire acties met betrekking tot acties.  De eerste is de mogelijkheid om toe te voegen routes die leid verkeer uw app bij uw VNET verlaten.  De tweede actie is de mogelijkheid om certificaten en netwerkgegevens te synchroniseren.

![][7]

**Mailroutering** Zoals hierboven is de routes die zijn gedefinieerd in uw VNET zijn wat wordt gebruikt voor het routeren van verkeer naar uw VNET uit uw app.  Er zijn enkele toepassingen Hoewel waar klanten wilt verzenden extra uitgaand verkeer uit een app in de VNET en deze deze mogelijkheid is opgegeven.  Wat gebeurt er met de verkeer nadat die is tot aan hoe de klant hun VNET configureert.  

**Certificaten** De Status van het certificaat weerspiegelt een selectievakje wordt uitgevoerd door de App-Service voor het valideren van dat de certificaten die we voor de VPN-verbinding nog steeds goede zijn.  Wanneer VNET-integratie is ingeschakeld, klikt u vervolgens is als dit de eerste integratie aan die VNET van alle apps in deze ASP, er een vereiste uitwisseling van certificaten om ervoor te zorgen de beveiliging van de verbinding.  Samen met de certificaten krijgen we de DNS-configuratie, routes en andere soortgelijke zaken aan bod die beschrijven van het netwerk.
Als deze certificaten of netwerkgegevens wordt gewijzigd moet u Klik op "Synchronisatie netwerk".  **Opmerking**: wanneer u klikt op "Synchronisatie netwerk" Klik u een kort storing in connectiviteit tussen uw app en uw VNET veroorzaakt.  Terwijl uw app wordt niet opnieuw worden gestart de kwaliteitsverlies connectivity kan zorgen dat uw site niet goed.  

##<a name="accessing-on-premise-resources"></a>Toegang tot lokale bronnen##

Een van de voordelen van de functie VNET integratie is als uw VNET is verbonden met uw aan premises netwerk met een VPN-verbinding voor de Site naar Site en vervolgens uw apps toegang tot uw aan premises resources uit uw app hebben kunnen.  Dit werkt wel mogelijk moet u uw aan premises VPN gateway bijwerken met de routes voor uw punt met Site IP-bereik.  Wanneer de Site naar Site-VPN is stelt u eerst moeten de scripts die worden gebruikt voor het configureren van deze definiëren routes, inclusief uw wijs VPN van de Site.  Als u de komma toevoegt aan de Site VPN na het maken van de VPN van uw Site naar Site en vervolgens moet u de routes handmatig bijwerken.  Meer informatie over hoe u dat doen varieert per gateway en niet hier worden beschreven.  

>[AZURE.NOTE] Terwijl de functie Integratie met VNET met een VPN-verbinding van het Site-Site werkt voor toegang tot lokale bronnen het momenteel niet geschikt is voor een VPN ExpressRoute hetzelfde te doen.  Dit geldt bij integratie met een klassieke of resourcemanager VNET.  Als u moet via een VPN-ExpressRoute toegang tot bronnen kunt u een ASE die kan worden uitgevoerd in uw VNET gebruiken. 

##<a name="pricing-details"></a>Prijsgegevens##
Er zijn een paar nuances bevatten waarmee u houden moet rekening bij gebruik van de functie VNET integratie prijzen.  Er zijn 3 gerelateerde kosten voor het gebruik van deze functie:

- ASP prijzen van laag vereisten
- Gegevens overbrengen kosten
- Kosten van de VPN Gateway.

Voor uw apps kunnen deze functie te gebruiken, moeten ze zich in een standaard- of Premium App-Service plannen.  Deze kosten hier kunt u meer details bekijken: [Prijzen van App-Service][ASPricing]. 

Vanwege de manier waarop punt naar Site VPN's worden verwerkt, u kunt tevens een boete voor uitgaande gegevens via uw verbinding VNET integratie zelfs als de VNET in de dezelfde Datacenter.  Om te zien wat de kosten zijn Kijk hier: [Overbrengen prijzen detailgegevens][DataPricing].  

Het laatste item is de kosten van de gateways VNET.  Als u niet nodig gateways voor iets anders zoals Site naar Site VPN's hebt vervolgens betaalt u voor gateways ter ondersteuning van de functie VNET integratie.  Er zijn details op deze kosten hier: [VPN Gateway prijzen][VNETPricing].  

##<a name="troubleshooting"></a>Problemen oplossen##

Wanneer de functie is eenvoudig om in te stellen dat betekent niet dat uw ervaring is gratis probleem.  U zijn problemen met het openen van uw gewenste eindpunt er enkele hulpprogramma's die u gebruiken kunt om te controleren van de app-console.  Er zijn twee console ervaringen die u kunt gebruiken.  Een van de Kudu-console is en de andere is de console die u in de Portal Azure bereiken kunt.  U kunt de Kudu-console openen vanaf uw app-Ga naar Extra > Kudu.  Het resultaat is hetzelfde als het gaat u naar [sitenaam]. scm.azurewebsites.net.  Wanneer dat wordt geopend gewoon gaat u naar het tabblad foutopsporing-console.  Als u aan de Azure portal gehoste console en vervolgens vanuit uw app-Ga naar Extra > Console.  


####<a name="tools"></a>Hulpmiddelen voor####

De hulpmiddelen voor ping, nslookup en tracert werken niet via de console vanwege beveiligingsbeperkingen.  Als u wilt doorvoeren de void er zijn twee afzonderlijke hulpprogramma's toegevoegd.  We hebben toegevoegd een hulpmiddel met de naam nameresolver.exe om te testen van DNS-functionaliteit.  De syntaxis van de luidt als volgt:

    nameresolver.exe hostname [optional: DNS Server]

U kunt nameresolver gebruiken om te controleren van de hostnamen die afhankelijk van uw app.  Deze manier kunt u als u iets verkeerd geconfigureerd met uw DNS- of misschien geen toegang tot uw DNS-server testen.

De volgende hulpmiddel kunt u test voor TCP-connectiviteit met een combinatie van host en poort.  Dit programma heet tcpping.exe en de syntaxis van de is:

    tcpping.exe hostname [optional: port]

Dit hulpmiddel kunt nagaan u als u een specifieke host en poort kunt bereiken, maar niet langer uit dezelfde taak dat u met het ICMP hulpprogramma ping gebaseerd.  Het hulpprogramma ICMP ping kunt u nagaan of uw host omhoog is.  Met tcpping erachter u komt als u toegang hebt tot een specifieke poort op een host geretourneerd.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Toegang tot VNET foutopsporing gehost resources####

Er zijn een aantal zaken aan bod die kan voorkomen dat uw app kunnen bereiken van een bepaalde host en poort.  De meeste gevallen moet dit is een van de drie dingen:

- **Er is een firewall de manier waarop**  Als er een firewall in de manier waarop wordt u de TCP-time-out raken.  Die is 21 seconden in dit geval.  Gebruik het hulpmiddel tcpping om te controleren.  TCP-outs kunnen worden vanwege veel zaken voorbij firewalls, maar er starten.  
- **DNS is niet toegankelijk**  De DNS-time-out is 3 seconden per DNS-server.  Als er 2 DNS-servers die is 6 seconden.  Gebruik nameresolver om te zien als DNS werkt.  Onthoud dat u niet nslookup gebruiken zoals die geen van de DNS-records uw VNET is geconfigureerd gebruikmaakt met.
- **Ongeldige P2S IP-bereik** De komma naar site IP-bereik moet in de RFC 1918 privé IP-bereiken (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) als het bereik IP-adressen buiten die gebruikt, zal dingen werken niet.  

Als deze items niet uw probleem beantwoorden, kijkt u als volgt te eerste voor de eenvoudige dingen zoals:  

- Wordt de Gateway weergegeven die u in de Portal?
- Kunnen certificaten weergeven als dat wordt gesynchroniseerd?
- Iedereen de netwerkconfiguratie wijzigen zonder een "synchronisatie-netwerk' in de desbetreffende ASP's? 

Als de gateway omlaag is brengt u deze back-up.  Als uw certificaten gesynchroniseerd zijn vervolgens gaat u naar de ASP-weergave van de integratie met de VNET en "Synchronisatie netwerk" raken.  Als u vermoedt dat er een wijziging in de configuratie van uw VNET is en deze synchroniseren is niet zou doen met uw ASP vervolgens gaat u naar de ASP-weergave van de integratie met de VNET en klik op "Synchronisatie netwerk" net zoals een herinnering, hierdoor wordt een kort storing met de verbinding VNET en uw apps.  

Als die al is in orde moet u de details dan een stapje verder:

- Zijn er andere apps VNET integratie met resources in de dezelfde VNET bereiken? 
- Kunt u gaat u naar de app-console en tcpping gebruiken voor communicatie met andere bronnen in uw VNET?  

Als een van de bovenstaande voorwaarden voldaan wordt vervolgens uw VNET-integratie is in orde en het probleem is ergens anders.  Dit is waar deze wordt onmiddellijk meer van een uitdaging zijn, omdat er geen eenvoudige manier om te zien waarom u een host: poort geen bereiken.  Enkele van de oorzaken zijn:

- u hebt een firewall van op uw host toegang tot de toepassing-poort van uw punt naar site IP-bereik voorkomen.  Subnetten vaak kruist vereist toegang van het publiek.
- uw target host is niet beschikbaar
- uw toepassing is niet beschikbaar
- de verkeerde IP-of hostnaam
- uw toepassing luistert op een andere poort dan naar wens.  U kunt dit controleren door te gaan naar die host en "netstat - aon" vanaf de prompt cmd.  Hier ziet u welke ID, luistert op wat poort proces.  
- uw netwerk beveiligingsgroepen zijn geconfigureerd zodanig dat toegang tot uw toepassinghost en poort van uw punt naar site IP-bereik

Onthoud dat u welke IP niet weet in uw punt naar Site IP-bereik dat uw app gebruiken wilt, zodat u nodig hebt om toegang te krijgen van het volledige bereik.  

Extra stappen zijn:

- Meld u aan bij een andere VM in uw VNET en probeert te krijgen tot uw resource-host: poort daarvandaan.  Er zijn enkele TCP ping hulpprogramma's die u voor deze doel gebruiken kunt of zelfs telnet gebruiken kunt als nodig zijn.  Het doel hier is alleen om te bepalen of connectivity er van deze andere VM. 
- opstarten een toepassing op een andere VM en test toegang tot die host en poort uit vanaf de beheerconsole van uw app  
####<a name="on-premise-resources"></a>Lokale bronnen####
Als uw resources on-premises kan niet worden bereikt en vervolgens het eerste wat u moet controleren is als u een resource in uw VNET kan bereiken.  Als dat werkt zijn de volgende stappen zijn heel eenvoudig.  Vanuit een VM in uw VNET die u wilt proberen te bereiken van de aan premises-toepassing.  U kunt telnet of een hulpprogramma met TCP ping gebruiken.  Als uw VM kan geen bereiken van uw aan premises resource en zorg er eerst werkt uw Site naar Site VPN-verbinding.  Als u werkt dit moet u de dezelfde zaken aan bod hierboven alsmede het aan premises gatewayconfiguratie en status controleren.  

Nu zijn als uw VNET gehost VM uw aan premises-systeem kunt bereiken, maar de app niet en vervolgens de reden waarschijnlijk een van de volgende opties is:
- uw routes zijn niet geconfigureerd met uw wijst naar de site IP-bereiken in uw aan premises gateway
- uw netwerk beveiligingsgroepen worden geblokkeerd door de toegang voor uw punt naar Site IP-bereik
- uw aan premises firewalls worden geblokkeerd door verkeer vanaf uw punt naar Site IP-bereik
- u hebt een gebruiker gedefinieerd Route(UDR) in uw VNET waardoor uw punt naar verkeer van de Site op basis van uw aan premises netwerk bereiken

## <a name="hybrid-connections-and-app-service-environments"></a>Hybride verbindingen en App Service omgevingen##
Er zijn 3 functies die toegang tot VNET gehost bronnen.  Ze zijn:

- VNET-integratie
- Hybride verbindingen
- App-Service omgevingen

Hybride verbindingen moet u bij het installeren van een relay-agent de naam van de hybride verbinding Manager(HCM) in uw netwerk.  De HCM moet kunnen verbinding maken met Azure en ook in uw toepassing.  Deze oplossing is met name handig van een extern netwerk zoals uw aan premises netwerk of zelfs een ander cloud gehost netwerk omdat deze niet voor een toegankelijke internet-eindpunt vereist is.  De HCM alleen op Windows wordt uitgevoerd en u kunt maximaal 5 exemplaren actief zijn voor maximale beschikbaarheid hebben.  Hybride verbindingen biedt alleen ondersteuning voor TCP door en elk eindpunt CH heeft worden vergeleken met een combinatie van specifieke host: poort.  

De App-omgeving-functie kunt u een exemplaar van de Azure-Service voor App uitvoeren in uw VNET.  Hiermee kunt uw apps toegang tot bronnen in uw VNET zonder eventuele extra stappen uitvoeren.  Een deel van de andere voordelen van een App-Service-omgeving is die u 8 core specifiek werknemers met 14 GB RAM kunt gebruiken.  Een ander voordeel is dat u het systeem aan uw wensen kunt schalen.  In tegenstelling tot de meerdere tenant-omgevingen waar uw ASP omvang beperkte, in een ASE bepalen u hoe veel bronnen die u wilt verlenen aan het systeem.  Met betrekking tot de focus van het netwerk van dit document is een van de dingen die u met een ASE die u niet met VNET-integratie echter dat deze met een VPN-ExpressRoute werken kunt.  

Hoewel er dat enkele hoofdletters/kleine letters elkaar overlappen gebruiken, kunt geen van deze functie een van de andere kolommen te vervangen.  U weet welke functie gebruik is gekoppeld aan uw wensen voldoet en hoe u wilt gebruiken.  Bijvoorbeeld:

- Als u een ontwikkelaar bent en u wilt uitvoeren van een site in Azure wordt aangegeven en hebt deze toegang heeft tot de database op het werkstation onder uw bureau is de eenvoudigste wat u moet gebruiken hybride verbindingen.  
- Als u een grote organisatie die een groot aantal wil wordt Webeigenschappen in het publiek cloud en in uw eigen netwerk beheren, en u wilt gaan met de App-Service-omgeving.  
- Als er wordt een aantal App Service apps gehost en wilt gewoon toegang krijgen tot bronnen in uw VNET en vervolgens VNET-integratie is de manier om te gaan.  

Voorbij de gebruik gevallen zijn er enkele eenvoudig gerelateerd aspecten.  Als uw VNET al is verbonden met uw netwerk door aan premises vervolgens gebruik VNET integratie of een App-Service-omgeving is een eenvoudige manier premises bronnen in beslag neemt.  Aan de andere kant, dat als uw VNET niet met uw aan premises netwerk verbonden is is veel meer realiseren voor het instellen van een site naar site-VPN-verbinding met uw VNET vergeleken met het installeren van de HCM.  

Naast de functionele verschillen zijn er ook prijzen verschillen.  De functie App-omgeving is een Premium serviceaanbod maar biedt de meeste netwerk configuratiemogelijkheden naast andere handige functies.  VNET integratie met standaard of Premium ASP's kan worden gebruikt en is ideaal voor het veilig door andere bronnen in uw VNET uit de meerdere tenant App Service.  Hybride verbindingen is momenteel is afhankelijk van een BizTalk account met niveaus die gratis starten en vervolgens geleidelijk meer dure prijzen op basis van het bedrag dat u nodig hebt.  Wanneer u gaat naar Hoewel veel netwerken werken, is er geen andere functies zoals hybride verbindingen die u toegang tot bronnen in ook meer dan 100 afzonderlijke netwerken kunt inschakelen.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
