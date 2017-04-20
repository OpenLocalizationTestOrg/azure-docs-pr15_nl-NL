<properties
    pageTitle="Azure opslag Service versleuteling voor gegevens in rust | Microsoft Azure"
    description="Azure opslag Service versleuteling gebruiken voor het coderen van uw Azure-blobopslag aan de service wanneer de gegevens op te slaan en deze ontsleutelen wanneer u de gegevens ophaalt."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure opslag Service versleuteling voor gegevens in rust

Azure opslag Service versleuteling (SSE) voor gegevens in rust kunt u beveiligen en beschermen van uw gegevens om te voldoen aan uw organisatie-beveiliging en naleving verplichtingen. Met deze functie Azure opslag automatisch uw gegevens voordat u persistent maken met storage worden gecodeerd en ontsleutelt vóór ophalen. De versleuteling, decoderen en key management zijn volledig doorzichtig aan gebruikers.

De volgende secties vindt gedetailleerde instructies over het gebruik van de Service-versleuteling opslag-functies, evenals de ondersteunde scenario's en de gebruiker ervaringen.

## <a name="overview"></a>Overzicht

Azure opslagruimte biedt een uitgebreide set van beveiligingsmogelijkheden voor waarmee samen ontwikkelaars op beveiligde toepassingen. Gegevens kunnen worden beveiligd tijdens overdracht tussen een toepassing en Azure met behulp van [Aan de clientzijde versleuteling](storage-client-side-encryption.md), HTTPs of SMB 3.0. Opslag Service versleuteling biedt versleuteling in rust, afhandeling versleuteling decoderen en key management in een volledig doorzichtig manier. Alle gegevens worden gecodeerd met 256-bits [AES-versleuteling gebruikt](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blok versleuteling beschikbaar.

SSE werkt door te coderen van de gegevens als deze is geschreven met Azure Storage, en kan worden gebruikt voor blok BLOB's, BLOB van pagina's en BLOB's toevoegt. Dit werkt het volgende:

-   Algemene opslag accounts en Blob storage-accounts
-   Standaard opslag en Premium-opslag 
-   Alle redundantie worden geëffend (LRS, ZRS, GRS, AB-GRS)
-   Azure resourcemanager opslag accounts (maar niet klassieke) 
-   Alle regio 's

Als u wilt in- of uitschakelen van opslag Service versleuteling voor een account opslag, meld u aan bij de [portal van Azure](https://azure.portal.com) en selecteer een account opslag. Klik op het blad instellingen, zoek naar de sectie Blob-Service, zoals in deze schermafbeelding en versleuteling op.

![Optie voor portal schermopname met-codering](./media/storage-service-encryption/image1.png)

Nadat u de instelling versleuteling op, kunt u deze kunt inschakelen of uitschakelen van opslag Service versleuteling.

![Eigenschappen van de portal schermopname met-versleuteling](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Scenario's voor versleuteling

Opslag Service versleuteling kan worden ingeschakeld op een opslagniveau-account. De volgende klant-scenario's wordt ondersteund:

-   Versleuteling van blok BLOB's, toevoegen BLOB's en pagina BLOB's.

-   Codering van gearchiveerde VHD's en sjablonen Azure vanuit on-premises heeft aangebracht.

-   Codering van onderliggende OS en gegevens schijven voor IaaS VMs gemaakt met behulp van uw VHD's.

SSE heeft de volgende beperkingen:

-   Versleuteling klassieke opslag accounts wordt niet ondersteund.

-   Versleuteling klassieke opslag accounts gemigreerd naar resourcemanager opslag accounts wordt niet ondersteund.

-   Bestaande gegevens - SSE alleen nieuwe gegevens worden gecodeerd nadat de versleuteling is ingeschakeld. Als u bijvoorbeeld een nieuwe Resource Manager opslag-account maken maar versleuteling niet inschakelt en klikt u BLOB's of gearchiveerde VHD's uploaden naar dat account opslag en zet vervolgens SSE, wordt deze BLOB's niet worden gecodeerd tenzij worden herschreven of gekopieerd.

-   Marketplace - ondersteuning: inschakelen versleuteling van VMs gemaakt op basis van het gebruik van de [Azure-portal](https://portal.azure.com), PowerShell en CLI Azure Marketplace. De afbeelding VHD, blijven de versleutelde; echter worden een schrijft uitgevoerd nadat de VM heeft of omhoog gecodeerd.

-   Tabel, wachtrijen, en gegevens van de bestanden niet worden versleuteld.

##<a name="getting-started"></a>Aan de slag

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Stap 1: [een nieuwe opslag-account maken](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Stap 2: Schakel codering.

U kunt met behulp van de [Azure portal](https://portal.azure.com)versleuteling inschakelen.

> [AZURE.NOTE] Als u wilt via een programma in- of uitschakelen van de opslagruimte Service-codering in een opslag-account, kunt u de [Provider REST API van Azure opslag Resource](https://msdn.microsoft.com/library/azure/mt163683.aspx), de [Opslag Resource Provider clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md)of de [Azure CLI](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Stap 3: Gegevens kopiëren naar opslag-account

Als u SSE inschakelen voor een account voor de opslag en schrijf vervolgens BLOB's aan dat account opslag, wordt de BLOB's worden gecodeerd. Een BLOB's al bevinden in dat account opslag niet gecodeerd totdat ze worden herschreven. U kunt de gegevens uit één opslag-account naar een met SSE versleuteld, kopiëren of zelfs SSE inschakelen en de BLOB's uit een container naar een andere kopiëren naar ervoor dat vorige gegevens worden gecodeerd. U kunt een van de volgende hulpmiddelen gebruiken om dit te doen.

#### <a name="using-azcopy"></a>AzCopy gebruiken

AzCopy is een opdrachtregel hulpprogramma ontworpen voor het kopiëren van gegevens en naar Microsoft Azure Blob, bestand en tabel opslag eenvoudige opdrachten gebruiken met optimale prestaties van Windows. U kunt dit gebruiken om te kopiëren van uw BLOB's van de ene opslag-account naar een andere waarop SSE ingeschakeld. 

Ga naar [overbrengen van gegevens met het hulpprogramma AzCopy voor voor](storage-use-azcopy.md)meer informatie.

#### <a name="using-the-storage-client-libraries"></a>Gebruik van de opslagruimte Client-bibliotheken

U kunt blob-gegevens kopiëren naar en vanuit blobopslag of tussen met onze uitgebreide set opslag clientbibliotheken, inclusief .NET, C++, Java, Android, Node.js, PHP, Python en Ruby opslag-accounts.

Ga naar onze [aan de slag met Azure-blobopslag .NET met](storage-dotnet-how-to-use-blobs.md)meer informatie.

#### <a name="using-a-storage-explorer"></a>Via een Opslagverkenner

U kunt een explorer opslag opslag-accounts maken, uploaden en downloaden van gegevens, inhoud van BLOB's bekijken en navigeren door mappen. U kunt een van de volgende BLOB's uploaden naar uw account opslagruimte met ingeschakelde codering. Met enkele opslag Explorer-vensters vertegenwoordigen, kunt u ook gegevens uit bestaande blobopslag kopiëren naar een andere container in de opslagruimte-account of een nieuw account voor de opslag dat SSE ingeschakeld heeft.

Ga naar [Azure opslag Explorers](storage-explorers.md)meer informatie.

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Stap 4: De status van de versleutelde gegevens opvragen

Een bijgewerkte versie van de Client voor opslag-bibliotheken er is geïmplementeerd waarmee u de status van een object om te bepalen als deze is versleuteld al dan niet query. Voorbeelden worden, toegevoegd aan dit document in de nabije toekomst.

Ondertussen kunt u de [Eigenschappen van Account krijgen](https://msdn.microsoft.com/library/azure/mt163553.aspx) om te controleren of de opslag-account ingeschakeld versleuteling heeft of weergeven van de eigenschappen van de account opslag in de portal van Azure bellen.

##<a name="encryption-and-decryption-workflow"></a>Versleuteling en decoderen werkstroom

Hier volgt een korte beschrijving van de werkstroom/ontsleutelen:

-   De klant kunt versleuteling voor de opslag-account.

-   Wanneer de klant schrijft nieuwe gegevens (Blob plaatsen, blokkeren plaatsen, pagina plaatsen, enzovoort) is met Blob storage; elke schrijven is versleuteld met 256-bits [AES-versleuteling gebruikt](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blok versleuteling beschikbaar.

-   Wanneer de klant moet toegang tot gegevens (Blob ophalen, enzovoort), is gegevens automatisch ontsleuteld om terug te gaan naar de gebruiker.

-   Als versleuteling is uitgeschakeld, nieuwe schrijft niet langer zijn versleuteld en bestaande versleutelde gegevens blijft versleutelde totdat herschreven door de gebruiker. Terwijl versleuteling is ingeschakeld, kunt u schrijft met Blob storage worden gecodeerd. De status van gegevens met de gebruiker schakelen tussen het in-of uitschakelen versleuteling voor de opslag-account niet gewijzigd.

-   Alle versleutelingssleutels zijn opgeslagen, versleuteld en beheerd door Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Veelgestelde vragen over opslag Service versleuteling voor gegevens in rust

**V: ik hebt een bestaande klassieke opslag-account. Kan ik SSE inschakelen op is geïnstalleerd?**

A: Nee, SSE wordt alleen ondersteund op resourcemanager opslag-accounts.

**V: hoe kan ik gegevens versleutelen in mijn klassieke opslag-account?**

A: u kunt een nieuwe Resource Manager opslag-account maken en uw gegevens met behulp van [AzCopy](storage-use-azcopy.md) van uw bestaande klassieke opslag-account bij uw account voor opslag van gemaakte resourcemanager kopiëren. 

Een andere optie is uw account klassieke opslag migreren naar een Resource beheren opslag-account. Zie [Platform ondersteund migratie van IaaS Resources uit klassieke naar resourcemanager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/)voor meer informatie.

**V: ik heb een bestaande resourcemanager opslag-account. Kan ik SSE inschakelen op is geïnstalleerd?**

A: Ja, maar alleen nieuwe geschreven BLOB's worden gecodeerd. Deze niet terug te gaan en coderen van gegevens die al aanwezig is. 

**V: ik wilt versleutelen van de huidige gegevens in een bestaand resourcemanager opslag-account?**

A: u kunt SSE inschakelen op elk gewenst moment in een resourcemanager opslag-account. BLOB's die al aanwezig waren worden echter niet gecodeerd. Als u wilt versleutelen die BLOB's, kunt u deze naar een andere naam of een andere container te kopiëren en verwijder de niet-versleutelde versies.

**V: ik gebruik Premium opslag; kan ik SSE gebruiken?**

A: Ja, SSE wordt ondersteund op zowel de standaardopslag en de Premium-opslag.

**V: als ik een nieuw account voor de opslag maken en SSE inschakelen en daarna maken van een nieuwe VM met dat account opslag, betekent dit dat mijn VM is versleuteld?**

A: Ja. Een schijven gemaakt die gebruikmaken van het nieuwe account voor de opslag worden gecodeerd, zo lang maken als ze zijn gemaakt nadat SSE is ingeschakeld. Als de VM is gemaakt met van Azure Marketplace, blijft de afbeelding VHD versleutelde; echter worden een schrijft uitgevoerd nadat de VM heeft of omhoog gecodeerd.

**V: kan ik nieuwe opslag-accounts maken met SSE met Azure PowerShell en Azure CLI ingeschakeld?**

A: Ja.

**V: hoe veel meer kost Azure opslag als SSE is ingeschakeld?**

A: Er is geen extra kosten.

**V: wie beheert de toetsen versleuteling?**

A: de toetsen worden beheerd door Microsoft.

**V: kan ik mijn eigen toetsen versleuteling gebruiken?**

A: we werken aan het leveren van mogelijkheden voor klanten om hun eigen versleuteling toetsen weer te geven.

**V: kan ik toegang tot de toetsen versleuteling intrekken?**

A: niet op dit moment; de gebruikte toetsen worden volledig beheerd door Microsoft.

**V: is standaard ingeschakeld SSE wanneer ik een nieuw account voor de opslag maken?**

A: SSE is niet ingeschakeld al dan niet standaard; u kunt de Azure-portal te kunnen gebruiken. U kunt ook via programmacode deze functie te gebruiken op de opslag Resource Provider REST API inschakelen.

**V: hoe is dit verschil tussen Azure stationsversleuteling?**

A: deze functie wordt gebruikt voor het coderen van gegevens in Azure-blobopslag. De schijf-codering van Azure gebruikt OS en gegevens schijven in IaaS VMs versleutelen. Ga naar onze [Opslagruimte Security Guide](storage-security-guide.md)voor meer informatie.

**V: Wat gebeurt er als ik SSE, en klik vervolgens gaat u en inschakelen Azure schijfversleuteling op de schijven?**

A: dit werkt naadloos samen. Uw gegevens worden gecodeerd door beide methoden.

**V: Mijn opslag-account is ingesteld geografische-redundante worden gerepliceerd. Als ik SSE inschakelt, worden mijn redundante kopie ook gecodeerd?**

A: Ja, alle exemplaren van het account opslag zijn versleuteld en alle redundantieopties – lokaal overtollige opslag (LRS), Zone-redundante opslag (ZRS) geografische redundante opslag (GRS) en leestoegang geografische redundante opslag (AB-GRS) – worden ondersteund.

**V: ik kan geen versleuteling inschakelen op mijn account opslag.**

A: is het een resourcemanager opslag-account? Klassieke opslag accounts worden niet ondersteund. 

**V: is SSE alleen in bepaalde gebieden toegestaan?**

A: het SSE is beschikbaar in alle regio's. 

**V: hoe ik met iemand contact als ik problemen hebt of feedback wilt geven?**

A: Neem contact op met [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) voor eventuele problemen die betrekking hebben op Service-versleuteling opslag.

##<a name="next-steps"></a>Volgende stappen

Azure opslagruimte biedt een uitgebreide set van beveiligingsmogelijkheden voor waarmee samen ontwikkelaars op beveiligde toepassingen. Ga naar de [Opslag Security Guide](storage-security-guide.md)voor meer informatie.
