<properties
    pageTitle="Een webhook configureren op Azure activiteit Log waarschuwingen | Microsoft Azure"
    description="Lees hoe u het gebeurtenissenlogboek meldingen gebruiken om te bellen van webhooks. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Een webhook configureren op een Azure activiteit Log waarschuwingen

Webhooks kunt u een Azure waarschuwingsbericht doorsturen naar andere systemen voor na verwerking of aangepaste acties. U kunt een webhook over een waarschuwing voor het routeren deze naar de services die SMS-bericht sturen, meld u bugs bij te werken, hoogte van een team via chat/messaging services of voer een willekeurig aantal andere acties. Dit artikel wordt beschreven hoe u een webhook instellen op een melding Azure activiteit Log en hoe de nettolading voor het HTTP-bericht naar een webhook eruitziet. Voor informatie over het instellen en het schema voor een Azure metrische melding, [Zie deze pagina in plaats daarvan](insights-webhooks-alerts.md). U kunt ook een activiteit Log melding instellen om e-mail wanneer geactiveerd te verzenden.

>[AZURE.NOTE] Deze functie is momenteel in de proefversie, dus verwachten variabele kwaliteit en prestaties.

U kunt een logboek melding met de [PowerShell-Cmdlets Azure](insights-powershell-samples.md#create-alert-rules), [Platforms CLI](insights-cli-samples.md#work-with-alerts)of [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)instellen.

## <a name="authenticating-the-webhook"></a>De webhook verifiëren
TThe webhook kan verifiëren gebruikmaakt van een van de volgende manieren:

1. **Autorisatie voor token-e** - de webhook URI wordt opgeslagen met een token-ID, bijvoorbeeld. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Basisverificatie** - de webhook URI wordt opgeslagen met een gebruikersnaam en wachtwoord, bijvoorbeeld. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nettolading schema
De POST-bewerking bevat de volgende JSON nettolading en het schema voor alle activiteit Log gebaseerde waarschuwingen. Dit schema is vergelijkbaar met het account waarmee door metrisch gebaseerde waarschuwingen.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|De elementnaam van het       |Beschrijving|
|---                |---|
|status             |Wordt gebruikt voor metrische waarschuwingen. Altijd ingesteld op "geactiveerd" voor activiteit Log waarschuwingen.|
|context            |De context van de gebeurtenis.|
|resourceProviderName|De resource-provider van de risico resource.|
|conditionType      |Altijd 'gebeurtenis'.|
|naam               |De naam van de waarschuwing regel.|
|ID                 |Resource-ID van de waarschuwing.|
|Beschrijving        |Beschrijving van de waarschuwing als set tijdens het maken van de waarschuwing.|
|subscriptionId     |Azure abonnement-ID.|
|tijdstempel          |De tijd waarop de gebeurtenis is gegenereerd door de Azure-service die de aanvraag wordt verwerkt.|
|resourceId         |Resource-ID van de risico resource.|
|resourceGroupName  |Naam van de resourcegroep voor de resource risico|
|Eigenschappen         |Set `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die bevat informatie over de gebeurtenis.|
|gebeurtenis              |Element met metagegevens over de gebeurtenis.|
|autorisatie      |De RBAC-eigenschappen van de gebeurtenis. Meestal gaat hierbij om de "actie", 'rol' en het "bereik".|
|categorie           |De categorie van de gebeurtenis. Ondersteunde waarden zijn: administratieve, Waarschuw, beveiliging, ServiceHealth, aanbeveling.|
|beller             |E-mailadres van de gebruiker van wie de bewerking, UPN-claim of name claim op basis van beschikbaarheid uitgevoerd. Kan niet null zijn voor bepaalde systeem-oproepen.|
|correlationId      |Meestal een GUID op als tekenreeks. Gebeurtenissen met correlationId deel uitmaakt van dezelfde groter actie en een correlationId meestal te delen.|
|eventDescription   |Statische tekstbeschrijving van de gebeurtenis.|
|eventDataId        |Unieke id voor de gebeurtenis.|
|eventSource        |De naam van de Azure-service of de infrastructuur die de gebeurtenis gegenereerd.|
|httpRequest        |Bevat meestal de "clientRequestId", "clientIpAddress" en 'methode' (bijvoorbeeld HTTP-methode plaatsen).|
|niveau              |Een van de volgende waarden: 'kritieke","Fout","Waarschuwing", 'Ter informatie' en"Uitgebreid."|
|Bewerkingsnummer        |Meestal een GUID gedeeld door de gebeurtenissen die overeenkomt met één bewerking.|
|operationName      |De naam van de bewerking.|
|Eigenschappen         |Eigenschappen van de gebeurtenis.|
|status             |Tekenreeks. De status van de bewerking. Algemene waarden zijn: "De slag", 'In uitvoering', 'Geslaagd', "Is mislukt", "Actieve", "Opgelost".|
|subStatus          |Meestal bevat de HTTP-statuscode van het bijbehorende REST-gesprek. Deze kan ook andere tekenreeksen met een beschrijving van een substatus bevatten. Algemene substatus zijn onder andere: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), niet inhoud (HTTP-statuscode: 204), ongeldige aanvragen (HTTP-statuscode: 400), niet gevonden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), time-out van de Gateway (HTTP-statuscode: 504)|

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het logboek](monitoring-overview-activity-logs.md)
- [Azure automatisering scripts (Runbooks) op Azure waarschuwingen uitvoeren](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Logica-App gebruiken voor het verzenden van een SMS-bericht via Twilio uit een Azure melding](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar als u wilt werken met een melding voor een activiteit Log kan worden gewijzigd.
- [Logica-App gebruiken om een vertraging bericht uit een Azure melding te verzenden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar als u wilt werken met een melding voor een activiteit Log kan worden gewijzigd.
- [Gebruik logica App een bericht verzenden naar een wachtrij Azure uit een Azure melding](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In dit voorbeeld is voor metrische waarschuwingen, maar als u wilt werken met een melding voor een activiteit Log kan worden gewijzigd.
