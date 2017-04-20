<properties
   pageTitle="Resources met REST API en sjabloon implementeren | Microsoft Azure"
   description="Azure resourcemanager en resourcemanager REST API gebruiken om te implementeren van een resources naar Azure. De resources worden gedefinieerd in een sjabloon resourcemanager."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Resources met resourcemanager sjablonen en resourcemanager REST API implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

In dit artikel wordt uitgelegd hoe u de Resource Manager REST API met resourcemanager sjablonen met uw resources implementeren naar Azure.  

> [AZURE.TIP] Voor hulp bij het oplossen van een fout fouten tijdens de implementatie, raadpleegt u:
>
> - [Implementatie bewerkingen weergeven met REST API](resource-manager-troubleshoot-deployments-rest.md) voor meer informatie over het aanvragen van informatie waarmee u oplossen de fout
> - [Problemen oplossen met veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten in

De sjabloon kan een lokaal bestand of een extern bestand die beschikbaar zijn via een URI zijn. Wanneer uw sjabloon bevindt zich in een opslag-account, kunt u het beperken van toegang aan de sjabloon en een gedeelde toegangstoken handtekening (SA's) tijdens implementatie verstrekken.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Met de REST API implementeren
1. [Algemene parameters en kopteksten](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), inclusief verificatietokens instellen.
2. Als u een bestaande resourcegroep niet hebt, kunt u een resourcegroep maken. Geef uw abonnements-id, de naam van de nieuwe resourcegroep, en de locatie die u nodig hebt voor uw oplossing. Zie [een resourcegroep maken](https://msdn.microsoft.com/library/azure/dn790525.aspx)voor meer informatie.

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Valideer de implementatie voordat deze wordt uitgevoerd door de bewerking [valideren de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790547.aspx) uit te voeren. Wanneer u test de implementatie, bieden u parameters exact in zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

3. Maak een implementatie. Geef uw abonnements-id, de naam van de resourcegroep om te implementeren, de naam van de implementatie en een koppeling aan uw sjabloon. Zie voor informatie over het sjabloonbestand [Parameter-bestand](#parameter-file). Zie voor meer informatie over de REST API maken van een resourcegroep [maken de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790564.aspx). Zoals u ziet dat de **modus** is ingesteld op **incrementele**. Als u wilt uitvoeren op een volledige implementatie, stel **modus** op **voltooid**. Wees voorzichtig met het gebruik van de volledige modus kunt u per ongeluk bronnen die zich niet in uw sjabloon verwijderen.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Desgewenst kunt u aan te melden antwoord inhoud wordt **debugSetting** opnemen in het verzoek van inhoud van de aanvraag (of beide).

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      U kunt uw opslag-account instellen voor het gebruik van een gedeelde toegangstoken handtekening (SA's). Zie [Delegeren van Access met een handtekening gedeeld Access](https://msdn.microsoft.com/library/ee395415.aspx)voor meer informatie.

4. De status van de sjabloonimplementatie ophalen. Zie [informatie over de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790565.aspx)voor meer informatie.

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Volgende stappen
- Zie [Deploy resources gebruik .NET-bibliotheken en een sjabloon](virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de bibliotheek .NET-client.
- Als u wilt definiëren parameters in een sjabloon, raadpleegt u [ontwerpfuncties sjablonen](resource-group-authoring-templates.md#parameters).
- Zie voor hulp bij uw-oplossing implementeert in verschillende omgevingen, [ontwikkeling en testomgevingen in Microsoft Azure wordt aangegeven](solution-dev-test-environments.md).
- Voor meer informatie over het gebruik van een verwijzing KeyVault voor geven secure waarden, raadpleegt u [geven secure waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).
