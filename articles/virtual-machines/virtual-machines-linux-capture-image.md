<properties
    pageTitle="Een VM Linux wilt gebruiken als een sjabloon vastleggen | Microsoft Azure"
    description="Leer hoe u vastleggen en een afbeelding van een Linux-Azure virtuele machine (VM) die zijn gemaakt met het implementatiemodel van Azure resourcemanager generalize."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Een Linux virtuele machine waarop Azure vastleggen

Volg de stappen in dit artikel voor generalize en vastleggen van uw Linux Azure virtuele machine (VM) in het implementatiemodel resourcemanager. Wanneer u de VM generalize, kunt u persoonlijke gegevens verwijderen en de VM moet worden gebruikt als een afbeelding voorbereiden. U vervolgens maken een generalized virtuele harde schijf (VHD) voor het besturingssysteem, VHD's voor gekoppelde gegevensschijven en een [resourcemanager sjabloon](../azure-resource-manager/resource-group-overview.md) voor nieuwe VM implementaties. 

Als u wilt maken met de afbeelding VMs, netwerk resources voor elke nieuwe VM instellen en het gebruik van de sjabloon (een notatie voor JavaScript-Object of JSON,-bestand) te implementeren van genomen VHD afbeeldingen. Op deze manier kunt u een VM met de huidige softwareconfiguratie, worden dezelfde manier als die u afbeeldingen in de Azure Marketplace gebruiken repliceren.

>[AZURE.TIP]Als u een kopie van uw bestaande Linux VM maken met de gespecialiseerde status voor back-up of foutopsporing wilt, raadpleegt u [een kopie van een Linux virtuele machine waarop Azure maken](virtual-machines-linux-copy-vm.md). En als u uploaden van een VHD Linux vanuit een on-premises implementatie VM wilt, raadpleegt u [uploaden en een VM Linux maken op basis van de afbeelding van de aangepaste schijf](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u de volgende vereisten voldoet aan:

* **Azure VM gemaakt in het implementatiemodel resourcemanager** - als u een Linux VM hebt gemaakt, kunt u de [portal](virtual-machines-linux-quick-create-portal.md), de [Azure CLI](virtual-machines-linux-quick-create-cli.md)of [resourcemanager sjablonen](virtual-machines-linux-cli-deploy-templates.md). 

    Configureer de VM naar wens. Bijvoorbeeld [gegevensschijven toevoegt](virtual-machines-linux-add-disk.md), updates toepassen en -toepassingen installeren. 
* **Azure CLI** - installatie van de [Azure CLI](../xplat-cli-install.md) op een lokale computer.

## <a name="step-1-remove-the-azure-linux-agent"></a>Stap 1: De Azure Linux-agent verwijderen

De opdracht **waagent** met de parameter **identiteitsgegevens** eerst uitvoeren op de VM Linux. Deze opdracht Hiermee verwijdert u bestanden en gegevens om de VM voorbereiden noemt. Zie de [gebruikershandleiding Azure Linux Agent](virtual-machines-linux-agent-user-guide.md)voor meer informatie.

1. Verbinding maken met uw Linux VM met een SSH-client.

2. Typ de volgende opdracht in het venster SSH:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Deze opdracht alleen uitvoeren op een VM die u van plan bent om vast te leggen als een afbeelding. Dit betekent niet dat de afbeelding van alle gevoelige informatie is uitgeschakeld of geschikt voor distributie is.

3. Typ **y** om door te gaan. U kunt toevoegen de **-afdwingen** parameter om te voorkomen dat dit bevestigen.

4. Nadat de opdracht is voltooid, typt u **Afsluiten**. Deze stap Hiermee sluit u de SSH-client.

    
## <a name="step-2-capture-the-vm"></a>Stap 2: De VM vastleggen

Gebruik de CLI Azure generalize en de VM vastleggen. In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen zijn **myResourceGroup**, **myVnet**en **myVM**.

5. Open vanuit uw lokale computer, de Azure CLI en [Meld u aan bij uw Azure-abonnement](../xplat-cli-connect.md). 

6. Zorg ervoor dat in de modus van bronbeheer.

    ```
    azure config mode arm
    ```

7. Sluit de VM die u al hebt opgeheven met behulp van de volgende opdracht uit:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize de VM met de volgende opdracht uit:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. De opdracht **azure vm vastleggen** , waarin wordt vastgelegd de VM nu uitvoeren. In het volgende voorbeeld wordt de afbeelding VHD's zijn vastgelegd met namen die beginnen met **MyVHDNamePrefix**en de optie **-t** Hiermee geeft u een pad naar de sjabloon **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]De afbeeldingsbestanden VHD worden al dan niet standaard in hetzelfde opslag-account waarmee u de oorspronkelijke VM gemaakt. Gebruik het *hetzelfde opslag-account* voor de opslag van de VHD's voor elke nieuwe VMs u maken op basis van de afbeelding. 

6. Als u de locatie van een vastgelegde afbeelding zoekt, opent u de JSON-sjabloon in een teksteditor. Zoek in de **storageProfile**, de **uri** van de **afbeelding** zich bevindt in de container **system** . Bijvoorbeeld is de URI van de afbeelding van de schijf OS vergelijkbaar met`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Stap 3: Een VM uit de vastgelegde afbeelding maken
Nu de afbeelding met een sjabloon gebruiken om te maken van een VM Linux. Deze stappen leest u het gebruik van de Azure CLI en de JSON-bestandssjabloon die u vastgelegd als u wilt de VM maken in een nieuw virtuele netwerk.

### <a name="create-network-resources"></a>Netwerk resources maken

De sjabloon wilt gebruiken, moet u eerst een virtueel netwerk en NIC instellen voor uw nieuwe VM. Het is raadzaam om dat u een resourcegroep voor deze resources maken in de locatie waar uw afbeelding VM is opgeslagen. De opdrachten uitvoeren is vergelijkbaar met de volgende, wordt vervangen door namen voor de resources en een juiste Azure locatie ("centralus" in deze opdrachten):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>De Id van de NIC ophalen

Als u wilt implementeren een VM van de afbeelding met behulp van de JSON die u tijdens de opname hebt opgeslagen, moet u de Id van de NIC Dit alsnog doen door de volgende opdracht uit te voeren:

    azure network nic show MyResourceGroup1 myNIC

De **Id** in de uitvoer is vergelijkbaar met`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Een VM maken
Nu de volgende opdracht uit te maken van uw VM van de vastgelegde VM-afbeelding. Met de parameter **-f** kunt u opgeven van het pad naar het opgeslagen sjabloon JSON-bestand.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

In de uitvoer van de opdracht, wordt u gevraagd om een nieuwe naam voor de VM, de admin-gebruikersnaam en wachtwoord en de Id van de NIC die u eerder hebt gemaakt.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

In het onderstaande voorbeeld ziet u wat u te zien voor een succesvolle implementatie:

    + Initialisatie van de sjabloon configuraties en parameters
    + Maken van een distributie-info: sjabloon implementatie xxxxxxx gemaakt
    + Wachten op implementatie om uit te voeren van gegevens: DeploymentName: MyDeployment gegevens: ResourceGroupName: MyResourceGroup1 gegevens: ProvisioningState: is geslaagd van gegevens: tijdstempel: xxxxxxx gegevens: modus: incrementele gegevens: de waarde Type naam


    data:    ------------------  ------------  -------------------------------------

    gegevens: vmName tekenreeks myNewVM


    gegevens: vmSize tekenreeks Standard_D1


    gegevens: adminUserName tekenreeks myAdminuser


    gegevens: beheerderswachtwoord SecureString niet gedefinieerd


    gegevens: networkInterfaceId tekenreeks /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic info: groep implementatie opdracht OK maken

### <a name="verify-the-deployment"></a>Controleer of de implementatie

Nu SSH naar de virtuele machine die u hebt gemaakt om te controleren of de implementatie en de nieuwe VM gebruiken. Als u verbinding maakt via SSH, wilt zoeken het IP-adres van de VM die u hebt gemaakt door de volgende opdracht uit te voeren:

    azure network public-ip show MyResourceGroup1 myIP

Het openbare IP-adres wordt vermeld in de uitvoer van de opdracht. Standaard verbinding met de Linux VM door SSH op poort 22.

## <a name="create-additional-vms"></a>Aanvullende VMs maken
Gebruik de vastgelegde afbeelding en de sjabloon te implementeren extra VMs met behulp van de stappen in de vorige sectie. Andere opties voor het maken van VMs van de afbeelding zijn met behulp van een sjabloon quickstart of u de opdracht **azure vm maken** .

### <a name="use-the-captured-template"></a>De vastgelegde sjabloon gebruiken

Als u de vastgelegde afbeelding en de sjabloon, als volgt te werk (gedetailleerde in de vorige sectie):

* Zorg ervoor dat uw afbeelding VM zich in dezelfde opslag account die als host van uw VM VHD fungeert.
* Kopieer het sjabloonbestand JSON en geef een unieke naam voor de schijf OS van de nieuwe VM schijf (of VHD's). Bijvoorbeeld: Geef een unieke naam voor de **osDisk** VHD, vergelijkbaar met in het **storageProfile**, onder **vhd**, in het vak **uri**`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Maak een NIC in dezelfde of een ander virtuele netwerk.
* Met de gewijzigde sjabloon JSON-bestand, maakt u een implementatie in de resourcegroep waarin u het virtuele netwerk hebt ingesteld.

### <a name="use-a-quickstart-template"></a>Een sjabloon quickstart gebruiken

Als u het netwerk instellen automatisch als u een VM van de afbeelding maakt wilt, kunt u deze resources in een sjabloon. Zie bijvoorbeeld de GitHub [101-vm-van-gebruiker-afbeelding van de sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) . Deze sjabloon maakt een VM van uw aangepaste afbeelding en de benodigde virtueel netwerk, openbare IP-adres en NIC resources. Zie [hoe u een virtuele machine uit een aangepaste afbeelding met een resourcemanager-sjabloon maken](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/)voor stapsgewijze instructies voor het gebruik van de sjabloon in de portal van Azure.

### <a name="use-the-azure-vm-create-command"></a>Gebruik de azure vm opdracht maken

Meestal gemakkelijkst met een sjabloon resourcemanager een VM maken op basis van de afbeelding. U kunt echter maken de VM _imperatively_ met behulp van de opdracht **azure vm maken** met de **-Q** (**--afbeelding urn**) parameter. Als u deze methode gebruikt, doorgeven u ook de parameter **d** (**--os-schijf-vhd**) als u wilt opgeven van de locatie van het besturingssysteem VHD-bestand voor de nieuwe VM. Dit bestand moet zich in de container VHD's van het opslag-account waarin de afbeelding VHD-bestand is opgeslagen. De opdracht kopieert de VHD voor de nieuwe VM automatisch aan de container **VHD's** .

Voordat u **azure vm maken** met de afbeelding, moet u de volgende stappen uitvoeren:

1.  Een resourcegroep maken of een bestaande resourcegroep voor de implementatie identificeren.

2.  De bron van een openbare IP-adres en een resource NIC maken voor de nieuwe VM. Zie voor de stappen voor het maken van een virtueel netwerk, openbare IP-adres en NIC met behulp van de CLI, eerder in dit artikel. (**azure vm maken** een NIC ook kunt maken, maar u moet extra parameters voor een virtueel netwerk en subnet doorgeven.)


Voer een opdracht die URI's worden doorgegeven aan de nieuwe OS VHD-bestand zowel de bestaande afbeelding. In dit voorbeeld een grootte Standard_A1 VM is gemaakt in de regio Oost VS.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Voor extra de opdrachtopties uitvoeren `azure help vm create`.

## <a name="next-steps"></a>Volgende stappen

Als u wilt uw VMs met de CLI beheren, raadpleegt u de taken in [Deploy en virtuele machines beheren met behulp van Azure resourcemanager sjablonen en de CLI Azure](virtual-machines-linux-cli-deploy-templates.md).
