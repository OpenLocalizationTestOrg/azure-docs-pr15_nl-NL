<properties 
   pageTitle="Releaseopmerkingen voor StorSimple virtuele matrix Updates | Microsoft Azure"
   description="Kritieke openstaande actie-items en oplossingen beschreven voor de StorSimple virtuele matrix Update 0,3 uitvoert."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtuele matrix Update 0,3 releaseopmerkingen

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen welke de kritieke openstaande actie-items en de problemen opgelost op updates van Microsoft Azure StorSimple virtuele matrix.

De releaseopmerkingen worden voortdurend bijgewerkt en wanneer kritieke problemen vereisen van een tijdelijke oplossing worden ontdekt, ze zijn toegevoegd. Voordat u uw StorSimple virtuele matrix implementeert, zorgvuldig door de gegevens in de release-informatie.

Update 0,3 overeenkomt met de software versie **10.0.10288.0**.

> [AZURE.NOTE] Updates zijn storend en start het apparaat opnieuw. Als I/O uitgevoerd worden, wordt in het apparaat downtime bijhoudt.


## <a name="whats-new-in-the-update-03"></a>Wat is er nieuw in de Update 0,3

Update 0,3 is hoofdzakelijk een bug-fix opbouwen. In deze versie, zijn verschillende bugs bij te werken met resultaat back-fouten in de vorige versie gericht.

## <a name="issues-fixed-in-the-update-03"></a>Problemen in de Update 0,3 worden opgelost

De volgende tabel bevat een overzicht van problemen in deze release worden opgelost.

| Nee.  | Functie                              | Probleem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Back-ups                                |Een probleem is zichtbaar in de eerdere versie, waar de back-ups om uit te voeren voor een bestandsshare zou mislukt. Als dit probleem is opgetreden, wordt de back-uptaak zou mislukt en wordt een melding voor een kritieke op de service StorSimple Manager op de hoogte stellen van de gebruiker is verheven. Dit probleem heeft geen invloed op de gegevens op de waarden voor aandelen of toegang tot de gegevens. De onderliggende oorzaak is geïdentificeerd en vaste in deze release. <br></br> De correctie geldt niet met terugwerkende kracht voor aandelen die al dit probleem zien. Klanten die dit probleem zien moeten eerst Update 0,3 toepassen en vervolgens contact opnemen met Microsoft Support back-ups volledige systeem u kunt het probleem oplossen. In plaats van het contact opneemt met Microsoft Support, kunnen klanten ook terugzetten naar een nieuwe delen van een correct back-up voor de desbetreffende aandelen.                                                                                                                                                                                 |
| 2    | iSCSI                         | Een probleem is zichtbaar in de eerdere versie, waar de hoeveelheden verdwijnen wanneer u gegevens kopieert naar een volume op de virtuele StorSimple-matrix. Dit probleem is opgelost in deze release. <br></br> De correcties van kracht alleen op nieuw gemaakte volumes. De correcties worden niet met terugwerkende kracht toegepast volume die al dit probleem zien. Klanten wordt aangeraden naar voren en de desbetreffende hoeveelheden online via de portal van Azure klassieke, een back-up uitvoeren voor deze volumes en zet deze volumes nieuwe volume.                                                               |


## <a name="known-issues-in-the-update-03"></a>Bekende problemen in de Update 0,3

De volgende tabel bevat een overzicht van bekende problemen voor de virtuele StorSimple-matrix en bevat de problemen op release die uit de vorige versies. 


| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | De virtuele apparaten die is gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. | Deze virtuele apparaten moeten worden overgenomen voor de algemene release met een werkstroom voor noodgevallen herstel (DR). |
| **2.** | Ingerichte gegevens Schijfopruiming | Zodra u hebt deze is ingericht een gegevensschijf van een bepaalde opgegeven grootte en de bijbehorende StorSimple virtueel apparaat hebt gemaakt, moet u niet uitvouwen of de gegevensschijf verkleinen. Wilt doen resultaten in een verlies van alle gegevens in de lokale lagen van het apparaat. |   |
| **3.** | Groepsbeleid | Wanneer een apparaat een domein behoren is, kan toepassen van een Groepsbeleid afnemen als het apparaat betrekking heeft. | Zorg ervoor dat uw virtuele matrix bevindt zich in een eigen organisatie-eenheid voor Active Directory en geen GPO (GPO) zijn toegepast.|
| **4.** | Lokale web UI | Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige UI lokale webpagina's zoals probleemoplossing of onderhoud niet goed werkt. Knoppen op deze pagina's ook werkt mogelijk niet. | Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen.|
| **5.** | Lokale web UI | In een Hyper-V virtuele machine, het via netwerkinterfaces zoals in het web UI worden weergegeven als 10 GB/s interfaces. | Dit is een weerspiegeling van Hyper-V. Hyper-V ziet u altijd 10 GB/s voor virtuele netwerkadapters. |
| **6.** | Gelaagde volumes of waarden voor aandelen | Bytebereik vergrendelen voor toepassingen die werken met de StorSimple doorverbonden volumes wordt niet ondersteund. Als byte bereik vergrendelen is ingeschakeld, werkt StorSimple trapsgewijs schakelen niet. | Aanbevolen maatregelen omvatten: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Kies gegevens moeten worden geplaatst van deze toepassing in lokaal vastgemaakte hoeveelheden in plaats van trapsgewijze volumes.<br></br>*Voorbehoud*: wanneer volumes lokaal gebruiken vastgemaakt en byte bereik vergrendelen is ingeschakeld, het volume van het lokaal vastgemaakte online kan zijn zelfs voordat het herstellen voltooid is. In deze processen, als een herstellen uitgevoerd wordt, moet klikt u vervolgens u wachten voor het herstellen om te voltooien. |
| **7.** | Trapsgewijze waarden voor aandelen | Werken met grote bestanden kan leiden tot traag laag af. | Tijdens het werken met grote bestanden, wordt u aangeraden dat het grootste bestand kleiner dan 3% van de grootte van het delen is. |
| **8.** | Capaciteit voor aandelen gebruikt | Ziet u mogelijk verbruik delen als er geen gegevens op de optie voor delen. Dit komt omdat de gebruikte capaciteit voor aandelen metagegevens bevat. |   |
| **9.** | Problemen oplossen | U kunt alleen de herstel van een bestandsserver naar hetzelfde domein als die van het bronapparaat uitvoeren. Herstel naar een apparaat in een ander domein wordt niet ondersteund in deze release. | Dit is geïmplementeerd in een latere versie. |
| **10.** | Azure PowerShell | De virtuele StorSimple-apparaten kunnen niet worden beheerd via de PowerShell Azure in deze release. | Het beheer van de virtuele apparaten moet worden uitgevoerd via de portal van Azure klassieke en de lokale Internet UI. |
| **11.** | Wachtwoord wijzigen | De virtuele matrix apparaat-console accepteert alleen invoer in en-US toetsenbord-indeling. |   |
| **12.** | CHAP | CHAP referenties eenmaal hebt gemaakt, kunnen niet worden verwijderd. Ook als u de referenties CHAP wijzigt, moet u de hoeveelheden offline te zetten en brengt u ze online zodat de wijziging van kracht. | Dit probleem is opgelost in een latere versie. |
| **13.** | iSCSI-server  | De 'gebruikt opslagruimte' weergegeven voor een iSCSI-volume mogelijk anders in de StorSimple Manager-service en de iSCSI-host. | De host iSCSI heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de toegewezen wanneer het volume de maximale grootte is blokken.|
| **14.** | Bestandsserver  | Als een bestand in een map heeft een alternatieve gegevens Stream (ADVERTENTIES) gekoppeld, is niet de ADVERTENTIES back-up gemaakt of hersteld via herstel, klonen en herstellen van items niveau.| |


## <a name="next-step"></a>Volgende stap

[Update 0,3 installeren](storsimple-ova-install-update-01.md) op uw StorSimple virtuele matrix.

## <a name="references"></a>Verwijzingen

Zoekt u een oudere versie notitie? Ga naar: 

- [StorSimple virtuele matrix Update 0,1-0,2 Releaseopmerkingen](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuele matrix algemeen beschikbaarheid releaseopmerkingen](storsimple-ova-pp-release-notes.md)
 

