<properties
    pageTitle="Migreren IaaS resources van klassiek naar Azure resourcemanager met behulp van Azure CLI | Microsoft Azure"
    description="In dit artikel begeleidt bij de migratie platform ondersteund van resources van klassiek naar Azure resourcemanager met behulp van Azure CLI"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migreren IaaS resources van klassiek naar Azure resourcemanager met behulp van Azure CLI

Volgende stappen uit hoe u Azure opdrachtregel-interface (CLI)-opdrachten gebruiken om te migreren van infrastructuur als een (IaaS) serviceresources uit het implementatiemodel klassieke aan het implementatiemodel resourcemanager Azure. In het artikel moet de [Azure CLI](../xplat-cli-install.md).

>[AZURE.NOTE] Alle bewerkingen die hier worden beschreven, zijn idempotency is ingeschakeld. Als u problemen dan een niet-ondersteunde functie of een configuratiefout ondervindt, is het raadzaam dat u opnieuw de voorbereiden proberen, of bewerking afgebroken. Het platform wordt vervolgens probeer het opnieuw.

## <a name="step-1-prepare-for-migration"></a>Stap 1: Migratie voorbereiden

Hier volgen enkele aanbevolen procedures die wordt aangeraden terwijl u migreren IaaS resources uit klassieke naar resourcemanager evalueren:

- Lees door de [lijst met niet-ondersteunde configuraties of functies](virtual-machines-windows-migration-classic-resource-manager.md). Hebt u virtuele machines die niet-ondersteunde configuraties of functies gebruiken, wordt u aangeraden wachten op de ondersteuning van de configuratie van functies/om te worden aangekondigd. U kunt ook deze functie verwijderen of verplaatsen uit die configuratie migratie inschakelen als deze aan uw behoeften voldoet.
-   Als u scripts die uw infrastructuur en toepassingen vandaag implementeren hebben geautomatiseerde, kunt u instellingen voor een soortgelijke maken met behulp van deze scripts voor migratie. U kunt ook de steekproef omgevingen instellen met behulp van de Azure-portal.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Stap 2: Uw abonnement instellen en de provider registreren

Voor de migratie-scenario's, moet u voor het instellen van uw omgeving voor beide klassieke en resourcemanager. [Azure CLI installeren](../xplat-cli-install.md) en [selecteert u uw abonnement](../xplat-cli-connect.md).

Meld u aan bij uw account.
    
    azure login

Selecteer het Azure abonnement met behulp van de volgende opdracht uit.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Registratie is een tijdstap, maar deze moet worden gedaan eenmaal voordat u probeert migratie. Zonder te registreren ziet u het volgende foutbericht weergegeven 

>   *BadRequest: Abonnement is niet geregistreerd voor migratie.* 

Met de provider van de resource migratie registreren met behulp van de volgende opdracht uit. Houd er rekening mee dat in sommige gevallen, deze opdracht treedt er een time-out. De registratie is geslaagd.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Wacht vijf minuten voor de registratie om te voltooien. U kunt de status van de goedkeuringswerkstroom controleren met behulp van de volgende opdracht uit. Zorg ervoor dat RegistrationState `Registered` voordat u verdergaat.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Schakel nu over CLI naar de `asm` modus.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 3: Zorg ervoor dat u er voldoende cores Azure resourcemanager VM Azure buiten het gebied van uw huidige implementatie of VNET

Voor deze stap moet u overschakelen naar `arm` modus. Deze stap met de volgende opdracht uit.

```
azure config mode arm
```

U kunt de volgende opdracht uit CLI gebruiken om te controleren van de huidige hoeveelheid cores die u in Azure resourcemanager hebt. Meer informatie over core quota, raadpleegt u [limieten en Azure bronbeheer](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Wanneer u klaar bent met het verifiëren van deze stap, kunt u Ga terug naar `asm` modus.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Stap 4: Optie 1 - virtuele machines in een cloudservice migreren 

De lijst met cloudservices verkrijgen met behulp van de volgende opdracht en kies vervolgens de cloudservice die u wilt migreren. Houd er rekening mee dat als de VMs in de cloudservice zich in een virtueel netwerk of als ze web/werknemer rollen hebt, u een foutbericht wordt weergegeven krijgt.

    azure service list

Voer de volgende opdracht aan de implementatie-naam voor de cloudservice van de uitgebreide uitvoer. In de meeste gevallen is de naam van de implementatie hetzelfde als de naam van de cloud-service.

    azure service show <serviceName> -vv

De virtuele machines in de cloudservice voor migratie voorbereiden. U hebt twee opties waaruit u kunt kiezen.

Als u de VMs migreren naar een virtueel platform gemaakt-netwerk wilt, gebruikt u de volgende opdracht uit.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Als u migreren naar een bestaand virtual netwerk in het implementatiemodel resourcemanager wilt, gebruikt u de volgende opdracht uit.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Nadat de voorbereidingsbewerking voltooid is, kunt u zoeken via de uitgebreide uitvoer voor het ophalen van de migratiestatus van de VMs en zorg ervoor dat ze afkomstig zijn de `Prepared` staat.

    azure vm show <vmName> -vv

Controleer de configuratie voor de voorbereide resources met behulp van CLI of de Azure-portal. Als u niet klaar voor migratie bent en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht uit.

    azure service deployment abort-migration <serviceName> <deploymentName>

Als de voorbereide configuratie tevreden bent, kunt u naar voren verplaatsen en de resources doorvoeren met behulp van de volgende opdracht uit.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Stap 4: Optie 2 - virtuele machines in een virtueel netwerk migreren

Kies het virtuele netwerk dat u wilt migreren. Houd er rekening mee dat als het virtuele netwerk web/werknemer rollen of VMs met niet-ondersteunde configuraties bevat, u een foutbericht gegevensvalidatie krijgt.

Worden alle virtuele netwerken het abonnement met behulp van de volgende opdracht uit.

    azure network vnet list
    
De uitvoer ziet er ongeveer zo uitziet:

![Schermafbeelding van de opdrachtregel met de naam van de hele virtueel netwerk is gemarkeerd.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Klik in het bovenstaande voorbeeld is de **virtualNetworkName** de hele naam **"Groep classicubuntu16 classicubuntu16"**.

Het virtuele netwerk van uw keuze migratie voorbereiden met behulp van de volgende opdracht uit.

    azure network vnet prepare-migration <virtualNetworkName>

Controleer de configuratie voor de voorbereide virtuele machines via CLI of via de portal van Azure. Als u niet klaar voor migratie bent en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht uit.

    azure network vnet abort-migration <virtualNetworkName>

Als de voorbereide configuratie tevreden bent, kunt u naar voren verplaatsen en de resources doorvoeren met behulp van de volgende opdracht uit.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Stap 5: Een opslag-account migreren

Wanneer u klaar bent met de virtuele machines gemigreerd, wordt aangeraden u het account opslag migreren.

Het account opslag-migratie voorbereiden met behulp van de volgende opdracht uit

    azure storage account prepare-migration <storageAccountName>

Controleer de configuratie voor het account voorbereide opslag via CLI of via de portal van Azure. Als u niet klaar voor migratie bent en u wilt teruggaan naar de oude staat, gebruikt u de volgende opdracht uit.

    azure storage account abort-migration <storageAccountName>

Als de voorbereide configuratie tevreden bent, kunt u naar voren verplaatsen en de resources doorvoeren met behulp van de volgende opdracht uit.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Volgende stappen

- [Platform ondersteund migratie van IaaS resources uit klassieke naar resourcemanager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Technische uitgebreide kennismaking op platform ondersteund migratie van klassiek naar resourcemanager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
