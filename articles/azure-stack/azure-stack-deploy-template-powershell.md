<properties
    pageTitle="Sjablonen met PowerShell Azure gestapelde implementeren | Microsoft Azure"
    description="Leer hoe u een virtuele machine met een sjabloon resourcemanager en PowerShell implementeren."
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
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Sjablonen in Azure stapel via PowerShell implementeren

PowerShell gebruiken om te implementeren van Azure resourcemanager sjablonen op de Azure stapel Haalbaarheidstest.  Resourcemanager sjablonen implementeren en inrichten van alle resources voor uw toepassing een eenmalige, gecoördineerde betrekking heeft.

## <a name="run-azurerm-powershell-cmdlets"></a>Voer AzureRM PowerShell-cmdlets

In dit voorbeeld u een script uitvoeren om een virtuele machine implementeren naar Azure stapel Haalbaarheidstest met een sjabloon resourcemanager.  Zorgen dat u hebt [geïnstalleerd en geconfigureerd PowerShell](azure-stack-connect-powershell.md) voordat u verder gaat,  

De VHD gebruikt in deze voorbeeldsjabloon voor een is een marketplace standaardafbeelding (WindowsServer-2012-R2-Datacenter).

1.  Ga naar <http://aka.ms/AzureStackGitHub>, zoekt de sjabloon **101-eenvoudige-windows-vm** en sla deze op de volgende locatie: c:\\sjablonen\\azuredeploy-101-eenvoudige-windows-vm.json.

2.  Voer de volgende implementatiescript in PowerShell. Vervang de *gebruikersnaam* en *wachtwoord* door uw gebruikersnaam en wachtwoord. Klik op volgende wordt gebruikt, verhoogd de waarde voor de parameter *$myNum* om te voorkomen dat uw implementatie overschrijven.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Open de portal Azure stapel, klikt u op **Bladeren**en **virtuele machines**klikt u op zoek naar uw nieuwe virtuele machine (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Voorbeeld van de video: hybride implementatie van virtuele machines

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Volgende stappen

[Sjablonen met Visual Studio implementeren](azure-stack-deploy-template-visual-studio.md)
