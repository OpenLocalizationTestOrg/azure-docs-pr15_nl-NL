<properties
   pageTitle="Automatiseren van toepassingsimplementatie waarbij VM extensies | Microsoft Azure"
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

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Toepassingsimplementatie waarbij resourcemanager Azure-sjablonen

Zodra alle Azure infrastructurele vereisten zijn geïdentificeerd en omgezet in een sjabloon voor de implementatie, moet de werkelijke toepassingsimplementatie worden toegewezen. Toepassingsimplementatie hier verwijst naar de installatie van de werkelijke toepassing binaire bestanden naar Azure resources. Voor de steekproef muziek Store, .net Core en IIS moet worden geïnstalleerd en geconfigureerd op elke virtuele machine. De binaire muziek Store bestanden moeten worden geïnstalleerd op de virtuele machine en de muziek Store-database gemaakt vooraf.

In dit document een gedetailleerd overzicht van hoe VM extensies toepassingsimplementatie- en configuratie van Azure virtuele machines kunnen automatiseren. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor een optimale ervaring, vooraf een exemplaar van de oplossing aan uw Azure-abonnement en werk samen met de sjabloon Azure resourcemanager implementeren. De volledige sjabloon vindt u hier: [Muziek Store-implementatie in Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Configuratiescript

VM extensies zijn gespecialiseerde programma's die op virtuele machines voeren te leveren configuratie automatisering. Extensies zijn beschikbaar voor veel specifieke doeleinden, zoals anti-virus uitvoert, configuratie voor logboekregistratie en Docker configuratie. De extensie aangepast Script kan worden gebruikt voor een script uitvoeren op een virtuele machine. Met het voorbeeld van de muziek Store is het snel aan de aangepast script-extensie voor de Windows virtuele machines configureren en installeren van de muziek Store-servicetoepassing.

Voordat u met informatie over hoe VM extensies zijn gedefinieerd in een sjabloon Azure resourcemanager, controleert u het script die wordt uitgevoerd. Dit script configureert de virtuele Windows-computer als host voor de muziek Store-servicetoepassing. Wanneer wordt uitgevoerd, wordt het script alle nodig software, de muziek store-servicetoepassing installeren vanuit het besturingselement voor gegevensbronnen, en de database voorbereiden. 

> In dit voorbeeld is ter demonstratie.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>VM Script-extensie

VM extensies kan worden uitgevoerd ten opzichte van een virtuele machine opbouwen tegelijk door de resource extensie op te nemen in de sjabloon resourcemanager Azure. De extensie kan worden toegevoegd met de wizard Resources Visual Studio toevoegen of door een geldig JSON invoegen in de sjabloon. De resource Script extensie is genest binnen de resource VM; Dit kan worden bekeken in het volgende voorbeeld.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [VM Script extensie](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Zoals u ziet de onderstaande JSON waarop het script in GitHub zijn opgeslagen. Dit script kan ook worden opgeslagen in Azure-blobopslag. Azure resourcemanager sjablonen toestaan dat de tekenreeks script uitvoeren om te worden opgesteld zodat de sjabloon parameterwaarden kunnen worden gebruikt als parameters uitvoeren van scripts. In dit geval gegevens worden weergegeven bij het distribueren van de sjablonen en deze waarden vervolgens kunnen worden gebruikt bij het uitvoeren van het script.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Zie voor meer informatie over het gebruik van de extensie aangepast script [aangepast scriptextensies met resourcemanager sjablonen](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Volgende stap

<hr>

[Meer Azure resourcemanager sjablonen verkennen](https://github.com/Azure/azure-quickstart-templates)
