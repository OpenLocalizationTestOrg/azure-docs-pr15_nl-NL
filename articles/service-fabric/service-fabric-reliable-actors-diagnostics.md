<properties
   pageTitle="Betrokkenen diagnostisch hulpprogramma en monitoring | Microsoft Azure"
   description="In dit artikel worden de diagnostische hulpprogramma's en functies in de Service stof betrouwbare betrokkenen runtime, inclusief de gebeurtenissen en prestatiemeteritems dat door deze controle van prestaties."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnostisch hulpprogramma en prestatiecontroles voor betrouwbare betrokkenen
De runtime betrouwbare betrokkenen genereert [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) evenementen en [van prestatiemeteritems](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze inzichten in hoe de runtime functioneert bieden en helpen bij problemen oplossen en prestatiecontroles uit.

## <a name="eventsource-events"></a>EventSource gebeurtenissen
De naam van de provider EventSource voor betrouwbare betrokkenen runtime is "Microsoft-ServiceFabric-betrokkenen". Gebeurtenissen van deze gebeurtenisbron worden weergegeven in het venster [Diagnostisch hulpprogramma gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) wanneer de acteur-toepassing [Foutopsporing in Visual Studio is uitgevoerd wordt](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en -technologieën die bij het oplossen van verzamelen en/of EventSource gebeurtenissen weergeven zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Diagnostisch hulpprogramma Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) [Semantic logboekregistratie](https://msdn.microsoft.com/library/dn774980.aspx)en de [Microsoft TraceEvent bibliotheek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Trefwoorden
Alle gebeurtenissen die deel uitmaakt van de betrouwbare betrokkenen EventSource zijn gekoppeld aan een of meer trefwoorden. Hiermee worden filteren van gebeurtenissen die zijn verzameld. De volgende trefwoord bits zijn gedefinieerd.

|Bits|Beschrijving|
|---|---|
|0x1|Instellen van belangrijke gebeurtenissen die de werking van de runtime stof betrokkenen samenvatten.|
|0x2|Reeks gebeurtenissen die acteur methode oproepen beschrijven. Zie voor meer informatie het [inleidende onderwerp op betrokkenen](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Reeks gebeurtenissen met betrekking tot acteur staat. Zie het onderwerp op [acteur staat management](service-fabric-reliable-actors-state-management.md)voor meer informatie.|
|0x8|Reeks inschakelen gebaseerde gelijktijdigheid in de acteur-gebeurtenissen. Zie het onderwerp op [bij gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency)voor meer informatie.|

## <a name="performance-counters"></a>Prestatie-items
De runtime betrouwbare betrokkenen Hiermee definieert u de volgende prestatiemeteritem categorieën.

|Categorie|Beschrijving|
|---|---|
|Service stof acteur|Items die specifiek bij Azure-Service stof betrokkenen, bijvoorbeeld tijd die u hebt gemaakt om op te slaan acteur staat|
|Service stof acteur-methode|Items specifiek zijn voor methoden geïmplementeerd door de Service stof betrokkenen, bijvoorbeeld hoe vaak een acteur-methode wordt aangeroepen|

Elk van de bovenstaande categorieën heeft een of meer items.

De [Prestaties Monitor met een Windows](https://technet.microsoft.com/library/cc749249.aspx) -toepassing die beschikbaar zijn in de Windows-besturingssysteem standaard kan worden gebruikt voor het verzamelen en prestatiemeteritemgegevens weergeven. [Diagnostisch hulpprogramma Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van prestatiemeteritemgegevens en uploaden naar Azure tabellen.

### <a name="performance-counter-instance-names"></a>Prestatiemeteritem exemplaarnamen
Een cluster met een groot aantal acteur services of acteur-partities heeft een groot aantal exemplaren van acteur prestatiemeteritem. De namen van prestatiemeteritem exemplaar kunt identificeren de specifieke [partition](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) en acteur-methode (indien van toepassing) het exemplaar van de teller prestaties is gekoppeld.

#### <a name="service-fabric-actor-category"></a>Service stof acteur categorie
Voor de categorie `Service Fabric Actor`, de namen van de teller-exemplaar zijn in de volgende indeling:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* is de tekenreeksweergave van de Service stof partition-ID in te stellen dat het exemplaar van de teller prestaties is gekoppeld aan. De partition-ID is een GUID en de tekenreeks wordt gegenereerd door het [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met opmaak aanduiding "D".

*ActorRuntimeInternalID* is de tekenreeksweergave van een 64-bits geheel getal dat wordt gegenereerd door de runtime stof betrokkenen voor het interne gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat de uniekheid conflict met de namen van andere prestatiemeteritem exemplaar voorkomen. Gebruikers moeten niet proberen interpreteren van dit gedeelte van de naam van het exemplaar.

De volgende is een voorbeeld van een naam van het exemplaar voor een item dat hoort bij de `Service Fabric Actor` categorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

In het bovenstaande voorbeeld `2740af29-78aa-44bc-a20b-7e60fb783264` is de tekenreeksweergave van de Service stof partition-ID, en `635650083799324046` is de 64-bits-ID die is gegenereerd voor de runtime bevindt zich op een interne gebruiken.

#### <a name="service-fabric-actor-method-category"></a>Service stof acteur methode categorie
Voor de categorie `Service Fabric Actor Method`, de namen van de teller-exemplaar zijn in de volgende indeling:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* is de naam van de acteur-methode die het exemplaar van de teller prestaties is gekoppeld. De opmaak van de methodenaam van de wordt bepaald op basis van bepaalde logica stof betrokkenen runtime waarmee de leesbaarheid van de naam met beperkingen voor de maximale lengte van de prestatiemeteritem exemplaarnamen in Windows.

*ActorsRuntimeMethodId* is de tekenreeksweergave van een 32-bits geheel getal dat wordt gegenereerd door de runtime stof betrokkenen voor het interne gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat de uniekheid conflict met de namen van andere prestatiemeteritem exemplaar voorkomen. Gebruikers moeten niet proberen interpreteren van dit gedeelte van de naam van het exemplaar.

*ServiceFabricPartitionID* is de tekenreeksweergave van de Service stof partition-ID in te stellen dat het exemplaar van de teller prestaties is gekoppeld aan. De partition-ID is een GUID en de tekenreeks wordt gegenereerd door het [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met opmaak aanduiding "D".

*ActorRuntimeInternalID* is de tekenreeksweergave van een 64-bits geheel getal dat wordt gegenereerd door de runtime stof betrokkenen voor het interne gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat de uniekheid conflict met de namen van andere prestatiemeteritem exemplaar voorkomen. Gebruikers moeten niet proberen interpreteren van dit gedeelte van de naam van het exemplaar.

De volgende is een voorbeeld van een naam van het exemplaar voor een item dat hoort bij de `Service Fabric Actor Method` categorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

In het bovenstaande voorbeeld `ivoicemailboxactor.leavemessageasync` is de methodenaam van de `2` is de 32-bits-ID die is gegenereerd voor intern gebruik van de runtime, `89383d32-e57e-4a9b-a6ad-57c6792aa521` is de tekenreeksweergave van de Service stof partition-ID, en `635650083804480486` is de 64-bits-ID die is gegenereerd voor de runtime bevindt zich op een interne gebruiken.

## <a name="list-of-events-and-performance-counters"></a>Lijst met gebeurtenissen en prestatiemeteritems

### <a name="actor-method-events-and-performance-counters"></a>Acteur methode evenementen en prestatiemeteritems
De runtime betrouwbare betrokkenen Hiermee plaatst u de volgende gebeurtenissen die betrekking hebben op [acteur methoden](service-fabric-reliable-actors-introduction.md#actors).

|De gebeurtenisnaam van de|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorMethodStart|7|Uitgebreide|0x2|Betrokkenen runtime wordt een methode acteur roepen.|
|ActorMethodStop|8|Uitgebreide|0x2|De uitvoering van een methode acteur, is voltooid. Dat wil zeggen asynchrone aanroep van de runtime met de methode acteur heeft geretourneerd en de taak die is geretourneerd door de methode acteur is voltooid.|
|ActorMethodThrewException|9|Waarschuwing|0x3|Er is een uitzondering opgetreden tijdens de uitvoering van een methode acteur van tijdens asynchrone aanroep van de runtime met de acteur-methode of tijdens de uitvoering van de taak die het resultaat van de acteur-methode. Deze gebeurtenis wordt aangegeven dat sommige sorteren van fout in de acteur-code die nog moet worden onderzoek.|

De runtime betrouwbare betrokkenen publiceert de volgende prestatiemeteritems voor de uitvoering van acteur methoden.

|De naam van categorie|De Tellernaam van de|Beschrijving|
|---|---|---|
|Service stof acteur-methode|Aanroepen/Sec|Aantal keren dat de methode acteur-service wordt aangeroepen per seconde|
|Service stof acteur-methode|Gemiddelde milliseconden per aanroep|Tijd uitvoering van de methode acteur-service (in milliseconden)|
|Service stof acteur-methode|Uitzonderingen verzonden per seconde|Aantal keren dat de acteur service-methode heeft een uitzondering per seconde|

### <a name="concurrency-events-and-performance-counters"></a>Bij gelijktijdigheid evenementen en prestatiemeteritems
De runtime betrouwbare betrokkenen Hiermee plaatst u de volgende gebeurtenissen die betrekking hebben op [bij gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency).

|De gebeurtenisnaam van de|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Uitgebreide|0x8|Deze gebeurtenis is geschreven aan het begin van elke nieuwe inschakelen in een acteur. Bevat het aantal in behandeling acteur-oproepen die wachten om de vergrendeling per acteur die worden toegepast op basis van inschakelen bij gelijktijdigheid te verkrijgen.|

De runtime betrouwbare betrokkenen publiceert de volgende prestatiemeteritems die betrekking hebben op bij gelijktijdigheid.

|De naam van categorie|De Tellernaam van de|Beschrijving|
|---|---|---|
|Service stof acteur|# van acteur-oproepen wachten op acteur vergrendelen|Aantal in behandeling acteur oproepen in afwachting van vergrendelen voor de per acteur die worden toegepast op basis van inschakelen bij gelijktijdigheid|
|Service stof acteur|Gemiddelde milliseconden per vergrendelen wachten|Tijd (in milliseconden) vergrendelen voor de per acteur die worden toegepast op basis van inschakelen bij gelijktijdigheid|
|Service stof acteur|Gemiddelde milliseconden acteur vergrendelen gehouden|Tijd (in milliseconden) waarvoor de per acteur vergrendeling|

### <a name="actor-state-management-events-and-performance-counters"></a>Acteur staat management evenementen en prestatiemeteritems
De runtime betrouwbare betrokkenen Hiermee plaatst u de volgende gebeurtenissen die betrekking hebben op [acteur staat management](service-fabric-reliable-actors-state-management.md).

|De gebeurtenisnaam van de|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorSaveStateStart|10|Uitgebreide|0x4|Betrokkenen runtime wordt de status van acteur opslaan.|
|ActorSaveStateStop|11|Uitgebreide|0x4|Betrokkenen runtime is voltooid de acteur-staat op te slaan.|

De runtime betrouwbare betrokkenen publiceert de volgende prestatiemeteritems die betrekking hebben op acteur staat management.

|De naam van categorie|De Tellernaam van de|Beschrijving|
|---|---|---|
|Service stof acteur|Gemiddelde milliseconden per opslaan staat bewerking|Tijd acteur status opslaan (in milliseconden)|
|Service stof acteur|Gemiddelde milliseconden per laden staat bewerking|Tijd aan het laden van acteur staat (in milliseconden)|

### <a name="events-related-to-actor-replicas"></a>Gebeurtenissen die zijn gerelateerd aan acteur replica 's
De runtime betrouwbare betrokkenen Hiermee plaatst u de volgende gebeurtenissen die betrekking hebben op [acteur replica's](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|De gebeurtenisnaam van de|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Informatie|0x1|Acteur replica gewijzigd rol primair. Dit betekent dat de betrokkenen voor deze partition binnen deze replica wordt gemaakt.|
|ReplicaChangeRoleFromPrimary|2|Informatie|0x1|Rol acteur replica gewijzigd in niet-primaire. Dit betekent dat de betrokkenen voor deze partition niet meer in deze replica worden gemaakt. Geen nieuwe aanvragen worden bezorgd bij al hebt gemaakt in deze replica betrokkenen. De betrokkenen wordt verwijderd nadat alle aanvragen in uitvoering zijn voltooid.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Acteur activering en deactiveren gebeurtenissen en prestatiemeteritems
De runtime betrouwbare betrokkenen Hiermee plaatst u de volgende gebeurtenissen die betrekking hebben op [acteur activering en deactiveren](service-fabric-reliable-actors-lifecycle.md).

|De gebeurtenisnaam van de|Gebeurtenis-ID|Niveau|Trefwoord|Beschrijving|
|---|---|---|---|---|
|ActorActivated|5|Informatie|0x1|Een acteur al is geactiveerd.|
|ActorDeactivated|6|Informatie|0x1|Een acteur is gedeactiveerd.|

De runtime betrouwbare betrokkenen publiceert de volgende prestatiemeteritems die betrekking hebben op acteur activering en deactiveren.

|De naam van categorie|De Tellernaam van de|Beschrijving|
|---|---|---|
|Service stof acteur|Gemiddelde OnActivateAsync milliseconden|Tijd OnActivateAsync methode uitvoering (in milliseconden)|

### <a name="actor-request-processing-performance-counters"></a>Verwerking van vergaderverzoeken acteur prestatie-items
Wanneer een client een methode via een acteur-proxy-object roept, resulteert dit in een bericht naar de acteur-service is verzonden via het netwerk. De service verwerkt het verzoekbericht en stuurt een reactie terug naar de klant. De runtime betrouwbare betrokkenen publiceert de volgende prestatiemeteritems die betrekking hebben op verwerking van acteur-vergaderverzoeken.

|De naam van categorie|De Tellernaam van de|Beschrijving|
|---|---|---|
|Service stof acteur|# met openstaande aanvragen|Aantal aanvragen in de service wordt verwerkt|
|Service stof acteur|Gemiddelde milliseconden per aanvraag|Tijd (in milliseconden) door de service verwerking een aanvraag|
|Service stof acteur|Gemiddelde milliseconden voor verzoek deserialisatie|Tijd (in milliseconden) terugconverteren van acteur request-bericht wanneer het is ontvangen door de service|
|Service stof acteur|Gemiddelde milliseconden voor antwoord serialisatie|Tijd (in milliseconden) het bericht acteur antwoord op de service serialiseren voordat het antwoord wordt verzonden naar de klant|

## <a name="next-steps"></a>Volgende stappen
 - [Hoe betrouwbare betrokkenen het platform Service stof gebruiken](service-fabric-reliable-actors-platform.md)
 - [Acteur API-documentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Voorbeeld van een code](https://github.com/Azure/servicefabric-samples)
