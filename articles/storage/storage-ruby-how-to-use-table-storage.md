<properties
    pageTitle="Het gebruik van Azure-tabelopslag van Ruby | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud met Azure-tabelopslag, een gegevensopslag NoSQL."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Het gebruik van Azure-tabelopslag van Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u het uitvoeren van veelvoorkomende scenario's met de tabel Azure-service. In de voorbeelden worden geschreven de Ruby-API gebruiken. De scenario's waarvoor opnemen **maken en verwijderen van een tabel, invoegen en query's uitvoeren entiteiten in een tabel**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing bouwen

Voor instructies hoe u een Ruby-toepassing bouwen, raadpleegt u [Ruby op Rails webtoepassing op een VM Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Uw toepassing toegang hebben tot opslag configureren

Als u wilt gebruiken Azure opslag, die u wilt downloaden en gebruiken van het Ruby azure pakket dat beschikt over een aantal gemak bibliotheken die met de REST van de opslag-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>Gebruik RubyGems het pakket verkrijgen

1. Gebruik een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).

2. Typ **gem azure installeren** in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Gebruik uw favoriete teksteditor, de volgende handelingen uit toevoegen aan het begin van het Ruby-bestand waarop u wilt opslagruimte gebruiken:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen

De azure module leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** voor informatie die nodig is om te koppelen aan een Azure Storage-account. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens voordat u **Azure::TableService** met de volgende code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Als u deze waarden uit een klassieke of resourcemanager opslag-account in de Portal Azure:

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com).
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

## <a name="create-a-table"></a>Een tabel maken

Het object **Azure::TableService** kunt u werken met tabellen en entiteiten. Als u wilt een tabel maakt, gebruikt u de **maken\_table()** methode. Het volgende voorbeeld wordt een tabel of het afdrukken van de fout als er een.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt toevoegen een entiteit, moet u eerst een hash-object dat de entiteitseigenschappen van uw definieert maken. Houd er rekening mee dat elke entiteit moet u een **PartitionKey** en **RowKey**. Dit zijn de unieke id's van uw entiteiten, en waarden die veel sneller dan de andere eigenschappen kunnen worden doorzocht. Azure opslag gebruikt **PartitionKey** van de tabel entiteiten automatisch verdeeld over veel opslagruimte knooppunten. Entiteiten met de dezelfde **PartitionKey** worden opgeslagen op hetzelfde knooppunt. De **RowKey** is de unieke ID van de entiteit binnen de partition waarbij deze hoort.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Een entiteit bijwerken

Er zijn meerdere methoden beschikbaar voor het bijwerken van een bestaande entiteit:

* **bijwerken\_entity():** Een bestaande entiteit bijwerken door deze te vervangen.
* **samenvoegen\_entity():** Een bestaande entiteit worden bijgewerkt door het samenvoegen van nieuwe eigenschapswaarden in de bestaande entiteit.
* **invoegen\_of\_samenvoegen\_entity():** Een bestaande entiteit worden bijgewerkt door deze te vervangen. Als er geen entiteit bestaat, wordt een nieuw ingevoegd:
* **invoegen\_of\_vervangen\_entity():** Een bestaande entiteit worden bijgewerkt door het samenvoegen van nieuwe eigenschapswaarden in de bestaande entiteit. Als er geen entiteit bestaat, wordt een nieuw ingevoegd.

Het volgende voorbeeld wordt het bijwerken van het gebruik van een entity **bijwerken\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Met **bijwerken\_entity()** en **samenvoegen\_entity()**, als de entiteit die u wilt bijwerken en vervolgens de updatebewerking, mislukt niet bestaat. Dus als u opslaan van een entiteit wilt ongeacht of deze al bestaat, moet u in plaats daarvan gebruiken **invoegen\_of\_vervangen\_entity()** of **invoegen\_of\_samenvoegen\_entity()**.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten

Soms is het handig om in te dienen meerdere bewerkingen samen in een batch om ervoor te zorgen atomaire verwerkt door de server. Vandaar dat u eerst een **Batch** -object maken en gebruik vervolgens de **uitvoeren\_batch()** methode op **TableService**. Het volgende voorbeeld ziet u twee entiteiten met RowKey 2 en 3 in een batch verzendt. U ziet dat deze alleen voor entiteiten met de dezelfde PartitionKey werkt.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Query voor een entiteit

Als u wilt een entiteit in een tabel een query uitvoert, gebruikt u de **krijgen\_entity()** methode, door de naam van de tabel, **PartitionKey** en **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Een reeks entiteiten query

Als u wilt query een reeks entiteiten in een tabel, query hash-object maken en gebruiken de **query\_entities()** methode. Het volgende voorbeeld worden alle entiteiten met de dezelfde **PartitionKey**aan:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Als het resultaat is te groot voor één query om terug te keren, een token voortgezet wordt geretourneerd die u kunt gebruiken om op te halen van de volgende pagina's.

## <a name="query-a-subset-of-entity-properties"></a>Query een subset van entiteitseigenschappen

Een query toevoegen aan een tabel kunt u slechts een paar eigenschappen ophalen uit een entiteit. Deze techniek, 'raming', genaamd Hiermee reduceert u bandbreedte en prestaties van query's, met name voor grote entiteiten kunt verbeteren. De select-component gebruiken en geven van de namen van de eigenschappen die u wilt overbrengen naar de klant.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Als u wilt verwijderen van een entiteit, gebruikt u de **verwijderen\_entity()** methode. U moet de naam van de tabel waarin de entiteit, de PartitionKey en RowKey van de entiteit doorgeven.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u wilt een tabel verwijderen, gebruikt u de **verwijderen\_table()** methode en keer op de naam van de tabel die u wilt verwijderen.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Volgende stappen

Meer informatie over opslagtaken voor meer complexe, volgt u deze koppelingen:

- [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK voor Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) -bibliotheek op GitHub
