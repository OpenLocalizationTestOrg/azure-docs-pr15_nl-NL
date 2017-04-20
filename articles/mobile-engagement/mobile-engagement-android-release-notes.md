<properties
    pageTitle="Azure mobiele betrokkenheid Android SDK-integratie"
    description="Meest recente updates en procedures voor Android SDK voor Azure Mobile betrokkenheid"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Releaseopmerkingen

## <a name="423-08102016"></a>4.2.3 (10-08/2016)

- Geen meer Wi-Fi-vergrendelen.
- Een deadlock oplossen bij het aanroepen van getDeviceId vóór initialisatie (bug geïntroduceerd in 4.2.0).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)

- Verbeteringen in stabiliteit.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)

- Beveiliging: web weergave lokaal bestandstoegang uitschakelen.
- Beveiliging: verwijderen `EngagementPreferenceActivity` klasse die verouderde en onbeveiligde uitbreidt `PreferenceActivity` class.
- Beveiliging: bereik activiteiten nu beschreven worden als u wilt gebruiken `exported="false"`, deze vlag kan ook worden gebruikt in eerdere versies van SDK.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)

- De SDK is nu gelicentieerd onder MIT.
- Toestaan dat een aangepaste apparaat-id opgeven SDK initialisatie tegelijk.

## <a name="415-02012016"></a>4.1.5 (01-02-2016)

- Verbeteringen in stabiliteit.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)

- Verbeteringen in stabiliteit.

## <a name="413-1292015"></a>4.1.3 (9-12-2015)

- Verbeteringen in stabiliteit.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)

- Verbeteringen in stabiliteit.

## <a name="411-11042015"></a>4.1.1 (11 04--2015)

- Verbeteringen in stabiliteit.

## <a name="410-08252015"></a>4.1.0 (25-08-2015)

- Nieuwe machtiging model voor Android M verwerken.
- Kan nu locatie functies configureren gedurende runtime in plaats van `AndroidManifest.xml`.
- Een fout machtiging oplossen: als u `ACCESS_FINE_LOCATION`, klikt u vervolgens `ACCESS_COARSE_LOCATION` is niet meer nodig hebt.
- Verbeteringen in stabiliteit.

## <a name="400-07062015"></a>4.0.0 (07-06/2015)

-   Interne protocol wijzigingen om analytics en push meer betrouwbare.
-   Systeemeigen push (GCM/ADM) is nu ook gebruikt voor in de app-meldingen zodat u de systeemeigen push-referenties voor elk gewenst type push campaign moet configureren.
-   Totaalbeeld melding oplossen: matrixgegevens weergegeven alleen 10s na daadwerkelijk wordt ingedrukt.
-   Een fout in de weergave oplossen: te klikken op een koppeling, is de URL van de actie standaard ook uitvoeren.
-   Verhelp niet vaak voorkomen vastlopen betrekking hebben op de lokale opslagbeheer.
-   Beheer van de tekenreeks dynamische configuratie oplossen.
-   Gebruiksrechtovereenkomst bijwerken.

## <a name="300-02172015"></a>3.0.0 (17-02-2015)

-   Eerste versie van Azure mobiele betrokkenheid
-   configuratie van toepassings-id wordt vervangen door de configuratie van een verbinding tekenreeks.
-   API verzenden en ontvangen van willekeurige XMPP berichten van willekeurige XMPP entiteiten verwijderd.
-   API verzenden en ontvangen van berichten tussen apparaten verwijderd.
-   Verbeteringen in de beveiliging.
-   Google Play en SmartAd bijgehouden verwijderd.
