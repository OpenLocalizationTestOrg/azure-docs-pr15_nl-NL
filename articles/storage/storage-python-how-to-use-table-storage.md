<properties
    pageTitle="Het gebruik van Table storage uit Python | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud met Azure-tabelopslag, een gegevensopslag NoSQL."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Het gebruik van Table storage uit Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u veelvoorkomende scenario's uitvoeren met behulp van de Azure tabel storage-service. In de voorbeelden in Python zijn geschreven en gebruikt u de [Microsoft Azure opslag SDK voor Python]. De gedekte scenario's bevatten maken en verwijderen van een tabel, behalve invoegen en query's uitvoeren entiteiten in een tabel.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Een tabel maken

Het object **TableService** kunt u werken met services van de tabel. De volgende code maakt een object **TableService** . De code aan de bovenkant van een bestand Python waarin u wilt via programmacode toegang hebben tot Azure opslag hebt toegevoegd:

    from azure.storage.table import TableService, Entity

De volgende code maakt een object **TableService** met de naam en account-toets voor de account voor opslag.  'Mijn account' en 'mykey' vervangen door uw accountnaam en van toetsen.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt toevoegen een entiteit, moet u eerst een woordenlijst of entiteit waarin uw Entiteitsnamen en waarden maken. Houd er rekening mee dat voor elke entiteit, moet u **PartitionKey** en **RowKey**. Hierna ziet u de unieke id's van uw entiteiten. Met deze waarden veel sneller dan kunt u uw andere eigenschappen zoeken, kunt u zoeken. De tabel entiteiten automatisch verdeeld over veel opslagruimte knooppunten gebruikt het systeem **PartitionKey** .
Entiteiten waarvan de dezelfde **PartitionKey** worden opgeslagen op hetzelfde knooppunt. **RowKey** is de unieke ID van de entiteit binnen de partition waarbij deze hoort.

Als u wilt een entiteit toevoegen aan de tabel, een woordenlijstobject aan doorgeven de **invoegen\_entiteit** methode.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

U kunt ook een exemplaar van de klasse **entiteit** doorgeven de **invoegen\_entiteit** methode.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Een entiteit bijwerken

Deze code ziet hoe u de oude versie van een bestaande entiteit vervangen door een bijgewerkte versie.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Als de entiteit die wordt bijgewerkt niet bestaat, klikt u vervolgens de updatebewerking, mislukt. Als u wilt een entiteit ongeacht of deze vóór bevatte opslaan, voert u **invoegen\_of\_replace_entity**.
In het volgende voorbeeld wordt de eerste oproep vervangen door de bestaande entiteit. Het tweede gesprek een nieuwe entiteit, sinds geen entiteit met de opgegeven **PartitionKey** wordt ingevoegd en **RowKey** bestaat in de tabel.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Een groep entiteiten wijzigen

Soms is het handig om in te dienen meerdere bewerkingen samen in een batch om ervoor te zorgen atomaire verwerkt door de server. Voor het uitvoeren van dat, u de klasse **TableBatch gebruikt** . Wanneer u verzenden de batch wilt, dat u belt **doorvoeren\_batch**. Houd er rekening mee dat alle entiteiten in de dezelfde partition zijn moeten om te kunnen worden gewijzigd als batch. In het onderstaande voorbeeld worden twee entiteiten bij elkaar opgeteld in een batch.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Batches kunnen ook worden gebruikt met de syntaxis van de manager contex:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Query voor een entiteit

Als u wilt een entiteit in een tabel een query uitvoert, gebruikt u de **krijgen\_entiteit** methode door **PartitionKey** en **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Een reeks entiteiten query

In dit voorbeeld wordt gezocht naar dat alle taken in Seattle op basis van **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Query een subset van entiteitseigenschappen

Een query toevoegen aan een tabel kunt u slechts een paar eigenschappen ophalen uit een entiteit.
Deze techniek, *raming*, genaamd Hiermee reduceert u bandbreedte en prestaties van query's, met name voor grote entiteiten kunt verbeteren. Gebruik van de parameter **selecteren** en geven van de namen van de eigenschappen die u wilt overbrengen naar de klant.

De query in de volgende code retourneert alleen de beschrijvingen van entiteiten in de tabel.

[AZURE.NOTE] Het codefragment van de volgende werkt alleen ten opzichte van de cloudopslagservice. Dit wordt niet ondersteund door de emulator opslag.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Een entiteit verwijderen

U kunt een entiteit met behulp van de sleutel partition en rij verwijderen.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Een tabel verwijderen

Een tabel verwijdert met de volgende code uit een opslag-account.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Table storage hebt geleerd, volgt u deze koppelingen voor meer informatie.

- [Python Developer Center](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag teamblog]
- [Microsoft Azure-opslag SDK voor Python]

[Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-opslag SDK voor Python]: https://github.com/Azure/azure-storage-python
