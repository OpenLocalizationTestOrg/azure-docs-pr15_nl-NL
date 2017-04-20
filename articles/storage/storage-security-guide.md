<properties
    pageTitle="Azure opslag beveiligingshandleiding | Microsoft Azure"
    description="Details van de vele methoden voor het beveiligen van Azure-opslag, inclusief, maar niet beperkt tot RBAC, opslag Service versleuteling, aan de clientzijde versleuteling, SMB 3.0 en Azure schijfversleuteling."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure handleiding voor opslag-beveiliging

##<a name="overview"></a>Overzicht

Azure opslagruimte biedt een uitgebreide set van beveiligingsmogelijkheden voor waarmee samen ontwikkelaars op beveiligde toepassingen. De opslag-account zelf kan worden beveiligd met toegangsbeheer op basis van rollen en Azure Active Directory. Gegevens kunnen worden beveiligd tijdens overdracht tussen een toepassing en Azure met behulp van [Aan de clientzijde versleuteling](storage-client-side-encryption.md), HTTPS of SMB 3.0. Gegevens kan worden ingesteld op automatisch worden gecodeerd als geschreven met Azure Storage met [Opslag Service versleuteling (SSE)](storage-service-encryption.md). OS en gegevens schijven die worden gebruikt door virtuele machines kunnen worden ingesteld te coderen met [Azure schijfversleuteling](../security/azure-security-disk-encryption.md). Gedelegeerd toegang tot de gegevensobjecten in Azure-opslag kan worden verleend [Gedeeld Access handtekeningen](storage-dotnet-shared-access-signature-part-1.md)gebruiken.

In dit artikel wordt een overzicht van elk van deze beveiligingsvoorzieningen die kunnen worden gebruikt met Azure Storage bieden. Koppelingen zijn opgegeven naar artikelen met details van elke functie dat geeft u eenvoudig kunt doen nader onderzoek op elk onderwerp.

Hier volgen de onderwerpen in dit artikel:

-   [Management vlak beveiliging](#management-plane-security) – beveiligen van uw Account opslag

    Het vlak van management bestaat uit de resources die worden gebruikt voor het beheren van uw account opslag. In dit gedeelte bespreken we het implementatiemodel van Azure resourcemanager en het gebruik van Rolgebaseerd toegangsbeheer (RBAC) toegang tot uw accounts opslag beheren. We wordt ook overleggen over het beheren van uw opslagruimte account sleutels en hoe u ze opnieuw te genereren.

-   [Gegevens vlak beveiliging](#data-plane-security) – toegang tot uw gegevens beveiligen

    In deze sectie, zullen we er toegang tot de werkelijke gegevens-objecten in uw opslag-account, zoals BLOB's, bestanden, wachtrijen en tabellen, met Access handtekeningen gedeeld en Clienttoegangsbeleid die zijn opgeslagen. Aan bod komen zowel service level SA's en -account op gebruikersniveau SA's. We ziet er ook het beperken van toegang tot een specifiek IP-adres (of een bereik van IP-adressen), hoe u beperkingen instellen voor het protocol dat wordt gebruikt in HTTPS en hoe een handtekening gedeeld toegang intrekken zonder te wachten op verlopen.

-   [Versleuteling tijdens overdracht](#encryption-in-transit)

    In deze sectie wordt beschreven hoe om gegevens te beveiligen wanneer u deze in- of uitzoomen op Azure Storage overbrengen. Bespreken we de aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor bestandsshares Azure. We wordt ook kost, bekijkt aan de clientzijde versleuteling, waarmee u de gegevens versleutelen voordat deze wordt overgebracht op te slaan in een clienttoepassing en tot de versleutelde gegevens na het overbrengen geen opslagruimte meer.

-   [Versleuteling in rust](#encryption-at-rest)

    We wordt communiceren over opslag Service-versleuteling (SSE), en hoe u deze inschakelen voor een account opslag, wat resulteert in uw blok BLOB's, pagina BLOB's, en BLOB's automatisch gecodeerde geschreven met Azure Storage toevoegt. We gaan ook bekijken hoe u Azure-schijf versleuteling en de eenvoudige verschillen en dozen van schijfversleuteling versus SSE versus aan de clientzijde versleuteling verkennen. We bekijkt FIPS-naleving voor Amerikaanse overheid computers kort.

-   Gebruik van [Opslagruimte Analytics](#storage-analytics) toegang Azure opslag controleren

    In deze sectie wordt beschreven hoe om informatie te vinden in de opslagruimte analytics-logboeken voor een nieuw vergaderverzoek. We gaat u naar reële opslag analytics logboekgegevens en lees hoe u onderscheiden of een nieuw vergaderverzoek met de toets opslag-account, met een handtekening toegang tot gedeelde bestaat uit de of anoniem en of deze is geslaagd of mislukt.

-   [Op basis van Browser Clients met CORS inschakelen](#Cross-Origin-Resource-Sharing-CORS)

    In dit gedeelte moment spreekt over het toestaan van cross-origin resource delen (CORS). Bespreken we tussen domeinen access en hoe u omgaat met deze met de CORS-mogelijkheden in Azure Storage ingebouwd.

##<a name="management-plane-security"></a>Management vlak beveiliging

Het vlak van management bestaat uit de bewerkingen die betrekking hebben op de opslag-account zelf. Bijvoorbeeld kunt u maken of verwijderen van een opslag-account, krijgt een lijst met accounts van opslag in een abonnement, ophalen van de opslag-account te gebruiken of genereren van de opslag-account te gebruiken.

Wanneer u een nieuw opslag-account hebt gemaakt, selecteert u een implementatiemodel van Classic of resourcemanager. Het model Klassiek van het maken van resources in Azure kunt alleen ofwel volledig, ofwel access naar het abonnement en klik in inschakelen, het opslag-account.

Deze handleiding is gericht op het model resourcemanager die de aanbevolen gemiddelden voor opslag-accounts maken. Met de resourcemanager opslag accounts plaats toegang geven aan het hele abonnement, kunt u bepalen access op meer eindige machtigingsniveau dat u wilt het management vlak Rolgebaseerd toegangsbeheer (RBAC) gebruikt.

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Het beveiligen van uw account opslagruimte met toegangsbeheer op basis van rollen RBAC)

Computers nader bekeken wat RBAC is en hoe u deze kunt gebruiken. Elke Azure abonnement heeft een Azure Active Directory. Gebruikers, groepen en toepassingen van die directory kunnen beheren van resources in het Azure abonnement met het implementatiemodel resourcemanager toegang is verleend. Dit is Rolgebaseerd toegangsbeheer (RBAC) genoemd. Als u wilt deze toegang beheren, kunt u de [Azure-portal](https://portal.azure.com/), de [Hulpmiddelen voor Azure CLI](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)of de [Azure Storage Resource Provider REST API's](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Met het model resourcemanager plaatst u het account opslag in de groep en toegang van een resource op het vlak van management van die specifieke opslag-account met Azure Active Directory. Bijvoorbeeld: u kunt geven specifieke gebruikers de mogelijkheid voor toegang tot de opslag-account te drukken, terwijl andere gebruikers informatie over de opslag-account weergeven kunnen, maar geen toegang de opslag-account te gebruiken tot.

####<a name="granting-access"></a>Het verlenen van toegang

Toegang wordt verleend door de juiste RBAC rol toewijzen aan gebruikers, groepen en -toepassingen, op het juiste bereik. Als u wilt verlenen van toegang tot het hele abonnement, moet u een rol bij het abonnement toewijzen. U kunt toegang verlenen tot alle resources in een resourcegroep door machtigingen te verlenen aan de resourcegroep zelf. U kunt ook specifieke rollen toewijzen aan specifieke resources, zoals opslag-accounts.

Dit zijn de belangrijkste punten die u weten moet over het gebruik van RBAC voor toegang tot de bewerkingen management van een Azure Storage-account:

-   Wanneer u access toewijst, toewijzen u in feite een rol aan het account dat u wilt beschikken. Toegang tot de bewerkingen die worden gebruikt voor het beheren van dat opslag-account, maar niet op de gegevensobjecten in het account, kunt u bepalen. Bijvoorbeeld, kunt u machtigen om op te halen de eigenschappen van het account opslag (zoals redundantie), maar niet aan een container of de gegevens in een container binnen Blob Storage.

-   Geef ze leesmachtiging de opslag-account te gebruiken voor iemand gemachtigd voor toegang tot de gegevensobjecten in de opslagruimte-account, en die gebruiker kan deze toetsen gebruiken voor toegang tot de BLOB's, wachtrijen, tabellen en bestanden.

-   Kunnen u rollen toewijzen aan een specifieke gebruikersaccount, een groep gebruikers, of naar een specifieke toepassing.

-   Elke rol heeft een lijst met acties en niet acties. De rol Inzender VM bevat bijvoorbeeld een actie uit "listKeys" waarmee de opslag-account te gebruiken om te worden gelezen. De inzender heeft "Niet acties" zoals het bijwerken van de toegang voor gebruikers in de Active Directory.

-   Rollen voor opslag opnemen (maar zijn niet beperkt tot) de volgende handelingen uit:

    -   Eigenaar – kunt ze alles, inclusief toegang kunnen beheren.

    -   Inzender – deze kan uitvoeren alles wat u kunt de eigenaar van de kunt, met uitzondering van access toewijzen. Iemand met deze rol kan bekijken en genereren van de opslag-account te gebruiken. Met de toetsen van de account opslag ze toegang tot de gegevensobjecten.

    -   Lezer – ze kunnen informatie over de opslag-account, behalve geheimen bekijken. Bijvoorbeeld als u een rol met reader machtigingen voor de opslag-account aan iemand toewijzen, kunnen ze de eigenschappen van het account opslag bekijken, maar ze geen wijzigingen aanbrengt in de eigenschappen of bekijken van de opslag-account te gebruiken.

    -   Opslag Account Inzender – kunnen ze het account opslag beheren – ze kunnen lezen van het abonnement resourcegroepen en bronnen, en maken en beheren van abonnement resource groep implementaties. Ze ook toegang tot de opslag-account te drukken, wat op zijn beurt betekent dat ze toegang tot het vlak gegevens.

    -   Access Gebruikersbeheerder – kunnen ze de toegang van gebruikers aan het account opslag beheren. Ze kunnen bijvoorbeeld lezerstoegang verlenen aan een specifieke gebruiker.

    -   VM Inzender – dat ze kunnen beheren virtuele machines, maar niet de opslag-account waarmee ze bent verbonden. Deze rol kan een lijst met de opslag-account te drukken, wat betekent dat de gebruiker aan wie u deze rol toewijzen op het vlak gegevens kunt bijwerken.

        In de volgorde van een gebruiker een virtuele machine maken, die ze moeten kunnen maken van het bijbehorende VHD-bestand in een opslag-account. Om dat te doen, moeten ze kunnen de accountsleutel opslag opgehaald en doorgegeven tot de API voor het maken van de VM. Daarom moet ze deze machtiging zodat ze kunnen een lijst met de toetsen opslag-account.

- De mogelijkheid om te definiëren aangepaste rollen is een functie waarmee u kunt het opstellen van een reeks acties uit een lijst met beschikbare acties die kunnen worden uitgevoerd op Azure resources.

- De gebruiker moet worden ingesteld in uw Azure Active Directory voordat u een rol aan deze toewijzen kunt.

- U kunt een rapport met wie verleend/ingetrokken welk type toegang/naar wie en klik op welke bereik via PowerShell of de CLI Azure maken.

####<a name="resources"></a>Resources

-   [Azure Active Directory Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md)

    In dit artikel wordt uitgelegd het toegangsbeheer op basis van Azure Active Directory-rol en hoe deze werkt.

-   [RBAC: Ingebouwd in rollen](../active-directory/role-based-access-built-in-roles.md)

    Dit artikel wordt uitgelegd alle van de ingebouwde functies die beschikbaar zijn in RBAC.

-   [Wat zijn resourcemanager implementatie en klassieke implementatie](../resource-manager-deployment-model.md)

    In dit artikel wordt uitgelegd de resourcemanager implementatie en klassieke implementatiemodellen en wordt uitgelegd van de voordelen van het gebruik van de groepen resourcemanager- en resourcekalenders

-   [Azure berekeningscluster-, netwerk- en opslag Providers onder Azure bronbeheer](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    In dit artikel wordt uitgelegd hoe het berekenen van Azure, netwerk- en opslag van Providers onder het model resourcemanager werken.

-   [Rolgebaseerd toegangsbeheer met de REST API beheren](../active-directory/role-based-access-control-manage-access-rest.md)

    In dit artikel leest hoe u met de REST API RBAC beheren.

-   [Azure opslag Resource Provider REST API verwijzing](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Dit is de verwijzing voor de API's die u gebruiken kunt voor het beheren van uw account opslag via programmacode.

-   [Handleiding voor ontwikkelaars naar auth met Azure resourcemanager API](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    In dit artikel leest hoe u verifiëren met behulp van de Resource Manager-API's.

-   [Rolgebaseerd toegangsbeheer voor Microsoft Azure uit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dit is een koppeling naar een video op kanaal 9 uit de 2015 MS Ignite vergadering. In deze sessie, ze computers nader bekeken management en de rapportagemogelijkheden in Azure openen en te verkennen aanbevelingen rond het beveiligen van toegang tot Azure abonnementen die gebruikmaken van Azure Active Directory.

###<a name="managing-your-storage-account-keys"></a>Uw opslag Account sleutels beheren

Opslag account toetsen zijn geschikt 512 bits tekenreeksen die zijn gemaakt door Azure die samen met de naam van het opslag-account, kan worden gebruikt voor toegang tot de gegevensobjecten die zijn opgeslagen in de opslagruimte-account, bijvoorbeeld BLOB's, entiteiten in een tabel, berichten en bestanden op een Azure-bestanden delen. Toegang tot de opslag account toetsen controleert de toegang tot het vlak van gegevens voor dat account opslag beheren.

Elke opslag-account heeft twee sleutels 'Sleutel 1' en 'Belangrijke 2' in de [portal van Azure](http://portal.azure.com/) en in de PowerShell-cmdlets genoemd. Deze kunnen worden hersteld handmatig met behulp van verschillende manieren, met inbegrip van, maar niet beperkt tot het gebruik van de [Azure-portal](https://portal.azure.com/), PowerShell, de CLI Azure of via een programma via de bibliotheek .NET opslag-Client of de REST API van Azure opslag Services.

Er zijn een aantal redenen om te genereren van de toetsen van uw opslag-account.

-   U kunt ze opnieuw regelmatig vanwege de beveiliging te genereren.

-   Als iemand beheerd inbreken in om een toepassing de sleutel ophalen die is vastgelegde of opgeslagen in een configuratiebestand ze volledige toegang geven tot uw account opslag, zou u de toetsen van uw opslagruimte account opnieuw genereren.

-   Een andere hoofdletters/kleine letters voor sleutel opnieuw wordt gegenereerd is als uw team gebruikt een opslag Explorer-toepassing waarin de accountsleutel opslag behoudt en een van de teamleden verlaat. De toepassing wilt blijven werken, omdat ze toegang hebben met uw account opslag nadat ze verdwenen zijn. Dit is daadwerkelijk de belangrijkste reden die ze account niveau gedeeld Access handtekeningen hebt gemaakt, maar u kunt een account op gebruikersniveau SA's gebruiken in plaats van de toegangstoetsen opslaan in een configuratiebestand.

####<a name="key-regeneration-plan"></a>Sleutel opnieuw wordt gegenereerd plannen

U wilt niet dat alleen de sleutel die u zonder een planning gebruikt te genereren. Als u dat doet, kunt u alle access afgekapt aan dat account opslag, waardoor primaire verstoringen. Daarom er zijn twee toetsen. U opnieuw te genereren van één toets tegelijk.

Voordat u uw sleutels genereren, moet u er een lijst met al uw toepassingen die afhankelijk zijn van het account opslag, evenals andere services die u gebruikt in Azure wordt aangegeven. Bijvoorbeeld als u Azure Media Services die afhankelijk van uw opslag-account zijn gebruikt, moet u opnieuw synchroniseren de toegangstoetsen met uw service media nadat u de sleutel opnieuw genereren. Als u alle toepassingen, zoals een opslag explorer gebruikt, moet u kunt de nieuwe sleutels naar deze toepassingen ook opgeven. Houd er rekening mee dat als u VMs waarvan VHD-bestanden worden opgeslagen in de opslagruimte-account hebt, ze worden niet worden beïnvloed door de toetsen opslag-account opnieuw te genereren.

U kunt uw sleutels in de portal van Azure opnieuw genereren. Zodra de toetsen opnieuw worden gegenereerd kunnen maximaal tien minuten worden gesynchroniseerd op opslagservices nemen.

Wanneer u klaar bent, volgt de algemene proces met informatie over hoe u uw sleutel moet wijzigen. In dit geval is ervan uitgegaan dat u momenteel toets 1 gebruikt en gaat u alles als u wilt gebruiken sleutel 2 in plaats daarvan wijzigen.

1.  2-toets om ervoor te zorgen dat beveiligd is genereren. U kunt dit doen in de portal van Azure.

2.  In alle toepassingen waar de opslag-sleutel is opgeslagen, de opslag-sleutel als u wilt gebruiken sleutel 2 van nieuwe waarde te wijzigen. Test en publiceer de toepassing.

3.  Nadat alle zijn van de toepassingen en services omhoog en toets 1 is uitgevoerd, genereren. Dit zorgt ervoor dat iedereen aan wie u geen uitdrukkelijk om de nieuwe sleutel gegeven hebt niet langer toegang tot de opslag-account hebben wordt.

Als u momenteel toets 2 gebruikt, kunt u gebruikt u dezelfde stappen, maar u kunt de namen van de belangrijkste omkeren.

U kunt elke toepassing voor het gebruik van de nieuwe sleutel wijzigen en publiceren via een paar dagen, migreren. Nadat alle labels klaar bent, moet u vervolgens terug te gaan en de oude sleutel genereren, zodat deze niet meer werkt.

Er is een andere optie voor de opslag accountsleutel in een [Azure toets kluis](https://azure.microsoft.com/services/key-vault/) als een geheim hebt opgeslagen en hebben uw toepassingen de sleutel daarvandaan ophalen. Wanneer u de sleutel opnieuw genereren en bijwerken van de Azure-toets kluis, moet de toepassingen niet worden geïmplementeerd omdat ze gewoon door de nieuwe sleutel uit het Azure-toets kluis automatisch. Houd er rekening mee dat kunt u beschikken over de toepassing telkens wanneer die u deze nodig hebt voor de sleutel lezen, of u kunt deze in het geheugen in cache en als dit mislukt wanneer u gebruikt, de sleutel ophalen opnieuw uit de Azure-toets kluis.

Azure-toets kluis gebruikt, voegt ook een ander beveiligingsniveau voor uw opslagruimte sleutels. Als u deze methode te gebruiken, hebt u nooit de belangrijkste vastgelegde opslag in een configuratiebestand, waarmee wordt verwijderd die doorverwezen iemand toegang tot de toetsen zonder specifieke machtigingen aan.

Een ander voordeel van het gebruik van Azure-toets kluis is dat u kunt ook toegang aan uw sleutels met Azure Active Directory beheren. Dit betekent dat u kunt toegang verlenen tot de klein aantal toepassingen die nodig hebt voor het ophalen van de toetsen uit kluis van Azure-toets en weten dat andere toepassingen niet de toetsen worden zonder ze machtiging specifiek toegang tot.

Opmerking: het wordt aanbevolen slechts één van de toetsen tegelijkertijd in al uw toepassingen gebruiken. Als u toets 1 in enkele startpunten en 2-toets in andere gebruikt, is het niet mogelijk voor het omwisselen van uw sleutels zonder een toepassing toegang verliezen.

####<a name="resources"></a>Resources

-   [Over Accounts Azure opslag](storage-create-storage-account.md#regenerate-storage-access-keys)

    In dit artikel geeft een overzicht van opslag accounts en weergeven, kopiëren en opnieuw genereren toegangstoetsen opslag.

-   [Azure opslag Resource Provider REST API verwijzing](https://msdn.microsoft.com/library/mt163683.aspx)

    In dit artikel bevat koppelingen naar specifieke artikelen over het ophalen van de toetsen opslag-account en opnieuw te genereren van de opslag-account te gebruiken voor de REST API gebruiken om een Azure-Account. Opmerking: Dit is voor resourcemanager opslag-accounts.

-   [Bewerkingen voor opslag-accounts](https://msdn.microsoft.com/library/ee460790.aspx)

    In dit artikel in de Service opslagbeheer REST API Reference bevat koppelingen naar specifieke artikelen op te halen en in de opslagruimte account te gebruiken met de REST API opnieuw te genereren. Opmerking: Dit is voor de klassieke opslag-accounts.

-   [Stel dat afsluiting om belangrijke management – beheer van toegang tot gegevens van Azure opslagruimte met Azure AD](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    In dit artikel leest hoe u toegang tot uw sleutels Azure-opslag in Azure-toets kluis met Active Directory. Ook ziet u hoe u gebruikt om een taak Azure automatisering te genereren van de toetsen per uur worden berekend.

##<a name="data-plane-security"></a>Beveiliging van gegevens vlak

Beveiliging van gegevens vlak verwijst naar de gebruikte methoden voor het beveiligen van de gegevensobjecten die zijn opgeslagen in Azure opslag – de BLOB's, wachtrijen, tabellen en bestanden. We methoden voor het coderen van de gegevens en de beveiliging tijdens overdracht van de gegevens hebt gezien, maar hoe ga u over het toestaan van toegang tot de objecten?

Er zijn in principe twee methoden voor toegang tot de gegevensobjecten zelf beheren. De eerste is door de toegang tot de toetsen van de account opslag beheren en de tweede gedeeld Access handtekeningen wordt gebruikt om toegang te verlenen aan specifieke gegevensobjecten voor een specifieke hoeveelheid tijd.

Een geldt opmerking dat u openbare toegang aan uw BLOB's verlenen kunt door in te stellen van het toegangsniveau voor de container met de BLOB's dienovereenkomstig gewijzigd. Als u toegang hebt ingesteld voor een container Blob of Container, kan deze openbare leestoegang voor de BLOB's in dat onderdeel. Dit betekent dat iedereen met een URL die verwijst naar een blob in dat onderdeel kunt openen in een browser zonder een handtekening gedeeld Access gebruiken of steeds de opslag-account te gebruiken.

###<a name="storage-account-keys"></a>Opslag Account toetsen

Opslag account toetsen zijn geschikt 512 bits tekenreeksen die zijn gemaakt door Azure die, samen met de naam van het opslag-account, kan worden gebruikt voor toegang tot de gegevensobjecten die zijn opgeslagen in de opslagruimte-account.

U kunt bijvoorbeeld BLOB's lezen, schrijven naar wachtrijen, tabellen maken en wijzigen. Veel van deze acties kunnen worden uitgevoerd via de portal Azure, of gebruik een van de vele opslag Explorer-toepassingen. U kunt ook schrijven code voor het gebruik van de REST API of op een van de opslagruimte Client-bibliotheken naar deze bewerkingen uitvoeren.

Zoals beschreven in de sectie voor het [Beheer van vlak beveiliging](#management-plane-security), kan toegang tot de toetsen opslagruimte voor een klassiek opslag-account kan worden verleend door te geven van volledige toegang tot het Azure abonnement. Toegang tot de toetsen opslagruimte voor een opslag-account met het model Azure ResourceManager kan worden gecontroleerd door Rolgebaseerd toegangsbeheer RBAC ().

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Hoe u Gemachtigdentoegang aan objecten in uw account met Access handtekeningen gedeeld en Clienttoegangsbeleid die zijn opgeslagen

Een Access gedeeld handtekening is een tekenreeks met een beveiligingstoken dat kan worden gekoppeld aan een URI waarmee u gemachtigdentoegang tot opslagobjecten en het instellen van beperkingen zoals de machtigingen en de datum/tijd-bereik van access.

U kunt toegang verlenen tot BLOB's, containers, berichten, bestanden en tabellen. Met tabellen, kunt u daadwerkelijk machtigen voor toegang tot een bereik van entiteiten in de tabel door het opgeven van de partition en rij belangrijke bereiken waarnaar u wilt dat de gebruiker toegang heeft. Bijvoorbeeld, hebt u gegevens die zijn opgeslagen met een partitiesleutel van geografische staat, u kunt iemand toegang tot uitsluitend de gegevens voor Californië.

In een ander voorbeeld mogelijk u Geef een webtoepassing een token SA's waarmee deze posten naar een wachtrij schrijven en geven van een werknemer rol-toepassing een token SA's voor het ophalen van berichten uit de wachtrij en ze verwerken. Of u kunt een klant een token SA's ze kunnen gebruiken voor het uploaden van afbeeldingen aan een container in Blob Storage en een web-toepassing machtigen om te lezen die afbeeldingen. In beide gevallen wordt er is een scheiding van bezwaren: elke toepassing alleen de toegang die ze nodig hebben om te kunnen uitvoeren van hun taak kan worden opgegeven. Dit is mogelijk met behulp van Access handtekeningen gedeeld.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Waarom u wilt gedeeld Access handtekeningen gebruiken

Waarom zou u wilt een SA's gebruiken in plaats van alleen uw accountsleutel opslag, dat wil zoveel eenvoudiger zeggen? Geven van uw accountsleutel opslag is vergelijkbaar met de toetsen van uw opslagruimte Koninkrijk delen. Volledige toegang wordt toegewezen. Iemand kan uw sleutels gebruiken en hun hele muziekbibliotheek uploaden naar uw account opslag. Ze kunnen ook uw bestanden vervangen door virus geïnfecteerd versies of uw gegevens worden gestolen. Afwezig onbeperkte toegang geven tot uw account opslag is iets dat niet licht moeten worden genomen.

Met gedeelde Access handtekeningen, kunt u een client alleen de machtigingen die zijn vereist voor een beperkte tijdsduur uitproberen geven. Bijvoorbeeld als iemand een blob naar uw account uploaden is, kunt u ze schrijven toegang verlenen voor alleen voldoende tijd vrij voor het uploaden van de blob (afhankelijk van de grootte van de blob, natuurlijk). En als u van gedachten verandert, kunt u die toegang intrekken.

Bovendien kunt u opgeven dat aanvragen die zijn gemaakt met een SA's beperkt tot een bepaalde IP-adres of IP-adresbereiken buiten Azure zijn. U kunt ook vereisen dat aanvragen worden gedaan met een bepaald protocol (HTTPS of HTTP-/ HTTPS). Dit betekent als u alleen wilt toestaan dat HTTPS-verkeer is toegestaan, u de vereiste protocol alleen op HTTPS instellen kunt en HTTP-verkeer wordt geblokkeerd.

####<a name="definition-of-a-shared-access-signature"></a>Definitie van een gedeelde Access-handtekening

Een Access gedeeld handtekening is een verzameling queryparameters toegevoegd aan de URL die verwijst naar de resource

dat vindt u informatie over de toegang is toegestaan en de periode waarvoor u de toegang is toegestaan. Hier volgt een voorbeeld; Deze URI biedt alleen toegang tot een blob voor vijf minuten. Opmerking SA's queryparameters moet URL-codering, zoals % 3A voor dubbele punt (:) of % 20 voor een spatie.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Hoe de toegang tot gedeelde handtekening is geverifieerd door de Azure Storage-Service

Als de storage-service de aanvraag ontvangt, de parameters invoer voor de query en Hiermee maakt u een handtekening met dezelfde methode als het programma dat bellen. Vervolgens worden de twee handtekeningen vergeleken. Als ze akkoord gaat, kunt klikt u vervolgens de storage-service controleren de versie van de service opslagruimte Controleer of dat deze geldig is, controleert u of de huidige datum en tijd in het opgegeven venster zijn, zorg ervoor dat de toegang aangevraagd overeenkomt met het verzoek, enzovoort.

Bijvoorbeeld, met onze URL, zou als de URL die naar een bestand in plaats van een blob wijst is, deze aanvraag mislukt omdat hiermee dat de toegang tot gedeelde handtekening is bedoeld voor een blob. Als de opdracht REST wordt aangeroepen is bij een blob, zou dit mislukt omdat de toegang tot gedeelde handtekening geeft aan dat alleen leestoegang is toegestaan.

####<a name="types-of-shared-access-signatures"></a>Typen gedeelde toegang handtekeningen

-   Een service level SA's kan worden gebruikt voor toegang tot specifieke resources in een opslag-account. Enkele voorbeelden hiervan zijn een lijst met BLOB's in een container, een blob downloaden, het bijwerken van een item in een tabel, berichten toevoegen aan een wachtrij of een bestand uploaden naar een bestandsshare opgehaald.

-   Een account op gebruikersniveau SA's kan worden gebruikt voor toegang tot alle items waarin een service level SA's kan worden gebruikt voor. Bovendien kunt deze opties voor resources die niet zijn toegestaan met een service level SA's, zoals de mogelijkheid om te maken van containers, tabellen, wachtrijen en bestandsshares geven. U kunt ook toegang tot meerdere services in één keer opgeven. Zoals u iemand mogelijk geven toegang tot BLOB's en bestanden in uw account opslag.

####<a name="creating-an-sas-uri"></a>Een URI SA's maken

1.  U kunt een ad-hoc URI maken op aanvraag, alle queryparameters telkens wanneer definiëren.

    Dit is echt flexibele, maar als er een logische set met parameters die elke keer lijken, een opgeslagen-beleid is een beter beeld.

2.  U kunt een opgeslagen-beleid voor een hele container, bestandsshare, tabel of wachtrij maken. Vervolgens kunt u dit als basis voor het SA's URI's die u maakt. Machtigingen op basis van de opgeslagen Clienttoegangsbeleid kunnen eenvoudig worden ingetrokken. U kunt maximaal 5 beleidsregels die zijn gedefinieerd op elke container, wachtrij, tabel of bestandsshare hebben.

    Als u zijn veel mensen de BLOB's in een specifieke container lezen, kon u bijvoorbeeld een opgeslagen-beleid met de mededeling dat "leestoegang geven" en eventuele andere instellingen die niet hetzelfde zijn telkens wanneer maken. Vervolgens kunt u een SAS URI de instellingen van het beleid van de toegang die is opgeslagen en de verlooptijd datum/tijd te geven. Het voordeel van deze is dat u niet hoeft te alle de queryparameters elke keer opgeven.

####<a name="revocation"></a>Intrekken

Stel dat uw SA's meer veilig is of als u wilt deze vanwege zakelijke beveiliging of toepasselijke regelgeving wijzigen. Hoe u toegang tot een resource die SA's met intrekken? Dit is afhankelijk van hoe u de URI SAS hebt gemaakt.

Als u ad-hoc-URI's gebruikt, hebt u drie opties. U kunt de actie-SA's tokens met korte verloopbeleid en gewoon wachten om door de SA's verloopt. U kunt de naam van wijzigen of verwijderen van de resource (ervan uitgaande dat het token is beperkt tot één object). U kunt de opslagruimte account toetsen wijzigen. Deze laatste optie grote gevolgen, afhankelijk van hoeveel services dat account opslag gebruikt kunt hebben en waarschijnlijk is niet iets dat die u doen wilt zonder een planning.

Als u een SA's afgeleid van een opgeslagen-beleid gebruikt, kunt u access verwijderen door het intrekken van het beleid Access opgeslagen: u kunt deze net wijzigen zodat deze al is verlopen of kunt u deze helemaal verwijderen. Hiermee wordt onmiddellijk van kracht en elke SA's gemaakt met behulp van beleid toegang die zijn opgeslagen, wordt ongeldig. Bijwerken of verwijderen van het Access opgeslagen beleid mogelijk impact aanvragen voor toegang tot die specifieke container, bestandsshare, tabel of wachtrij via SA's, maar als de clients worden geschreven, zodat ze een nieuwe SA's aanvragen wanneer het oude account ongeldig is, wordt dit werkt prima.

Omdat met een SA's afgeleid van een opgeslagen-beleid, u de mogelijkheid om af te trekken dat SA's direct hebt, is het aanbevolen wordt aanbevolen Clienttoegangsbeleid opgeslagen indien mogelijk altijd wilt gebruiken.

####<a name="resources"></a>Resources

Raadpleeg de volgende artikelen voor meer gedetailleerde informatie over het gebruik van Access-handtekeningen gedeeld en opgeslagen Clienttoegangsbeleid, inclusief voorbeelden:

-   Hierna ziet u de artikelen overzicht.

    -   [Service SA 's](https://msdn.microsoft.com/library/dn140256.aspx)

        In dit artikel bevat voorbeelden van het gebruik van een service level SA's met BLOB's, berichten, tabel bereiken en bestanden.

    -   [Het bouwen van een service SA 's](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Het bouwen van een account SA 's](https://msdn.microsoft.com/library/mt584140.aspx)

-   Hierna ziet u zelfstudies voor het gebruik van de bibliotheek .NET-client Access handtekeningen gedeeld en opgeslagen-beleid maken.

    -   [Gedeelde toegang handtekeningen (SA's) gebruiken](storage-dotnet-shared-access-signature-part-1.md)

    -   [Gedeelde handtekeningen in Access, deel 2: Maken en gebruiken van een SA's met de Blob-Service](storage-dotnet-shared-access-signature-part-2.md)

        In dit artikel wordt uitgelegd van het model SA's, voorbeelden van handtekeningen van Access gedeeld en gebruikt u aanbevelingen voor de beste manier van SA's. Ook besproken, is het intrekken van machtigingen.

-   Toegang beperken op basis van IP-adres (IP-ACL's)

    -   [Wat is een eindpunt toegangsbeheerlijst (ACL's)?](../virtual-network/virtual-networks-acl.md)

    -   [Het bouwen van een Service SA 's](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Dit is de naslagartikel voor service level SA's; het bevat een voorbeeld van IP-ACLing.

    -   [Het bouwen van een Account SA 's](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Dit is de naslagartikel voor account niveau SA's; het bevat een voorbeeld van IP-ACLing.

-   Verificatie

    -    [Verificatie voor de van Azure-opslagservices](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Access handtekeningen aan de slag Zelfstudievideo gedeeld

    -   [SA's aan de slag zelfstudie](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Versleuteling tijdens overdracht

###<a name="transport-level-encryption--using-https"></a>Transport niveau versleuteling – HTTPS gebruiken

Een andere stap die u uitvoeren moet om te zorgen dat de beveiliging van uw gegevens van Azure Storage is coderen van de gegevens tussen de client en Azure Storage. De eerste aanbeveling is altijd het [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -protocol, waardoor beveiligde communicatie via de openbare Internet wilt gebruiken.

U moet altijd HTTPS gebruiken tijdens de REST API's bellen of toegang krijgen tot in opslag objecten. **Access-handtekeningen gedeeld**, kunnen worden gebruikt om de gemachtigde toegang tot Azure Storage-objecten, is ook een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt wanneer u een Access-handtekeningen gedeeld, ervoor te zorgen dat iedereen verstuurt koppelingen met SA's tokens van het juiste protocol gebruikmaken gebruikt.

####<a name="resources"></a>Resources

-   [HTTPS inschakelen voor een app in Azure App-Service](../app-service-web/web-sites-configure-ssl-certificate.md)

    Dit artikel leest u hoe u HTTPS inschakelen voor een Azure-Web-App.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Gebruik van versleuteling tijdens overdracht met Azure bestandsshares

Azure bestandsopslag ondersteunt HTTPS bij gebruik van de REST API, maar u kunt meer vaak gebruikt als een bestandsshare SMB op een VM is aangesloten. SMB 2.1 biedt geen ondersteuning voor versleuteling, zodat verbindingen zijn alleen toegestaan binnen dezelfde regio in Azure wordt aangegeven. Echter SMB 3.0 ondersteunt codering en kan worden gebruikt met Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10, zodat meerdere regio openen en zelfs van access op het bureaublad.

Houd er rekening mee dat terwijl Azure bestandsshares kan worden gebruikt met Unix, de Linux SMB-client niet nog codering, ondersteunt zodat toegang alleen binnen een Azure regio is toegestaan. Ondersteuning voor Linux codering is op het overzicht van Linux ontwikkelaars verantwoordelijk voor het SMB-functionaliteit. Wanneer ze versleuteling toevoegt, hebt u de dezelfde mogelijkheid voor toegang tot een bestandsshare Azure op Linux zoals u zou voor Windows doen.

####<a name="resources"></a>Resources

-   [Het gebruik van Azure bestandsopslag met Linux](storage-how-to-use-files-linux.md)

    In dit artikel leest hoe u een bestandsshare Azure koppelen op een Linux-systeem en uploaden/downloaden van bestanden.

-   [Aan de slag met Azure bestandsopslag in Windows](storage-dotnet-how-to-use-files.md)

    In dit artikel geeft een overzicht van Azure bestandsshares en hoe u koppelen en deze gebruiken met behulp van PowerShell en .NET.

-   [Inside Azure bestandsopslag](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    In dit artikel wordt gemeld die de algemene beschikbaarheid van Azure bestandsopslag en bevat technische informatie over de SMB 3.0-codering.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Aan de clientzijde versleuteling gebruikt om gegevens die u naar opslag verzendt te beveiligen

Aan de clientzijde versleuteling wordt is door een andere optie waarmee u ervoor zorgen dat uw gegevens veilig is terwijl tussen een clienttoepassing voor de en opslag worden verplaatst. De gegevens worden gecodeerd voordat worden overgebracht naar Azure Storage. Wanneer het ophalen van de gegevens van Azure-opslag, de gegevens ontsleuteld nadat het aan de clientzijde is ontvangen. Hoewel de gegevens worden versleuteld gaan via een verbinding, raadzaam is dat u ook HTTPS gebruikt, omdat deze gegevens integriteitscontroles ingebouwd in welke help Verklein netwerkfouten dat dit gevolgen heeft de integriteit van de gegevens bevat.

Aan de clientzijde versleuteling is ook een methode voor het coderen van uw gegevens in rust, zoals de gegevens zijn opgeslagen in de versleutelde vorm. Bespreken we dit in de sectie uitgebreider op [versleuteling in rust](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Versleuteling in rust

Er zijn drie Azure functies waarmee versleuteling in rust. Azure schijfversleuteling wordt gebruikt voor het coderen van de schijven OS en gegevens in IaaS virtuele Machines. De twee andere – aan de clientzijde versleuteling en SSE – worden beide gebruikt voor het coderen van gegevens in Azure opslag. Laten we Bekijk elk van deze, en vervolgens een vergelijking doen en zien wanneer elkaar kan worden gebruikt.

Terwijl u aan de clientzijde versleuteling kunt gebruiken voor het coderen van de gegevens tijdens overdracht (die ook zijn opgeslagen in de versleutelde vorm in opslag), kunt u desgewenst gewoon HTTPS gebruiken tijdens de overdracht en dat nog enkele manier om de gegevens die moeten worden automatisch gecodeerd wanneer deze is opgeslagen. Er zijn twee manieren hiervoor--Azure schijfversleuteling en SSE. Een wordt gebruikt voor het coderen van de gegevens op OS en gegevens schijven die worden gebruikt door VMs rechtstreeks en de andere wordt gebruikt voor het coderen van gegevens naar Azure-blobopslag geschreven.

###<a name="storage-service-encryption-sse"></a>Opslag Service versleuteling (SSE)

SSE kunt u dat de storage-service automatisch coderen van de gegevens bij het schrijven van deze met Azure Storage aanvragen. Wanneer u de gegevens van Azure Storage gelezen, wordt deze door de storage-service worden ontsleuteld voordat het wordt geretourneerd. Hiermee kunt u uw gegevens te beveiligen zonder te wijzigen van de code of code hebt toegevoegd aan alle toepassingen.

Dit is een instelling die van toepassing op de hele opslag-account. U kunt inschakelen en deze functie uitschakelen door de waarde van de instelling wijzigen. Hiervoor kunt u de Azure-portal, PowerShell, de CLI Azure, de opslag Resource Provider REST API of de bibliotheek .NET opslag-Client. SSE is standaard uitgeschakeld.

Op dit moment kan worden de toetsen die wordt gebruikt voor het coderen beheerd door Microsoft. We de toetsen oorspronkelijk genereren en de veilige opslag van de toetsen, evenals de normale draaiing beheren, zoals gedefinieerd door interne beleid van Microsoft. U wordt in de toekomst krijg de mogelijkheid om het beheren van uw eigen versleutelingssleutels en geef een migratiepad van Microsoft beheerde toetsen naar toetsen klant worden beheerd.

Deze functie is beschikbaar voor Standard en Premium opslag accounts gemaakt met behulp van het implementatiemodel resourcemanager. SSE geldt alleen als u wilt blokkeren BLOB's, pagina BLOB's, en BLOB's toevoegen. De andere soorten gegevens, inclusief tabellen, wachtrijen en -bestanden worden niet gecodeerd.

Gegevens worden alleen gecodeerd als SSE is ingeschakeld en de gegevens naar Blob Storage is geschreven. In- of uitschakelen van SSE heeft geen gevolgen voor bestaande gegevens. Met andere woorden, wanneer u deze codering inschakelt, deze niet terug te gaan en gegevens die al; versleutelen ook wordt de gegevens die al bestaat wanneer u SSE uitschakelt ontsleutelen.

Als u deze functie gebruiken met een klassieke opslag-account wilt, kunt u een nieuwe Resource Manager opslag-account maken en AzCopy gebruikt de gegevens wilt kopiëren naar het nieuwe account. 

###<a name="client-side-encryption"></a>Aan de clientzijde versleuteling

We aan de clientzijde versleuteling is vermeld wanneer de codering van de gegevens tijdens overdracht bespreekt. Deze functie kunt u uw gegevens in een clienttoepassing voordat u deze verzendt via een verbinding met Azure Storage worden geschreven en uw gegevens via programmacode decoderen nadat u hebt opgehaald van Azure Storage via een programma te versleutelen.

Dit biedt versleuteling tijdens overdracht, maar ook vindt u hier de functie van versleuteling in rust. Houd er rekening mee dat hoewel de gegevens tijdens overdracht is versleuteld, nog steeds HTTPS gebruiken aangeraden om te profiteren van de ingebouwde gegevens integriteitscontroles waarmee netwerkfouten dat dit gevolgen heeft de integriteit van gegevens beperken.

Een voorbeeld van waar u kunt dit gebruiken is als u een webtoepassing die BLOB's opgeslagen en opgehaald van BLOB's hebt en u wilt dat de toepassing en de gegevens moeten zo veilig mogelijk. In dat geval zou u aan de clientzijde versleuteling gebruiken. Het verkeer tussen de client en de Azure Blob-Service bevat de versleutelde resource en niemand kunt interpreteren van de gegevens tijdens overdracht en deze opnieuw samenstellen in uw persoonlijke BLOB's.

Aan de clientzijde versleuteling is ingebouwd in de Java en de .NET opslag clientbibliotheken, welke beurtelings gebruik van de Azure-toets kluis API's, zodat u heel gemakkelijk kunt kunt implementeren. Het proces van de gegevens coderen en decoderen de methode envelop, worden opgeslagen en gebruikt metagegevens die worden gebruikt door de versleuteling in elk opslagobject. Bijvoorbeeld voor BLOB's, deze opgeslagen in de metagegevens blob terwijl voor wachtrijen deze toegevoegd aan elk bericht wachtrij.

U kunt genereren en beheren van uw eigen versleutelingssleutels voor de versleuteling zelf. U kunt ook de toetsen die zijn gegenereerd door het bestand Azure opslag-Client gebruiken kunt, of u de Azure-toets kluis genereren van de toetsen. U kunt uw versleutelingssleutels opslaan in de opslag van uw on-premises implementatie sleutels of u ze kunt opslaan in een Azure-toets kluis. Azure-toets kluis, kunt u toegang verlenen tot de geheimen in Azure-toets kluis aan specifieke gebruikers met Azure Active Directory. Dit betekent dat niet alleen iedereen kan lezen van de Azure-toets kluis en ophalen van de toetsen die u voor aan de clientzijde versleuteling gebruikt.

####<a name="resources"></a>Resources

-   [Versleutelen en ontsleutelen BLOB's in Microsoft Azure Storage met Azure-toets kluis](storage-encrypt-decrypt-blobs-key-vault.md)

    In dit artikel leest hoe u aan de clientzijde versleuteling met Azure-toets kluis, waaronder over het maken van de KEK en sla deze op de kluis via PowerShell.

-   [Aan de clientzijde versleuteling en Azure belangrijke kluis voor Microsoft Azure-opslag](storage-client-side-encryption.md)

    In dit artikel geeft een uitleg van aan de clientzijde versleuteling en bevat voorbeelden van het gebruik van de bibliotheek van de client opslag versleutelen en ontsleutelen resources uit de vier storage-services. Het is ook moment spreekt over Azure-toets kluis.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Gebruik van Azure schijfversleuteling versleutelen schijven die door uw virtuele machines gebruikt

Azure schijfversleuteling is een nieuwe functie die zich in het voorbeeld. Deze functie kunt u de OS schijven en gegevensschijven die worden gebruikt door een IaaS virtuele Machine versleutelen. Voor Windows, zijn de stations versleuteld met gestandaardiseerde BitLocker versleutelingstechnologie. De schijven zijn versleuteld met de technologie DM-Crypt voor Linux. Dit is geïntegreerd met Azure-toets kluis waarmee u kunt beheren en beheren van de schijf versleuteling toetsen.

De oplossing Azure schijfversleuteling ondersteunt de volgende drie klant-versleuteling scenario's:

-   Schakel codering op nieuwe IaaS VMs samengesteld op basis van de klant gecodeerde VHD bestanden en klant-versleutelingssleutels, die zijn opgeslagen in Azure-toets kluis.

-   Schakel codering op nieuwe IaaS VMs gemaakt van Azure Marketplace.

-   Schakel codering op bestaande IaaS VMs al worden uitgevoerd in Azure wordt aangegeven.

>[AZURE.NOTE] Voor Linux VMs al worden uitgevoerd in Azure wordt aangegeven of nieuwe Linux VMs gemaakt op basis van afbeeldingen in de Azure Marketplace, is versleuteling van de schijf OS momenteel niet ondersteund. Versleuteling van het OS Volume voor Linux VMs wordt alleen ondersteund voor VMs die zijn versleuteld on-premises implementatie en geüpload naar Azure. Deze beperking geldt alleen voor de schijf OS. codering van gegevensvolumes voor een VM Linux wordt ondersteund.

De oplossing ondersteunt de volgende handelingen uit voor IaaS VMs voor openbare preview-versie wanneer ingeschakeld in Microsoft Azure:

-   Integratie met Azure belangrijke kluis

-   Standaard [A, D en G reeks IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   Versleuteling voor IaaS VMs gemaakt met behulp van [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) model inschakelen

-   Alle Azure openbare [regio 's](https://azure.microsoft.com/regions/)

Deze functie zorgt ervoor dat alle gegevens op de schijf VM worden gecodeerd in rust in Azure opslag.

####<a name="resources"></a>Resources

-   [Azure schijfversleuteling voor Windows en Linux IaaS virtuele Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    In dit artikel wordt beschreven hoe de preview-versie van Azure schijfversleuteling en bevat een koppeling voor het downloaden van het papier wit.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Vergelijking van Azure schijfversleuteling, SSE en aan de clientzijde versleuteling

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs en hun VHD-bestanden

Voor schijven die door IaaS VMs gebruikt, wordt u aangeraden Azure schijf-versleuteling gebruikt. U kunt SSE voor het coderen van de VHD-bestanden die worden gebruikt voor het back-deze schijven in Azure-opslag inschakelen, maar dat alleen nieuwe geschreven gegevens worden gecodeerd. Dit betekent dat als u een VM maken en klik vervolgens SSE inschakelen voor de opslag rekening met het VHD-bestand, kunt u alleen de wijzigingen worden gecodeerd, niet het oorspronkelijke VHD-bestand.

Als u een VM maakt gebruik van een afbeelding van Azure Marketplace, een [recente kopie](https://en.wikipedia.org/wiki/Object_copying) van de afbeelding aan uw account opslagruimte in Azure Storage worden uitgevoerd in Azure en deze niet is versleuteld, zelfs als er SSE ingeschakeld. Nadat de taak wordt gemaakt van de VM en weer verdergaat bijwerken van de afbeelding, start SSE coderen van de gegevens. Om die reden is het aanbevolen versleuteling Azure schijf op VMs gemaakt op basis van afbeeldingen in de Azure Marketplace als u wilt dat ze volledig versleuteld.

Als u een vooraf gecodeerde VM naar Azure vanuit on-premises brengen, is mogelijk aan de toetsen versleuteling uploaden naar Azure-toets kluis en gaat u verder met de codering voor die VM dat u on-premises implementatie zijn gebruikt. Azure schijfversleuteling is u omgaat met dit scenario ingeschakeld.

Als er niet-versleutelde VHD vanuit on-premises, kunt u in de galerie als een aangepaste afbeelding uploaden en een VM hieruit inrichten. Als u dit met de sjablonen resourcemanager doen, kunt u deze om te schakelen Azure schijfversleuteling wanneer deze wordt opgestart de VM vragen.

Wanneer u een gegevensschijf toevoegen en klik op de VM te koppelen, kunt u Azure schijfversleuteling inschakelen op de gegevensschijf. Deze gegevensschijf lokaal eerst zal worden versleuteld en klikt u vervolgens de service management laag doet een fikse schrijven tegen opslag zodat de opslag-inhoud is versleuteld.

####<a name="client-side-encryption"></a>Aan de clientzijde versleuteling####

Aan de clientzijde versleuteling is het meest veilige methode coderen van uw gegevens, omdat er vóór de overdracht versleutelt en de gegevens in rust worden gecodeerd. Dit is echter vereist dat u code toevoegen aan uw toepassingen met opslag, waarmee u niet kunt doen. U kunt in dat geval HTTPs gebruiken voor uw gegevens in de overdracht en SSE coderen van de gegevens van de rest.

Aan de clientzijde versleutelen, kunt u de tabel entiteiten, berichten en BLOB's coderen. U kunt alleen BLOB's coderen met SSE. Als u tabel en wachtrij gegevens moeten worden versleuteld nodig hebt, kunt u aan de clientzijde versleuteling moet gebruiken.

Aan de clientzijde versleuteling wordt volledig beheerd door de toepassing. Dit is het meest veilige methode, maar u hoeft te programmeren wijzigingen aanbrengen in uw toepassing en key management processen wilt plaatsen op hun plaats staan. Hiermee gebruikt u wanneer u de extra beveiliging tijdens overdracht, en u wilt dat uw opgeslagen gegevens moeten worden versleuteld.

Aan de clientzijde versleuteling is meer belasting van de client en moet u rekening hiervoor in uw plannen schaalbaarheid, vooral als u worden gecodeerd en een groot aantal gegevens.

####<a name="storage-service-encryption-sse"></a>Opslag Service versleuteling (SSE)

SSE wordt beheerd door Azure Storage. Gebruik van SSE biedt geen voor de beveiliging van de gegevens tijdens overdracht, maar deze coderen van de gegevens zoals met Azure Storage geschreven. Er is geen invloed op de prestaties bij gebruik van deze functie.

U kunt alleen blokkeren BLOB's versleutelen, BLOB's toevoegen en BLOB's met SSE pagina. Als u versleutelen wachtrijgegevens of gegevens in een tabel wilt, moet u rekening houden aan de clientzijde-versleuteling gebruikt.

Als u een archief of een bibliotheek VHD-bestanden die u als basis gebruikt voor het maken van nieuwe virtuele machines hebt, kunt u een nieuw account voor de opslag maken, SSE inschakelen en vervolgens de VHD-bestanden uploaden naar dat account. Deze VHD-bestanden worden gecodeerd door Azure opslag.

Als u Azure schijfversleuteling is ingeschakeld voor de schijven in een VM en SSE ingeschakeld voor het vasthouden van de bestanden VHD opslag-account hebt, werkt het prima; Deze treden tweemaal worden versleuteld onlangs geschreven gegevens.

##<a name="storage-analytics"></a>Opslag Analytics

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Opslag Analytics gebruiken om de autorisatie type te houden

Voor elk account opslag, kunt u Azure opslag analyses uitvoeren logboekregistratie en opslag van gegevens aan de doelstellingen inschakelen. Dit is een prima hulpmiddel moeten gebruiken wanneer u wilt controleren van de prestatiegegevens van een account opslag, of problemen met een account opslag omdat u prestatieproblemen ondervindt.

Een ander deel van de gegevens die u in de opslagruimte analytics-logboeken zien kunt is de verificatiemethode gebruikt door iemand wanneer ze toegang hebben tot opslag. Bijvoorbeeld, met Blob Storage ziet u als ze een handtekening Access gedeeld of de toetsen opslag-account gebruikt, of als de blob toegankelijk is openbare.

Dit is heel handig zijn als u toegang tot opslag hecht zijn beveiligen. U kunt in-blobopslag bijvoorbeeld stelt u alle van de containers op privé en implementeren van het gebruik van een service SA's overal in uw toepassingen. Vervolgens kunt u de logboeken regelmatig om te zien als uw BLOB's worden geopend met de toetsen opslag account, waarin op een niet nakomen van een waardepapier duiden kunnen, of als de BLOB's openbaar zijn, maar deze niet mag worden controleren.

####<a name="what-do-the-logs-look-like"></a>Hoe zien de logboeken eruit?

Nadat u de account opslageenheden inschakelen en logboekregistratie via de portal van Azure analytics-gegevens snel worden gestart. De logboekregistratie en aan de doelstellingen voor elke service staat los; de logboekregistratie is alleen geschreven wanneer er activiteit in dat account opslag, terwijl de aan de doelstellingen worden geregistreerd elke minuut, per uur of elke dag, afhankelijk van hoe u dit wilt configureren.

De logboeken worden opgeslagen in blok BLOB's in een container met de naam $logs in de opslagruimte-account. Deze container wordt automatisch gemaakt wanneer opslag Analytics is ingeschakeld. Nadat deze container is gemaakt, verwijderen u deze niet, hoewel u de inhoud ervan kunt verwijderen.

Klik onder de container $logs er is een map voor elke service en submappen er voor het jaar/maand/dag/uur te zijn. Klik onder uur, worden de logboeken gewoon genummerd. Dit is wat de mapstructuur eruitziet:

![Weergave van logboekbestanden](./media/storage-security-guide/image1.png)

Elke aanvraag met Azure Storage zijn vastgelegd. Hier ziet u een momentopname van een logboekbestand, met de eerste paar velden.

![Momentopname van een logboekbestand](./media/storage-security-guide/image2.png)

U kunt zien dat u de Logboeken gebruiken kunt voor het bijhouden van elk type oproepen naar een opslag-account.

####<a name="what-are-all-of-those-fields-for"></a>Wat zijn al deze velden voor?

Er is een artikel weergegeven in de onderstaande die bronnen vindt u de lijst met veel velden in de logboeken en wat ze worden gebruikt voor. Hier volgt de lijst met velden in volgorde:

![Momentopname van velden in een logboekbestand](./media/storage-security-guide/image3.png)

We bent geïnteresseerd de items voor GetBlob en hoe ze worden geverifieerd, zodat we nodig om te zoeken naar gegevens met 'Get-Blob' type-bewerking en controleer de aanvraag-status (4<sup>th</sup> kolom) en het autorisatie-type (8<sup>th</sup> kolom).

Bijvoorbeeld, in de eerste paar rijen in de bovenstaande vermelding, de status van de aanvraag is 'Geslaagd' en het autorisatie-type 'geverifieerd'. Dit betekent dat het verzoek is gevalideerd met de toets opslag-account.

####<a name="how-are-my-blobs-being-authenticated"></a>Hoe worden mijn BLOB's worden geverifieerd?

Zijn er drie zaken die we belangstelling hebben.

1.  De blob openbaar is en deze is toegankelijk via een URL zonder handtekening Access gedeeld. In dit geval de status van de aanvraag is 'AnonymousSuccess' en het autorisatie-type 'anonieme' is.

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **anonieme**; mystorage...

2.  De blob privé is en een Access gedeeld handtekening is toegepast. In dit geval de status van de aanvraag is 'SASSuccess' en het autorisatie-type is 'SA's '.

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **SA's**; mystorage...

3.  De blob privé is en de toets opslag is gebruikt voor toegang tot deze. In dit geval de status van de aanvraag is '**geslaagd**' en het autorisatie-type is '**geverifieerd**'.

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Success**; 206; 59; 22; **geverifieerd**; mystorage...

U kunt de bericht-Analyzer van Microsoft te bekijken en te analyseren deze logboeken. Het bevat de mogelijkheden voor het zoeken en filteren. Zoals u mogelijk wilt zoeken naar exemplaren van GetBlob om te controleren of de gebruik oplevert, dat wil zeggen om ervoor te zorgen dat iemand is geen toegang heeft tot uw account opslag ongewenst.

####<a name="resources"></a>Resources

-   [Opslag Analytics](storage-analytics.md)

    In dit artikel is een overzicht van opslag analyses en hoe u deze inschakelt.

-   [Opslagindeling Analytics Log](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    In dit artikel ziet u de opslagruimte Analytics-indeling en een gedetailleerd overzicht van de beschikbare velden daarin, inclusief verificatietype, die welk type verificatie wordt gebruikt voor het verzoek aangeeft.

-   [Monitor met een Account opslag in de portal van Azure](storage-monitor-storage-account.md)

    In dit artikel leest hoe u de controle van de doelstellingen en registratie voor een opslag-account configureren.

-   [End-to-End-problemen oplossen met Azure opslageenheden en logboekregistratie, AzCopy en bericht Analyzer](storage-e2e-troubleshooting.md)

    In dit artikel moment spreekt over het oplossen van door middel van de opslag-analyses en wordt uitgelegd hoe u de Microsoft bericht Analyzer.

-   [Handleiding voor het besturingssysteem van Microsoft Message-Analyzer](https://technet.microsoft.com/library/jj649776.aspx)

    In dit artikel is van de verwijzing voor de Microsoft bericht Analyzer en koppelingen naar een zelfstudie, aan de slag en functie samenvatting bevat.

##<a name="cross-origin-resource-sharing-cors"></a>Cross-Origin Resource delen (CORS)

###<a name="cross-domain-access-of-resources"></a>Beperkte toegang van resources

Wanneer een webbrowser in één domein een HTTP-aanvraag voor een resource uit een ander domein, is dit een cross-origin HTTP-verzoek genoemd. Een HTML-pagina served van contoso.com maakt bijvoorbeeld een verzoek voor een jpeg die worden gehost op fabrikam.blob.core.windows.net. Om beveiligingsredenen beperken browsers cross-origin HTTP-aanvragen is gestart vanaf binnen scripts, zoals JavaScript. Dit betekent dat als sommige JavaScript-code op een webpagina op contoso.com die jpeg op fabrikam.blob.core.windows.net aanvraagt, de browser wordt niet toestaan dat het verzoek.

Wat heeft dit kunt u met Azure Storage doen? Ook als u statische activa, zoals JSON of XML-gegevensbestanden opslaat in Blob Storage Fabrikam via een opslag-account genoemd, het domein dat voor de activa fabrikam.blob.core.windows.net, en worden ze toegang tot de webtoepassing contoso.com niet met JavaScript omdat de domeinen anders zijn. Dit geldt ook zijn als u probeert te bellen een van de Azure Storage-Services, zoals Table Storage – die JSON gegevens moeten worden verwerkt door de JavaScript-client retourneren.

####<a name="possible-solutions"></a>Mogelijke oplossingen

Een manier om op te lossen dit is een aangepast domein zoals "storage.contoso.com" toewijzen aan fabrikam.blob.core.windows.net. Het probleem is dat u alleen dat aangepaste domein aan één opslag-account toewijzen kunt. Wat gebeurt er als de activa worden opgeslagen in meerdere opslag-accounts?

Er is een andere manier om dit probleem oplossen dat de webtoepassing als proxy voor de opslag-oproepen. Dit betekent dat als u bij het uploaden van een bestand met Blob Storage, de webtoepassing zou lokaal schrijven en vervolgens kopiëren naar Blob Storage of dit zou alles in het geheugen gelezen en deze vervolgens naar Blob Storage te schrijven. U kunt ook een speciale webtoepassing (zoals een Web-API) die de bestanden lokaal uploads en schrijft deze naar Blob Storage schrijven. In beide gevallen moet u rekening voor die functie bij het bepalen van de schaalbaarheid nodig heeft.

####<a name="how-can-cors-help"></a>Hoe kunt CORS?

Azure opslag kunt u om in te schakelen CORS – Cross Origin resources delen. Voor elk account opslag, kunt u domeinen die toegang de bronnen in dat account opslag tot. We kunnen in ons geval die hierboven beschreven, bijvoorbeeld CORS inschakelen op het account van de opslagruimte fabrikam.blob.core.windows.net en configureert voor toegang tot contoso.com. Vervolgens de web-toepassing contoso.com rechtstreeks toegang tot de informatiebronnen in fabrikam.blob.core.windows.net.

Hierbij moet u onthouden is dat CORS access toestaat, maar biedt geen verificatie vereist voor alle niet-openbare access, met opslag resources is. Dit betekent dat u kunt alleen BLOB's openen als ze openbare zijn of u een Access gedeeld handtekening zodat u de juiste machtiging toevoegen. Tabellen, wachtrijen en bestanden geen openbare toegang hebt en een SA's vereisen.

CORS is standaard uitgeschakeld op alle services. U kunt CORS inschakelen met behulp van de REST API of in de bibliotheek van de client opslag bellen een van de methoden voor het instellen van de service-beleid. Wanneer u dat doet, kunt u ook een regel CORS, dat wil in XML-bestand zeggen. Hier volgt een voorbeeld van een regel voor CORS die is ingesteld met de bewerking Service-eigenschappen instellen voor de Service Blob voor een opslag-account. U kunt met behulp van de opslagruimte client-bibliotheek of de REST API's voor de opslag van Azure bewerking uitvoeren.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Dit is de betekenis van elke rij:

-   **AllowedOrigins** Hiermee wordt aangegeven welke domeinen niet-overeenkomende kunnen aanvragen en ontvangen van de storage-service. Dit geeft aan dat contoso.com en fabrikam.com gegevens van Blob Storage voor een specifieke opslag-account opvragen kunnen. U kunt dit ook instellen op een jokerteken (\*) toe te staan dat alle domeinen toegangsaanvragen.

-   **AllowedMethods** Dit is de lijst met methoden (HTTP-verzoek woorden) die kunnen worden gebruikt bij het maken van het verzoek. In dit voorbeeld zijn alleen opslag en GET toegestaan. U kunt dit instellen op een jokerteken (\*) toe te staan dat alle methoden moet worden gebruikt.

-   **AllowedHeaders** Dit is de aanvraag kopteksten waarvan het oorspronkelijke domein bij het maken van de aanvraag kunt opgeven. In dit voorbeeld alle metagegevens kopteksten beginnen met x-ms-metagegevens, x-ms-metagegevens-doel, en x-ms-metagegevens-abc zijn toegestaan. Het jokerteken (\*) geeft aan dat een koptekst die begint met het opgegeven voorvoegsel is toegestaan.

-   **ExposedHeaders** Hiermee wordt uitgelegd welke kopteksten antwoord door de browser naar de uitgever van de aanvraag moeten worden weergegeven. In dit voorbeeld een kop die beginnen met ' x-ms - metagegevens-"wordt geëvalueerd.

-   **MaxAgeInSeconds** Dit is de maximale hoeveelheid tijd waarop een browser de Preflight-opties-aanvraag wordt opgeslagen. (Voor meer informatie over het Preflight-verzoek, raadpleegt u het eerste artikel onderstaande.)

####<a name="resources"></a>Resources

Check deze bronnen voor meer informatie over CORS en hoe u deze inschakelen.

-   [Cross-Origin Resource (CORS)-ondersteuning voor de van Azure-opslagservices op Azure.com delen](storage-cors-support.md)

    Dit artikel bevat een overzicht van CORS en het instellen van de regels voor de verschillende storage-services.

-   [Cross-Origin Resource (CORS)-ondersteuning voor de Services Azure opslagruimte op MSDN delen](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Dit is de documentatie bij CORS ondersteuning voor de opslag Azure Services. Dit bevat koppelingen naar artikelen toepassen op elke storage-service, en ziet u een voorbeeld en wordt uitgelegd van elk element in het bestand CORS.

-   [Microsoft Azure-opslag: Inleiding tot CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Dit is een koppeling naar artikel in het eerste blog CORS aankondigen en laat zien hoe u deze gebruiken.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Veelgestelde vragen over de opslag van Azure-beveiliging

1.  **Hoe kan ik controleren of de integriteit van de BLOB's die ik in- of uitzoomen op Azure Storage brengen ben als ik het HTTPS-protocol niet gebruiken?**

    Als om een reden moet u HTTP gebruiken in plaats van HTTPS en u werkt met blok BLOB's, kunt u MD5 controleren om te controleren of de integriteit van de BLOB's die worden overgebracht. Zo kunt met bescherming tegen netwerk/transport laag fouten, maar niet noodzakelijkerwijs met tussenliggende aanvallen.

    Als u HTTPS, waarmee transportbeveiliging, en het gebruik van MD5 controleren overtollige en overbodige is gebruiken kunt.
    
    Check de [Azure Blob MD5 overzicht](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx)voor meer informatie.

2.  **Hoe zit het met FIPS-naleving voor de Amerikaanse overheid?**

    De Verenigde Staten federale informatie Processing Standard (FIPS) definieert cryptografische algoritmen goedgekeurd voor gebruik door federale Amerikaanse overheid computersystemen voor het beveiligen van vertrouwelijke gegevens. Inschakelen van FIPS modus op een Windows-server of bureaublad Hiermee wordt aan het besturingssysteem dat alleen FIPS gevalideerde cryptografische algoritmen moeten worden gebruikt. Als een toepassing niet-compatibele algoritmen gebruikt, wordt de toepassingen worden afgebroken. With.NET Framework versies 4.5.2 of hoger, de toepassing automatisch de cryptografische algoritmen wilt FIPS-compatibele algoritmen gebruiken wanneer de computer bevindt zich in FIPS-modus activeren.

    Microsoft focus naar elke klant te bepalen of FIPS-modus inschakelen. We denkt dat er geen dwingende redenen voor klanten die niet zijn onderhevig aan de wettelijke voorschriften FIPS-modus standaard inschakelen.

    **Resources**

-   [Waarom We bent niet aanbevelen 'FIPS-modus' meer](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    In dit artikel blog biedt een overzicht van FIPS en wordt uitgelegd waarom ze FIPS-modus standaard niet ingeschakeld.

-   [FIPS 140 gegevensvalidatie](https://technet.microsoft.com/library/cc750357.aspx)

    In dit artikel wordt aandacht besteed aan hoe Microsoft-producten en cryptografische modules aan de standaard FIPS voor de federale Amerikaanse overheid voldoet.

-   ["Systeem cryptografische: gebruik FIPS compatibele algoritmen voor codering, hashing en ondertekening" beveiligingsrisico van instellingen in Windows XP en in nieuwere versies van Windows](https://support.microsoft.com/kb/811833)

    In dit artikel moment spreekt over het gebruik van FIPS-modus in oudere Windows-computers.
