<properties
    pageTitle="Infrastructuur Naming richtlijnen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren van de richtlijnen voor de naamgeving in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Infrastructuur naming richtlijnen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

In dit artikel ligt de nadruk met informatie over benadering naamgevingsconventies voor alle uw verschillende Azure resources voor het maken van een logische en gemakkelijk kunnen worden geïdentificeerd reeks resources in uw omgeving.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementatie van richtlijnen voor naamgevingsconventies

Beslissingen:

- Wat zijn de naamgevingsconventies voor Azure resources?

Taken:

- De voor-en achtervoegsel gebruiken in uw resources om de consistentie te definiëren.
- Opslag accountnamen gegeven van de vereiste voor hen globaal uniek definiëren.
- De naamgevingsconventie worden gebruikt en distribueren naar alle betrokkenen een consistente over implementaties van het document.

## <a name="naming-conventions"></a>Naamgevingsconventies

U moet een goede naamgevingsconventie hebt geplaatst voordat u maakt in Azure wordt aangegeven. Een naamgevingsconventie zorgt ervoor dat alle resources een overzichtelijk naam hebben, waardoor Verlaag de administratieve last die is gekoppeld aan om deze resources te beheren.

U kunt kiezen om te volgen van een specifieke reeks naamgevingsconventies gedefinieerd voor de hele organisatie of voor een bepaald Azure-abonnement of een account. Hoewel het is eenvoudig voor personen binnen organisaties tot stand brengen van impliciete regels tijdens het werken met Azure resources, wanneer een team moet werken aan een project op Azure, dat model wordt niet goed op schaal.

Een reeks vooraf naamgevingsconventies afspreken. Er zijn enkele overwegingen met betrekking tot naamgevingsconventies die meerdere die sets van regels.

## <a name="affixes"></a>Voor-en achtervoegsel

Als u wilt opzoeken om te definiëren van een naamgevingsconventie, wordt een beslissing geleverd mee of het voorvoegsel op:

- Het begin van de naam (voorvoegsel)
- Het einde van de naam (achtervoegsel)

Bijvoorbeeld hier zijn twee mogelijke namen voor een resourcegroep met de `rg` brengt:

- Rg-WebApp (voorvoegsel)
- WebApp-Rg (achtervoegsel)

Voor-en achtervoegsel kunnen verwijzen naar verschillende aspecten die de bepaalde bronnen beschrijven. De volgende tabel ziet u enkele voorbeelden doorgaans worden gebruikt.

| Aspecten                               | Voorbeelden                                                               | Notities                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Omgeving                          | ontwikkelaar, stg, Prod.                                                         | Afhankelijk van het doel en de naam van elke omgeving.                                                     |
| Locatie                             | usw (West Verenigde Staten), gebruiken (Oost VS 2)                                         | Afhankelijk van het gebied van het datacenter of de regio van de organisatie.                               |
| Azure onderdeel, service of product | Rg voor resourcegroep, VNet voor virtuele netwerk                        | Afhankelijk van het product die de resource ondersteund wordt.                                          |
| Rol                                 | SQL, ora, sp, iis                                                      | Afhankelijk van de rol van de virtuele machine.                                                              |
| Exemplaar                             | 01, 02, 03, enzovoort.                                                       | Voor resources die meer dan één exemplaar hebben. Bijvoorbeeld laden gebalanceerde endwebservers in een cloudservice. |


Bij het maken van uw naamgevingsconventies, zorg dat ze duidelijk welke achtervoegsel bepaalt voor elk type van resource, en in welke volgorde (voorvoegsel tegenover achtervoegsel).

## <a name="dates"></a>Datums

Het verdient vaak om te bepalen de datum van het maken van de naam van een resource. U wordt aangeraden de datumnotatie JJJJMMDD. Deze indeling zorgt ervoor dat niet alleen de volledige datum is opgenomen, maar ook twee resources waarvan de naam verschillen alleen op de datum op alfabetische volgorde en in chronologische volgorde wordt gesorteerd op hetzelfde moment.

## <a name="naming-resources"></a>Naamgeving van resources

Elk type definiëren van de resource in de naamgevingsconventie, waarin de regels die bepalen hoe namen toewijzen aan elke resource die is gemaakt nodig hebt. Deze regels gelden voor alle typen resources, bijvoorbeeld:

- Abonnementen
- Accounts
- Opslag-accounts
- Virtuele netwerken
- Subnetten
- Beschikbaarheid van sets
- Resourcegroepen
- Virtuele machines
- Eindpunten
- Beveiligingsgroepen netwerk
- Rollen

Biedt voldoende informatie om te bepalen met welke bron dit logboekitem verwijst, moet u beschrijvende namen gebruiken om ervoor te zorgen dat de naam.

## <a name="computer-names"></a>Computernamen

Wanneer u een virtuele machine (VM) maakt, moet Microsoft Azure een VM naam van maximaal 15 tekens die wordt gebruikt voor de naam van de resource. Azure gebruikt dezelfde naam voor het besturingssysteem is geïnstalleerd in de VM. Echter deze namen niet altijd mogelijk hetzelfde.

Geval een VM vanuit een afbeeldingsbestand VHD al met een besturingssysteem wordt gemaakt, wordt de naam van de VM in Azure kan afwijken van de naam van de computer besturingssysteem van de VM. Deze situatie kunt een mate van problemen met de toevoegen aan VM management, dus wordt niet aanbevolen. De resource Azure VM dezelfde naam als de naam van de computer die u aan het besturingssysteem van die VM toewijst toewijzen.

Het is raadzaam dat de naam van de Azure VM hetzelfde als de naam van de onderliggende besturingssysteem-computer is.

## <a name="storage-account-names"></a>Opslag accountnamen

Opslag-accounts zijn de speciale regels ten opzichte van hun namen. U kunt alleen kleine letters en cijfers. Zie [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) voor meer informatie. Bovendien moet de accountnaam opslag, samen met core.windows.net, een globaal geldig, uniek DNS-naam. Bijvoorbeeld als de opslagruimte-account wordt aangeroepen mystorageaccount, moeten de volgende resulterende DNS-namen uniek zijn:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.Table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 