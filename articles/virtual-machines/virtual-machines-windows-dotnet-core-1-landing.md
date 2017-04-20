<properties
   pageTitle="Azure virtuele machines DotNet Core zelfstudie 1 | Microsoft Azure"
   description="Azure virtuele machines DotNet Core zelfstudie"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Toepassing implementaties naar Azure virtuele Machines automatiseren

Deze vierdelige reeks details implementatie en configuratie van Azure resource en toepassingen die gebruikmaken van sjablonen Azure Resource beheren. In deze reeks, een voorbeeldsjabloon wordt geïmplementeerd en de implementatiesjabloon onderzocht. Het doel van deze reeks is om te informeren op de relatie tussen Azure resources en op te geven handen erdoorheen implementeren van volledig geïntegreerde resourcemanager Azure-sjablonen op. In dit document wordt ervan uitgegaan dat een eenvoudige mate van kennis met Azure resourcemanager, vertrouwd raken voordat deze zelfstudie wordt gestart met Azure resourcemanager basisbegrippen.

## <a name="music-store-application"></a>Muziek store-servicetoepassing

De steekproef gebruikt in deze reeks is een .net Core-toepassing een muziek-Store winkelen voor ervaring simuleren. Deze toepassing kan worden toegepast op een Linux- of Windows virtuele systeem, voorbeeld implementaties voor beide zijn gemaakt. De toepassing bevat een webtoepassing en een SQL-database. Voordat u de artikelen in deze reeks leest, implementeren de toepassing met de implementatie-knop gevonden op deze pagina. Wanneer volledig geïmplementeerd, is de architectuur toepassing / Azure ziet eruit als in het volgende diagram. 

De sjabloon muziek Store resourcemanager vindt u hier, [Muziek Store Windows-sjabloon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Muziek Store-toepassing](./media/virtual-machines-windows-dotnet-core/music-store.png)

Elk van deze onderdelen, waaronder de koppelen sjabloon JSON onderzocht in de volgende vier artikelen.

- [**Toepassingsarchitectuur**](./virtual-machines-windows-dotnet-core-2-architecture.md) – toepassingsonderdelen zoals websites en databases moet worden gehost op Azure computer resources zoals virtuele machines en Azure SQL-databases. In dit document worden doorlopen toewijzing berekeningscluster nodig, Azure resources en implementeren van deze resources op basis van een sjabloon resourcemanager Azure. 

- [**Toegang en beveiliging**](./virtual-machines-windows-dotnet-core-3-access-security.md) – wanneer hostingprovider toepassingen in Azure wordt aangegeven, is het moet u rekening houden met hoe de toepassing wordt geopend, en hoe verschillende toegang tot de onderdelen van toepassing elkaar. In dit document details leveren en beveiligen van internettoegang tot een toepassing en tussen toepassingsonderdelen.

- [**Beschikbaarheid en schaal**](./virtual-machines-windows-dotnet-core-4-availability-scale.md) – beschikbaarheid en schaal verwijzen naar de mogelijkheid toepassingen om te blijven uitvoeren tijdens infrastructuur downtime en de mogelijkheid om te berekenen resources om te voldoen aan de toepassing aanvraag schaal. Deze documentgegevens de onderdelen die nodig zijn om te implementeren van een taakverdeling en maximaal beschikbare toepassing.

- [**Implementatie van toepassing**](./virtual-machines-windows-dotnet-core-5-app-deployment.md) - bij de implementatie van toepassingen naar Azure virtuele Machines, de methode waarmee de binaire bestanden van toepassing zijn geïnstalleerd op de virtuele Machine moet worden beschouwd. In dit document details automatiseren toepassingsinstallatie met behulp van Azure virtuele machines aangepast scriptextensies.

Het doel bij het ontwikkelen van Azure resourcemanager sjablonen is de implementatie van Azure-infrastructuur en de installatie en configuratie van toepassingen die wordt gehost op deze Azure infrastructuur automatiseren. Werken via deze artikelen vindt u een voorbeeld van dit probleem.

## <a name="deploy-the-music-store-application"></a>De muziek store-toepassing implementeren

De muziek Store-toepassing kan worden geïmplementeerd met deze knop.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

De sjabloon Azure Resource Manager is vereist voor de volgende parameterwaarden.

|Parameternaam |Beschrijving   |
|---|---|
|ADMINUSERNAME   | Beheerder-gebruikersnaam die wordt gebruikt in de virtuele machine en de Azure SQL-Database.  |
|BEHEERDERSWACHTWOORD | Wachtwoord dat wordt gebruikt in de Azure virtuele machines en SQL-Database.  |
|NUMBEROFINSTANCES | Het aantal virtuele machines moet worden gemaakt. Elk van deze virtuele machines de webtoepassing muziek Store hosten en al het verkeer gelijkmatig verdeeld over deze is. |
|PUBLICIPADDRESSDNSNAME | Globaal unieke DNS-naam die is gekoppeld aan het openbare IP-adres. |

Wanneer u de sjabloonimplementatie is voltooid, gaat u naar het openbare IP-adres met een internetbrowser. De .net Core muziek site worden weergegeven.

## <a name="next-steps"></a>Volgende stappen

<hr>

[Stap 1: toepassingsarchitectuur met Azure resourcemanager-sjablonen](./virtual-machines-windows-dotnet-core-2-architecture.md)

[Stap 2: toegang en beveiliging in Azure resourcemanager-sjablonen](./virtual-machines-windows-dotnet-core-3-access-security.md)

[Stap 3: beschikbaarheid en schaal in Azure resourcemanager-sjablonen](./virtual-machines-windows-dotnet-core-4-availability-scale.md)

[Stap 4 - toepassingsimplementatie waarbij Azure resourcemanager-sjablonen](./virtual-machines-windows-dotnet-core-5-app-deployment.md)


