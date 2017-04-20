<properties
    pageTitle="Hulpprogramma's en PaaS services voor Azure Stack | Microsoft Azure"
    description="Leer hoe u aan de slag met PaaS services Azure gestapelde."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Hulpprogramma's voor Azure stapel

U kunt de hulpmiddelen voor hieronder beschreven downloaden. Als u wilt worden gesteld van nieuwe services en hulpprogramma's voor, volgt u #AzureStack voor Twitter.

## <a name="template-tools"></a>Hulpmiddelen voor de sjabloon

### <a name="azure-stack-github-templates"></a>Azure stapel Github-sjablonen
Verken de groeiende verzameling [Azure stapel GitHub sjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates). Net als [Azure](https://github.com/Azure/azure-quickstart-templates), wordt deze sjablonen voor Azure resourcemanager 'Snel aan de slag' ernaar te u aan de slag met eenvoudige bouwstenen en voorbeelden, gereed is voor de implementatie in de Microsoft Azure stapel Technical Preview bewijs van Concept-omgeving. Eerste partijen werkbelasting voorbeelden voor het opgenomen zijn [ad-niet-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014-niet-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013-niet-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), als u ook enkele eenvoudige 101 sjablonen [101-eenvoudige-windows-vm wilt](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Marketplace item verpakking hulpmiddel en voorbeeld
[Downloaden en gebruiken van het hulpmiddel verpakking](http://www.aka.ms/azurestackmarketplaceitem) om items marketplace voor uw eigen aangepaste sjablonen toevoegen aan de stapel van Azure marketplace te maken. Instructies voor het maken van een item marketplace en beschikbaar maken voor uw tenants vindt u in het [item zelf Marketplace maken](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Speciale tools voor ontwikkelaars


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Gebruik Visual Studio en Azure stapel TP2 op de MAS CON01 virtuele machine
Als u Visual Studio op de console VM gebruiken om te werken met Azure stapel sjablonen wilt, moet u de juiste versies van de vereiste hulpmiddelen installeren. Gebruik de volgende procedure bij het installeren van de ondersteunde versies voor TP2.

1. Verbinding met extern bureaublad gebruiken voor aanmelding bij de MAS CON01 virtuele machine met de referenties azurestack\azurestackadmin.
2. Installeren en open Web Platform installatieprogramma.
3. Zoek en installeer **Visual Studio-Community-2015 met Microsoft Azure SDK - 2.9.5**.
4. Met de **Software**, verwijder de **Microsoft Azure PowerShell (sep)** die wordt geïnstalleerd als onderdeel van de SDK.
5. Open het installatieprogramma van de Web-Platform.
6. Zoek en installeer **Microsoft Azure PowerShell - Azure stapel Technical Preview 2**. 
7. Start opnieuw op de MAS CON01 virtuele machine.
8. Verbinding met extern bureaublad gebruiken voor aanmelding bij de MAS CON01 virtuele machine met de referenties azurestack\azurestackadmin.
9. Open Visual Studio en controleer of u kunt verbinding maken met de stapel Azure-omgeving, sjablonen en enzovoort. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell is een module die cmdlets voor het beheren van Azure en Azure stapel met Windows PowerShell biedt. U kunt de cmdlets gebruiken voor het maken, te testen, te implementeren en beheren van oplossingen en services die via de stapel Azure-platform is bezorgd.
[SDK van Azure PowerShell downloadt](http://aka.ms/azStackPsh) en [installeert u PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure cross platform opdracht lijn interfaces
Snel de opdrachtregel Azure (Azure CLI) als u een set open source shell-opdrachten gebruiken voor het maken en beheren van resources in Microsoft Azure stapel wilt installeren.

[De vensters CLI downloaden](http://aka.ms/azstack-windows-cli)

[Downloaden van de Mac CLI](http://aka.ms/azstack-linux-cli)

[De CLI Linux downloaden](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Als u zich op een Mac of Linux-computer, kunt u ook de CLI verkrijgen met de opdracht`npm install -g azure-cli@0.9.11`</br>
> + Als u certificaat validatieproblemen krijgt, voert u de opdracht`set NODE_TLS_REJECT_UNAUTHORIZED=0`
