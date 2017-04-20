<properties
    pageTitle="Azure gebeurtenis Hubs archief scenario | Microsoft Azure"
    description="Voorbeeld waarin de SDK van Azure Python om te laten zien met de functie gebeurtenis Hubs archief."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Gebeurtenis Hubs archief Stapsgewijze instructies: Python

Gebeurtenis Hubs archief is een nieuwe functie van de gebeurtenis Hubs waarmee u voor het leveren van de stream-gegevens automatisch in de Hub van uw evenement bij een account Azure-blobopslag van uw keuze. Hiermee kunt u gemakkelijk om uit te voeren batch processing op streaming realtimegegevens. In dit artikel wordt beschreven hoe u gebeurtenis Hubs archief met Python. Zie het [overzichtsartikel](event-hubs-archive-overview.md)voor meer informatie over de gebeurtenis Hubs archief.

In dit voorbeeld wordt de SDK van Azure Python gebruikt om te laten zien met de functie archief. De sender.py stuurt gesimuleerd milieu telemetrielogboek naar gebeurtenis Hubs in JSON-indeling. De Hub gebeurtenis is geconfigureerd voor het gebruik van de functie voor het archiveren om te schrijven van deze gegevens om blob-opslag in batches. De archivereader.py vervolgens deze BLOB's leest en Hiermee maakt u een bestand toevoegen per apparaat en schrijft de gegevens in de CSV-bestanden.

Wat wordt worden uitgevoerd

1.  Een Azure-blobopslag-account en een container blob binnen, met behulp van de Azure portal maken

2.  Een gebeurtenis Hub naamruimte, met behulp van de Azure portal maken

3.  De Hub van een gebeurtenis maken met de functie voor het archiveren ingeschakeld, met behulp van de Azure portal

4.  Gegevens verzenden naar de gebeurtenis Hub met een script Python

5.  De bestanden vanuit het archief lezen en deze te verwerken met een ander Python script

Vereisten voor

1.  Python 2.7.x

2.  Een Azure-abonnement

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Maak een account Azure Storage

1.  Meld u aan bij de [portal van Azure][].

2.  In het linkernavigatiedeelvenster van de portal, klik op Nieuw en vervolgens klikt u op gegevens + opslagruimte en klik vervolgens op opslag-Account.

3.  Vul de velden in het blad opslag-account en klik op **maken**.

    ![][1]

4.  Klik op **BLOB's**nadat u het bericht **Implementaties is geslaagd** , gaat u naar de nieuwe opslag-account en klik in het blad **Essentials** ziet. Wanneer het blad **Blob service** wordt geopend, klikt u op **+ Container** aan de bovenkant. De container **archief**een naam in en sluit het blad **Blob-service** .

5.  Klik op **toegangstoetsen** in het linker blad en kopieer de naam van de opslag-account en de waarde van **sleutel1**. Sla deze waarden naar Kladblok of een andere tijdelijke locatie.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Een script Python gebeurtenissen verzenden naar uw Hub gebeurtenis maken

1.  Open uw favoriete Python-editor, zoals [Visual Studio-Code][].

2.  Maak een script **sender.py**genoemd. Dit script stuurt 200 gebeurtenissen naar de Hub van de gebeurtenis. Deze zijn eenvoudige milieu metingen verzonden in JSON.

3.  Plak de volgende code in sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Werk de voorgaande code om uw naamruimtenaam en sleutelwaarden die u hebt aangeschaft bij het maken van de gebeurtenis Hubs naamruimte te gebruiken.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Een Python script maken om te lezen van uw bestanden archiveren

1.  Vul het blad en klik op **maken**.

2.  Maak een script **archivereader.py**genoemd. Dit script de archiefbestanden leest en maken van een bestand per apparaat om de gegevens alleen voor dat apparaat te schrijven.

3.  Plak de volgende code in archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Zorg ervoor dat u de juiste waarden voor uw opslagaccountnaam en van toetsen plakken in de oproep door naar `startProcessing`.

## <a name="run-the-scripts"></a>De scripts worden uitgevoerd

1.  Open een opdrachtprompt met Python in het pad en voer deze opdrachten Python vereiste pakketten installeren:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Als u een eerdere versie van een van beide azure-opslag hebt of azure mogelijk moet u gebruikt de **--upgraden** optie

    Mogelijk moet u ook de volgende handelingen uit te voeren (niet nodig op de meeste systemen):

    ```
    pip install cryptography
    ```

2.  Afhankelijk van waar u sender.py en archivereader.py opgeslagen wijzigen in uw adreslijst en deze opdracht uitvoert:

    ```
    start python sender.py
    ```
    
    Hiermee start u een nieuw Python proces om uit te voeren van de afzender.

3. Nu wacht enkele minuten duren voordat het archief om uit te voeren. Typ de volgende opdracht in uw oorspronkelijke opdrachtvenster:

    ```
    python archivereader.py
    ```

In dit archief processor wordt de lokale map om te downloaden van alle BLOB's van de opslagruimte account/container. Deze worden verwerkt door een die niet leeg zijn en de resultaten als CSV-bestanden naar de lokale map schrijven.

## <a name="next-steps"></a>Volgende stappen

U meer informatie over de gebeurtenis Hubs kunt via de volgende koppelingen:

- [Overzicht van de gebeurtenis Hubs archiveren][]
- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].
- De [schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs][] steekproef.
- [Overzicht van de Hubs gebeurtenissen][]
 

[Azure-portal]: https://portal.azure.com/
[Overzicht van de gebeurtenis Hubs archiveren]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio-Code]: https://code.visualstudio.com/
[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[De schaal van het verwerken van de gebeurtenis met gebeurtenis Hubs aanpassen]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
