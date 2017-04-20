<properties 
    pageTitle="Windows Phone Silverlight SDK-overzicht" 
    description="Overzicht van de Windows Phone Silverlight SDK voor Azure mobiele betrokkenheid"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK overzicht voor Azure mobiele betrokkenheid

Begin hier om de details over het integreren van Azure Mobile betrokkenheid in een Windows Phone Silverlight-App. Als u wilt zelf proberen eerst, controleert u of u onze [15 minuten zelfstudie](mobile-engagement-windows-phone-get-started.md)hebt voltooid.

Klik op om de [SDK inhoud](mobile-engagement-windows-phone-sdk-content.md) weer te geven

##<a name="integration-procedures"></a>Integratie van procedures

1. Begin hier: [het integreren van Mobile betrokkenheid in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-integrate-engagement.md)

2. Voor meldingen: [het integreren van bereiken (meldingen) in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Een melding geven voor de implementatie plannen: [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Releaseopmerkingen

###<a name="330-04192016"></a>3.3.0 (04/19/2016)
Deel van de nuget *MicrosoftAzure.MobileEngagement* inpakken **v3.4.0**

-   Toegevoegde "TestLogLevel" API inschakelen/uitschakelen/filter console logboeken dat door de SDK.

Zie de [volledige releaseopmerkingen](mobile-engagement-windows-phone-release-notes.md) voor eerdere versie

##<a name="upgrade-procedures"></a>De upgrade procedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

U moet mogelijk verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Zie de volledige [Procedures upgraden](mobile-engagement-windows-phone-upgrade-procedure.md). Als u migreert van 0.10.1 naar 0.11.0 moet u eerst de procedure 'van 0.9.0 naar 0.10.1"Volg vervolgens de procedure 'van 0.10.1 naar 0.11.0' bijvoorbeeld.

###<a name="from-200-to-330"></a>Uit 2.0.0 naar 3.3.0

####<a name="test-logs"></a>Test-Logboeken

Console-logboeken geproduceerd door de SDK kunnen nu zijn ingeschakeld/uitgeschakeld/gefilterd. U kunt u dit aanpassen door de eigenschap bijwerken `EngagementAgent.Instance.TestLogEnabled` naar een van de waarde die verkrijgbaar is bij het `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Een upgrade uitvoert vanuit oudere versies

Zie [Procedures een upgrade uitvoeren](mobile-engagement-windows-phone-upgrade-procedure.md)
 
