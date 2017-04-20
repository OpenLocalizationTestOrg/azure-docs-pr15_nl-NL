<properties
    pageTitle="Logica App tijdslimieten en de instellingen | Microsoft Azure"
    description="Overzicht van de grenzen van de service en de waarden van de systeemconfiguratie is beschikbaar voor logica-Apps."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logica App limieten- en configuratieservices

Hieronder vindt u informatie over de huidige limieten en configuratiegegevens voor Azure logica Apps.

## <a name="limits"></a>Limieten

### <a name="http-request-limits"></a>Limieten voor HTTP-aanvraag

Hierna ziet u limieten voor een enkele HTTP-aanvraag en/of verbindingslijn oproep

#### <a name="timeout"></a>Time-out

|Naam|Limiet|Notities|
|----|----|----|
|Time-out van de aanvraag|1 minuut|Een [patroon asynchrone](app-service-logic-create-api-app.md) of [tot een loopback](app-service-logic-loops-and-scopes.md) kan naar wens helpen|

#### <a name="message-size"></a>Grootte van berichten

|Naam|Limiet|Notities|
|----|----|----|
|Grootte van berichten|50 MB|50MB kunnen niet worden ondersteund door sommige verbindingslijnen en API's.  Aanvraag trigger ondersteunt maximaal 25MB|
|Expressie evaluatie limiet|131.072 tekens|`@concat()`, `@base64()`, `string` niet langer zijn dan deze|

#### <a name="retry-policy"></a>Probeer beleid

|Naam|Limiet|Notities|
|----|----|----|
|Herhalingspogingen|4|Met de [parameter van het beleid opnieuw](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) kunt configureren|
|Maximale vertraging|1 uur|Met de [parameter van het beleid opnieuw](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) kunt configureren|
|Min vertraging|20 min|Met de [parameter van het beleid opnieuw](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) kunt configureren|

### <a name="run-duration-and-retention"></a>Duur volgens de uitvoeren en bewaarbeleid

Hierna ziet u de limieten voor een enkele logica-app uitvoeren.

|Naam|Limiet|Notities|
|----|----|----|
|Duur uitvoeren|90 dagen||
|Opslag bewaarbeleid|90 dagen|Dit is van de begintijd uitvoeren|
|Min, terugkeerpatroon|15 sec||
|Max, terugkeerpatroon|500 dagen||


### <a name="looping-and-debatching-limits"></a>Herhalen en limieten debatching

Hierna ziet u limieten voor een enkele logica-app uitvoeren.

|Naam|Limiet|Notities|
|----|----|----|
|ForEach items|5.000|U kunt de [actie OpenQuery](../connectors/connectors-native-query.md) groter matrices naar wens filteren|
|Tot iteraties|10.000||
|SplitOn items|5.000||
|ForEach parallellisme|20|U kunt instellen op een opeenvolgende foreach door toe te voegen `"operationOptions": "Sequential"` naar de `foreach` actie|


### <a name="throughput-limits"></a>Doorvoer limieten

Hierna ziet u limieten voor een exemplaar van de app één logica. 

|Naam|Limiet|Notities|
|----|----|----|
|Triggers per seconde|100|Werkstromen kunnen worden verdelen over meerdere apps naar wens|

### <a name="definition-limits"></a>Definitie limieten

Hierna ziet u limieten voor de definitie van een enkel logica-app.

|Naam|Limiet|Notities|
|----|----|----|
|Acties in ForEach|1|U kunt geneste werkstromen om uit te breiden dit naar wens toevoegen|
|Acties per werkstroom|60|U kunt geneste werkstromen om uit te breiden dit naar wens toevoegen|
|Actie nesten diepteas toegestaan|5|U kunt geneste werkstromen om uit te breiden dit naar wens toevoegen|
|Loopt per regio per abonnement|1000||
|Triggers per werkstroom|10||
|Maximum aantal tekens per expressie|8192||
|Max `trackedProperties` grootte in tekens|16.000|
|`action`/`trigger`Naamlimiet|80||
|`description`maximale lengte|256||
|`parameters`limiet|50||
|`outputs`limiet|10||

## <a name="configuration"></a>Configuratie

### <a name="ip-address"></a>IP-adres

Oproepen vanuit een [verbindingslijn](../connectors/apis-list.md) wordt afkomstig zijn uit het IP-adres hieronder.

Oproepen vanuit een logica app rechtstreeks (dat wil zeggen via [HTTP](../connectors/connectors-native-http.md) of [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) mogelijk afkomstig zijn uit een van de [Azure Datacenter IP-bereiken](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Logica App regio|Uitgaande IP|
|-----|----|
|Australië Oost|40.126.251.213|
|Australië Zuidoost|40.127.80.34|
|Brazilië Zuid|191.232.38.129|
|Centraal India|104.211.98.164|
|Central US|40.122.49.51|
|Oost-Azië|23.99.116.181|
|Oost-VS|191.237.41.52|
|Oost-Amerikaanse 2|104.208.233.100|
|Japan Oost|40.115.186.96|
|Japan West|40.74.130.77|
|Centraal Noord-Amerikaanse|65.52.218.230|
|Noord-Europa|104.45.93.9|
|Zuid-Central US|104.214.70.191|
|Zuidoost-Azië|13.76.231.68|
|Zuid-India|104.211.227.225|
|West Europa|40.115.50.13|
|West India|104.211.161.203|
|West VS|104.40.51.248|


## <a name="next-steps"></a>Volgende stappen  

- Als u wilt beginnen met logica Apps, volgt u de zelfstudie [een logica-App maken](app-service-logic-create-a-logic-app.md) .  
- [Algemene voorbeelden en scenario's weergeven](app-service-logic-examples-and-scenarios.md)
- [U kunt bedrijfsprocessen automatiseren met logica Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Meer informatie over het integreren van uw systemen met logica Apps](http://channel9.msdn.com/Events/Build/2016/P462)