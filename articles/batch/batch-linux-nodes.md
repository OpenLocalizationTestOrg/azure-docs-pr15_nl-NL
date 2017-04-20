<properties
    pageTitle="Linux knooppunten in groepen van Azure Batch | Microsoft Azure"
    description="Leer hoe u uw werkbelasting parallelle berekeningscluster op groepen Linux virtuele machines in Azure Batch verwerken."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Linux berekeningscluster knooppunten in groepen van Azure Batch inrichten

U kunt Azure Batch parallelle berekeningscluster werkbelasting uitgevoerd voor zowel Linux als Windows virtuele machines. Dit artikel wordt uitgelegd hoe u het maken van groepen Linux berekeningscluster knooppunten in de Batch-service met behulp van beide de [Batch Python] [ py_batch_package] en [Batch.NET] [ api_net] clientbibliotheken.

> [AZURE.NOTE] [Toepassing-pakketten](batch-application-packages.md) worden momenteel niet ondersteund op Linux berekeningscluster knooppunten.

## <a name="virtual-machine-configuration"></a>VM configuratie

Wanneer u een groep berekeningscluster knooppunten in Batch maakt, hebt u twee opties waaruit u om de grootte van knooppunt en besturingssysteem te selecteren: Cloud Services-configuratie en VM configuratie.

**Configuratie van cloud Services** biedt dat Windows knooppunten *alleen*berekenen. Beschikbaar berekeningscluster knooppunt tekengrootten worden vermeld in [formaten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md)en beschikbare besturingssystemen worden vermeld in de [Azure Gast OS releases en SDK compatibiliteit matrix](../cloud-services/cloud-services-guestos-update-matrix.md). Wanneer u een groep met Azure-Cloudservices knooppunten maakt, moet u de grootte van het knooppunt en de "OS familie" die zijn gevonden in de hierboven genoemde artikelen opgeven. Voor toepassingen van Windows knooppunten berekenen, wordt meestal Cloud Services gebruikt.

**VM configuratie** biedt Linux- en Windows-afbeeldingen voor berekeningscluster knooppunten. Beschikbare berekeningscluster knooppunt tekengrootten worden vermeld in [formaten voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) en de [grootte voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Wanneer u een groep met VM configuratie knooppunten maakt, moet u de grootte van de knooppunten, de verwijzing naar VM afbeelding en de Batch knooppunt agent SKU op de knooppunten zijn geïnstalleerd.

### <a name="virtual-machine-image-reference"></a>Verwijzing naar de afbeelding van virtuele machines

De Batch-service wordt gebruikt [VM schaal sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Linux berekeningscluster knooppunten. De afbeeldingen van het besturingssysteem voor deze virtuele machines worden verstrekt door de [Azure Marketplace][vm_marketplace]. Wanneer u een overzicht van de afbeelding VM configureert, geeft u de eigenschappen van een afbeelding van de VM Marketplace. De volgende eigenschappen zijn vereist wanneer u een overzicht van de afbeelding VM maakt:

| **Afbeeldingseigenschappen verwijzing** | **Voorbeeld** |
| ----------------- | ------------------------ |
| Publisher         | Canonieke                |
| Aanbieding             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Versie           | meest recente                   |

> [AZURE.TIP] U kunt meer informatie over deze eigenschappen en hoe u afbeeldingen op Marketplace in [navigeren en selecteer Linux VM afbeeldingen in Azure wordt aangegeven met CLI of PowerShell](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Houd er rekening mee dat niet alle afbeeldingen op Marketplace momenteel compatibel zijn met Batch. Zie [knooppunt agent SKU](#node-agent-sku)voor meer informatie.

### <a name="node-agent-sku"></a>Knooppunt agent SKU

De Batch knooppunt-agent is een programma dat wordt uitgevoerd op elk knooppunt in de groep en de opdracht-en-besturingselementen-interface tussen het knooppunt en de Batch-service. Er zijn verschillende implementaties van de knooppunt-agent, bekend als SKU's, voor verschillende besturingssystemen. In feite wanneer u een virtuele machineconfiguratie maakt, u eerst de verwijzing van de afbeelding VM opgeven en geeft het knooppunt agent installeren op de afbeelding. Meestal elke agent knooppunt SKU is compatibel met meerdere VM afbeeldingen. Hier volgen een paar voorbeelden van knooppunt agent SKU's:

* batch.node.Ubuntu 14.04
* batch.node.centos 7
* batch.node.Windows amd64

> [AZURE.IMPORTANT] Niet alle VM afbeeldingen die beschikbaar in de Marketplace zijn zijn compatibel met de momenteel beschikbaar Batch knooppunt agenten. U kunt de Batch SDK's moet gebruiken voor een overzicht van de beschikbare knooppunt-agent SKU's en de VM afbeeldingen waarmee ze compatibel zijn. Zie de [lijst van Virtual Machine afbeeldingen](#list-of-virtual-machine-images) verderop in dit artikel voor meer informatie.

## <a name="create-a-linux-pool-batch-python"></a>Maken van een resourcegroep die Linux: Batch Python

Het volgende codefragment toont een voorbeeld van het gebruik van de [Bibliotheek van Microsoft Azure Batch Client voor Python] [ py_batch_package] een resourcegroep die Ubuntu Server berekeningscluster-knooppunten te maken. Documentatie bij de module Batch Python kunt u vinden op [azure.batch pakket] [py_batch_docs] op alleen de documenten.

In dit codefragment maakt een [ImageReference] [ py_imagereference] expliciet en Hiermee geeft u op elk van de eigenschappen (publisher aanbieding, SKU, versie). In productiecode, echter is raadzaam dat u de [list_node_agent_skus gebruikt] [ py_list_skus] methode om te bepalen en selecteer in de beschikbare afbeelding en knooppunt agent SKU combinaties gedurende runtime.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Zoals eerder is vermeld, raden we in plaats van het maken van de [ImageReference] [ py_imagereference] expliciet, gebruikt u de [list_node_agent_skus] [ py_list_skus] methode om dynamisch van de combinaties van momenteel ondersteunde knooppunt agent/Marketplace afbeelding te selecteren. Het codefragment van de volgende Python ziet u gebruik van deze methode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Maken van een resourcegroep die Linux: Batch .NET

Het volgende codefragment toont een voorbeeld van het gebruik van de [Batch.NET] [ nuget_batch_net] clientbibliotheek met het maken van een groep Ubuntu Server berekeningscluster knooppunten. U vindt de [naslagmateriaal Batch .NET] [ api_net] op MSDN.

De volgende codefragment wordt de [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] methode om te selecteren in de lijst met momenteel ondersteund Marketplace afbeelding en knooppunt agent SKU combinaties. Deze methode is wenselijk omdat de lijst met ondersteunde combinaties te bezoeken kan veranderen. Meestal worden ondersteunde combinaties toegevoegd.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Hoewel het vorige fragment wordt gebruikt voor de [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] methode dynamisch lijst en selecteer in de afbeelding en knooppunt agent SKU combinaties (aanbevolen) ondersteund, u kunt ook een [ImageReference] [ net_imagereference] expliciet:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Lijst met afbeeldingen van virtuele machines

De volgende tabel bevat de Marketplace VM afbeeldingen die compatibel met de beschikbare Batch knooppunt agenten zijn wanneer in dit artikel voor het laatst werd bijgewerkt. Het is belangrijk te weten dat deze lijst niet definitieve is omdat afbeeldingen en knooppunt agenten kunnen worden toegevoegd of verwijderd op elk gewenst moment. Het is raadzaam dat de Batch toepassingen en services altijd [list_node_agent_skus gebruiken] [ py_list_skus] (Python) en [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) om te bepalen en selecteer in de momenteel beschikbaar SKU's.

> [AZURE.WARNING] De volgende lijst mogelijk op elk gewenst moment wijzigen. Gebruik altijd de **lijst knooppunt agent SKU** methoden beschikbaar in de Batch-API's naar de lijst en selecteer vervolgens in de compatibele VM en het knooppunt agent SKU's wanneer u uw taken uitvoert.

| **Publisher** | **Aanbieding** | **Afbeelding van de SKU** | **Versie** | **Knooppunt agent SKU-ID** |
| ------- | ------- | ------- | ------- | ------- |
| Canonieke | UbuntuServer | 14.04.0-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.1-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.2-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.3-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.4-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 14.04.5-LTS | meest recente | batch.node.Ubuntu 14.04 |
| Canonieke | UbuntuServer | 16.04.0-LTS | meest recente | batch.node.Ubuntu 16.04 |
| Credativ | Debian | 8 | meest recente | batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS-HPC | 7.1 | meest recente | batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | meest recente | batch.node.centos 7 |
| Oracle | Oracle-Linux | 7.0 | meest recente | batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | meest recente | batch.node.opensuse 13.2 |
| SUSE | openSUSE-sprong | 42,1 | meest recente | batch.node.opensuse 42,1 |
| SUSE | SLES HPC | 12 | meest recente | batch.node.opensuse 42,1 |
| SUSE | SLES | 12-SP1 | meest recente | batch.node.opensuse 42,1 |
| Microsoft-advertenties | standaard-gegevens-wetenschappelijke-vm | standaard-gegevens-wetenschappelijke-vm | meest recente | batch.node.Windows amd64 |
| Microsoft-advertenties | Linux-gegevens-wetenschappelijke-vm | linuxdsvm | meest recente | batch.node.centos 7 |
| Legitieme Microsoft Windows Server | WindowsServer | 2008 R2 SP1 | meest recente | batch.node.Windows amd64 |
| Legitieme Microsoft Windows Server | WindowsServer | 2012-Datacenter | meest recente | batch.node.Windows amd64 |
| Legitieme Microsoft Windows Server | WindowsServer | 2012-R2-Datacenter | meest recente | batch.node.Windows amd64 |
| Legitieme Microsoft Windows Server | WindowsServer | Windows Server-Technical Preview | meest recente | batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Verbinding maken met Linux knooppunten

Tijdens de ontwikkeling of tijdens het oplossen van vindt u deze mogelijk nodig te melden bij de knooppunten in uw groep. In tegenstelling tot Windows berekeningscluster knooppunten, kunt u Remote Desktop Protocol (RDP) niet gebruiken om te verbinden met Linux knooppunten. In plaats daarvan kunt de Batch-service SSH toegang op elk knooppunt voor verbinding met extern.

Het volgende Python-codefragment wordt gemaakt van een gebruiker op elk knooppunt in een groep, die vereist voor verbinding met extern is. De verbindingsgegevens secure shell (SSH) voor elk knooppunt wordt afgedrukt.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Hier volgt een voorbeeld van uitvoer voor de bovenstaande code voor een groep met vier Linux knooppunten:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Houd er rekening mee dat in plaats van een wachtwoord, kunt u een openbare sleutel SSH wanneer u een gebruiker op een knooppunt maakt. In de SDK Python dit klaar is met behulp van de parameter **ssh_public_key** op [ComputeNodeUser][py_computenodeuser]. In .NET dit klaar is met behulp van de [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] eigenschap.

## <a name="pricing"></a>Prijzen

Azure Batch is gebaseerd op Azure-Cloudservices en Azure virtuele Machines technologie. De Batch-service zelf wordt aangeboden gratis, wat betekent dat u alleen voor de resources berekeningscluster wordt geheven dat dat uw oplossingen Batch gebruiken. Als u de **Configuratie van Cloud Services**kiest, u moet betalen op basis van de [Cloudservices prijzen] [ cloud_services_pricing] structuur. Als u **Virtuele machineconfiguratie**kiest, u moet betalen op basis van de [prijzen van virtuele Machines] [ vm_pricing] structuur.

## <a name="next-steps"></a>Volgende stappen

### <a name="batch-python-tutorial"></a>Batch Python zelfstudie

Voor een verder zelfstudie over het werken met Batch met behulp van Python, raadpleegt u [aan de slag met de Azure Batch Python-client](batch-python-tutorial.md). De companion [voorbeeld] [ github_samples_pyclient] bevat een Help-functie, `get_vm_config_for_distro`, waarin wordt weergegeven met een andere methode de configuratie van een virtuele machine te verkrijgen.

### <a name="batch-python-code-samples"></a>Voorbeelden van batch Python code

Bekijk de andere [Python code voorbeelden] [ github_samples_py] in de [azure-batch-voorbeelden] [ github_samples] bibliotheek op GitHub voor verschillende scripts u zien hoe algemene batchbewerkingen zoals toepassingen, functie en maken van taken uit te voeren. Het [Leesmij] [ github_py_readme] die wordt geleverd bij de Python voorbeelden heeft meer informatie over het installeren van de vereiste pakketten.

### <a name="batch-forum"></a>Batch-forum

Het [Forum van Azure Batch] [ forum] op MSDN is een prima plek om te bespreken Batch en vragen over de service. Nuttige 'stickied'-berichten lezen en posten uw vragen dat ze zich voordoen terwijl u uw Batch oplossingen bouwen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
