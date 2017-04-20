<properties
   pageTitle="Implementatie bewerkingen met Azure CLI weergeven | Microsoft Azure"
   description="Wordt beschreven hoe u met de CLI Azure problemen uit resourcemanager implementatie opsporen."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Implementatie-bewerkingen met Azure CLI weergeven

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

Als u een fout ontvangen hebt wanneer u resources implementatie in Azure, wilt u mogelijk meer details over de implementatie-bewerkingen die zijn uitgevoerd. Azure CLI biedt opdrachten waarmee u kunt vinden van de fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

U kunt enkele fouten voorkomen door het valideren van uw sjabloon en de infrastructuur voor implementatie. U kunt ook extra aanvraag en antwoord informatie tijdens de implementatie die mogelijk nuttig later voor probleemoplossing vastleggen. Zie voor meer informatie over valideren en vergaderverzoeken en antwoorden logboekinformatie, [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Controlelogboeken gebruiken om op te lossen

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Als u wilt zien fouten voor een implementatie, gebruik de volgende stappen uit:

1. Als u wilt de controlelogboeken wordt weergegeven, voert u de opdracht **azure groep log weergeven** . Kunt u ook de **--laatste-implementatie** optie om op te halen alleen het logboek voor de meest recente implementatie.

        azure group log show ExampleGroup --last-deployment

2. De opdracht **azure groep log show** geeft als resultaat een groot aantal gegevens. Voor probleemoplossing wilt u meestal richten op bewerkingen die is mislukt. Het volgende script wordt de optie **--json** en het [jq](https://stedolan.github.io/jq/) JSON-hulpprogramma naar het logboek voor implementatie fouten zoeken.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    U kunt zien **Eigenschappen** bevat informatie in json over de mislukte bewerking.

    U kunt de **--uitgebreide** en **- vv** opties voor meer informatie uit de logboeken.  Gebruik de **--uitgebreide** optie voor het weergeven van de stappen die de bewerkingen leest u over op `stdout`. Gebruik voor de geschiedenis van een volledige aanvraag, de optie **- vv** . De berichten bevatten vaak belangrijke informatie over de oorzaak van eventuele fouten.

3. Om u te richten op het statusbericht voor mislukte posten, gebruikt u de volgende opdracht:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Implementatie bewerkingen gebruiken om op te lossen

1. Krijgen de algemene status van een distributie met de opdracht **implementatie van azure groep weergeven** . In het onderstaande voorbeeld worden de implementatie is mislukt.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Als u wilt zien van het bericht op mislukte bewerkingen voor een implementatie, gebruiken:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Volgende stappen

- Zie [oplossen van veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van implementatiefouten bepaalde.
- Voor meer informatie over het gebruik van de controlelogboeken om de andere soorten acties te houden, raadpleegt u [bewerkingen met resourcemanager controleren](resource-group-audit.md).
- Zie [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy.md)valideren van uw implementatie voordat deze wordt uitgevoerd.
