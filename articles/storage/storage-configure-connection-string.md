<properties 
    pageTitle="Een verbindingsreeks met Azure Storage configureren | Microsoft Azure"
    description="Een verbindingsreeks bij een Azure opslag-account configureren. Een verbindingsreeks bevat de vereiste informatie voor access met een account voor de opslag van uw toepassing gedurende runtime verifiëren."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Azure opslag verbindingstekenreeksen configureren

## <a name="overview"></a>Overzicht

Een verbindingsreeks bevat de vereiste toegang tot gegevens in een account Azure opslag van uw toepassing gedurende runtime verificatie-informatie. U kunt een verbindingsreeks om te configureren:

- Verbinding maken met de emulator Azure opslag.
- Toegang tot een account opslagruimte in Azure wordt aangegeven.
- Toegang tot de opgegeven resources in Azure wordt aangegeven via de handtekening van een gedeelde access (SA's).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>De verbindingsreeks opslaan

Uw toepassing moet voor toegang tot de verbindingsreeks gedurende runtime om te verifiëren aanvragen voor Azure Storage. Hebt u een aantal verschillende opties voor het opslaan van uw verbindingsreeks:

- U kunt de verbindingsreeks in opslaan voor een toepassing actief is op het bureaublad of op een apparaat, een `app.config `bestand of een `web.config` bestand. De verbindingsreeks toevoegen aan de sectie **AppSettings** .
- Voor een toepassing die wordt uitgevoerd in een Azure-cloudservice, kunt u de verbindingsreeks in het [schema (.cscfg) van Azure-service configuratiebestand](https://msdn.microsoft.com/library/ee758710.aspx)opslaan. De verbindingsreeks toevoegen aan de sectie **ConfigurationSettings** van het configuratiebestand service.
- U kunt ook de verbindingsreeks rechtstreeks in uw code. Voor de meeste scenario's echter wordt aangeraden de configuratietekenreeks in een configuratiebestand op te slaan.

De verbindingsreeks in een configuratiebestand opslaan kunt u gemakkelijk de verbindingsreeks om te schakelen tussen de emulator opslag en een account Azure opslag in de cloud bijwerken. U moet alleen de verbindingsreeks te laten verwijzen naar uw doelomgeving.

U kunt de klas [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) gebruiken voor toegang tot uw verbindingsreeks gedurende runtime ongeacht waar uw toepassing wordt uitgevoerd.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Een verbindingsreeks voor de opslag emulator maken

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Zie [gebruiken de Emulator Azure opslag voor ontwikkeling en testen](storage-use-emulator.md) voor meer informatie over de opslag emulator.

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Een verbindingsreeks bij een Azure opslag-account maken

Als u wilt een verbindingsreeks bij uw account Azure opslag maken, gebruikt u onderstaande indeling van de verbindingsreeks. Aangeven of u wilt verbinden met de opslag-account via HTTPS (aanbevolen) of HTTP, vervangen `myAccountName` met de naam van uw account voor opslagruimte en vervang `myAccountKey` met uw account toegangstoets:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Bijvoorbeeld de verbindingsreeks ziet er ongeveer uit het volgende voorbeeld-verbindingsreeks:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure opslag ondersteunt HTTP- en HTTPS in een verbindingsreeks; echter wordt met HTTPS ten zeerste aanbevolen.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Een verbindingsreeks met een gedeelde access-handtekening maken

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Een verbindingsreeks voor een expliciete opslag-eindpunt maken

U kunt de eindpunten van de service expliciet opgeven in de verbindingsreeks in plaats van de Standaardeindpunten. Als u wilt een verbindingsreeks waarmee een expliciete eindpunt maken, geeft u het volledige service-eindpunt voor elke service, inclusief de protocolspecificatie (HTTPS (aanbevolen) of HTTP), in de volgende indeling:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Een scenario waar wilt u mogelijk een expliciete eindpunt Geef is als u uw Blob storage eindpunt hebt toegewezen aan een aangepast domein. U kunt in dat geval geeft u uw aangepaste eindpunt voor Blob storage in de verbindingsreeks en eventueel de eindpunten van de standaardinstelling voor de andere service opgeven als deze in uw toepassing worden gebruikt.

Hier ziet voorbeelden van geldige verbindingstekenreeksen die een expliciete eindpunt voor de service Blob opgeven:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

De waarde van het eindpunt dat wordt vermeld in de verbindingstekenreeks wordt gebruikt om het verzoek URI's met de Blob-service te maken en deze de vorm van een URI's die worden geretourneerd naar uw code bepaalt.

Houd er rekening mee dat als u besluit om een service-eindpunt uit de verbindingsreeks weglaat, klikt u vervolgens is niet mogelijk om te gebruiken die verbindingsreeks voor toegang tot gegevens in die service vanuit uw code.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Een verbindingsreeks maken met een achtervoegsel eindpunt

Als u wilt maken van een verbindingsreeks voor storage-service in regio's of exemplaren met verschillende eindpunt achtervoegsels, zoals voor Azure China of Azure beheermodel, gebruikt u de indeling van de volgende verbindingsreeks. Aangeven of u wilt verbinden met de opslag-account via HTTP of HTTPS, vervangen `myAccountName` vervangen door de naam van uw account opslag, `myAccountKey` met uw account toegangstoets en vervangen `mySuffix` met het achtervoegsel URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Bijvoorbeeld de verbindingsreeks lijken te zijn op de volgende verbindingsreeks:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Parseren van een verbindingsreeks

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Volgende stappen

- [Gebruik de Emulator Azure opslag voor ontwikkelen en testen](storage-use-emulator.md)
- [Azure opslag Explorer-vensters vertegenwoordigen](storage-explorers.md)
- [Gedeelde toegang handtekeningen (SA's) gebruiken](storage-dotnet-shared-access-signature-part-1.md)
