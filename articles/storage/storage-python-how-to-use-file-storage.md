<properties
    pageTitle="Het gebruik van Azure bestandsopslag uit Python | Microsoft Azure"
    description="Informatie over het gebruik van de bestandsopslag Azure uit Python uploaden, lijst, downloaden en verwijder bestanden."
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

# <a name="how-to-use-azure-file-storage-from-python"></a>Het gebruik van Azure bestandsopslag uit Python

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u veelvoorkomende scenario's met bestandsopslag uitvoeren. In de voorbeelden in Python zijn geschreven en gebruikt u de [Microsoft Azure opslag SDK voor Python]. De scenario's waarvoor zijn geüpload, vermelding, downloaden en verwijderen van bestanden.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Een gedeelde map maken

Het object **FileService** kunt u werken met waarden voor aandelen, mappen en bestanden. De volgende code maakt een object **FileService** . De volgende Klik boven aan elke Python bestand waarin u via programmacode toegang hebben tot Azure opslag wilt toevoegen.

    from azure.storage.file import FileService

De volgende code maakt een **FileService** -object met de naam en account-toets voor de account voor opslag.  'Mijn account' en 'mykey' vervangen door uw accountnaam en van toetsen.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Klik in het volgende voorbeeld, kunt u een object **FileService** maken de optie voor delen als deze niet bestaat.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Een bestand uploaden naar een delen

Een opslag van Azure bestandsshare op ieder geval bevat een hoofdmap waar de bestanden kunnen zich bevinden. In dit gedeelte leert u hoe u een bestand vanaf een lokale opslag naar de hoofdmap van een delen uploaden.

Voor het maken van een bestand en gegevens uploaden, gebruikt u de **maken\_bestand\_uit\_pad**, **maken\_bestand\_uit\_stream**, **maken\_bestand\_uit\_bytes** of **maken\_bestand\_uit\_tekst** methoden. Ze zijn op hoog niveau methoden die de benodigde logische groepen te verdelen wanneer de grootte van de gegevens groter is dan 64 MB uitvoeren.

**maken\_bestand\_uit\_pad** uploadt u de inhoud van een bestand vanaf het opgegeven pad, en **maken\_bestand\_uit\_stream** uploadt u de inhoud van een al geopende bestand/stream. **maken\_bestand\_uit\_bytes** een matrix van bytes, uploads en **maken\_bestand\_uit\_tekst** uploadt u de waarde van de opgegeven tekst met de opgegeven codering (standaard UTF-8).

In het volgende voorbeeld wordt de inhoud van het bestand **sunset.png** uploads in het bestand **mijnbestand** .

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Hoe u: een map maken

U kunt ook opslag indelen door te plaatsen van bestanden in submappen in plaats van dat alle labels in de hoofdmap. De Azure bestand storage-service kunt u net zo veel mappen maken als uw account wordt toestaan. De onderstaande code maakt een submap met de naam **sampledir** onder de hoofdmap.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Hoe u: een lijst met bestanden en mappen in een delen

U kunt de bestanden en mappen in een delen gebruiken de **lijst\_mappen\_en\_bestanden** methode. Deze methode geeft als resultaat een genereren. De volgende code wordt de **naam** van elke bestanden en mappen in een delen aan de console.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Bestanden downloaden

Als u wilt gegevens uit een bestand downloaden **ophalen\_bestand\_naar\_pad**, **ophalen\_bestand\_naar\_stream**, **ophalen\_bestand\_naar\_bytes**, of **krijgen\_bestand\_naar\_tekst**. Ze zijn op hoog niveau methoden die de benodigde logische groepen te verdelen wanneer de grootte van de gegevens groter is dan 64 MB uitvoeren.

Het volgende voorbeeld worden **krijgen\_bestand\_naar\_pad** de inhoud van het bestand **mijnbestand** downloaden en naar het bestand **out-sunset.png** opslaat.

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Een bestand verwijderen

Ten slotte, als u wilt een bestand verwijderen, belt u **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van bestandsopslag hebt geleerd, volgt u deze koppelingen voor meer informatie.

- [Python Developer Center](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag teamblog]
- [Microsoft Azure-opslag SDK voor Python]

[Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure-opslag SDK voor Python]: https://github.com/Azure/azure-storage-python
