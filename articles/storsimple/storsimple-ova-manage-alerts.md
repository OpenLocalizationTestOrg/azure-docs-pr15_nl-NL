<properties 
   pageTitle="Weergeven en beheren van StorSimple virtuele matrix waarschuwingen | Microsoft Azure"
   description="Beschrijving StorSimple virtuele matrix waarschuwing voorwaarden en ernst en het gebruik van de service StorSimple Manager om waarschuwingen te beheren."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>Gebruik van de service StorSimple Manager weergeven en beheren van meldingen voor de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

Het tabblad **waarschuwingen** in de StorSimple Manager-service biedt een manier om te controleren en meldingen StorSimple virtuele matrix – gerelateerd op basis van realtime wissen. Vanuit dit tabblad kunt u de servicestatusproblemen zijn van uw StorSimple virtuele matrices (ook wel bekend als StorSimple on-premises implementatie virtuele apparaten) en de algehele StorSimple van Microsoft Azure-oplossing centraal controleren.

Deze zelfstudie wordt beschreven hoe u configureert waarschuwingen, veelgebruikte voorwaarden die waarschuwing, waarschuwingen niveaus en hoe u weergeven en bijhouden van waarschuwingen. Daarnaast bevat deze waarschuwing Snelzoekgids voor tabellen, waarmee u kunt snel een melding voor een specifieke zoek en correct reageren.

![Pagina waarschuwingen](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt kiezen of u wilt worden gewaarschuwd per e-mail meldingen voorwaarden voor elk van uw StorSimple virtuele apparaten. Bovendien kunt u andere geadresseerden waarschuwingsbericht identificeren door hun e-mailadressen invoeren in de **andere geadresseerden van het e** -vak, gescheiden door puntkomma's.

>[AZURE.NOTE] U kunt maximaal 20 e-mailadressen per virtuele apparaat kunt invoeren.

Nadat u een e-mailmelding voor een virtueel apparaat hebt ingeschakeld, ontvangt de leden van de meldingslijst een e-mailbericht telkens wanneer een melding voor een kritieke plaatsvindt. De berichten worden verzonden vanuit *storsimple-alerts-noreply@mail.windowsazure.com* en wordt de waarschuwing voorwaarde beschrijven. Geadresseerden kunnen klikken op **Afmelden** om zelf te verwijderen uit de lijst met e-melding.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>E-mailmelding van waarschuwingen voor een virtueel apparaat inschakelen

1. Ga naar **apparaten** > **configuratie** voor het virtuele apparaat. Ga naar de sectie **Instellingen voor meldingen** .

    ![instellingen voor meldingen](./media/storsimple-ova-manage-alerts/alerts2.png)

2. Klik onder **Instellingen voor meldingen**instellen het volgende:

    1. Selecteer **Ja**in het veld **e-mailbericht verzenden** .

    2. In het veld **e-servicebeheerders** Selecteer **Ja** als u wilt de service-beheerder hebt en alle CO-beheerders de waarschuwing meldingen ontvangen.

    3. Voer in het veld **andere e-geadresseerden** de e-mailadressen van alle andere geadresseerden waaraan u de waarschuwingen toekennen. Voer de namen in de indeling *someone@somewhere.com*. Gebruik puntkomma te scheiden van de e-mailadressen. U kunt maximaal 20 e-mailadressen per virtuele apparaat configureren. 

        ![waarschuwingen Meldingsconfiguratie](./media/storsimple-ova-manage-alerts/alerts3.png)

3. Onderaan op de pagina, klikt u op **Opslaan** om op te slaan van uw configuratie.

4. Als u wilt een test-e-mailbericht verzenden, klikt u op het pictogram met de pijl naast **een test e-mail verzenden**. De StorSimple Manager-service wordt statusberichten weergegeven als de test-melding wordt doorgestuurd. 

5. Wanneer het volgende bericht wordt weergegeven, klikt u op **OK**. 

    ![Meldingen testen verzonden e-mailmelding](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Als het meldingsbericht voor de test niet kan worden verzonden, wordt de StorSimple Manager-service een toepasselijk bericht weergegeven. Klik op **OK**, wacht enkele minuten en probeer het opnieuw te verzenden uw Meldingsbericht test. 

    Het meldingsbericht voor de test worden de volgende strekking.

    ![Waarschuwingen testen e-voorbeeld](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Veelgebruikte voorwaarden die waarschuwing

Uw StorSimple virtuele matrix genereert waarschuwingen in antwoord op een aantal voorwaarden. Hier volgen de meest voorkomende waarschuwing voorwaarden:

- **Verbindingsproblemen** – deze waarschuwingen optreden wanneer er sprake is van problemen bij het overbrengen van gegevens. Communicatieproblemen kunnen optreden tijdens de overdracht van gegevens naar en vanuit het Azure opslag-account of door gebrek aan connectiviteit tussen de virtuele apparaten en de StorSimple Manager-service. Communicatieproblemen zijn enkele van de moeilijkst om op te lossen omdat er zo veel punten van is mislukt. U moet altijd eerst controleren of dat netwerkconnectiviteit en internettoegang beschikbaar zijn voordat u verdergaat met meer geavanceerde probleemoplossing. Ga naar [StorSimple virtuele matrix systeemvereisten](storsimple-ova-system-requirements.md)voor informatie over poorten en firewallinstellingen. Voor hulp bij het oplossen, gaat u naar [problemen met de cmdlet verbinding testen](storsimple-troubleshoot-deployment.md).

- **Prestatieproblemen** – deze meldingen worden veroorzaakt wanneer uw systeem is niet optimaal, zoals wanneer dit een druk bezet is.

Daarnaast ziet u mogelijk waarschuwingen betreffende beveiliging, updates of taak fouten.

## <a name="alert-severity-levels"></a>Waarschuwing niveaus

Waarschuwingen hebben verschillende niveaus, afhankelijk van de invloed van de waarschuwing situatie heeft en dat u een antwoord op de melding. De niveaus zijn:

- **Kritiek** – deze melding wordt in antwoord op een voorwaarde die dat dit gevolgen de succesvolle prestaties van uw systeem heeft is. Actie is vereist om ervoor te zorgen dat de service wordt niet onderbroken StorSimple.

- **Waarschuwing** – met deze voorwaarde kritiek kan worden als niet opgelost. U moet de situatie onderzoeken en schakelt u het probleem moet actie ondernemen.

- **Informatie** : deze melding bevat informatie die gebruikt worden kan voor het bijhouden en beheren van uw systeem.

## <a name="view-and-track-alerts"></a>Weergeven en bijhouden van meldingen

Het servicedashboard van de StorSimple Manager biedt u een snelle blik op het aantal waarschuwingen op uw virtuele apparaten, gerangschikt op prioriteitsniveau.

![Waarschuwingen dashboard](./media/storsimple-ova-manage-alerts/alerts6.png)

Het prioriteitsniveau klikken, opent het tabblad **waarschuwingen** . De resultaten zijn alleen de meldingen die overeenkomen met die prioriteitsniveau.

![Waarschuwingen rapport beperkt tot soort bericht](./media/storsimple-ova-manage-alerts/alerts7.png)

Een melding in de lijst klikken biedt aanvullende details voor de melding, met inbegrip van de laatste keer dat de melding is gerapporteerd, het aantal exemplaren van de melding op het apparaat en de aanbevolen actie voor het oplossen van de melding.

Als u de gegevens te verzenden naar Microsoft Support nodig hebt, kunt u de informatie in de waarschuwing kopiëren naar een tekstbestand. Nadat u hebt uitgevoerd van de aanbeveling en opgelost de waarschuwing voorwaarde on-premises implementatie, moet u de melding van wissen door te schakelen van de melding op het tabblad **waarschuwingen** en klikken op **verwijderen**. Selecteer elke waarschuwing als u wilt wissen meerdere waarschuwingen, op een willekeurige kolom, behalve de **melding voor een** kolom en klik vervolgens op **wissen** nadat u alle meldingen wilt wissen hebt geselecteerd. Houd er rekening mee dat sommige waarschuwingen automatisch zijn uitgeschakeld wanneer het probleem opgelost is of wanneer het systeem de melding met nieuwe gegevens bijwerkt.

Wanneer u op **wissen**klikt, hebt u de mogelijkheid om opmerkingen over de waarschuwing en de stappen die u hebt gemaakt, het probleem op te lossen. 

![Waarschuwing opmerkingen](./media/storsimple-ova-manage-alerts/clear-alert.png)

Klik op het pictogram controleren ![selectievakje-pictogram](./media/storsimple-ova-manage-alerts/check-icon.png) opslaan in uw opmerkingen.

Sommige gebeurtenissen worden gewist door het systeem als een ander evenement met nieuwe informatie wordt geactiveerd. In dat geval ziet u het volgende bericht weergegeven.

![Schakel het selectievakje waarschuwingsbericht](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Waarschuwingen voor sorteren en controleren

Het tabblad **waarschuwingen** kunt maximaal 250 meldingen weergeven. Als u dat aantal waarschuwingen hebben overschreden, worden niet alle meldingen worden weergegeven in de standaardweergave. U kunt de volgende velden om aan te passen waarschuwingen worden weergegeven combineren:

- **Status** : U kunt **actief** of **uitgeschakeld** meldingen weergeven. Actieve waarschuwingen zijn nog steeds geactiveerd op uw systeem, terwijl uitgeschakeld waarschuwingen zijn handmatig uitgeschakeld door een beheerder of via een programma uitgeschakeld omdat het systeem de waarschuwing voorwaarde met nieuwe informatie bijgewerkt.

- **Ernst** : U kunt meldingen van alle niveaus (kritieke, waarschuwing, informatie) of alleen een bepaalde ernst, zoals alleen kritieke meldingen weergeven.

- **Bron** : U kunt meldingen van alle bronnen weergeven of de meldingen die afkomstig van de service of een of meer van de virtuele apparaten zijn te beperken.

- **Tijdsbereik** – door het opgeven van de **van** en **tot** datums en tijdstempels, kunt u zoeken in waarschuwingen tijdens de periode waarin u geïnteresseerd bent.

## <a name="alerts-quick-reference"></a>Naslaggids voor meldingen

De volgende tabellen bevatten aantal Microsoft Azure StorSimple waarschuwingen die optreden kunnen, alsmede aanvullende informatie en aanbevelingen indien beschikbaar. StorSimple virtueel apparaat waarschuwingen vallen in een van de volgende categorieën:

- [Cloud connectivity waarschuwingen](#cloud-connectivity-alerts)

- [Configuratie van meldingen](#configuration-alerts)

- [Foutmeldingen](#job-failure-alerts)

- [Prestaties van meldingen](#performance-alerts)

- [Beveiligingsmeldingen](#security-alerts)

- [Waarschuwingen bijwerken](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Cloud connectivity waarschuwingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Apparaat *<device name>* is niet verbonden met de cloud.|Het benoemde apparaat kan geen verbinding maken met de cloud. |Kan geen verbinding met de cloud. Dit kan worden veroorzaakt door een van de volgende opties:<ul><li>Het is mogelijk dat er een probleem met de netwerkinstellingen op uw apparaat.</li><li>Het is mogelijk dat er een probleem met de referenties voor de opslag-account.</li></ul>Ga naar de [lokale web UI](storsimple-ova-web-ui-admin.md) van het apparaat voor meer informatie over het oplossen van problemen met de serververbinding.|


### <a name="configuration-alerts"></a>Configuratie van meldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Configuratie van on-premises implementatie-virtueel apparaat niet worden ondersteund.|Vertragingen.|De huidige configuratie kan dit leiden tot vertragingen. Zorg ervoor dat uw server de van de minimale configuratievereisten voldoet. Ga naar [StorSimple virtuele matrix vereisten](storsimple-ova-system-requirements.md)voor meer informatie. 
|U worden afmelden bij ingerichte schijfruimte op <*apparaatnaam*> uitgevoerd.|Waarschuwing Schijfopruiming.|U weinig ingerichte schijfruimte. Overweeg om ruimte vrij, om te werkbelasting verplaatsen naar een ander volume of delen of u gegevens verwijdert.

### <a name="job-failure-alerts"></a>Foutmeldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Back-up van <*apparaatnaam*> kan niet worden voltooid.|Back-uptaak is mislukt.|Kan een back-up maken. Houd rekening met het volgende:<ul><li>Verbindingsproblemen kunnen worden voorkomen van de back-up is voltooid. Zorg ervoor dat er geen verbindingsproblemen. Ga naar de [lokale web UI](storsimple-ova-web-ui-admin.md) voor uw virtuele apparaat voor meer informatie over het oplossen van problemen met de serververbinding.</li><li>U kunt de beschikbare opslaglimiet hebt bereikt. Overweeg om ruimte vrij, te verwijderen van een back-ups die niet meer nodig zijn.</li></ul> De problemen oplossen, schakelt u de melding en probeer het opnieuw.|
|Herstellen van <*apparaatnaam*> kan niet worden voltooid.|Herstel de taak is mislukt.|Kan niet herstellen uit back-up. Houd rekening met het volgende:<ul><li>Uw back-lijst is mogelijk niet geldig. Hiermee vernieuwt u de lijst om te controleren of dat het nog geldig is.</li><li>Mogelijk verbindingsproblemen verhinderd door de bewerking voor terugzetten is voltooid. Zorg ervoor dat er geen verbindingsproblemen.</li><li>U kunt de beschikbare opslaglimiet hebt bereikt. Overweeg om ruimte vrij, te verwijderen van een back-ups die niet meer nodig zijn.</li></ul>De problemen oplossen, schakelt u de melding en probeer het opnieuw herstellen.|
|Klonen van <*apparaatnaam*> kan niet worden voltooid.|Klonen taak is mislukt.|Kan een klonen niet maken. Houd rekening met het volgende:<ul><li>Uw back-lijst is mogelijk niet geldig. Hiermee vernieuwt u de lijst om te controleren of dat het nog geldig is.</li><li>Mogelijk verbindingsproblemen verhinderd door de klonen bewerking is voltooid. Zorg ervoor dat er geen verbindingsproblemen.</li><li>U kunt de beschikbare opslaglimiet hebt bereikt. Overweeg om ruimte vrij, te verwijderen van een back-ups die niet meer nodig zijn.</li></ul>De problemen oplossen, schakelt u de melding en probeer het opnieuw.|

### <a name="performance-alerts"></a>Prestaties van meldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Onverwachte vertragingen in de overdracht van gegevens zich voordoet.|Traag gegevens overbrengen.|Bandbreedteregeling fouten optreden wanneer u de doelen schaalbaarheid van een service opslagruimte overschrijdt. De storage-service doet dit om ervoor te zorgen dat geen enkele client of tenant de service kosten van anderen kunt gebruiken. Ga naar [Monitor, automatisch opsporen, en problemen met Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)voor meer informatie over het oplossen van uw account Azure opslag.
|U weinig lokale reservering schijfruimte op <*apparaatnaam*>.|Traag antwoord tijd.|10% van de totale ingerichte grootte voor <*apparaatnaam*> is gereserveerd op het lokale apparaat en u nu weinig op de gereserveerde ruimte. De werklast op <*apparaatnaam*> genereert een hoger percentage van lospeuteren of u onlangs een grote hoeveelheid gegevens hebt gemigreerd. Dit kan resulteren in verminderde prestaties. Houd rekening met een van de volgende bewerkingen uit om op te lossen dit:<ul><li>Vergroot de bandbreedte cloud aan dit apparaat.</li><li>Verkleinen of werkbelasting verplaatsen naar een ander volume of delen.</li></ul>

### <a name="security-alerts"></a>Beveiligingsmeldingen

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Wachtwoord voor <*apparaatnaam*> verloopt in <*getal*> dagen.|Wachtwoord waarschuwing.| Uw wachtwoord verloopt < getal < dagen. Het is een goed idee om uw wachtwoord. Ga naar [het StorSimple virtuele matrix apparaat beheerderswachtwoord wijzigen](storsimple-ova-change-device-admin-password.md)voor meer informatie.

### <a name="update-alerts"></a>Waarschuwingen bijwerken

|Waarschuwing tekst|Gebeurtenis|Meer informatie / aanbevolen acties|
|:---|:---|:---|
|Nieuwe updates zijn beschikbaar voor uw apparaat.|Updates voor de virtuele matrix StorSimple zijn beschikbaar.|U kunt nieuwe updates installeren via **de onderhoudspagina voor** .|
|Kan niet worden gecontroleerd op nieuwe updates op <*apparaatnaam*>.|Werk is mislukt. |Er is een fout opgetreden tijdens de installatie van nieuwe updates. U kunt handmatig de updates installeren. Als het probleem zich blijft voordoen, neemt u contact op met [Microsoft ondersteuning](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de virtuele StorSimple-matrix](storsimple-ova-overview.md).


