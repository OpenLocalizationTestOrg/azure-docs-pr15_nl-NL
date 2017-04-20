<properties
   pageTitle="Veelvoorkomende oorzaken van Cloudservice rollen hergebruik | Microsoft Azure"
   description="De rol van een cloud-service die ineens wordt herhaald, kan leiden tot aanzienlijk downtime. Hier volgen enkele veelvoorkomende problemen die ertoe leiden dat rollen gerecycled, waarmee u downtime reduceren."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Veelvoorkomende problemen die ertoe leiden dat rollen naar de Prullenbak

In dit artikel worden enkele van de meest voorkomende oorzaken van problemen op te lossen beschreven en vindt u tips voor hoe u deze problemen oplossen. Een aanduiding dat er een probleem met een toepassing is is als het exemplaar van de rol kan niet worden gestart of deze bladert u tussen de Staten geïnitialiseerd, bezet en stoppen.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Ontbrekende runtime afhankelijkheden

Als een rol in uw toepassing afhankelijk van een is constructie die geen deel uitmaakt van .NET Framework of de Azure beheerde bibliotheek, moet u die constructie expliciet opnemen in de toepassingspakket. Houd er rekening mee dat andere Microsoft-kaders niet beschikbaar op Azure al dan niet standaard zijn. Als uw rol, is afhankelijk van dergelijk kader, moet u deze stroombaan toevoegen aan het toepassingspakket.

Voordat u opbouwen en uw toepassing inpakken, controleert u het volgende:

- Als het gebruik van Visual studio, zorg er dan voor dat de **Lokale kopie** eigenschap is ingesteld op **waar** voor elke waarnaar wordt verwezen constructie in uw project die geen deel uitmaakt van de Azure SDK of .NET Framework.

- Zorg ervoor dat het bestand web.config niet verwijst naar een niet-gebruikte stroombaan in het element gecompileerd.

- De **Actie bouwen** van elk .cshtml-bestand is ingesteld op **inhoud**. Dit zorgt ervoor dat de bestanden juist wordt weergegeven in het pakket en kunnen andere waarnaar wordt verwezen bestanden worden weergegeven in het pakket.

## <a name="assembly-targets-wrong-platform"></a>Constructie doelen verkeerde platform

Azure is een 64-bits-omgeving. .NET assemblies gecompileerd voor een 32-bits doel werken daarom niet op Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rol genereert onverwerkte uitzonderingen terwijl geïnitialiseerd of stoppen

Uitzonderingen de eventuele uitzonderingen die zijn gegenereerd door de methoden van de klasse [RoleEntryPoint] , waaronder de [OnStart], [OnStop]en methoden [uitvoeren] , zijn onverwerkte uitzonderingen. Als een onverwerkte uitzondering in een van de volgende manieren optreedt, wordt de rol Prullenbak. Als de rol is net zo vaak hergebruik, kan deze worden uitzondering opgetreden onverwerkte telkens wanneer die wordt geprobeerd om te beginnen.

## <a name="role-returns-from-run-method"></a>Rol geeft als resultaat van de methode Run

De methode [Run] is bedoeld om uit te voeren voor onbepaalde tijd. Als uw code de methode [uitvoeren overschrijft] , moet deze voor onbepaalde tijd slaapstand. Als de methode [Run] retourneert, wordt de rol herhaald.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Onjuiste DiagnosticsConnectionString-instelling

Als toepassing Azure diagnostische hulpprogramma's gebruikt, het configuratiebestand service moet opgeven de `DiagnosticsConnectionString` configuratie-instelling. Deze instelling, moet een HTTPS-verbinding met uw account opslagruimte in Azure opgeven.

Om ervoor te zorgen dat uw `DiagnosticsConnectionString` instelling juist is voordat u uw toepassingspakket implementeert bij Azure, controleert u of de volgende handelingen uit:  

- De `DiagnosticsConnectionString` punten instellen voor een geldige opslag-account in Azure wordt aangegeven.  
  Deze instelling wijst standaard aan het account geëmuleerde opslag, zodat u expliciet deze instelling wijzigen moet voordat u uw toepassingspakket implementeren. Als u deze instelling niet wijzigt, wordt een uitzondering opgetreden wanneer het exemplaar van de rol probeert te starten van de diagnostische monitor. Hierdoor kunnen de rol exemplaar naar de Prullenbak voor onbepaalde tijd.

- De verbindingsreeks is opgegeven in de volgende [indeling](../storage/storage-configure-connection-string.md). (Het protocol moet worden opgegeven als HTTPS). *MyAccountName* vervangen door de naam van uw account voor opslagruimte en *MyAccountKey* met uw access-sleutel:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Als u uw toepassing met behulp van Azure hulpprogramma's voor Microsoft Visual Studio ontwikkelt, kunt u de [eigenschap pagina's](https://msdn.microsoft.com/library/ee405486) voor het instellen van deze waarde.

## <a name="exported-certificate-does-not-include-private-key"></a>Geëxporteerde certificaat is niet inbegrepen bij persoonlijke sleutel

Als u wilt een Webrol onder SSL uitvoeren, moet u ervoor zorgen dat uw geëxporteerde management certificaat de persoonlijke sleutel bevat. Als u de *Windows-Certificaatbeheer* exporteren van het certificaat gebruikt, moet u **Ja** selecteren voor de optie **de persoonlijke sleutel exporteren** . Het certificaat moet worden geëxporteerd in de indeling PFX, dat wil de enige indeling die momenteel worden ondersteund zeggen.

## <a name="next-steps"></a>Volgende stappen

Meer [artikelen probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices weergeven

Meer rol hergebruik scenario's in [de reeks blogartikelen Kevin Williamson van](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)weergeven.

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Uitvoeren]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
