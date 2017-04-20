<properties 
   pageTitle="Releaseopmerkingen voor versie StorSimple 8000 los | Microsoft Azure"
   description="Beschrijving van de nieuwe functies, openstaande actie-items en beschikbaar tijdelijke oplossingen voor de juli 2014 Microsoft Azure StorSimple release."
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

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Releaseopmerkingen voor StorSimple 8000 reeks versie - juli 2014 

## <a name="overview"></a>Overzicht

De releaseopmerkingen volgen identificeren de kritieke openstaande actie-items voor de StorSimple 8000 reeks juli 2014 algemene beschikbaarheid (GA) versie van Microsoft Azure StorSimple. Deze release overeenkomt met softwareversie 6.3.9600.17215.  

Tenzij anderszins is opgegeven, worden deze releaseopmerkingen toepassen op alle modellen van het apparaat StorSimple. De releaseopmerkingen worden voortdurend bijgewerkt; zoals kritieke problemen vereisen van een tijdelijke oplossing zijn gevonden, wordt ze zijn toegevoegd. Houd rekening met de volgende informatie voordat u uw Microsoft Azure StorSimple-oplossing implementeert.  

## <a name="known-issues-in-this-release"></a>Bekende problemen in deze release
De volgende tabel bevat een overzicht van bekende problemen in deze release.  
 
| Nee. | Functie | Probleem | Opmerkingen/tijdelijke oplossing | Dit geldt voor fysiek apparaat | Dit geldt voor virtueel apparaat |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory opnieuw instellen | In sommige gevallen, wanneer u een Beginwaarden factory uitvoert het apparaat StorSimple vastgelopen en dit bericht weergeven: **terugzetten naar factory wordt uitgevoerd (fase 8)**. Dit gebeurt als u op CTRL + C drukt terwijl de cmdlet uitgevoerd wordt. | Druk CTRL + C niet op na het starten van een fabriek opnieuw instellen. Als u al in deze status bent, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 2 | Schijf quorum | In uitzonderlijke gevallen als de meeste schijven in de ruimte EBOD van een 8600 apparaat geen verbinding met resultaat geen quorum schijf, klikt u vervolgens de opslag van toepassingen niet offline. Deze worden offline blijven, zelfs als de schijven opnieuw verbinding wordt gemaakt. | U moet start het apparaat opnieuw op. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support voor de volgende stappen. | Ja | Nee |
| 3 | Cloud momentopname fouten | In uitzonderlijke gevallen kan een momentopname cloud mislukken met de fout **Maximum back-limiet is bereikt**. Dit gebeurt als u meer dan 255 online klonen op hetzelfde apparaat, van hetzelfde oorspronkelijke volume die is verwijderd. | | Ja | Ja |
| 4 | Onjuiste controller-ID | Wanneer een vervangende controller wordt uitgevoerd, mogelijk controller 0 als controller 1 weergegeven. Tijdens de vervangende controller, wanneer de afbeelding is geladen vanuit het knooppunt peer worden de controller-ID weergegeven in eerste instantie als van de besturing van het peer-ID. In uitzonderlijke gevallen kan ook dit gedrag zichtbaar na het systeem opnieuw opstarten. | Er is geen gebruikersactie vereist. Deze situatie worden zelf opgelost nadat de vervangende controller voltooid is. | Ja | Nee |
| 5 | Apparaat grafieken bewaken | In de service StorSimple Manager, het apparaat controleren grafieken werken niet als eenvoudige of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. | Wijzig de configuratie van de web-proxy voor het apparaat dat is geregistreerd bij uw Manager StorSimple-service zodat dat verificatie is ingesteld op geen. Klik hiertoe uitvoeren de de Windows PowerShell voor de cmdlet Set-HcsWebProxy van StorSimple. | Ja | Ja |
| 6 | Opslag-accounts | Gebruik van de Storage-service het account opslagruimte verwijderen is een niet-ondersteunde scenario. Dit leidt tot een situatie waarin gebruikersgegevens kan niet worden opgehaald. | | Ja | Ja |
| 7 | Failback | Een failback binnen 24 uur na herstel (DR) wordt niet ondersteund. | | Ja | Nee |
| 8 | Apparaat failover | Meerdere failovers van een container volume van het bronapparaat met dezelfde voor verschillende apparaten wordt niet ondersteund. Failover vanaf een apparaat met één dode naar meerdere apparaten, kunt u het volume containers op de eerste apparaat overgenomen Gegevenseigendom kwijtraakt. Na een failover wordt deze containers volume worden weergegeven of gedragen zich anders wanneer u deze in de portal van Azure klassieke weergeven. | | Ja | Nee |
| 9 | Installatie | Tijdens StorSimple Adapter voor SharePoint-installatie moet u een apparaat IP-adres voor de installatie te voltooien. | | Ja | Nee |
| 10 | Netwerkinterfaces | Netwerkinterfaces gegevens 2 en 3 van de gegevens zijn omgewisseld in de software. | Neem contact op met Microsoft Support als u moet deze interfaces configureren. | Ja | Nee |


 
