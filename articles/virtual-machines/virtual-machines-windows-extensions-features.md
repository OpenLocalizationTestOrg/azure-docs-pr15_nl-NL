<properties
 pageTitle="VM extensions en functies | Microsoft Azure"
 description="Leer welke extensies zijn beschikbaar voor Azure virtuele machines, gegroepeerd op wat ze geven of te verbeteren."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Over VM extensies en functies

## <a name="azure-vm-extensions"></a>Azure VM extensies

Azure virtuele machines extensies zijn kleine toepassingen die post-implementatie configuratie en automatisering taak op Azure virtuele Machines bieden. Als een virtuele Machine vereist is voor software die moet worden geïnstalleerd, anti-virus uitvoert protection of Docker configuratie, kan er bijvoorbeeld extensie VM worden gebruikt om deze taken te voltooien. Azure VM extensies kunnen worden uitgevoerd met de CLI Azure, PowerShell Resource beheren sjablonen en de Azure-portal. Extensies kunnen worden geleverd met de implementatie van een nieuwe virtuele machines of voor een bestaande systeem uitgevoerd.

Dit document bevat vereisten voor Azure virtuele machines extensie en informatie over de beschikbare VM extensies detecteren. 

## <a name="azure-vm-agent"></a>Azure VM Agent

De Azure VM-Agent beheert de interactie tussen een Azure virtuele machines en de Controller uit Azure stof. De VM-agent is verantwoordelijk voor veel functionele aspecten van implementeert en beheren van Azure virtuele Machines, inclusief VM extensies uitgevoerd. De Azure VM-Agent is vooraf geïnstalleerd op Azure-afbeeldingen en kan worden geïnstalleerd op ondersteunde besturingssystemen. 

Zie voor informatie over ondersteunde besturingssystemen en de installatie-instructies kunt [Azure virtuele machines Agent](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Kennismaken met VM extensies

Groot aantal verschillende VM extensies zijn beschikbaar voor gebruik met Azure virtuele Machines. Als u wilt zien voor een volledige lijst, voer de volgende opdracht met de CLI Azure, de locatie vervangen door de gewenste locatie.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Algemene VM extensies

|De Extensienaam van de   |Beschrijving   |Meer informatie   |
|---|---|---|
|Aangepast Script-extensie voor Windows  | Uitvoeren van scripts ten opzichte van een Azure virtuele machines  |[Aangepast Script-extensie voor Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC-extensie voor Windows | PowerShell DSC (gewenste provinciale configuratie) extensie.  | [Docker VM-extensie](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Diagnostische gegevens van Azure-extensie | Azure diagnostische gegevens beheren |[Diagnostische gegevens van Azure-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
