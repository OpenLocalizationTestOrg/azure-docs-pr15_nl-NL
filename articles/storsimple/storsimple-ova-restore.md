<properties
   pageTitle="Terugzetten uit een back-up van uw virtuele StorSimple-matrix"
   description="Meer informatie over het herstellen van een back-up van uw StorSimple virtuele matrix."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Terugzetten uit een back-up van uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht 

In dit artikel is van toepassing op Microsoft Azure StorSimple virtuele matrix (ook wel bekend als de StorSimple on-premises implementatie virtueel apparaat of een virtueel apparaat StorSimple) actieve maart 2016 beschikbaarheid (GA) van de algemene release of hoger. In dit artikel worden stapsgewijze het herstellen van een back-ups instellen van de waarden voor aandelen of volumes op uw StorSimple virtuele matrix. Het artikel ook een gedetailleerd overzicht van de werking van het herstelproces is itemniveau op uw StorSimple virtuele matrix die is geconfigureerd als een bestandsserver.


## <a name="restore-shares-from-a-backup-set"></a>Waarden voor aandelen terugzetten uit een back-ups instellen


**Voordat u probeert te herstellen waarden voor aandelen, zorg ervoor dat er voldoende ruimte op het apparaat om deze bewerking te voltooien.** Als u wilt terugzetten uit een back-up, in de [klassieke Azure-portal](https://manage.windowsazure.com/), kunt u de volgende stappen uitvoeren.

#### <a name="to-restore-a-share"></a>U herstelt een delen

1.  Blader naar de **back-up-catalogus**. Filteren op het gewenste apparaat en tijdsbereik naar uw back-ups zoeken. Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-restore/image1.png) de query uit te voeren.


1.  Klik in de lijst met back-sets weergegeven, en selecteert u een specifieke back-up. Vouw de back-up om te zien van de verschillende waarden voor aandelen eronder. Klik op en selecteer een delen die u wilt herstellen.

2.  Onderaan op de pagina, klikt u op **herstellen als nieuwe**.

3.  Hiermee wordt de wizard **terugzetten als nieuwe delen** starten. Klik op de pagina **opgeven naam en locatie** :


    1.  Controleer of de naam van het bron-apparaat. Dit moet het apparaat met de optie voor delen die u wilt herstellen. De Apparaatselectie grijs wordt weergegeven. Selecteer een andere bron-apparaat, moet u de wizard afsluiten en opnieuw de back-up opnieuw instellen.

    2.  Geef een naam op delen. De naam van de delen moet 3 tot en met 127 tekens bevatten.

    3.  Bekijk de grootte, het type en de machtigingen die zijn gekoppeld aan de optie voor delen dat u wilt herstellen. Is mogelijk om te wijzigen van de eigenschappen van het delen via Windows Verkenner nadat het herstellen voltooid is.

    4.  Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Wanneer de terugzettaak voltooid is, wordt de terugzetten wordt gestart en ziet u nog een melding. Om de voortgang van terugzetten, klikt u op **taak weergeven**. Hiermee gaat u naar de pagina **taken** .

2.  U kunt de voortgang van de terugzettaak bijhouden. Als het herstellen 100% voltooid is, Ga terug naar de pagina **waarden voor aandelen** op uw apparaat.

3.  U kunt nu de nieuwe herstelde delen in de lijst met waarden voor aandelen weergeven op uw apparaat. Houd er rekening mee dat herstellen is voltooid aan hetzelfde type als de optie voor delen. Een doorverbonden delen is hersteld, want doorverbonden en een lokaal vastgemaakte delen als een lokaal vastgemaakte delen.

U hebt nu de apparaatconfiguratie voltooid en hebt geleerd hoe u back-up maken of herstelt een delen. 


## <a name="restore-volumes-from-a-backup-set"></a>Volumes terugzetten uit een back-ups instellen


Als u wilt terugzetten uit een back-up, in de klassieke Azure portal, kunt u de volgende stappen uitvoeren. De bewerking voor terugzetten herstelt u de back-up naar een nieuwe volume op hetzelfde virtuele apparaat; u kunt geen terugzetten naar een ander apparaat.

#### <a name="to-restore-a-volume"></a>Een volume herstellen

1.  Blader naar de **back-up-catalogus**. Filteren op het gewenste apparaat en tijdsbereik naar uw back-ups zoeken. Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-restore/image1.png) de query uit te voeren.

2.  Klik in de lijst met back-sets weergegeven, en selecteert u een specifieke back-up. Vouw de back-up om te zien van de verschillende hoeveelheden eronder. Selecteer het volume die u wilt herstellen. 

5.  Onderaan op de pagina, klikt u op **herstellen als nieuwe**. De wizard **terugzetten als nieuwe volume** wordt gestart.

1.  Klik op de pagina **opgeven naam en locatie** :


    1.  Controleer of de naam van het bron-apparaat. Dit moet het apparaat met het volume die u wilt herstellen. De Apparaatselectie is niet beschikbaar. Selecteer een andere bron-apparaat, moet u de wizard afsluiten en opnieuw de back-up opnieuw instellen.

    2.  Geef een volumenaam voor het volume als nieuwe wordt hersteld. Naam van het volume moet 3 tot en met 127 tekens bevatten.

    3.  Klik op het pijlpictogram.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Selecteer de juiste ACRs in de vervolgkeuzelijst op de pagina **opgeven hosts die dit volume kunnen gebruiken** .

    ![](./media/storsimple-ova-restore/image13.png)

1.  Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-restore/image1.png). Hiermee wordt een terugzettaak starten en ziet u de volgende melding dat de taak uitgevoerd wordt.

2.  Wanneer de terugzettaak voltooid is, wordt de terugzetten wordt gestart en ziet u nog een melding. Om de voortgang van terugzetten, klikt u op **taak weergeven**. Hiermee gaat u naar de pagina **taken** .

3.  U kunt de voortgang van de terugzettaak bijhouden. Ga terug naar de pagina **Volumes** op uw apparaat.

4.  U kunt het volume van het nieuwe herstelde nu in de lijst van hoeveelheden weergeven op uw apparaat. Houd er rekening mee herstellen bij het volume van hetzelfde type wordt uitgevoerd. Het volume van een gelaagde want doorverbonden is hersteld en een lokaal vastgemaakte volume als een lokaal vastgemaakte volume is hersteld.

5.  Zodra het volume online in de lijst van hoeveelheden verschijnt, is het volume is beschikbaar voor gebruik.  Op de iSCSI initiator-host, vernieuwt u de lijst met doelen in het eigenschappenvenster van iSCSI initiator.  Een nieuw doel waarin de volumenaam van de herstelde moet worden weergegeven als 'inactief' onder de statuskolom.

6.  Selecteer het doel en klik op **verbinding maken**.   Nadat het beginpunt is verbonden met het doel, moet de status om **verbonden**te wijzigen. 

7.  Klik in het venster **Schijfopruiming Management** verschijnt de gekoppelde hoeveelheden zoals wordt weergegeven in de volgende afbeelding. Met de rechtermuisknop op het ontdekt volume (Klik op de schijfnaam van de), en klik vervolgens op **Online**.

> [AZURE.IMPORTANT] Als u een volume of een delen herstellen uit een back-up wilt instellen, als de terugzettaak mislukt, een doeltoepassing volume of delen nog steeds worden gemaakt in de portal. Het is belangrijk dat u dit volume doellijst verwijderen of in de portal delen te minimaliseren ten eventuele toekomstige problemen die voortvloeien uit met dit element.

## <a name="item-level-recovery-ilr"></a>Itemniveau herstel (ILR)

Deze release maakt u kennis met het herstelproces is itemniveau (ILR) op een StorSimple virtueel apparaat geconfigureerd als een bestandsserver. De functie kunt u gedetailleerde herstel van bestanden en mappen van een wolk back-up van alle aandelen op het apparaat StorSimple doen. Gebruikers kunnen verwijderde bestanden ophalen uit een recente back-ups met een model zelf.

Elke delen heeft een *.backups* -map met de meest recente back-ups. De gebruiker kan Navigeer naar de gewenste back-up, kopieer relevante bestanden en mappen uit de back-up en herstellen. Hierdoor oproepen voor beheerders voor het terugzetten van bestanden van back-ups.

1.  Als u de ILR uitvoert, kunt u de back-ups tot en met Windows Verkenner weergeven. Klik op het specifieke delen die u wilt kijkt u naar de back-up voor. Hier ziet u een *.backups* -map onder de optie voor delen waarin de back-ups hebt gemaakt. Vouw de map *.backups* om weer te geven van de back-ups. De map wordt vervolgens de geëxplodeerde weergave van de gehele back-hiërarchie weergegeven. Deze weergave is gemaakt op aanvraag en gewoonlijk duurt slechts enkele seconden om te maken.

    De laatste 5 back-ups op deze manier worden weergegeven en kunnen worden gebruikt om een itemniveau herstellen. De 5 recente back-ups zijn zowel de standaardinstelling is gepland en de handmatige back-ups.

    
    -   **Geplande back-ups** met de naam als &lt;apparaatnaam&gt;DailySchedule-JJJJMMDD-UUMMSS-UTC.

    -   **Handmatige back-ups** Ad-hoc-JJJJMMDD-UUMMSS-UTC genoemd.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Geef aan dat de back-up met de meest recente versie van het bestand. Hoewel de mapnaam een UTC-tijdstempel in elk van de bovenstaande gevallen bevat, is de tijd waarop de map is gemaakt in de werkelijke apparaat tijd waarop de back-up is gestart. Gebruik de tijdstempel van de map te zoeken en de back-ups identificeren.

2.  Zoek de map of het bestand dat u wilt kunnen terugzetten in de back-up die u in de vorige stap hebt geïdentificeerd. Opmerking dat u kunt de bestanden of mappen die u machtigingen hebt voor alleen weergeven. Als u geen toegang tot bepaalde bestanden of mappen, moet u contact op met de beheerder van een delen die met Windows Verkenner voor het bewerken van de machtigingen voor delen en u toegang geven tot de specifieke bestand of map. Het wordt aanbevolen aanbevolen dat de beheerder van het delen een groep in plaats van één gebruiker zijn.

3.  Kopieer het bestand of de map naar de gewenste optie voor delen op uw bestandsserver StorSimple.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Video beschikbaar**

Bekijk de video om te zien hoe u kunt waarden voor aandelen maken, een back-up waarden voor aandelen en herstellen van gegevens op een virtuele StorSimple-matrix.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple virtuele matrix via het lokale web UI](storsimple-ova-web-ui-admin.md).
