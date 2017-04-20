<properties
    pageTitle="Geavanceerde configuratie voor Azure Mobile betrokkenheid Android SDK"
    description="Beschrijving van de geavanceerde configuratie-opties met inbegrip van de Android bestandenlijst met Azure Mobile betrokkenheid Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Geavanceerde configuratie voor Azure Mobile betrokkenheid Android SDK

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-advanced-configuration.md)

Deze procedure wordt beschreven hoe u verschillende configuratieopties voor Android betrokkenheid van Azure mobiele apps configureren.

## <a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Vereisten voor machtigingen
Specifieke machtigingen, die hier staan vermeld voor verwijzing en in-regel in de functie voor specifieke vereist zijn bepaalde opties. Deze machtigingen toevoegen aan de AndroidManifest.xml van uw project direct voor of na de `<application>` tag.

De machtiging code moet er als volgt te werk waar u de juiste machtigingen van de onderstaande tabel invullen.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Machtiging | Wanneer gebruikt |
| ---------- | --------- |
| INTERNET | Vereist. Voor eenvoudige rapporten |
| ACCESS_NETWORK_STATE | Vereist. Voor eenvoudige rapporten |
| RECEIVE_BOOT_COMPLETED | Vereist. Naar het beheercentrum meldingen worden weergegeven na het apparaat opnieuw starten |
| WAKE_LOCK | Aanbevolen. Hiermee verzamelen van gegevens bij gebruik van Wi-Fi of als het scherm is uitgeschakeld |
| TRILLEN | Optioneel. Trillingen kunt wanneer meldingen worden ontvangen |
| DOWNLOAD_WITHOUT_NOTIFICATION | Optioneel. Android totaalbeeld melding kunt |
| WRITE_EXTERNAL_STORAGE | Optioneel. Android totaalbeeld melding kunt |
| ACCESS_COARSE_LOCATION | Optioneel. Schakelt realtime locatie rapportage |
| ACCESS_FINE_LOCATION | Optioneel. Mogelijk maakt op basis van GPS locatie rapportage |

Beginnen met Android M, [Sommige machtigingen tijdens runtime worden beheerd](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Als u al werkt met ``ACCESS_FINE_LOCATION``, moet u niet ook gebruiken ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android Manifest configuratieopties

### <a name="crash-report"></a>Rapport vastlopen

Toevoegen als u wilt uitschakelen foutenrapporten, deze code tussen de `<application>` en `</application>` labels:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Drempelwaarde voor burst

Standaard logboeken op de betrokkenheid service-rapporten in realtime. Als uw toepassing rapport logboeken vaak variëren, is het beter in de buffer de logboeken en om ze allemaal tegelijk op een grondtal normale werktijd ('Burstmodus' genoemd). Klik hiervoor toevoegen u deze code tussen de `<application>` en `</application>` labels:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burst-modus wordt de levensduur enigszins, maar meteen invloed heeft op de Monitor betrokkenheid: alle sessies en taken duur worden afgerond op de drempelwaarde voor burst (dus sessies en taken korter dan de drempelwaarde voor burst zijn mogelijk niet zichtbaar). Drempelwaarde voor uw burst moet niet meer dan 30000 (30s).

### <a name="session-timeout"></a>Sessietime-out

 U kunt een activiteit beëindigen door het drukken op de toets **Home** of **terug** door in te stellen van de telefoon niet actief geweest of door te gaan in een andere toepassing. Standaard wordt een sessie tien seconden na het einde van de laatste activiteit beëindigd. Zo voorkomt u een sessie gesplitste telkens wanneer de gebruiker heeft afgesloten en keert terug naar de toepassing snel, waarin kunt opgetreden wanneer de gebruiker een afbeelding, hervat gecontroleerd of er een melding, enzovoort. U kunt deze parameter wijzigen. Klik hiervoor toevoegen u deze code tussen de `<application>` en `</application>` labels:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Log melden uitschakelen

### <a name="using-a-method-call"></a>Middel van een methode-gesprek

Als u betrokkenheid om te stoppen met het verzenden van Logboeken wilt, kunt u bellen:

    EngagementAgent.getInstance(context).setEnabled(false);

In dit gesprek is permanente: site gebruikmaakt van een gedeelde voorkeurenbestand.

Als wanneer u deze functie belt betrokkenheid actief is, duurt één minuut voor de service stoppen. Maar deze starten de service helemaal de volgende keer dat Won't starten u de toepassing.

Kunt u rapportage opnieuw door te bellen met dezelfde functie log inschakelen `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>-Integratie in uw eigen`PreferenceActivity`

In plaats van oproepen van deze functie kunt u ook deze instelling integreren rechtstreeks in uw bestaande `PreferenceActivity`.

U kunt betrokkenheid om uw voorkeurenbestand (met de gewenste modus) te gebruiken de `AndroidManifest.xml` bestand met `application meta-data`:

-   De `engagement:agent:settings:name` sleutel wordt gebruikt voor de naam van het gedeelde voorkeurenbestand definiëren.
-   De `engagement:agent:settings:mode` sleutel wordt gebruikt voor het definiëren van de modus van het voorkeurenbestand gedeelde. Dezelfde gebruikt als in uw `PreferenceActivity`. De modus moet worden doorgegeven als een getal: als u een combinatie van constante vlaggen in uw code gebruikt, controleert u de totale waarde.

Altijd betrokkenheid gebruikt de `engagement:key` Booleaanse toets binnen het voorkeurenbestand voor het beheren van deze instelling.

Het volgende voorbeeld van `AndroidManifest.xml` ziet u de standaardwaarden:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

U kunt toevoegen een `CheckBoxPreference` in de indeling van uw voorkeur zoals de volgende:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
