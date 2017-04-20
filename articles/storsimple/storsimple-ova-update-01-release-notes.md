<properties 
   pageTitle="Releaseopmerkingen voor StorSimple virtuele matrix Updates | Microsoft Azure"
   description="Kritieke openstaande actie-items en oplossingen beschreven voor de StorSimple virtuele matrix Update 0,2 en 0,1 uitgevoerd."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Releaseopmerkingen voor StorSimple virtuele matrix Update 0,2 en 0,1

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen welke de kritieke openstaande actie-items en de problemen opgelost op updates van Microsoft Azure StorSimple virtuele matrix. (Microsoft Azure StorSimple virtuele matrix is ook bekend als het StorSimple on-premises implementatie virtuele apparaat of het virtuele apparaat StorSimple.) 

De releaseopmerkingen worden voortdurend bijgewerkt en wanneer kritieke problemen vereisen van een tijdelijke oplossing worden ontdekt, ze zijn toegevoegd. Voordat u uw StorSimple virtuele apparaat implementeert, zorgvuldig door de gegevens in de release-informatie.

Update 0,2 overeenkomt met de software versie **10.0.10280.0**; Update 0,1 is versie **10.0.10279.0**. De volgende secties bevatten de wijzigingen die u voor elke update. 

> [AZURE.NOTE] Updates zijn storend en start het apparaat wordt opnieuw. Als I/O uitgevoerd worden, wordt het apparaat downtime in rekening gebracht.

## <a name="issues-fixed-in-the-update-02"></a>Problemen opgelost met de Update 0,2
Update 0,2 bevat alle wijzigingen van Update 0,1 behalve de correctie die worden beschreven in de volgende tabel:

Functie                              | Probleem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Updates                                 | In de laatste versie, zijn niet updates automatisch gedetecteerd in de klassieke Azure portal, zodat u had gebruik van de lokale Web-gebruikersinterface voor het installeren van updates. Dit probleem is opgelost in deze release. Na de installatie van Update 0,2, kunt u toekomstige updates met behulp van de Azure klassieke portal installeren.                       

## <a name="whats-new-in-the-update-01"></a>Wat is er nieuw in de Update 0,1

Update 0,1 bevat de volgende correcties en verbeteringen. 

- **Verbeterde tolerantie voor de cloud, bijvoorbeeld**: deze release heeft verschillende correcties rond herstel, back-up, herstellen en trapsgewijs schakelen in het geval van een wolk connectivity verstoringen. 

- **Verbeterde prestaties herstellen**: deze release heeft correcties die aanzienlijk omlaag de voltooiingstijd van de taken herstellen inkorten hebt.

- **Automatisch ruimte terug optimalisatie**: wanneer gegevens is verwijderd op dun ingerichte volumes, de niet-gebruikte opslagruimte blokken moeten opnieuw worden gebruikt. Deze release verbeterd, het proces ruimte terug vanuit de cloud met als resultaat de ongebruikte ruimte wordt sneller beschikbaar ten opzichte van de vorige versies.

- **Nieuwe virtuele schijf afbeeldingen**: nieuwe VHD, VHDX en VMDK zijn nu beschikbaar via de portal van Azure klassieke. U kunt deze afbeeldingen om het inrichten van nieuwe Update 0,1 apparaten downloaden.

- **Verbetering van de nauwkeurigheid van de status van taken in de portal**: In de eerdere versie van de software, niet taakstatus rapporteren in de portal gedetailleerde is. Dit probleem is opgelost in deze release.

- **Domein join-ervaring**: correcties met betrekking tot lid worden van een domein en naam van het apparaat.


## <a name="issues-fixed-in-the-update-01"></a>Problemen opgelost met de Update 0,1

De volgende tabel bevat een overzicht van problemen in deze release worden opgelost.

| Nee.  | Functie                              | Probleem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | In sommige versies VMware, is de schijf OS gezien als verspreid veroorzaakt door waarschuwingen en storend voor normale bewerkingen. Dit is opgelost in deze release.                                                                                                                                                                                    |
| 2    | iSCSI-server                         | In de laatste versie, is de gebruiker om op te geven van een gateway voor elke netwerkinterface ingeschakeld van uw StorSimple virtueel apparaat vereist. Dit gedrag wordt gewijzigd in deze release zodat de gebruiker heeft minimaal één gateway voor alle ingeschakelde netwerkinterfaces configureren.                                                                              |
| 3    | Support-pakket                      | Ondersteuning voor pakket siteverzameling is in de eerdere versie van de software mislukt wanneer de grootte van het pakket groter zijn dan 1 GB zijn. Dit probleem is opgelost in deze release.                                                                                                                                                                               |
| 4    | Cloudtoegang                         |  In de laatste versie, als de virtuele matrix StorSimple geen netwerkverbinding heeft en opnieuw is opgestart, heeft de lokale gebruikersinterface verbindingsproblemen. Dit probleem is opgelost in deze release.                                                                                                                            |
| 5    | Grafieken bewaken                    | De cloud capaciteit gebruik grafieken weergegeven in de vorige versie, een failover apparaat volgen onjuiste waarden in de portal van Azure klassieke. Dit is vastgezet op de huidige versie.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Bekende problemen in de Update 0,1

De volgende tabel bevat een overzicht van bekende problemen voor de virtuele StorSimple-matrix en bevat de problemen op release die uit de vorige versies. **De versie van de problemen vermeld in deze release zijn gemarkeerd met een sterretje. Bijna alle problemen in deze lijst hebt overgebracht van de versie GA van StorSimple virtuele matrix.**


| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | De virtuele apparaten die is gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. | Deze virtuele apparaten moeten worden overgenomen voor de algemene release met een werkstroom voor noodgevallen herstel (DR). |
| **2.** | Ingerichte gegevens Schijfopruiming | Zodra u hebt deze is ingericht een gegevensschijf van een bepaalde opgegeven grootte en de bijbehorende StorSimple virtueel apparaat hebt gemaakt, moet u niet uitvouwen of de gegevensschijf verkleinen. Een verlies van alle gegevens in de lokale lagen van het apparaat dat treden u probeert te doen. |   |
| **3.** | Groepsbeleid | Wanneer een apparaat een domein behoren is, kan toepassen van een Groepsbeleid afnemen als het apparaat betrekking heeft. | Zorg ervoor dat uw virtuele matrix bevindt zich in een eigen organisatie-eenheid voor Active Directory en geen GPO (GPO) zijn toegepast.|
| **4.** | Lokale web UI | Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige UI lokale webpagina's zoals probleemoplossing of onderhoud niet goed werkt. Knoppen op deze pagina's ook werkt mogelijk niet. | Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen.|
| **5.** | Lokale web UI | In een Hyper-V virtuele machine, het via netwerkinterfaces zoals in het web UI worden weergegeven als 10 GB/s interfaces. | Dit is een weerspiegeling van Hyper-V. Hyper-V ziet u altijd 10 GB/s voor virtuele netwerkadapters. |
| **6.** | Gelaagde volumes of waarden voor aandelen | Bytebereik vergrendelen voor toepassingen die werken met de StorSimple doorverbonden volumes wordt niet ondersteund. Als byte bereik vergrendelen is ingeschakeld, werkt StorSimple trapsgewijs schakelen niet. | Aanbevolen maatregelen omvatten: <br></br>Bytebereik vergrendelen in uw toepassingslogica uitschakelen.<br></br>Kies gegevens moeten worden geplaatst van deze toepassing in lokaal vastgemaakte hoeveelheden in plaats van trapsgewijze volumes.<br></br>*Voorbehoud*: als volumes lokaal gebruiken vastgemaakt en byte bereik vergrendelen is ingeschakeld, moet u zich realiseren dat het lokaal vastgemaakte volume online zijn kan even voordat het herstellen voltooid is. In deze processen, als een herstellen uitgevoerd wordt, moet klikt u vervolgens u wachten voor het herstellen om te voltooien. |
| **7.** | Trapsgewijze waarden voor aandelen | Werken met grote bestanden kan leiden tot traag laag af. | Tijdens het werken met grote bestanden, wordt u aangeraden dat het grootste bestand kleiner dan 3% van de grootte van het delen is. |
| **8.** | Capaciteit voor aandelen gebruikt | Ziet u mogelijk delen verbruik bij afwezigheid van alle gegevens op de optie voor delen. Dit komt omdat de gebruikte capaciteit voor aandelen metagegevens bevat. |   |
| **9.** | Problemen oplossen | U kunt alleen de herstel van een bestandsserver naar hetzelfde domein als die van het bronapparaat uitvoeren. Herstel naar een apparaat in een ander domein wordt niet ondersteund in deze release. | Hiermee worden uitgevoerd in een latere versie. |
| **10.** | Azure PowerShell | De virtuele StorSimple-apparaten kunnen niet worden beheerd via de PowerShell Azure in deze release. | Het beheer van de virtuele apparaten moet worden uitgevoerd via de portal van Azure klassieke en de lokale Internet UI. |
| **11.** | Wachtwoord wijzigen | De virtuele matrix apparaat-console accepteert alleen invoer in en-US toetsenbord-indeling. |   |
| **12.** | CHAP | CHAP referenties eenmaal hebt gemaakt, kunnen niet worden verwijderd. Ook als u de referenties CHAP wijzigt, moet u naar de hoeveelheden offline te zetten en brengt u ze online zodat de wijziging van kracht. | Deze wordt opgelost in een latere versie. |
| **13.** | iSCSI-server  | De 'gebruikt opslagruimte' weergegeven voor een iSCSI-volume mogelijk anders in de StorSimple Manager-service en de iSCSI-host. | De host iSCSI heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de toegewezen wanneer het volume de maximale grootte is blokken.|
| **14.** | Bestand server *  | Als een bestand in een map heeft een alternatieve gegevens Stream (ADVERTENTIES) gekoppeld, is niet de ADVERTENTIES back-up gemaakt of hersteld via herstel, klonen en herstellen van items niveau.| |


## <a name="next-step"></a>Volgende stap

[Updates installeren](storsimple-ova-install-update-01.md) op uw StorSimple virtuele matrix.
