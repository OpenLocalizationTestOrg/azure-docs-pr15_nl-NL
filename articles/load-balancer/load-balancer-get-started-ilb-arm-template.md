<properties
   pageTitle="Maken van een interne taakverdeling met een sjabloon in resourcemanager | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met een sjabloon in resourcemanager"
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

# <a name="create-an-internal-load-balancer-using-a-template"></a>Een interne taakverdeling met een sjabloon maken

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met behulp van Klik om te implementeren

De voorbeeldsjabloon die beschikbaar zijn in de openbare opslagplaats gebruikt een parameterbestand met de standaard waarden gebruikt voor het genereren van het scenario hierboven is beschreven. Als u wilt deze sjabloon met klik te implementeren, volgt u [deze koppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)implementeert, klik op **Deploy naar Azure**, vervangt u de waarden van de parameter standaard zo nodig en volg de instructies in de portal.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren via PowerShell

Als u wilt implementeren van de sjabloon die u hebt gedownload via PowerShell, de onderstaande stappen uit te voeren.

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../../articles/powershell-install-configure.md) en volg de instructies helemaal naar het einde aan te melden bij Azure en selecteert u uw abonnement.
2. Het parameterbestand downloaden naar uw lokale schijf.
3. Het bestand bewerken en opslaan.
4. Voer de cmdlet **New-AzureRmResourceGroupDeployment** als u wilt maken van een resourcegroep met de sjabloon.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon met behulp van de Azure CLI implementeren

Als u wilt implementeren de sjabloon met behulp van de Azure CLI, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
2. Voer de opdracht **azure config modus** om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Open het parameterbestand, selecteert u de inhoud ervan en sla deze op een bestand in uw computer. In dit voorbeeld wordt de parameterbestand hebt opgeslagen naar *parameters.json*.

4. De opdracht **implementatie van azure groep maken** om te implementeren van de nieuwe interne taakverdeling met behulp van de sjabloon en parameter bestanden die u hebt gedownload en gewijzigd hierboven uitvoeren. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Volgende stappen

[Een laden gelijkmatige verdeling modus bron IP-affiniteit met configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)



