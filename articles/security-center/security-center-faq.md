<properties
   pageTitle="Azure Beveiligingscentrum Veelgestelde vragen (FAQ) | Microsoft Azure"
   description="Deze Veelgestelde vragen vindt u antwoorden op vragen over Azure Beveiligingscentrum."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Beveiligingscentrum veelgestelde Veelgestelde vragen

Deze Veelgestelde vragen vindt u antwoorden op vragen over Azure Beveiligingscentrum, een service waarmee u voorkomen dat kunt, detecteren en reageren op threats met deze beter kunt zien in en controle over de beveiliging van uw Microsoft Azure-resources.

## <a name="general-questions"></a>Algemene vragen

### <a name="what-is-azure-security-center"></a>Wat is Azure Beveiligingscentrum?
Azure Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats met deze beter kunt zien in en controle over de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

### <a name="how-do-i-get-azure-security-center"></a>Hoe krijg ik Azure Beveiligingscentrum?
Azure Beveiligingscentrum is ingeschakeld met uw Microsoft Azure-abonnement en weergegeven via de [portal van Azure](https://azure.microsoft.com/features/azure-portal/). ([Meld u aan bij de portal](https://portal.azure.com), selecteert u **Bladeren**, en schuif naar **Beveiligingscentrum**).  

## <a name="billing"></a>Facturering

### <a name="how-does-billing-work-for-azure-security-center"></a>Hoe werkt facturering voor Azure Beveiligingscentrum?
Beveiligingscentrum wordt aangeboden in twee lagen: gratis en de standaardversie.

De gratis laag kunt u beveiligingsbeleid voor apparaten instellen en ontvangen beveiligingsmeldingen, incidenten en aanbevelingen die u begeleid bij het proces van het configureren van de benodigde besturingselementen. U kunt ook de beveiligingsstatus van uw Azure resources en partneroplossingen geïntegreerd met uw Azure-abonnement controleren voordat u met de gratis laag.

De standaard laag biedt de gratis laag plus geavanceerde functies detectie: gevaar bedrijfsinformatie, doorgevoerd analyse vastlopen analyse en afwijking detectie. Er is een gratis proefversie van 90 dagen van de standaard laag beschikbaar. Als u wilt bijwerken, selecteert u prijzen laag in het [beveiligingsbeleid](security-center-policies.md#setting-security-policies-for-subscriptions). Zie [Beveiligingscentrum prijzen](security-center-pricing.md) voor meer informatie.

## <a name="data-collection"></a>Gegevens verzamelen

Beveiligingscentrum verzamelt gegevens van uw virtuele machines om te kunnen hun beveiligingsstatus beoordelen, aanbevelingen voor beveiliging bieden en u attent maken op threats. Wanneer u eerst Beveiligingscentrum, worden gegevens verzamelen is ingeschakeld op alle virtuele machines in uw abonnement. U wordt aangeraden de gegevens verzamelen, maar u kunt opt-out door te [schakelen van de gegevensverzameling](#how-do-i-disable-data-collection) in het beleid Beveiligingscentrum.

### <a name="how-do-i-disable-data-collection"></a>Hoe kan ik gegevens verzamelen uitschakelen?

U kunt **gegevens verzamelen** voor een abonnement in het beveiligingsbeleid op elk gewenst moment uitschakelen. ([Meld u aan bij de portal van Azure](https://portal.azure.com), selecteert u **Bladeren**, selecteer **Beveiligingscentrum**, en selecteer **beleid**.)  Wanneer u een abonnement selecteert, wordt een nieuwe blade wordt geopend en biedt u de optie **gegevens verzamelen** om uit te schakelen. Selecteer de optie **agenten verwijderen** in het bovenste lint agenten uit bestaande virtuele machines verwijderen.

> [AZURE.NOTE] Beveiligingsbeleid voor apparaten kunnen worden ingesteld op het niveau van de Azure-abonnement en het niveau van de resourcegroep, maar moet u een abonnement gegevens verzamelen om uit te schakelen.

### <a name="how-do-i-enable-data-collection"></a>Hoe kan ik gegevens verzamelen inschakelen?
U kunt gegevens verzamelen inschakelen voor uw Azure abonnementen in het beveiligingsbeleid. Als u wilt verzamelen inschakelen, [Meld u aan bij de portal van Azure](https://portal.azure.com), selecteert u **Bladeren**, selecteer **Beveiligingscentrum**, en selecteer **beleid**. **Gegevens verzamelen** ingesteld **op** en de gewenste gegevens te verzamelen om te opslag-accounts configureren (Zie vraag "[waar zijn mijn gegevens opgeslagen?](#where-is-my-data-stored)"). Bij het **verzamelen van gegevens** is ingeschakeld, automatisch beveiliging configuratie en gebeurtenis worden gegevens verzameld uit alle ondersteunde virtuele machines in het abonnement.

> [AZURE.NOTE] Beveiligingsbeleid voor apparaten kunnen worden ingesteld op het niveau van de Azure-abonnement en het niveau van de resourcegroep, maar de configuratie van de gegevensverzameling doet zich voor op alleen het abonnement.

### <a name="what-happens-when-data-collection-is-enabled"></a>Wat gebeurt er als de gegevensverzameling is ingeschakeld?
Gegevens verzamelen is via de Azure Monitoring-Agent en de extensie Azure beveiliging controleren ingeschakeld. De extensie Azure beveiliging Monitoring doorzoekt voor verschillende beveiliging relevante en stuurt dit in [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) sporen. Bovendien maakt het besturingssysteem logboekvermeldingen.  De controle-Agent van Azure logboekvermeldingen leest en ETW wordt getraceerd en kopieert u deze aan uw account opslag voor analyse.  Dit is de opslag-account dat u in het beveiligingsbeleid hebt geconfigureerd. Zie voor meer informatie over de opslag-account, vraag "[waar zijn mijn gegevens opgeslagen?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Heeft de extensie Monitoring Agent of beveiliging Monitoring invloed op de prestaties van mijn server (s)?
De agent en extensie een nominale hoeveelheid systeembronnen verbruikt en moet weinig invloed hebben op de prestaties.

### <a name="where-is-my-data-stored"></a>Waar is mijn gegevens opgeslagen?
Voor elke regio waarin u virtuele machines uitgevoerd hebt, kunt u het opslag-account waarin de gegevens die worden verzameld uit deze virtuele machines is opgeslagen kiezen. Hiermee kunt u gemakkelijk voor u om de gegevens in hetzelfde geografische gebied voor privacy en gegevens te garanderen doeleinden. U kiezen het account opslagruimte voor een abonnement in het beveiligingsbeleid. ([Meld u aan bij de portal van Azure](https://portal.azure.com), selecteert u **Bladeren**, selecteer **Beveiligingscentrum**, en selecteer **beleid**.) Wanneer u op een abonnement klikt, wordt een nieuwe blade geopend. Selecteer **kiezen opslag accounts** om een regio te selecteren.

> [AZURE.NOTE] Beveiligingsbeleid voor apparaten kunnen worden ingesteld op het niveau van de Azure-abonnement en het niveau van de resourcegroep maar selecteren van een gebied voor uw account opslag doet zich voor op alleen het abonnement.

Zie voor meer informatie over Azure opslag en opslag-accounts, [Opslag documentatie](https://azure.microsoft.com/documentation/services/storage/) en [over Azure opslag-accounts](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Azure Beveiligingscentrum

### <a name="what-is-a-security-policy"></a>Wat is een beveiligingsbeleid?
Een beveiligingsbeleid definieert de reeks besturingselementen die worden aanbevolen voor bronnen binnen de opgegeven abonnement of de resourcegroep. In Azure Beveiligingscentrum definieert u beleidsregels voor uw Azure abonnementen en resourcegroepen op basis van uw bedrijf beveiligingsvereisten die zijn en het type van toepassingen of gevoeligheid van de gegevens in elk abonnement.

Resources die worden gebruikt voor ontwikkel- of kunnen bijvoorbeeld verschillende beveiligingsvereisten die zijn dan die worden gebruikt voor productietoepassingen. Toepassingen gebruiken met gereglementeerde gegevens zoals PII (Personally Identifiable Information) wellicht ook een hoger niveau van beveiliging. De beveiligingsbeleid voor apparaten zijn ingeschakeld in Azure Beveiligingscentrum wordt station aanbevelingen voor beveiliging en controle. Meer informatie over beveiligingsbeleid voor apparaten, raadpleegt u [beveiliging servicestatus bewaken in Beveiligingscentrum Azure](security-center-monitoring.md).

> [AZURE.NOTE] Bij een conflict tussen abonnement niveau beleid en niveau Groepsbeleid van de resource, is het niveau Groepsbeleid van resource voorrang.

### <a name="who-can-modify-a-security-policy"></a>Wie kan een beveiligingsbeleid wijzigen?
Beveiligingsbeleid voor apparaten zijn geconfigureerd voor elk abonnement of de resourcegroep. Als u wilt een beveiligingsbeleid aan het abonnement of het niveau van de resourcegroep wijzigen, moet u een eigenaar of Inzender van abonnement.

Als u wilt weten hoe u een beveiligingsbeleid configureren, raadpleegt u de [instelling beveiligingsbeleid voor apparaten in Beveiligingscentrum Azure](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Wat is een aanbeveling beveiliging?
Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources. Mogelijke beveiligingsrisico's worden aangeduid, worden aanbevelingen gemaakt. De aanbevelingen helpt u bij het proces van het configureren van het benodigde besturingselement. Voorbeelden zijn:

- Inrichten van antimalware om te identificeren en schadelijke software
- [Beveiligingsgroepen netwerk](../virtual-network/virtual-networks-nsg.md) en regels voor het besturingselement verkeer naar virtuele machines configureren
- Inrichten van een firewall van de toepassing web om u te beschermen tegen aanvallen uw webtoepassingen doelgroepen
- De ontbrekende systeemupdates
- Adressering van OS configuraties die niet overeenkomen met de aanbevolen basislijnen

Alleen de aanbevelingen die zijn ingeschakeld in beveiligingsbeleid voor apparaten worden hier weergegeven.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Hoe kan ik de huidige beveiligingsstatus van mijn Azure resources zien?
Een tegel **Resources status** op het blad **Beveiligingscentrum** ziet u de algehele houding beveiliging van uw omgeving die is gesplitst per virtuele machines, webtoepassingen en andere resources. Elke resource heeft een indicator weergegeven als een mogelijke beveiligingsrisico's zijn geïdentificeerd. Te klikken op de tegel van de servicestatus Resources wordt uw resources en weergegeven waar aandacht vereist is of problemen kunnen zich voordoen aangeeft.

### <a name="what-triggers-a-security-alert"></a>Wat een beveiligingswaarschuwing geactiveerd?
Azure Beveiligingscentrum automatisch worden verzameld, worden geanalyseerd en logboekgegevens uit uw Azure resources, het netwerk en partneroplossingen zoals antimalware en firewalls worden. Wanneer threats zijn gevonden, wordt een beveiligingswaarschuwing wordt gemaakt. Voorbeelden hiervan zijn detectie van:

- Beschadigde virtuele machines communiceren met bekende schadelijke IP-adressen
- Geavanceerde malware gedetecteerd met Windows Foutrapportage
- Brutekrachtaanvallen tegen virtuele machines
- Beveiligingsmeldingen van geïntegreerde beveiliging-partneroplossingen zoals Anti-Malware of Web-toepassing Firewalls

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wat is het verschil tussen threats gedetecteerd en een melding op door Microsoft Security Response Center versus Azure Beveiligingscentrum?
De MSRC Microsoft Security antwoord Center () voert selecteert u beveiliging cmdlets voor controle van de Azure netwerk en de infrastructuur en bedreiging intelligence en abuse klachten van derden ontvangt. Wanneer MSRC realiseert dat klantgegevens is geopend door een feest onrechtmatige of door onbevoegden of dat van de klant gebruik van Azure niet aan de voorwaarden voor het aanvaardbaar gebruiken voldoet, krijgt een incident beveiligingsinstellingen de klant. Melding gebeurt meestal door een e-mailbericht te sturen naar de contactpersonen beveiliging die is opgegeven in Azure Beveiligingscentrum of de eigenaar van de Azure-abonnement als u een contactpersoon voor beveiliging is opgegeven.

Beveiligingscentrum is een Azure-service die continu worden bewaakt Azure omgeving van de klant en analyses om op te sporen automatisch een breed scala van mogelijk schadelijke activiteit toepast. Deze detectie zijn als beveiligingswaarschuwingen in het dashboard Beveiligingscentrum opgehaald.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Hoe worden de machtigingen in Azure Beveiligingscentrum afgehandeld?
Azure Beveiligingscentrum ondersteunt Rolgebaseerd-toegang. Zie voor meer informatie over Rolgebaseerd toegangsbeheer (RBAC) in Azure wordt aangegeven, [Toegangsbeheer op basis van Azure Active Directory-rol](../active-directory/role-based-access-control-configure.md).

Wanneer een gebruiker opent Beveiligingscentrum, alleen aanbevelingen en waarschuwingen die zijn gerelateerd aan de gebruiker toegang tot heeft resources zijn zichtbaar. Dit betekent dat gebruikers alleen items met betrekking tot resources waar de gebruiker de rol van eigenaar, Inzender of Reader is toegewezen aan het abonnement of een resourcegroep die een resource behoort zien.

Als u doen wilt:

- **Een beveiligingsbeleid bewerken**, moet u een eigenaar of Inzender van het abonnement zijn.
- **Toepassen aanbeveling**, moet u een eigenaar of Inzender van het abonnement zijn.
- **Inzicht in de stand beveiliging in al uw abonnementen hebt**, moet u een eigenaar, Inzender of Reader (IT-beheerder, Security Team) zijn van elk abonnement.
- **Inzicht in de beveiligingsstatus van uw resources hebt**, moet u een resourcegroep eigenaar Inzender of Reader (DevOps) zijn.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welke Azure resources worden gecontroleerd door Azure Beveiligingscentrum?
Azure Beveiligingscentrum bewaakt de volgende Azure bronnen:

- Virtuele machines (VMs) (inclusief [Cloud Services](../cloud-services/cloud-services-choose-me.md))
- Azure virtuele netwerken
- Azure SQL-service
- Partneroplossingen geïntegreerd met uw Azure abonnement zoals een web application-firewall op VMs en klik op [App-omgeving](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuele Machines

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Welke soorten virtuele machines worden ondersteund?
Beveiliging statuscontrole en aanbevelingen zijn beschikbaar voor virtuele machines (VMs) gemaakt met behulp van beide de [klassieke en resourcemanager implementatiemodellen](../azure-classic-rm.md).

Windows VMs ondersteund:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

Ondersteunde Linux VMs:

- Ubuntu versies 12.04, 14.04, 16.04
- Debian versies 7, 8
- CentOS versies 6. \*, 7.*
- Rood rol Enterprise Linux (RHEL) versies 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) versies 11. \*, 12.*
- Oracle Linux versies 6. \*, 7.*

VMs uitgevoerd in een cloudservice worden ook ondersteund. Alleen cloud services web en werknemer rollen uitgevoerd in productie sleuven worden gecontroleerd. Zie voor meer informatie over cloudservice binnenkomt, [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Waarom wordt niet Azure Beveiligingscentrum herkend door de antimalware-oplossing op mijn VM Azure uitgevoerd?

Azure Beveiligingscentrum heeft alleen zicht op antimalware geïnstalleerd via Azure uitbreidingen. Beveiligingscentrum is bijvoorbeeld niet kan detecteren antimalware die vooraf geïnstalleerd op een afbeelding die u hebt opgegeven of is als u antimalware geïnstalleerd op uw virtuele machines met uw eigen processen (zoals configuratie management systemen).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Waarom krijg ik het bericht 'Scannen gegevens ontbreken' voor mijn VM?

Dit kan enige tijd duren voordat (doorgaans kleiner is dan een uur) scan gegevens om te vullen nadat de gegevens verzamelen in Azure Beveiligingscentrum is ingeschakeld. Scannen is niet vullen voor VMs gestopt status.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Waarom krijg ik het bericht "VM-Agent is ontbrekende?"

De VM-Agent moeten worden geïnstalleerd op VMs om te verzamelen inschakelen. De VM-Agent is al dan niet standaard geïnstalleerd voor VMs die zijn geïmplementeerd van Azure Marketplace. Zie het blogbericht [VM Agent en uitbreidingen](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)voor informatie over het installeren van de VM-Agent op andere VMs.
