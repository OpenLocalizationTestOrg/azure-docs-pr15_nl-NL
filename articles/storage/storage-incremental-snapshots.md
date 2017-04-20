<properties
    pageTitle="Incrementele momentopnamen gebruiken voor back-up en herstellen van Azure virtuele machines | Microsoft Azure"
    description="Een aangepaste oplossing voor het back-up en herstellen van uw Azure virtuele machines schijven met incrementele momentopnamen maken."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Back-up van Azure virtuele machines schijven met incrementele momentopnamen

## <a name="overview"></a>Overzicht

Azure opslagruimte biedt de mogelijkheid aan momentopnamen van BLOB's. De staat blob vastleggen momentopnamen op dat moment. In dit artikel wordt beschreven een scenario van hoe u de back-ups van virtuele machine schijven met momentopnamen kunt bijhouden. U kunt deze methodologie kunt gebruiken wanneer u niet wilt gebruiken Azure back-up en herstel-Service en wilt maken van een aangepaste back-up strategie voor uw VM schijven.

Azure virtuele machines schijven worden opgeslagen als BLOB van pagina's in Azure opslag. Aangezien we over back-up strategie voor VM schijven in dit artikel spreekt, wordt we die verwijzen naar momentopnamen in de context van BLOB van pagina's. Meer informatie over momentopnamen, raadpleegt u voor het [maken van een momentopname van een Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Wat is een momentopname?

Een momentopname blob is een alleen-lezen-versie van een blob die op een punt in tijd is geregistreerd. Wanneer u een momentopname hebt gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. Momentopnamen zelf een formule back-up van een blob zoals deze wordt weergegeven op een moment. Tot REST versie 2015-04-05 had u de mogelijkheid om te kopiëren volledige momentopnamen. Met de andere versie 2015-07-08 en hoger, u kunt u ook incrementele momentopnamen kopiëren.

## <a name="full-snapshot-copy"></a>Volledige momentopname kopiëren

Momentopnamen kunnen worden gekopieerd naar een ander opslag-account als een blob wilt behouden back-ups van de basis blob. U kunt ook een momentopname kopiëren via de basis blob, dat wil zeggen als de blob naar een eerdere versie herstellen. Wanneer u een momentopname van één opslag-account wordt gekopieerd naar een andere, wordt deze beslaan dezelfde ruimte als de basis pagina blob. Daarom kopiëren hele momentopnamen van de ene opslag-account naar een andere traag zijn en ook veel ruimte in de doel-opslag-account gebruikt.

>[AZURE.NOTE] Als u de basis blob naar een andere bestemming kopieert, wordt de momentopnamen van de blob worden niet gekopieerd samen met deze. Op dezelfde manier als u een grondtal blob met een kopie overschrijft, momentopnamen die is gekoppeld aan de basis blob worden niet beïnvloed en blijven intact onder de naam van het grondtal blob.

### <a name="back-up-disks-using-snapshots"></a>Een back-up schijven met momentopnamen

Als een back-up strategie voor uw schijven VM, kunt u periodieke momentopnamen van de schijf of pagina blob en kopieert u deze naar een ander opslag account met hulpprogramma's zoals [Kopie Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) bewerking of [AzCopy](storage-use-azcopy.md). U kunt een momentopname kopiëren naar een blob bestemming pagina met een andere naam. De resulterende waarde van doel pagina blob is een schrijfbare pagina blob en niet een momentopname. Verderop in dit artikel wordt beschreven stappen om back-ups van virtuele machine schijven met momentopnamen te zetten.

### <a name="restore-disks-using-snapshots"></a>Schijven met momentopnamen herstellen

Wanneer het tijd om te zetten de schijf naar een vorige stabiele versie opgenomen in een van de back-up momentopnamen is, kunt u een momentopname via de blob grondtal pagina kopiëren. Nadat de momentopname wordt gepromoveerd tot de basis pagina blob, de momentopname blijft, maar de bron wordt overschreven door een kopie die kan worden zowel gelezen en geschreven. Verderop in dit artikel worden stappen voor het herstellen van een eerdere versie van de schijf van de momentopname wordt beschreven.

### <a name="implementing-full-snapshot-copy"></a>Volledige momentopname kopiëren implementeren

U kunt een kopie van de volledige momentopname volgt, implementeren

-   Eerst een momentopname van het grondtal blob de bewerking [Momentopname Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) te gebruiken.
-   Kopieer vervolgens de momentopname naar een doel-opslag-account met behulp van [Blob kopiëren](https://msdn.microsoft.com/library/azure/dd894037.aspx).
-   Herhaal deze procedure voor het behoud van back-ups van uw grondtal blob.

## <a name="incremental-snapshot-copy"></a>Incrementele momentopname kopiëren

De nieuwe functie in [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API kunt u veel beter back-up van de momentopnamen van de pagina BLOB's of schijven. De API resulteert in een lijst van wijzigingen tussen de basis blob en de momentopnamen. Hierdoor wordt de hoeveelheid opslagruimte op het back-account gebruikt. De API ondersteunt BLOB van pagina's op de Premium-opslag, evenals standaardopslag. Met deze API, kunt u sneller en efficiënt back-oplossingen bouwen voor Azure VMs. Dit is alleen beschikbaar met de REST-versie 2015-07-08 en hoger.

Incrementele momentopname kopiëren, kunt u kopiëren van één opslag-account naar een ander het verschil tussen,

-   Basis blob en de momentopname of
-   De twee momentopnamen van de basis blob

De volgende voorwaarden is voldaan, opgegeven

- De label is gemaakt of hoger voor Jan-1-2016.
- De blob is niet overschreven met [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) of [Blob kopiëren](https://msdn.microsoft.com/library/azure/dd894037.aspx) tussen twee momentopnamen.


**Opmerking**: deze functie is beschikbaar voor Premium en Standard BLOB van Azure-pagina's.

Wanneer u over een aangepaste back-met momentopnamen, de momentopnamen van de ene opslag account kopiëren naar een andere kunnen heel lang en een groot aantal opslagruimte in beslag neemt. In plaats van de volledige momentopname kopiëren naar een back-up opslaan-account, kunt u het verschil tussen opeenvolgende momentopnamen aan een pagina back-up-blob schrijven. Hierdoor wordt de tijd om te kopiëren en ruimte voor de opslag van back-ups aanzienlijk verminderd.

### <a name="implementing-incremental-snapshot-copy"></a>Implementatie van incrementele momentopname kopiëren

U kunt incrementele momentopname kopiëren volgt, implementeren

-   Een momentopname van het grondtal blob [Momentopname Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx)gebruiken.
-   De momentopname kopiëren naar de doel-back-up opslaan-account via [Kopiëren Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Dit is de pagina back-up-blob. Een momentopname van deze pagina back-up-label en opslaan in de back-account.
-   Nog een momentopname van het grondtal blob momentopname Blob gebruiken.
-   Het verschil tussen de eerste en tweede momentopnamen van basistoewijzing blob met [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx)krijgen. Gebruik de nieuwe parameter **prevsnapshot** om op te geven van de DateTime-waarde van de opname die u wilt ontvangen van het verschil met. Als deze parameter aanwezig is, bevat het antwoord REST alleen op de pagina's die zijn gewijzigd tussen doel momentopname en vorige momentopname inclusief wissen pagina's.
-   Gebruik [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) om deze wijzigingen toepassen op de pagina back-up-blob.
-   Ten slotte een momentopname van de pagina back-up-blob en opslaan in de back-up opslaan-account.

In de volgende sectie, maar wij beschrijven uitgebreider hoe u kunt voor het behoud van back-ups van schijven via incrementele momentopname kopiëren

## <a name="scenario"></a>Scenario

In deze sectie wordt een scenario waarbij een aangepaste back-up strategie voor VM schijven met momentopnamen worden beschreven.

Houd rekening met een VM van de Azure Active Directory-reeks met een premium opslag P30 schijf die is gekoppeld. De P30 schijf *mypremiumdisk* genoemd wordt opgeslagen in een premium-opslag-account *mypremiumaccount*genoemd. Een standaard-opslag-account genoemd *mybackupstdaccount* wordt gebruikt voor het opslaan van de back-up van *mypremiumdisk*. Willen we een momentopname van *mypremiumdisk* elke 12 uur behouden.

Voor meer informatie over het maken van opslag-account en schijven, raadpleegt u [over Azure opslag-accounts](storage-create-storage-account.md).

Meer informatie over een back-up Azure VMs, raadpleegt u [van plan bent Azure VM back-ups](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Stappen voor het beheren van back-ups van een schijf met incrementele momentopnamen

De onderstaande stappen wordt momentopnamen van *mypremiumdisk* en voor het behoud van de back-ups in *mybackupstdaccount*. De back-up is een standaardpagina-blob *mybackupstdpageblob*genoemd. De pagina back-up-blob worden altijd dezelfde staat als de laatste momentopname van *mypremiumdisk*doorgevoerd.

1.  U maakt eerst het blob back-pagina voor de premium-opslag-schijf. Moet u dit doen een momentopname van *mypremiumdisk* genoemd *mypremiumdisk_ss1*.
2.  Kopieer deze momentopname naar mybackupstdaccount als een pagina blob *mybackupstdpageblob*genoemd.
3.  Een momentopname van *mybackupstdpageblob* *mybackupstdpageblob_ss1*, met [Momentopname Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) genoemd en sla deze op *mybackupstdaccount*.
4.  Tijdens het venster back-up maken van een andere momentopname van *mypremiumdisk*, zeg *mypremiumdisk_ss2*en opslaan in *mypremiumaccount*.
5.  De incrementele wijzigingen tussen de twee momentopnamen, *mypremiumdisk_ss2* en *mypremiumdisk_ss1*, [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) op *mypremiumdisk_ss2* met **prevsnapshot** parameter is ingesteld op het tijdstempel van *mypremiumdisk_ss1*krijgen. Deze incrementele wijzigingen naar de pagina back-up blob *mybackupstdpageblob* in *mybackupstdaccount*schrijven. Als er verwijderde bereiken in de incrementele wijzigingen, moeten u ze vanaf de pagina back-up-blob wissen. Gebruik [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) incrementele wijzigingen naar de pagina back-up-blob schrijven.
6.  Een momentopname van de pagina back-up blob *mybackupstdpageblob*, *mybackupstdpageblob_ss2*genoemd. De vorige momentopname *mypremiumdisk_ss1* uit premium opslag account verwijderen.
7.  Herhaal stap 4-6 elke back-venster. Op deze manier kunt u voor het behoud van back-ups van *mypremiumdisk* in een standaard-opslag-account.

![Een back-up schijf met incrementele momentopnamen](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Stappen voor het herstellen van een schijf uit momentopnamen

De onderstaande stappen wordt premium schijf, *mypremiumdisk* naar een eerdere momentopname van de back-up opslaan account *mybackupstdaccount*herstellen.

1.  Identificeer de komma in tijd die u wilt de premium-schijf te herstellen. Stel dat die momentopname *mybackupstdpageblob_ss2*, die is opgeslagen in de back-up opslaan account *mybackupstdaccount*is.
2.  In mybackupstdaccount, één niveau verhogen de momentopname *mybackupstdpageblob_ss2* als de nieuwe pagina voor back-up grondtal blob *mybackupstdpageblobrestored*.
3.  Een momentopname van deze pagina voor herstelde back-up blob, *mybackupstdpageblobrestored_ss1*genoemd.
4.  Kopieer de herstelde pagina blob *mybackupstdpageblobrestored* van *mybackupstdaccount* naar *mypremiumaccount* als de nieuwe premium schijf *mypremiumdiskrestored*.
5.  Een momentopname van *mypremiumdiskrestored*, *mypremiumdiskrestored_ss1* om toekomstige incrementele back-ups genoemd.
6.  Punt Active Directory reeks VM naar de herstelde *mypremiumdiskrestored* schijf en loskoppelen van de oude *mypremiumdisk* uit de VM.
7.  Het back-up-proces wordt beschreven in de vorige sectie voor de herstelde schijf *mypremiumdiskrestored*, de *mybackupstdpageblobrestored* gebruiken als de pagina back-up-blob start.

![Schijf herstellen uit momentopnamen](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het maken van momentopnamen maken van een blob en planning VM back-infrastructuur van het gebruik van de onderstaande koppelingen.

- [Een momentopname van een Blob maken](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [De infrastructuur van uw VM back-ups plannen](../backup/backup-azure-vms-introduction.md)
