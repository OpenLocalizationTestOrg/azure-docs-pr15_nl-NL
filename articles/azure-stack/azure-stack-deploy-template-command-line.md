<properties
    pageTitle="Sjablonen met de opdrachtregel Azure gestapelde implementeren | Microsoft Azure"
    description="Leer hoe u de platforms opdrachtregelinterface () gebruiken om te implementeren van sjablonen van binnen de ClientVM of na het gebruik van de VPN verbinding maken met Azure stapel."
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
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Sjablonen in Azure stapel via de opdrachtregel implementeren

Gebruik de opdracht Azure resourcemanager sjablonen naar de Azure stapel Haalbaarheidstest implementeren. Azure resourcemanager sjablonen implementeren en inrichten van alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft.

## <a name="download-template"></a>Inleiding tot besturingselementen        
Als u wilt testen een-implementatie waarbij de CLI, moet u de bestanden azuredeploy.json en azuredeploy.parameters.json downloaden uit de [opslagruimte account voorbeeldsjabloon maken](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Sjabloon implementeren
Navigeer naar de map waar deze bestanden zijn gedownload en voer de volgende opdracht om te implementeren van de sjabloon:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Deze opdracht implementeren de sjabloon in de resource groep **cliRG** op de Azure stapel Haalbaarheidstest standaardlocatie opgeslagen.

## <a name="validate-template-deployment"></a>Valideer de sjabloonimplementatie
Als u wilt zien van deze resource groeperen en opslag-account, kunt u de volgende opdrachten gebruiken:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Volgende stappen

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
