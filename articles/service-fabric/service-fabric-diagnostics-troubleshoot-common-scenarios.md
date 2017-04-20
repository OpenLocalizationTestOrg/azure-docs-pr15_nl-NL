<properties
   pageTitle="Problemen oplossen met de gebeurtenistracering | Microsoft Azure"
   description="De meest voorkomende problemen bij het implementeren van services op Microsoft Azure-Service stof."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Veelvoorkomende problemen oplossen van problemen als u services op Azure-Service stof implementeren

Wanneer u services op uw computer ontwikkelaars uitvoert, hoeft u eenvoudig te gebruiken in [Visual Studio hulpmiddelen voor foutopsporing](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). [Statusrapporten](service-fabric-view-entities-aggregated-health.md) zijn voor externe kolomgroepen altijd een goede locatie om te beginnen. De eenvoudigste manieren om toegang tot deze rapporten zijn via PowerShell of [SFX](service-fabric-visualizing-your-cluster.md). In dit artikel wordt ervan uitgegaan dat u zijn voor foutopsporing in een externe cluster en hebben een basiskennis van het gebruik van een van deze hulpmiddelen.

##<a name="application-crash"></a>De toepassing is vastgelopen
De "Partition lager is dan doelgerichte replica of exemplaar tellen" rapport is een goede aanwijzing die uw service, loopt vast. Als u wilt weten gaat waar uw service, loopt vast iets meer onderzoek. Wanneer uw service wordt uitgevoerd op schaal, zijn uw aanbevolen vriend een reeks goed doordacht sporen.  Het is raadzaam dat u [Azure diagnostische hulpprogramma's probeert](service-fabric-diagnostics-how-to-setup-wad.md) voor het verzamelen van deze sporen en een oplossing zoals [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md) gebruiken voor het weergeven en de traces zoeken.

![SFX Partition systeemstatus](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Tijdens de service of acteur initialisatie
Uitzonderingen de eventuele uitzonderingen voordat het servicetype wordt geïnitialiseerd, wordt het proces voor het vastlopen. Voor deze soorten loopt, wordt het toepassingsgebeurtenislogboek de fout van uw service weergegeven.
Hierna ziet u de meest voorkomende uitzonderingen om te zien voordat de service wordt geïnitialiseerd.

***System.IO.FileNotFoundException***

Deze fout wordt vaak vanwege afhankelijkheden constructie ontbreken. Controleer de eigenschap CopyLocal in Visual Studio of de globale constructie-cache voor het knooppunt.

***System.Runtime.InteropServices.COMException***
 *bij System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Hiermee wordt aangeduid dat de naam van het geregistreerde service niet overeenkomen met de servicemanifest.

[Diagnostisch hulpprogramma Azure](service-fabric-diagnostics-how-to-setup-wad.md) kan worden geconfigureerd voor het gebeurtenislogboek toepassing voor alle uw knooppunten automatisch uploaden.

###<a name="runasync-or-onactivateasync"></a>RunAsync() of OnActivateAsync()
Als het vastlopen bij de initialisatie of het uitvoeren van uw geregistreerde servicetype of acteur gebeurt, wordt de uitzondering wordt geblokkeerd door stof van Azure-Service. U kunt deze van de EventSource providers gedetailleerde in de sectie "Volgende stappen" weergeven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over bestaande diagnostisch hulpprogramma dat is verstrekt door de Service stof:

* [Betrouwbare betrokkenen diagnostische gegevens](service-fabric-reliable-actors-diagnostics.md)
* [Diagnostische hulpprogramma's voor betrouwbare Services](service-fabric-reliable-services-diagnostics.md)
