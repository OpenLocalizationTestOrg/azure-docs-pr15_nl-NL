<properties
    pageTitle="Meld u aan analyses gegevensbeveiliging | Microsoft Azure"
    description="Meer informatie over hoe Log Analytics uw privacy beschermen en worden uw gegevens."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Meld u aan beveiliging van Analytics-gegevens

Microsoft is erop gebrand om uw privacy te beschermen en beveiligen van uw gegevens, tijdens het uitvoeren van software en services die u helpen de IT-infrastructuur van uw organisatie beheren. We herkennen dat wanneer u uw gegevens aan anderen belasten, vertrouwensrelatie strikte beveiliging vereist. Microsoft volgt strikte richtlijnen voor naleving en beveiliging, uit codering aan een dienst.

Is de hoogste prioriteit bij Microsoft beveiligen en beschermen van gegevens. Neem contact met ons opnemen met vragen, suggesties of problemen over een van de volgende informatie, inclusief onze beveiligingsbeleid voor apparaten op [Azure ondersteuningsopties](http://azure.microsoft.com/support/options/).

In dit artikel wordt uitgelegd hoe gegevens is die worden verzameld, verwerkt en beveiligd door Log Analytics in de bewerkingen Management Suite Kantoorbeheersysteem. U kunt agenten verbinding maken met de webservice, System Center Operations Manager gebruiken voor het verzamelen van operationele gegevens of gegevens ophalen van Azure diagnostische hulpprogramma's voor gebruik door Log Analytics. De verzamelde gegevens is verzonden via Internet met behulp van certificaatverificatie SSL 3 naar de Log Analytics-service, die wordt gehost in Microsoft Azure. Gegevens worden gecomprimeerd door de agent voordat deze wordt verzonden.

De Log Analytics-service beheert uw cloudgebaseerde gegevens veilig met behulp van de volgende methoden:

- gegevens scheiding
- Gegevensretentie
- fysieke beveiliging
- Oproepbeheer
- naleving
- beveiliging standaarden certificaten


## <a name="data-segregation"></a>Gegevens scheiding

Gegevens van de klant is geïsoleerd logisch op elk onderdeel overal in de OMS-service. Alle gegevens is gelabeld per organisatie. Deze labelen zich blijft voordoen gedurende de levenscyclus van de gegevens en deze op elke laag die u van de service wordt afgedwongen. Elke klant heeft een speciale Azure blob die de lange termijn gegevens bevat

## <a name="data-retention"></a>Gegevensretentie

Geïndexeerde logboekgegevens zoeken is opgeslagen en behouden op basis van uw abonnement prijzen. Zie [Log Analytics prijzen](https://azure.microsoft.com/pricing/details/log-analytics/)voor meer informatie.

Microsoft verwijdert klantgegevens 30 dagen na de werkruimte OMS is gesloten. Microsoft ook verwijderd van de Azure opslag Account waarin de gegevens zich bevinden. Wanneer u gegevens van de klant wordt verwijderd, wordt geen fysieke stations worden verwijderd.

De volgende tabel worden enkele van de beschikbare oplossingen in OMS en voorbeelden de typen gegevens ze verzamelen.

| **Oplossing** | **Gegevenstypen** |
| --- | --- |
| Configuratie bepalen | Configuratiegegevens, metagegevens en staat gegevens |
| Capaciteit plannen | Prestatiegegevens en metagegevens |
| Antimalware | Van configuratiegegevens en metagegevens |
| Systeem Update Assessment | Metagegevens en de statusgegevens |
| Log-beheer | De gebruiker gedefinieerde gebeurtenislogboeken, gebeurtenislogboeken van Windows en/of IIS-logboeken |
| Het bijhouden van wijzigingen | Software-inventarisatie en metagegevens van de Windows-Service |
| SQL- en Active Directory Assessment | WMI-gegevens, register prestatiegegevens en beheer van SQL Server-dynamische resultaten bekijken |

De volgende tabel ziet u voorbeelden van gegevens:

| **Gegevenstype** | **Velden** |
| --- | --- |
| Waarschuwen | Waarschuw naam, beschrijving van waarschuwing, BaseManagedEntityId, probleem-ID, IsMonitorAlert, RuleId, ResolutionState, prioriteit, ernst, categorie, eigenaar, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, aantalherhalingen |
| Configuratie | Klant-id, AgentID, id van de entiteit, ManagedTypeID, ManagedTypePropertyID, HuidigeWaarde, ChangeDate |
| Gebeurtenis | EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, getal, categorie, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Notitie:** Wanneer u gebeurtenissen met aangepaste velden naar het gebeurtenissenlogboek van Windows registreert, worden ze OMS verzameld. |
| Metagegevens | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, naam netwerk, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, weergavenaam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Prestaties | Objectnaam, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| De staat | StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fysieke beveiliging

De Analytics Log in OMS-service is bemand door personeel van Microsoft en alle activiteiten worden vastgelegd en kunnen worden gecontroleerd. De service volledig wordt uitgevoerd in Azure wordt aangegeven en voldoet aan de Azure algemene technische criteria. U kunt meer informatie over de fysieke beveiliging van Azure activa weergeven op pagina 18 van het [Overzicht van Microsoft Azure-beveiliging](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fysiek toegangsrechten beveiligen gebieden worden gewijzigd binnen één werkdag voor iedereen die niet langer verantwoordelijk voor de OMS-service is, zoals bestandsoverdracht en beë. U kunt lezen over de globale fysieke infrastructuur die we bij [Microsoft-Datacenters gebruiken](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Oproepbeheer

OMS heeft een oproepbeheer-proces die voor alle Microsoft-services worden aangehouden. Om samen te vatten, we:

- Gebruik een gedeelde verantwoordelijkheid model waarin een gedeelte van beveiliging verantwoordelijkheid Microsoft behoort terwijl een gedeelte bij de klant hoort
- Azure-incidenten beheren
  - Een incident op de eerste vermelding, start u een onderzoek detecteren
  - Beoordeel de impact en ernst van een incident door een teamlid incident antwoord op bellen. Op basis van bewijs, de beoordeling mogelijk of niet kan leiden tot verdere escalatie naar het antwoord security team.
  - Een diagnose stellen bij een incident door experts van de beveiliging antwoord om te leiden van de technische of forensisch onderzoek, insluiting, risicobeperking en tijdelijke oplossing strategieën identificeren. Als het beveiligingsteam is van mening dat klantgegevens kan hebben worden blootgesteld aan een onrechtmatige of onbevoegde persoon, begint het parallelle uitvoering van het proces klant Incident melding parallel.  
  - Stabiliseren en herstellen uit het incident. Het team incident antwoord Hiermee maakt u een abonnement herstel te verminderen van het probleem. Crisis insluiting stappen zoals risico systemen in quarantaine plaatsen treedt op onmiddellijk en parallel met diagnose. Mogelijk langer term beperkingen worden gepland die zich voordoen nadat het direct risico is verstreken.  
  - Sluit het incident en een postmortemkeuring leiden. Het team incident antwoord Hiermee maakt u een postmortemkeuring omtrek van de details van het incident, met de bedoeling herzien beleidsregels, processen en processen om te voorkomen dat een herhaling van de gebeurtenis.
- Gebruikers informeren over-incidenten
  - Het bereik van risico klanten en op te geven iedereen die wordt beïnvloed als een kennisgeving mogelijk gedetailleerde bepalen
  - Maak een kennisgeving voor klanten met gedetailleerde weinig gegevens zodat deze kunnen uitvoeren van een onderzoek op hun einde en voldoen aan eventuele verplichtingen die ze in hun eindgebruikers hebt aangebracht terwijl de meldingen niet ten onrechte vertragen.
  - Bevestig en declareren het incident, indien nodig.
  - Een melding weergegeven klanten met een incident melding zonder rond vertraging en volgens een wettelijke of contractuele resourcebeperkingen. Melding van beveiligingsincidenten worden afgeleverd aan een of meer van de klant beheerders door middel van die Microsoft selecteert, waaronder via e-mail.
- Leiden team gereedheid en training
  - Personeel van Microsoft zijn vereist voor het voltooien van beveiliging en chatstatus training, waarmee ze herkennen en mogelijke beveiligingskwesties rapporteren.  
  - Operatoren werken op de Microsoft Azure-service hebt toevoeging training verplichtingen omringende hun toegang tot gevoelige systemen klantgegevens hostingprovider.
  - Personeel van Microsoft security antwoord ontvangen gespecialiseerde training voor hun rollen


In geval van verlies van gegevens van een klant kennis we elke klant binnen één dag. Klant-gegevensverlies opgetreden echter nooit met OMS. Daarnaast kunnen we kopieën van de gegevens die zijn gemaakt en wordt deze geografisch verdeeld.

Zie voor meer informatie over hoe Microsoft moet op beveiligingsincidenten reageren, [Microsoft Azure beveiliging antwoord in de Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Naleving

OMS software development en -service van het team informatie beveiligings- en governance programma ondersteunt de zakelijke vereisten en voldoet aan de wetgeving, zoals beschreven op [Microsoft Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/) en [Vertrouwen Center naleving van Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Hoe OMS tot stand brengt beveiligingsvereisten die zijn, geeft aan wat besturingselementen voor beveiliging, beheert en bewaakt de risico's, worden er ook beschreven. Jaarlijks we een evaluatie uitvoeren van beleid, standaarden, procedures en richtlijnen.

Elk lid van het projectteam OMS ontwikkeling ontvangt formele toepassing beveiligingstraining. Intern, gebruiken we een versiebeheer voor software development. Elk project software is door het systeem voor versiebeheer beveiligd.

Microsoft heeft een beveiliging en naleving team die verantwoordelijk is en alle services in Microsoft beoordeelt. IT-specialisten voor beveiliging maken van het team en zijn niet gekoppeld aan de technische afdelingen die OMS ontwikkelen. De beveiliging managers hun eigen Managementketen hebt en onafhankelijke beoordelingen van producten en services om ervoor te zorgen beveiliging en naleving leiden.

Microsoft Raad van Bestuur per is gesteld en jaarrapport over alle de informatiebeveiliging bij Microsoft-programma's.

Het team OMS software development en -service werkt actief samen met de teams Microsoft Legal and naleving en andere IT-partners aan te schaffen tal van certificaten.

## <a name="security-standards-certifications"></a>Beveiliging standaarden certificaten

Meld u aan analyses OMS momenteel voldoen aan de volgende normen voor beveiliging:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) en [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) compatibele
- Betaling kaart industrie (PCI compatibele) gegevensstandaard (PCI DSS) door de Raad PCI beveiliging standaarden.
- [Service organisatie besturingselementen (SOC) 1 Type 1 en SOC 2 Type 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) compatibele
- Windows gemeenschappelijke technische Criteria
- Microsoft betrouwbare Computing-certificering
- Als een Azure-service, wordt de OMS onderdelen voldoen aan Azure nalevingsvereisten. U vindt meer bij [Microsoft Trust Center naleving](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Cloud computing gegevensstroom-beveiliging
In het volgende diagram ziet u de beveiligingsarchitectuur van een wolk als de stroom van gegevens uit uw bedrijf en hoe deze is beveiligd, zoals is verplaatst naar de Log Analytics-service, uiteindelijk zichtbaar voor u in de portal OMS. Meer informatie over elke stap volgt het diagram.

![Afbeelding van de gegevensverzameling OMS en beveiliging](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. zich registreren voor Log analyses en gegevens verzamelen

Voor uw organisatie gegevens te verzenden naar Log analyses, moet u Windows agenten, agenten Azure virtuele machines of OMS agenten waarop voor Linux configureren. Als u Operations Manager agenten gebruikt, klikt u gebruikt een configuratiewizard in de bewerkingen-console ze te configureren. Gebruikers (die mogelijk zijn u, andere individuele gebruikers of een groep personen) maakt u een of meer OMS-accounts (OMS werkruimten), en kunt vinden met behulp van een van de volgende accounts hebt geregistreerd:


- [Organisatie-ID](../active-directory/sign-up-organization.md)

- [Microsoft-Account - Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Een werkruimte OMS is waarbij gegevens die worden verzameld, samengevoegd, geanalyseerd en gepresenteerd. Een werkruimte hoofdzakelijk wordt gebruikt om te partitiegegevens en elke werkruimte uniek is. U wilt bijvoorbeeld de productiegegevens beheerd met één OMS workspace en uw testgegevens beheerd met een andere werkruimte hebben. Een beheerder gebruikerstoegang helpen werkruimten ook met de gegevens. Elke werkruimte kan bevatten meerdere gebruikersaccounts die zijn gekoppeld, en elke gebruikersaccount hebben toegang tot meerdere OMS-werkruimte. U maken werkruimten op basis van het datacenter regio. Elke werkruimte worden gerepliceerd naar andere datacenters in de regio, hoofdzakelijk voor beschikbare OMS-service.

Wanneer de configuratiewizard is voltooid, maakt elke groep Operations Manager-beheer voor Operations Manager, verbinding met de Log Analytics-service. U de Wizard Computers toevoegen gebruiken om te kiezen welke computers in de groep management gegevens te verzenden naar de service zijn toegestaan. Voor andere typen agent verbindt elk veilig met de OMS-service.

Alle communicatie tussen verbonden systemen en de Log Analytics-service is versleuteld.  Het protocol TLS (HTTPS) wordt gebruikt voor versleuteling.  Het Microsoft SDL-proces wordt gevolgd om ervoor te zorgen dat log Analytics is bijgewerkt met de meest recente vorderingen in cryptografische protocollen.

Elk type agent verzamelt gegevens van Log analysegegevens. Het type gegevens die worden verzameld, is afhankelijk van de soorten oplossingen die worden gebruikt. Hier ziet u een overzicht van gegevensverzameling bij [solutions Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md). Meer gedetailleerde informatie van de siteverzameling is ook beschikbaar voor de meeste oplossingen. Een oplossing is een bundel van vooraf gedefinieerde weergaven, log zoekquery's, regels voor het verzamelen van gegevens en verwerking logica. Alleen beheerders kunnen Log Analytics gebruiken om te importeren van een oplossing. Nadat de oplossing is geïmporteerd, wordt verplaatst naar de servers voor het beheer Operations Manager (indien van toepassing), en vervolgens naar een agenten die u hebt gekozen. Daarna verzamelen de agenten de gegevens.

## <a name="2-send-data-from-agents"></a>2. gegevens uit agenten verzenden

U kunt alle agent typen met een sleutel registratie registreren en een beveiligde verbinding tot stand is gebracht tussen de agent en de certificaten gebaseerde verificatie en SSL gebruiken met poort 443 Log Analytics-service. OMS wordt een geheime store om genereren en onderhouden van toetsen. Persoonlijke sleutels zijn gedraaid zijn, elke 90 dagen en worden opgeslagen in Azure wordt aangegeven en worden beheerd door de Azure bewerkingen die strikte regelgeving en naleving procedures volgen.

Met Operations Manager, u een werkruimte met de Log Analytics-service registreren en een beveiligde HTTPS-verbinding tot stand is gebracht tussen de server voor Operations Manager.

Een alleen-lezen-opslag-sleutel wordt gebruikt voor agenten van Windows Azure virtuele machines waarop, om te lezen diagnostische gebeurtenissen in Azure tabellen.

Als een agent niet mogelijk om te communiceren met de service voor welke reden dan ook is, is de verzamelde gegevens in een tijdelijke cache lokaal opgeslagen en de server management probeert opnieuw verzenden van de gegevens elke 8 minuten voor 2 uur. In de cache opgeslagen gegevens van de agent is door het archief met referenties van het besturingssysteem beveiligd. Als de service niet de gegevens na 2 uur verwerken, worden de gegevens in de agenten wachtrij. Als de wachtrij vol raakt, begint OMS gegevenstypen, beginnend met prestatiegegevens weghalen. De limiet van de wachtrij agent is een registersleutel zodat u wijzigen kunt, indien nodig. Verzamelde gegevens worden gecomprimeerd en verzonden naar de service, overslaan on-premises implementatie-databases, zodat deze geen belasting aan deze toegevoegd. Als de verzamelde gegevens is verstuurd, is deze verwijderd uit de cache.

Zoals hierboven is beschreven, wordt via SSL gegevens uit uw agenten verzonden met Microsoft Azure-datacenters. (Optioneel) kunt u ExpressRoute extra beveiliging bieden voor de gegevens. ExpressRoute is een manier om rechtstreeks verbinding maken met Azure van uw bestaande WAN-netwerk, zoals een met meerdere protocollen label overstappen op een andere (MPLS) VPN, verstrekt door een netwerkprovider. Zie [ExpressRoute](https://azure.microsoft.com/services/expressroute/)voor meer informatie.


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. de Log Analytics-service ontvangt en verwerkt van gegevens

De Log Analytics-service zorgt ervoor dat binnenkomende gegevens van een betrouwbare bron door het valideren van certificaten en de integriteit van gegevens met Azure-verificatie. De onverwerkte onbewerkte gegevens vervolgens als een blob in [Microsoft Azure Storage](../storage/storage-introduction.md) is opgeslagen en niet is versleuteld. Elke opslag Azure blob heeft echter een reeks unieke set toetsen die toegankelijk alleen voor die gebruiker is. Het type gegevens dat is opgeslagen, is afhankelijk van de soorten oplossingen die zijn geïmporteerd en voor het verzamelen van gegevens. De Log Analytics-service verwerkt vervolgens de onbewerkte gegevens voor de opslag van Azure-blog.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Log Analytics gebruiken voor toegang tot de gegevens

U kunt zich aanmeldt bij Log Analytics in de portal OMS met behulp van de organisatie-account of een Microsoft-account dat u eerder hebt ingesteld. Alle verkeer tussen de OMS-portal en Log Analytics in OMS is verzonden via een beveiligde HTTPS-kanaal. Wanneer u met de portal OMS, een sessie-ID wordt gegenereerd op de gebruiker-client (webbrowser) en gegevens worden opgeslagen in een lokale cache totdat de sessie wordt beëindigd. Wanneer is afgesloten, wordt de cache verwijderd. Aan de clientzijde cookies, die geen persoonlijke gegevens bevatten, worden niet automatisch verwijderd. Sessiecookies HTTPOnly worden gemarkeerd en zijn beveiligd. Na een vooraf bepaald inactief periode, is de portal OMS-sessie beëindigd.

Met de portal OMS, kunt u gegevens exporteren naar een CSV-bestand en u toegang hebt tot gegevens met behulp van de zoeken-API's. CSV-bestand is beperkt tot 50.000 rijen per exporteren en API gegevens is beperkt tot 5000 rijen per zoeken.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met Log Analytics](log-analytics-get-started.md) naar meer informatie over Log Analytics en get omhoog en worden uitgevoerd in minuten.
