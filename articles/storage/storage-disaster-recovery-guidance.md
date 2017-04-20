<properties
    pageTitle="Wat moet u doen in het geval van een storing Azure Storage | Microsoft Azure"
    description="Wat moet u doen in het geval van een storing Azure Storage"
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
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Wat u moet doen als er een storing Azure Storage optreedt

Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd beschikbaar zijn. Soms dwingt voorbij de impact van onze besturingselement ons op manieren die ertoe leiden dat ongeplande service bijvoorbeeld in een of meer regio's. Als u deze niet vaak voorkomen exemplaren afhandelen, bieden wij instructies voor de volgende hoog niveau van Azure Storage-services.

## <a name="how-to-prepare"></a>Voorbereidingen 

Het is belangrijk is voor elke klant voor het voorbereiden van hun eigen gaan. De hoeveelheid herstellen uit een storing opslag meestal heeft betrekking op zowel bewerkingen personeel en geautomatiseerde procedures om te activeren van uw toepassingen functioneel status. Raadpleeg de Azure documentatie hieronder voor het maken van uw eigen gaan:

-   [Herstel en beschikbaarheid van Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Technische ondersteuning van Azure tolerantie](../resiliency/resiliency-technical-guidance.md)

-   [Azure Site herstel-service](https://azure.microsoft.com/services/site-recovery/)

-   [Azure opslag herhaling](storage-redundancy.md)

-   [Azure back-up-service](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Hoe u het detecteren 

De aanbevolen manier om te bepalen de status van de Azure-service is zich kunnen abonneren op het [Dashboard voor servicestatus van Azure-Service](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Wat u moet doen als er een storing opslag optreedt

Als een of meer opslagruimte services op een of meer regio's tijdelijk niet beschikbaar zijn, zijn er twee opties voor oplossingen doornemen. Als u direct toegang om uw gegevens, houd rekening met de optie 2.

### <a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op een herstel

In dit geval is geen actie ondernemen vereist. We werken naar eer en geweten als u wilt herstellen van de servicebeschikbaarheid van Azure. U kunt de servicestatus op het [Dashboard voor servicestatus: Azure](https://azure.microsoft.com/status/)controleren.

### <a name="option-2-copy-data-from-secondary"></a>Optie 2: Gegevens uit secundaire kopiëren

Als u ervoor hebt gekozen [leestoegang geografische-redundante opslag (AB-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (aanbevolen) voor uw opslag-accounts, hebt u alleen toegang tot uw gegevens uit de tweede regio. U kunt extra kunt gebruiken, zoals [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)en de [bibliotheek van de verplaatsing van Azure-gegevens](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) uit de tweede regio kopiëren naar een ander opslag-account in een unimpacted regio en wijs vervolgens uw toepassingen aan dat account opslag voor beide lezen en schrijven van beschikbaarheid.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Wat u kunt verwachten als een opslag storing

Als u ervoor hebt gekozen voor [geografische-redundante opslag (GRS)](storage-redundancy.md#geo-redundant-storage) of [leestoegang geografische-redundante opslag (AB-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (aanbevolen), blijven Azure opslagruimte uw gegevens behouden duurzame in twee regio's (primaire en secundaire). In beide regio's houdt Azure Storage voortdurend meerdere kopieën van uw gegevens.

Wanneer een regionale noodgevallen van invloed is op uw primaire regio, wordt eerst geprobeerd om de service in dat gebied te herstellen. Afhankelijk van de aard van de noodgevallen en de gevolgen, in sommige soms we mogelijk niet herstellen van de primaire regio. Op dat moment wordt we een geografische-failover uitvoeren. De gegevens voor cross-regio replicatie is een asynchroon proces waarbij een vertraging kunt uitgevoerd, dus is het mogelijk dat de wijzigingen die nog niet zijn gerepliceerd naar de tweede regio mogelijk verloren. U kunt de ["laatste synchronisatie Time" van uw account opslagruimte](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) u kunt gegevens over de status herhaling zoeken.

Een paar punten met betrekking tot de opslag geografische-failover-ervaring:

-   Opslag geografische-failover wordt alleen worden veroorzaakt door het team Azure Storage: Er is geen klant actie vereist.

-   Uw bestaande eindpunten van de service opslag voor BLOB's, tabellen, wachtrijen en bestanden ongewijzigd blijft na het foutherstel. de DNS-vermelding moet worden bijgewerkt om te schakelen tussen de primaire regio de tweede regio.

-   Voordat en tijdens een de geografische-overname, hoeft u niet schrijftoegang uw account opslag vanwege de gevolgen van de noodgevallen maar u kunt nog steeds lezen uit de secundaire als uw opslag-account is geconfigureerd als AB-GRS.

-   Wanneer de geografische-overname is voltooid en de DNS-wijzigingen doorgegeven, worden uw lees- en schrijftoegang tot uw account opslag wordt hervat. ["Laatste geografische Failover Time" van uw opslag-account](https://msdn.microsoft.com/library/azure/ee460802.aspx) als u meer informatie wilt, kunt u zoeken.

-   Na de overname, uw opslagruimte-account wordt volledig werkt, maar in een "anders werken" status, zoals deze wordt daadwerkelijk gehost in een gebied zelfstandige met niet mogelijk geografische-herhaling. Als u wilt dit risico beperken door we herstellen van de oorspronkelijke primaire regio en voer een geografische-failback als u wilt herstellen van de oorspronkelijke staat. Als het oorspronkelijke primaire gebied onherstelbare is, wordt we nog een secundaire regio toewijzen.
Raadpleeg het artikel over het teamblog opslagruimte over [redundantieopties en AB-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)voor meer informatie over de infrastructuur van Azure Storage geografische herhaling.

##<a name="best-practices-for-protecting-your-data"></a>Aanbevolen procedures voor het beveiligen van uw gegevens

Er zijn enkele aanbevolen benaderingen back-up van uw opslaggegevens regelmatig.

-   VM schijven – gebruik van de [back-up van Azure-service](https://azure.microsoft.com/services/backup/) back-up de VM schijven die worden gebruikt door uw Azure virtuele machines.

-   Blok BLOB's – maken een [momentopname](https://msdn.microsoft.com/library/azure/hh488361.aspx) van elk blok blob of de BLOB's naar een andere opslag-account in een ander gebied met [AzCopy](storage-use-azcopy.md) [Azure PowerShell](storage-powershell-guide-full.md)of de [bibliotheek van de verplaatsing van Azure-gegevens](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)kopiëren.

-   Tabellen – gebruik [AzCopy](storage-use-azcopy.md) om de gegevens in een tabel exporteren naar een ander opslag-account in een andere regio.

-   Bestanden – gebruik [AzCopy](storage-use-azcopy.md) of [Azure PowerShell](storage-powershell-guide-full.md) kopiëren van uw bestanden naar een ander opslag-account in een andere regio.
