<properties
    pageTitle="Azure resourcemanager sjablonen gebruiken in Azure stapel (tenant ontwikkelaars) | Microsoft Azure"
    description="Informatie over het gebruik van Azure resourcemanager sjablonen Azure gestapelde te implementeren en inrichten van alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure resourcemanager sjablonen gebruiken in Azure-Stack

Azure resourcemanager sjablonen implementeren en inrichten van alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft. U definieert de bronnen voor de toepassing en hoe deze wordt geïmplementeerd.  U kunt ook sjablonen wijzigingen wilt aanbrengen in de resources in de resourcegroep implementeren.

Deze sjablonen kunnen worden geïmplementeerd met de portal van Microsoft Azure stapel, PowerShell, de opdrachtregel en Visual Studio.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

De volgende sjablonen zijn beschikbaar op [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Implementatie van SharePoint (niet-hoog beschikbaarheid)

De extensie DSC PowerShell gebruiken om het maken van een SharePoint 2013-farm met het volgende:

-   Een virtueel netwerk

-   Drie opslag-accounts

-   Twee externe netwerktaakverdelers

-   Eén VM geconfigureerd als een domeincontroller voor een nieuw bos met één domein

-   Eén VM geconfigureerd als een SQL Server-2014 zelfstandige server

-   Eén VM een één computer SharePoint 2013-farm configureren

## <a name="deploy-ad-non-high-availability"></a>Implementeren van AD (niet-hoog beschikbaarheid)

De extensie DSC PowerShell gebruiken om het maken van een AD domeincontroller-server die het volgende bevat:

-   Een virtueel netwerk

-   Één opslag-account

-   Een externe taakverdeling

-   Eén VM geconfigureerd als een domeincontroller voor een nieuw bos met één domein

## <a name="deploy-adsql-non-high-availability"></a>Implementeren van AD/SQL (niet-hoog beschikbaarheid)

De extensie DSC PowerShell gebruiken om het maken van een SQL Server-2014 zelfstandige server die het volgende bevat:

-   Een virtueel netwerk

-   Twee opslag-accounts

-   Een externe taakverdeling

-   Eén VM geconfigureerd als een domeincontroller voor een nieuw bos met één domein

-   Eén VM geconfigureerd als een SQL Server-2014 zelfstandige server

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Gebruik de extensie PowerShell DSC configureren van een bestaande VM lokale Configuration Manager (KGV) en deze naar een Azure automatisering Account DSC halen Server registreren.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Een virtuele machine maken op basis van de afbeelding van een gebruiker

Maak een virtuele machine uit een aangepaste afbeelding. Deze sjabloon implementeert ook een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="simple-vm"></a>Eenvoudige VM

Een eenvoudige Windows VM die een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface bevat implementeren.

## <a name="cancel-a-running-template-deployment"></a>Een actieve sjabloonimplementatie annuleren

Als u een actieve sjabloonimplementatie, gebruikt u de `Stop-AzureRmResourceGroupDeployment` PowerShell-cmdlet.


## <a name="next-steps"></a>Volgende stappen

[Sjablonen met de portal implementeren](azure-stack-deploy-template-portal.md)

[Azure resourcemanager-overzicht](../azure-resource-manager/resource-group-overview.md)

