<properties
   pageTitle="Problemen met traag back-up van bestanden en mappen in Azure back-up | Microsoft Azure"
   description="Bevat voor probleemoplossing richtlijnen waarmee u een diagnose stellen bij de oorzaak van Azure back-up prestatieproblemen"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problemen met traag back-up van bestanden en mappen in Azure back-up

In dit artikel bevat voor probleemoplossing richtlijnen waarmee u een diagnose stellen bij de oorzaak van back-up vertragingen voor bestanden en mappen wanneer u Azure back-up gebruikt. Wanneer u de back-Azure-agent gebruikt voor het back-up van bestanden, het back-proces mogelijk langer duren dan verwacht. Dit kan worden veroorzaakt door een of meer van de volgende opties:

-   [Er zijn prestatieproblemen op de computer waarop wordt back-up.](#cause1)
-   [Een ander proces of antivirussoftware verstoort de back-up van Azure-proces.](#cause2)
-   [De back-up-agent wordt uitgevoerd op een Azure virtuele machine (VM).](#cause3)  
-   [U bent een back-up een groot aantal (miljoenen) bestanden.](#cause4)

Voordat u het oplossen van problemen begint, raden die u kunt downloaden en installeren van de [meest recente back-up van Azure-agent](http://aka.ms/azurebackup_agent). We geven veelgebruikte updates naar de back-up-agent verschillende problemen oplossen, functies toe te voegen en de prestaties verbeteren.

Ook wordt aangeraden de [Veelgestelde vragen over back-up van Azure-service](backup-azure-backup-faq.md) om ervoor te zorgen dat u niet een ondervindt van de algemene configuratieproblemen te controleren.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Oorzaak: Prestatieknelpunten op de computer

Knelpunten op de computer waarop wordt back-up kunnen vertragingen opleveren. Bijvoorbeeld knelpunten kan leiden tot de mogelijkheid van de computer te lezen of schrijven op schijf of bandbreedte om gegevens te verzenden via het netwerk.

Windows biedt een ingebouwde hulpprogramma dat [Prestaties Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) om op te sporen deze knelpunten heeft genoemd.

Hier volgen enkele prestatie-items en bereiken die handig is bij het oplossen van knelpunten optimale back-ups.

| Item  | Status  |
|---|---|
|Logische schijf (fysieke schijf)--% inactief   | • 100% idle tot 50% inactief = orde</br>• 49% idle 20% inactief = waarschuwing of Monitor</br>• 19% idle op 0% inactief = Kritiek of niet-specificatie|
|  Logische schijf (fysieke schijf)--% gem. Schijf Sec lezen of schrijven |  • 0,001 ms 0.015 MS = orde</br>• 0.015 ms 0.025 MS = waarschuwing of Monitor</br>• 0.026 ms of meer = Kritiek of niet-specificatie|
|  Logische schijf (fysieke schijf)--huidige lengte (voor alle exemplaren) | 80 verzoeken om meer dan 6 minuten |
| Geheugen--van toepassingen niet geheugen: Bytes|• Minder dan 60% van resourcegroep verbruikt = orde<br>• 61 80% van toepassingen verbruikt = waarschuwing of Monitor</br>• Groter is dan 80% van toepassingen verbruikt = Kritiek of niet-specificatie|
| Geheugen--groep geheugen: Bytes |• Minder dan 60% van resourcegroep verbruikt = orde</br>• 61 80% van toepassingen verbruikt = waarschuwing of Monitor</br>• Groter is dan 80% van toepassingen verbruikt = Kritiek of niet-specificatie|
| Geheugen--beschikbaar Megabytes| • 50% van geheugen beschikbaar of meer = orde</br>• 25% van geheugen beschikbaar = Monitor</br>• 10% van geheugen beschikbaar = waarschuwing</br>• Kleiner is dan 100 MB of 5% van geheugen beschikbaar = Kritiek of niet-specificatie|
|Processor--\%processortijd (alle exemplaren)|• Minder dan 60% verbruikt = orde</br>• 61 90% verbruikt = Monitor of waarschuwing</br>• 91 tot 100% verbruikt = kritiek|


> [AZURE.NOTE] Als u constateert dat de infrastructuur klikken is, wordt u aangeraden dat u de schijven regelmatig voor betere prestaties defragmenteert.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Oorzaak: Een ander proces of antivirussoftware verhinderd back-up van Azure

We hebt verschillende exemplaren waar andere processen in de Windows-bestandssysteem negatief prestaties van het proces van Azure back-up-agent hebt beïnvloed gezien. Als u zowel de back-up van Azure-agent en een ander programma gebruiken back-up van gegevens, of als antivirussoftware wordt uitgevoerd en een slot op bestanden heeft om het back-up worden gemaakt, wordt het veelvoud vergrendelingen op kunnen bestanden bijvoorbeeld conflict veroorzaken. In dit geval de back-up mislukt mogelijk of het project mogelijk langer duren dan verwacht.

Aanbevolen in dit scenario is de andere back-programma's om te zien of de back-tijd voor de back-Azure-agent verandert uitschakelen. Zorg ervoor dat meerdere back-taken niet worden uitgevoerd op hetzelfde moment is meestal voldoende om te voorkomen dat ze elkaar beïnvloeden.

Voor antivirusprogramma, raden we u aan dat u de volgende bestanden en locaties uitsluiten:

- C:\Program Files\Microsoft Azure herstel Services Agent\bin\cbengine.exe als een proces
- C:\Program Files\Microsoft Azure herstel Services Agent\ mappen
- Locatie van krassen (als u niet de standaardlocatie gebruikt)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Oorzaak: Back-up-agent wordt uitgevoerd op een Azure virtuele machines

Als u de back-up-agent in een VM uitvoert, zijn prestaties lager zijn dan wanneer u deze op een fysieke machine uitvoert. Dit is normaal vanwege IO's / s beperkingen.  U kunt echter de prestaties optimaliseren door over te schakelen van de gegevensstations die back-up met Azure Premium Storage gemaakt zijn. We werken aan dit probleem op te lossen en de correctie is beschikbaar in toekomstige versie.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Oorzaak: Een back-up een groot aantal (miljoenen) bestanden

Een grote hoeveelheid gegevens verplaatsen duurt langer dan een kleinere hoeveelheid gegevens verplaatsen. In sommige gevallen is back-up gerelateerd aan niet alleen de grootte van de gegevens, maar ook het aantal bestanden of mappen. Dit geldt met name wanneer miljoenen kleine bestanden (een paar bytes naar een paar kB) zijn back-up gemaakt.

Dit komt omdat terwijl u bent een back-up van de gegevens en verplaatsen naar Azure, Azure tegelijk van de uw bestanden ordenen is. In sommige gevallen wordt niet vaak voorkomen door de catalogusbewerking mogelijk langer duren dan verwacht.

De volgende punten kunt u meer informatie over het knelpunt en hiervan werken met de volgende stappen:

- **UI voortgang voor de gegevensoverdracht wordt weergegeven**. De gegevens worden nog steeds verzonden. De netwerkbandbreedte of de grootte van gegevens veroorzaakt vertragingen.

- **UI, voortgang voor de overdracht van de gegevens niet wordt weergegeven**. Open de logboeken vinden op C:\Microsoft Azure herstel Services Agent\Temp en controleer vervolgens op de vermelding FileProvider::EndData in de logboeken. Dit item geeft aan dat de gegevensoverdracht is voltooid en staat er voor de catalogusbewerking. De back-taken niet worden annuleren. Wacht in plaats daarvan iets meer voor de catalogusbewerking om te voltooien. Als het probleem zich blijft voordoen, neemt u contact [Azure ondersteunen](https://portal.azure.com/#create/Microsoft.Support).
