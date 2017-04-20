<properties
   pageTitle="Een cluster Azure Container-Service implementeren | Microsoft Azure"
   description="Een Container-Azure-Service cluster met behulp van de Azure-portal, de Azure CLI of PowerShell implementeren."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Een cluster Azure Container-Service implementeren

Azure Container-Service biedt snelle implementatie van populaire open source container cluster en integratie oplossingen. Met behulp van Azure Container-Service, kunt u de domeincontroller/OS en Docker Swarm clusters met Azure resourcemanager sjablonen of de Azure portal implementeren. U deze clusters implementeert met behulp van Azure virtuele machines schaal Sets en de clusters profiteren van Azure-abonnementen voor de pagina over netwerken en opslag. Voor toegang tot Azure Container-Service, moet u eerst een Azure-abonnement. Als u deze niet hebt, kunt klikt u registreren voor een [gratis proefversie](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

In dit document begeleidt u bij het implementeren van een cluster Azure Container-Service via de [portal van Azure](#creating-a-service-using-the-azure-portal), de [interface van Azure opdrachtregel (CLI)](#creating-a-service-using-the-azure-cli)en de [Azure PowerShell-module](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Een service maken met behulp van de Azure-portal

Meld u aan bij de portal van Azure, selecteert u daarna **Nieuw**en zoek de Azure Marketplace voor **Azure Container-Service**.

![Implementatie 1 maken](media/acs-portal1.png)  <br />

Selecteer **Azure Container-Service**en klik op **maken**.

![Implementatie 2 maken](media/acs-portal2.png)  <br />

Voer de volgende gegevens:

- **Gebruikersnaam in te voeren**: dit is de gebruikersnaam in te voeren die worden gebruikt voor een account voor elk van de virtuele machines en VM schaal Hiermee stelt u in het cluster Azure Container-Service.
- **Abonnement**: Selecteer een Azure-abonnement.
- **Resourcegroep**: Selecteer een bestaande resourcegroep of een nieuw account te maken.
- **Locatie**: Selecteer een Azure gebied voor de implementatie van Azure Container-Service.
- **SSH openbare sleutel**: de openbare sleutel die wordt gebruikt voor verificatie ten opzichte van de Container-Service Azure virtuele machines toevoegen. Is het belangrijk dat deze toets geen regeleinden bevat en dat wordt het voorvoegsel 'ssh-rsa' en de 'username@domain' postfix. Deze ziet er als volgt te werk: **ssh-rsa AAAAB3Nz... <> …... UcyupgH azureuser@linuxvm **. Zie de artikelen [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) en [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) voor hulp bij het maken van Secure Shell (SSH) toetsen.

Klik op **OK** wanneer u klaar bent om te gaan.

![Implementatie 3 maken](media/acs-portal3.png)  <br />

Selecteer een type integratie. De opties zijn:

- **Domeincontroller/OS**: een cluster domeincontroller/OS implementeren.
- **Swarm**: een cluster Docker Swarm implementeert.

Klik op **OK** wanneer u klaar bent om te gaan.

![Implementatie 4 maken](media/acs-portal4.png)  <br />

Voer de volgende gegevens:

- **Basispagina tellen**: het aantal modellen in het cluster.
- **Agent tellen**: voor Docker Swarm, dit is het oorspronkelijke aantal agenten in het agent schaal instellen. Voor domeincontroller/OS, als dit is het oorspronkelijke aantal agenten in een set privé schaal. Daarnaast is een set openbare schaal gemaakt, die een vooraf bepaald aantal agenten bevat. Het aantal agenten in deze set openbare schaal wordt bepaald door hoeveel outmodellen zijn gemaakt in het cluster--één openbare agent voor één diamodel en twee openbare agenten voor drie of vijf outmodellen.
- **Agent VM grootte**: de grootte van de agent virtuele machines.
- **DNS-voorvoegsel**: een unieke naam voor wereld die wordt gebruikt voor het voorvoegsel belangrijke punten in de volledig gekwalificeerde domeinnamen voor de service.

Klik op **OK** wanneer u klaar bent om te gaan.

![Implementatie 5 maken](media/acs-portal5.png)  <br />

Klik op **OK** nadat service validatie is voltooid.

![Implementatie 6 maken](media/acs-portal6.png)  <br />

Klik op **maken** om de implementatieproces te starten.

![Implementatie 7 maken](media/acs-portal7.png)  <br />

Als u ervoor hebt gekozen om een opslaglocatie van de implementatie bij de Azure-portal te, ziet u de implementatiestatus.

![Implementatie 8 maken](media/acs-portal8.png)  <br />

Wanneer de implementatie is voltooid, is het cluster Azure Container-Service is gereed voor gebruik.

## <a name="create-a-service-by-using-the-azure-cli"></a>Een service maken met behulp van de Azure CLI

Als u wilt maken dat een exemplaar van Azure Container-Service via de opdrachtregel, moet u eerst een Azure-abonnement. Als u deze niet hebt, kunt klikt u registreren voor een [gratis proefversie](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Ook moet u beschikken over [installatie](../xplat-cli-install.md) en [configuratie van](../xplat-cli-connect.md) de Azure CLI.

Als u wilt een domeincontroller/OS of Docker Swarm cluster implementeert, selecteert u een van de volgende sjablonen in GitHub. Houd er rekening mee dat beide van deze sjablonen voor hetzelfde, met uitzondering van de standaardselectie orchestrator zijn.

* [Domeincontroller/OS sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Sjabloon swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Zorg er vervolgens dat de CLI Azure is verbonden met een Azure-abonnement. U kunt dit doen met behulp van de volgende opdracht uit:

```bash
azure account show
```
Als een Azure-account is niet geretourneerd, gebruikt u de volgende opdracht uit de CLI aanmelden bij Azure.

```bash
azure login -u user@domain.com
```

Configureer de hulpmiddelen Azure CLI als Azure Resource Manager wilt gebruiken.

```bash
azure config mode arm
```

Een Azure resourcegroep en de Container Service cluster maken met de volgende opdracht uit, waar:

- **RESOURCE_GROUP** is de naam van de resourcegroep die u wilt gebruiken voor deze service.
- **Locatie** is de Azure regio waar de resourcegroep en de implementatie van Azure Container-Service wordt gemaakt.
- **TEMPLATE_URI** is de locatie van de implementatie-bestand. Houd er rekening mee dat dit het logboekbestand, niet een verwijzing naar de UI GitHub moet zijn. U vindt deze URL, selecteer het bestand azuredeploy.json in GitHub, en klik op de knop **onbewerkte** .

> [AZURE.NOTE] Wanneer u deze opdracht uitvoert, wordt u de shell voor implementatie parameterwaarden gevraagd.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Geef de Sjabloonparameters

Deze versie van de opdracht moet u parameters interactief definiëren. Als u wilt opgeven van parameters, zoals een tekenreeks JSON-indeling, kunt u doen met behulp van de `-p` schakelen. Bijvoorbeeld:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

U kunt ook een parameterbestand JSON-indeling opgeeft met behulp van de `-e` schakelen:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Om een voorbeeld van parameters een bestand met de naam weer te geven `azuredeploy.parameters.json`, zoekt u hiernaar met de sjablonen Azure Container-Service in GitHub.

## <a name="create-a-service-by-using-powershell"></a>Een service maken met behulp van PowerShell

U kunt ook een cluster Azure-Service voor Container met PowerShell implementeren. In dit document is gebaseerd op de versie 1.0 [Azure PowerShell-module](https://azure.microsoft.com/blog/azps-1-0/).

Als u wilt een domeincontroller/OS of Docker Swarm cluster implementeert, selecteert u een van de volgende sjablonen. Houd er rekening mee dat beide van deze sjablonen voor hetzelfde, met uitzondering van de standaardselectie orchestrator zijn.

* [Domeincontroller/OS sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Sjabloon swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Controleer voordat u een cluster maakt van uw Azure-abonnement, of dat de PowerShell-sessie heeft zijn aangemeld bij Azure. U kunt hiervoor met de `Get-AzureRMSubscription` opdracht:

```powershell
Get-AzureRmSubscription
```

Als u nodig hebt om aan te melden bij Azure, gebruikt u de `Login-AzureRMAccount` opdracht:

```powershell
Login-AzureRmAccount
```

Als u naar een nieuwe resourcegroep distribueren bent, moet u eerst de resourcegroep maken. Als u wilt een nieuwe resourcegroep maken, gebruikt u de `New-AzureRmResourceGroup` opdracht en geef een naam en locatie-gebied voor de groep voor resource:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Nadat u een resourcegroep hebt gemaakt, kunt u uw cluster kunt maken met de volgende opdracht uit. De URI van de gewenste sjabloon wordt opgegeven voor de `-TemplateUri` parameter. Wanneer u deze opdracht uitvoert, wordt u PowerShell implementatie parameterwaarden gevraagd.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Geef de Sjabloonparameters

Als u bekend met PowerShell bent, weet u dat u door de beschikbare parameters voor een cmdlet bladeren kunt door een minteken (-) te typen en vervolgens op de TAB-toets te drukken. Deze functionaliteit werkt ook met parameters die u in uw sjabloon definieert. Zodra u de sjabloonnaam te typen, de cmdlet ophaalt van de sjabloon, parseert de parameters, en wordt de Sjabloonparameters aan de opdracht dynamisch. Dit kunt u heel gemakkelijk kunt u de sjabloon parameterwaarden opgeven. En, als u een vereiste parameterwaarde vergeet, PowerShell vraagt u om de waarde.

Hieronder vindt u de volledige opdracht, met parameters die zijn opgenomen. U kunt uw eigen waarden opgeven voor de namen van de resources.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Volgende stappen

Nu u een functioneel cluster hebt, kunt u bekijken deze documenten voor verbinding en management details:

- [Verbinding maken met een cluster Azure Container-Service](container-service-connect.md)
- [Werken met Azure Container Service en domeincontroller/OS](container-service-mesos-marathon-rest.md)
- [Werken met Azure Container Service en Docker Swarm](container-service-docker-swarm.md)
