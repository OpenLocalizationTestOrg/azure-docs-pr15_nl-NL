<properties
   pageTitle="Een Internet die tegenover elkaar liggen taakverdeling in resourcemanager met een sjabloon maken | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding van taakverdeling in resourcemanager met een sjabloon"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Een internetverbinding van taakverdeling met een sjabloon maken

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel worden de resourcemanager implementatiemodel. U kunt ook [informatie over het maken van een die tegenover elkaar liggen taakverdeling klassieke implementatiemodel met Internet](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met behulp van Klik om te implementeren

De voorbeeldsjabloon die beschikbaar zijn in de openbare opslagplaats gebruikt een parameterbestand met de standaard waarden gebruikt voor het genereren van het scenario hierboven is beschreven. Als u wilt deze sjabloon met klik te implementeren, volgt u [deze koppeling](http://go.microsoft.com/fwlink/?LinkId=544801)implementeert, klik op **Deploy naar Azure**, vervangt u de waarden van de parameter standaard zo nodig en volg de instructies in de portal.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren via PowerShell

Als u wilt implementeren van de sjabloon die u hebt gedownload via PowerShell, de onderstaande stappen uit te voeren.

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde aan te melden bij Azure en selecteert u uw abonnement.

2. Voer de cmdlet **New-AzureRmResourceGroupDeployment** als u wilt maken van een resourcegroep met de sjabloon.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon met behulp van de Azure CLI implementeren

Als u wilt implementeren de sjabloon met behulp van de Azure CLI, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
2. Voer de opdracht **azure config modus** om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Vanuit uw browser, Ga naar [De sjabloon Quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), de inhoud van de json-bestand kopiëren en plakken in een nieuw bestand in uw computer. In dit scenario zou u de waarden onder kopiëren naar een bestand met de naam **c:\lb\azuredeploy.parameters.json**.
4. Voer de cmdlet **implementatie van azure groep maken** als u wilt implementeren van de nieuwe taakverdeling met behulp van de sjabloon en parameter bestanden die u hebt gedownload en gewijzigd hierboven. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
