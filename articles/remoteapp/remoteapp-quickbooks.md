<properties 
    pageTitle="QuickBooks in Azure RemoteApp implementeren | Microsoft Azure" 
    description="Informatie over het delen van QuickBooks met Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Hoe kan u QuickBooks in Azure RemoteApp implementeren?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Gebruik de volgende informatie QuickBooks delen als een app in Azure RemoteApp.


U kunt QuickBooks 2015 Enterprise delen met Azure RemoteApp in een hybride of cloud-verzameling. Het bedrijfsbestand moet zich op een VM met QuickBooks-databaseserver die losstaat van de Azure RemoteApp-servers bevinden. Nooit opslaan van het bedrijfsbestand op uw afbeelding Azure RemoteApp - verlies van gegevens naar verwachting als u dit doet. Alleen QuickBooks Enterprise ondersteunt het QuickBooks-bestand op een extern delen met QuickBooks-databaseserver toegankelijk via standaard Windows-netwerken hostingprovider.   

> [AZURE.IMPORTANT] De QuickBooks-databaseserver die als host het bedrijfsbestand fungeert moet zich op een aparte VM binnen de dezelfde VNET als de verzameling Azure RemoteApp bevinden.  

## <a name="steps-to-deploy-quickbooks"></a>Stappen voor het implementeren van QuickBooks

1. Maken van een VM Azure en QuickBooks, QuickBooks-databaseserver, installeren en plaats het bedrijfsbestand op een VM Azure.  Zorg ervoor dat de firewallregels correct configureren.
2. QuickBooks installeren op een [aangepaste afbeelding](remoteapp-imageoptions.md) en maak een [siteverzameling van Azure RemoteApp](remoteapp-collections.md)cloud of hybride, binnen de exacte dezelfde VNET waarin de VM de QuickBooks-databaseserver hosten door de bedrijfsbestanden zich bevindt. 
3.  [Publiceren](remoteapp-publish.md) QuickBooks-app voor gebruikers
4.  De Azure RemoteApp gehoste QuickBooks-client starten, navigeren met behulp van de standaard Windows-netwerken voor VM waarop de QuickBooks-database-server en open het bedrijfsbestand. 

## <a name="documentation-references"></a>Documentatie verwijzingen

- QuickBooks [ondersteunde configuraties](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [distributieopties](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

U kunt ook Mijn Ignite presentatie, [over grondbeginselen van Microsoft Azure RemoteApp-beheer- en](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - vooruitspoelen naar 1:02:45 om te gaan naar het onderdeel QuickBooks raadplegen.

## <a name="deployment-architecture"></a>Implementatie-architectuur

![QuickBooks + Azure RemoteApp-implementatie](./media/remoteapp-quickbooks/ra-quickbooks.png)