<properties 
pageTitle="Cloudservice levenscyclus gebeurtenissen afhandelen | Microsoft Azure" 
description="Leer hoe de methoden levenscyclus van de rol van een Cloudservice kunnen worden gebruikt in .NET" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>De levenscyclus van een webpagina of werknemer rol in .NET aanpassen

Wanneer u een rol werknemer maakt, kunt u de [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) klasse waarmee methoden om berichten te overschrijven waarmee u kunt reageren op gebeurtenissen die levenscyclus uitbreiden. Voor web rollen is deze klasse is optioneel, zodat u deze gebruiken moet om te reageren op gebeurtenissen die levenscyclus.

## <a name="extend-the-roleentrypoint-class"></a>De klasse RoleEntryPoint uitbreiden

De klasse [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) bevat methoden die door Azure worden aangeroepen wanneer deze **wordt gestart**, **wordt uitgevoerd**, of **stopt** een rol web of werknemer. U kunt desgewenst deze methoden voor het beheer van initialisatie van de rol, rol afsluiten reeksen of de uitvoeringsthread van de rol negeren. 

Wanneer u **RoleEntryPoint**uitbreidt, moet u zich bewust zijn van de volgende problemen van de methoden:

-   De methoden [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) en [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) retourneren een Booleaanse waarde, dus is het mogelijk om terug te keren **Onwaar** door deze methoden.

     Als uw code **onwaar retourneert**, de rol proces is onverwacht is beëindigd, zonder een afsluiten die u geplaatst hebt mogelijk. In het algemeen, moet u niet uit de methode **OnStart** **Onwaar** retourneren.
     
-   Een onbekende uitzondering binnen een overbelasting van een methode **RoleEntryPoint** wordt behandeld als een onverwerkte uitzondering.

     Als een uitzondering binnen een van de levenscyclus van manieren plaatsvindt, Azure wekt de [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) gebeurtenis en klikt u vervolgens het proces is beëindigd. Nadat uw rol offline genomen is, wordt deze opnieuw gestart door Azure. Wanneer een onverwerkte uitzondering plaatsvindt, de [stoppen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) -gebeurtenis is niet geactiveerd en de methode **OnStop** niet wordt genoemd.

Als uw rol niet wordt gestart of is hergebruik tussen de geïnitialiseerd, bezet, en stoppen Staten, is het mogelijk dat uw code een onverwerkte uitzondering binnen een van de levenscyclus van gebeurtenissen telkens wanneer de rol opnieuw is opgestart genereren. In dit geval gebruikt de gebeurtenis [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) de oorzaak van de uitzondering en deze correct worden verwerkt. Uw rol kan ook worden geretourneerd uit de methode [Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , waardoor de rol om opnieuw te starten. Zie voor meer informatie over de implementatie Staten [Veelvoorkomende problemen die oorzaak rollen naar Prullenbak](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Als u de **Azure hulpprogramma's voor Microsoft Visual Studio** gebruikt voor het ontwikkelen van uw toepassing, uitbreiden de rol projectsjablonen de klas **RoleEntryPoint** automatisch voor u, in de bestanden *WebRole.cs* en *WorkerRole.cs* .

## <a name="onstart-method"></a>OnStart methode

De methode **OnStart** wordt aangeroepen wanneer uw exemplaar van de rol online door Azure is gebracht. Terwijl de code OnStart wordt uitgevoerd, het exemplaar van de rol is gemarkeerd als **bezet** en geen externe verkeer doorgestuurd naar deze door de taakverdeling. U kunt deze methode als u wilt uitvoeren initialisatie werk, zoals gebeurtenishandlers implementeren en te starten [Diagnostisch hulpprogramma Azure](cloud-services-how-to-monitor.md)negeren.

Als **OnStart** **true**retourneert, wordt het exemplaar is geïnitialiseerd en Azure de **RoleEntryPoint.Run** aanroept. Als **OnStart** **false**retourneert, wordt de rol onmiddellijk, beëindigt zonder uit te voeren van alle reeksen gepland afsluiten.

Het volgende voorbeeld ziet u hoe u de methode **OnStart** overschrijven. Deze methode configureert en een diagnostische monitor begint wanneer het rol-exemplaar wordt gestart en een overdracht van logboekregistratie gegevens naar een account opslag ingesteld:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop-methode

De methode **OnStop** wordt aangeroepen nadat een exemplaar van de rol heeft genomen offline Azure en voordat het proces afgesloten. U kunt deze methode als u wilt bellen code vereist voor uw exemplaar van de rol naar correct is afgesloten negeren.

> [AZURE.IMPORTANT] Code die wordt uitgevoerd in de methode **OnStop** heeft een beperkte periode om te voltooien wanneer deze wordt aangeroepen voor redenen dan een door de gebruiker geactiveerde afsluiten. Nadat deze tijd is verstreken, wordt het proces is beëindigd, dus moet u ervoor zorgen dat code in de **OnStop** methode kunnen snel uitvoeren of maximaal wordt niet uitgevoerd tot voltooiing toegestaan. De methode **OnStop** wordt aangeroepen nadat de gebeurtenis **stoppen** wordt geactiveerd.


## <a name="run-method"></a>Methode uitvoeren

U kunt de methode **Run** implementatie van een thread langdurige voor uw exemplaar van de rol negeren.

De methode **Run** overschrijven is niet vereist. de standaardimplementatie begint een thread die eeuwen slaapstand. Als u de methode **Run** overschrijven, moet uw code voor onbepaalde tijd blokkeren. Als de methode **Run** als resultaat, wordt de rol automatisch probleemloos gerecycled; met andere woorden, Azure activeert het **stoppen van** gebeurtenis en de **OnStop** aanroept zodat uw reeksen afsluiten kunnen worden uitgevoerd voordat de rol is verbroken.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Implementeren van de ASP.NET levenscyclus methoden voor een Webrol

U kunt de ASP.NET levenscyclus methoden, behalve die door de klasse **RoleEntryPoint** initialisatie en afsluiten reeksen voor een Webrol beheren. Dit kan zijn handig voor compatibiliteit als u een bestaande ASP.NET-toepassing naar Azure zijn overbrengen. De ASP.NET levenscyclus methoden worden aangeroepen vanuit de **RoleEntryPoint** methoden. De **toepassing\_starten** methode wordt aangeroepen nadat de methode **RoleEntryPoint.OnStart** is voltooid. De **toepassing\_einde** methode wordt aangeroepen voordat de methode **RoleEntryPoint.OnStop** wordt genoemd.

## <a name="next-steps"></a>Volgende stappen
Informatie over het [maken van een servicepakket cloud](cloud-services-model-and-package.md).