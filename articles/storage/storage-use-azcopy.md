<properties
    pageTitle="Gegevens kopiëren of verplaatsen met Storage met AzCopy | Microsoft Azure"
    description="Het hulpprogramma AzCopy gebruiken om gegevens verplaatsen of kopiëren naar of uit blob, tabel en de bestandsinhoud. Gegevens met Azure Storage uit lokale bestanden, kopiëren of gegevens binnen of tussen opslag-accounts. Uw gegevens eenvoudig migreren met Azure Storage."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen

## <a name="overview"></a>Overzicht

AzCopy is een opdrachtregel hulpprogramma ontworpen voor het kopiëren van gegevens en naar Microsoft Azure Blob, bestand en tabel opslag eenvoudige opdrachten gebruiken met optimale prestaties van Windows. U kunt gegevens van het ene object naar een andere kopiëren vanuit uw opslag-account of tussen opslag-accounts.

> [AZURE.NOTE] Deze handleiding wordt ervan uitgegaan dat u al bekend met [Azure Storage bent](https://azure.microsoft.com/services/storage/). Als dat niet zo is, leest de documentatie [Inleiding tot Azure Storage](storage-introduction.md) is handig. Belangrijker is, moet u [een account opslag](storage-create-storage-account.md#create-a-storage-account) maken om te kunnen aan de slag met AzCopy.

## <a name="download-and-install-azcopy"></a>Download en installeer AzCopy

### <a name="windows"></a>Windows

Download de [meest recente versie van AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac/Linux

AzCopy is niet beschikbaar voor Mac/Linux OSs. Azure CLI is echter een geschikt alternatief voor het kopiëren van gegevens naar en vanuit Azure Storage. Gelezen [met behulp van de Azure CLI met Azure-opslag](storage-azure-cli.md) voor meer informatie.

## <a name="writing-your-first-azcopy-command"></a>Uw eerste AzCopy-opdracht schrijven

De syntaxis van de eenvoudige voor AzCopy opdrachten luidt als volgt:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Open een opdrachtvenster en navigeer naar de map AzCopy installeren op uw computer - waar de `AzCopy.exe` uitvoerbare zich bevindt. Desgewenst kunt u de locatie van de AzCopy installatie toevoegen aan het systeempad van uw. Standaard AzCopy is geïnstalleerd naar `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` of `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

De volgende voorbeelden worden verschillende scenario's voor het kopiëren van gegevens aan en uit Microsoft Azure BLOB's, bestanden en de tabellen. Raadpleeg de sectie [AzCopy Parameters](#azcopy-parameters) voor een gedetailleerde uitleg van de parameters in elke steekproef.

## <a name="blob-download"></a>BLOB: downloaden

### <a name="download-single-blob"></a>Eén blob downloaden

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Opmerking Als de map `C:\myfolder` niet bestaat, maakt u deze en download AzCopy `abc.txt ` naar de nieuwe map.

### <a name="download-single-blob-from-secondary-region"></a>Eén blob downloaden uit de tweede regio secundaire

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Houd er rekening mee dat u leestoegang geografische-redundante opslag ingeschakeld nodig hebt.

### <a name="download-all-blobs"></a>Alle BLOB's downloaden

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Stel dat de volgende BLOB's bevinden zich in de opgegeven container:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Na het downloaden betrekking heeft, wordt de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Als u geen optie opgeeft `/S`, geen BLOB's worden gedownload.

### <a name="download-blobs-with-specified-prefix"></a>BLOB's met de opgegeven voorvoegsel downloaden

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Wordt ervan uitgegaan dat de volgende BLOB's bevinden zich in de opgegeven container. Alle BLOB's beginnen met het voorvoegsel `a` worden gedownload:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Na het downloaden betrekking heeft, wordt de map `C:\myfolder` bevat de volgende bestanden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Het voorvoegsel is van toepassing op de virtuele map, waarin het eerste deel van de naam van de blob uitmaakt. In het voorbeeld hierboven, de virtuele map niet overeenkomen met het opgegeven voorvoegsel, zodat deze niet is gedownload. Bovendien als de optie `\S` niet is opgegeven, AzCopy eventuele BLOB's niet gedownload.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>De tijd van de laatste wijziging geëxporteerde bestanden om in hetzelfde zijn als de bron BLOB's instellen

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

U kunt ook BLOB's uitsluiten van het downloaden betrekking heeft op basis van hun tijd van laatste wijziging. Als u wilt uitsluiten van BLOB's waarvan het laatste gewijzigd tijd is bijvoorbeeld dezelfde of nieuwer dan het doelbestand toevoegen de `/XN` optie:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Of als u wilt uitsluiten van BLOB's waarvan het laatste gewijzigd tijd is dezelfde of ouder zijn dan het doelbestand toevoegen de `/XO` optie:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: uploaden

### <a name="upload-single-file"></a>Bestand uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Als de opgegeven bestemmingscontainer niet bestaat, AzCopy maakt deze en upload het bestand erin.

### <a name="upload-single-file-to-virtual-directory"></a>Bestand uploaden naar virtuele map

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Als de opgegeven virtuele map niet bestaat, AzCopy wordt uploadt u het bestand als u wilt opnemen van de virtuele map in de naam (*bijvoorbeeld* `vd/abc.txt` in het bovenstaande voorbeeld).

### <a name="upload-all-files"></a>Alle bestanden uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Optie `/S` uploadt u de inhoud van de opgegeven map naar de Blob storage recursief, wat betekent dat alle submappen en de bijbehorende bestanden ook worden geüpload. Bijvoorbeeld, wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Na de uploadbewerking bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy wordt niet recursief uploaden. Na de uploadbewerking bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Bestanden die overeenkomen met opgegeven patroon uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Wordt ervan uitgegaan dat de volgende bestanden bevinden zich in map `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Na de uploadbewerking bevat de container de volgende bestanden:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Als u geen optie opgeeft `/S`, AzCopy wordt alleen uploaden BLOB's die niet bevinden zich in een virtuele map:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Geef het MIME-inhoudstype van een blob bestemming

AzCopy wordt standaard ingesteld van het type inhoud van een blob bestemming naar `application/octet-stream`. Begin met versie 3.1.0, kunt u expliciet opgeven het inhoudstype via de optie `/SetContentType:[content-type]`. Deze syntaxis Hiermee stelt u het inhoudstype voor alle BLOB's in een uploadbewerking.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Als u hebt opgegeven `/SetContentType` zonder een waarde, klikt u vervolgens AzCopy wordt elke blob of instellen van het bestand inhoudstype op basis van de bestandsextensie.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: kopiëren

### <a name="copy-single-blob-within-storage-account"></a>Kopiëren van één blob binnen opslag-account

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Wanneer u een blob binnen een account opslag kopieert, wordt een [kopie van de serverzijde](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) -bewerking wordt uitgevoerd.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopiëren van één blob Storage-accounts

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Wanneer u een blob Storage accounts kopieert, wordt een [kopie van de serverzijde](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) -bewerking wordt uitgevoerd.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Eén blob van secundaire regio naar primaire regio kopiëren

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Houd er rekening mee dat u leestoegang geografische-redundante opslag ingeschakeld nodig hebt.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopiëren van één blob en bijbehorende momentopnamen opslag-accounts

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Na de bewerking kopiëren bevat de doelcontainer de blob en de momentopnamen. Als dat de label in het bovenstaande voorbeeld heeft twee momentopnamen, bevat de container de volgende blob en momentopnamen:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchroon kopiëren BLOB's opslag-accounts

AzCopy al dan niet standaard worden gegevens tussen eindpunten opslagruimte asynchroon gekopieerd. De kopieeropdracht wordt daarom uitgevoerd in de achtergrond met ongebruikte bandbreedte capaciteit waarop geen SLA in hoe u snel een blob worden gekopieerd en AzCopy controleert de status kopie regelmatig totdat het kopiëren is voltooid, of is mislukt.

De `/SyncCopy` optie zorgt ervoor dat de kopieeropdracht consistente snelheid krijgt. AzCopy kunt u de synchrone kopie uitvoeren door de BLOB's kopiëren van de opgegeven bron naar lokale geheugen downloaden en deze vervolgens uploaden naar de bestemming van de Blob storage.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`mogelijk extra egress kosten vergeleken met asynchrone kopie genereren, de aanbevolen werkwijze is deze optie wilt gebruiken in een Azure-VM die zich in dezelfde regio als uw bron-opslag-account om te voorkomen egress kosten.

## <a name="file-download"></a>Bestand: downloaden

### <a name="download-single-file"></a>Enkel bestand downloaden

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Als de opgegeven bron een Azure bestandsshare, dan moet u de exacte bestandsnaam opgeven (*bijvoorbeeld* `abc.txt`) downloaden van één bestand of de optie opgeven `/S` om alle bestanden in de recursief delen te downloaden. U probeert een bestandspatroon en de optie opgeven `/S` samen resulteert in een fout.

### <a name="download-all-files"></a>Alle bestanden downloaden

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Houd er rekening mee dat lege mappen worden niet gedownload.

## <a name="file-upload"></a>Bestand: uploaden

### <a name="upload-single-file"></a>Bestand uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Alle bestanden uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Houd er rekening mee dat lege mappen die niet worden geüpload.

### <a name="upload-files-matching-specified-pattern"></a>Bestanden die overeenkomen met opgegeven patroon uploaden

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Bestand: kopiëren

### <a name="copy-across-file-shares"></a>Ander bestandsshares kopiëren

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopieer uit bestandsshare naar blob

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Houd er rekening mee dat asynchroon kopiëren van bestandsopslag naar pagina Blob niet wordt ondersteund.

### <a name="copy-from-blob-to-file-share"></a>Kopieer van blob naar bestand delen

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Bestanden synchroon kopiëren

Kunt u de `/SyncCopy` optie gegevens uit bestandsopslag naar bestandsopslag, bestandsopslag met Blob Storage en Blob Storage naar bestandsopslag synchroon kopiëren, AzCopy doet u dit door te downloaden van de brongegevens naar lokale geheugen en upload deze opnieuw op bestemming.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Wanneer het kopiëren van bestandsopslag naar Blob Storage, het standaardtype voor blob is blok blob, gebruiker kan opgeven optie `/BlobType:page` om te wijzigen van het type blob.

Houd er rekening mee dat `/SyncCopy` mogelijk extra egress kosten vergelijken asynchroon kopie genereren, de aanbevolen werkwijze is deze optie wilt gebruiken in Azure VM die in hetzelfde gebied, als uw account van de opslagruimte bron om te voorkomen egress kosten.

## <a name="table-export"></a>Tabel: exporteren

### <a name="export-table"></a>Wizard tabel exporteren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy schrijft een manifest bestand naar de map voor de opgegeven bestemming. Het manifest bestand wordt gebruikt in het importproces te zoeken van de benodigde gegevensbestanden en gegevensvalidatie. Het manifest bestand gebruikt de volgende naamgevingsconventie al dan niet standaard:

    <account name>_<table name>_<timestamp>.manifest

Gebruiker kan ook opgeven voor de optie `/Manifest:<manifest file name>` voor het instellen van de manifest bestandsnaam.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Gesplitste exporteren in meerdere bestanden

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy wordt als u een *volume-index* gebruikt in de bestandsnamen van de gesplitste gegevens om te onderscheiden van meerdere bestanden. Het volume-index bestaat uit twee delen, een *partition belangrijke bereik index* en een *bestandsindex splitsen*. Beide indexen zijn gebaseerd op nul.

De index van de belangrijkste bereik partition is 0 als gebruiker de optie geen geeft `/PKRS`.

Stel AzCopy genereert twee gegevensbestanden nadat de gebruiker Hiermee kunt u de optie `/SplitSize`. De resulterende bestandsnamen van gegevens mogelijk:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Opmerking die de minimale mogelijke waarde van de optie `/SplitSize` 32 MB. Als de opgegeven bestemming is blobopslag, AzCopy wordt het gegevensbestand splitsen zodra de grootte bereikt de groottebeperkingen blob (200GB), ongeacht of option `/SplitSize` is opgegeven door de gebruiker.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Wizard tabel exporteren naar JSON- of CSV-bestandsindeling voor gegevens

Tabellen exporteert AzCopy al dan niet standaard naar JSON-gegevensbestanden. Kunt u de optie `/PayloadFormat:JSON|CSV` de tabellen exporteren als JSON- of CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Wanneer u de nettolading CSV-indeling opgeeft, AzCopy genereren ook een schemabestand met de bestandsextensie `.schema.csv` voor elk gegevensbestand.

### <a name="export-table-entities-concurrently"></a>Tabel entiteiten gelijktijdig exporteren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy begint bewerkingen gelijktijdig als u wilt exporteren entiteiten wanneer de gebruiker Hiermee kunt u de optie `/PKRS`. Elke bewerking Hiermee exporteert u één partition belangrijke bereik.

Houd er rekening mee dat het aantal bewerkingen gelijktijdig ook wordt bepaald door de optie `/NC`. AzCopy het aantal processors core gebruikt als de standaardwaarde van `/NC` bij het kopiëren van de tabel entiteiten, zelfs als `/NC` niet is opgegeven. Wanneer de gebruiker bevat de optie `/PKRS`, AzCopy maakt gebruik van de kleinste van de twee waarden - partition belangrijke bereiken versus impliciet of expliciet opgegeven bewerkingen gelijktijdig - tot Bepaal het aantal bewerkingen gelijktijdig te starten. Voor meer informatie, typt u `AzCopy /?:NC` via de opdrachtregel.

### <a name="export-table-to-blob"></a>Wizard tabel exporteren naar blob

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy genereert een gegevensbestand JSON in de container blob met naamgevingsconventie te volgen:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

De indeling nettolading voor minimale metagegevens wordt gevolgd door het gegenereerde JSON-gegevensbestand. Zie voor meer informatie over deze indeling nettolading, [Nettolading-indeling voor tabel servicebewerkingen](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Houd er rekening mee dat bij het tabellen exporteren naar BLOB's, AzCopy wordt de tabel entiteiten naar lokale tijdelijke gegevensbestanden downloaden en upload deze entiteiten naar de blob. Deze bestanden tijdelijke gegevens worden automatisch naar de map logboek bestand met het standaardpad "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>', kunt u de optie/z [logboek-bestand-map] om te wijzigen van het logboek bestand maplocatie en de locatie van de bestanden tijdelijke gegevens dus wijzigen. Grootte van de tijdelijke bestanden wordt bepaald door uw tabel entiteiten grootte en de grootte die u hebt opgegeven met de optie /SplitSize, hoewel het tijdelijke gegevensbestand in lokale schijf wordt onmiddellijk worden verwijderd nadat deze is geüpload naar de blob, zorg ervoor dat er voldoende ruimte lokale schijf voor de opslag van deze tijdelijke gegevensbestanden voordat ze worden verwijderd.

## <a name="table-import"></a>Tabel: importeren

### <a name="import-table"></a>Tabel importeren

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

De optie `/EntityOperation` wordt aangegeven hoe entiteiten in de tabel invoegt. Mogelijke waarden zijn:

- `InsertOrSkip`: Hiermee worden een bestaande entiteit overgeslagen of een nieuwe entiteit ingevoegd als deze niet in de tabel bestaat nog.
- `InsertOrMerge`: Een bestaande entiteit samengevoegd of een nieuwe entiteit ingevoegd als deze niet in de tabel bestaat nog.
- `InsertOrReplace`: Hiermee vervangt u een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet in de tabel bestaat nog.

Opmerking dat kunt u de optie opgeven `/PKRS` in een scenario voor het importeren. In tegenstelling tot het scenario exporteren, waarin u de optie moet opgeven `/PKRS` bewerkingen gelijktijdig, AzCopy wordt standaard begint gestart bewerkingen gelijktijdig wanneer u een tabel importeert. Het standaardaantal bewerkingen gelijktijdig gestart is gelijk aan het aantal processors core; u kunt echter een verschillend aantal gelijktijdige met de optie opgeven `/NC`. Voor meer informatie, typt u `AzCopy /?:NC` via de opdrachtregel.

Houd er rekening mee dat AzCopy biedt alleen ondersteuning voor JSON, niet CSV importeren. AzCopy niet ondersteunen tabel invoer uit JSON gebruiker gemaakt en bestanden bestandenlijst. Beide bestanden moeten afkomstig zijn uit een AzCopy tabel exporteren. Om fouten te voorkomen, wijzig niet de geëxporteerde JSON of bestandenlijst bestand.

### <a name="import-entities-to-table-using-blobs"></a>Entiteiten aan een tabel met BLOB's importeren

Wordt ervan uitgegaan dat een container Blob bevat de volgende handelingen uit: A JSON-bestand dat staat voor een Azure-tabel en het bijbehorende manifest bestand.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

U kunt de volgende opdracht entiteiten importeren in een tabel met de manifest-bestand in dat onderdeel blob uitvoeren:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Andere functies AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Alleen de gegevens die niet in de bestemming voorkomt kopiëren

De `/XO` en `/XN` parameters kunt u oudere of nieuwere bron resources wordt gekopieerd, respectievelijk uitsluiten. Als u alleen bron bronnen die niet in de bestemming kopiëren wilt, kunt u beide parameters in de opdracht AzCopy opgeven:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Opmerking: Dit wordt niet ondersteund wanneer de bron of het doel een tabel is.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Een antwoordbestand gebruiken om op te geven opdrachtregelparameters

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

U kunt eventuele opdrachtregelparameters AzCopy opnemen in een antwoordbestand. AzCopy verwerkt de parameters in het bestand als wanneer ze op de opdrachtregel had is opgegeven voor het uitvoeren van een directe vervangen met de inhoud van het bestand.

Wordt ervan uitgegaan dat een antwoordbestand met de naam `copyoperation.txt`, die de volgende regels bevat. Elke parameter AzCopy kan worden opgegeven in een ononderbroken lijn

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

of op afzonderlijke regels:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy loopt vast als u de parameter over twee regels verdelen, zoals hier wordt getoond voor de `/sourcekey` parameter:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Meerdere antwoord bestanden gebruiken om op te geven opdrachtregelparameters

Wordt ervan uitgegaan dat een antwoordbestand met de naam `source.txt` waarmee wordt opgegeven dat een Broncontainer:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

En een antwoordbestand met de naam `dest.txt` waarmee wordt opgegeven dat een doelmap in het bestandssysteem:

    /Dest:C:\myfolder

En een antwoordbestand met de naam `options.txt` waarmee wordt opgegeven dat opties voor AzCopy:

    /S /Y

Als u wilt bellen AzCopy met deze bestanden antwoord, die zich bevinden in een map `C:\responsefiles`, gebruik deze opdracht:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy verwerkt deze opdracht net zoals wanneer u alle afzonderlijke parameters op de opdrachtregel hebt opgenomen:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Geef de handtekening van een gedeelde access (SA's)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

U kunt ook een SA's op de container URI opgeven:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Logboek bestandsmap

Telkens wanneer die u een opdracht aan AzCopy geven, er wordt gecontroleerd of een logboek-bestand zich in de standaard archiefmap of of deze in een map die u hebt opgegeven via deze optie bestaat. Als het bestand logboek niet in beide plaatsen bestaat, wordt AzCopy wordt de bewerking verwerkt als nieuwe en genereert een nieuw logboek-bestand.

Als het bestand logboek bestaat, kan AzCopy wordt gecontroleerd of de opdrachtregel die u invoert overeenkomt met de opdrachtregel in het logboek-bestand. Als de twee regels van de opdracht overeenkomen, hervat AzCopy de onvoltooide bewerking. Als ze niet overeenkomen, wordt u gevraagd of het bestand wilt overschrijven logboek start een nieuwe bewerking of de huidige bewerking te annuleren.

Als u de standaardlocatie gebruiken voor het logboek-bestand wilt:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Als u de optie weglaat `/Z`, of de optie opgeven `/Z` zonder het mappad, zoals hierboven, AzCopy wordt het logboek bestand in de standaardlocatie, dat wil zeggen `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Als het bestand logboek al bestaat, hervat AzCopy de bewerking op basis van het logboek-bestand.

Als u opgeven van een aangepaste locatie voor het logboek-bestand wilt:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

In dit voorbeeld wordt het logboek-bestand als dit nog niet bestaat. Als deze bestaat nog, hervat AzCopy de bewerking op basis van het logboek-bestand.

Als u een bewerking AzCopy hervatten wilt:

    AzCopy /Z:C:\journalfolder\

In dit voorbeeld wordt hervat de laatste bewerking kan om te voltooien.

### <a name="generate-a-log-file"></a>Een logboekbestand genereren

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Als u de optie opgeven `/V` zonder een bestandspad naar het uitgebreide logboek, AzCopy maakt vervolgens het logboekbestand in de standaardlocatie, dat wil zeggen `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Anders kunt u een logboekbestand maken in een aangepaste locatie:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Als u een relatieve pad volgen de optie opgeven `/V`, zoals `/V:test/azcopy1.log`, wordt het uitgebreide logboek in de huidige werkmap in een submap gemaakt `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geef het aantal bewerkingen gelijktijdig te starten

Optie `/NC` geeft het aantal gelijktijdige kopiëren. AzCopy wordt standaard een bepaald aantal bewerkingen gelijktijdig om uit te breiden de overdracht gegevensdoorvoer gestart. Voor bewerkingen van de tabel is het aantal bewerkingen gelijktijdig gelijk aan het aantal processors die u hebt. Voor Blob en de bestandsnaam bewerkingen uitvoeren, het aantal bewerkingen gelijktijdig is gelijk aan 8 maal het aantal processors die u hebt. Als u AzCopy via een netwerk met een lage bandbreedte uitvoert, kunt u een laag getal voor /NC om te voorkomen die worden veroorzaakt door resource concurrenten is mislukt.

### <a name="run-azcopy-against-azure-storage-emulator"></a>AzCopy voor Azure opslag Emulator uitgevoerd

U kunt AzCopy ten opzichte van de [Azure opslag Emulator](storage-use-emulator.md) voor BLOB's uitvoeren:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

en tabellen:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy Parameters

Parameters voor AzCopy worden hieronder beschreven. U kunt ook een van de volgende opdrachten vanaf de opdrachtregel voor hulp bij het typen in het AzCopy gebruiken:

- Voor gedetailleerde help voor AzCopy:`AzCopy /?`
- Voor gedetailleerde hulp bij elke parameter AzCopy:`AzCopy /?:SourceKey`
- Voor de opdrachtregel voorbeelden:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Gegevensbron: "bron"

Hiermee geeft u de brongegevens waarmee u wilt kopiëren. De bron kan zijn de systeemmap van een bestand, een container blob, een virtuele map blob, een bestandsshare opslag, een opslag van skyline, of een Azure-tabel.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="destdestination"></a>/ Dest: "bestemming"

Hiermee geeft u de bestemming om naar te kopiëren. De bestemming kan zijn de systeemmap van een bestand, een container blob, een virtuele map blob, een bestandsshare opslag, een opslag van skyline of een Azure-tabel.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="patternfile-pattern"></a>/ Patroon: "bestand-patroon"

Hiermee geeft u een bestandspatroon waarmee wordt aangegeven welke bestanden om te kopiëren. Het gedrag van de parameter /Pattern wordt bepaald door de locatie van de brongegevens en de aanwezigheid van de optie recursieve-modus. Recursieve modus wordt opgegeven via de optie/s.

Als de opgegeven bron een map in het bestandssysteem, klikt u vervolgens standaard jokertekens zijn van kracht en het opgegeven bestandspatroon wordt vergeleken met bestanden in de map. Als de optie die /s is opgegeven, klikt u vervolgens AzCopy is ook komt overeen met het opgegeven patroon ten opzichte van alle bestanden in eventuele submappen onder de map.

Als de opgegeven bron een container blob of virtuele map is, worden klikt u vervolgens jokertekens niet toegepast. Als het opgegeven bestandspatroon optie die /s is opgegeven, klikt u vervolgens AzCopy geïnterpreteerd als een voorvoegsel blob. Als de optie die /s niet is opgegeven, klikt u vervolgens AzCopy komt overeen met het bestandspatroon dat ten opzichte van de exacte blob namen.

Als de opgegeven bron een Azure bestandsshare, dan moet u de exacte bestandsnaam opgeven, (bijvoorbeeld abc.txt) als u wilt kopiëren van één bestand, of de optie opgeven /S om te kopiëren van alle bestanden in de recursief delen. Geef een bestandspatroon en de optie er /S samen treedt een fout.

AzCopy gebruikt hoofdlettergevoelig overeenkomende als de /Source een container blob of blob virtuele map is, en gebruikt niet hoofdlettergevoelig overeenkomende in alle andere gevallen.

De standaard-bestandspatroon gebruikt als er geen bestandspatroon wordt opgegeven is *.* voor een locatie op systeem of een lege voorvoegsel voor een opslaglocatie van Azure. Meerdere bestand patronen opgeven wordt niet ondersteund.

**Van toepassing op:** BLOB's, bestanden

### <a name="destkeystorage-key"></a>/ DestKey: "storage-key"

Hiermee geeft u de accountsleutel opslag voor de resource destination.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="destsassas-token"></a>/ DestSAS: "SA's-token"

Hiermee geeft u een gedeeld Access handtekening (SA's) met machtigingen voor lezen en schrijven voor de bestemming (indien beschikbaar). Omsluiten de SA's met dubbele aanhalingstekens is geplaatst, zoals het mogelijk speciale opdrachtregel tekens bevat.

Als de waarde van doel-resource een blob container, een bestandsshare of een tabel is, kunt u deze optie gevolgd door het token SA's opgeven of u kunt de SA's opgeven als onderdeel van de bestemming blob container, bestandsshare of van de tabel URI, zonder deze optie.

Als de bron- en doeltabellen beide BLOB's zijn, klikt u vervolgens de bestemming blob moet zich bevinden in hetzelfde account als de bron-blob storage.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="sourcekeystorage-key"></a>/ SourceKey: "storage-key"

Hiermee geeft u de accountsleutel opslag voor de resource bron.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="sourcesassas-token"></a>/ SourceSAS: "SA's-token"

Hiermee geeft u een handtekening Access gedeeld met de lees- en lijst met machtigingen voor de bron (indien beschikbaar). Omsluiten de SA's met dubbele aanhalingstekens is geplaatst, zoals het mogelijk speciale opdrachtregel tekens bevat.

Als de bron-resource een container blob is en een sleutel noch een SA's is opgegeven, wordt de container blob via anonieme toegang worden gelezen.

Als de bron een bestandsshare is of tabel, een toets of een SA's moet worden opgegeven.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="s"></a>/S

Hiermee geeft u recursieve modus voor het kopiëren van. In de modus recursieve wordt AzCopy alle BLOB's of bestanden die overeenkomen met het opgegeven bestandspatroon, inclusief de computers in submappen kopiëren.

**Van toepassing op:** BLOB's, bestanden

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blokkeren" | 'pagina' | 'toevoegen'

Geeft aan of de bestemming blob een blob blok, een blob pagina of een blob toevoegen. Deze optie geldt alleen wanneer u een blob wilt uploaden. Anders, wordt een fout gegenereerd. Als de bestemming een blob is en deze optie niet is opgegeven, klikt u standaard AzCopy Hiermee maakt u een blok blob.

**Van toepassing op:** BLOB 's

### <a name="checkmd5"></a>/ CheckMD5

Berekent een MD5-hash voor gedownloade gegevens en controleert of de MD5-hash die zijn opgeslagen in de blob of van het bestand inhoud-MD5 eigenschap komt overeen met de berekende hash. Het selectievakje MD5 is standaard uitgeschakeld dus moet u deze optie als u wilt de controle MD5 uitvoeren bij het downloaden van gegevens opgeven.

Houd er rekening mee dat Azure Storage niet garanderen dat de MD5-hash opgeslagen voor de blob of het bestand bijgewerkt is. Het is van client verantwoordelijkheid de MD5 bijwerken wanneer de blob of het bestand is gewijzigd.

De inhoud-MD5-eigenschap voor een Azure blob of het bestand AzCopy altijd ingesteld na het uploaden naar de service.  

**Van toepassing op:** BLOB's, bestanden

### <a name="snapshot"></a>/ Momentopname

Geeft aan of momentopnamen overbrengen. Deze optie is alleen geldig wanneer de bron een blob is.

De namen van de momentopnamen overgedragen blob worden gewijzigd in deze indeling: .extensie blob-naam (momentopname-tijd)

Standaard momentopnamen niet worden gekopieerd.

**Van toepassing op:** BLOB 's

### <a name="vverbose-log-file"></a>/ V: [uitgebreide-logboekbestand]

Uitvoer van uitgebreide statusberichten in een logboekbestand.

Het logboekbestand heet standaard AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`. Als u een bestaande bestandslocatie voor deze optie opgeeft, wordt de uitgebreide gegevens worden toegevoegd aan dit bestand.  

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="zjournal-file-folder"></a>/ Z [logboek-bestand-map]

Hiermee geeft u een logboekmap een bewerking wilt hervatten.

AzCopy ondersteunt altijd als een bewerking is onderbroken hervatten.

Als deze optie niet opgegeven is of deze is opgegeven zonder een mappad, maakt AzCopy het logboek-bestand in de standaardlocatie, dat wil % LocalAppData%\Microsoft\Azure\AzCopy zeggen.

Telkens wanneer die u een opdracht aan AzCopy geven, er wordt gecontroleerd of een logboek-bestand zich in de standaard archiefmap of of deze in een map die u hebt opgegeven via deze optie bestaat. Als het bestand logboek niet in beide plaatsen bestaat, wordt AzCopy wordt de bewerking verwerkt als nieuwe en genereert een nieuw logboek-bestand.

Als het bestand logboek bestaat, kan AzCopy wordt gecontroleerd of de opdrachtregel die u invoert overeenkomt met de opdrachtregel in het logboek-bestand. Als de twee regels van de opdracht overeenkomen, hervat AzCopy de onvoltooide bewerking. Als ze niet overeenkomen, wordt u gevraagd of het bestand wilt overschrijven logboek start een nieuwe bewerking of de huidige bewerking te annuleren.

Het logboek-bestand wordt verwijderd na de bewerking is voltooid.

Houd er rekening mee dat een hervat uit een logboek-bestand dat is gemaakt door een eerdere versie van AzCopy niet wordt ondersteund.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="parameter-file"></a>/@:"parameter-file"

Hiermee geeft u een bestand met parameters. AzCopy verwerkt de parameters in het bestand, net als wanneer ze op de opdrachtregel is doorgegeven.

In een antwoordbestand, kunt u meerdere parameters opgeeft op één regel, of geef elke parameter op een aparte regel. Houd er rekening mee dat een afzonderlijke parameter kan niet worden gebruikt voor het bereik van meerdere regels.

Antwoord bestanden kunnen opmerkingen lijnen die met het teken beginnen # opnemen.

U kunt meerdere antwoord bestanden opgeven. Bedenk wel dat AzCopy geneste antwoord bestanden niet ondersteunt.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="y"></a>/ Y

Alle AzCopy bevestigingsverzoeken onderdrukt.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="l"></a>/ L

Hiermee geeft u een vermelding-bewerking. Er zijn geen gegevens wordt gekopieerd.

AzCopy wordt interpreteren van het gebruik van deze optie als een simulatie voor het uitvoeren van de opdrachtregel zonder deze optie/l en tellen hoeveel objecten worden gekopieerd, kunt u de optie /V op hetzelfde moment om te controleren welke objecten in het uitgebreide logboekbestand worden gekopieerd.

Het gedrag van deze optie wordt ook bepaald door de locatie van de brongegevens en de aanwezigheid van de recursieve modus optie /S en bestand optie patroon /Pattern.

AzCopy moet de machtiging voor lijst of lezen van deze bronlocatie wanneer u deze optie.

**Van toepassing op:** BLOB's, bestanden

### <a name="mt"></a>/MT

Hiermee stelt u het gedownloade bestand laatste wijziging tijd moet hetzelfde als de bron blob of van het bestand.

**Van toepassing op:** BLOB's, bestanden

### <a name="xn"></a>/XN

Aangeven dat een nieuwere bron bron. De resource niet worden gekopieerd als de tijd van de laatste wijziging van de bron dezelfde is of hoger zijn dan de bestemming.

**Van toepassing op:** BLOB's, bestanden

### <a name="xo"></a>/XO

Sluit een oudere bron resource. De resource niet worden gekopieerd als de tijd van de laatste wijziging van de bron dezelfde is of ouder zijn dan de bestemming.

**Van toepassing op:** BLOB's, bestanden

### <a name="a"></a>/A

Alleen bestanden met het kenmerk Archief uploadt.

**Van toepassing op:** BLOB's, bestanden

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Alleen de bestanden die een van de opgegeven kenmerken set hebt geüpload.

Beschikbare kenmerken opnemen:

- R = alleen-lezenbestanden
- A = bestanden bent u er klaar voor het archiveren
- S = systeembestanden
- H = Verborgen bestanden
- C = gecomprimeerde bestanden
- N = normale bestanden
- E = versleutelde bestanden
- T = tijdelijke bestanden
- O = Offline bestanden
- Ik bestanden niet-geïndexeerde =

**Van toepassing op:** BLOB's, bestanden

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Sluit bestanden die een of meer van de opgegeven kenmerken set uit.

Beschikbare kenmerken opnemen:

- R = alleen-lezenbestanden
- A = bestanden bent u er klaar voor het archiveren
- S = systeembestanden
- H = Verborgen bestanden
- C = gecomprimeerde bestanden
- N = normale bestanden
- E = versleutelde bestanden
- T = tijdelijke bestanden
- O = Offline bestanden
- Ik bestanden niet-geïndexeerde =

**Van toepassing op:** BLOB's, bestanden

### <a name="delimiterdelimiter"></a>/ Scheidingsteken: "scheidingsteken"

Geeft het scheidingsteken dat wordt gebruikt om te scheiden van virtuele mappen in een blob-naam.

Standaard AzCopy gebruik / als scheidingsteken. Echter AzCopy ondersteunt het gebruik van een algemene teken (zoals @, # of %) als scheidingsteken. Als u nodig hebt om op te nemen op een van deze tekens op de opdrachtregel, moet u de naam van het bestand met dubbele aanhalingstekens is geplaatst.

Deze optie is alleen van toepassing voor het downloaden van BLOB's.

**Van toepassing op:** BLOB 's

### <a name="ncnumber-of-concurrent-operations"></a>/ AC: "getal-van-gelijktijdige-bewerkingen"

Hiermee geeft u het aantal gelijktijdige bewerkingen.

AzCopy al dan niet standaard wordt een bepaald aantal bewerkingen gelijktijdig om uit te breiden de overdracht gegevensdoorvoer gestart. Houd er rekening mee dat groot aantal bewerkingen in een omgeving met lage bandbreedte gelijktijdig mogelijk de netwerkverbinding overspoeld en voorkomen dat de bewerkingen volledig uitvoeren. Gelijktijdige bewerkingen op basis van de werkelijke beschikbare netwerkbandbreedte beperken.

De bovengrens voor bewerkingen gelijktijdig is 512.

**Van toepassing op:** BLOB's, bestanden, tabellen

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tabel"

Geeft aan dat de `source` resource is een beschikbaar in de lokale ontwikkelomgeving wordt uitgevoerd in de opslagruimte emulator blob.

**Van toepassing op:** BLOB's, tabellen

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tabel"

Geeft aan dat de `destination` resource is een beschikbaar in de lokale ontwikkelomgeving wordt uitgevoerd in de opslagruimte emulator blob.

**Van toepassing op:** BLOB's, tabellen

### <a name="pkrskey1key2key3"></a>/ PKRS: "sleutel1 # sleutel2 key3 #..."

Het belangrijkste bereik partition om in te schakelen voor het exporteren van gegevens in een tabel parallel, waardoor de snelheid van de exportbewerking gesplitst.

Als deze optie niet opgeeft is, wordt AzCopy één thread tabel entiteiten exporteren. Als de gebruiker opgeeft /PKRS bijvoorbeeld: "aa #bb" en klik vervolgens AzCopy begint drie gelijktijdige bewerkingen.

Elke bewerking exporteert een van de drie belangrijkste bereiken van partition, zoals hieronder wordt weergegeven:

  [eerste-partition-sleutel aa)

  [aa, bb)

  [bb, laatste-partition-toets]

**Van toepassing op:** Tabellen

### <a name="splitsizefile-size"></a>/ SplitSize: "bestandsgrootte"

Hiermee geeft u het geëxporteerde bestand splitsen grootte in MB, de minimale waarde toegestaan is 32.

Als deze optie niet opgeeft is, wordt AzCopy gegevens in een tabel exporteren naar bestand.

Als de gegevens in een tabel is geëxporteerd naar een blob en de grootte van het geëxporteerde bestand de limiet 200 GB voor blobgrootte bereikt, klikt u vervolgens splitsen AzCopy het geëxporteerde bestand ook als deze optie is niet opgegeven.

**Van toepassing op:** Tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Hiermee geeft u het tabelgedrag voor het importeren van gegevens.

- InsertOrSkip - slaat over een bestaande entiteit of een nieuwe entiteit ingevoegd als deze niet bestaat in de tabel.

- InsertOrMerge - samengevoegd van een bestaande entiteit of een nieuwe entiteit ingevoegd als deze nog niet bestaat in de tabel.

- InsertOrReplace - Hiermee vervangt u een bestaande entiteit of een nieuwe entiteit ingevoegd als deze nog niet bestaat in de tabel.

**Van toepassing op:** Tabellen

### <a name="manifestmanifest-file"></a>/ Manifest: "manifest-bestand"

Hiermee geeft u het bestand dat manifest voor de tabel exporteren en importeren bewerking.

Deze optie is optioneel tijdens de exportbewerking, AzCopy genereert een manifest bestand met vooraf gedefinieerde naam als deze optie niet is opgegeven.

Deze optie is vereist voor het zoeken naar de gegevensbestanden tijdens het importeren.

**Van toepassing op:** Tabellen

### <a name="synccopy"></a>/ SyncCopy

Geeft aan of BLOB's of bestanden tussen twee Azure Storage eindpunten synchroon te kopiëren.

AzCopy al dan niet standaard gebruikt serverzijde asynchroon kopie. Geef op deze optie voor het uitvoeren van een synchroon kopie gedownload van BLOB's of bestanden naar lokale geheugen en uploadt ze vervolgens naar de opslag van Azure.

U kunt deze optie kunt gebruiken bij het kopiëren van bestanden in-blobopslag, binnen bestandsopslag of vanuit blobopslag in bestandsopslag of vice versa.

**Van toepassing op:** BLOB's, bestanden

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "inhoudstype"

Hiermee geeft u het MIME-inhoudstype bestemming BLOB's of bestanden.

AzCopy wordt het inhoudstype voor een bestand of blob ingesteld op application/octet-stream al dan niet standaard. Door expliciet een waarde voor deze optie kunt u het inhoudstype voor alle BLOB's of bestanden instellen.

Als u deze optie zonder een waarde opgeven, klikt u vervolgens instellen AzCopy elke blob of inhoudstype op basis van de bestandsextensie van bestand.

**Van toepassing op:** BLOB's, bestanden

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Hiermee geeft u de opmaak van het geëxporteerde tabelbestand.

Als deze optie niet opgeeft is, al dan niet standaard AzCopy Hiermee exporteert u de gegevensbestand tabel in de indeling van JSON.

**Van toepassing op:** Tabellen

## <a name="known-issues-and-best-practices"></a>Bekende problemen en aanbevolen procedures

### <a name="limit-concurrent-writes-while-copying-data"></a>Gelijktijdige schrijft tijdens het kopiëren van gegevens beperken

Wanneer u BLOB's of bestanden met AzCopy kopieert, houd er rekening mee dat een andere toepassing kan worden wijzigen van de gegevens terwijl u deze kopieert. Indien mogelijk, zorgen dat de gegevens die u kopieert niet wordt gewijzigd tijdens het kopiëren. Bijvoorbeeld wanneer u een VHD die is gekoppeld aan een Azure virtuele machines kopieert, zorg dat er geen andere toepassingen momenteel naar de VHD schrijft. Een goede manier hiervoor is door de overdracht van de resource moet worden gekopieerd. U kunt ook eerst een momentopname van de VHD maken en kopieer de momentopname.

Als u niet kunt voorkomen andere toepassingen schrijven BLOB's of bestanden dat terwijl ze worden gekopieerd, klikt u vervolgens Denk eraan dat op het moment dat de taak is voltooid, de gekopieerde bronnen niet meer mogelijk volledige overeenkomst met de bron-resources.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Één exemplaar van de AzCopy uitvoeren op één computer.
AzCopy is ontworpen voor het gebruik van de resource van uw computer te versnellen de gegevensoverdracht maximaliseren, is het raadzaam u slechts één exemplaar van de AzCopy uitvoeren op één computer en geef de optie `/NC` als u meer gelijktijdige bewerkingen nodig hebt. Voor meer informatie, typt u `AzCopy /?:NC` via de opdrachtregel.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS MD5-algoritmen inschakelen voor AzCopy wanneer u ' gebruik FIPS compatibele algoritmen voor codering, hashing en ondertekening '.
AzCopy al dan niet standaard .NET MD5 implementatie gebruikt voor het berekenen van de MD5 wanneer objecten kopiëren, maar er zijn enkele beveiligingsvereisten die zijn die nodig hebt AzCopy FIPS compatibele MD5 instelling inschakelen.

U kunt een bestand app.config maken `AzCopy.exe.config` met de eigenschap `AzureStorageUseV1MD5` en deze bestemmen met AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Voor de eigenschap "AzureStorageUseV1MD5" • waar - wordt de standaardwaarde en AzCopy gebruikt in .NET MD5-implementatie.
• ONWAAR – AzCopy wordt gebruikt in FIPS-compatibele MD5 algoritmen.

Houd er rekening mee dat FIPS-algoritmen is standaard uitgeschakeld in uw Windows-computer, kunt u secpol.msc Typ in het venster uitvoeren en schakel deze schakeloptie bij beveiligingsinstelling -> lokaal beleid-Beveiliging > Opties -> systeem cryptografische: gebruik FIPS-algoritmen voor codering, hashing en ondertekening.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende bronnen voor meer informatie over de opslag van Azure en AzCopy.

### <a name="azure-storage-documentation"></a>Azure opslag documentatie:

- [Inleiding tot Azure opslag](storage-introduction.md)
- [Het gebruik van .NET-blobopslag](storage-dotnet-how-to-use-blobs.md)
- [Het gebruik van bestandsopslag van .NET](storage-dotnet-how-to-use-files.md)
- [Het gebruik van .NET-tabelopslag](storage-dotnet-how-to-use-tables.md)
- [Het maken, beheren of verwijderen van een opslag-account](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure opslag blogberichten:
- [Inleiding tot Azure opslag gegevens verkeer bibliotheek Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Inleiding tot synchroon kopiëren en aangepaste inhoudstypen](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Aankondigen algemene beschikbaarheid van AzCopy 3.0 plus preview-versie van AzCopy 4.0 met ondersteuning voor tabel- en -bestand](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Geoptimaliseerd voor grootschalige kopie scenario 's](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Ondersteuning voor leestoegang geografische-redundante opslag](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Gegevens met opnieuw meer gestart modus en SA's token overbrengen](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Cross-account kopie Blob gebruiken](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Uploaden/downloaden van bestanden voor Azure BLOB 's](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
