<properties
   pageTitle="Uw StorSimple bandbreedte-distributiegroepen beheren | Microsoft Azure"
   description="Wordt beschreven hoe StorSimple bandbreedte sjablonen, waarmee u kunt bepalen van bandbreedtegebruik beheren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>Gebruik van de service StorSimple Manager StorSimple bandbreedte sjablonen beheren

## <a name="overview"></a>Overzicht

Bandbreedte sjablonen kunt u voor het configureren van bandbreedtegebruik netwerk over meerdere tijdgegevens-planningen trapsgewijs van de gegevens van het apparaat StorSimple naar de cloud.

Met bandbreedte planningen beperken, kunt u het volgende doen:

- Geef aangepaste bandbreedte planningen afhankelijk van het gebruik van de werklast-netwerk.

- Centraal beheer en de planningen op meerdere apparaten gebruiken in een eenvoudig en naadloos wijze.

> [AZURE.NOTE] Deze functie is alleen beschikbaar voor StorSimple fysieke apparaten en niet voor virtuele apparaten.

De bandbreedte-sjablonen voor uw service worden weergegeven in een tabelvormige indeling en de volgende gegevens bevatten:

- **Naam** : een unieke naam toegewezen aan de sjabloon bandbreedte wanneer deze is gemaakt.

- **Planning** : het aantal planningen opgenomen in een bepaald bandbreedte-sjabloon.

- **Wordt gebruikt voor** – het aantal volume met de bandbreedte-sjablonen.

U kunt de pagina StorSimple Manager service **configureren** in de portal van Azure klassieke bandbreedte distributiegroepen beheren.

U vindt ook aanvullende informatie om te helpen met het configureren van bandbreedte sjablonen in:

- Vragen en antwoorden over bandbreedte-sjablonen
- Aanbevolen procedures voor het bandbreedte sjablonen

## <a name="add-a-bandwidth-template"></a>Een sjabloon bandbreedte toevoegen

De volgende stappen als u wilt maken van een nieuwe sjabloon voor de bandbreedte uitvoeren.

#### <a name="to-add-a-bandwidth-template"></a>Een sjabloon bandbreedte toevoegen

1. Klik op **toevoegen/bewerken bandbreedte sjabloon**op de pagina StorSimple Manager service **configureren** .

2. Klik in het dialoogvenster **Add/Edit bandbreedte sjabloon** :

   1. Selecteer **Nieuw** een nieuwe sjabloon voor de bandbreedte toevoegen uit de vervolgkeuzelijst **sjabloon** .
   2. Geef een unieke naam voor de bandbreedte-sjabloon.

3. Een **planning van de bandbreedte**definiëren. Een planning maken:

   1. Kies de dagen van de week die de planning is geconfigureerd voor de vervolgkeuzelijst. U kunt meerdere dagen selecteren door de selectievakjes vóór de desbetreffende dagen in de lijst te schakelen.
   2. Selecteer de optie **Hele dag** als de planning wordt afgedwongen voor de hele dag. Wanneer deze optie is ingeschakeld, kunt u niet langer een **Begintijd** of **Eindtijd**van een opgeven. De planning wordt uitgevoerd van 12:00 AM tot 23:59 uur.
   3. Selecteer een **Begintijd**in de vervolgkeuzelijst. Dit is wanneer u de planning wordt gestart.
   4. Selecteer in de vervolgkeuzelijst een **Eindtijd**. Dit is wanneer u de planning wordt gestopt.

         > [AZURE.NOTE] Overlappende schema's zijn niet toegestaan. Als de begin- en eindtijden in een overlappende planning resulteren zal, ziet u een foutbericht toe.

   5. Geef de **bandbreedte rente**. Dit is de bandbreedte in Megabits per seconde () die wordt gebruikt door uw apparaat StorSimple in bewerkingen die betrekking hebben op personen de cloud (uploads en downloads). Een getal tussen 1 en 1.000 voor dit veld opgeven.

   6. Klik op het pictogram van het selectievakje ![pictogram](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). De sjabloon die u hebt gemaakt worden, toegevoegd aan de lijst met sjablonen van de bandbreedte op de servicepagina **configureren** .

    ![Nieuwe bandbreedte-sjabloon maken](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Klik op **Opslaan** onder aan de pagina en klik vervolgens op **Ja** wanneer u wordt gevraagd om bevestiging. Wijzigingen die u hebt aangebracht in de configuratie wordt opgeslagen.

## <a name="edit-a-bandwidth-template"></a>Sjabloon voor een bandbreedte bewerken

De volgende stappen als u wilt bewerken van een sjabloon bandbreedte uitvoeren.

### <a name="to-edit-a-bandwidth-template"></a>Een sjabloon bandbreedte bewerken

1. Klik op **toevoegen/bewerken bandbreedte sjabloon**.

2. Klik in het dialoogvenster **Add/Edit bandbreedte sjabloon** :

   1. Kies in de vervolgkeuzelijst **sjabloon** een bestaande bandbreedte sjabloon die u wilt wijzigen.
   2. Breng de wijzigingen. (U kunt wijzigen van de bestaande instellingen.)
   3. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). U ziet de gewijzigde sjabloon in de lijst met bandbreedte sjablonen op de pagina service configureren.

3. Klik op **Opslaan** onder aan de pagina uw wijzigingen op te slaan. Klik op **Ja** wanneer u wordt gevraagd om bevestiging.

> [AZURE.NOTE] U wordt niet toegestaan uw wijzigingen opslaan als de bewerkte planning overlapt met een bestaande planning in de bandbreedte-sjabloon die u wilt wijzigen.

## <a name="delete-a-bandwidth-template"></a>Een sjabloon bandbreedte verwijderen

De volgende stappen als u wilt verwijderen van een sjabloon bandbreedte uitvoeren.

#### <a name="to-delete-a-bandwidth-template"></a>Een sjabloon bandbreedte verwijderen

1. Selecteer in de lijst in tabelvorm van de bandbreedte-sjablonen voor uw service, de sjabloon die u wilt verwijderen. Een verwijderpictogram (**x**) wordt weergegeven aan extreme rechts van de geselecteerde sjabloon. Klik op het pictogram **x** als u wilt verwijderen van de sjabloon.

2. U wordt gevraagd om bevestiging. Klik op **OK** om door te gaan.

Als de sjabloon wordt gebruikt door een volume (s), wordt u niet toegestaan om deze te verwijderen. Er verschijnt een foutbericht met de mededeling dat de sjabloon gebruikt wordt. Dialoogvenster met een foutbericht wordt weergegeven waarin wordt gemeld dat de verwijzingen aan de sjabloon moeten worden verwijderd.

U kunt de verwijzingen aan de sjabloon verwijderen door de pagina **Volume Containers** openen en wijzigen van de volume-containers die met deze sjabloon, zodat ze gebruik een andere sjabloon of een aangepaste of onbeperkte bandbreedte-instelling. Wanneer alle verwijzingen zijn verwijderd, kunt u de sjabloon verwijderen.

## <a name="use-a-default-bandwidth-template"></a>Een standaardsjabloon bandbreedte gebruiken

Een standaardsjabloon bandbreedte wordt geleverd en wordt gebruikt door volume containers al dan niet standaard om af te dwingen bandbreedte besturingselementen bij het openen van de cloud. De standaardsjabloon ook bepalend is klaar voor gebruikers die hun eigen sjablonen maken. De details van deze standaardsjabloon zijn:

- **Naam** : onbeperkt 's nachts en tijdens het weekend

- **Planning** : een één planning van maandag tot vrijdag die van toepassing een tarief bandbreedte van 1 Mbps tussen 8 AM en 5 PM apparaat tijd is. De bandbreedte is ingesteld op onbeperkt voor de rest van de week.

De standaardsjabloon kan worden bewerkt. Het gebruik van deze sjabloon (inclusief bewerkte versies) wordt bijgehouden.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Een hele dag duurt bandbreedte sjabloon vanaf een bepaalde tijd maken

Volg deze procedure om een schema dat begint op een bepaalde tijd en de hele dag is uitgevoerd te maken. In het voorbeeld wordt de planning begint op 9 AM in de ochtend en wordt uitgevoerd tot en met 9 AM de volgende ochtend. Het is belangrijk te weten dat de begin- en eindtijden voor een bepaald schema moet worden beide zijn in de dezelfde 24-uurs-planning en kan geen gegevens voor meerdere dagen. Als u nodig hebt voor het instellen van bandbreedte sjablonen meerdere dagen, moet u meerdere planningen gebruiken (zoals in het voorbeeld).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Naar een hele dag duurt bandbreedte-sjabloon maken

1. Maak een planning dat begint op 9 AM in de ochtend en wordt uitgevoerd tot middernacht.

2. Een ander schema toevoegen. De tweede planning om uit te voeren van middernacht tot en met 9 AM in de ochtend configureren.

3. Sla de sjabloon bandbreedte.

De samengestelde planning vervolgens gestart tegelijk van uw keuze en driedaagse uitvoeren.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Vragen en antwoorden over bandbreedte-sjablonen

**Q**. Wat gebeurt er met de bandbreedte besturingselementen wanneer u tussen de planning? (Een planning is beëindigd en een ander postvakbeleid nog niet is gestart.)

**A**. In dat geval wordt geen bandbreedte-besturingselementen worden gebruikt. Dit betekent dat het apparaat onbeperkte bandbreedte gebruiken kunt wanneer trapsgewijs schakelen naar de cloud.

**Q**. Kunt u bandbreedte sjablonen op een offline-apparaat wijzigen?

**A**. Niet is mogelijk bandbreedte sjablonen op volumes containers gewijzigd als het bijbehorende apparaat offline.

**Q**. Kunt u de sjabloon voor een bandbreedte dat is gekoppeld aan een container volume wanneer de bijbehorende hoeveelheden offline zijn bewerken?

**A**. U kunt een bandbreedte sjabloon die is gekoppeld aan een volume-container waarvan volumes offline zijn wijzigen. Houd er rekening mee dat wanneer u offline-volumes zijn geen gegevens wordt worden doorverbonden van het apparaat in de cloud.

**Q**. Kunt u een standaardsjabloon verwijderen?

**A**. Hoewel u een standaardsjabloon verwijderen kunt, is het niet een goed idee kunt doen. Het gebruik van een standaardsjabloon, bewerkte versies, inclusief wordt bijgehouden. De gegevens bijhouden wordt geanalyseerd en in de loop van de tijd, wordt gebruikt voor het verbeteren van de standaardsjabloon.

**Q**. Hoe bepaal u dat uw sjablonen bandbreedte moeten worden gewijzigd?

**A**. Een van de tekens die u wilt de bandbreedte-sjablonen wijzigen is tijdens het starten van het netwerk traag of meerdere keren in een dag smoorspoel zien. Als dit gebeurt, controleert u het netwerk opslag en het gebruik door te kijken de o prestaties en netwerkdoorvoer grafieken.

Welke uit de netwerkgegevens doorvoer, het tijdstip en het volume containers waarin het netwerk knelpunt plaatsvindt. Als dit gebeurt wanneer u gegevens in de cloud is worden doorverbonden (deze informatie ophalen uit o prestaties voor alle volume containers voor apparaat naar cloud), en vervolgens moet u de bandbreedte sjablonen die zijn gekoppeld aan uw containers volume aanpassen.

Nadat de gewijzigde sjablonen gebruikt worden, moet u het netwerkverkeer opnieuw voor aanzienlijk vertragingstijden. Als deze nog steeds bestaan, moet u terugkeren naar uw bandbreedte-sjablonen.

**Q**. Wat gebeurt er als meerdere volume containers op mijn apparaat plant die elkaar overlappen, maar andere limieten gelden voor elk?

**A**. Stel dat u dat er een apparaat met 3 volume containers. De planningen die is gekoppeld aan deze containers volledig overlappen. Voor elk van deze containers zijn de bandbreedte gebruikt 5, 10 en 15 Mbps respectievelijk. Wanneer o op alle van deze containers op hetzelfde moment optreedt zijn, het minimum van de 3 bandbreedte limieten kan worden toegepast: in dit geval 5 Mbps als deze i/o-aanvragen voor uitgaande dezelfde wachtrij delen.

## <a name="best-practices-for-bandwidth-templates"></a>Aanbevolen procedures voor het bandbreedte sjablonen

Volg deze aanbevolen procedures voor uw apparaat StorSimple:

- Bandbreedte sjablonen configureren op uw apparaat om in te schakelen variabele beperken van de netwerkdoorvoer door het apparaat op verschillende momenten van de dag. Deze sjablonen voor de bandbreedte gebruikt in combinatie met back-planningen kunnen effectief gebruikmaken van extra netwerkbandbreedte voor cloud bewerkingen na kantooruren.

- Bereken het werkelijke bandbreedte vereist is voor een bepaalde implementatie op basis van de grootte van de implementatie en de vereiste herstel tijd doelstelling (RTO).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
