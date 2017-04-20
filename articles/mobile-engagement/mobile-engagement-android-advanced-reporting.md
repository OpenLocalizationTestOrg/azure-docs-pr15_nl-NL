<properties
    pageTitle="Geavanceerde opties voor rapportage voor Azure Mobile betrokkenheid Android SDK"
    description="Wordt uitgelegd hoe u moet doen geavanceerde rapportage om vast te leggen analytics voor Azure Mobile betrokkenheid Android SDK"
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
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Geavanceerde rapportage met betrokkenheid op android-apparaat

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-advanced-reporting.md)

In dit onderwerp wordt beschreven extra rapportage-scenario's in uw Android-toepassing. U kunt deze opties toepassen op de app die is gemaakt in de handleiding aan de [Slag](mobile-engagement-android-get-started.md) .

## <a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

De zelfstudie u voltooid is opzettelijk directe en eenvoudige, maar er zijn geavanceerde opties die u kunt kiezen.

## <a name="modifying-your-activity-classes"></a>Wijzigen van uw `Activity` klassen

In deze [zelfstudie aan de slag](mobile-engagement-android-get-started.md), alle u had moet doen om te is uw `*Activity` subcategorieën overnemen van de bijbehorende `Engagement*Activity` klassen. Als uw oudere activiteiten uitgebreide bijvoorbeeld `ListActivity`, brengt u deze uitbreiden `EngagementListActivity`.

> [AZURE.IMPORTANT] Bij gebruik van `EngagementListActivity` of `EngagementExpandableListActivity`, moet u ervoor zorgen dat een bellen naar `requestWindowFeature(...);` bestaat uit de voordat u de oproep door naar `super.onCreate(...);`, anders een fout optreedt.

U vindt deze klassen in de `src` map, en ze in uw project kunt kopiëren. De klassen zijn ook in de **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatieve methode: bellen `startActivity()` en `endActivity()` handmatig

Als u niet kunt of niet wilt overbelastingen uw `Activity` klassen, u kunt in plaats daarvan starten en te beëindigen van uw activiteiten door de ondersteuning voor de `EngagementAgent`van rechtstreeks methoden.

> [AZURE.IMPORTANT] De Android SDK belt nooit de `endActivity()` methode, zelfs wanneer de toepassing is gesloten (op android-apparaat, toepassingen zijn nooit heb afgesloten). Het is dus *ten zeerste* aanbevolen om te bellen de `startActivity()` methode in de `onResume` terugbellen van *alle* uw activiteiten en de `endActivity()` methode in de `onPause()` terugbellen van *alle* uw activiteiten. Dit is de enige manier om ervoor te zorgen dat sessies niet meer zijn. Als een sessie is meer, wordt de service betrokkenheid nooit verbroken van de betrokkenheid-end (sinds de service verbonden blijft zo lang maken als een sessie in behandeling is).

Hier volgt een voorbeeld:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

In dit voorbeeld is vergelijkbaar met de `EngagementActivity` klasse en de bijbehorende varianten, waarvan de broncode is opgegeven in de `src` map.

## <a name="using-applicationoncreate"></a>Application.onCreate() gebruiken

Een code, die u in plaatsen `Application.onCreate()` en in andere toepassing terugbellen voor alle aan uw toepassing processen, inclusief de service betrokkenheid wordt uitgevoerd. Deze mogelijk ongewenste kant effecten, zoals overbodige geheugentoewijzingen en threads in van de betrokkenheid proces, of dubbele broadcast ontvangers of services.

Als u negeren `Application.onCreate()`, wordt aangeraden het volgende codefragment toevoegen aan het begin van uw `Application.onCreate()` functie:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

U kunt hetzelfde doen voor `Application.onTerminate()`, `Application.onLowMemory()`, en `Application.onConfigurationChanged(...)`.

U kunt ook uitbreiden `EngagementApplication` in plaats van uitbreiden `Application`: de terugbellen `Application.onCreate()` heeft het proces selectievakje en oproepen `Application.onApplicationProcessCreate()` alleen als het huidige proces degene die de betrokkenheid hostingservice is, dezelfde regels voor de andere terugbellen gelden.

## <a name="tags-in-the-androidmanifestxml-file"></a>Codes in het bestand AndroidManifest.xml

In de service-code in het bestand AndroidManifest.xml, de `android:label` kenmerk kunt u de naam van de service betrokkenheid kiezen, zoals deze wordt weergegeven aan eindgebruikers in het scherm 'Services uitgevoerd' van hun telefoon. Het is raadzaam dit kenmerk instellen voor het `"<Your application name>Service"` (bijvoorbeeld `"AcmeFunGameService"`).

Geven de `android:process` kenmerk zorgt ervoor dat de betrokkenheid-service wordt uitgevoerd in een eigen proces (betrokkenheid uitgevoerd in hetzelfde proces terwijl uw toepassing zorgt ervoor dat uw Hoofdgegeven/UI-thread potentieel minder heeft gereageerd).

## <a name="building-with-proguard"></a>Bouwen met ProGuard

Als u uw toepassingspakket met ProGuard samenstelt, moet u sommige klassen behouden. U kunt het volgende configuratie-fragment gebruiken:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
