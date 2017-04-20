<properties
    pageTitle="Het gebruik van de wachtrij opslag van Python | Microsoft Azure"
    description="Informatie over het gebruik van de wachtrij Azure-service van Python maken en wachtrijen, verwijderen en invoegen, krijgen en berichten verwijderen."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Het gebruik van de wachtrij opslag van Python

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u het uitvoeren van veelvoorkomende scenario's voor het gebruik van de Azure wachtrij storage-service. In de voorbeelden in Python zijn geschreven en gebruikt u de [Microsoft Azure opslag SDK voor Python]. De scenario's waarvoor opnemen **Invoegen**, **inspecteren**, **ophalen**en **verwijderen van** berichten in wachtrij plaatsen, evenals **maken en verwijderen van wachtrijen**. Voor meer informatie over wachtrijen, raadpleegt u de sectie [Vervolgstappen].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken

Het object **QueueService** kunt u werken met wachtrijen. De volgende code maakt een object **QueueService** . Voeg de volgende Klik boven aan elke Python bestand waarin u wilt via programmacode toegang hebben tot Azure opslag:

    from azure.storage.queue import QueueService

De volgende code maakt een **QueueService** -object met de naam en account-toets voor de account voor opslag. 'Mijn account' en 'mykey' vervangen door uw accountnaam en van toetsen.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht invoegen in een wachtrij

U een bericht in een wachtrij invoegt, gebruikt u de **plaatsen\_bericht** methode naar een nieuw bericht maken en toevoegen aan de wachtrij.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Procedure: Bekijken van het volgende bericht

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij door de ondersteuning voor de **korte weergave\_berichten** methode. Standaard **korte weergave\_berichten** peeks aan één bericht.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Procedure: Berichten in wachtrij

Uw code Hiermee verwijdert u een bericht uit een wachtrij in twee stappen. Wanneer u belt **krijgen\_berichten**, u het volgende bericht openen in een wachtrij al dan niet standaard. Een bericht dat wordt geretourneerd uit **krijgen\_berichten** onzichtbare naar een andere code lezen van berichten van deze wachtrij verandert. Al dan niet standaard blijft dit bericht onzichtbare gedurende 30 seconden. Als u klaar bent met het bericht verwijderen uit de wachtrij, wilt u ook moet bellen **verwijderen\_bericht**. In dit proces van het verwijderen van een bericht zorgt ervoor dat wanneer uw code mislukt verwerkingstijd van een bericht vanwege hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw oproepen code **verwijderen\_bericht** zodra het bericht is verwerkt.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Er zijn twee manieren kunt u voor het bericht ophalen uit een wachtrij aanpassen.
U gaat eerst een reeks berichten (maximaal 32). Tweede, kunt u een langer of korter invisibility time-out instellen, zodat uw code meer of minder tijd volledig verwerkingstijd van elk bericht. De volgende code voorbeeld wordt de **krijgen\_berichten** methode 16 berichten in een oproep ontvangt. Vervolgens wordt verwerkt elk bericht met een lus. Ook wordt de time-out invisibility ingesteld op vijf minuten voor elk bericht.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht in-place in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie kunt gebruiken bij de status van de werktaak. De code hieronder worden gebruikt de **bijwerken\_bericht** methode voor het bijwerken van een bericht. De time-out voor zichtbaarheid is ingesteld op 0, wat betekent het bericht onmiddellijk wordt weergegeven en de inhoud wordt bijgewerkt.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Procedure: Lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij krijgen. De **krijgen\_wachtrij\_metagegevens** methode wordt gevraagd om de wachtrijservice om terug te keren metagegevens over de wachtrij en de **approximate_message_count**. Het resultaat is alleen bij benadering omdat berichten kunnen worden toegevoegd of verwijderd nadat de wachtrijservice moet op uw aanvraag kunt invullen reageren.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen

Als u wilt verwijderen van een wachtrij en alle bijbehorende berichten die dit, bellen de **verwijderen\_wachtrij** methode.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van wachtrij opslagruimte hebt geleerd, volgt u deze koppelingen voor meer informatie.

- [Python Developer Center](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag teamblog]
- [Microsoft Azure-opslag SDK voor Python]

[Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-opslag SDK voor Python]: https://github.com/Azure/azure-storage-python
