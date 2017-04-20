<properties
    pageTitle="Gebruik van de Azure CLI met Azure opslag | Microsoft Azure"
    description="Informatie over het gebruik van de opdrachtregel Azure (Azure CLI) met Azure-opslag te maken en beheren van opslag accounts en werken met Azure BLOB's en bestanden. De CLI Azure is een functie platforms "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Gebruik van de Azure CLI met Azure opslag

## <a name="overview"></a>Overzicht

De CLI Azure biedt een reeks bron openen, platforms de opdrachten voor het werken met het Azure-Platform. Deze biedt veel dezelfde functies zijn gevonden in de [portal van Azure](https://portal.azure.com) , evenals de functionaliteit in access uitgebreide gegevens.

In deze handleiding, bekijken we het gebruik van [Azure opdrachtregel-Interface (Azure CLI)](../xplat-cli-install.md) om uit te voeren allerlei ontwikkeling en beheer van de taken met Azure opslagmedia. Het is aan te raden wij u downloaden en installeren of naar de meest recente Azure CLI upgraden vóór het gebruik van deze handleiding.

Deze handleiding wordt ervan uitgegaan dat u de basisconcepten van Azure Storage kennen. De gids biedt een aantal scripts om te laten zien van het gebruik van de Azure CLI met Azure opslagmedia. Zorg ervoor dat de scriptvariabelen op basis van uw configuratie vóór elke script uitvoeren.

> [AZURE.NOTE] De handleiding bevat de Azure CLI voor opdrachten en parameters script voorbeelden voor klassieke opslag-accounts. Zie [gebruik van de Azure CLI voor Mac, Linux, en Windows met Azure resourcebeheer](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) voor Azure CLI opdrachten voor resourcemanager opslag-accounts.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Aan de slag met Azure opslagcapaciteit en de CLI Azure 5 minuten

Deze handleiding Ubuntu voor voorbeelden wordt gebruikt, maar andere platforms OS op dezelfde manier moeten uitvoeren.

**Ervaring met Azure:** Krijg een Microsoft Azure-abonnement en een Microsoft-account dat is gekoppeld aan dit abonnement. Zie voor informatie over opties voor Azure aankoop [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/), [Opties aanschaffen](https://azure.microsoft.com/pricing/purchase-options/)en [Lid biedt](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, Microsoft Partner Network, en BizSpark en andere Microsoft-programma's).

Zie [beheerdersrollen toewijzen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) voor meer informatie over Azure abonnementen.

**Na het maken van een abonnement op Microsoft Azure en account:**

1. Download en installeer de Azure CLI de aanwijzingen in [de CLI Azure installeren](../xplat-cli-install.md).
2. Zodra de CLI Azure is geïnstalleerd, wordt u gebruikmaken van de azure opdracht vanaf de opdrachtregel (Terminal, opdrachtprompt we vaker doen,) voor toegang tot de opdrachten Azure CLI zijn. Type `azure` voor opdrachten en u ziet het volgende resultaat.

    ![Azure opdrachtuitvoer][Image1]

3. Typ in de gebruikersinterface van de regel opdracht `azure storage` bevat een lijst met alle opdrachten van de azure opslag en ophalen van een eerste indruk van de functies van de CLI Azure. U kunt de naam van de opdracht met de parameter **-h** typen (bijvoorbeeld `azure storage share create -h`) details van de opdrachtsyntaxis van de kunnen zien.
4. Nu krijgt u een eenvoudig script met basisopdrachten Azure CLI toegang hebben tot Azure opslag. Het script eerst vraagt u om in te stellen van twee variabelen voor uw account voor opslagruimte en van toetsen. Het script wordt vervolgens een nieuwe container maken in deze nieuwe opslag-account en een afbeeldingsbestand (blob) uploaden naar dat onderdeel. Nadat het script alle BLOB's in die container lijsten, worden het afbeeldingsbestand gedownload naar de doelmap dat op de lokale computer bestaat.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Open in uw lokale computer, uw voorkeur teksteditor (vim bijvoorbeeld). Typ het bovenstaande script in de teksteditor.

6. U moet nu de scriptvariabelen op basis van uw configuratie-instellingen bijwerken.

    - **< storage_account_name >** Gebruik van de opgegeven naam in het script of voer een nieuwe naam voor uw account opslag. **Belangrijke:** De naam van de opslag-account moet uniek zijn in Azure wordt aangegeven. Dit moet kleine letters, ook!

    - **< storage_account_key >** De access-toets van uw account opslag.

    - **< container_name >** Gebruik van de opgegeven naam in het script of typ een nieuwe naam voor de container.

    - **< image_to_upload >** Geef een pad op naar een afbeelding op uw lokale computer, zoals: "~ / images/HelloWorld.png".

    - **< destination_folder >** Geef een pad op naar een lokale map voor het opslaan van bestanden die zijn gedownload van Azure-opslag, zoals: "~/downloadImages".

7. Nadat u de benodigde variabelen in vim hebt bijgewerkt, drukt u op toetsencombinaties "Esc,:, wq!" het script opslaan.

8. Als u wilt deze script uitvoeren, typt u de naam van het script-bestand in de console we vaker doen. Wanneer dit script is uitgevoerd, kunt u een lokale doelmap waarin het gedownloade bestand nodig hebt. De volgende schermafbeelding ziet u een voorbeeld-uitvoer:

Wanneer het script is uitgevoerd, kunt u een lokale doelmap waarin het gedownloade bestand nodig hebt.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Opslag-accounts met de CLI Azure beheren

### <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement

Tijdens het grootste deel van de opdrachten opslag werkt zonder een abonnement op Azure, raden we u aan verbinding maken met uw abonnement van de Azure CLI. Volg de stappen in [verbinding maken met een Azure-abonnement van de Azure CLI](../xplat-cli-connect.md)configureren de CLI Azure voor gebruik met uw abonnement.

### <a name="create-a-new-storage-account"></a>Maak een nieuwe opslag-account

Als u wilt gebruiken Azure opslag, moet u een opslag-account. Nadat u verbinding maken met uw abonnement op de computer hebt geconfigureerd, kunt u een nieuw Azure opslag-account maken.

        azure storage account create <account_name>

De naam van uw account opslag moet tussen 3 en 24 tekens bevatten en gebruiken van getallen en alleen kleine letters.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Een standaardaccount voor Azure opslagruimte in omgevingsvariabelen instellen

U kunt meerdere opslag-accounts hebben in uw abonnement. U kunt kiest u een van deze en stel deze in de omgevingsvariabelen voor alle opdrachten in de opslagruimte in dezelfde sessie. Hiermee kunt u de opdrachten van Azure CLI opslag uitvoeren zonder op te geven van de opslag-account en belangrijke expliciet.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Een andere manier om een opslag standaardaccount instellen met het gebruik van de verbindingsreeks. U eerst de verbindingsreeks op opdracht:

        azure storage account connectionstring show <account_name>

Vervolgens Kopieer de verbindingsreeks uitvoer en stel deze in op omgevingsvariabele:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Maken en beheren van BLOB 's

Azure-blobopslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die toegankelijk is vanuit een willekeurige plaats in de wereld via HTTP of HTTPS. In deze sectie wordt ervan uitgegaan dat u al bekend met de Azure Blob storage concepten bent. Zie [aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md) en [Blob Service concepten](http://msdn.microsoft.com/library/azure/dd179376.aspx)voor gedetailleerde informatie.

### <a name="create-a-container"></a>Een container maken

Elke blob in Azure opslag moet zich in een container. U kunt maken met een privé container met de `azure storage container create` opdracht:

        azure storage container create mycontainer

> [AZURE.NOTE] Er zijn drie toegangsniveaus anonieme lezen: **uitschakelen**, **Blob**en **Container**. Als u wilt voorkomen dat anonieme toegang BLOB's, door de parameter van de machtiging in te stellen op **uitschakelen**. Standaard is de nieuwe container privé is en kan alleen worden geopend door de eigenaar van het account. Anonieme toegang in de openbare gelezen blob-resources, maar niet in de metagegevens van de container of aan de lijst met BLOB's in de container, stelt u de parameter machtiging naar **Blob**toestaan. Container metagegevens en de lijst met BLOB's in de container, wordt de parameter machtiging u volledige openbare leestoegang voor blob resources, stellen aan **Container**. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)voor meer informatie.

### <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Azure-blobopslag ondersteunt blok BLOB's en pagina BLOB's. Zie [lidmaatschap blok BLOB's, BLOB's toevoegen en BLOB van pagina's](http://msdn.microsoft.com/library/azure/ee691964.aspx)voor meer informatie.

Als u wilt uploaden BLOB's in naar een container, kunt u de `azure storage blob upload`. Standaard wordt met deze opdracht de lokale bestanden uploadt naar een blob blokkeren. Als u wilt opgeven van het type voor de blob, kunt u de `--blobtype` parameter.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>BLOB's downloaden van een container

Het volgende voorbeeld wordt het downloaden van BLOB's uit een container.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>BLOB's kopiëren

U kunt BLOB's binnen een of meer opslagruimte accounts en regio's asynchroon kopiëren.

Het volgende voorbeeld laat zien hoe BLOB's van de ene opslag-account naar een andere kopiëren. In dit voorbeeld maken we een container waar BLOB's openbaar, zijn anoniem toegankelijk zijn.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

In dit voorbeeld kunt u een kopie asynchroon uitvoeren. U kunt de status van elke kopieeropdracht controleren door te voeren de `azure storage blob copy show` bewerking.

Opmerking die de bron-URL is opgegeven voor de kopieeropdracht zijn toegankelijk voor het publiek, of een token SA's (gedeelde toegang handtekening) opnemen.

### <a name="delete-a-blob"></a>Een blob verwijderen

Als u wilt een blob verwijderen, gebruikt u de onder opdracht:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Maken en beheren van gedeelde bestanden

Azure bestandsopslag biedt gedeelde opslag voor toepassingen via het standaard SMB-protocol. Microsoft Azure virtuele machines en cloudservices, evenals de on-premises implementatie-toepassingen, kunnen delen van gegevens uit een bestand via gekoppelde waarden voor aandelen. U kunt bestandsshares en gegevens uit een bestand via de CLI Azure beheren. Zie voor meer informatie over de opslag van Azure-bestanden, [aan de slag met Azure bestandsopslag in Windows](storage-dotnet-how-to-use-files.md) of [het gebruik van Azure bestandsopslag met Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Een bestandsshare maken

Een bestandsshare van Azure is een bestandsshare SMB in Azure wordt aangegeven. Alle mappen en bestanden moeten worden gemaakt in een bestandsshare. Een account kan bevatten een onbeperkt aantal waarden voor aandelen en een delen kan een onbeperkt aantal bestanden, tot aan de grenzen van de capaciteit van het account opslagruimte opslaan. Het volgende voorbeeld wordt een bestandsshare **mijnshare**.

        azure storage share create myshare

### <a name="create-a-directory"></a>Een map maken

Een map biedt een optioneel hiërarchische structuur voor een Azure bestandsshare. Het volgende voorbeeld wordt de map **myDir** in het bestand delen.

        azure storage directory create myshare myDir

Opmerking deze mappad kunt opnemen meerdere niveaus, *bijvoorbeeld* **een / b**. Echter, moet u ervoor zorgen dat alle bovenliggende mappen bestaat. Bijvoorbeeld: voor pad **een / b**, moet u directory **een** eerst maken en vervolgens directory **b**maken.

### <a name="upload-a-local-file-to-directory"></a>Een lokaal bestand uploaden naar map

Het volgende voorbeeld een bestand uit **~/temp/samplefile.txt** naar de map **myDir** geüpload. Bewerk het bestandspad zodat deze naar een geldig bestand op uw lokale computer verwijst:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Houd er rekening mee dat een bestand in de optie voor delen kan maximaal 1 TB grootte.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Een lijst met de bestanden in de hoofdmap van delen of de map

U kunt een lijst met de bestanden en submappen in de hoofdmap van een delen of een map met de volgende opdracht:

        azure storage file list myshare myDir

Houd er rekening mee dat de mapnaam optioneel voor de aanbieding-bewerking is. Indien weggelaten, wordt de opdracht de inhoud van de hoofdmap van de optie voor delen.

### <a name="copy-files"></a>Bestanden kopiëren

Begin met versie 0.9.8 van Azure CLI, kunt u een bestand kopiëren naar een ander bestand, een bestand aan een blob of een blob naar een bestand. Hieronder gedemonstreerd hoe u deze met CLI opdrachten kopiëren-bewerkingen uitvoeren. Naar een bestand kopiëren naar de nieuwe map:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Een blob naar een map kopiëren:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele verwante artikelen en bronnen voor meer informatie over de opslag van Azure.

- [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
- [Azure opslag REST API verwijzing](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
