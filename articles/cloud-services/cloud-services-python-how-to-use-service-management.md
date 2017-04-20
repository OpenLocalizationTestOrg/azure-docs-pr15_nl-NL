<properties
    pageTitle="Het gebruik van de API (Python) - functie handleiding voor het servicebeheer"
    description="Leer hoe u via programmacode beheertaken algemene service uit Python."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Het gebruik van servicebeheer van Python

> [AZURE.NOTE] API voor Management-service wordt vervangen met de nieuwe Resource Management API, momenteel beschikbaar in een preview-versie.  Zie de [documentatie van Azure resourcebeheer](http://azure-sdk-for-python.readthedocs.org/) voor meer informatie over het gebruik van de nieuwe Resource Management API van Python.

Deze handleiding ziet u hoe u via programmacode beheertaken algemene service uit Python. De klasse **ServiceManagementService** in de [SDK van Azure voor Python](https://github.com/Azure/azure-sdk-for-python) ondersteuning biedt voor toegang tot het veel van de service management-gerelateerde functionaliteit die beschikbaar is in de [portal van Azure klassieke] [ management-portal] (zoals **maken, bijwerken en verwijderen van cloudservices, implementaties, data management-services, en virtuele machines**). Deze functionaliteit is handig in het bouwen van toepassingen die toegang tot het servicebeheer nodig zijn.

## <a name="WhatIs"> </a>Wat servicebeheer is
De API voor het beheer van Service biedt toegang tot het veel van de functie Servicebeheer beschikbaar via de [portal van Azure klassieke][management-portal]. De SDK Azure voor Python kunt u uw cloudservices en opslag accounts beheren.

Als u wilt de Service Management-API gebruiken, moet u [een Azure-account maken](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Concepten
De SDK Azure voor Python terugloopt de [API voor beheer van Azure-Service][svc-mgmt-rest-api], namelijk een REST API. Alle API-bewerkingen worden uitgevoerd via SSL en elkaar wederzijds geverifieerd met behulp van X.509 v3-certificaten. De management-service kan worden geopend vanuit binnen een service in Azure wordt aangegeven of rechtstreeks via Internet uitgevoerd vanuit elke toepassing die u kunt een HTTPS-aanvraag verzendt en ontvangt een antwoord HTTPS.

## <a name="Installation"> </a>Installatie

Alle functies die worden beschreven in dit artikel zijn beschikbaar in de `azure-servicemanagement-legacy` pakket, die u kunt installeren met behulp van pip. Voor meer informatie over de installatie (bijvoorbeeld als u nog niet eerder met Python), raadpleegt u in dit artikel: [Python installeren en de SDK van Azure](../python-how-to-install.md)

## <a name="Connect"> </a>Hoe: verbinding maken met servicebeheer
Als u wilt verbinden met het eindpunt servicebeheer, moet u uw Azure abonnements-ID en een geldig management-certificaat. U kunt uw abonnements-ID via de [portal van Azure klassieke]aanvragen[management-portal].

> [AZURE.NOTE] Sinds Azure SDK voor Python v0.8.0 is het nu mogelijk gebruik van certificaten die zijn gemaakt met OpenSSL wanneer u zich in Windows.  Er moeten Python punt 2.7.4 of hoger. U wordt aangeraden gebruikers OpenSSL gebruiken in plaats van .pfx, aangezien de ondersteuning voor .pfx certificaten waarschijnlijk in de toekomst worden verwijderd.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Beheer van certificaten op de Mac-Windows-Linux (OpenSSL)
U kunt [OpenSSL](http://www.openssl.org/) gebruiken om uw management-certificaat te maken.  Moet u twee certificaten, één telefoonnummer voor de server hebt gemaakt (een `.cer` bestand) en een voor de client (een `.pem` bestand). U maakt de `.pem` bestand, uitvoeren:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

U maakt de `.cer` certificaat en uitvoeren:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Zie voor meer informatie over Azure certificaten [Certificaten overzicht Azure-Cloudservices](./cloud-services-certs-create.md). Zie de documentatie op [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)voor een gedetailleerde beschrijving van OpenSSL parameters.

Nadat u deze bestanden hebt gemaakt, moet u uploaden de `.cer` bestand naar Azure via de bewerking 'Uploaden' van het tabblad "Instellingen" van de [Azure klassieke portal][management-portal], en moet u waar u opgeslagen Noteer de `.pem` bestand.

Als u beschikt over uw abonnements-ID, een certificaat hebt gemaakt en die zijn geüpload, de `.cer` bestand Azure u verbinding kunt maken naar het eindpunt Azure management door de abonnements-id en het pad naar de `.pem` te **ServiceManagementService**bestand:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Klik in het voorgaande voorbeeld, `sms` is een object **ServiceManagementService** . De **ServiceManagementService** -klasse is de primaire klas gebruikt voor het beheren van Azure services.

### <a name="management-certificates-on-windows-makecert"></a>Beheer van certificaten op Windows (MakeCert)

U kunt een zelfondertekend certificaat maken over het gebruik van uw computer `makecert.exe`.  Open een **opdrachtprompt Visual Studio** als een **beheerder** en gebruik van de volgende opdracht, *AzureCertificate* vervangen door de certificaatnaam van het dat u wilt gebruiken.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Hiermee maakt u de opdracht de `.cer` bestand en installeert u deze in het **persoonlijke** archief. Zie [Overzicht van de certificaten voor Azure Cloud Services](./cloud-services-certs-create.md)voor meer informatie.

Nadat u het certificaat hebt gemaakt, moet u uploaden de `.cer` bestand naar Azure via de bewerking 'Uploaden' van het tabblad "Instellingen" van de [Azure klassieke portal][management-portal].

Als u beschikt over uw abonnements-ID, een certificaat hebt gemaakt en die zijn geüpload, de `.cer` bestand Azure u verbinding kunt maken naar het eindpunt Azure management door het doorgeven van de abonnements-id en de locatie van het certificaat in uw **persoonlijke** archief aan **ServiceManagementService** (opnieuw, vervangt u *AzureCertificate* met de naam van uw certificaat):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Klik in het voorgaande voorbeeld, `sms` een object **ServiceManagementService** . De **ServiceManagementService** -klasse is de primaire klas gebruikt voor het beheren van Azure services.

## <a name="ListAvailableLocations"> </a>Hoe: een lijst met beschikbare locaties

U kunt de locaties die beschikbaar zijn voor hostingservices gebruiken de **lijst\_locaties** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Als u een cloudservice binnenkomt of opslagservice maakt die u moet een geldige locatie opgeeft. De **lijst\_locaties** methode retourneert altijd een bijgewerkte lijst van de momenteel beschikbare locaties. Schrijven van dit zijn de beschikbare locaties:

- West Europa
- Noord-Europa
- Zuidoost-Azië
- Oost-Azië
- Central US
- Centraal Noord-Amerikaanse
- Zuid-Central US
- West VS
- Oost-VS
- Japan Oost
- Japan West
- Brazilië Zuid
- Australië Oost
- Australië Zuidoost

## <a name="CreateCloudService"> </a>Hoe: een cloudservice maken

Wanneer u een toepassing maken en uitvoeren in Azure wordt aangegeven, worden de code en de configuratie bij elkaar genoemd een Azure- [cloudservice][] (een *service wordt gehost* in eerdere versies van Azure genoemd). De **maken\_gehost\_service** methode kunt u een nieuwe gehoste service maken door de servicenaam van een gehoste (die uniek zijn in Azure), een label (automatisch gecodeerd naar base64), een beschrijving en een locatie te geven.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

U kunt een lijst met alle gehoste services voor uw abonnement met de **lijst\_gehost\_services** methode:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Als u wilt dat informatie over een bepaalde gehoste service, kunt u doen door de naam van de gehoste service aan de **krijgen\_gehost\_service\_eigenschappen** methode:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Nadat u een cloudservice hebt gemaakt, kunt u uw code implementeren bij de service met de **maken\_implementatie** methode.

## <a name="DeleteCloudService"> </a>Hoe: een cloudservice verwijderen

U kunt een cloudservice verwijderen door de naam van de service aan de **verwijderen\_gehost\_service** methode:

    sms.delete_hosted_service('myhostedservice')

Voordat u een service verwijderen kunt, moeten u eerst alle implementaties voor de service verwijderen. (Zie [hoe: verwijderen van een implementatie](#DeleteDeployment) voor meer informatie.)

## <a name="DeleteDeployment"> </a>Hoe: een implementatie verwijderen

Als u wilt verwijderen van een implementatie, gebruikt u de **verwijderen\_implementatie** methode. Het volgende voorbeeld ziet u hoe u het verwijderen van een implementatie met de naam `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Hoe: Maak een storage-service

Een [storage-service](../storage/storage-create-storage-account.md) biedt u toegang tot Azure [BLOB's](../storage/storage-python-how-to-use-blob-storage.md), [tabellen](../storage/storage-python-how-to-use-table-storage.md)en [wachtrijen](../storage/storage-python-how-to-use-queue-storage.md). Als u wilt een storage-service hebt gemaakt, moet u eerst een naam voor de service (tussen 3 en 24 kleine letters en uniek zijn binnen Azure), een beschrijving een label (maximaal 100 tekens, automatisch codering naar base64) en een locatie. Het volgende voorbeeld wordt getoond hoe een opslagservice maken door een locatie.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Opmerking in het voorgaande voorbeeld die de status van de **maken\_opslag\_account** bewerking kan worden opgehaald door het doorgeven van het resultaat **maken\_opslag\_account** naar de **krijgen\_bewerking\_status** methode.  

U kunt uw opslag-accounts en hun eigenschappen met lijst de **lijst\_opslag\_accounts** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Hoe: een storage-service verwijderen

U kunt een storage-service verwijderen door de naam van de service opslag naar de **verwijderen\_opslag\_account** methode. Een opslagservice verwijdert, worden alle gegevens die zijn opgeslagen in de service (BLOB's, tabellen en wachtrijen).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Hoe: een lijst met beschikbare besturingssystemen

U kunt de besturingssystemen die beschikbaar zijn voor hostingservices gebruiken de **lijst\_besturingsomgeving\_systemen** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

U kunt ook de **lijst\_besturingsomgeving\_systeem\_gezinnen** methode, die de besturingssystemen door huishouden van groepen:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Hoe: Maak een afbeelding van het besturingssysteem

Voeg een afbeelding van het besturingssysteem naar de bibliotheek van de afbeelding via de **toevoegen\_os\_afbeelding** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Als u de afbeeldingen van het besturingssysteem die beschikbaar zijn, gebruikt u de **lijst\_os\_afbeeldingen** methode. Het bevat alle platform afbeeldingen en gebruikersafbeeldingen:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Hoe: een afbeelding van het besturingssysteem verwijderen

Als u wilt de afbeelding van een gebruiker verwijderen, gebruikt u de **verwijderen\_os\_afbeelding** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Hoe: een virtuele machine maken

Als u wilt een virtuele machine hebt gemaakt, moet u eerst een [cloudservice](#CreateCloudService)maken.  Maak vervolgens de VM implementatie via de **maken\_virtual\_machine\_implementatie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Hoe: een virtuele machine verwijderen

Als u wilt verwijderen van een virtuele machine, u eerst verwijdert de implementatie via de **verwijderen\_implementatie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

De cloudservice kunt u het verwijderen met de **verwijderen\_gehost\_service** methode:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Procedure: Een virtuele Machine uit een afbeelding vastgelegde VM maken

Een afbeelding VM vastleggen, belt u eerst de **vastleggen\_vm\_afbeelding** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Gebruik vervolgens om er zeker van te zijn dat u de afbeelding wel hebt vastgelegd, de **lijst\_vm\_afbeeldingen** api, en zorg ervoor dat uw afbeelding wordt weergegeven in de resultaten:

    images = sms.list_vm_images()

U kunt de virtuele machine met de vastgelegde afbeelding ten slotte maken de **maken\_virtual\_machine\_implementatie** , zoals voordat, maar klik nu doorgeven in de vm_image_name

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Zie voor meer informatie over het vastleggen van een Linux virtuele Machine [Linux virtuele machines vastleggen.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Zie meer informatie over het vastleggen van een virtuele Windows-computer, [een virtuele Windows-computer vastleggen.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Vervolgstappen

U kunt de basisbeginselen van servicebeheer hebt geleerd, kunt u toegang tot de [volledige API-documentatie bij de SDK van Azure Python](http://azure-sdk-for-python.readthedocs.org/) en complexe taken gemakkelijk als u wilt beheren van uw toepassing python uitvoeren.

Zie het [Python Developer Center](/develop/python/)voor meer informatie.

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloudservice]:/services/cloud-services/

