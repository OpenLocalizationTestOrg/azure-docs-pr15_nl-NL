<properties
    pageTitle="Automatisch schalen acties gebruiken voor het verzenden van e-mail en webhook waarschuwingen. | Microsoft Azure"
    description="Lees hoe u automatisch schalen acties gebruiken voor het web-URL's belt of het verzenden van e-mailmeldingen in Azure Monitor. "
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Automatisch schalen acties gebruiken voor het verzenden van e-mail en webhook-meldingen in Azure Monitor

In dit artikel leest u hoe triggers zo instellen dat u kunt specifieke web-URL's belt of het e-mailberichten op basis van automatisch schalen acties in Azure verzenden.  

## <a name="webhooks"></a>Webhooks
Webhooks kunt u de Azure waarschuwingen doorsturen naar andere systemen voor na verwerking of aangepaste meldingen. De melding bijvoorbeeld zoekresultaten omleiden naar een services dat een binnenkomende webaanvraag om te verzenden dat SMS, log bugs bij te werken, hoogte een team chat gebruiken of messaging services kunnen worden verwerkt, enzovoort. De webhook URI moet een geldige HTTP of HTTPS-eindpunt.

## <a name="email"></a>E-mail
E-mail kan worden verzonden naar een geldig e-mailadres. Beheerders en CO-beheerders van het abonnement waarop de regel wordt uitgevoerd ook krijgt.


## <a name="cloud-services-and-web-apps"></a>Cloudservices- en WebApps
U kunt aanmelden via de Azure-portal voor Cloudservices en Server-Farms (Web-Apps).

- Kies de meetwaarde **schaal door** .

![met wilt verkleinen](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>VM schaal sets
Voor nieuwere virtuele Machines die zijn gemaakt met resourcemanager (virtuele Machine schaal sets), kunt u dit met REST API, resourcemanager sjablonen, PowerShell en CLI. Een portal interface is nog niet beschikbaar.
Wanneer de REST API of resourcemanager sjabloon gebruikt, voegt u het element meldingen met de volgende opties.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Veld                              |Verplicht? |Beschrijving|
|---                                |---        |---|
|bewerking                          |Ja        |waarde moet "Schaal"|
|sendToSubscriptionAdministrator    |Ja        |waarde moet liggen "true" of "false"|
|sendToSubscriptionCoAdministrators |Ja        |waarde moet liggen "true" of "false"|
|customEmails                       |Ja        |waarde is null [] of tekenreeksmatrix van e-mailberichten|
|webhooks                           |Ja        |waarden zijn null of geldige Uri|
|serviceUri                         |Ja        |een geldig https Uri|
|Eigenschappen                         |Ja        |waarde moet lege {} of sleutel-waardeparen kan bevatten|


## <a name="authentication-in-webhooks"></a>Verificatie in webhooks
Er zijn twee verificatie URI formulieren:

1. Token-base authenticatie, waarop u de webhook URI opslaan met een token-ID die als een queryparameter. Bijvoorbeeld https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Basisverificatie, waar u een gebruikers-ID en wachtwoord gebruiken. Bijvoorbeeld:https://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatisch schalen melding webhook nettolading schema
Wanneer de melding automatisch schalen wordt gegenereerd, wordt de volgende metagegevens is opgenomen in de nettolading webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Veld  |Verplicht?|    Beschrijving|
|---|---|---|
|status |Ja    |De status die aangeeft dat er een actie automatisch schalen is gegenereerd|
|bewerking| Ja |Voor een toename van de exemplaren, dient "Schaal Out" en voor een toename van de exemplaren, is 'Schaal In'|
|context|   Ja |De context van de actie automatisch schalen|
|tijdstempel| Ja |Tijdstempel als de actie automatisch schalen is geactiveerd|
|ID |Ja|   Resourcemanager-ID van de instelling automatisch schalen|
|naam   |Ja|   De naam van de instelling automatisch schalen|
|meer informatie|   Ja |Uitleg van de actie die de service automatisch schalen duurde en deze wijziging in het exemplaar tellen|
|subscriptionId|    Ja |Abonnements-ID van de doel-resource die is wordt vergroot|
|resourceGroupName| Ja|    Resourcegroep naam van de doel-resource die is wordt vergroot|
|Bronnaam   |Ja|   Naam van de doel-resource die is wordt vergroot|
|Brontype   |Ja|   De drie ondersteunde waarden: 'microsoft.classiccompute/domainnames/slots/roles' - Cloudservice rollen, "microsoft.compute/virtualmachinescalesets" - VM schaal Sets en "Microsoft.Web/serverfarms" - Web App|
|resourceId |Ja|Resourcemanager-ID van de doel-resource die is wordt vergroot|
|portalLink |Ja    |Azure portal koppeling naar de pagina overzicht van de resource doel|
|oldCapacity|   Ja |De huidige (oude) exemplaar telling wanneer een actie schaal hebt gemaakt, automatisch schalen|
|newCapacity|   Ja |De nieuwe exemplaar tellen die automatisch schalen de resource tot schaal|
|Eigenschappen|    Nee| Optioneel. Set < toets, waarde > paren (bijvoorbeeld woordenlijst < tekenreeks, tekenreeks >). Het veld eigenschappen is optioneel. U kunt in een aangepaste gebruikersinterface of de werkstroom voor het app op basis van logica, sleutels en waarden die kunnen worden doorgegeven met de nettolading invoeren. Een andere methode voor het doorgeven van aangepaste eigenschappen terug naar de uitgaande oproep voor webhook is het gebruik van de webhook URI zelf (als queryparameters)|
