<properties
 pageTitle="VM extensions en functies | Microsoft Azure"
 description="Leer welke extensies zijn beschikbaar voor Azure virtuele machines, gegroepeerd op wat ze geven of te verbeteren."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Over VM extensies en functies

## <a name="azure-vm-extensions"></a>Azure VM extensies

Azure virtuele machines extensies zijn kleine toepassingen die post-implementatie configuratie en automatisering taak op Azure virtuele Machines bieden. Als een virtuele Machine vereist is voor software die moet worden geïnstalleerd, anti-virus uitvoert protection of Docker configuratie, kan er bijvoorbeeld extensie VM worden gebruikt om deze taken te voltooien. Azure VM extensies kunnen worden uitgevoerd met de CLI Azure, PowerShell Resource beheren sjablonen en de Azure-portal. Extensies kunnen worden geleverd met de implementatie van een nieuwe virtuele machines of voor een bestaande systeem uitgevoerd.

Dit document bevat vereisten voor Azure virtuele machines extensie en informatie over de beschikbare VM extensies detecteren. 

## <a name="azure-vm-agent"></a>Azure VM Agent

De Azure VM-Agent beheert de interactie tussen een Azure virtuele machines en de Controller uit Azure stof. De VM-agent is verantwoordelijk voor veel functionele aspecten van implementeert en beheren van Azure virtuele Machines, inclusief VM extensies uitgevoerd. De Azure VM-Agent is vooraf geïnstalleerd op Azure-afbeeldingen en kan worden geïnstalleerd op ondersteunde besturingssystemen. 

Zie voor informatie over ondersteunde besturingssystemen en de installatie-instructies kunt [Azure Linux Agent User Guide](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Kennismaken met VM extensies

Groot aantal verschillende VM extensies zijn beschikbaar voor gebruik met Azure virtuele Machines. Als u wilt zien voor een volledige lijst, voer de volgende opdracht met de CLI Azure, de locatie vervangen door de gewenste locatie.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Algemene VM extensies

|De Extensienaam van de   |Beschrijving   |Meer informatie   |
|---|---|---|
|Aangepast Script-extensie voor Linux  | Uitvoeren van scripts ten opzichte van een Azure virtuele machines  |[Aangepast Script-extensie voor Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Docker-extensie |Hiermee installeert u de daemon Docker ter ondersteuning van externe Docker-opdrachten.  | [Docker VM-extensie](./virtual-machines-linux-dockerextension.md)  |
|VM Access-extensie | Krijgen van toegang aan Azure virtuele machines  |[VM Access-extensie](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Diagnostische gegevens van Azure-extensie |Azure diagnostische gegevens beheren |[Diagnostische gegevens van Azure-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

