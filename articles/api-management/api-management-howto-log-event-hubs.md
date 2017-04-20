<properties 
    pageTitle="Hoe u aan te melden gebeurtenissen Azure gebeurtenis Hubs in Azure API Management | Microsoft Azure" 
    description="Leer hoe u gebeurtenissen met Azure gebeurtenis Hubs in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Hoe u aan te melden gebeurtenissen Azure gebeurtenis Hubs in Azure API Management

Azure gebeurtenis Hubs is een zeer scalable ingress gegevensservice die miljoenen gebeurtenissen per seconde nemen kunt zodat u kunt verwerken en analyseren van de enorme hoeveelheid gegevens die zijn gemaakt met uw verbonden apparaten en toepassingen. Gebeurtenis Hubs fungeert als de "deur' voor een gebeurtenis pijplijn en nadat de gegevens worden verzameld in de hub van een gebeurtenis, deze kan worden omgezet en opgeslagen met elke realtime analytics-provider of batchen/opslag adapters. Gebeurtenis Hubs decouples de hoeveelheid een reeks gebeurtenissen van het verbruik van gebeurtenissen zodat consumenten gebeurtenis toegang hebt tot de gebeurtenissen op hun eigen planning.

In dit artikel is een aanvulling op de video [Azure API-Management integreren met gebeurtenis Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) en wordt beschreven hoe u API Management gebeurtenissen met Azure gebeurtenis Hubs.

## <a name="create-an-azure-event-hub"></a>Een Hub Azure gebeurtenis maken

Als u wilt maken van een nieuwe gebeurtenis-Hub, aanmelden bij de [portal van Azure klassieke](https://manage.windowsazure.com) en klikt u op **Nieuw**->**App Services**->**Service Bus**->**Gebeurtenis Hub**->**Snelle maken**. Voer de naam van een gebeurtenis-Hub, regio, selecteer een abonnement en selecteer een naamruimte. Als u een naamruimte nog niet eerder hebt gemaakt, kunt u een door een naam te typen in het tekstvak **Namespace** kunt maken. Nadat alle eigenschappen zijn geconfigureerd, klikt u op **een nieuwe gebeurtenis Hub maken** als u wilt maken van de Hub gebeurtenis.

![Hub gebeurtenis maken][create-event-hub]

Vervolgens Ga naar het tabblad **configureren** voor uw nieuwe gebeurtenis Hub en twee **gedeelde-beleid**maken. Naam van de eerste afbeelding **verzenden** en geef deze machtigingen **verzenden** .

![Beleid verzenden][sending-policy]

Naam van de tweede **ontvangen**, hieraan **beluisteren** machtigingen en klik op **Opslaan**.

![Beleid ontvangen][receiving-policy]

Elk beleid gedeelde toegang geeft toepassingen verzenden en ontvangen van gebeurtenissen naar en vanuit de Hub gebeurtenis. Voor toegang tot de verbindingstekenreeksen voor dit beleid, Ga naar het tabblad **Dashboard** van de gebeurtenis Hub en **informatie over de databaseverbinding**op.

![Verbindingsreeks][event-hub-dashboard]

De verbindingsreeks **verzenden** wordt gebruikt tijdens de registratie van gebeurtenissen en de verbindingsreeks **ontvangen** wordt gebruikt bij het downloaden van gebeurtenissen van de Hub gebeurtenis.

![Verbindingsreeks][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Een logboek API Management maken

Nu dat u een gebeurtenis-Hub hebt, is de volgende stap een [logboek](https://msdn.microsoft.com/library/azure/mt592020.aspx) in uw API-beheerservice zodanig configureren dat deze gebeurtenissen in de Hub gebeurtenis vastleggen kunt.

API Management vastleggen zijn geconfigureerd met de [API Management REST API](http://aka.ms/smapi). Voordat u de REST API voor de eerste keer gebruikt, Controleer de [vereisten](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) en zorg ervoor dat u [toegang tot de REST API ingeschakeld hebt](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Als u wilt een logboek maken, een aanvraag HTTP plaatsen met de volgende URL-sjabloon maken

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Vervang `{your service}` met de naam van uw exemplaar van de service API Management.
-   Vervang `{new logger name}` met de gewenste naam voor uw nieuwe logboek. U verwijst naar deze naam wanneer u het beleid [log-naar-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) configureren

De volgende koppen toevoegen aan het verzoek.

-   Inhoudstype: toepassing/json
-   Autorisatie: SharedAccessSignature uid =...
    -   Voor instructies voor het genereren van de `SharedAccessSignature` Zie [Azure API Management REST API verificatie](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Geef het hoofdgedeelte van de aanvraag voor het gebruik van de volgende sjabloon.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`moet zijn ingesteld op `AzureEventHub`.
-   `description`biedt een optionele beschrijving van het logboek en kan zijn een tekenreeks met lengte nul desgewenst.
-   `credentials`bevat de `name` en `connectionString` van uw Hub Azure-gebeurtenis.

Wanneer u de aanvraag maken als het logboek wordt gemaakt een statuscode van `201 Created` als resultaat gegeven. 

>[AZURE.NOTE] Zie [een logboek maken](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT)voor andere mogelijke geretourneerde codes en hun redenen. Om te zien hoe andere bewerkingen uitvoeren, zoals lijstmachtigingen, bijwerken en verwijderen, raadpleegt u de documentatie van de entiteit [logboek](https://msdn.microsoft.com/library/azure/mt592020.aspx) .

## <a name="configure-log-to-eventhubs-policies"></a>Beleidsregels voor log-naar-eventhubs configureren

Wanneer uw logboek in API Management is geconfigureerd, kunt u uw beleid log-naar-eventhubs om de gewenste gebeurtenissen. Het beleid log-naar-eventhubs kan worden gebruikt in de beleidssectie voor binnenkomende of in de sectie uitgaande beleid.

Als u wilt configureren beleidsregels, aanmelden bij de [portal van Azure klassieke](https://manage.windowsazure.com)uw API-beheerservice navigeren en op **publisher-portal** of **beheren** voor toegang tot de publisher-portal.

![Publisher-portal][publisher-portal]

**Beleidsregels** op in het menu API beheer aan de linkerkant, schakelt u het gewenste product en API en klik op **toevoegen beleid**. In dit voorbeeld wordt een beleid toevoegt aan de **Echo API** in het product **onbeperkt** .

![Beleid toevoegen][add-policy]

Plaats de cursor in de `inbound` beleid sectie en klik op het beleid **Log to EventHub** om in te voegen de `log-to-eventhub` sjabloon voor beleid-instructie.

![Beleidseditor][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Vervang `logger-id` met de naam van het beheer van de API-logboek die u in de vorige stap hebt geconfigureerd.

U kunt een expressie die resulteert in een tekenreeks als de waarde voor de `log-to-eventhub` element. In dit voorbeeld wordt een tekenreeks met de datum en tijd, de servicenaam van de, aanvraag-id, verzoek IP-adres en de bewerkingsnaam van de geregistreerd.

Klik op **Opslaan** om op te slaan de configuratie van de bijgewerkte beleid. Zodra deze is opgeslagen het beleid actief is en gebeurtenissen worden vastgelegd aan de toegewezen Hub voor de gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

-   Meer informatie over Azure gebeurtenis Hubs
    -   [Aan de slag met Azure gebeurtenis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Berichten met EventProcessorHost ontvangen](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Gebeurtenis-Hubs handleiding voor het programmeren](../event-hubs/event-hubs-programming-guide.md)
-   Meer informatie over API Management en gebeurtenis Hubs-integratie
    -   [Logboek entiteitsverwijzing](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-naar-eventhub beleidsverwijzing](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Bewaak uw API's met Azure API Management, gebeurtenis Hubs en Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Bekijk de video Stapsgewijze instructies

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






