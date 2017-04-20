<properties
    pageTitle="Geavanceerde configuratie voor Windows universele Apps betrokkenheid SDK"
    description="Geavanceerde configuratie-opties voor Azure Mobile betrokkenheid met universele Apps voor Windows"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Geavanceerde configuratie voor Windows universele Apps betrokkenheid SDK

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-advanced-configuration.md)

Deze procedure wordt beschreven hoe u verschillende configuratieopties voor Android betrokkenheid van Azure mobiele apps configureren.

## <a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="disable-automatic-crash-reporting"></a>Automatische foutenrapportage uitschakelen

U kunt het automatische vastlopen rapportage-functie van betrokkenheid uitschakelen. Klik wanneer een onverwerkte uitzondering plaatsvindt, betrokkenheid gebeurt er niets.

> [AZURE.WARNING] Als u deze functie uitschakelt, klikt u vervolgens verzendt als een onverwerkte vastlopen in uw app klinkt betrokkenheid geen sluit het vastlopen **en** niet de sessie en taken.

Schakel automatische foutenrapportage aanpassen uw configuratie afhankelijk van de manier waarop die u deze gedeclareerd:

#### <a name="from-engagementconfigurationxml-file"></a>Uit `EngagementConfiguration.xml` bestand

Rapport vastlopen ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Uit `EngagementConfiguration` object tijdens runtime

Rapport vastlopen ingesteld op onwaar met behulp van uw EngagementConfiguration-object.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Realtime melden uitschakelen

Standaard logboeken op de betrokkenheid service-rapporten in realtime. Als uw toepassing Logboeken regelmatig rapporten, is het beter in de buffer de logboeken en om deze allemaal tegelijk melden op basis van de normale werktijd. Dit heet 'Burstmodus'.

Bel de methode hiervoor:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde (in **milliseconden)**. Wanneer u activeren de realtime logboekregistratie wilt, belt u de methode zonder een parameter of met de waarde 0.

Burst-modus wordt de levensduur enigszins, maar meteen invloed heeft op de Monitor betrokkenheid: alle sessies en taken duur worden afgerond op de drempelwaarde voor burst (dus sessies en taken korter dan de drempelwaarde voor burst zijn mogelijk niet zichtbaar). We raden u aan een drempelwaarde burst niet meer dan 30000 (30s) gebruiken. Opgeslagen logboeken zijn beperkt tot 300 items. Als u verzendt te lang is, kunt u sommige logboeken gaan verloren.

> [AZURE.WARNING] De drempelwaarde voor burst kan niet worden geconfigureerd met een periode van minder dan één seconde. Als u dit doet, wordt de SDK ziet u een spoor met de fout en automatisch worden herstelt u de standaardwaarde en nul seconden. Hierdoor wordt de SDK als u wilt rapporteren van de logboeken in realtime.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
