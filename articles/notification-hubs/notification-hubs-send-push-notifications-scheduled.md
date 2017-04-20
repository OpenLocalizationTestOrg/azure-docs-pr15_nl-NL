<properties
    pageTitle="Het verzenden van geplande meldingen | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe meldingen gepland met Azure melding Hubs."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="push-meldingen, push-bericht, planning push-meldingen"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Procedure: Geplande meldingen verzenden


##<a name="overview"></a>Overzicht

Als u een scenario waarin u wilt een melding in de toekomst verzenden op een gegeven moment, maar ik heb een eenvoudige manier om te activeren van uw back-enddatabase-code aan wie de melding hebt. Standaard laag melding Hubs ondersteunt een functie waarmee u meldingen omhoog naar 7 dagen in de toekomst plannen.

Gebruiken bij het verzenden een melding gewoon de klasse [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) in de melding Hubs SDK zoals wordt weergegeven in het volgende voorbeeld:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

U kunt ook een eerder geplande melding met de notificationId opzeggen:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Er zijn geen beperkingen ten aanzien van het aantal geplande meldingen die u kunt verzenden.