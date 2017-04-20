<properties
   pageTitle="Implementatie bewerkingen met REST API weergeven | Microsoft Azure"
   description="Wordt beschreven hoe de Azure resourcemanager REST API gebruiken om te bepalen problemen uit resourcemanager-implementatie."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Implementatie bewerkingen weergeven met Azure resourcemanager REST API

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

Als u een fout ontvangen hebt wanneer u resources implementatie in Azure, wilt u mogelijk meer details over de implementatie-bewerkingen die zijn uitgevoerd. De REST API biedt bewerkingen waarmee u kunt zoeken naar de fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

U kunt enkele fouten voorkomen door het valideren van uw sjabloon en de infrastructuur vóór implementatie. U kunt ook extra aanvraag en antwoord informatie tijdens de implementatie die mogelijk nuttig later voor probleemoplossing vastleggen. Zie voor meer informatie over valideren en vergaderverzoeken en antwoorden logboekinformatie, [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Problemen met REST API

1. Implementeren uw resources met de bewerking voor [de sjabloonimplementatie van een maken](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Om te behouden informatie die mogelijk nuttig zijn voor foutopsporing, stelt u de eigenschap **debugSetting** in JSON-aanvraag voor **requestContent** en/of **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
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
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    De waarde **debugSetting** is standaard ingesteld op **geen**. Wanneer de waarde **debugSetting** , Overweeg zorgvuldig het type gegevens dat u tijdens de implementatie doorgeeft. Door logboekregistratie informatie over de aanvraag of antwoord waarbij u potentieel gevoelige gegevens die zijn opgehaald door de implementatie-bewerkingen. 

2. Informatie ophalen over een-implementatie waarbij de bewerking voor [informatie over de sjabloonimplementatie van een](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Opmerking met name de elementen **provisioningState** , **correlationId** en **fout** geantwoord. De **correlationId** wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en is handig wanneer u werkt met technische ondersteuning van een probleem kunt oplossen.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Lees informatie over de implementatie-bewerkingen met de [lijst van alle bewerkingen van de sjabloon implementatie](https://msdn.microsoft.com/library/azure/dn790518.aspx) -bewerking. 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Het antwoord aanvraag en/of antwoord informatie op basis van wat u hebt opgegeven in de eigenschap **debugSetting** tijdens de implementatie opgenomen.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Gebeurtenissen krijgen van de controlelogboeken voor de implementatie met de bewerking [lijst management gebeurtenissen in een abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Volgende stappen

- Zie [oplossen van veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van implementatiefouten bepaalde.
- Voor meer informatie over het gebruik van de controlelogboeken om de andere soorten acties te houden, raadpleegt u [bewerkingen met resourcemanager controleren](resource-group-audit.md).
- Zie [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy.md)valideren van uw implementatie voordat deze wordt uitgevoerd.
