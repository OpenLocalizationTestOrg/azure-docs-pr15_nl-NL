<properties 
   pageTitle="Wat is StorSimple momentopname Manager? | Microsoft Azure"
   description="De StorSimple momentopname Manager, de architectuur en de functies beschreven."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="what-is-storsimple-snapshot-manager"></a>Wat is StorSimple momentopname Manager?

## <a name="overview"></a>Overzicht

StorSimple momentopname Manager is een Microsoft Management Console (MMC)-module waardoor het eenvoudiger gegevensbescherming en back-beheer in een Microsoft Azure StorSimple-omgeving wordt. Met StorSimple momentopname Manager, kunt u beheren StorSimple van Microsoft Azure-gegevens in het datacenter en in de cloud als één geïntegreerde opslagoplossing, dus back-processen te vereenvoudigen en kosten.

Dit overzicht maakt u kennis met de StorSimple momentopname Manager, worden de functies beschreven en wordt uitgelegd van de rol in Microsoft Azure StorSimple. 

Zie voor een overzicht van de gehele Microsoft Azure StorSimple-systeem, inclusief het apparaat StorSimple, StorSimple Manager-service, StorSimple momentopname Manager en StorSimple Adapter voor SharePoint, [StorSimple 8000-serie: een hybride cloud oplossing](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- U kunt StorSimple momentopname Manager niet gebruiken voor het beheren van Microsoft Azure StorSimple virtuele matrices (ook wel bekend als StorSimple on-premises implementatie virtuele apparaten).
>
>- Als u van plan bent om te StorSimple Update 2 installeren op uw apparaat StorSimple, zorg ervoor dat u naar de meest recente versie van StorSimple momentopname Manager downloaden en installeren **voordat u StorSimple Update 2 installeert**. De meest recente versie van StorSimple momentopname Manager is compatibel en werkt met alle versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple momentopname Manager gebruikt, moet u deze (u hoeft niet naar de vorige versie verwijderen voordat u de nieuwe versie installeert) bij te werken.

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple momentopname Manager doel hebben en architectuur

StorSimple momentopname Manager biedt een centrale beheerconsole die u gebruiken kunt om te maken consistente, point-in-time back-ups van lokale en cloud-gegevens. U kunt bijvoorbeeld de console gebruiken:

- Configureren, een back-up en volumes verwijderen.
- Configureer volume groepen om ervoor te zorgen dat de back-ups van gegevens van toepassing-consistent is.
- Back-beleid beheren, zodat de back-up op een vooraf ingestelde planning.
- Lokale maken en cloud momentopnamen, die kunnen worden opgeslagen in de cloud en die wordt gebruikt voor problemen oplossen.

De Manager van de momentopname StorSimple haalt de lijst met toepassingen die zijn geregistreerd bij de provider VSS op de host. Vervolgens als wilt maken van back-ups van toepassing-consistente, controleert de hoeveelheden door een toepassing gebruikt en stappen worden voorgesteld volume groepen om over te configureren. StorSimple momentopname Manager gebruikt deze groepen volume back-ups die toepassing consistent zijn gegenereerd. (Toepassing consistentie bestaat wanneer alle bestanden gerelateerde en databases worden gesynchroniseerd en de werkelijke status van de toepassing op een specifiek punt in de tijd vertegenwoordigen.) 

Back-ups van StorSimple momentopname Manager duren voordat de vorm van incrementele momentopnamen, die alleen de wijzigingen sinds de laatste back-up vastleggen. Hierdoor kunnen back-ups minder opslagruimte nodig en worden gemaakt en snel hersteld. De Windows Volume schaduw Copy Service (VSS) StorSimple momentopname Manager gebruikt om ervoor te zorgen dat momentopnamen toepassing consistente gegevens vastleggen. (Voor meer informatie, Ga naar de integratie met Windows Volume schaduw Copy-Service sectie.) Met StorSimple momentopname Manager, u kunt maken van back-planningen of direct back-ups naar wens. Als u gegevens wilt terugzetten uit een back-up en StorSimple momentopname Manager kunt selecteren u uit een catalogus van lokale of cloud momentopnamen. Azure StorSimple herstelt u alleen de gegevens die nodig is dit nodig is, waardoor vertragingen in de gegevensbeschikbaarheid tijdens herstellen bewerkingen.)

![StorSimple momentopname Manager architectuur](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple momentopname Manager architectuur** 

## <a name="support-for-multiple-volume-types"></a>Ondersteuning voor meerdere volumetypen

U kunt de StorSimple momentopname Manager gebruiken om te configureren en back-up van de volgende typen van hoeveelheden: 

- **Standaardvolumes** – een standaardvolume is een enkel partition op een eenvoudige schijf. 

- **Eenvoudige volumes** – een eenvoudig volume is een dynamisch volume met schijfruimte uit één dynamische schijf. Een eenvoudig volume bestaat uit één regio op een schijf of meerdere regio's die zijn gekoppeld op dezelfde schijf. (U kunt eenvoudige volumes maken alleen op dynamische schijven.) Eenvoudige volumes zijn niet fouttolerantie.

- **Dynamische volumes** – een dynamisch volume is een volume die is gemaakt op een dynamische schijf. Een database dynamische schijven gebruiken voor het bijhouden van informatie over volumes die zijn opgenomen op dynamische schijven op een computer. 

- **Dynamische volumes met spiegelen** – dynamische volumes met spiegelen zijn gebaseerd op de architectuur RAID 1. Met RAID 1, identieke gegevens geschreven op twee of meer schijf, een gespiegeld set produceren. Een leesbevestiging kan vervolgens worden verwerkt door een schijf waarop de gevraagde gegevens.

- **Cluster gedeelde clustervolumes** – met cluster gedeeld delen (CSVs), meerdere knooppunten in een failovercluster kunnen tegelijk een lezen of schrijven naar dezelfde schijf. Failover uit één knooppunt naar een ander knooppunt kunt snel, zonder een wijziging in station eigendom of koppelen, ontkoppelen en als u een volume plaatsvinden. 

>[AZURE.IMPORTANT] Geen verschillende CSVs en CSVs in de dezelfde momentopname. Mengen CSVs en CSVs in een momentopname wordt niet ondersteund. 
 
U kunt StorSimple momentopname Manager herstellen hele volume groepen of afzonderlijke volumes klonen en afzonderlijke bestanden herstellen.

- [Hoeveelheden en volume-groepen](#volumes-and-volume-groups) 
- [Back-typen en back-beleid](#backup-types-and-backup-policies) 

Zie voor meer informatie over de functies van StorSimple momentopname Manager en het gebruik van deze [StorSimple momentopname Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Hoeveelheden en volume-groepen

Met StorSimple momentopname Manager, die u volumes maken en klik vervolgens in het volume van groepen configureren. 

Volume-groepen StorSimple momentopname Manager gebruikt om te maken van back-ups die toepassing consistent zijn. Toepassing consistentie bestaat wanneer alle bestanden gerelateerde en databases worden gesynchroniseerd en geven van de werkelijke status van een toepassing op een specifiek punt in de tijd. Volume-groepen (die ook wel bekend als *consistentie groepen*) vormen de basis van een back-up of terugzetten.

Volume-groepen zijn hetzelfde als het volume van containers. Een container volume bevat een of meer volumes die een cloud-opslag-account en andere kenmerken, zoals versleuteling en bandbreedte verbruik delen. Een container één volume kan maximaal 256 dun ingerichte StorSimple volumes bevatten. Ga naar [uw containers volume beheren](storsimple-manage-volume-containers.md)voor meer informatie over het volume van containers. Volume-groepen zijn verzamelingen van hoeveelheden die u kunt configureren om te vereenvoudigen back-up. Als u twee volumes die deel uitmaakt van verschillende volume containers, op een groep één volume plaatsen en maak een back-beleid voor die groep volume selecteert, wordt elk volume reservekopie in de juiste volume-container, met de juiste opslag-account.

>[AZURE.NOTE] Enkel volume van een groep volume moeten afkomstig zijn uit een serviceprovider één cloud.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integratie met Windows Volume schaduw Copy-Service

De Windows Volume schaduw Copy Service (VSS) StorSimple momentopname Manager gebruikt om vast te leggen van toepassing-consistente gegevens. VSS zorgt voor een toepassing consistente door te communiceren met VSS-compatibele toepassingen kunnen het maken van incrementele momentopnamen coördineren. VSS zorgt ervoor dat de toepassingen tijdelijk inactief of quiescent, zijn wanneer momentopnamen worden. 

De uitvoering StorSimple momentopname Manager van VSS werkt met SQL Server en algemene NTFS-volumes. Het proces is als volgt: 

1. Een persoon, dat wil meestal een gegevensbeheer en beveiliging oplossing (zoals StorSimple momentopname Manager) of een back-toepassing zeggen, roept VSS en wordt gevraagd om deze naar Verzamel informatie over de software schrijver in de doeltoepassing instellen.

2. VSS contact op met de schrijver-component om op te halen een beschrijving van de gegevens. De schrijver retourneert de beschrijving van de gegevens die moeten worden back-up is gemaakt. 

3. VSS geeft de schrijver voor het voorbereiden van de toepassing voor back-up. De schrijver bereidt de gegevens voor back-up maken door te vullen openstaande transacties bijwerken transactielogboek, enzovoort, en wordt een bericht verzonden VSS.

4. VSS Hiermee geeft u de schrijver tijdelijk opgeslagen van de gegevens van de toepassing stoppen en zorg ervoor dat er geen gegevens naar het volume is geschreven terwijl de schaduwkopie wordt gemaakt. Deze stap zorgt ervoor dat de consistentie van de gegevens, en wordt niet meer dan 60 seconden.

5. VSS Hiermee geeft u de provider om de schaduwkopie te maken. Providers die software of hardware gebaseerde worden kunnen, het beheren van de hoeveelheden dat moment actief zijn en schaduwkopieën van te maken op aanvraag. De provider Hiermee maakt u de schaduwkopie en VSS krijgt wanneer deze is voltooid.

6. VSS contact op met de schrijver naar de toepassing die I/O kunt hervatten hoogte en om te bevestigen dat I/O is onderbroken tijdens het maken van de schaduw kopiëren. 

7. Als de kopie geslaagd is, retourneert VSS locatie van de kopie naar degene. 

8. Als gegevens is geschreven terwijl de schaduwkopie is gemaakt, klikt u vervolgens zijn de back-up inconsistent. VSS Hiermee verwijdert u de schaduwkopie en degene krijgt. Degene kan worden automatisch herhaalt u de back-procedure of de beheerder om te proberen deze op een later tijdstip een melding.

Zie de volgende afbeelding.

![VSS proces](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows Volume schaduw Copy-Service proces** 

## <a name="backup-types-and-backup-policies"></a>Back-typen en back-beleid

Met StorSimple momentopname Manager, kunt u een back-up van gegevens en sla deze lokaal en in de cloud. U kunt StorSimple momentopname Manager direct een back-up van gegevens of u kunt een back-beleid maken van een schema voor het maken van back-ups automatisch. Beleid voor het back-up kunnen u opgeven hoeveel momentopnamen bewaard. 

### <a name="backup-types"></a>Back-typen

U kunt StorSimple momentopname Manager gebruiken om de volgende typen back-ups te maken:

- **Lokale momentopnamen** – lokale momentopnamen point-in-time kopieën van volumegegevens die zijn opgeslagen op het apparaat StorSimple zijn. Meestal worden dit type back-up gemaakt en snel hersteld. U kunt een lokale momentopname kunt gebruiken, zoals u zou doen met een lokale back-up.

- **Cloud momentopnamen** – Cloud momentopnamen zijn point-in-time kopieën van volumegegevens die zijn opgeslagen in de cloud. Een momentopname cloud is gelijk aan een momentopname gerepliceerd op een andere, externe opslagsysteem. Cloud momentopnamen zijn met name handig voor noodgevallen herstel scenario's.

### <a name="on-demand-and-scheduled-backups"></a>Op aanvraag en geplande back-ups

Met StorSimple momentopname Manager, u kunt een eenmalige back-up wilt maken direct starten of u kunt een back-beleid gebruiken om terugkerende back-bewerkingen te plannen.

Een back-beleid wordt een set geautomatiseerde regels die u gebruiken kunt om te regelmatige back-ups plannen. Een back-beleid kunt u de frequentie en parameters voor het maken van momentopnamen van een specifieke volume-groep definiëren. U kunt beleid gebruiken om op te geven en vervaldatum datums, tijden, frequentie en bewaarbeleid vereisten, van lokale en cloud momentopnamen. Een beleid wordt toegepast onmiddellijk nadat u deze hebt gedefinieerd. 

U kunt StorSimple momentopname Manager configureren of back-beleidsregels voor elk gewenst moment opnieuw te configureren. 

U configureren de volgende informatie voor elk back-beleid die u maakt:

- **Naam** : de unieke naam van het geselecteerde back-beleid.

- **Type** , het type van het back-beleid; lokale momentopname of cloud momentopname.

- **Volume-groep** : de volume-groep waaraan u het geselecteerde back-beleid is toegewezen.

- **Bewaarbeleid** – het aantal back-ups moeten worden bewaard. Als u het selectievakje **alle** inschakelt, worden alle back-ups worden bewaard totdat het maximum aantal back-ups per volume is bereikt, waarna het beleid wordt mislukt en er een foutbericht weergegeven. U kunt ook opgeven dat een aantal back-ups moeten worden bewaard (tussen 1 en 64).

- **Datum** , de datum waarop het back-beleid is gemaakt.

Ga naar [Gebruik StorSimple momentopname Manager maken en beheren van back-beleidsregels](storsimple-snapshot-manager-manage-backup-policies.md)voor informatie over het configureren van beleidsregels voor back-up.

### <a name="backup-job-monitoring-and-management"></a>Back-uptaak voor controle en beheer

U kunt de StorSimple momentopname Manager bewaken en beheer komende, geplande en voltooide taken voor de back-up. Daarnaast biedt StorSimple momentopname Manager een catalogus van maximaal 64 voltooide back-ups. U kunt de catalogus gebruiken om te zoeken en herstellen van volumes of afzonderlijke bestanden. 

Ga naar [Gebruik StorSimple momentopname Manager weergeven en beheren van back-taken](storsimple-snapshot-manager-manage-backup-jobs.md)voor informatie over het controleren van back-taken.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).

- Download [StorSimple momentopname Manager](https://www.microsoft.com/download/details.aspx?id=44220).
