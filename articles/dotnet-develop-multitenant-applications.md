<properties
    pageTitle="Meerdere Tenant Web Application patroon | Microsoft Azure"
    description="Architectuur overzichten en ontwerppatronen die wordt beschreven hoe u een webtoepassing met meerdere tenant implementeert op Azure zoeken."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Multitenant toepassingen in Azure wordt aangegeven

Een toepassing voor de multitenant is een gedeelde bron waarmee afzonderlijke gebruikers of "tenants," om weer te geven van de toepassing alsof deze was hun eigen. Een typisch voorbeeld die gepaard met een toepassing voor de multitenant is een waarin alle gebruikers van de toepassing wilt mogelijk aanpassen van de gebruikerservaring maar anders hebben dezelfde basismogelijkheden voor business eisen. Voorbeelden van grote multitenant toepassingen zijn Office 365, Outlook.com en visualstudio.com.

Vanuit het oogpunt van de toepassingsprovider van een is de voordelen van het multitenancy voornamelijk zich verhouden tot werkwijzen en kosten efficiëntie. Één versie van uw toepassing kan de behoeften van vele tenants/klanten, waarin de samenvoeging van systeem beheerderstaken zoals monitoring, prestaties optimaliseren onderhoud van software en back-ups van gegevens.

Hieronder vindt u een lijst van de belangrijkste doelstellingen en vereisten vanuit het oogpunt van een provider.

- **Provisioning**: moet u kunnen zijn voor het inrichten van nieuwe tenants voor de toepassing.  Voor multitenant toepassingen met een groot aantal tenants is dit meestal moet u dit proces doordat de inrichting van selfservice-automatiseren.
- **Onderhoud**: U moet kunnen upgraden van de toepassing en andere onderhoudstaken uitvoeren, terwijl meerdere tenants gebruikt.
- **Controle**: U moet kunnen controleren van de toepassing allen tijde sprake is van problemen identificeren en oplossen van problemen. Dit geldt ook voor monitoring hoe elke tenant de toepassing wordt gebruikt.

Een toepassing voor correct toegepast multitenant biedt de volgende voordelen aan gebruikers.

- **Moeten worden geïsoleerd**: de activiteiten van afzonderlijke tenants hebben geen invloed op het gebruik van de toepassing door andere tenants. Tenants geen toegang tot elkaars gegevens. Het lijkt op de tenant alsof ze exclusief gebruik van de toepassing hebben.
- **Beschikbaarheid**: afzonderlijke tenants wilt dat de toepassing kan worden misschien voortdurend gecontroleerd, garanties die zijn gedefinieerd in een SLA. De activiteiten van andere tenants moeten opnieuw, geen invloed op de beschikbaarheid van de toepassing.
- **Schaalbaarheid**: de toepassing wordt aangepast aan het voldoen aan de vraag van afzonderlijke tenants. De aanwezigheid en acties van andere tenants moeten geen invloed op de prestaties van de toepassing.
- **Kosten**: kosten zijn lager dan het uitvoeren van een toepassing speciale, één-tenant omdat meerdere pachtadres kunt het delen van resources.
- **Aanpassingsmogelijkheden**. De mogelijkheid om aan te passen van de toepassing voor een individuele tenant op verschillende manieren zoals toevoegen of verwijderen van functies, kleuren en logo's wijzigen of zelfs hun eigen code of script toe te voegen.

Kortom, terwijl er veel enkele zaken waarmee u rekening houden moet om een zeer scalable service te leveren zijn, zijn er ook een getal van de doelstellingen en de vereisten die voor veel multitenant toepassingen gelden. Een deel mogelijk niet relevant in specifieke scenario's en de urgentie van een afzonderlijke doelstellingen en vereisten verschillen in elk scenario. Als een provider van de multitenant toepassing, ook hebt doelstellingen en vereisten zoals de tenants doelstellingen en vereisten, winstgevendheid, facturering, meerdere serviceniveaus, inrichting, onderhoud cmdlets voor controle en automatisering de vergadering.

Zie [een toepassing meerdere Tenant op Azure hostingprovider][]voor meer informatie over aanvullende ontwerpoverwegingen van een toepassing voor de multitenant. Zie voor informatie over algemene gegevens architectuur patronen van meerdere tenant software-als-een-service (SaaS) databasetoepassingen, [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure biedt veel functies waarmee u kunt het adres van de belangrijkste problemen bij het ontwerpen van een multitenant systeem.

**Moeten worden geïsoleerd**

- Segment Website Tenants door Host-Headers met of zonder SSL-communicatie
- Segment Website Tenants door queryparameters
- Webservices in werknemer rollen
    - Rollen van werknemer. die gegevens op de backend van een toepassing voor de meestal verwerken.
    - Web rollen die meestal als de frontend voor toepassingen fungeren.

**Opslag**

Het beheren van de gegevens zoals Azure SQL-Database of Azure Storage services zoals de service van de tabel waarmee services voor de opslag van grote hoeveelheden ongestructureerde gegevens en de Blob-service waarmee services om op te slaan grote hoeveelheden ongestructureerde tekst of binaire gegevens zoals video, audio en afbeeldingen.

- Beveiligingsgegevens Multitenant in SQL-Database nodig per-tenant SQL Server-aanmeldingen.
- Azure-tabellen gebruiken voor de toepassing Resources door het opgeven van een container niveau-beleid, kunt u de mogelijkheid om aan te passen machtigingen zonder dat u moet nieuwe URL's voor de resources die zijn beveiligd met gedeelde toegang handtekeningen actie.
- Azure wachtrijen voor toepassing Resources Azure wachtrijen worden gebruikt bij het station verwerking namens tenants, maar kunnen ook worden gebruikt voor het werk dat is vereist voor Inrichtings- of management verdelen.
- Service Bus wachtrijen voor toepassing Resources waardoor werken aan een gedeelde een service, kunt u één wachtrij waar iedere afzender tenant alleen machtigingen heeft (zoals afgeleid van claims uitgegeven door ACS) om te voeren naar die wachtrij, terwijl u alleen de ontvangers van de service zijn gemachtigd om op te halen uit de wachtrij de gegevens die afkomstig zijn uit meerdere tenants.


**Verbinding en beveiligingsservices**

- Azure-Service Bus, een SMS-infrastructuur die bevindt zich tussen toepassingen zodat ze uitwisselen van berichten in een losse manier voor verbeterde schaal en tolerantie.

**Netwerkservices**

Azure biedt diverse netwerken services die ondersteuning voor verificatie en beheerbaarheid van uw gehoste toepassingen. Deze services zijn:

- Azure Virtual Network kunt u inrichten en beheren van virtuele particuliere netwerken (VPN's) in Azure wordt aangegeven, evenals veilig koppelen aan on-premises implementatie IT-infrastructuur.
- Virtual Network verkeer Manager kunt u balance '-binnenkomende verkeer op meerdere services voor de gehoste Azure laden of ze in het dezelfde datacenter een of meer verschillende datacenters overal ter wereld uitvoert.
- Azure Active Directory (Azure AD) is een moderne, REST gebaseerde service die mogelijkheden voor beheer en toegang beheer voor uw cloud-toepassingen biedt. Met behulp van Azure AD voor toepassing Resources Azure AD aan u een eenvoudige manier voor verificatie en machtiging gebruikers toegang te krijgen tot uw webtoepassingen en services terwijl u de functies van verificatie en machtiging om te worden meegenomen afmelden bij uw code.
- Azure-Service Bus biedt een beveiligde expresberichten en gegevens-mailstroom de mogelijkheid voor distributed en hybride-toepassingen, zoals de communicatie tussen Azure gehost toepassingen en on-premises implementatie-toepassingen en -services, zonder complexe infrastructuur voor firewall en beveiliging. Met behulp van Service Bus Relay voor toepassing Resources met de services die beschikbaar zijn als eindpunten mogelijk behoren tot de tenant (bijvoorbeeld hosten buiten het systeem, zoals on-premises), of ze mogelijk services deze is ingericht specifiek voor de tenant (omdat gevoelige, tenant-specifieke gegevens ze passeert).



**Inrichting van Resources**

Azure biedt een aantal manieren inrichten nieuwe tenants voor de toepassing. Voor multitenant toepassingen met een groot aantal tenants is dit meestal moet u dit proces doordat de inrichting van selfservice-automatiseren.

- Werknemer rollen kunt u naar leveren en intrekken per pachter resources (bijvoorbeeld wanneer een nieuwe tenant positief of negatief-up of annuleert), aan de doelstellingen verzamelen voor meting gebruiken en beheren na een bepaalde planning schaal of in antwoord op het overschrijden van drempelwaarden van key performance indicators. Deze dezelfde rol kan ook worden gebruikt voor push-updates en upgrades met de oplossing.
- Azure BLOB's kunnen worden gebruikt voor het inrichten van berekeningscluster of vooraf geïnitialiseerd opslagbronnen voor nieuwe tenants terwijl de container niveau clienttoegangsbeleid te beveiligen van de berekeningscluster service-pakketten, VHD afbeeldingen en andere resources.
- Opties voor het inrichten van de SQL-Database resources voor een tenant zijn onder andere:

    -   DDL in scripts of ingesloten als resources uit assemblies
    -   SQL Server 2008 R2 DAC pakketten geïmplementeerd via programmacode.
    -   Kopiëren uit een database basispagina verwijzing
    -   Database importeren en exporteren gebruiken voor het inrichten van nieuwe databases uit een bestand.



<!--links-->

[Een toepassing meerdere Tenant op Azure hostingprovider]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
