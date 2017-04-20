<properties 
   pageTitle="Weergeven en beheren van StorSimple waarschuwingen | Microsoft Azure"
   description="Beschrijving StorSimple waarschuwing voorwaarden en ernst, waarschuwingen configureren en het gebruik van de service StorSimple Manager om waarschuwingen te beheren."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>De service StorSimple Manager gebruiken om te bekijken en StorSimple waarschuwingen beheren

## <a name="overview"></a>Overzicht

Het tabblad **waarschuwingen** in de StorSimple Manager-service biedt een manier om te controleren en StorSimple apparaat – meldingen op basis van realtime wissen. Vanuit dit tabblad kunt u de servicestatusproblemen zijn van uw StorSimple-apparaten en de algehele StorSimple van Microsoft Azure-oplossing centraal controleren.

Deze zelfstudie worden veelgebruikte voorwaarden die zijn waarschuwingen, waarschuwing niveaus en hoe u meldingen voor waarschuwingen configureren. Daarnaast bevat deze waarschuwing Snelzoekgids voor tabellen, waarmee u kunt snel een melding voor een specifieke zoek en correct reageren.

![Pagina waarschuwingen](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Veelgebruikte voorwaarden die waarschuwing

Uw apparaat StorSimple genereert waarschuwingen in antwoord op een aantal voorwaarden. Hier volgen de meest voorkomende waarschuwing voorwaarden:

- **Problemen met hardware** – deze meldingen geven over de status van de hardware. Ze laten u weten als firmware upgrades nodig zijn, als een netwerkinterface problemen heeft of als er een probleem is met een van uw gegevensstations.

- **Verbindingsproblemen** – deze waarschuwingen optreden wanneer er sprake is van problemen bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit het Azure opslag-account of door gebrek aan connectiviteit tussen de apparaten en de StorSimple Manager-service. Communicatieproblemen zijn enkele van de moeilijkst om op te lossen omdat er zo veel punten van is mislukt. U moet altijd eerst controleren of dat netwerkconnectiviteit en internettoegang beschikbaar zijn voordat u verdergaat met meer geavanceerde probleemoplossing. Voor hulp bij het oplossen, gaat u naar [problemen met de cmdlet verbinding testen](storsimple-troubleshoot-deployment.md).

- **Prestatieproblemen** – deze meldingen worden veroorzaakt wanneer uw systeem is niet optimaal, zoals wanneer dit een druk bezet is.

Daarnaast ziet u mogelijk waarschuwingen betreffende beveiliging, updates of taak fouten.

## <a name="alert-severity-levels"></a>Waarschuwing niveaus

Waarschuwingen hebben verschillende niveaus, afhankelijk van de invloed van de waarschuwing situatie heeft en dat u een antwoord op de melding. De niveaus zijn:

- **Kritiek** – deze melding wordt in antwoord op een voorwaarde die dat dit gevolgen de succesvolle prestaties van uw systeem heeft is. Actie is vereist om ervoor te zorgen dat de service wordt niet onderbroken StorSimple.

- **Waarschuwing** – met deze voorwaarde kritiek kan worden als niet opgelost. U moet de situatie onderzoeken en schakelt u het probleem moet actie ondernemen.

- **Informatie** : deze melding bevat informatie die gebruikt worden kan voor het bijhouden en beheren van uw systeem.

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt kiezen of u wilt worden gewaarschuwd per e-mail meldingen voorwaarden voor elk van uw StorSimple-apparaten. Bovendien kunt u andere geadresseerden waarschuwingsbericht identificeren door hun e-mailadressen invoeren in de **andere geadresseerden van het e** -vak, gescheiden door puntkomma's.

>[AZURE.NOTE] U kunt maximaal 20 e-mailadressen per apparaat.

Nadat u een e-mailmelding voor een apparaat hebt ingeschakeld, ontvangt de leden van de meldingslijst een e-mailbericht telkens wanneer een melding voor een kritieke plaatsvindt. De berichten worden verzonden vanuit *storsimple-alerts-noreply@mail.windowsazure.com* en wordt de waarschuwing voorwaarde beschrijven. Geadresseerden kunnen klikken op **Afmelden** om zelf te verwijderen uit de lijst met e-melding.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>E-mailmelding van waarschuwingen voor een apparaat inschakelen

1. Ga naar **apparaten** > **configureren** voor het apparaat.

2. Klik onder **Instellingen voor meldingen**instellen het volgende:

    1. Selecteer **Ja**in het veld **e-mailbericht verzenden** .

    2. In het veld **e-servicebeheerders** Selecteer **Ja** als u wilt de service-beheerder hebt en alle CO-beheerders de waarschuwing meldingen ontvangen.

    3. Voer in het veld **andere e-geadresseerden** de e-mailadressen van alle andere geadresseerden waaraan u de waarschuwingen toekennen. Voer de namen in de indeling *someone@somewhere.com*. Gebruik puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per apparaat configureren. 

        ![Waarschuwingen Meldingsconfiguratie](./media/storsimple-manage-alerts/AlertNotify.png)

3. Als u wilt een test-e-mailbericht verzenden, klikt u op het pictogram met de pijl naast **een test e-mail verzenden**. De StorSimple Manager-service wordt statusberichten weergegeven als de test-melding wordt doorgestuurd. 

4. Wanneer het volgende bericht wordt weergegeven, klikt u op **OK**. 

    ![Meldingen testen verzonden e-mailmelding](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Als het meldingsbericht voor de test niet kan worden verzonden, wordt de StorSimple Manager-service een toepasselijk bericht weergegeven. Klik op **OK**, wacht enkele minuten en probeer het opnieuw te verzenden uw Meldingsbericht test. 

## <a name="view-and-track-alerts"></a>Weergeven en bijhouden van meldingen

Het servicedashboard van de StorSimple Manager biedt u een snelle blik op het aantal waarschuwingen op uw apparaten, gerangschikt op prioriteitsniveau.

![Waarschuwingen dashboard](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Het prioriteitsniveau klikken, opent het tabblad **waarschuwingen** . De resultaten zijn alleen de meldingen die overeenkomen met die prioriteitsniveau.

![Waarschuwingen rapport beperkt tot soort bericht](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Een melding in de lijst klikken biedt aanvullende details voor de melding, met inbegrip van de laatste keer dat de melding is gerapporteerd, het aantal exemplaren van de melding op het apparaat en de aanbevolen actie voor het oplossen van de melding. Als dit een melding voor een hardware is, wordt dit ook het hardwareonderdeel identificeren.

![Voorbeeld van waarschuwing op hardware](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de informatie in de waarschuwing kopiëren naar een tekstbestand. Nadat u hebt uitgevoerd van de aanbeveling en opgelost de waarschuwing voorwaarde on-premises implementatie, moet u de melding van het apparaat wissen door te schakelen van de melding op het tabblad **waarschuwingen** en klikken op **verwijderen**. Selecteer elke waarschuwing als u wilt wissen meerdere waarschuwingen, op een willekeurige kolom, behalve de **melding voor een** kolom en klik vervolgens op **wissen** nadat u alle meldingen wilt wissen hebt geselecteerd. Houd er rekening mee dat sommige waarschuwingen automatisch zijn uitgeschakeld wanneer het probleem opgelost is of wanneer het systeem de melding met nieuwe gegevens bijwerkt.

Wanneer u op **wissen**klikt, hebt u de mogelijkheid om opmerkingen over de waarschuwing en de stappen die u hebt gemaakt, het probleem op te lossen. Sommige gebeurtenissen worden gewist door het systeem als een ander evenement met nieuwe informatie wordt geactiveerd. In dat geval ziet u het volgende bericht weergegeven.

![Schakel het selectievakje waarschuwingsbericht](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Waarschuwingen voor sorteren en controleren

U vindt het mogelijk efficiënter uitvoeren van rapporten op waarschuwingen, zodat u kunt bekijken en schakelt u ze in groepen. Het tabblad **waarschuwingen** kunt daarnaast kunnen maximaal 250 meldingen weergeven. Als u dat aantal waarschuwingen hebben overschreden, worden niet alle meldingen worden weergegeven in de standaardweergave. U kunt de volgende velden om aan te passen waarschuwingen worden weergegeven combineren:

- **Status** : U kunt **actief** of **uitgeschakeld** meldingen weergeven. Actieve waarschuwingen zijn nog steeds geactiveerd op uw systeem, terwijl uitgeschakeld waarschuwingen zijn handmatig uitgeschakeld door een beheerder of via een programma uitgeschakeld omdat het systeem de waarschuwing voorwaarde met nieuwe informatie bijgewerkt.

- **Ernst** : U kunt meldingen van alle niveaus (kritieke, waarschuwing, informatie) of alleen een bepaalde ernst, zoals alleen kritieke meldingen weergeven.

- **Bron** : U kunt meldingen van alle bronnen weergeven of de meldingen die afkomstig van de service of één of alle apparaten zijn te beperken.

- **Tijdsbereik** – door het opgeven van de **van** en **tot** datums en tijdstempels, kunt u zoeken in waarschuwingen tijdens de periode waarin u geïnteresseerd bent.

## <a name="alerts-quick-reference"></a>Naslaggids voor meldingen

De volgende tabellen bevatten aantal Microsoft Azure StorSimple waarschuwingen die optreden kunnen, alsmede aanvullende informatie en aanbevelingen indien beschikbaar. StorSimple apparaat waarschuwingen vallen in een van de volgende categorieën:

- [Cloud connectivity waarschuwingen](#cloud-connectivity-alerts)

- [Cluster waarschuwingen](#cluster-alerts)

- [Waarschuwingen voor noodgevallen herstel](#disaster-recovery-alerts)

- [Hardware waarschuwingen](#hardware-alerts)

- [Foutmeldingen](#job-failure-alerts)

- [Lokaal vastgemaakte volume-meldingen](#locally-pinned-volume-alerts)

- [Netwerken waarschuwingen](#networking-alerts)

- [Prestaties van meldingen](#performance-alerts)

- [Beveiligingsmeldingen](#security-alerts)

- [Ondersteuning voor pakket waarschuwingen](#support-package-alerts)

- [Waarschuwingen bijwerken](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Cloud connectivity waarschuwingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Connectiviteit met <*cloud referentie naam*> kan niet worden gemaakt.|Kan geen verbinding maken met het account opslag.|Het lijkt erop dat er mogelijk een verbindingsprobleem met de met uw apparaat. Voer de `Test-HcsmConnection` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat herkennen en verhelpt het probleem. Als de instellingen juist zijn, wordt het probleem mogelijk met de referenties van de opslag-account waarvoor u de melding is verheven. In dit geval gebruikt de `Test-HcsStorageAccountCredential` cmdlet om te bepalen of er zijn problemen die u kunt oplossen.<ul><li>Controleer uw netwerkinstellingen.</li><li>Controleer de referenties van uw opslag-account.</li></ul>|
|We hebben een heartbeat niet ontvangen van uw apparaat voor de laatste <*getal*> minuten.|Kan geen verbinding maken met apparaat.|Het lijkt erop dat er een verbindingsprobleem met de met uw apparaat is. Gebruik de `Test-HcsmConnection` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat identificeren en verhelpt het probleem of neem contact op met uw netwerkbeheerder.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple gedrag wanneer cloud connectivity mislukt

Wat gebeurt er als cloud-connectiviteit voor mijn StorSimple apparaat uitgevoerd in productie mislukt?

Als cloud connectivity op uw apparaat van de productie StorSimple mislukt, klikt u vervolgens afhankelijk van de status van uw apparaat met de volgende kan zich voordoen: 

- **Voor de lokale gegevens op uw apparaat**: voor enige tijd, wordt er geen onderbreking en leest blijft worden aangeboden. Als het aantal uitstaande IOs verhogen en een limiet overschrijdt, kunnen echter gelezen mislukt starten. 

    Afhankelijk van de hoeveelheid gegevens op uw apparaat blijft de schrijft ook optreden voor de eerste paar uur na de onderbreking in de cloud-connectiviteit. Het schrijven wordt vervolgens vertragen en begint u uiteindelijk mislukt als de cloud-connectiviteit voor enkele uren wordt onderbroken. (Er tijdelijke opslag is op het apparaat voor gegevens die moeten worden verplaatst naar de cloud. In dit gebied wordt leeggemaakt wanneer de gegevens worden verzonden. Als connectivity mislukt, gegevens in deze opslaggebied wordt niet worden doorgevoerd in de cloud, en IO, mislukt.)   

 
- **Voor de gegevens in de cloud**: op de meeste cloud connectivity fouten, wordt een fout geretourneerd. Nadat de verbinding is hersteld, worden de IOs hervat zonder dat de gebruiker om het volume online weer te geven. In uitzonderlijke gevallen tussenkomst mogelijk nodig terug online het volume van de Azure klassieke portal overbrengen. 
 
- **Voor cloud momentopnamen Bezig**: de bewerking opnieuw een paar keer binnen 4 en 5 uur wordt uitgevoerd en als de verbinding niet is hersteld, de cloud-momentopnamen, mislukt.


### <a name="cluster-alerts"></a>Cluster waarschuwingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Apparaat niet via <*apparaatnaam*>.|Er is een apparaat in de onderhoudsmodus voor.|Apparaat niet via vanwege invoeren of het onderhoudsmodus afsluiten. Dit is normaal en geen actie nodig is. Nadat u deze melding hebt bevestigd, schakelt u het op de pagina waarschuwingen.|
|Apparaat niet via <*apparaatnaam*>.|Apparaatfirmware of software is alleen bijgewerkt.|Er is een clusterfailover vanwege een update. Dit is normaal en geen actie nodig is. Nadat u deze melding hebt bevestigd, schakelt u het op de pagina waarschuwingen.|
|Apparaat niet via <*apparaatnaam*>.|Controller is afgesloten of opnieuw gestart.|Apparaat niet via omdat de actieve controller is afgesloten of opnieuw worden gestart door een beheerder. Geen actie nodig is. Nadat u deze melding hebt bevestigd, schakelt u het op de pagina waarschuwingen.|
|Apparaat niet via <*apparaatnaam*>.|Geplande failover.|Controleren of dit is een geplande failover. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|
|Apparaat niet via <*apparaatnaam*>.|Niet-geplande failover.|StorSimple is ontworpen om u te automatisch herstellen uit niet-geplande failover. Als u een groot aantal deze waarschuwingen ziet, neemt u contact op met Microsoft Support.|
|Apparaat niet via <*apparaatnaam*>.|Andere/onbekende oorzaak.|Als u een groot aantal deze waarschuwingen ziet, neemt u contact op met Microsoft Support. Nadat het probleem opgelost is, schakelt u deze melding op de pagina waarschuwingen.|
|Een service kritieke apparaat rapporten status, zoals is mislukt.|DataPath service is mislukt. |Neem contact op met Microsoft Support voor hulp.|
|Virtuele IP-adres voor netwerkinterface <*gegevens #*> status rapporten, zoals is mislukt. |Andere/onbekende oorzaak. |Soms tijdelijke voorwaarden kunnen leiden tot deze waarschuwingen. Als dit het geval is, wordt klikt u vervolgens deze melding worden automatisch uitgeschakeld na het enige tijd. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support.|
|Virtuele IP-adres voor netwerkinterface <*gegevens #*> status rapporten, zoals is mislukt.|De naam van de gebruikersinterface: <*gegevens #*> IP-adres <IP address> online kan niet worden gebracht omdat een dubbele IP-adres op het netwerk is gevonden. |Zorg ervoor dat het dubbele IP-adres is verwijderd uit het netwerk of opnieuw configureren de interface met een ander IP-adres.|


### <a name="disaster-recovery-alerts"></a>Waarschuwingen voor noodgevallen herstel

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Herstelbewerkingen kunnen alle instellingen voor deze service niet herstellen. Configuratiegegevens apparaat is een inconsistente status voor sommige apparaten.|Inconsistenties in de gegevens gedetecteerd na herstel.|Versleutelde gegevens op de service wordt niet gesynchroniseerd met die op het apparaat. Autoriseer het apparaat <*apparaatnaam*> vanuit StorSimple Manager om de synchronisatieproces te starten. De Windows PowerShell-Interface voor StorSimple gebruiken om uit te voeren de `Restore-HcsmEncryptedServiceData` leveren op apparaat <*apparaatnaam*> cmdlet, het oude wachtwoord als invoer voor deze cmdlet herstellen van het beveiligingsprofiel. Voer de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet de service gegevens versleutelingssleutel bijgewerkt. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|


### <a name="hardware-alerts"></a>Hardware waarschuwingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Hardware-onderdeel <*onderdeel-ID*> rapporten status als <*status*>.||Soms tijdelijke voorwaarden kunnen leiden tot deze waarschuwingen. Als dat het geval is, wordt deze waarschuwing automatisch uitgeschakeld na het enige tijd. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support.|
|Passieve controller defect.|De passieve controller uit (secundaire) werkt niet.|Uw apparaat is ingeschakeld, maar een van uw werkt niet goed. Probeer opnieuw te starten dat controller. Als het probleem niet is opgelost, neemt u contact op met Microsoft Support.|

### <a name="job-failure-alerts"></a>Foutmeldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Back-up van <*bron volume groeps-ID*> is mislukt.|Back-uptaak is mislukt.|Verbindingsproblemen kunnen worden voorkomen van de back-up is voltooid. Als er geen verbindingsproblemen, is het mogelijk dat u het maximum aantal back-ups hebt bereikt. Verwijder eventuele back-ups die niet meer nodig zijn en probeer het opnieuw. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|
|Klonen van <*bron back-element id's*> <*bestemming volume seriële getallen*> is mislukt.|Klonen taak is mislukt.|Hiermee vernieuwt u de back-lijst om te bevestigen dat de back-up nog geldig is. Als de back-up geldig is, is het mogelijk dat cloud-verbindingsproblemen verhinderen dat de bewerking klonen is voltooid. Als er geen verbindingsproblemen, is het mogelijk dat u de opslaglimiet hebt bereikt. Verwijder eventuele back-ups die niet meer nodig zijn en probeer het opnieuw. Nadat u de gewenste actie voor het oplossen van het probleem hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|
|Terugzetten van <*bron back-element id's*> is mislukt.|Dit is een terugzettaak is mislukt.|Hiermee vernieuwt u de back-lijst om te bevestigen dat de back-up nog geldig is. Als de back-up geldig is, is het mogelijk dat cloud-verbindingsproblemen verhinderen dat de bewerking voor terugzetten is voltooid. Als er geen verbindingsproblemen, is het mogelijk dat u de opslaglimiet hebt bereikt. Verwijder eventuele back-ups die niet meer nodig zijn en probeer het opnieuw. Nadat u de gewenste actie voor het oplossen van het probleem hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|

### <a name="locally-pinned-volume-alerts"></a>Lokaal vastgemaakte volume-meldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Het maken van lokale volume <*volumenaam*> is mislukt.| Een taak voor het maken van het volume is mislukt. <*Fout bericht overeenkomt met de mislukte foutcode*>.|Mogelijk verbindingsproblemen verhinderd door de bewerking voor het maken van ruimte is voltooid. Lokaal vastgemaakte volumes dik is ingericht en het proces van het maken van de ruimte heeft betrekking op lopen doorverbonden hoeveelheden die moeten de cloud. Als er geen verbindingsproblemen, is het mogelijk dat u de lokale ruimte op het apparaat hebt leeg. Bepalen of ruimte aanwezig is op het apparaat voordat u deze opnieuw.|
|Uitbreiding van lokale volume <*volumenaam*> is mislukt.|De taak van de wijziging volume is mislukt vanwege <*fout bericht overeenkomt met de mislukte foutcode*>.|Mogelijk verbindingsproblemen verhinderd door de volume uitbreiding bewerking is voltooid. Lokaal vastgemaakte volumes dik is ingericht en het proces voor het uitbreiden van de bestaande ruimte heeft betrekking op lopen doorverbonden hoeveelheden die moeten de cloud. Als er geen verbindingsproblemen, is het mogelijk dat u de lokale ruimte op het apparaat hebt leeg. Bepalen of ruimte aanwezig is op het apparaat voordat u deze opnieuw.|
|Conversie van volume <*volumenaam*> is mislukt.|De taak van de conversie het volume van het volumetype uit lokaal converteren vastgemaakt aan het doorverbonden is mislukt.|Conversie van het volume van het type lokaal vastgemaakt aan doorverbonden kan niet worden voltooid. Zorg ervoor dat er geen verbindingsproblemen voorkomen van de bewerking wordt voltooid. Ga naar [problemen met de Test-HcsmConnection-cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)voor het oplossen van problemen met de serververbinding.<br>Het volume van het oorspronkelijke lokaal vastgemaakte is nu gemarkeerd als een doorverbonden volume sinds enkele van de gegevens uit het lokaal vastgemaakte volume heeft zich verspreid in de cloud tijdens de conversie. Het volume van de resulterende doorverbonden is nog steeds lokale ruimte op het apparaat dat kan niet opnieuw worden gebruikt voor toekomstige lokale volumes die.<br>Eventuele verbindingsproblemen oplossen, schakelt u de melding en dit volume terug converteren naar de oorspronkelijke type lokaal vastgemaakte volume om ervoor te zorgen dat alle gegevens beschikbaar wordt gesteld lokaal opnieuw.|
|Conversie van volume <*volumenaam*> is mislukt.|De taak van de conversie volume converteren van het volumetype uit doorverbonden naar lokaal vastgemaakt is mislukt.|Conversie van het volume van het type gelaagde naar lokaal vastgemaakt kan niet worden voltooid. Zorg ervoor dat er geen verbindingsproblemen voorkomen van de bewerking wordt voltooid. Ga naar [problemen met de Test-HcsmConnection-cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)voor het oplossen van problemen met de serververbinding.<br>Het oorspronkelijke doorverbonden volume nu gemarkeerd als een lokaal vastgemaakte volume als onderdeel van de conversie heeft nog steeds gegevens in de cloud, terwijl de dik ingerichte ruimte op het apparaat voor dit volume niet meer kunt vrijgemaakt voor toekomstige lokale volumes.<br>Eventuele verbindingsproblemen oplossen, schakelt u de melding en dit volume terug converteren naar de oorspronkelijke type gelaagde volume om ervoor te zorgen lokale ruimte dik deze is ingericht op het apparaat opnieuw kan worden gebruikt.|
|Lokale ruimte verbruik voor lokale momentopnamen van bijna <*groepsnaam voor volume*>|Lokale momentopnamen voor het back-beleid mogelijk geen ruimte meer snel uitvoeren en om te voorkomen dat host schrijven fouten zijn ongeldig.|Veelgebruikte lokale momentopnamen samen met een hoge gegevens lospeuteren van de hoeveelheid die is gekoppeld aan deze groep Back-beleid veroorzaken lokale ruimte op het apparaat te snel worden gebruikt. Verwijder eventuele lokale momentopnamen die niet meer nodig zijn. Ook uw lokale momentopname planningen dit back-beleid minder veelgebruikte lokale momentopnamen en zorg ervoor dat cloud regelmatig momentopnamen worden bijwerken. Als deze acties worden niet die u hebt gemaakt, lokale ruimte voor deze momentopnamen mogelijk binnenkort leeg en het systeem automatisch verwijdert u deze om ervoor te zorgen dat host schrijft blijven worden verwerkt.|
|Lokale momentopnamen voor <*groepsnaam volume*> zijn ongeldig gemaakt.|De lokale momentopnamen voor <*groepsnaam volume*> zijn ongeldig en vervolgens verwijderd omdat ze zijn overschrijden van de lokale ruimte op het apparaat.|Om ervoor te zorgen dat dit niet in de toekomst terugkerende, de lokale momentopname planningen voor dit back-beleid bekijken en verwijderen van een lokale momentopnamen die niet meer nodig zijn. Veelgebruikte lokale momentopnamen samen met een hoge gegevens lospeuteren van de hoeveelheid die is gekoppeld aan deze groep Back-beleid kunnen ertoe leiden dat lokale ruimte op het apparaat te snel worden gebruikt.|
|Terugzetten van <*bron back-element id's*> is mislukt.|De terugzettaak is mislukt.|Als u lokaal hebt vastgemaakt of een combinatie van lokaal vastgemaakte en doorverbonden hoeveelheden in dit back-beleid, vernieuwt u de back-lijst om te bevestigen dat is de back-up nog steeds geldig. Als de back-up geldig is, is het mogelijk dat cloud-verbindingsproblemen verhinderen dat de bewerking voor terugzetten is voltooid. De lokaal vastgemaakte hoeveelheden die zijn wordt teruggezet als onderdeel van deze groep momentopname niet beschikt over alle hun gegevens dan ook gedownload naar het apparaat en hebt u een combinatie van trapsgewijze en lokaal vastgemaakte in deze groep momentopname, ze worden niet gesynchroniseerd met elkaar. Om te kunnen voltooien het terugzetten, de hoeveelheden maken in deze groep offline op de host en probeer het opnieuw herstellen. Houd er rekening mee dat eventuele wijzigingen aan de volumegegevens die zijn uitgevoerd tijdens het herstelproces verloren gaan.|

### <a name="networking-alerts"></a>Netwerken waarschuwingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|StorSimple (s) kan niet worden gestart.|DataPath fout |Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support.|
|Dubbele IP-adres voor 'Data0' gedetecteerd.| |Het is een conflict voor IP-adres '10.0.0.1' gevonden. De resource netwerk 'Data0' op het apparaat *<device1>* offline is. Zorg ervoor dat dit IP-adres niet wordt gebruikt door een andere entiteit in dit netwerk. Om op te lossen netwerkproblemen, gaat u naar [problemen met de cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Neem contact op met uw netwerkbeheerder voor hulp bij het oplossen van dit probleem. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support. |
|IPv4-(of IPv6) adres voor 'Data0' is offline.| |De resource netwerk 'Data0' met IP-adres '10.0.0.1.' en prefix-lengte '22' op het apparaat *<device1>* offline is. Zorg ervoor dat de schakeloptie poorten waaraan deze interface is verbonden operationele. Om op te lossen netwerkproblemen, gaat u naar [problemen met de cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Prestaties van meldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Het laden van het apparaat heeft overschreden <*drempelwaarde*>.|Langzamer dan verwacht antwoord tijden.|Uw apparaat rapporten gebruik onder een dik invoer/uitvoer laden. Hierdoor kan uw apparaat niet werkt ook dat wordt weergegeven. Bekijk de werkbelasting die u hebt toegevoegd aan het apparaat en bepalen of er zijn die kan worden verplaatst naar een ander apparaat of die niet meer nodig zijn.<br>Als u wilt weten over de huidige status, gaat u naar [de StorSimple Manager-service te controleren van uw apparaat gebruiken](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Beveiligingsmeldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Microsoft Support-sessie is begonnen.|Externe toegang tot support-sessie.|Bevestig dat dit toegang wordt geverifieerd. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|
|Wachtwoord voor <*element*> verloopt over <*tijdsduur*>.|Bijna verlopen wachtwoorden nadert.|Uw wachtwoord wijzigen voordat het verloopt.|
|Configuratiegegevens voor de beveiliging voor <*element-ID*> ontbreken.||De hoeveelheden die is gekoppeld aan deze container volume kunnen niet worden gebruikt om de configuratie van uw StorSimple te repliceren. Om ervoor te zorgen dat uw gegevens veilig worden opgeslagen, is het raadzaam dat u de container volume en alle volumes die is gekoppeld aan de container volume verwijderen. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.|
|<*getal*> login pogingen is mislukt voor <*element-ID*>.|Meerdere aanmeldingspoging mislukte.|Uw apparaat mogelijk onder aanval of een geautoriseerde gebruiker probeert te communiceren met een onjuist wachtwoord.<ul><li>Neem contact op met uw gemachtigde gebruikers en controleer of dat deze pogingen van een betrouwbare bron zijn. Als u verder gaat om een groot aantal mislukte aanmelding pogingen weer te geven, kunt u uitschakelen van extern beheer en contact opneemt met de netwerkbeheerder. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.</li><li>Controleer of uw Manager momentopname-sessies zijn geconfigureerd met het juiste wachtwoord. Nadat u de gewenste actie hebt besteed, schakelt u deze melding op de pagina waarschuwingen.</li></ul>Ga naar [een verlopen apparaatwachtwoord wijzigen](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password)voor meer informatie.|
|Een of meer fouten opgetreden tijdens het wijzigen van de service gegevenssleutel.||Er zijn fouten opgetreden bij het wijzigen van de service gegevenssleutel. Nadat u de foutvoorwaarden hebt verwerkt, voert u uit de `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet uit de Windows PowerShell-Interface voor StorSimple op uw apparaat bijwerken van de service. Als dit probleem zich blijft voordoen, neemt u contact op met Microsoft ondersteuning. Nadat u het probleem opgelost, schakelt u deze melding op de pagina waarschuwingen.|

### <a name="support-package-alerts"></a>Ondersteuning voor pakket waarschuwingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Maken van ondersteuningspakket is mislukt.|StorSimple kan niet het pakket genereren.|Probeer deze bewerking opnieuw. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support. Nadat u het probleem opgelost hebt, schakelt u deze melding op de pagina waarschuwingen.|

### <a name="update-alerts"></a>Waarschuwingen bijwerken

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Hotfix is geïnstalleerd.|De update van de software/firmware is voltooid.|De hotfix is geïnstalleerd op uw apparaat.|
|Handmatige updates beschikbaar.|Een melding van de beschikbare updates.|Via de Windows PowerShell-Interface voor StorSimple op uw apparaat deze updates installeren. <br>Ga naar het [bijwerken van uw apparaat StorSimple 8000 reeks](storsimple-update-device.md)voor meer informatie.|
|Nieuwe updates beschikbaar.|Een melding van de beschikbare updates.|Via **de onderhoudspagina voor** of met behulp van de Windows PowerShell-Interface voor StorSimple op uw apparaat, kunt u deze updates installeren. <br>Ga naar het [bijwerken van uw apparaat StorSimple 8000 reeks](storsimple-update-device.md)voor meer informatie.|
|Updates installeren is mislukt.|Updates zijn niet geïnstalleerd.|Uw systeem is niet mogen de updates installeren. Via **de onderhoudspagina voor** of met behulp van de Windows PowerShell-Interface voor StorSimple op uw apparaat, kunt u deze updates installeren. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support. <br>Ga naar het [bijwerken van uw apparaat StorSimple 8000 reeks](storsimple-update-device.md)voor meer informatie.|
|Kan niet automatisch controleren op nieuwe updates.|Automatische controle is mislukt.|U kunt handmatig controleren op nieuwe updates van **de onderhoudspagina voor** .|
|Nieuwe WUA agent beschikbaar.|Een melding van de beschikbare update.|De meest recente Windows Update-Agent downloaden en installeren vanaf de Windows PowerShell-interface.|
|Versie van firmware onderdeel <*onderdeel-ID*> komen niet overeen met hardware.|Firmware-updates zijn niet geïnstalleerd.|Neem contact op met Microsoft ondersteuning.|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple fouten en probleemoplossing bij een operationele-apparaat](storsimple-troubleshoot-operational-device.md).

