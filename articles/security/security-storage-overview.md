<properties
   pageTitle="Overzicht van de beveiliging Azure opslag | Microsoft Azure"
   description=" Azure opslag is de cloud-opslag-oplossing voor moderne toepassingen die afhankelijk zijn van de levensduur, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten. Dit artikel bevat een overzicht van de core Azure beveiligingsfuncties die kunnen worden gebruikt met Azure opslagmedia. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Azure opslag beveiliging-overzicht

Azure opslag is de cloud-opslag-oplossing voor moderne toepassingen die afhankelijk zijn van de levensduur, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten. Azure opslagruimte biedt een uitgebreide reeks beveiligingsmogelijkheden:

- Het account opslag kan worden beveiligd met toegangsbeheer op basis van rollen en Azure Active Directory.
- Gegevens kunnen worden beveiligd tijdens overdracht tussen een toepassing en Azure met behulp van aan de clientzijde versleuteling, HTTPS of SMB 3.0.
- Gegevens kan worden ingesteld op automatisch worden gecodeerd als geschreven met Azure Storage opslag Service-versleuteling gebruikt.
- OS en gegevens schijven die worden gebruikt door virtuele machines kunnen worden ingesteld te coderen met Azure schijfversleuteling.
- Gedelegeerd toegang tot de gegevensobjecten in Azure-opslag kan worden verleend gedeeld Access handtekeningen gebruiken.
- De verificatiemethode gebruikt door iemand wanneer ze toegang hebben tot opslag kan worden bijgehouden door middel van opslag analyses.

Zie voor een gedetailleerde beveiliging in Azure opslagruimte wilt bekijken, de [opslag van Azure beveiligingshandleiding](../storage/storage-security-guide.md). Deze handleiding bevat een uitgebreide Kennismaking in de beveiligingsfuncties van Azure Storage zoals opslagruimte account toetsaanslagen gegevensversleuteling tijdens overdracht en klik in de rest en opslag analytics.

Dit artikel bevat een overzicht van Azure beveiligingsfuncties die kunnen worden gebruikt met Azure opslagmedia. Koppelingen zijn opgegeven naar artikelen met details van elke functie zo geven kunt u meer informatie.

Hier volgen de core-functies om te worden beschreven in dit artikel:

- Toegangsbeheer op basis van rollen
- Gedelegeerd toegang tot objecten voor gegevensopslag
- Versleuteling tijdens overdracht
- Versleuteling bij rest/opslag Service-versleuteling
- Azure schijfversleuteling
- Azure belangrijke kluis

## <a name="role-based-access-control-rbac"></a>Rolgebaseerd toegangsbeheer RBAC)

U kunt uw account opslagruimte met toegangsbeheer op basis van rollen (RBAC) beveiligen. Beperken van toegang op basis van de beveiliging [wilt weten](https://en.wikipedia.org/wiki/Need_to_know) en [laagste bevoegdheidsniveau](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beginselen is dwingende voor organisaties die de wilt afdwingen beveiligingsbeleid voor apparaten voor toegang tot gegevens. Deze toegangsrechten zijn verleend door de juiste RBAC rol toewijzen aan groepen en toepassingen op een bepaald bereik. U kunt [ingebouwde RBAC rollen](../active-directory/role-based-access-built-in-roles.md), zoals opslag Account Inzender, bevoegdheden toewijzen aan gebruikers.

Meer informatie:

- [Azure Active Directory Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Gedelegeerd toegang tot objecten voor gegevensopslag

Een gedeelde access-handtekening (SA's) gedelegeerd toegang geeft tot bronnen in uw account opslag. De SA's betekent dat kunt u een client beperkte machtigingen aan objecten in uw account opslagruimte voor een bepaalde termijn van tijd en met een opgegeven reeks machtigingen verlenen. Zonder dat u moet het delen van uw account toegangstoetsen kunt u deze beperkte machtigingen verlenen. De SA's is een URI die in de queryparameters alle informatie die nodig zijn voor geverifieerde toegang aan een resourceweergave opslag omsluit. Voor toegang tot opslag resources met de SA's, moet de client alleen om door te geven in de SA's naar de juiste constructor of methode.

Meer informatie:

- [Wat is het model SA's?](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Maken en gebruiken van een SA's met-blobopslag](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
Versleuteling tijdens overdracht is een methode om gegevens te beschermen wanneer deze worden verzonden via netwerken. U kunt met Azure opslagmedia beveiligen gebruik van gegevens:

- [Transport niveau versleuteling](../storage/storage-security-guide.md#encryption-in-transit), zoals HTTPS wanneer u gegevens in- of uitzoomen op Azure Storage overbrengen.
- [Kabel versleuteling](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals SMB 3.0-versleuteling voor bestandsshares Azure.
- [Aan de clientzijde versleuteling](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), de gegevens versleutelen voordat deze wordt overgebracht op te slaan en tot de versleutelde gegevens na het overbrengen geen opslagruimte meer.

Meer informatie over aan de clientzijde versleuteling:

- [Aan de clientzijde versleuteling voor Microsoft Azure-opslag](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud security reeks besturingselementen: coderen gegevens tijdens overdracht](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Versleuteling in rust

Voor veel organisaties is [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) een verplichte stap naar gegevensprivacy, naleving en gegevens te garanderen. Er zijn drie Azure functies waarmee codering van gegevens die is "at rest':

- [Opslag Service versleuteling](../storage/storage-security-guide.md#encryption-at-rest) kunt u dat de storage-service gegevens automatisch versleutelen bij het schrijven van deze met Azure Storage aanvragen.
- [Aan de clientzijde versleuteling](../storage/storage-security-guide.md#client-side-encryption) bevat ook de functie van versleuteling in rust.
- [Azure schijfversleuteling](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) kunt u de OS schijven en gegevensschijven die worden gebruikt door een VM IaaS versleutelen.

Meer informatie over de opslag Service versleuteling:

- [Azure opslag Service versleuteling](https://azure.microsoft.com/services/storage/) is beschikbaar voor [Azure-blobopslag](https://azure.microsoft.com/services/storage/blobs/). Zie voor meer informatie over andere opslagtypen Azure, [bestand](https://azure.microsoft.com/services/storage/files/), [schijf (Premium opslag)](https://azure.microsoft.com/services/storage/premium-storage/) [tabel](https://azure.microsoft.com/services/storage/tables/)en [wachtrij](https://azure.microsoft.com/services/storage/queues/).
- [Azure opslag Service versleuteling voor gegevens in rust](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure schijfversleuteling

Azure schijfversleuteling voor virtuele machines (VMs) kunt u organisatie-beveiliging en nalevingsvereisten door te coderen van uw schijven VM (inclusief opstarten en gegevens schijven) met sleutels en beleidsregels die u in [Azure toets kluis bepalen](https://azure.microsoft.com/services/key-vault/).

Schijfversleuteling voor VMs werkt voor Linux en Windows-besturingssystemen. Toets kluis wordt gebruikt om te beschermen, beheren en het gebruik van uw schijf versleutelingssleutels controleren. Alle gegevens in uw schijven VM is versleuteld in rust met behulp van de standaard-versleutelingstechnologie in uw Azure Storage-accounts. De schijfversleuteling-oplossing voor Windows is gebaseerd op [Microsoft BitLocker stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx)en de Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Meer informatie:

- [Azure schijfversleuteling voor Windows en Linux IaaS virtuele Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure belangrijke kluis

[Azure toets kluis](https://azure.microsoft.com/services/key-vault/) Azure schijf-versleuteling gebruikt om te bepalen en schijf versleuteling toetsen en geheimen in uw belangrijkste kluis-abonnement en ervoor zorgen dat dat alle gegevens in de virtuele machine schijven worden gecodeerd in rust in uw Azure-opslag beheren. U kunt toets kluis controle toetsen en het gebruik van beleid.

Meer informatie:

- [Wat is Azure-toets kluis?](../key-vault/key-vault-whatis.md)
- [Aan de slag met Azure-toets kluis](../key-vault/key-vault-get-started.md)
