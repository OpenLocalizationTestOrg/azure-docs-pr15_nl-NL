<properties
   pageTitle="Navigeren en selecteer Windows VM afbeeldingen | Microsoft Azure"
   description="Informatie over het bepalen van de uitgever, aanbieding en SKU voor afbeeldingen bij het maken van een virtuele Windows-computer met het implementatiemodel resourcemanager."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Navigeren en selecteert u Windows VM afbeeldingen in Azure met PowerShell of de CLI

In dit onderwerp wordt beschreven hoe VM afbeelding uitgevers, aanbiedingen, SKU's en versies zoeken voor elke locatie waarin u mogelijk implementeren. Om een voorbeeld, ziet u enkele veelgebruikte Windows VM afbeeldingen:

## <a name="table-of-commonly-used-windows-images"></a>Tabel van veelgebruikte Windows-afbeeldingen


| PublisherName                        | Aanbieding                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Enterprise-geoptimaliseerd-voor-DW      |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Enterprise-geoptimaliseerd-voor-OLTP    |
| Legitieme Microsoft Windows Server           | WindowsServer                              | 2012-R2-Datacenter                  |
| Legitieme Microsoft Windows Server           | WindowsServer                              | 2012-Datacenter               |
| Legitieme Microsoft Windows Server           | WindowsServer                              | 2008 R2 SP1 |
| Legitieme Microsoft Windows Server           | WindowsServer                              | Windows Server-Technical Preview |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
