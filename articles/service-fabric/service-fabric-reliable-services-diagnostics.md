<properties
   pageTitle="Diagnostische hulpprogramma's voor statuscontrole betrouwbare Services | Microsoft Azure"
   description="Diagnostische functionaliteit voor Stateful betrouwbare Services"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostische functionaliteit voor Stateful betrouwbare Services
De klas Stateful betrouwbare Services StatefulServiceBase Hiermee [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen die kunnen worden gebruikt om de fouten opsporen in de service, bevatten inzichten in hoe de runtime is, en oplossen van problemen met plaatst.

## <a name="eventsource-events"></a>EventSource gebeurtenissen
De naam van de EventSource voor de klas Stateful betrouwbare Services StatefulServiceBase is 'Microsoft-ServiceFabric-Services'. Gebeurtenissen van deze gebeurtenisbron worden weergegeven in het venster [Diagnostisch hulpprogramma gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) wanneer de service [Foutopsporing in Visual Studio is uitgevoerd wordt](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en -technologieën die bij het oplossen van verzamelen en/of EventSource gebeurtenissen weergeven zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure diagnostische gegevens](../cloud-services/cloud-services-dotnet-diagnostics.md)en de [Microsoft TraceEvent bibliotheek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Gebeurtenissen

|De gebeurtenisnaam van de|Gebeurtenis-ID|Niveau|Gebeurtenisbeschrijving|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Informatie|Wanneer het service RunAsync taak wordt gestart|
|StatefulRunAsyncCancellation|2|Informatie|Wanneer het service RunAsync taak is geannuleerd|
|StatefulRunAsyncCompletion|3|Informatie|Wanneer het service RunAsync taak is voltooid|
|StatefulRunAsyncSlowCancellation|4|Waarschuwing|Wanneer het service RunAsync taak duurt te lang is geannuleerd voltooien|
|StatefulRunAsyncFailure|5|Fout|Wanneer het service RunAsync taak genereert een uitzondering|

## <a name="interpret-events"></a>Gebeurtenissen interpreteren

Gebeurtenissen StatefulRunAsyncInvocation, StatefulRunAsyncCompletion en StatefulRunAsyncCancellation zijn handig voor de service-schrijver voor meer informatie over de levenscyclus van een service--, evenals de timing voor wanneer een service is gestart, geannuleerd of voltooid. Dit is handig wanneer foutopsporing serviceproblemen of informatie over de levenscyclus van de service.

Schrijvers van de service moeten aandacht besteden aan StatefulRunAsyncSlowCancellation en StatefulRunAsyncFailure gebeurtenissen want die problemen met de service geven.

StatefulRunAsyncFailure geeft wanneer de service RunAsync() taak een uitzondering genereert. Een uitzondering opgetreden geeft meestal aan een fout of fout in de service. Daarnaast wordt de uitzondering door de service mislukt, zodat deze wordt verplaatst naar een ander knooppunt. Dit is een dure bewerking en verzoeken voor oproepen kan worden vertraagd terwijl de service wordt verplaatst. Schrijvers van de service moeten de oorzaak van de uitzondering en deze indien mogelijk te beperken.

StatefulRunAsyncSlowCancellation geeft wanneer een aanvraag annulering voor de taak RunAsync langer duren dan vier seconden. Wanneer een service te lang is geannuleerd voltooien duurt, heeft dit gevolgen voor de mogelijkheid om snel opnieuw worden gestart op een ander knooppunt. Dit kan van invloed zijn op de algehele beschikbaarheid van de service.
