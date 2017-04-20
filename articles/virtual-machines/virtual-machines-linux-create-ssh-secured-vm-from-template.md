<properties
    pageTitle="Maken van een Linux VM met een sjabloon van Azure | Microsoft Azure"
    description="Maak een VM Linux op Azure met een sjabloon van Azure resourcemanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Maken van een Linux VM met een sjabloon van Azure

Dit artikel leest u hoe u snel een Linux virtuele Machine op Azure met een sjabloon van Azure.  In het artikel moet:

- een Azure-account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)).

- de [Azure CLI](../xplat-cli-install.md) aangemeld `azure login`.

- de modus Azure CLI _, moeten zich in_ Azure resourcemanager `azure config mode arm`.

U kunt ook snel een sjabloon Linux VM implementeren met behulp van de [Azure-portal](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Overzicht van de opdracht snelle

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

Sjablonen kunt u met de instellingen die u wilt aanpassen tijdens het starten, instellingen, zoals gebruikersnamen en hostnamen VMs op Azure maken. Voor dit artikel zijn we een Azure-sjabloon met behulp van een VM Ubuntu samen met een netwerk-beveiligingsgroep (NSG) starten met poort 22 openen voor SSH.

Azure resourcemanager sjablonen zijn JSON-bestanden die kunnen worden gebruikt voor eenvoudige eenmalige taken, zoals het starten van een VM Ubuntu als voltooid in dit artikel.  Azure sjablonen kunnen ook worden gebruikt om complexe Azure configuraties van volledige omgevingen, zoals een stapel testen, ontwikkelaar of productie implementatie te maken.

## <a name="create-the-linux-vm"></a>De VM Linux maken

Het volgende voorbeeld ziet u hoe bellen `azure group create` een resourcegroep maken en implementeren van een Linux SSH beveiligde VM tegelijkertijd met [deze sjabloon resourcemanager Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Vergeet niet dat in uw voorbeeld moet u namen die uniek voor uw omgeving zijn gebruiken. In dit voorbeeld wordt `myResourceGroup` als de naam van de resource, en `myVM` als de naam VM.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

De uitvoer ziet er als het volgende blok in de uitvoer:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Dit voorbeeld wordt geïmplementeerd een VM met de `--template-uri` parameter.  U kunt ook downloaden of een sjabloon lokaal maken en doorgeven de sjabloon met de `--template-file` parameter door een pad naar het sjabloonbestand als een argument. De CLI Azure vraagt u om de vereiste via de sjabloon voor parameters.

## <a name="next-steps"></a>Volgende stappen

De [galerie met sjablonen](https://azure.microsoft.com/documentation/templates/) als u wilt ontdekken welke app kaders om te implementeren het volgende zoeken.
