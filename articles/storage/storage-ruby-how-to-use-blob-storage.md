<properties
    pageTitle="Het gebruik van blobopslag (object opslag) uit Ruby | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure-blobopslag (object opslag)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Het gebruik van Ruby-blobopslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure-blobopslag is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. Blobopslag kunt elk gewenst type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstaller opslaan. Blobopslag is ook object opslag genoemd.

Deze handleiding leert u hoe u veelvoorkomende scenario's met blobopslag uitvoert. In de voorbeelden worden geschreven de Ruby-API gebruiken. De scenario's waarvoor bevatten **uploaden, vermelding, downloadt,** en **verwijderen van** BLOB's.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing bouwen

Maak een Ruby-toepassing. Zie voor instructies [Ruby op Rails webtoepassing op een Azure-VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing toegang hebben tot opslag configureren

Als u wilt gebruiken Azure opslag, die u wilt downloaden en gebruiken van het Ruby azure pakket, waaronder een reeks gemak bibliotheken die met de opslagruimte REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>Gebruik RubyGems het pakket verkrijgen

1. Gebruik een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).

2. Typ 'azure gem installeren' in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Met uw favoriete teksteditor, voeg de volgende naar het begin van het Ruby-bestand waarop u wilt opslagruimte gebruiken:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

De azure module leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS_KEY** voor informatie die nodig is om te koppelen aan een account Azure opslag. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens voordat u **Azure::Blob::BlobService** met de volgende code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Als u deze waarden uit een klassieke of resourcemanager opslag-account in de portal van Azure:

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Navigeer naar de opslag-account dat u wilt gebruiken.
3. Klik in het blad instellingen aan de rechterkant, op **Toegangstoetsen**.
4. Klik in het Access-toetsen blad dat wordt weergegeven, ziet u de toegangstoets 1 en toegangstoets 2. U kunt een van deze. 
5. Klik op het pictogram kopiëren om de toets naar het Klembord kopiëren. 

Als u deze waarden van een account klassieke opslag in de klassieke Azure-portal:

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de opslag-account dat u wilt gebruiken.
3. Klik op **Beheren TOEGANGSTOETSEN** onderaan in het navigatiedeelvenster.
4. Klik in het venstermenu dialoogvenster ziet u de opslagaccountnaam, toegangstoets primaire en secundaire toegangstoets. Voor de sneltoets, kunt u de primaire bewerking of de secundaire fase. 
5. Klik op het pictogram kopiëren om de toets naar het Klembord kopiëren.

## <a name="create-a-container"></a>Een container maken

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Het object **Azure::Blob::BlobService** kunt u werken met containers en BLOB's. Als u wilt een container maken, gebruikt u de **maken\_container()** methode.

Het volgende voorbeeld wordt een container of het afdrukken van de fout als er een.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Als u de bestanden in de container openbaar maken wilt, kunt u de machtigingen van de container instellen.

U kunt alleen wijzigen de <strong>maken\_container()</strong> oproep om door te geven de **: openbare\_access\_niveau** optie:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Geldige waarden voor de **: openbare\_access\_niveau** optie zijn:

* **blob:** Hiermee geeft u volledige openbare leestoegang voor container en blob gegevens. Clients BLOB's in de container via anonieme aanvraag kunnen opsommen, maar kunnen niet opsommen containers in het opslag-account.

* **container:** Hiermee geeft u openbare leestoegang voor BLOB's. BLOB-gegevens in deze container kunnen worden gelezen via anonieme aanvraag, maar de gegevens van de container is niet beschikbaar. Clients kunnen niet opsommen BLOB's in de container via anonieme mailaanvraag.

U kunt ook het toegangsniveau van openbare van een container wijzigen met behulp van **instellen\_container\_acl()** methode om op te geven van de openbare toegangsniveau.

Het volgende voorbeeld wordt het toegangsniveau van openbare aan **container**gewijzigd:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Inhoud uploaden naar een blob, gebruikt u de **maken\_blokkeren\_blob()** methode voor het maken van de blob, een bestand of een tekenreeks als de inhoud van de blob gebruiken.

De volgende code uploadt het bestand **test.png** als een nieuwe blob met de naam 'afbeelding-blob' in de container.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de containers **list_containers()** methode te gebruiken.
U kunt de BLOB's in een container gebruiken **lijst\_blobs()** methode.

Hiermee voert de URL's van alle BLOB's in de containers voor het account.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>BLOB's downloaden

Als u wilt downloaden BLOB's de **krijgen\_blob()** methode voor het ophalen van de inhoud.

Het volgende voorbeeld wordt gedemonstreerd met **krijgen\_blob()** de inhoud van "afbeelding-blob" downloaden en deze naar een lokaal bestand te schrijven.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Een Blob verwijderen
Ten slotte, als u wilt een blob verwijderen, gebruikt u de **verwijderen\_blob()** methode. Het volgende voorbeeld ziet u hoe u een blob verwijdert.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Volgende stappen

Meer informatie over opslagtaken voor meer complexe, volgt u deze koppelingen:

- [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK voor Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) -bibliotheek op GitHub
- [Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)
