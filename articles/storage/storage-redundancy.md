<properties
    pageTitle="Azure opslag herhaling | Microsoft Azure"
    description="Gegevens in uw account van Microsoft Azure opslag worden voor levensduur en betere beschikbaarheid gerepliceerd. Replicatieopties opnemen lokaal redundante opslag (LRS), zone-redundante opslag (ZRS), geografische-redundante opslag (GRS) en leestoegang geografische-redundante opslag (AB-GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure opslag herhaling

De gegevens in uw account van Microsoft Azure opslag is altijd om ervoor te zorgen levensduur en betere beschikbaarheid gerepliceerd. Replicatie wordt gekopieerd van uw gegevens, binnen de dezelfde Datacenter, of naar een tweede Datacenter, dat afhankelijk van de optie herhaling die u kiest. Replicatie uw gegevens beschermen en behoudt u uw toepassing-up-tijd in het geval van problemen met de tijdelijke hardware. Als uw gegevens worden gerepliceerd naar een tweede Datacenter, beveiligt dat ook u uw gegevens ten opzichte van een volledige uitval in de primaire locatie.

Replicatie zorgt ervoor dat uw account opslag voldoet aan de [Service Level Agreement (SLA) voor opslag,](https://azure.microsoft.com/support/legal/sla/storage/) zelfs licht fouten. Zie dat de SLA voor informatie over Azure opslag garandeert voor levensduur en beschikbaarheid. 

Wanneer u een opslag-account hebt gemaakt, kunt u een van de volgende replicatieopties selecteren:  

- [Lokaal redundante opslag (LRS)](#locally-redundant-storage)
- [Zone-redundante opslag (ZRS)](#zone-redundant-storage)
- [Geografische-redundante opslag (GRS)](#geo-redundant-storage)
- [Leestoegang geografische-redundante opslag (AB-GRS)](#read-access-geo-redundant-storage)

Leestoegang geografische-redundante opslag (AB-GRS) is de standaardoptie wanneer u een nieuw account voor de opslag maakt.

De volgende tabel biedt een beknopt overzicht van de verschillen tussen LRS, ZRS GRS en AB-GRS, terwijl de volgende secties adres van elk type herhaling uitgebreider.

| Replicatiestrategie                                                               | LRS | ZRS | GRS | AB-GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Gegevens worden gerepliceerd naar meerdere datacenters.                                     | Nee  | Ja | Ja | Ja    |
| Gegevens kunnen worden gelezen uit de secundaire locatie als de primaire locatie. | Nee  | Nee  | Nee  | Ja    |
| Het aantal exemplaren van gegevens op afzonderlijke knooppunten worden onderhouden.                             | 3   | 3   | 6   | 6      |

Zie de [Prijzen van Azure-opslag](https://azure.microsoft.com/pricing/details/storage/) voor prijsgegevens voor de verschillende redundantieopties.

>[AZURE.NOTE] Premium opslag ondersteunt alleen lokaal redundante opslag (LRS). Zie voor informatie over Premium opslag [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Lokaal redundante opslag

Lokaal redundante opslag (LRS) wordt overgenomen door uw gegevens driemaal binnen een eenheid voor de schaal van opslagruimte die wordt gehost in een datacenter in de regio waarin u uw opslagruimte-account hebt gemaakt. Een verzoek om te schrijven retourneert succes alleen zodra deze is geschreven op alle drie replica's. Deze drie replica's elke zijn opgeslagen in afzonderlijke foutenstructuuranalyse domeinen en de upgrade domeinen binnen één opslag-eenheid voor tijdschaal. 

Een eenheid voor tijdschaal opslag is een verzameling rekken van opslagknooppunten. Een domein foutenstructuuranalyse (FD) is een groep van knooppunten die staan voor een fysieke eenheid is mislukt en kunnen worden beschouwd als knooppunten die deel uitmaken van hetzelfde fysieke rek. Een upgrade domein (per) is een groep van knooppunten die samen tijdens het uitvoeren van een service-upgrade (uitrol) zijn bijgewerkt. De drie replica's zijn verdeeld over UDs en FDs binnen één opslag schaaleenheid om ervoor te zorgen dat gegevens beschikbaar is, zelfs als een eenmalige rek van invloed op hardware mislukt of wanneer knooppunten tijdens een uitrol worden bijgewerkt. 

LRS is de laagste kosten-optie en biedt minimaal levensduur vergeleken met andere opties. Een storing datacenter niveau (fire, overbelasting enz.) alle drie replica's mogelijk verloren of onherstelbare. Als u wilt dit risico beperken verdient geografische overtollige opslag (GRS) voor de meeste toepassingen.

Lokaal redundante opslag mogelijk nog steeds wenselijk in bepaalde scenario's: 

- Biedt een hoogste maximale bandbreedte van Azure Storage replicatie-opties.

- Als uw toepassing worden gegevens die kunnen eenvoudig worden gerepareerd opgeslagen, kunt u kunt ervoor kiezen voor LRS.

- Sommige-toepassingen zijn beperkt tot repliceren van gegevens alleen binnen een land vanwege gegevens beheermodel vereisten. Een gepaarde gebied kan worden in een ander land; Zie [Azure regio's](https://azure.microsoft.com/regions/) voor meer informatie over regio paren.

## <a name="zone-redundant-storage"></a>Zone-redundante opslag

Zone-redundante opslag (ZRS) wordt overgenomen door uw gegevens asynchroon over datacenters binnen één of twee regio's naast het opslaan van drie replica vergelijkbaar met LRS, dus leveren hoger levensduur dan LRS. Gegevens die zijn opgeslagen in ZRS is blijvend zelfs als het primaire datacenter niet beschikbaar of onherstelbare is.
Klanten die het gebruik van ZRS plannen moeten zich realiseren dat: 

- ZRS is alleen beschikbaar voor blok BLOB's in de algemene opslag accounts en wordt alleen in versies van de service opslag 2014-02-14 ondersteund en hoger. 

- Aangezien asynchroon herhaling moet een vertraging worden, een lokale storing is het mogelijk dat wijzigingen die nog niet zijn gerepliceerd naar de secundaire verloren worden als de gegevens van de primaire kan niet worden hersteld.

- De replica zijn mogelijk niet beschikbaar totdat Microsoft failover naar de secundaire start.

- ZRS accounts kunnen niet later worden geconverteerd naar LRS of GRS. Een bestaand LRS of GRS-account kan niet op dezelfde manier worden geconverteerd naar een ZRS-account.

- ZRS accounts beschikt niet over de doelstellingen of logboekregistratie mogelijkheid. 

## <a name="geo-redundant-storage"></a>Geografische-redundante opslag

Geografische-redundante opslag (GRS) wordt overgenomen door uw gegevens een secundaire gebied dat honderden mijl weg van het primaire regio. Als uw account opslagruimte GRS ingeschakeld heeft, zijn uw gegevens is duurzame zelfs in geval van een volledige regionale storing of een noodgevallen waarin de primaire regio kan niet worden hersteld.

Voor een opslagruimte met GRS ingeschakeld, wordt eerst een update vastgelegd voor de primaire regio, waar deze drie keer worden gerepliceerd. Vervolgens worden de update asynchroon gerepliceerd naar de secundaire regio, waar deze wordt drie keer ook gerepliceerd. 

Met GRS de primaire en secundaire gebieden replica's beheren op meerdere afzonderlijke foutenstructuuranalyse domeinen en domeinen binnen een opslageenheid voor tijdschaal zoals wordt beschreven met LRS upgraden.

Overwegingen:

- Aangezien asynchroon herhaling moet een vertraging worden, een regionale storing is het mogelijk dat wijzigingen die nog niet zijn gerepliceerd naar de tweede regio verloren worden als de gegevens uit het primaire regio kan niet worden hersteld.

- De replica is alleen beschikbaar als Microsoft failover naar de tweede regio start.

- Als een toepassing wil lezen uit de tweede regio moet de gebruiker AB-GRS inschakelen. 

Wanneer u een opslag-account hebt gemaakt, selecteert u de primaire regio voor het account. De tweede regio wordt bepaald op basis van de primaire regio en kan niet worden gewijzigd. De volgende tabel ziet u de koppelingen primaire en secundaire regio.

| Primaire             | Secundaire           |
|---------------------|---------------------|
| Centraal Noord-Amerikaanse    | Zuid-Central US    |
| Zuid-Central US    | Centraal Noord-Amerikaanse    |
| Oost-VS             | West VS             |
| West VS             | Oost-VS             |
| Amerikaans Oost 2           | Central US          |
| Central US          | Amerikaans Oost 2           |
| Noord-Europa        | West Europa         |
| West Europa         | Noord-Europa        |
| Zuid-Oost-Azië     | Oost-Azië           |
| Oost-Azië           | Zuid-Oost-Azië     |
| Oost-China          | Noord China         |
| Noord China         | Oost-China          |
| Japan Oost          | Japan West          |
| Japan West          | Japan Oost          |
| Brazilië Zuid        | Zuid-Central US    |
| Australië Oost      | Australië Zuidoost |
| Australië Zuidoost | Australië Oost      |
| India Zuid         | India centraal       |
| India centraal       | India Zuid         |
| Amerikaans beurs Iowa         | Amerikaans beurs Virginia     |
| Amerikaans beurs Virginia     | Amerikaans beurs Iowa         |
| Canada centraal      | Canada Oost          |
| Canada Oost         | Canada centraal      |
| Verenigd Koninkrijk West             | Verenigd Koninkrijk Zuid            |
| Verenigd Koninkrijk Zuid            | Verenigd Koninkrijk West             |
| Duitsland centraal     | Duitsland goed   |
| Duitsland goed   | Duitsland centraal     |
| West Amerikaans 2           | Centraal West VS     |
| Centraal West VS     | West Amerikaans 2           |

Zie voor actuele informatie over regio's die worden ondersteund door Azure, [Azure regio's](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Leestoegang geografische-redundante opslag

Leestoegang geografische-redundante opslag (AB-GRS) maximaal beschikbaar voor uw account opslag via de alleen-lezen toegang tot de gegevens in de tweede locatie, naast de replicatie tussen de twee regio's GRS gekregen. 

Als u alleen-lezen toegang tot uw gegevens in de tweede regio inschakelt, is uw gegevens beschikbaar op een secundaire eindpunt, behalve de primaire eindpunt voor uw account opslag. Het secundaire eindpunt is vergelijkbaar met het primaire eindpunt, maar voegt het achtervoegsel `–secondary` naar de accountnaam van het. Als uw primaire eindpunt voor de service Blob is bijvoorbeeld `myaccount.blob.core.windows.net`, is uw secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangstoetsen voor uw account opslag zijn hetzelfde voor de primaire en secundaire eindpunten.

Overwegingen:

- Uw toepassing heeft tot het beheren van welke eindpunt deze interactief werken is met bij gebruik van AB-GRS. 

- AB-GRS is bedoeld voor hoge beschikbaarheid. Raadpleeg de [prestaties controlelijst](storage-performance-checklist.md)voor schaalbaarheid richtlijnen.

## <a name="next-steps"></a>Volgende stappen

- [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/)
- [Over accounts Azure opslag](storage-create-storage-account.md)
- [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md)
- [Microsoft Azure opslag redundantieopties en leestoegang geografische redundante opslag](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP papier - Azure opslag: Een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
