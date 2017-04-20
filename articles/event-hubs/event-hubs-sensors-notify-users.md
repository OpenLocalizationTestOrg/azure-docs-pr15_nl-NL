<properties 
   pageTitle="Gebruikers van de gegevens van sensoren of andere systemen ontvangen een melding | Microsoft Azure"
   description="Beschreven hoe u de gebeurtenis Hubs gebruiken om gebruikers sensorgegevens te waarschuwen."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Gebruikers van de gegevens van sensoren of andere systemen ontvangen een melding

Stel dat u hebt een toepassing die gegevens in realtime bewaakt of rapporten volgens een schema oplevert. Als u de website waarop deze realtime grafieken of rapporten worden weergegeven bekijkt, ziet u mogelijk iets die worden uitgevoerd moeten. Wat gebeurt er als u moet zijn gewaarschuwde in sommige gevallen, maar niet bij te onthouden om te controleren van de website? Stel dat u een light vergroten in een vergunning hebt en u weten onmiddellijk moet als de light uitvalt. Een manier om te doen zou zijn met een lichtsensor in de handel, schikken voor een e-mailbericht verzonden als de light uitgeschakeld is.

![][1]

Stel dat u een huisdier instapweigering faciliteit uitgevoerd en u moet worden gewaarschuwd voor lage voorraad levering niveaus in een ander scenario. U mogelijk bijvoorbeeld rangschikken verzonden SMS-bericht van uw ERP-systeem als uw magazijnvoorraad van eten naar een kritieke niveau ligt. 

![][2]

Hoe kom ik aan cruciale informatie wanneer bepaalde voorwaarden is voldaan, niet wanneer u een statische rapport uitchecken rond openen, is het probleem. Als u een [Azure gebeurtenis Hub][] of [Azure IoT Hub][] gebruikt om gegevens te ontvangen van apparaten of enterprise-toepassingen zoals [Dynamics AX][], hebt u verschillende opties voor het verwerken van deze. U deze kunt weergeven op een website, kunt u deze analyseren, kunt u ze hebt opgeslagen en u ze kunt gebruiken om te activeren opdrachten wilt hebben. Hiervoor kunt u krachtige hulpprogramma's zoals [Azure Websites][], [SQL Azure wordt aangegeven][], [HDInsight][], [Cortana Intelligence Suite][], [IoT Suite][], [Logica Apps][]of [Azure melding Hubs][]. Maar soms enige u wilt doen, is deze gegevens verzenden naar iemand met een minimum aan realiseren. Als u wilt laten zien hoe u dit doen met alleen redelijk code, hebt we een nieuwe steekproef, [AppToNotifyUsers][]opgegeven. Opties die worden geleverd zijn e-mail (SMTP), SMS en telefoon.

## <a name="application-structure"></a>Toepassingsstructuur

De toepassing is geschreven in C# en het Leesmij-bestand in de steekproef bevat alle informatie die u wilt wijzigen, maken en publiceren van de toepassing. De volgende secties bevatten een overzicht van de werking van de toepassing.

We beginnen met aangenomen dat er kritieke gebeurtenissen aan een Azure gebeurtenis Hub of IoT Hub wordt gedrukt. Een hub doet, zo lang maken als u toegang tot deze hebt en de verbindingsreeks kent.

Als u nog een gebeurtenis Hub of IoT-hub, kunt u eenvoudig een pot test met een schild Arduino en een Pi frambozen, volgens de instructies in het project [De puntjes verbinding](https://github.com/Azure/connectthedots) instellen. De lichtsensor op het schild Arduino light niveaus tot en met de Pi verzendt naar een [Azure gebeurtenis Hub][] (**ehdevices**) en een taak [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) worden waarschuwingen op een tweede gebeurtenis hub (**ehalerts**) als de light niveaus ontvangen onder een bepaald niveau vallen.

Wanneer **AppToNotify** wordt gestart, wordt een configuratiebestand (App.config) om de URL en de referenties voor de gebeurtenis Hub de meldingen ontvangen gelezen. Deze vervolgens Hiermee wordt een proces om de continu dat gebeurtenis Hub voor elk bericht dat afkomstig is tot en met – zo lang maken als u hebt toegang tot de URL voor de gebeurtenis Hub of IoT hub en geldige referenties, dat deze gebeurtenis Hubs reader code leest continu wat er nieuw te houden. Tijdens het opstarten leest de toepassing ook de URL en de referenties voor de SMS-berichten service (e-mail, SMS, telefoon) die u wilt gebruiken, en de naam/adres van de afzender en een lijst met geadresseerden.

Zodra de monitor gebeurtenis Hub wordt gedetecteerd dat een bericht, wordt er een proces dat die met de methode die is opgegeven in het configuratiebestand bericht verzendt. Houd er rekening mee dat deze wordt verzonden elk bericht dat er wordt gedetecteerd. Als u het beeldscherm zodat deze verwijzen naar de Hub van een gebeurtenis die de tien berichten per seconde ontvangt, tien berichten per seconde – tien e-mailberichten per seconde tien SMS-berichten per seconde tien telefoongesprekken per seconde worden verzonden door de afzender. Zorg ervoor dat u de Hub van een gebeurtenis die alleen ontvangt de meldingen die moeten worden verzonden, niet in het geval van een gebeurtenis-Hub die ontvangt van alle onbewerkte gegevens van uw sensoren of toepassingen controleren voor daarom.

## <a name="applicability"></a>Toepassing

De code in dit voorbeeld ziet u alleen het controleren van de gebeurtenis Hubs en hoe externe berichtenservices bellen in het geval dat u wilt deze functionaliteit toevoegen aan uw toepassing. Houd er rekening mee dat deze oplossing een DIY, voorbeeld ontwikkelaars gerichte alleen is. Deze verhelpt niet enterprise vereisten, zoals redundantie, storing, opnieuw starten na het mislukt, enzovoort. Zie de volgende onderwerpen voor meer oplossingen voor uitgebreide en productie:

- Gebruik verbindingslijnen of push-meldingen met de [Logica-Apps Azure](../app-service-logic/app-service-logic-connectors-list.md) -service.
- Gebruik van [Azure melding Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx), volgens de beschrijving van de blog [uitzending push-meldingen voor mobiele apparaten met Azure melding Hubs miljoenen](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Volgende stappen

Het is eenvoudig om het maken van een eenvoudige berichtenservice die e-mailberichten of SMS-berichten naar geadresseerden verzendt, of ze, met relay gegevens ontvangen door een gebeurtenis Hub of IoT Hub-oproepen. Als u wilt de melding van de gebruikers op basis van gegevens die zijn ontvangen door deze hubs-oplossing implementeert, gaat u naar [AppToNotifyUsers][].

Zie de volgende artikelen voor meer informatie over deze hubs:

- [Azure gebeurtenis Hubs]
- [Azure IoT Hub]
- Aan de slag met een [gebeurtenis Hubs zelfstudie].
- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs].

Als u wilt de melding van de gebruikers op basis van gegevens die zijn ontvangen door deze hubs-oplossing implementeert, gaat u naar:

- [AppToNotifyUsers][]

[Gebeurtenis Hubs zelfstudie]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT Hub]: https://azure.microsoft.com/services/iot-hub/
[Azure gebeurtenis Hubs]: https://azure.microsoft.com/services/event-hubs/
[Azure gebeurtenis Hub]: https://azure.microsoft.com/services/event-hubs/
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure-Websites]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure wordt aangegeven]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana Intelligence-Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[Logica Apps]: https://azure.microsoft.com/services/app-service/logic/
[Azure melding Hubs]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png