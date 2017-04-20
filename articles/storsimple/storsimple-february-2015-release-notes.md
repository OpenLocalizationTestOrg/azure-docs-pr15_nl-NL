<properties 
   pageTitle="StorSimple 8000 Update 0,3 releaseopmerkingen | Microsoft Azure"
   description="Beschrijving van de nieuwe functies en reparaties, openstaande actie-items en beschikbaar tijdelijke oplossingen voor de februari 2015 Microsoft Azure StorSimple release (Update 0,3)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 reeks Update 0,3 releaseopmerkingen - februari 2015

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke openstaande actie-items voor StorSimple 8000 reeks Update 0,3 uitgebracht in februari 2015. Ze bevatten ook een lijst met de StorSimple-software en firmware updates zijn opgenomen in deze release. Dit is de derde release nadat de StorSimple 8000 reeks versie beschikbaar is gesteld over het algemeen in juli 2014.
  
Deze update wordt niet gewijzigd voor de versie van de software apparaat van de update januari. Deze blijft versie 6.3.9600.17312. U kunt bevestigen dat de update door te schakelen van de datum **Laatst bijgewerkt** is geïnstalleerd. Als de datum is 10-2-2015 of hoger, wordt de update is geïnstalleerd.  

Het is raadzaam dat u naar zoeken en beschikbare updates toepassen onmiddellijk nadat u uw apparaat StorSimple hebt geïnstalleerd. U kunt ook automatische updates downloaden en installeren van essentiële updates van Microsoft zodra ze zijn vrijgegeven inschakelen. Zie [uw apparaat StorSimple bijwerken](storsimple-update-device.md)voor meer informatie.  

Controleer de gegevens in de releaseopmerkingen voordat u de update in uw StorSimple-oplossing implementeert.  

>[AZURE.IMPORTANT]   
>
> - Gebruik de StorSimple Manager-service en niet Windows PowerShell voor StorSimple de februari-update te installeren.   
> - Het duurt ongeveer een uur deze update te installeren. Echter als u cumulatieve updates worden geïnstalleerd, kan het proces ongeveer 3 uur duren om te voltooien.  
> - De februari-versie van StorSimple bevat geen updates van het virtuele StorSimple-apparaat. U kunt nog steeds beschikbare Windows-updates toepassen op het virtuele apparaat, inclusief recente beveiligingscorrecties, maar een wijziging in versie voor het virtuele apparaat wordt niet weergegeven.  

Zorg ervoor dat de volgende voorwaarden wordt voldaan vóór het bijwerken van uw apparaat StorSimple.  

- Zorg ervoor dat beide apparaatcontrollers worden uitgevoerd voordat u naar updates zoeken. Als een van beide controller wordt niet uitgevoerd, mislukt de gescande afbeelding. Als u wilt controleren of de controllers zijn in orde zijn, navigeer naar **Hardware Status** onder **de onderhoudspagina voor** . Als er onderdelen die **aandacht nodig**, neem dan contact op met Microsoft Support voordat u verdergaat.
- Ervoor zorgen dat vaste IP-adressen voor zowel controller 0 en 1 controller geschikt en kunnen worden verbonden met Internet terwijl deze worden gebruikt voor het onderhoud van de updates bij het apparaat. U kunt de [testverbinding cmdlet](https://technet.microsoft.com/library/hh849808.aspx) ping een bekend adres buiten het netwerk, zoals outlook.com, om te bevestigen dat de controller connectiviteit met het externe netwerk heeft.
- Zorg ervoor dat poort 80 en 443 beschikbaar op uw apparaat StorSimple voor uitgaande communicatie zijn. Zie de [vereisten voor uw apparaat StorSimple toegang](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)voor meer informatie.
- Als de versie van de software apparaat ouder is dan 6.3.9600.17312 (update van oktober 2014), de poorten gegevens 2 en 3 van de gegevens uitschakelen als ingeschakeld, voordat u begint met de update. Laat de 2 van de gegevens of gegevens 3 poorten ingeschakeld als u de update toepassen, is het mogelijk dat de controller apparaat om te gaan herstel. Houd er rekening mee dat wanneer u de netwerkinterfaces uitschakelt, de bijbehorende hoeveelheden wordt offline worden geplaatst en het i/o wordt voor de duur van de update worden onderbroken.  
  
## <a name="whats-new-in-the-february-release"></a>Wat is er nieuw in de februari-versie

Deze update bevat een oplossing voor de fabriek opnieuw probleem die zijn aangebracht instellen op apparaten die uit de GA had zijn bijgewerkt naar de versie oktober 2014 loslaat. Zie de [volgende problemen opgelost in deze release](#issues-fixed-in-the-february-release)voor meer informatie.   

Deze update bevat geen nieuwe functies of functionaliteit.  

## <a name="issues-fixed-in-the-february-release"></a>Problemen opgelost in de release februari

De volgende tabel beschrijft het probleem dat is vastgezet op deze update.  
 
| Nee. | Functie | Probleem | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Factory opnieuw instellen | U wilt uitvoeren van een fabriek opnieuw instellen op een apparaat dat u oorspronkelijk hebt u er de GA versie (versie 6.3.9600.17215) hebt geïnstalleerd, maar is bijgewerkt naar de oktober release (versie 6.3.9600.17312). De fabriek mislukt opnieuw instellen en het apparaat wordt vastloopt. | Ja | Nee |


## <a name="known-issues-in-the-february-release"></a>Bekende problemen in de februari-versie

De volgende tabel bevat een overzicht van bekende problemen in deze release.
 
| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Dit geldt voor fysiek apparaat  | Dit geldt voor virtueel apparaat |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Factory opnieuw instellen | In sommige gevallen, wanneer u een Beginwaarden factory uitvoert het apparaat StorSimple vastgelopen en dit bericht weergeven: **terugzetten naar factory wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt. | Druk CTRL + C niet op na het starten van een fabriek opnieuw instellen. Als u al in deze status bent, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 2 | Schijf quorum | In uitzonderlijke gevallen als de meeste schijven in de ruimte EBOD van een 8600device hebben geen verbinding met resultaat geen quorum schijf, klikt u vervolgens de opslag van toepassingen niet offline. Deze worden offline blijven, zelfs als de schijven opnieuw verbinding wordt gemaakt. | U moet start het apparaat opnieuw op. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 3 | Cloud momentopname fouten | In uitzonderlijke gevallen kan een momentopname cloud mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat, van hetzelfde oorspronkelijke volume die is verwijderd. |  | Ja | Ja |
| 4 | Onjuiste controller-ID | Wanneer een vervangende controller wordt uitgevoerd, mogelijk controller 0 als controller 1 weergegeven. Tijdens de vervangende controller, wanneer de afbeelding is geladen vanuit het knooppunt peer worden de controller-ID weergegeven in eerste instantie als van de besturing van het peer-ID. In uitzonderlijke gevallen kan ook dit gedrag zichtbaar na het systeem opnieuw opstarten. | Er is geen gebruikersactie vereist. Deze situatie worden zelf opgelost nadat de vervangende controller voltooid is. | Ja | Nee |
| 5 | Apparaat grafieken bewaken | In de service StorSimple Manager, het apparaat controleren grafieken werken niet als eenvoudige of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. | Wijzig de configuratie van de web-proxy voor het apparaat dat is geregistreerd bij uw Manager StorSimple-service zodat dat verificatie is ingesteld op geen. Klik hiertoe uitvoeren de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple. | Ja | Ja |
| 6 | Opslag-accounts | Gebruik van de Storage-service het account opslagruimte verwijderen is een niet-ondersteunde scenario. Dit leidt tot een situatie waarin gebruikersgegevens kan niet worden opgehaald. |  | Ja | Ja |
| 7 | Apparaat failover | Meerdere failovers van een container volume van het bronapparaat met dezelfde voor verschillende apparaten wordt niet ondersteund.  Failover vanaf een apparaat met één dode naar meerdere apparaten, kunt u het volume containers op de eerste apparaat overgenomen Gegevenseigendom kwijtraakt. Na een failover wordt deze containers volume worden weergegeven of gedragen zich anders wanneer u deze in de portal van Azure klassieke weergeven. |   | Ja | Nee |
| 8 | Installatie | Tijdens StorSimple Adapter voor SharePoint-installatie moet u een apparaat IP-adres in volgorde voor de installatie te voltooien. |  | Ja | Nee |
| 9 | Webproxy | Als de configuratie van uw web-proxy HTTPS als het opgegeven protocol heeft, klikt u vervolgens de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook in het proces aanzienlijk bronnen op uw apparaat worden gegenereerd. | Zorg ervoor dat de URL van de web-proxy HTTP als het opgegeven protocol heeft. Meer informatie over de [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md). | Ja | Nee |
| 10 | Webproxy | Als u configureert en webproxy op een apparaat met geregistreerde inschakelt, moet u start de actieve domeincontroller op uw apparaat. |  | Ja | Nee |
| 11 | Cloud hoge latentie en hoge i/o-werkbelasting | Wanneer uw apparaat StorSimple een combinatie van zeer hoog cloud vertragingstijden (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de hoeveelheden apparaat dieper op een anders werken staat en het i/o kan mislukken met een foutbericht 'apparaat niet gereed'. | U moet handmatig start opnieuw op apparaten of uitvoeren van een apparaat failover deze situatie te herstellen. | Ja | Nee |

## <a name="physical-device-updates-in-the-february-release"></a>Fysiek apparaat-updates in de februari vrijgeven

Deze update corrigeert het probleem voor het opnieuw instellen van factory die zijn aangebracht op apparaten die uit de GA had zijn bijgewerkt naar de versie oktober 2014 loslaat. Deze bevat geen andere updates naar de StorSimple-apparaat.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Seriële bijgevoegd SCSI (SA's) controller en firmware-updates in de februari vrijgeven

Deze release bevat geen updates van de controller uit seriële bijgevoegd SCSI (SA's) of de firmware. De stuurprogrammaupdate is in de oktober 2014 release.  

## <a name="virtual-device-updates-in-the-february-release"></a>Virtuele apparaatupdates in de februari vrijgeven

Deze release bevat niet alle updates voor het virtuele apparaat. De softwareversie van een virtueel apparaat wordt niet gewijzigd als u deze update toepassen.
 
