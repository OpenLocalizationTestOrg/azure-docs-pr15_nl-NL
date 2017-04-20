<properties
    pageTitle="Integratie van android SDK voor Azure mobiele betrokkenheid"
    description="Wordt beschreven hoe u het integreren van Azure Mobile betrokkenheid SDK in Android-apps"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Integratie van android SDK voor Azure mobiele betrokkenheid

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android-apparaat](mobile-engagement-android-sdk-overview.md)

In dit document worden alle de integratie en configuratie van beschikbare opties voor Azure Mobile betrokkenheid Android SDK.

## <a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Geavanceerde functies

### <a name="reporting-features"></a>Rapportfuncties

U kunt deze functies toevoegen:

1. [Geavanceerde opties voor rapportage](mobile-engagement-android-advanced-reporting.md)
2. [Locatie-opties voor rapportage](mobile-engagement-android-location-reporting.md)
3. [Geavanceerde configuratie-opties](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Meldingen:
[Het bereik (meldingen) integreren in uw Android-app](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [het integreren van GCM met mobiele betrokkenheid](mobile-engagement-android-gcm-integrate.md)

2. Amazon apparaat Messaging (ADM): [het integreren van ADM met mobiele betrokkenheid](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Tag abonnement-implementatie:
[Het gebruik van de geavanceerde Mobile betrokkenheid API tags in uw Android-app](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="423-08102016"></a>4.2.3 (10-08/2016)

 - Geen meer Wi-Fi-vergrendelen.
 - Een deadlock oplossen bij het aanroepen van getDeviceId vóór initialisatie (bug geïntroduceerd in 4.2.0).

Zie de [volledige releaseopmerkingen](mobile-engagement-android-release-notes.md)voor alle versies.

## <a name="upgrade-procedures"></a>De upgrade procedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, raadpleegt u [Procedures upgraden](mobile-engagement-android-upgrade-procedure.md).
