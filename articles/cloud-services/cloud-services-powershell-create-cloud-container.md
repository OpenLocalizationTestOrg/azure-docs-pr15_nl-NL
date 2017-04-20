<properties
   pageTitle="Een container cloud-service maken met PowerShell | Microsoft Azure"
   description="In dit artikel wordt uitgelegd hoe u een container cloud-service met PowerShell maakt. De container host web en werknemer rollen."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Gebruik een Azure PowerShell-opdracht om een lege cloud servicecontainer maken
In dit artikel wordt uitgelegd hoe u snel een Cloud Services-container met Azure PowerShell-cmdlets te maken. Volg de onderstaande stappen:

1. Installeer het Microsoft Azure PowerShell-cmdlet vanaf de downloadpagina van [Azure PowerShell](http://aka.ms/webpi-azps) .
2. Open de opdrachtprompt PowerShell.
3. Gebruik de [Toevoegen-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) aan te melden.

    > [AZURE.NOTE] Ga naar [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor verdere instructies over het installeren van de Azure PowerShell-cmdlet en verbinding maakt met uw Azure-abonnement.

4. Gebruik de cmdlet **New-AzureService** om het maken van een container lege Azure cloud-service.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. In dit voorbeeld om aan te roepen de cmdlet als volgt:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Voor meer informatie over het maken van de Azure cloudservice uitvoeren:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Volgende stappen

 * Als u wilt de cloud-service-implementatie beheren, raadpleegt u de opdrachten [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx) [Verwijderen AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)en [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) . U kunt ook verwijzen naar [cloudservices configureren](cloud-services-how-to-configure.md) voor meer informatie.

 * Als u uw project cloud-service wilt Azure publiceren, Ga naar het voorbeeld **PublishCloudService.ps1** van [continue bezorging voor cloudservice in Azure wordt aangegeven](cloud-services-dotnet-continuous-delivery.md).
