<properties
   pageTitle="Resourcemanager REST API's | Microsoft Azure"
   description="Een overzicht van de Resource Manager REST API's verificatie- en gebruiksgegevens voorbeelden"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Resourcemanager REST API 's

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [REST API](resource-manager-rest-api.md)

Achter elke oproep naar Azure resourcemanager achter elke sjabloon geïmplementeerd achter het account van elke geconfigureerde opslag is er een of meer aanroepen van de Azure Resource Manager RESTful API. In dit onderwerp is gewijd aan deze API's en hoe u ze zonder eventuele SDK helemaal kunt bellen. Dit is bijzonder nuttig zijn als u wilt dat volledig beheer van alle aanvragen voor Azure of als de SDK voor de voorkeurstaal niet beschikbaar is of biedt geen ondersteuning voor de bewerkingen die u wilt uitvoeren.

In dit artikel wordt niet leest u over elke API dat wordt weergegeven in Azure wordt aangegeven, maar sommige als een voorbeeld van hoe u verdergaan en verbinding maken met deze liever wordt gebruikt. Als u de basisbeginselen kunt u vervolgens verdergaan en de [Azure resourcemanager REST API Reference](https://msdn.microsoft.com/library/azure/dn790568.aspx) gedetailleerde informatie over het gebruik van de rest van de API's lezen.

## <a name="authentication"></a>Verificatie
Verificatie voor ARM wordt verwerkt door Azure AD (Active Directory). De verbinding met een API u eerst moet om te verifiëren met Azure AD voor het ontvangen van een verificatietoken die u kunt doorgeven aan elk verzoek. Terwijl we zijn met een beschrijving van een puur gesprek rechtstreeks naar de REST API's, wordt ook ervan uitgegaan dat u niet wilt verifiëren met een wachtwoord normale gebruikersnaam waar een pop-van-scherm mogelijk wordt u gevraagd voor de gebruikersnaam en wachtwoord en misschien zelfs andere verificatiemethoden gebruikt in twee factor verificatie-scenario's. We gaan daarom maken zogenaamd een Azure AD-toepassing en de hoofdsom van een Service die wordt gebruikt om aan te melden met. Maar denk eraan dat Azure AD ondersteunt verschillende verificatie procedures en al deze kan worden gebruikt om op te halen die verificatietoken die we nodig voor de volgende API aanvragen.
Volg [maken Azure AD-toepassing en Service principe](./resource-group-create-service-principal-portal.md) voor stapsgewijze instructies.

### <a name="generating-an-access-token"></a>Een Access-Token genereren 
Azure AD verificatie wordt uitgevoerd door het bellen naar Azure AD login.microsoftonline.com zich bevindt. Om te verifiëren dat u moeten beschikken over de volgende informatie:

* Azure AD-Tenant-ID (de naam van die Azure AD die u gebruikt voor het aanmelden, vaak hetzelfde als uw bedrijf, maar niet nodig)
* Toepassings-ID (die u hebt gemaakt tijdens de stap Azure AD-toepassing maken)
* Wachtwoord (die u hebt geselecteerd bij het maken van de Azure AD-toepassing)

In de onderstaande HTTP-aanvraag Zorg ervoor dat "Azure AD-Tenant-ID", "Toepassings-ID" en 'Wachtwoord' vervangen door de juiste waarden.

**Algemene HTTP-aanvraag:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... is (als de verificatie is geslaagd) leidt tot een soortgelijke antwoord als volgt:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(De access_token in de bovenstaande reactie is verkort om de leesbaarheid te verbeteren)

**Genereren toegangstoken gebruiken we vaker doen:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Genereren toegangstoken via PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Het antwoord bevat een Access-Token, informatie over hoe lang deze token geldig is en informatie over welke resource kunt u deze token voor.
Het toegangstoken die u hebt ontvangen in de vorige HTTP-oproep moet worden doorgegeven voor alle verzoek tot de API ARM als een koptekst met de naam 'Autorisatie' met de waarde "Vruchtdragende YOUR_ACCESS_TOKEN". U ziet u de afstand tussen "Vruchtdragende" en uw Access-Token.

Zoals u van de bovenstaande HTTP-resultaat ziet, geldt het token voor een bepaalde periode waarin u cache kunnen opslaan en opnieuw gebruiken die dezelfde token. Zelfs als het verifiëren van Azure AD voor elke API-oproep mogelijk is, is het worden zeer efficiënt verloopt.

## <a name="calling-arm-rest-apis"></a>Om te bellen kwijt ARM REST API 's

[Azure resourcemanager REST API's hier worden beschreven](https://msdn.microsoft.com/library/azure/dn790568.aspx) en deze bevindt zich buiten het bereik voor deze zelfstudie naar het gebruik van elke en elk document. Deze documentatie gebruik uitleg over de basisgebruik alleen een paar API's van de API's en na die we u aan de officiële documentatie verwijzen.

### <a name="list-all-subscriptions"></a>Lijst van alle abonnementen

Een van de eenvoudigste bewerkingen die u kunt doen is voor een overzicht van de beschikbare abonnementen waartoe u toegang hebt. In de onderstaande aanvraag kunt u zien hoe de Access-Token wordt doorgegeven als koptekst.

(YOUR_ACCESS_TOKEN vervangen door uw werkelijke Access Token.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... en daardoor, krijgt u een lijst met abonnementen die deze Service Principal is toegestaan voor toegang tot

(Abonnement-id's hieronder is verkort voor leesbaarheid)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Een lijst met alle resourcegroepen in een bepaald abonnement

Alle bronnen die beschikbaar zijn met de ARM-API's worden genest binnen een resourcegroep. We gaan query ARM voor bestaande resourcegroepen aan onze abonnement met de onder HTTP GET-aanvraag. Zoals u ziet hoe de abonnements-ID wordt doorgegeven als onderdeel van de URL ditmaal.

(YOUR_ACCESS_TOKEN en SUBSCRIPTION_ID vervangen door uw werkelijke Access Token en abonnements-ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Het antwoord dat u krijgt is afhankelijk van of u een resourcegroepen gedefinieerd hebt en zo ja, hoeveel.

(Abonnement-id's hieronder is verkort voor leesbaarheid)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Dusverre we hebt alleen zijn bij het controleren van de ARM API's voor informatie, kunt u tijd we enkele bronnen in plaats daarvan maken en we gaan eerst de eenvoudigste ze allemaal, een resourcegroep. De volgende HTTP-aanvraag Hiermee maakt u een nieuwe resourcegroep in een land/locatie van uw keuze en voegt u een of meer tags toe aan deze (een code in het voorbeeld hieronder slechts toevoegen).

(YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME vervangen door uw werkelijke Access Token, abonnements-ID en naam van de resourcegroep die u wilt maken)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Als dit lukt, krijgt u een soortgelijke antwoord op deze

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

U hebt een resourcegroep gemaakt in Azure wordt aangegeven. Gefeliciteerd!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Resources implementeren naar een resourcegroep die met een sjabloon van ARM

Met ARM, kunt u uw resources met ARM sjablonen implementeren. Een sjabloon ARM Hiermee definieert u verschillende bronnen en bijbehorende afhankelijkheden. Voor deze sectie we wordt alleen wordt ervan uitgegaan dat u bekend bent met ARM sjablonen en we zojuist ziet u hoe u de oproep door API naar het begin van de implementatie van een. Een gedetailleerde documentatie van ARM sjablonen vindt u hier.

Implementatie van een sjabloon ARM verschillen niet veel hoe u andere API's bellen. Een belangrijk aspect is dat implementatie van een sjabloon heel lang duren kunt, afhankelijk van wat zich in de sjabloon is en de API-aanroep NET retourneert en is het voor u als ontwikkelaar tot query voor de status van de implementatie om te weten wanneer de implementatie is voltooid.

In dit voorbeeld gebruiken we een vrij toegankelijke ARM sjabloon beschikbaar op [GitHub](https://github.com/Azure/azure-quickstart-templates). De sjabloon gaan we gebruiken wordt een VM Linux dashboard implementeren naar het gebied West VS. Hoewel deze sjabloon de sjabloon die beschikbaar in een openbare opslagplaats zoals GitHub zijn wordt, kunt u ook selecteren door te geven van de volledige sjabloon als onderdeel van de aanvraag kunt invullen. Houd er rekening mee dat we parameterwaarden opgeven als onderdeel van de aanvraag die wordt gebruikt in de gebruikte sjabloon.

(SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD en DNS_NAME_FOR_PUBLIC_IP geschikt te maken voor uw aanvraag kunt invullen waarden vervangen)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Het behoorlijk lang JSON-antwoord voor deze aanvraag ter verbetering van de leesbaarheid van deze documentatie zijn weggelaten. Het antwoord bevat informatie over de implementatie van de sjablonen die u zojuist hebt gemaakt.

