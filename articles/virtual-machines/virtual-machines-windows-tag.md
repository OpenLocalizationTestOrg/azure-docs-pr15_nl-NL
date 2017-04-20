<properties
   pageTitle="Hoe u een melding geven voor een VM | Microsoft Azure"
   description="Meer informatie over een virtuele Windows-computer die is gemaakt in het implementatiemodel resourcemanager met Azure labelen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Hoe u een melding geven voor een virtuele Windows-computer in Azure wordt aangegeven


In dit artikel worden de verschillende manieren om te markeren van een virtuele Windows-computer in Azure wordt aangegeven via het objectmodel resourcemanager-implementatie. Labels komen van de gebruiker gedefinieerde sleutel/waardeparen die rechtstreeks in een resource of een resourcegroep kunnen worden gebracht. Azure ondersteunt momenteel maximaal 15 tags per resource en resourcegroep. Labels kunnen worden ingesteld voor een bron op het moment van maken of toegevoegd aan een bestaande bron. Houd er rekening mee dat tags worden ondersteund voor resources die zijn gemaakt via het alleen resourcemanager implementatiemodel. Als u een melding geven voor een Linux virtuele machine wilt, raadpleegt u [hoe u een melding geven voor een Linux virtuele machine in Azure wordt aangegeven](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Labelen met PowerShell

Als u wilt maken, toevoegen en verwijderen van tags via PowerShell, moet u eerst de [PowerShell-omgeving met Azure resourcemanager][]instellen. Als u de instelling hebt voltooid, kunt u de labels op berekeningscluster-, netwerk- en resources op mijn maken of nadat de resource is gemaakt via PowerShell kunt plaatsen. In dit artikel wordt vestigen op weergeven/bewerken labels in de virtuele Machines geplaatst.

Eerst, navigeer naar een virtuele Machine tot en met de `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Als uw VM al labels bevat, ziet u vervolgens alle tags op uw resource:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Als u toevoegen van tags via PowerShell wilt, kunt u de `Set-AzureRmResource` opdracht. Houd er rekening mee bij het bijwerken van labels via PowerShell, labels worden bijgewerkt als geheel. Dus als u één tag aan een resource die beschikt over labels toevoegt, moet u alle tags die u wilt worden geplaatst op de resource opnemen. Hieronder volgt een voorbeeld van hoe u aanvullende markeringen toevoegen aan een resource via PowerShell-Cmdlets.

Deze eerste cmdlet Hiermee stelt u alle codes in de *MyTestVM* naar de variabele *$tags* geplaatst met de `Get-AzureRmResource` en `Tags` eigenschap.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

De tweede opdracht geeft de labels voor de opgegeven variabele.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

De derde opdracht wordt een extra code toegevoegd aan de variabele *$tags* . Houd rekening met het gebruik van de **+=** de nieuwe sleutel/waarde-paar toevoegen aan de lijst *$tags* .

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

De vierde opdracht stelt alle codes gedefinieerd in de variabele *$tags* aan de opgegeven resource. In dit geval is MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

De opdracht vijfde worden alle codes weergegeven op de resource. Zoals u ziet, is *locatie* nu gedefinieerd als een tag met *MyLocation* als de waarde.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Meer informatie over het taggen via PowerShell, raadpleegt u de [Cmdlets van Azure Resource][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het taggen van uw Azure resources, raadpleegt u [Azure resourcemanager overzicht][] en [Tags gebruiken om u te organiseren van uw Azure-Resources][].
* Als u wilt zien hoe tags kunt u het gebruik van Azure resources beheren, raadpleegt u [informatie over uw factuur Azure][] en [inzicht in uw verbruik van Microsoft Azure krijgen][].

[PowerShell-omgeving met Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Azure Resource-Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure resourcemanager-overzicht]: ../azure-resource-manager/resource-group-overview.md
[Markeringen gebruiken om te organiseren van uw Azure-Resources]: ../resource-group-using-tags.md
[Informatie over uw Azure factuur]: ../billing/billing-understand-your-bill.md
[Inzicht in uw verbruik van Microsoft Azure krijgen]: ../billing-usage-rate-card-overview.md
