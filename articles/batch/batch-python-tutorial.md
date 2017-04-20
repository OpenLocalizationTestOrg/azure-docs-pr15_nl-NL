<properties
    pageTitle="Zelfstudie: aan de slag met de client Azure Batch Python | Microsoft Azure"
    description="Informatie over de basisbeginselen van Azure Batch en het ontwikkelen van de Batch-service met een eenvoudige scenario"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Aan de slag met de Azure Batch Python-client

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Informatie over de basisbeginselen van [Azure Batch] [ azure_batch] en de [Batch Python] [ py_azure_sdk] client zoals wordt een kleine Batch-toepassing geschreven in Python besproken. We kijken naar hoe twee steekproeven scripts gebruiken de Batch-service voor een parallelle werkbelasting op Linux virtuele machines in de cloud, en hoe ze werken met [Azure-opslag](./../storage/storage-introduction.md) voor bestand tijdelijke en ophalen. U wordt informatie over een algemene Batch toepassing werkstroom krijgen een grondtal begrip van de belangrijkste onderdelen van de Batch zoals projecten, taken, toepassingen en knooppunten berekenen.

![Batch oplossing werkstroom (basic)][11]<br/>

## <a name="prerequisites"></a>Vereisten voor

In dit artikel wordt ervan uitgegaan dat u eerder gewerkt van Python en bekend zijn met Linux hebt. Ook wordt ervan uitgegaan dat het lukt om te voldoen aan de vereisten voor het maken van account die zijn opgegeven onder voor Azure en de Batch- en -services.

### <a name="accounts"></a>Accounts

- **Azure-account**: als u nog geen een Azure-abonnement, [een gratis Azure-account maken][azure_free_account].
- **Batch account**: wanneer u een Azure-abonnement, [Maak een Batch Azure-account](batch-account-create-portal.md)hebt.
- **Opslag-account**: Zie [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [over Azure opslag-accounts](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Codevoorbeeld

De Python Zelfstudievideo [voorbeeld] [ github_article_samples] is een van de vele voorbeelden van de code Batch zijn gevonden in de [azure-batch-voorbeelden] [ github_samples] bibliotheek op GitHub. U kunt alle in de voorbeelden downloaden door te klikken op **klonen of download > downloaden ZIP** op de startpagina van de bibliotheek of door te klikken op de [azure-batch-voorbeelden-master.zip] [ github_samples_zip] directe downloadkoppeling. Nadat u de inhoud van het ZIP-bestand hebt uitgepakt, de twee scripts voor deze zelfstudie zijn gevonden de `article_samples` directory:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python-omgeving

Als u wilt het voorbeeldscript *python_tutorial_client.py* uitvoeren op uw lokale computer, moet u eerst een **Python interpreter** compatibel met versie **2.7** of **3,3 +**. Het script is getest in zowel Linux en Windows.

### <a name="cryptography-dependencies"></a>cryptografische afhankelijkheden

Moet u de afhankelijkheden voor de [cryptografische] installeren[ crypto] bibliotheek, vereist door de `azure-batch` en `azure-storage` Python-pakketten. Voer een van de volgende bewerkingen geschikt te maken voor uw platform of verwijzen naar de [installatie van cryptografische] [ crypto_install] details voor meer informatie:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Als voor Python 3.3 + op Linux installeert, gebruikt u de python3-equivalenten voor de afhankelijkheden Python. Bijvoorbeeld op Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure-pakketten

Vervolgens installeert u de **Batch Azure** en **Azure Storage** Python-pakketten. U kunt dit doen met **pip** en de *requirements.txt* hier vinden:

`/azure-batch-samples/Python/Batch/requirements.txt`

Probleem **pip** -opdracht de Batch en opslag-pakketten installeren:

`pip install -r requirements.txt`

U kunt de [azure-batch] installeren[ pypi_batch] en [azure-opslag] [ pypi_storage] Python handmatig pakketten:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Mogelijk moet u de opdrachten met het voorvoegsel `sudo` als u een account zonder machtigingen gebruikt. Bijvoorbeeld `sudo pip install -r requirements.txt`. Zie voor meer informatie over het installeren van Python-pakketten [Pakketten installeren] [ pypi_install] op readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Voorbeeld van de batch Python Zelfstudievideo code

Het voorbeeld van de zelfstudie code Batch Python bestaat uit twee Python scripts en een paar gegevensbestanden.

- **python_tutorial_client.PY**: de interactie tussen en met de services Batch en opslag uitvoeren van een parallelle werkbelasting op berekeningscluster knooppunten (virtuele machines). Het script *python_tutorial_client.py* op uw lokale computer wordt uitgevoerd.

- **python_tutorial_task.PY**: het script die wordt uitgevoerd op berekenen knooppunten in Azure om uit te voeren van de werkelijke hoeveelheid werk. In de steekproef parseert *python_tutorial_task.py* de tekst in een bestand dat is gedownload van Azure Storage (de invoer-bestand). Het resultaat vervolgens een tekstbestand (het uitvoerbestand) waarin een lijst met de bovenste drie woorden die worden weergegeven in het bestand dat invoer. Nadat het uitvoerbestand is gemaakt, uploadt *python_tutorial_task.py* het bestand met Azure Storage. Hierdoor downloaden voor de clientscript uitvoeren op uw werkstation. Het script *python_tutorial_task.py* wordt uitgevoerd parallel op meerdere berekeningscluster knooppunten in de Batch-service.

- **./data/taskdata\*.txt**: deze drie tekstbestanden bieden de invoer voor de taken die worden uitgevoerd op de knooppunten berekeningscluster.

In het volgende diagram ziet u de primaire bewerkingen die door de client en taak scripts worden uitgevoerd. Deze eenvoudige werkstroom wordt gebruikt voor veel berekeningscluster-oplossingen die zijn gemaakt met de Batch. Terwijl deze niet alle functies beschikbaar zijn in de service Batch tonen wordt, bevat bijna elke Batch scenario gedeelten van deze werkstroom.

![Voorbeeldwerkstroom batch][8]<br/>

[**Stap 1.**](#step-1-create-storage-containers) **Containers** in Azure-blobopslag maken.<br/>
[**Stap 2.**](#step-2-upload-task-script-and-data-files) Taak-script en invoer bestanden uploaden naar containers.<br/>
[**Stap 3.**](#step-3-create-batch-pool) Een Batch- **groep**maken.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** De groep **starttaak** downloads het script taak (python_tutorial_task.py) naar knooppunten, terwijl ze deelnemen aan de groep.<br/>
[**Stap 4.**](#step-4-create-batch-job) Maak een Batch- **taak**.<br/>
[**Stap 5.**](#step-5-add-tasks-to-job) **Taken** toevoegen aan de taak.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** De taken worden gepland op knooppunten uitvoeren.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 ter.** Elke taak de invoergegevens gedownload van Azure Storage en vervolgens wordt uitgevoerd.<br/>
[**Stap 6.**](#step-6-monitor-tasks) Taken controleren.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Als taken zijn voltooid, moet ze hun uitvoergegevens met Azure Storage uploaden.<br/>
[**Stap 7.**](#step-7-download-task-output) Download taak uitvoer van opslag.

Zoals genoemde, niet elke Batch-oplossing kunt u deze exacte stappen uitvoeren en eventueel ook ziet veel meer, maar deze u het algemene processen zijn gevonden in een oplossing Batch.

## <a name="prepare-client-script"></a>Clientscript voorbereiden

Voordat u de steekproef uitvoert, moet u uw accountreferenties Batch en opslag toevoegen aan *python_tutorial_client.py*. Als u dit nog niet hebt gedaan, opent u het bestand in uw favoriete editor en bijwerken van de volgende regels met uw referenties.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

U kunt uw accountreferenties Batch en opslagruimte binnen het blad account van elke service vinden in de [portal van Azure][azure_portal]:

![Referenties in de portal batch][9]
![opslag van referenties in de portal][10]<br/>

In de volgende secties analyseren we de stappen die wordt gebruikt door de scripts voor het verwerken van een werkbelasting in de Batch-service. We raden u regelmatig verwijzen naar de scripts in de editor, terwijl u uw weg de rest van het artikel werkt.

Ga naar de volgende regel in **python_tutorial_client.py** om te beginnen met stap 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Stap 1: Opslagcontainers maken

![Containers maken in Azure-opslag][1]
<br/>

Batch biedt ingebouwde ondersteuning voor de communicatie met Azure Storage. Containers in uw account opslag, vindt u de bestanden die nodig zijn voor de taken die worden uitgevoerd in uw account Batch. De containers bieden ook een plaats om de opslag van de uitvoergegevens die de taken produceren. Het eerste wat u dat het script *python_tutorial_client.py* doet is drie containers in [Azure-blobopslag](../storage/storage-introduction.md#blob-storage)maken:

- **toepassing**: deze container het Python script macro's starten met de taken, *python_tutorial_task.py*wordt opgeslagen.
- **Invoerbereik**: taken de gegevensbestanden verwerkingstijd van de *invoer* container wordt gedownload.
- **uitvoer**: wanneer u taken hebt voltooid invoer bestandsverwerking, worden ze de resultaten uploaden naar de container *uitvoer* .

Om te communiceren met een account voor de opslag en containers maken, gebruiken we de [azure-opslag] [ pypi_storage] pakket maken van een [BlockBlobService] [ py_blockblobservice] object--de "blob client." We vervolgens maken drie containers in het gebruik van de client blob opslag-account.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Zodra de containers zijn gemaakt, kan de toepassing nu de bestanden die worden gebruikt door de taken uploaden.

> [AZURE.TIP] [Het gebruik van Azure-blobopslag uit Python](../storage/storage-python-how-to-use-blob-storage.md) bevat een handig overzicht van het werken met Azure Storage containers en BLOB's. Dit komt boven aan de lijst lezen terwijl u aan de slag met Batch.

## <a name="step-2-upload-task-script-and-data-files"></a>Stap 2: Taak script en gegevens bestanden uploaden

![Toepassing voor de taak en invoer (gegevens) bestanden uploaden naar containers][2]
<br/>

In de bewerking voor het uploaden van bestanden definieert *python_tutorial_client.py* eerst verzamelingen van **toepassing** en **invoer** bestandspaden die zijn op de lokale computer. En vervolgens deze deze bestanden naar de containers die u in de vorige stap hebt gemaakt uploadt.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Lijst inzichtelijk, met de `upload_file_to_container` functie heet voor elk bestand in de verzamelingen en twee [ResourceFile] [ py_resource_file] verzamelingen worden ingevuld. De `upload_file_to_container` functie wordt weergegeven onder:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

Een [ResourceFile] [ py_resource_file] vindt u taken in de Batch met de URL naar een bestand in Azure-opslag die wordt gedownload naar een berekeningscluster knooppunt voordat deze taak wordt uitgevoerd. De [ResourceFile][py_resource_file]. de eigenschap **blob_source** geeft de volledige URL van het bestand zoals aanwezig in Azure opslag. De URL mogelijk ook een gedeelde access-handtekening (SA's) die beveiligde toegang tot het bestand geeft. De meeste taaktypen in Batch bevatten een eigenschap *ResourceFiles* , waaronder:

- [CloudTask][py_task]
- [Starttaak][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

In dit voorbeeld geen gebruikmaakt van de taaktypen JobPreparationTask of JobReleaseTask, maar u kunt meer informatie over deze in [uitvoeren voorbereiden en voltooiing projecttaken op Azure Batch knooppunten berekenen](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Gedeelde toegang handtekening (SA's)

Gedeelde toegang handtekeningen zijn tekenreeksen die beveiligde toegang tot containers en BLOB's in Azure opslag bieden. Het script *python_tutorial_client.py* gebruikmaakt van beide blob en container access handtekeningen gedeeld en laat zien hoe deze tekenreeksen van de handtekening gedeelde toegang verkrijgen met de Storage-service.

- **BLOB gedeeld access handtekeningen**: van de groep starttaak gebruik blob gedeelde toegang handtekeningen wanneer het downloaden van de taak script en invoer gegevensbestanden uit opslag (Zie [stap #3](#step-3-create-batch-pool) hieronder). De `upload_file_to_container` functie in *python_tutorial_client.py* bevat de code die wordt opgehaald van elke blob gedeelde toegang handtekening. Dit gebeurt door te bellen [BlockBlobService.make_blob_url] [ py_make_blob_url] in de module opslag.

- **Container gedeelde toegang handtekening**: als het werk op het knooppunt berekeningscluster van elke taak heeft voltooid, deze de uitvoerbestand uploadt naar de container *uitvoer* in Azure Storage. Hiervoor gebruik *python_tutorial_task.py* een container gedeelde access-handtekening waarmee schrijftoegang aan de container. De `get_container_sas_token` functie in *python_tutorial_client.py* wordt opgehaald van de container gedeelde toegang handtekening, klikt u vervolgens als opdrachtregelargumenten wordt doorgegeven aan de taken. Stap 5 #, [taken toevoegen aan een taak](#step-5-add-tasks-to-job), wordt het gebruik van de container SA's beschreven.

> [AZURE.TIP] Bekijk de reeks uit twee delen op gedeelde toegang handtekeningen, [deel 1: informatie over het model SA's](../storage/storage-dotnet-shared-access-signature-part-1.md) en [deel 2: maken en gebruiken van een SA's met de service Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), voor meer informatie over een beveiligde toegang tot gegevens in uw account opslag.

## <a name="step-3-create-batch-pool"></a>Stap 3: Batch groep maken

![Een resourcegroep die Batch maken][3]
<br/>

Een Batch die **groep** is een verzameling berekeningscluster knooppunten (virtuele machines) waarop Batch van een taak taken uitvoeren.

Nadat deze de taak script en gegevens bestanden naar de opslag-account uploadt, wordt de interactie met de Batch-service in *python_tutorial_client.py* gestart met behulp van de Batch Python-module. Hiervoor een [BatchServiceClient] [ py_batchserviceclient] wordt gemaakt:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Vervolgens wordt een reeks berekeningscluster knooppunten gemaakt in de Batch-account via een oproep naar `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Wanneer u een groep hebt gemaakt, definieert u een [PoolAddParameter] [ py_pooladdparam] waarmee wordt opgegeven dat verschillende eigenschappen voor de toepassingen:

- **ID** van de groep (*id* - vereist)<p/>Als u met de meeste entiteiten in Batch, moet uw nieuwe groep een unieke ID binnen uw Batch-account hebben. Uw code verwijst naar deze groep met de-ID en hoe u de groep in de [portal]-Azure identificeren is[azure_portal].

- **Aantal knooppunten berekenen** (*target_dedicated* - vereist)<p/>Deze eigenschap wordt aangegeven hoeveel VMs moeten worden geïmplementeerd in de groep. Het is belangrijk te weten dat alle Batchaccounts een standaard- **Quotum** waardoor het aantal **cores** (en dus berekeningscluster knooppunten) in een Batch-account hebt. U vindt de standaardquota en instructies voor het [vergroten van een quotum](batch-quota-limit.md#increase-a-quota) (zoals het maximum aantal cores in uw account Batch) in de [quota en -limieten voor de Batch Azure-service](batch-quota-limit.md). Als u merkt vragen "Waarom won't mijn groep hebt bereikt meer dan knooppunten X?" Dit quotum core mogelijk de oorzaak zijn.

- **Besturingssysteem** voor knooppunten (*virtual_machine_configuration* **of** *cloud_service_configuration* - vereist)<p/>In *python_tutorial_client.py*, maak een groep Linux knooppunten met een [VirtualMachineConfiguration][py_vm_config]. De `select_latest_verified_vm_image_with_node_agent_sku` werken `common.helpers` eenvoudiger werken met [Azure virtuele Machines Marketplace] [ vm_marketplace] afbeeldingen. Zie [Inrichten Linux berekenen van knooppunten in Azure Batch groepen](batch-linux-nodes.md) voor meer informatie over het gebruik van afbeeldingen op Marketplace.

- **Grootte van knooppunten berekenen** (*vm_size* - vereist)<p/>Aangezien we Linux knooppunten voor onze [VirtualMachineConfiguration geven][py_vm_config], wordt de grootte van een VM aan te geven (`STANDARD_A1` in dit voorbeeld) van [formaten voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-linux-sizes.md). Klik nogmaals Zie [inrichten Linux berekenen knooppunten in Azure Batch groepen](batch-linux-nodes.md) voor meer informatie.

- **Taak starten** (*start_task* - niet vereist)<p/>Samen met de hierboven fysieke knooppunt eigenschappen, kunt u ook een [starttaak] opgeven[ py_starttask] voor de groep (deze is niet vereist). De starttaak wordt uitgevoerd op elk knooppunt zoals u dat knooppunt deelneemt aan de groep en telkens wanneer een knooppunt opnieuw is opgestart. De starttaak is vooral handig voor het voorbereiden van berekeningscluster knooppunten voor de uitvoering van taken, zoals het installeren van de toepassingen die uw taken uitvoeren.<p/>In dit voorbeeldtoepassing kopieert de starttaak de bestanden die het downloaden van opslagruimte (die zijn opgegeven met behulp van de starttaak **resource_files** eigenschap) van de starttaak *werkmap* met de *gedeelde* map die voor alle taken op het knooppunt toegankelijk. In feite Hierdoor wordt gekopieerd `python_tutorial_task.py` met de gedeelde map op elk knooppunt terwijl het knooppunt deelneemt aan de groep, zodat alle taken die worden uitgevoerd op het knooppunt kunnen openen.

U ziet misschien de oproep door naar de `wrap_commands_in_shell` Help-functie. Deze functie wordt een verzameling afzonderlijke opdrachten en maakt een ononderbroken lijn van de opdracht geschikt te maken voor de opdrachtregel-eigenschap van een taak.

Let op in de bovenstaande codefragment is ook het gebruik van twee omgevingsvariabelen in de eigenschap **opdrachtregel** van de starttaak: `AZ_BATCH_TASK_WORKING_DIR` en `AZ_BATCH_NODE_SHARED_DIR`. Elk berekeningscluster knooppunt in een groep Batch wordt automatisch geconfigureerd met meerdere variabelen bevatten omgeving die specifiek voor de Batch zijn. Een proces dat wordt uitgevoerd door een taak heeft toegang tot deze omgevingsvariabelen.

> [AZURE.TIP] Meer informatie over de omgeving Zie variabelen die beschikbaar op berekeningscluster knooppunten in een groep Batch, evenals de informatie over de taak werken mappen zijn, **omgevingsinstellingen voor taken** en **bestanden en mappen** in het [overzicht van functies van Azure Batch](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Stap 4: Batchtaak maken

![Batchtaak maken][4]<br/>

Een Batch **taak** is een verzameling taken en is gekoppeld aan een groep berekeningscluster knooppunten. De taken in een taak uitvoeren op de bijbehorende toepassingen berekeningscluster knooppunten.

U kunt een taak niet alleen voor het organiseren en bijhouden van taken in gerelateerde werkbelasting, maar ook voor de toepassing van bepaalde beperkingen, zoals de maximale runtime voor de taak (en dus ook zijn taken) en taken prioriteit ten opzichte van andere taken in de Batch-account. In dit voorbeeld is de taak echter alleen gekoppeld aan de toepassingen die is gemaakt in stap #3. Geen extra eigenschappen zijn geconfigureerd.

Alle taken zijn gekoppeld aan een specifieke groep. Deze koppeling wordt aangegeven welke knooppunten van het project taken uitvoeren op. U de toepassingen opgeven met behulp van de [PoolInformation] [ py_poolinfo] eigenschap, zoals in het onderstaande codefragment.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nu dat een taak is gemaakt, worden taken worden toegevoegd aan het werk uitvoeren.

## <a name="step-5-add-tasks-to-job"></a>Stap 5: Taken toevoegen aan de taak

![Taken toevoegen aan een taak][5]<br/>
*(1) taken worden toegevoegd aan de taak, (2) de taken worden gepland om uit te voeren op knooppunten en (3) de taken de gegevensbestanden verwerkingstijd downloaden*

Batch **taken** zijn de afzonderlijke eenheden van werk die op de knooppunten berekeningscluster uitvoeren. Een taak heeft een opdrachtregel en wordt de scripts of uitvoerbare bestanden die u in die opdrachtregel opgeeft uitgevoerd.

Als u wilt dat is wel inzetten, moeten de taken worden toegevoegd aan een taak. Elke [CloudTask] [ py_task] is geconfigureerd met een opdrachtregel eigenschap en [ResourceFiles] [ py_resource_file] (zoals met van de groep starttaak) die de taak die is gedownload naar het knooppunt voordat de opdrachtregel automatisch wordt uitgevoerd. Elke taak verwerkt in de steekproef, slechts één bestand. De collectie ResourceFiles bevat dus één element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Wanneer ze toegang tot omgevingsvariabelen, zoals `$AZ_BATCH_NODE_SHARED_DIR` of een toepassing niet gevonden in van het knooppunt uitvoeren `PATH`, taakregels moet de shell aanroepen expliciet, zoals met `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Deze vereiste geldt onnodige als uw taken een toepassing in van het knooppunt uitvoert `PATH` en niet verwijzen naar alle omgevingsvariabelen.

Binnen de `for` loopback in de bovenstaande codefragment, kunt u zien dat de opdrachtregel voor de taak wordt opgesteld met vijf opdrachtregelargumenten die worden doorgegeven aan *python_tutorial_task.py*:

1. **bestandspad**: dit is het lokale pad naar het bestand op het knooppunt. Wanneer de ResourceFile object in `upload_file_to_container` is gemaakt in stap 2 hierboven, de bestandsnaam is gebruikt voor deze eigenschap (de `file_path` parameter in de constructor ResourceFile). Hiermee wordt aangeduid dat het bestand in dezelfde map op het knooppunt als *python_tutorial_task.py*kan worden gevonden.

2. **NumWords**: de bovenste *N* woorden moeten worden geschreven naar het uitvoerbestand.

3. **storageaccount**: de naam van het opslag-account dat eigenaar is van de container waaraan de taak-uitvoer moet worden geüpload.

4. **storagecontainer**: de naam van de opslagruimte container waarnaar de uitvoerbestanden die moeten worden geüpload.

5. **sastoken**: de gedeelde access-handtekening (SA's) die schrijftoegang aan de container **uitvoer** in Azure opslagruimte biedt. Het script *python_tutorial_task.py* deze handtekening gedeelde toegang wordt gebruikt wanneer wordt gemaakt van de verwijzing BlockBlobService. Hierdoor wordt schrijftoegang tot de container zonder een toegangstoets voor de opslag-account.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Stap 6: Monitortaken

![Monitortaken][6]<br/>
*Het script (1) de taken voor voltooiingsstatus bewaakt en (2) de taken upload resultaatgegevens met Azure Storage*

Als taken worden toegevoegd aan een taak, worden ze automatisch in de wachtrij en gepland voor uitvoering op berekeningscluster knooppunten in de groep die is gekoppeld aan de taak. Batch op basis van de instellingen die u opgeeft, worden alle taak-queuing, planning, opnieuw en andere taak beheer van de taken voor u.

Zijn er veel benaderingen monitoring uitvoering van taken. De `wait_for_tasks_to_complete` functie in *python_tutorial_client.py* biedt een eenvoudig voorbeeld van monitoring taken voor een bepaalde staat in dit geval de [voltooid] [ py_taskstate] staat.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Stap 7: Download uitvoer van de taak

![Uitvoer van de taak downloaden uit de opslag][7]<br/>

Nu dat de taak is voltooid, kan de uitvoer van de taken van Azure Storage worden gedownload. Dit gebeurt via een oproep naar `download_blobs_from_container` in *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] De oproep door naar `download_blobs_from_container` geeft aan dat de bestanden moeten worden gedownload naar de basismap in *python_tutorial_client.py* . Je mag rustig deze uitvoerlocatie wijzigen.

## <a name="step-8-delete-containers"></a>Stap 8: Verwijderen containers

Omdat wordt geheven voor gegevens die zich in Azure opslag bevindt, maar het is altijd een goed idee om een BLOB's die niet meer nodig zijn voor uw taken Batch verwijderen. In *python_tutorial_client.py*, is dit doen met drie oproepen naar [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Stap 9: De taak en de groep verwijderen

In de laatste stap, wordt u gevraagd om de taak en de toepassingen die zijn gemaakt door het script *python_tutorial_client.py* te verwijderen. Hoewel u geen btw voor projecten en taken zelf, u *bent geheven* betalen voor berekeningscluster knooppunten. Dus is het raadzaam dat u knooppunten toewijst alleen naar wens. Niet-gebruikte toepassingen verwijderen kan deel uitmaken van het proces onderhoud.

Van de BatchServiceClient [JobOperations] [ py_job] en [PoolOperations] [ py_pool] beide overeenkomstige verwijdering methoden, die worden genoemd als u bevestigen hebt:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Houd er rekening mee dat u wordt geheven voor resources berekeningscluster--kosten ongebruikte toepassingen verwijderen te minimaliseren. Let op dat een resourcegroep die verwijdert alle berekeningscluster knooppunten in die groep en dat alle gegevens op de knooppunten kunnen niet worden teruggezet nadat de groep wordt verwijderd.

## <a name="run-the-sample-script"></a>Het voorbeeldscript uitvoeren

Wanneer u de *python_tutorial_client.py* -script uitvoeren vanuit de zelfstudie [voorbeeld][github_article_samples], de console-uitvoer is vergelijkbaar met de volgende handelingen uit. Er is op een pauze `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` terwijl van de groep berekeningscluster knooppunten worden gemaakt, is gestart, en de opdrachten in de groep van toepassingen starttaak worden uitgevoerd. Gebruik van de [Azure portal] [ azure_portal] om te controleren van uw groep, berekenen knooppunten, functie en taken tijdens en na worden uitgevoerd. Gebruik van de [Azure portal] [ azure_portal] of de [Microsoft Azure opslag Explorer] [ storage_explorer] om weer te geven van de opslag-resources (containers en BLOB's) die zijn gemaakt door de toepassing.

>[AZURE.TIP] De *python_tutorial_client.py* -script uitvoeren vanuit het `azure-batch-samples/Python/Batch/article_samples` directory. Site gebruikmaakt van een relatief pad voor het `common.helpers` module importeren, zodat u ziet mogelijk `ImportError: No module named 'common'` als er geen toegangsproblemen de het script in deze map.

Wanneer u de steekproef in de standaardconfiguratie uitvoert, is typische execution tijdstip **ongeveer 5 tot en met 7 minuten** .

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen

Je mag rustig wijzigt *python_tutorial_client.py* en *python_tutorial_task.py* om te experimenteren met verschillende berekeningscluster scenario's. Bijvoorbeeld, kunt u een vertraging execution toe te voegen aan *python_tutorial_task.py* simuleren langdurige taken en ze controleren in de portal. Probeer meer taken toe te voegen of het aantal knooppunten berekeningscluster aan te passen. Instructies om te controleren en het gebruik van een bestaande toepassingen naar snelheid van tijd toestaan toevoegen.

Nu u bekend bent met de eenvoudige werkstroom van een oplossing Batch, is het tijd om het verloop de extra functies van de Batch-service.

- Bekijk het [overzicht van Azure Batch functies](batch-api-basics.md) artikel, waarin u wordt aangeraden als u geen ervaring met de service hebt.
- Beginnen op de andere Batch ontwikkeling artikelen onder **ontwikkeling uitgebreidere** in de [Batch leerpad][batch_learning_path].
- Bekijk een andere implementatie van het verwerken van de werklast "bovenste N woorden" met Batch in de [TopNWords] [ github_topnwords] voorbeeld.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Containers maken in Azure-opslag"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Toepassing voor de taak en invoer (gegevens) bestanden uploaden naar containers"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Batch toepassingen maken"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Batchtaak maken"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Taken toevoegen aan een taak"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Monitortaken"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Uitvoer van de taak downloaden uit de opslag"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Batch oplossing werkstroom (volledige diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Batch referenties in de Portal"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Opslag van referenties in de Portal"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Batch oplossing werkstroom (minimale diagram)"
