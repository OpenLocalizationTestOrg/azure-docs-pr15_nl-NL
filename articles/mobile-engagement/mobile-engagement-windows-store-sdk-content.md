<properties 
    pageTitle="Windows universele Apps SDK inhoud" 
    description="Meer informatie over de inhoud van de Windows universele Apps SDK voor Azure Mobile betrokkenheid"                    
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-content"></a>Windows universele Apps SDK inhoud

In dit document bevat en een beschrijving van de inhoud die wordt geïmplementeerd door de SDK in uw toepassing.

##<a name="the-resources-folder"></a>De `/Resources` map

Deze map bevat alle resources die Mobile betrokkenheid nodig heeft. U kunt ook aanpassen aan uw app.

- `EngagementConfiguration.xml`: Van de Mobile-betrokkenheid configuratiebestand, dit is waar u kunt Mobile betrokkenheid instellingen (verbindingsreeks Mobile betrokkenheid, rapport vastlopen...).

### <a name="html-folder"></a>de map/HTML

- `EngagementNotification.html`: De `Notification` weergave HTML-ontwerp voor app-spandoek met webonderdelen.

- `EngagementAnnouncement.html`: De `Announcement` webontwerp weergave html voor app verspreide weergaven.

### <a name="images-folder"></a>de map/images

- `EngagementIconNotification.png`: Het pictogram huisstijl aan de linkerkant van een melding vervanging van dit door een pictogram voor uw huisstijl.

- `EngagementIconOk.png`: De `Ok` pictogram van het bereik inhoudspagina's voor de knop actie of gegevensvalidatie.

- `EngagementIconNOK.png`: De `NOK` pictogram dat wordt gebruikt als de knop validatie bereik inhoudspagina's is uitgeschakeld.
 
- `EngagementIconClose.png`: De `Close` pictogram van het bereik meldingen en de inhoud voor de knop verwijderen klikt.

### <a name="overlay-folder"></a>/overlay map

- `EngagementPageOverlay.cs`: De verantwoordelijk voor het toevoegen van de betrokkenheid overlay-pagina hebt bereikt app UI aan het kind.
  
