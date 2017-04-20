<properties 
   pageTitle="Releaseopmerkingen StorSimple virtuele matrix | Microsoft Azure"
   description="Kritieke openstaande actie-items en oplossingen beschreven voor de virtuele StorSimple-matrix."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Releaseopmerkingen StorSimple virtuele matrix

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke openstaande actie-items op de versie van de algemene beschikbaarheid (GA) maart 2016 van de Microsoft Azure StorSimple virtuele matrix (ook wel bekend als het StorSimple on-premises implementatie virtuele apparaat of het virtuele apparaat StorSimple). Deze release overeenkomt met softwareversie 10.0.10271.0.

De releaseopmerkingen worden voortdurend bijgewerkt en wanneer kritieke problemen vereisen van een tijdelijke oplossing worden ontdekt, ze zijn toegevoegd. Voordat u uw StorSimple virtuele apparaat implementeert, zorgvuldig door de gegevens in de release-informatie. 

De volgende tabel bevat een overzicht van bekende problemen in deze release.


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
