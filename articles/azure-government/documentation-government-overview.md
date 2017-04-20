<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Dit vindt u een vergelijking van functies en informatie over het ontwikkelen van toepassingen voor de overheid van Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="08/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-documentation-overview"></a>Documentatieoverzicht Azure overheid

##  <a name="introduction-to-azure-government-documentation"></a>Inleiding tot Azure overheid documentatie

Deze site beschrijving van de mogelijkheden van [Microsoft Azure overheid](https://azure.microsoft.com/features/gov/) services, en algemene richtlijnen die van toepassing op alle klanten. Voordat het specifiek gereglementeerde gegevens te nemen in uw Azure Government-abonnement, moet u vertrouwd raken met de mogelijkheden van de Azure overheid en raadpleegt u uw accountteam als u vragen hebt.

U moet verwijzen naar de [Microsoft Azure vertrouwen naleving pagina beheercentrum](http://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx) voor actuele informatie voor de overheid Azure-services van toepassing op specifieke diploma's en regelgeving. Aanvullende Microsoft-services mogelijk ook beschikbaar, maar buiten het bereik van de services Azure overheid gedekt en niet worden behandeld in dit document. Azure overheid services mogelijk ook toegang tot een aantal extra bronnen, toepassingen of services die worden geleverd door derden, of door Microsoft onder afzonderlijk gebruiksvoorwaarden en de privacyverklaring beleidsregels, die niet zijn opgenomen in het bereik van dit document. U bent verantwoordelijk voor het controleren van de voorwaarden van alle zoals "invoegtoepassing" is voltooid, zoals Marketplace is voltooid, om ervoor te zorgen dat ze uw over naleving behoeften.

Azure overheid is beschikbaar voor entiteiten die gegevens die is onderhevig aan bepaalde Overheidsregels en vereisten (zoals NIST 800.171 (DIB), ITAR, IRS 1075, DoD N4 en CJIS) waar gebruik van Azure overheid is vereist om te voldoen aan regels te verwerken. Azure overheid klanten worden validatie in aanmerking komen.

Entiteiten met vragen over geschiktheid voor de overheid Azure moeten hun accountteam raadplegen.

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Beginselen voor het beveiligen van gegevens van de klant in Azure overheid

Azure overheid biedt een aantal functies en services die u gebruiken kunt om cloud oplossingen aan uw wensen geregeld/beheerde gegevens te maken. Een klantoplossing compatibele is niets meer dan de effectieve uitvoering van out-van-het-box Azure Government-functies, in combinatie met veiligheidsoverwegingen effen gegevens.
Wanneer u een oplossing in Azure overheid host, omgaat met Microsoft veel van deze vereisten is voldaan op het niveau van de infrastructuur cloud.

In het volgende diagram ziet u het Azure ingrijpende model. Microsoft biedt bijvoorbeeld eenvoudige cloudinfrastructuur DDOS, samen met de mogelijkheden van de klant zoals beveiliging apparatuur voor klant-specifiek toepassing die DDOS nodig heeft.

![alternatieve tekst](./media/azure-government-Defenseindepth.png)

Deze pagina bevat de moeten beschikken over principes voor het beveiligen van uw Services en -toepassingen en biedt richtlijnen en aanbevolen procedures voor het toepassen van deze beginselen; met andere woorden, hoe klanten moeten benutten slimme Azure overheid om te voldoen aan de verplichtingen en de verantwoordelijkheden die zijn vereist voor een oplossing die omgaat met ITAR gegevens.

De algehele principes voor het beveiligen van gegevens van de klant zijn:
* Gegevens met behulp van versleuteling beveiligen
* Geheimen beheren
* Moeten worden geïsoleerd beperken van toegang tot gegevens

##  <a name="protecting-customer-data-using-encryption"></a>Klantgegevens beschermen-versleuteling gebruikt

Risico beperken en wettelijke verplichtingen achter het stuur zit de toeneemt focus en het belang van gegevensversleuteling. De implementatie van een effectieve codering gebruiken voor het verbeteren van huidige beveiligingsmaatregelen voor netwerk- en -toepassing, en de algehele risico van uw omgeving cloud verkleinen.

### <a name="Overview"></a>Versleuteling in rust
De codering van gegevens in rust geldt voor de beveiliging van inhoud van klant schijf opgeslagen. Er zijn verschillende manieren die dit kan gebeuren:

### <a name="Overview"></a>Opslag Service-versleuteling

Azure opslag Service versleuteling is ingeschakeld op het niveau van de account opslagruimte met resultaat blok BLOB's en pagina BLOB's automatisch gecodeerde geschreven met Azure Storage. Als u de gegevens van Azure Storage gelezen, wordt deze door de opslagservice decoderen voordat het wordt geretourneerd. Gebruik deze optie als u uw gegevens te beveiligen zonder te wijzigen of de code hebt toegevoegd aan alle toepassingen.

### <a name="Overview"></a>Azure schijfversleuteling
Versleuteling Azure schijf de OS schijven en gegevensschijven die worden gebruikt door een Azure virtuele machines versleutelen. Integratie met Azure-toets kluis biedt beheer en helpt u Schijfopruiming versleuteling toetsen beheren.

### <a name="Overview"></a>Aan de clientzijde versleuteling
Aan de clientzijde versleuteling is ingebouwd in de Java en de .NET opslag clientbibliotheken die van Azure-toets kluis API's gebruikmaken, waardoor dit ingewikkelde willen implementeren. Azure-toets kluis gebruiken voor toegang tot de geheimen in Azure-toets kluis voor specifieke personen met Azure Active Directory.

### <a name="Overview"></a>Versleuteling tijdens overdracht

De eenvoudige beschikbaar voor connectiviteit met Azure overheid versleuteling ondersteunt Transport niveau beveiliging (TLS) 1.2 protocol en x.509-certificaten. Federale Information Processing Standard (FIPS) 140-2 niveau 1 cryptografische algoritmen ook voor infrastructuur netwerkverbindingen tussen Azure overheid datacenters gebruikt worden.  Windows Server 2012 R2 en Windows 8-plus VMs en Azure bestandsshares kunt SMB 3.0 voor versleuteling tussen de VM en het bestand-aandeel gebruiken. Aan de clientzijde versleuteling voor het coderen van de gegevens voordat ze worden overgebracht op te slaan in een clienttoepassing, en de om gegevens te decoderen daarachter wordt overgebracht geen opslagruimte meer.

### <a name="Overview"></a>Aanbevolen procedures voor versleuteling

* IaaS VMs:-Versleuteling Azure Schijfopruiming. Opslag Service versleuteling voor het coderen van de VHD-bestanden die worden gebruikt voor het back-up van deze schijven in Azure-opslag inschakelen, maar dit alleen nieuwe geschreven gegevens worden gecodeerd. Dit betekent dat alleen de wijzigingen worden gecodeerd als u een VM maken en klik vervolgens opslag Service versleuteling inschakelen voor de opslag rekening met het VHD-bestand, niet het oorspronkelijke VHD-bestand.
* Aan de clientzijde versleuteling: Dit is de meest veilige methode voor het coderen van uw gegevens, omdat er vóór de overdracht versleutelt en de gegevens in rust worden gecodeerd. Dit is echter vereist dat u code toevoegen aan uw toepassingen met opslag, waarmee u mogelijk niet wilt doen. In dat geval kunt u HTTPs voor uw gegevens in de overdracht en versleuteling voor opslag-Service voor het coderen van de gegevens van de rest. Aan de clientzijde versleuteling ook betrekking heeft op meer belasting van de client, moet u rekening hiervoor in uw plannen schaalbaarheid, vooral als u worden gecodeerd en een groot aantal gegevens.

Zie de [Opslagruimte Security Guide](/storage-security-guide)voor meer informatie over de versleutelingsopties in Azure wordt aangegeven.

##  <a name="protecting-customer-data-by-managing-secrets"></a>Klantgegevens beschermen door geheimen beheren

Secure key management is essentieel voor het beschermen van gegevens in de cloud. Klanten moeten streven naar vereenvoudigen key management en beheer heeft over sleutels die door cloud-toepassingen en services worden gebruikt om gegevens te versleutelen.

### <a name="Overview"></a>Aanbevolen procedures voor het beheren van geheimen

* Gebruik toets kluis om het risico van geheimen worden gepubliceerd via hard gecodeerde configuratiebestanden, scripts, of in broncode. Azure-toets kluis toetsen (zoals de versleutelingssleutels voor Azure schijfversleuteling) en geheimen (zoals wachtwoorden) worden gecodeerd, door ze te slaan in FIPS 140-2 niveau 2 gevalideerd hardware beveiligingsmodules (HSM's). U kunt importeren of toetsen in deze HSM's genereren voor toegevoegde assurance.
* Toepassing en sjablonen mag alleen bevatten URI verwijzingen naar de geheimen (wat betekent dat de werkelijke geheimen zijn niet in code, configuratie of broncode opslagplaatsen). Hiermee voorkomt u belangrijke phishing-aanvallen op interne en externe repo's, zoals oogst-bots in GitHub.
* Gebruik van sterke RBAC besturingselementen in sleutel kluis. Als een vertrouwde operator het bedrijf of de overdrachten aan een nieuwe groep in het bedrijf laat, moeten u ze niet mogen voor toegang tot de geheimen.  

Zie voor meer informatie [Toets kluis voor Azure overheid](/azure-government/azure-government-tech-keyvault)

##  <a name="isolation-to-restrict-data-access"></a>Moeten worden geïsoleerd beperken van toegang tot gegevens

Moeten worden geïsoleerd draait allemaal grenzen, segmentatie en containers gebruiken om te beperken van toegang tot de gegevens alleen geautoriseerde gebruikers, services en toepassingen. De scheiding tussen tenants is bijvoorbeeld een essentiële beveiliging om multitenant cloud platforms zoals Microsoft Azure. Logische moeten worden geïsoleerd helpt voorkomen dat een tenant verhinderd met de bewerkingen van een andere tenant.

### <a name="Overview"></a>Omgeving moeten worden geïsoleerd
De omgeving Azure overheid is een fysieke instantie die losstaat van de rest van het netwerk van Microsoft. Dit is bereikt door een reeks besturingselementen van fysieke en logische die het volgende omvat: beveiliging van fysieke grenzen biometrische apparaten en camera's gebruiken.  Gebruik van bepaalde referenties en meervoudige verificatie Microsoft-medewerkers waarbij logische toegang tot de productieomgeving.  Alle service-infrastructuur voor de overheid Azure bevindt zich binnen de Verenigde Staten.

#### <a name="Overview"></a>Per klant moeten worden geïsoleerd
Toegangsbeheer voor Azure implementeert netwerk en scheiding tot en met VLAN moeten worden geïsoleerd, ACL's, laden balancers en IP-filters

Klanten kunnen hun resources verder isoleren over abonnementen, resourcegroepen virtuele netwerken en subnetten.

Zie de [sectie moeten worden geïsoleerd van de Azure Security Guide](/azure-security-getting-started/#isolation)voor meer informatie over moeten worden geïsoleerd in Microsoft Azure.

Voor aanvullende informatie en updates Abonneer u op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
