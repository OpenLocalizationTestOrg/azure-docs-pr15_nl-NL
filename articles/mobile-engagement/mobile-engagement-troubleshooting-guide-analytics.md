<properties 
   pageTitle="Azure mobiele betrokkenheid de gids voor probleemoplossing - analyses" 
   description="Problemen met analyses, controle, segmentatie en Dashboard oplossen in Azure Mobile betrokkenheid" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Gids voor probleemoplossing voor analyses, controle, segmentatie en Dashboard problemen

Hieronder vindt u mogelijke problemen optreden bij hoe Azure Mobile betrokkenheid verzamelt voor informatie over uw toepassingen, apparaten en gebruikers.

## <a name="missingdelayed-information"></a>Ontbrekende/uitgestelde informatie

### <a name="issue"></a>Probleem
- Informatie is vertraagd in zichtbaar zijn in analyses, segmentatie of Dashboard.
- Informatie ontbreekt op controle.
- Er ontbreken analyses, segmentatie of Dashboard.
- Kunt u door segmentatie limieten.

### <a name="causes"></a>Oorzaken

- U kunt de analyse-API, Monitor API, en segmenten API om te zien of alle gegevens die u niet aanwezig op de gebruikersinterface via de API's zichtbaar.
- Als de SDK van Azure Mobile betrokkenheid niet correct is geïntegreerd in uw app vervolgens niet mogelijk om informatie in de analyse, segmentatie, controle of Dashboards te zien.
- Segmenten kunnen niet worden gewijzigd nadat ze zijn gemaakt, segmenten kunnen alleen worden "gekloonde" (overgenomen) of "verwijderd" (verwijderd). Segmenten mogen alleen 10 criteria.
- De beste manier om te testen ontbrekende gegevens uit de cmdlets voor controle is een testapparaat instellen, verwijderen en/of de app opnieuw op het testapparaat.
- Informatie wordt elke 24 uur voor analyses, segmentatie of Dashboards vernieuwd.
- Gegevens in nieuwe segmenten kan niet worden weergegeven tot 24 uur nadat ze zijn gemaakt, zelfs als het segment is gebaseerd op eerdere informatie.
- Filteren van gegevens in de gebruikersinterface van analyses, wordt alle voorbeelden van dit type ongeacht de versie van uw app (bijvoorbeeld "loopt" gefilterd op de naam wordt weergegeven uit versie 1 en 2-versie van uw app) weergegeven.
- De periode van analysegegevens is gebaseerd op de datum van de gebruikers apparaatinstellingen, zodat een gebruiker wiens telefoon is de datum niet correct ingesteld in de verkeerde tijdsperiode weergegeven kan.
- Geen serverzijde gegevens wordt geregistreerd bij het gebruik van de knop ' Test ' verplaatst, gegevens alleen is aangemeld voor reële push campagnes.

## <a name="cant-locate-items-in-ui"></a>Items vinden in de gebruikersinterface niet

### <a name="issue"></a>Probleem
- Niet mogelijk maken op basis van bepaalde ingebouwd segmenten of aangepaste app info markeren criteria.
- Kan niet vinden bepaalde ingebouwde of aangepaste app info markeren criteria in analyses, controle of Dashboards.
- Kan geen interpreteren van de gegevens in analyses, controle, segmentatie of Dashboards.

### <a name="causes"></a>Oorzaken

- Enkele items ingebouwd en app info tags zijn alleen beschikbaar als push criteria, maar mogelijk niet toegevoegd aan een segment of voor iedereen zichtbaar uit analyses, controle of Dashboard. 
- Voor in-en-klare items en app info tags kunnen niet worden toegevoegd aan een segment, moet u naar de instellingenlijst van de criteria in elke campagne doelgroepen om uit te voeren dezelfde functie als hebt samengesteld op basis van een segment.
- Zie de snelmenu's in de secties analyses, controle, segmentatie en Dashboards van de Azure Mobile betrokkenheid-gebruikersinterface voor meer hulp en hoe u informatie.

## <a name="crash-troubleshooting"></a>Vastlopen problemen oplossen

### <a name="issue"></a>Probleem
- Toepassing loopt vast in analyses, controle of Dashboard weergegeven.

### <a name="causes"></a>Oorzaken

- Om op te lossen zorg toepassing loopt vast in analyses, controle of Dashboard gezien ervoor dat de releaseopmerkingen voor bekende problemen met eerdere versies van de SDK controleren.
- Om op te lossen verder uitvoeren toepassing loopt van een gebeurtenis uit een testapparaat instellen met uw toepassing geïnstalleerd en het uiterlijk van uw apparaat-ID in de sectie "Monitor – gebeurtenissen" van de gebruikersinterface van Azure Mobile betrokkenheid. Voer de gebeurtenis dat wordt veroorzaakt door uw toepassing vastlopen en aanvullende informatie in de sectie "Monitor – vastlopen" van de gebruikersinterface van Azure Mobile betrokkenheid opzoeken uit. 

 
