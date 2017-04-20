<properties
   pageTitle="Verwijderen van een Azure cluster en de bijbehorende bronnen | Microsoft Azure"
   description="Leer hoe u volledig verwijderen van een Service stof cluster beide verwijderen met het cluster resourcegroep of door selectief u resources verwijdert."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Verwijderen van een Service stof cluster op Azure en de resources die wordt gebruikt

Een Service stof cluster bestaat uit de volgende van vele andere Azure resources naast de cluster resource zelf. Zo als geheel wilt verwijderen een cluster Service stof moet ook u alle resources die dit bestaat uit verwijderen.
U hebt twee opties: een resourcegroep die het cluster in verwijderen (die Hiermee verwijdert u de resource cluster en andere bronnen in de resourcegroep) of de resource cluster specifiek verwijderen en de bijbehorende resources (maar geen andere bronnen in de resourcegroep).

>[AZURE.NOTE] De cluster resource **niet** verwijdert, alle andere bronnen die uw cluster Service stof bestaat uit.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Verwijderen van de hele resourcegroep (RG) waarop de Service stof cluster zich bevindt

Dit is de eenvoudigste manier om ervoor te zorgen dat u alle resources die zijn gekoppeld aan uw cluster, waaronder de resourcegroep verwijderen. U kunt de resourcegroep via PowerShell verwijderen of via de portal van Azure. Als uw resourcegroep resources die niet zijn gerelateerd aan de Service stof cluster bevat, kunt u specifieke resources verwijderen.

### <a name="delete-the-resource-group-using-azure-powershell"></a>De resourcegroep via Azure PowerShell verwijderen

U kunt ook de resourcegroep verwijderen door de volgende Azure PowerShell-cmdlets uit te voeren. Zorg ervoor dat Azure PowerShell 1.0 of hoger is geïnstalleerd op uw computer. Als u dit nog niet hebt gedaan, volgt u de stappen in [installeren en configureren van Azure PowerShell.](../powershell-install-configure.md)

Open een PowerShell-venster en voer de volgende cmdlets uit PS:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Wordt u gevraagd het verwijderen te bevestigen als u niet hebt gebruikt de *-dwingen* optie. Klik op bevestiging worden de RG en alle resources die deze bevat verwijderd.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Verwijderen van een resourcegroep in de portal van Azure  

1. Meld u bij de [portal van Azure](https://portal.azure.com).
2. Navigeer naar de Service stof cluster die u wilt verwijderen.
3. Klik op de naam van de Resource-groep op de pagina met cluster essentials.
4. Hiermee opent u de **Resource groep Essentials** -pagina.
5. Klik op **verwijderen**.
6. Volg de instructies op de pagina voor het verwijderen van de resourcegroep worden voltooid.

![Resourcegroep verwijderen][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Verwijderen van de resource cluster en de resources die wordt gebruikt, maar geen andere bronnen in de resourcegroep

Als uw resourcegroep alleen de resources die zijn gerelateerd aan de Service stof cluster die u wilt verwijderen bevat, is het gemakkelijker om de hele resource-groep te verwijderen. Als u de resources één voor één in uw resourcegroep selectief verwijderen wilt, klikt u vervolgens deze stappen.

Als u uw cluster met behulp van de portal of gebruik een van de Service stof resourcemanager-sjablonen uit de galerie met sjablonen geïmplementeerd, klikt u vervolgens alle bronnen die gebruikmaakt van het cluster gemarkeerd met de volgende twee tags. U kunt ze gebruiken om te bepalen welke bronnen die u wilt verwijderen.

***Taggen #1:*** Toets = clusternaam, waarde = 'naam van het cluster'

***Taggen #2:*** Toets = bronnaam, waarde = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Specifieke bronnen in de portal van Azure verwijderen

1. Meld u bij de [portal van Azure](https://portal.azure.com).
2. Navigeer naar de Service stof cluster die u wilt verwijderen.
3. Ga naar **alle instellingen** op het blad essentials.
4. Klik op **labels** onder **Beheer van de Resource** in het blad instellingen.
5. Klik op een van de **labels** in het blad tags om een lijst met alle resources met deze tag wordt gebruikt.

    ![Resource-labels][ResourceTags]

6. Nadat u de lijst met tags resources hebt, klikt u op elk van de resources en verwijder deze.

    ![Gelabelde Resources][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>De bronnen via Azure PowerShell verwijderen

U kunt de resources één voor één verwijderen door de volgende Azure PowerShell-cmdlets uit te voeren. Zorg ervoor dat Azure PowerShell 1.0 of hoger is geïnstalleerd op uw computer. Als u dit nog niet hebt gedaan, volgt u de stappen in [installeren en configureren van Azure PowerShell.](../powershell-install-configure.md)

Open een PowerShell-venster en voer de volgende cmdlets uit PS:

```powershell
Login-AzureRmAccount
```
Voor elk van de bronnen wilt u verwijderen, voer de volgende opdracht uit:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Als u wilt verwijderen van de resource cluster, voert u het volgende:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Volgende stappen
Lees de volgende ook meer informatie over het upgraden van een cluster en partitioneren services:

- [Meer informatie over cluster upgrades](service-fabric-cluster-upgrade.md)
- [Meer informatie over het partitioneren statuscontrole services voor maximale schaal](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
