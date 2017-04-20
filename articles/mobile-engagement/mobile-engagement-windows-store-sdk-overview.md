<properties
    pageTitle="Universele SDK-integratie voor Windows"
    description="Windows universele integratie voor SDK voor Azure mobiele betrokkenheid"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Integratie van de Windows-universele SDK voor Azure mobiele betrokkenheid

In dit document worden alle integratie en configuratie opties beschikbaar voor de Azure Mobile betrokkenheid Windows universele SDK.

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, moet u eerst onze [15 minuten zelfstudie](mobile-engagement-windows-store-dotnet-get-started.md)uitvoeren.

## <a name="advanced-features"></a>Geavanceerde functies

### <a name="reporting-features"></a>Rapportfuncties
U kunt deze functies toevoegen:

1. [Geavanceerde opties voor rapportage](mobile-engagement-windows-store-advanced-reporting.md)

2. [Geavanceerde configuratie-opties](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Meldingen

[Het bereik (meldingen) integreren in uw Windows universele-app](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Tag abonnement-implementatie:

[Het gebruik van de geavanceerde Mobile betrokkenheid API labelen in uw Windows universele-app](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Releaseopmerkingen

###<a name="340-04192016"></a>3.4.0 (04/19/2016)

-   Verbeteringen in de overlay hebt bereikt.
-   Toegevoegde "TestLogLevel" API inschakelen/uitschakelen/filter console logboeken dat door de SDK.
-   Vaste activiteit in meldingen hebt samengesteld van de eerste activiteit niet wordt weergegeven op de App starten.

Zie de [volledige releaseopmerkingen](mobile-engagement-windows-store-release-notes.md) voor eerdere versies

##<a name="upgrade-procedures"></a>De upgrade procedures

Als u hebt al een oudere versie van betrokkenheid geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

Als u verschillende versies van de SDK hebt gemist, is het wellicht moet u enkele procedures volgen. Zie de volledige [Procedures upgraden](mobile-engagement-windows-store-upgrade-procedure.md). Als u migreert van 0.10.1 naar 0.11.0 moet u eerst de procedure 'van 0.9.0 naar 0.10.1"Volg vervolgens de procedure 'van 0.10.1 naar 0.11.0' bijvoorbeeld.

###<a name="from-330-to-340"></a>Uit 3.3.0 naar 3.4.0

####<a name="test-logs"></a>Test-Logboeken

Console-logboeken geproduceerd door de SDK kunnen nu zijn ingeschakeld/uitgeschakeld/gefilterd. Als u wilt aanpassen, werken de eigenschap `EngagementAgent.Instance.TestLogEnabled` op een van de waarden die verkrijgbaar is bij het `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Resources

De overlay bereik is verbeterd. Deze maakt deel uit van de bronnen voor het pakket van SDK NuGet.

Tijdens het bijwerken naar de nieuwe versie van de SDK, kunt u kiezen of u wilt behouden van uw bestaande bestanden uit de map overlay uw resources al dan niet:

* Als de vorige overlay voor u werkt of u integreert de `WebView` elementen handmatig, klikt u vervolgens kunt u besluiten te behouden uw afsluiten bestanden, het nog steeds werkt.
* Als u wilt bijwerken naar de nieuwe overlay, vervangt u de volledige `overlay` map van uw resources met de nieuwe sectie die u uit het pakket SDK (UWP apps: na de upgrade, kunt u de nieuwe overlay-map krijgen van % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Gebruik van de nieuwe overlay overschrijft aanpassingen op de vorige versie.

### <a name="upgrade-from-older-versions"></a>Een upgrade uitvoert vanuit oudere versies

Zie [Procedures een upgrade uitvoeren](mobile-engagement-windows-store-upgrade-procedure.md)
