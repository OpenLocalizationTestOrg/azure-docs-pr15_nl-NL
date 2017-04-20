<properties 
    pageTitle="Windows Phone Silverlight SDK Upgrade Procedures" 
    description="Windows Phone Silverlight SDK Upgrade Procedures voor Azure mobiele betrokkenheid"                  
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

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK Upgrade Procedures

Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

U moet mogelijk verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Als u migreert van 0.10.1 naar 0.11.0 moet u eerst de procedure 'van 0.9.0 naar 0.10.1"Volg vervolgens de procedure 'van 0.10.1 naar 0.11.0' bijvoorbeeld.

##<a name="from-200-to-330"></a>Uit 2.0.0 naar 3.3.0

### <a name="test-logs"></a>Test-Logboeken

Console-logboeken geproduceerd door de SDK kunnen nu zijn ingeschakeld/uitgeschakeld/gefilterd. U kunt u dit aanpassen door de eigenschap bijwerken `EngagementAgent.Instance.TestLogEnabled` naar een van de waarde die verkrijgbaar is bij het `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Uit 1.1.1 naar 2.0.0

Het volgende beschreven hoe u het migreren van een SDK-integratie van de service van Capptain SAS Capptain in een app mogelijk gemaakt door Azure Mobile betrokkenheid. 

> [Azure.IMPORTANT] Capptain en Mobile betrokkenheid zijn niet dezelfde services en het migreren van de client-app in de onderstaande procedure alleen worden gemarkeerd. Migreren van de SDK in de app migreert geen gegevens van de Capptain-servers met de servers Mobile betrokkenheid

Als u vanuit een eerdere versie migreert, raadpleegt u de website Capptain om te migreren naar 1.1.1 eerst de volgende procedure toepassen

### <a name="nuget-package"></a>Nuget pakket

Vervang **Capptain.WindowsPhone** door **MicrosoftAzure.MobileEngagement** Nuget-pakket.

### <a name="applying-mobile-engagement"></a>Mobiele betrokkenheid toepassen

De SDK wordt de term `Engagement`. U moet bijwerken van uw project zodat deze overeenkomen met deze wijziging.

U moet verwijderen van uw huidige Capptain nuget-pakket. Houd rekening met dat alle wijzigingen in de map Capptain bronnen worden verwijderd. Als u wilt behouden Breng deze bestanden vervolgens een kopie van deze.

Installeer daarna het nieuwe Microsoft Azure Engagement nuget pakket aan uw project. U vindt deze rechtstreeks op [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Deze actie vervangt alle resources-bestanden die worden gebruikt door betrokkenheid en wordt de nieuwe betrokkenheid DLL toegevoegd aan uw projectverwijzingen.

U moet uw projectverwijzingen wissen door te Capptain DLL verwijzingen verwijderen. Als u dit niet doet, wordt de versie van Capptain conflicteert en fouten er gebeurt.

Als u Capptain resources hebt aangepast, wordt de inhoud van uw oude bestanden kopiëren en plak deze in de nieuwe betrokkenheid-bestanden. Houd er rekening mee dat zowel xaml en cs bestanden moeten worden bijgewerkt.

Wanneer u klaar bent met deze stappen hebt u alleen voor het vervangen van oude Capptain verwijzingen door de nieuwe betrokkenheid verwijzingen.

1. Alle Capptain naamruimten moeten worden bijgewerkt.

    Voor de migratie:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Na de migratie:
    
        using Microsoft.Azure.Engagement;

2. Alle Capptain klassen met 'Capptain' moeten 'Betrokkenheid' bevatten.

    Voor de migratie:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Na de migratie:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Voor xaml-bestanden wijzigen Capptain naamruimte en kenmerken ook.

    Voor de migratie:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Na de migratie:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Voor de andere bronnen zoals Capptain afbeeldingen, houd er rekening mee dat ze ook hebt gekregen om te gebruiken "Betrokkenheid".

### <a name="application-id--sdk-key"></a>Toepassings-ID / SDK-toets

Betrokkenheid gebruikt een verbindingsreeks. U hoeft te geven van een toepassing-ID en een sleutel SDK met mobiele betrokkenheid, hoeft u alleen te geven van een verbindingsreeks. U kunt instellen deze aan uw bestand EngagementConfiguration.

De configuratie betrokkenheid kan worden ingesteld in uw `Resources\EngagementConfiguration.xml` bestand van uw project.

Bewerken dit bestand om op te geven:

-   De verbindingsreeks van de toepassing tussen labels `<connectionString>` en `<\connectionString>`.

Als u deze in plaats daarvan gedurende runtime opgeeft wilt, kunt u de volgende methode voordat de initialisatie van de agent betrokkenheid bellen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="items-name-change"></a>Items naam wijzigen

Alle items met de naam *capptain* hebben *betrokkenheid*naam gekregen. Op dezelfde manier voor *Capptain* naar *betrokkenheid*.

Voorbeelden van veelgebruikte Capptain items:

-   CapptainConfiguration EngagementConfiguration nu de naam
-   CapptainAgent EngagementAgent nu de naam
-   CapptainReach EngagementReach nu de naam
-   CapptainHttpConfig EngagementHttpConfig nu de naam
-   GetCapptainPageName GetEngagementPageName nu de naam

Houd er rekening mee dat naam heeft ook gevolgen voor overschreven methoden.



 
