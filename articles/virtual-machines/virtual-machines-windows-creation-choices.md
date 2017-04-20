<properties
    pageTitle="Andere manieren om te maken van een Windows-VM | Microsoft Azure"
    description="Worden de verschillende manieren voor het maken van een virtuele Windows-computer met bronbeheer."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Andere manieren om te maken van een virtuele Windows-computer met bronbeheer

Azure biedt verschillende manieren om te maken van een virtuele machine omdat virtuele machines zijn geschikt voor verschillende gebruikers en doeleinden. Dit betekent dat u aanbrengen verschillende opties over de virtuele machine en hoe wilt u deze hebt gemaakt. In dit artikel biedt een overzicht van deze opties en koppelingen naar instructies.

## <a name="azure-portal"></a>Azure-portal

Met behulp van de Azure portal is een eenvoudige manier om een virtuele machine, uitproberen met name als u pas met Azure begint. 

[Een virtuele machine met Windows met behulp van de portal maken](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Sjabloon

Virtuele machines vereisen een combinatie van resources (zoals een beschikbaarheid sets en opslag-accounts). In plaats van implementeert en elke resource afzonderlijk beheren, kunt u een sjabloon Azure resourcemanager die wordt geïmplementeerd en bepalingen alle bronnen in één, gecoördineerde bewerking maken.

- [Een virtuele Windows-computer met een resourcemanager-sjabloon maken](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Als u liever in een opdrachtshell werkt, kunt u Azure PowerShell.

- [Maken van een Windows-VM via PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Gebruik Visual Studio kunt maken, beheren en implementeren van VMs met de hulpmiddelen Azure voor Visual Studio en de SDK Azure.

[Azure Tools voor Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

