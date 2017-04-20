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
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>ADM integreren met betrokkenheid

> [AZURE.IMPORTANT] U moet de integratie procedure wordt beschreven in de How to betrokkenheid integreren op Android document voordat u deze handleiding volgen.
>
> Dit document is alleen nuttig als u al zijn geïntegreerd met het bereik module en plan push Amazon-apparaten. Als u wilt bereiken campagnes in uw toepassing integreren, lees eerst integreren betrokkenheid bereiken op android-apparaat.

##<a name="introduction"></a>Inleiding

Integratie van ADM kan de toepassing moet worden geplaatst wanneer doelgroepen Amazon Android-apparaten.

ADM nettoladingen verplaatst naar de SDK altijd bevatten de `azme` key in het gegevensobject. Dus als u in uw toepassing ADM voor andere doeleinden gebruikt, kunt u dit gereedschap duwt op basis van die toets filteren.

> [AZURE.IMPORTANT] Alleen Amazon Kindle apparaten met Android 4.0.3 of hoger worden ondersteund door Amazon apparaat verzenden van berichten. u kunt echter deze code veilig op andere apparaten integreren.

##<a name="sign-up-to-adm"></a>Aanmelden voor ADM

Als u niet hebt gedaan, moet u ADM inschakelen op uw Amazon-account.

De procedure is gedetailleerde op: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Na het voltooien van de procedure, krijgt u de:

-   OAuth (een Client-ID en een geheim Client) van de referenties in voor betrokkenheid moeten kunnen push uw apparaten.
-   Een API-sleutel die moeten worden geïntegreerd in uw toepassing.

##<a name="sdk-integration"></a>SDK-integratie

### <a name="managing-device-registrations"></a>Apparaat registraties beheren

Elk apparaat moet een opdracht registratiegegevens verzenden met de servers ADM, anders kan ze kunnen niet worden bereikt.

Als u al de [ADM clientbibliotheek gebruikt], en al [geïntegreerde ADM hebt] kunt u rechtstreeks naar de android-sdk-adm-ontvangen gaan.

Als u nog niet ADM geïntegreerd, heeft betrokkenheid eenvoudiger te schakelen in uw toepassing:

Bewerken uw `AndroidManifest.xml` bestand:

-   De naamruimte Amazon toevoegen het bestand moet beginnen als volgt:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Binnen de `<application/>` markeren en deze sectie toevoegen:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Na het toevoegen van de tag amazon, u moet mogelijk een fout opbouwen als uw Project bouwen doel onder Android 2.1. U moet een **Android 2.1 +** opbouwen doel gebruiken (geen probleem, kunt u nog steeds beschikken over een `minSdkVersion` ingesteld op 4).
-   De ADM API-sleutel als activa integreren door de volgende [deze procedure].

Volg de instructies van de volgende secties.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registratie-id om de betrokkenheid Push-service te communiceren en meldingen ontvangen

Toevoegen de volgende handelingen uit om te voldoen om te communiceren van de registratie-id van het apparaat dat bij de betrokkenheid Push-service en de meldingen ontvangen, uw `AndroidManifest.xml` bestand, binnen de `<application/>` markeren (ook als u ADM zonder betrokkenheid gebruiken):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Controleer of u de volgende machtigingen hebben uw `AndroidManifest.xml` (vóór de `</application>` tag).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Verlenen betrokkenheid OAuth-referenties

Uw referenties OAuth (cliënt-ID en geheim Client) in de Portal betrokkenheid verzenden.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM client-bibliotheek]:https://developer.amazon.com/sdk/adm/setup.html
[geïntegreerde ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[deze procedure]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
