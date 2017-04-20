<properties
   pageTitle="Aanbevolen procedures voor Software-Updates op Microsoft Azure IaaS | Microsoft Azure"
   description="Artikel vindt u een verzameling aanbevolen procedures voor software-updates in een Microsoft Azure IaaS-omgeving.  Dit is bedoeld voor IT-professionals en beveiliging analisten dat behandelt wijzigen besturingselement, software-update en activa management dagelijks, waaronder mensen die verantwoordelijk is voor de beveiliging en naleving inspanningen van hun organisatie."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Aanbevolen procedures voor software-updates op Microsoft Azure IaaS

Voordat u elk type discussie verdiepen op aanbevolen procedures voor een Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) -omgeving, is het belangrijk om te begrijpen wat zijn de scenario's die u beheren software-updates en de verantwoordelijkheden heeft. Het onderstaande diagram kunt u meer informatie over deze grenzen:

![Cloud-modellen en verantwoordelijkheden](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

De meest linkse kolom ziet u zeven verantwoordelijkheden (gedefinieerd in de volgende gedeelten) dat organisaties rekening moet houden, die bijdragen aan de veiligheid en privacy van een IT-omgeving.
 
Classificatie van gegevens en verantwoordelijkheid en Client & fungeert beveiliging zijn de verantwoordelijkheden die uitsluitend in het domein van klanten en fysieke, Host en netwerk verantwoordelijkheden zijn in het domein met cloud-providers in de modellen voor PaaS en SaaS. 

De resterende verantwoordelijkheden tussen klanten worden gedeeld en cloud serviceproviders. Sommige verantwoordelijkheden is vereist voor de CSP en de klant te beheren de verantwoordelijkheid samen, inclusief controle van hun domeinen. Bijvoorbeeld, houd rekening met identiteit & toegang tot management bij gebruik van de Azure Active Directory Services; de configuratie van services zoals meervoudige verificatie is snel aan de klant, maar de verantwoordelijkheid van Microsoft Azure ervoor zorgen dat effectieve functionaliteit is.

> [AZURE.NOTE] Lees voor meer informatie over gedeelde verantwoordelijkheden in de cloud, [Gedeelde verantwoordelijkheden ter Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Deze dezelfde beginselen toepassen in een hybride scenario waar uw bedrijf van Azure IaaS VMs die communiceren met on-premises implementatie resources gebruikmaakt zoals wordt weergegeven in het onderstaande diagram.

![Normale hybride scenario met Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Eerste beoordeling

Zelfs als uw bedrijf al gebruik maakt van een update management-systeem en u al beleid voor software-update op hun plaats staan, is het belangrijk om te vaak terugkeren naar de vorige beleid beoordelingen en bijgewerkt om ze op basis van uw huidige vereisten. Dit betekent dat u moet bekend zijn met de huidige status van de resources in uw bedrijf. Als u wilt deze status hebt bereikt, die u moet weten:

-   De fysieke en virtuele computers in uw onderneming.

-   Besturingssystemen en versies van elk van deze fysieke en virtuele computers.

-   Software-updates is geïnstalleerd op elke computer (servicepackversies, software-updates en andere wijzigingen).

-   De functie elke computer worden uitgevoerd in uw onderneming.

-   De toepassingen en -programma's die op elke computer worden uitgevoerd.

-   Eigendom- en contactgegevens informatie voor elke computer.

-   De activa aanwezig zijn in uw omgeving en de relatieve waarde om te bepalen welke gebieden moeten optimaal aandacht en beveiliging.

-   Van bekende beveiligingsproblemen en de processen heeft uw onderneming op hun plaats staan kenmerk nieuwe beveiligingskwesties of wijzigingen in beveiligingsniveau.

-   Countermeasures die zijn geïmplementeerd als u wilt beveiligen van uw omgeving.

Moet u deze gegevens regelmatig bijwerken moet, en beschikken die nodig zijn voor het in het hulpprogramma voor het beheer van uw software-updateproces.

## <a name="establish-a-baseline"></a>Een basislijn instellen

Het maken van een belangrijk onderdeel van het hulpprogramma voor het beheer van de software-updateproces is eerste standaard installaties van versies van besturingssystemen, toepassingen en hardware voor computers in uw onderneming; Deze worden basislijnen genoemd. Een basislijn is de configuratie van een product of de regeling die is ingesteld op een specifiek punt in tijd. Een toepassing of het besturingssysteem volgens de basislijn, vindt u bijvoorbeeld de mogelijkheid om een computer of service naar een specifieke status opnieuw te maken.

Basislijnen vormen de basis voor het zoeken en mogelijke problemen oplossen en de software management updateproces vereenvoudigen door het aantal software-updates die u in uw onderneming implementeren moet te verminderen zowel doordat de mogelijkheid om te controleren of.

Na het uitvoeren van de eerste controle van uw onderneming, moet u de informatie die wordt opgehaald uit de controle een operationele basislijn voor de IT-onderdelen binnen uw productieomgeving definiëren. Het is mogelijk dat een getal van basislijnen vereist, afhankelijk van de verschillende soorten hardware en software die zijn geïmplementeerd in bedrijf.

Sommige servers vereisen bijvoorbeeld een software-update om te voorkomen dat ze verkeerd-om wanneer ze het afsluiten invoeren waarop Windows Server 2012. Een basislijn voor deze servers moet deze software-update opnemen.

In grote organisaties is het meestal handig dat u op de computers in uw onderneming verdeeld over activacategorieën en bewaren van elke categorie aan een standaard basislijn met behulp van dezelfde versies van software en software-updates. Vervolgens kunt u deze activacategorieën bij het rangschikken van een verdeling software-update.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Abonneren op de juiste software-update meldingsservices

Nadat u een eerste controle van de software wordt gebruikt in uw onderneming uitvoert, moet u de beste methode voor het ontvangen van meldingen van nieuwe software-updates voor elke SOFTWAREPRODUCT en versie bepalen. Afhankelijk van het SOFTWAREPRODUCT mogelijk de beste migratiemethode voor melding e-mailmeldingen, websites of publicaties van de computer.

Bijvoorbeeld, de MSRC Microsoft Security antwoord Center () moet reageren op alle-beveiliging bezwaren over Microsoft-producten en vindt u de Microsoft Security Bulletin Service, een gratis e-mailmelding van nieuwe geïdentificeerd zwakke software-updates die zijn vrijgegeven om deze problemen op te lossen. U kunt u abonneren op deze service naar http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Overwegingen voor software-update

Nadat u een eerste controle van de software wordt gebruikt in uw onderneming uitvoert, moet u de vereisten voor het instellen van uw software-update management system, dat is afhankelijk van de software-update management systeem dat u gebruikt bepalen. Lees voor WSUS [Aanbevolen procedures met Windows Server Update Services](https://technet.microsoft.com/library/Cc708536)lezen, systeem-beheercentrum voor [Planning op Software-Updates in Configuration Manager](https://technet.microsoft.com/library/gg712696).

Er zijn echter enkele aandachtspunten voor de algemene en aanbevolen procedures die u, ongeacht de oplossing dat u gebruikt toepassen kunt zoals in de secties die volgt op.

### <a name="setting-up-the-environment"></a>Bij het instellen van de omgeving

Houd rekening met de volgende procedures bij het plannen voor het instellen van de software bijwerken management-omgeving:

-   **Siteverzamelingen maken productie software-update op basis van stabiele criteria**: In het algemeen, stabiele criteria gebruiken om te maken van siteverzamelingen voor uw software-update inventarisatie- en distributiegroepen helpt te vereenvoudigen alle stadia van het hulpprogramma voor het beheer van de software-updateproces. De stabiele criteria kunnen opnemen het besturingssysteem van geïnstalleerde clientversie en servicepack, Systeemrol of doelorganisatie.

-   **Maken preproductie verzamelingen die verwijzing computers bevatten**: de collectie preproductie bevat vertegenwoordiger configuraties van de besturingssystemen, regel van zakelijke software en andere software die in uw onderneming worden uitgevoerd.

U moet ook rekening houden waar de software-update-server zich bevindt, als deze weergegeven in de infrastructuur Azure IaaS in de cloud worden of als deze on-premises implementatie worden. Dit is een belangrijke beslissing omdat u moet de hoeveelheid verkeer tussen bronnen voor on-premises implementatie en Azure-infrastructuur evalueren. [Verbinding maken met een on-premises netwerk met een Microsoft Azure virtual netwerk](https://technet.microsoft.com/library/Dn786406.aspx) voor meer informatie over het koppelen van de infrastructuur van uw on-premises implementatie aan Azure begrijpen.

De ontwerpopties waarmee u waar de update-server bepalen kunnen, bevinden zich ook zijn afhankelijk van de infrastructuur van uw huidige en het systeem voor het bijwerken van software die u momenteel gebruikt. Lees voor WSUS [Windows Server Update Services implementeren in uw organisatie](https://technet.microsoft.com/library/hh852340.aspx) en voor System Center Configuration Manager Lees [Planning voor Sites en hiërarchieën in Configuration Manager](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Back-up maken

Regelmatige back-ups zijn belangrijk niet alleen voor de software-update management platform zelf maar ook voor de servers die wordt bijgewerkt. Organisaties met een [proces wijzigen](https://technet.microsoft.com/library/cc543216.aspx) op hun plaats staan moeten worden IT naar redenen waarom de server moet worden bijgewerkt, de geschatte downtime en mogelijke impact uitvullen. Om ervoor te zorgen dat er een terugdraaien configuratie op hun plaats staan geval een update is mislukt, zorg ervoor dat u regelmatig een back-up van het systeem.

Sommige back-opties voor Azure IaaS zijn onder andere:

-   [Azure IaaS werkbelasting beveiliging met Data Protection Manager](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Back-up van Azure virtuele machines](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Cmdlets voor controle

U moet normale rapporten uitvoeren om te controleren van het aantal ontbreken of updates en met de status van onvolledige, voor elke softwareupdate die gemachtigd is geïnstalleerd. Voor rapportage voor software-updates die nog niet zijn geautoriseerd kunt op dezelfde manier eenvoudiger implementatie beslissingen vergemakkelijken.

U moet ook rekening houden met de volgende taken uitvoeren:

-   Een controle van toepassing en geïnstalleerde beveiligingsupdates voor alle computers in uw bedrijf leiden.

-   Autoriseer en implementeren van de updates naar de juiste computers.

-   Bijhouden van de voorraad en installatiestatus en voortgang voor alle computers in uw bedrijf bijwerken.

Daarnaast tot algemene overwegingen die zijn beschreven in dit artikel, kunt u ook overwegen elk product bevindt zich aanbevolen oefening, bijvoorbeeld: als u een VM in Azure wordt aangegeven met SQL Server hebt, controleert u of dat u de aanbeveling software-updates voor dat product volgt.

## <a name="next-steps"></a>Volgende stappen

Gebruik de richtlijnen in dit artikel beschreven om u te helpen bij het bepalen van de beste opties voor software-updates voor virtuele machines in Azure IaaS. Er zijn veel overeenkomsten tussen aanbevolen procedures voor software-update in een traditioneel datacenter versus Azure IaaS, dus het wordt aanbevolen dat u het beleid voor uw huidige software-update als u wilt opnemen Azure VMs en de relevante aanbevolen procedures in dit artikel opnemen in uw algemene software-updateproces evalueren.
