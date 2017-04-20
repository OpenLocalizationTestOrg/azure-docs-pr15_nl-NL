<properties
   pageTitle="Problemen met de Cloud Services-toepassing inzichten gebruik | Microsoft Azure"
   description="Leer hoe u problemen met cloud-service via toepassing inzichten om gegevens te verwerken van Azure diagnostische gegevens."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Problemen met de Cloud Services-toepassing inzichten gebruik

Met [Azure SDK 2,8](https://azure.microsoft.com/downloads/) en Azure diagnostisch hulpprogramma extensie 1,5 u kunt nu verzenden uw diagnostisch hulpprogramma Azure-gegevens voor uw Cloudservice rechtstreeks naar de toepassing inzichten. De verschillende soorten logboeken die zijn verzameld met Azure naar diagnostische toepassingslogboeken aan de, gebeurtenislogboeken van windows, inclusief ETW logboeken en prestatiemeteritems kunnen nu worden verzonden inzicht krijgen in toepassing en in de portal-toepassing inzichten UI visualiseren. Wanneer gebruikt samen met de toepassing inzichten SDK kunt u nu inzichten in aan de doelstellingen en logboeken die afkomstig zijn uit uw toepassing, evenals de systeem- en -infrastructuur niveau gegevens die afkomstig zijn uit diagnostisch hulpprogramma Azure krijgen.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Diagnostisch hulpprogramma Azure om gegevens te verzenden inzicht krijgen in toepassing configureren

Volg deze stappen om uw project cloud service Azure diagnostische hulpprogramma's om gegevens te verzenden toepassing inzicht krijgen in te stellen.

1) Klik in Visual Studio Solution Explorer met de rechtermuisknop op een rol en selecteer **Eigenschappen** om de rol designer te openen

![Eigenschappen van de rol oplossing Explorer][1]

2) Selecteer het selectievakje in om te **verzenden naar diagnostische gegevens toepassing inzicht krijgen** in rol designer onder de sectie diagnostische hulpprogramma 's

![Rol designer verzenden naar diagnostische gegevens inzicht krijgen in toepassing][2]

3) Selecteer in het dialoogvenster dat verschijnt de Resource van toepassing inzichten die u wilt de gegevens van Azure diagnostische hulpprogramma's om te verzenden. Het dialoogvenster kunt u een bestaande toepassing inzichten-bron selecteert uit uw abonnement of handmatig een sleutel instrumentation voor een resource van toepassing inzichten. Als u een bestaande toepassing inzichten bron geen kunt vervolgens u op door te klikken op de koppeling **maken een nieuwe resource** dat wordt geopend met een browservenster bij de portal voor Azure klassieke waarop u een Resource van toepassing inzichten kunt maken. Zie voor meer informatie over het maken van een resource van toepassing inzichten [maken een nieuwe resource van toepassing inzichten](../application-insights/app-insights-create-new-resource.md)

![Selecteer inzichten resource van toepassing][3]

4) Zodra u de resource van toepassing inzichten hebt toegevoegd, wordt de instrumentation-toets voor de desbetreffende resource wordt opgeslagen als een service-configuratie-instelling met de naam **APPINSIGHTS_INSTRUMENTATIONKEY**. U kunt deze configuratieinstelling voor elke service te configureren of omgeving wijzigen door een andere configuratie selecteren in de vervolgkeuzelijst van de configuratie Service omlaag en een nieuwe instrumentation sleutel voor die configuratie op te geven.

![Selecteer de service te configureren][4]

De instelling van de configuratie **APPINSIGHTS_INSTRUMENTATIONKEY** wordt gebruikt door Visual Studio voor het configureren van de extensie diagnostische gegevens met de juiste toepassing inzichten resourcegegevens tijdens publicatie. De configuratie-instelling is een handige manier van verschillende instrumentation toetsen voor verschillende configuraties definiëren. Visual Studio wordt deze instelling vertalen en invoegen in de diagnostische gegevens extensie openbare configuratie wanneer publiceren. Als u wilt het proces van het configureren van de extensie diagnostische hulpprogramma's met PowerShell vereenvoudigen, bevat het pakket uitvoer van Visual Studio ook de openbare configuratie XML met de juiste toepassing inzichten apparatuur toets opgenomen. De openbare configuratiebestanden zijn gemaakt in de map extensies en het patroon PaaSDiagnostics volgen. <RoleName>. PubConfig.xml. Een PowerShell op basis-implementaties kunnen dit patroon gebruiken om elke configuratie toewijzen aan een rol.

5) Inschakelen van het **hulpprogramma's voor diagnose gegevens verzenden naar toepassing inzichten** configureert Azure diagnostische hulpprogramma's voor het verzenden van alle prestatiemeteritems en niveau foutenlogboeken die worden verzameld door de diagnostische gegevens van Azure-agent toepassing inzicht krijgen automatisch. Als u verder configureren welke gegevens worden verzonden inzicht krijgen in toepassing wilt moet u het bestand *diagnostics.wadcfgx* voor elke rol handmatig bewerken. Zie [Azure diagnostische hulpprogramma's om te verzenden gegevens inzicht krijgen in toepassing configureren](../azure-diagnostics-configure-applicationinsights.md) voor meer informatie over het handmatig bijwerken van de configuratie.

Zodra de Cloud-Service is geconfigureerd voor het verzenden van diagnostische gegevens van Azure-gegevens naar toepassing inzichten die u kunt het dashboard implementeren naar Azure zoals u gewend bent is ervoor te zorgen dat de extensie Azure diagnostische gegevens ingeschakeld. Zie [een Cloudservice gebruik van Visual Studio publiceren](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Gegevens van Azure diagnostische gegevens weergeven in een toepassing inzichten
De Azure diagnostische telemetrielogboek wordt weergegeven in de toepassing inzichten resource dat is geconfigureerd voor uw cloudservice.

Hieronder ziet u hoe de verschillende Azure diagnostische hulpprogramma's typen kaart me bij toepassing inzichten concepten.  

-  Prestatie-items worden weergegeven als aangepast aan de doelstellingen in toepassing inzichten
-  Windows-gebeurtenislogboeken worden weergegeven als sporen en aangepaste gebeurtenissen in de toepassing inzichten
-  Logboeken aan de toepassing, ETW-logboeken en eventuele logboeken aan de infrastructuur van diagnostische gegevens worden weergegeven als sporen in toepassing inzichten.

Diagnostische gegevens van Azure-gegevens weergeven in inzichten van toepassing:

- [Aan de doelstellingen Verkenner](../application-insights/app-insights-metrics-explorer.md) gebruiken om te visualiseren alle aangepaste prestatie-items of telt van verschillende soorten windows-logboekgebeurtenissen.

![Aangepast aan de doelstellingen in aan de doelstellingen Explorer][5]

- Gebruik [Zoeken](../application-insights/app-insights-diagnostic-search.md) kunt u zoeken in de verschillende logboeken voor het traceren door diagnostisch hulpprogramma Azure verzonden. Voor voorbeeld als u een onverwerkte uitzondering had in een rol waardoor de rol vastlopen en Prullenbak die gegevens in het kanaal *toepassing* van *gebeurtenissenlogboek van Windows weergegeven wilt*. U kunt de zoekfunctionaliteit kijkt u naar het gebeurtenissenlogboek van Windows-fout en krijgen van de volledige stacktrace voor de uitzondering zodat u kunt de oorzaak van het probleem te vinden.

![Traces zoeken][6]

## <a name="next-steps"></a>Volgende stappen

- [De toepassing inzichten SDK naar uw cloudservice toevoegen](../application-insights/app-insights-cloudservices.md) aan de gegevens over aanvragen, uitzonderingen, afhankelijkheden en eventuele aangepaste telemetrielogboek verzenden vanuit uw toepassing. Gecombineerd met de Azure diagnostische hulpprogramma's gegevens krijgt u een compleet overzicht van uw toepassings- en alle in dezelfde toepassing inzicht resource.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
