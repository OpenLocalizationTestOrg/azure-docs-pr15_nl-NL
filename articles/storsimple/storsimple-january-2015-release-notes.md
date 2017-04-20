<properties 
   pageTitle="StorSimple 8000 Update 0,2 Releaseopmerkingen | Microsoft Azure"
   description="Beschrijving van de nieuwe functies en reparaties, openstaande actie-items en beschikbaar tijdelijke oplossingen voor de januari 2015 Microsoft Azure StorSimple release (Update 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 reeks Update 0,2 Releaseopmerkingen - januari 2015

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke openstaande actie-items op de januari 2015-versie van Microsoft Azure StorSimple. Ze bevatten ook een lijst met de StorSimple-software en firmware updates zijn opgenomen in deze release. Dit is de tweede versie nadat de StorSimple 8000 reeks versie beschikbaar is gesteld over het algemeen in juli 2014.
  
Deze update wordt niet gewijzigd voor de versie van de software fysiek apparaat van de update oktober. Deze blijft versie 6.3.9600.17312. De afbeelding die wordt gebruikt door de afbeelding virtueel apparaat is gewijzigd in deze release. Daarom alle nieuwe virtuele apparaten gemaakt nadat 20-1-2015 de versie, aangezien 6.3.9600.17361 wordt weergegeven.  

Raadpleeg de volgende gegevens in de releaseopmerkingen voor het bijwerken van januari 2015.

> [AZURE.IMPORTANT]  
>
>- Deze update is niet beschikbaar via Windows Update en kan niet worden geïnstalleerd zoals andere updates. Uw apparaat, deze update niet ontvangt, zelfs als u de updates hebt toegepast met behulp van de Azure klassieke portal. Deze update geldt alleen voor virtuele apparaten die zijn gemaakt nadat 20 januari 2015. 
> 
>- De januari-versie van StorSimple bevat geen updates van het apparaat StorSimple. U kunt nog steeds beschikbare Windows-updates toepassen op het virtuele apparaat, inclusief recente beveiligingscorrecties, maar u ziet geen een wijziging in versie voor de fysieke StorSimple-apparaat.

## <a name="whats-new-in-the-january-release"></a>Wat is er nieuw in de januari-versie

Deze update bevat een oplossing die betrekking hebben op de hoeveelheden offlinesynchronisatie op het virtuele apparaat. (Zie [problemen opgelost in deze release](#issues-fixed-in-the-january-release).)  

De update bevat geen nieuwe functies of functionaliteit.  

## <a name="issues-fixed-in-the-january-release"></a>Problemen opgelost in de release januari

De volgende tabel beschrijft het probleem dat is vastgezet op deze update.

|Nee.|Functie|Probleem|Dit geldt voor fysiek apparaat|Dit geldt voor virtueel apparaat 
|---|-------|-----|--------------------------|-------------------------
|1|Volumes offlinesynchronisatie|Nadat hoog cloud vertragingstijden enkele minuten aanhouden, gaat de StorSimple virtueel apparaat hoeveelheden offline op de hosts. Deze fix Hiermee verhoogt u de drempelwaarde voor cloud vertragingstijden, daarmee ook minimaliseren de situaties waardoor de hoeveelheden offline te gaan op hosts.|Nee|Ja  

## <a name="known-issues-in-the-january-release"></a>Bekende problemen in de januari-versie

De volgende tabel bevat een overzicht van bekende problemen in deze release.
 
|Nee.|Functie|Probleem|Opmerkingen/tijdelijke oplossing|Dit geldt voor fysiek apparaat|Dit geldt voor virtueel apparaat 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Factory opnieuw instellen|  In sommige gevallen, wanneer u een Beginwaarden factory uitvoert het apparaat StorSimple vastgelopen en dit bericht weergeven: **terugzetten naar factory wordt uitgevoerd (fase 8).** Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt.| Druk CTRL + C niet op na het starten van een fabriek opnieuw instellen. Als u al in deze status bent, neemt u contact op met Microsoft Support voor de volgende stappen.|Ja|Nee|
|2|Schijf quorum| In uitzonderlijke gevallen als de meeste schijven in de ruimte EBOD van een 8600 apparaat geen verbinding met resultaat geen quorum schijf, klikt u vervolgens de opslag van toepassingen niet offline. Deze worden offline blijven, zelfs als de schijven opnieuw verbinding wordt gemaakt.|U moet start het apparaat opnieuw op. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen.|Ja |Nee
|3|Cloud momentopname fouten|In uitzonderlijke gevallen kan een momentopname cloud mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat, van hetzelfde oorspronkelijke volume die is verwijderd.||Ja|Ja
|4|Onjuiste controller-ID|Wanneer een vervangende controller wordt uitgevoerd, mogelijk controller 0 als controller 1 weergegeven. Tijdens de vervangende controller, wanneer de afbeelding is geladen vanuit het knooppunt peer worden de controller-ID weergegeven in eerste instantie als van de besturing van het peer-ID.  In uitzonderlijke gevallen kan ook dit gedrag zichtbaar na het systeem opnieuw opstarten.|Er is geen gebruikersactie vereist. Deze situatie worden zelf opgelost nadat de vervangende controller voltooid is.|Ja|Nee 
|5| Apparaat grafieken bewaken|In de service StorSimple Manager, het apparaat controleren grafieken werken niet als eenvoudige of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat.|Wijzig de configuratie van de web-proxy voor het apparaat dat is geregistreerd bij uw Manager StorSimple-service zodat dat verificatie is ingesteld op geen. Klik hiertoe uitvoeren de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple.|Ja|Ja
|6| Opslag-accounts|Gebruik van de Storage-service het account opslagruimte verwijderen is een niet-ondersteunde scenario. Dit leidt tot een situatie waarin gebruikersgegevens kan niet worden opgehaald.|| Ja |  Ja
|7|Apparaat failover| Meerdere failovers van een container volume van het bronapparaat met dezelfde voor verschillende apparaten wordt niet ondersteund.| Failover vanaf een apparaat met één dode naar meerdere apparaten, kunt u het volume containers op de eerste apparaat overgenomen Gegevenseigendom kwijtraakt. Na een failover wordt deze containers volume worden weergegeven of gedragen zich anders wanneer u deze in de portal van Azure klassieke weergeven.|Ja|Nee
|8| Installatie|Tijdens StorSimple Adapter voor SharePoint-installatie moet u een apparaat IP-adres in volgorde voor de installatie te voltooien.||Ja|Nee
|9| Webproxy|Als de configuratie van uw web-proxy HTTPS als het opgegeven protocol heeft, klikt u vervolgens de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook in het proces aanzienlijk bronnen op uw apparaat worden gegenereerd.|Zorg ervoor dat de URL van de web-proxy HTTP als het opgegeven protocol heeft. Zie meer informatie over de [web-proxy configureren voor uw apparaat](storsimple-configure-web-proxy.md).|Ja |Nee
|10|Webproxy|  Als u configureert en webproxy op een apparaat met geregistreerde inschakelt, moet u start de actieve domeincontroller op uw apparaat.|| Ja |Nee
|11|Cloud hoge latentie en hoge i/o-werkbelasting|Wanneer uw apparaat StorSimple een combinatie van zeer hoog cloud vertragingstijden (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de hoeveelheden apparaat dieper op een anders werken staat en het i/o kan mislukken met een foutbericht 'apparaat niet gereed'.|U moet handmatig start opnieuw op apparaten of uitvoeren van een apparaat failover deze situatie te herstellen.|Ja|Nee

## <a name="physical-device-updates-in-the-january-release"></a>Fysiek apparaat-updates in de januari vrijgeven

Deze update bevat geen andere wijzigingen aan op het apparaat StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Seriële bijgevoegd SCSI (SA's) controller en firmware-updates in de januari vrijgeven

Deze release bevat geen updates van de controller uit seriële bijgevoegd SCSI (SA's) of de firmware. De stuurprogrammaupdate is in de oktober 2014 release. 

## <a name="virtual-device-updates-in-the-january-release"></a>Virtuele apparaatupdates in de januari vrijgeven

Deze versie bevat een bijgewerkte afbeelding voor het virtuele apparaat. De virtuele apparaten gemaakt na 20 januari 2015 wordt versie van de software weergegeven als 6.3.9600.17361.

 
