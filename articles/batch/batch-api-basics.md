<properties
    pageTitle="Azure Batch Functieoverzicht voor ontwikkelaars | Microsoft Azure"
    description="Informatie over de functies van de Batch-service en de API's uit oogpunt ontwikkeling."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Overzicht van de functie batch voor ontwikkelaars

In dit overzicht van de belangrijkste onderdelen van de Batch Azure-service, wordt de primaire servicefuncties besproken en resources die Batch ontwikkelaars gebruiken kunnen om te maken van grootschalige parallel berekenen oplossingen.

Of u een gedistribueerde rekenkundige toepassing ontwikkelt of service die problemen directe [REST API] [ batch_rest_api] oproepen of u gebruikt een van de [Batch SDK's](batch-technical-overview.md#batch-development-apis), veel van de resources en de functies die in dit artikel beschreven die u wilt gebruiken.

> [AZURE.TIP] Voor een hoger niveau Inleiding tot de Batch-service, raadpleegt u de [Basisbeginselen van Azure Batch](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Batch servicewerkstroom

De volgende hoog niveau werkstroom wordt gebruikt voor bijna alle toepassingen en services die gebruikmaken van de Batch-service voor het verwerken van parallelle werkbelasting:

1. Upload de **bestanden** die u wilt verwerken met een [Azure Storage] [ azure_storage] account. Batch beschikt over ingebouwde ondersteuning voor toegang tot Azure-blobopslag en uw taken kunnen deze bestanden downloaden te [berekenen van knooppunten](#compute-node) wanneer de taken worden uitgevoerd.

2. Upload de **toepassingsbestanden** die uw taken wordt uitgevoerd. Deze bestanden kunnen worden binaire bestanden of scripts en bijbehorende afhankelijkheden en worden uitgevoerd door de taken in uw taken. Uw taken kunnen deze bestanden downloaden vanuit uw account, opslag, of kunt u de functie [toepassingspakketten](#application-packages) van Batch voor toepassingsbeheer en implementatie.

3. Een [groep](#pool) van berekeningscluster knooppunten te maken. Als u een groep hebt gemaakt, geeft u het aantal knooppunten berekeningscluster voor de groep, de grootte en het besturingssysteem. Wanneer u elke taak in uw taak wordt uitgevoerd, is deze toegewezen uitvoeren op een van de knooppunten in uw groep.

4. Een [taak](#job)maken. Een taak worden beheerd in een verzameling taken. U koppelen elke taak naar een specifieke groep waar van dat project taken worden uitgevoerd.

5. [Taken](#task) toevoegen aan de taak. Elke taak wordt uitgevoerd voor de toepassing of script die u hebt geüpload te verwerken van de gegevensbestanden die het downloaden van uw account opslag. Als u elke taak is voltooid, kunt deze de uitvoer uploaden naar Azure Storage.

6. Taakvoortgang bewaken en de uitvoer van de taak op te halen uit de opslag van Azure.

De volgende secties worden deze en de andere bronnen van Batch waarmee uw verdeelde rekenkundige scenario.

> [AZURE.NOTE] U moet een [Batch account](batch-account-create-portal.md) de service Batch wilt gebruiken. Vrijwel alle oplossingen gebruik bovendien een [Azure Storage] [ azure_storage] verwerking van het bestand opslaan en ophalen. Batch momenteel ondersteunt alleen het **algemene** opslag accounttype, zoals wordt beschreven in stap 5 van [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [over Azure opslag-accounts](../storage/storage-create-storage-account.md).

## <a name="batch-service-resources"></a>Batch serviceresources

Enkele van de volgende bronnen---accounts berekenen knooppunten, pools, taken en taken--is vereist door alle oplossingen die de Batch-service gebruiken. Andere, zijn zoals taakplanningen en -toepassingspakketten, handig maar optioneel functies.

- [Account](#account)
- [Knooppunt berekenen](#compute-node)
- [Groep](#pool)
- [Taak](#job)

  - [Project-planningen](#scheduled-jobs)

- [Taak](#task)

  - [Taak starten](#start-task)
  - [Projecttaak manager](#job-manager-task)
  - [Voorbereiding van en release van projecttaken](#job-preparation-and-release-tasks)
  - [Meerdere exemplaren taak (MPI)](#multi-instance-tasks)
  - [Taakafhankelijkheden](#task-dependencies)

- [Toepassingspakketten](#application-packages)

## <a name="account"></a>Account

Een account van de Batch is een uniek geïdentificeerd entiteit binnen de Batch-service. Alle verwerking is gekoppeld aan een Batch-account. Wanneer u bewerkingen met de Batch-service uitvoeren, moet u zowel de accountnaam van de en een van de account te drukken. U kunt [een Batch Azure-account met behulp van de Azure portal maken](batch-account-create-portal.md).

## <a name="compute-node"></a>Knooppunt berekenen

Een berekeningscluster knooppunt is een Azure virtuele machine (VM) die is toegewezen aan het verwerken van een gedeelte van de werklast van de toepassing. De grootte van een knooppunt bepaalt het aantal CPU cores, het geheugen en lokale systeem bestandsgrootte die is toegewezen aan het knooppunt. U kunt toepassingen van Windows of Linux knooppunten maken met behulp van Azure Cloud Services of virtuele Machines Marketplace afbeeldingen. Zie de volgende sectie [toepassingen](#pool) voor meer informatie over deze opties.

Knooppunten kunnen een uitvoerbaar bestand of script die wordt ondersteund door de omgeving van het besturingssysteem van het knooppunt worden uitgevoerd. Dit geldt ook voor \*.exe, \*cmd, \*type en PowerShell-scripts voor Windows-- en binaire bestanden, skelet en Python scripts voor Linux.

Alle berekenen knooppunten in Batch ook opnemen:

- Een [mapstructuur](#files-and-directories) en een bijbehorende [omgevingsvariabelen](#environment-settings-for-tasks) die beschikbaar voor de verwijzing door taken zijn.
- **Firewallinstellingen die zijn geconfigureerd voor toegang beheren.**
- [Externe toegang](#connecting-to-compute-nodes) tot Windows (Remote Desktop Protocol (RDP)) en knooppunten Linux (Secure Shell (SSH)).

## <a name="pool"></a>Groep

Een groep is een verzameling knooppunten die op uw toepassing wordt uitgevoerd. De groep kan handmatig worden gemaakt door u of automatisch door de service Batch wanneer u opgeeft dat het werk dat moet worden uitgevoerd. U kunt maken en beheren van een resourcegroep die voldoet aan de resourcevereisten van uw toepassing. Een groep kan alleen worden gebruikt door de Batch account waarin deze is gemaakt. Een account Batch kan meer dan één groep hebben.

Azure Batch van toepassingen maken boven aan het core Azure berekeningscluster-platform. Ze bieden grootschalige toewijzing, de installatie van toepassing, gegevens verdeling, statuscontrole en flexibele aanpassing van het aantal berekeningscluster knooppunten in een groep ([schaalbaarheid](#scaling-compute-resources)).

Elk knooppunt die is toegevoegd aan een groep is toegewezen een unieke naam en het IP-adres. Wanneer een knooppunt wordt verwijderd uit een groep, wijzigingen die zijn aangebracht in het besturingssysteem of de bestanden gaan verloren en de naam en het IP-adres zijn vrijgegeven voor toekomstig gebruik. Wanneer een knooppunt een resourcegroep die verlaat, wordt de levensduur is verstreken.

Wanneer u een groep hebt gemaakt, kunt u de volgende kenmerken:

- Knooppunt **besturingssysteem** en **versie** berekenen

    U hebt twee opties wanneer u een besturingssysteem voor de knooppunten in uw groep selecteert: **VM configuratie** en **Configuratie van de Cloud-Services**.

    **VM configuratie** biedt Linux- en Windows-afbeeldingen voor berekenen knooppunten van [Azure virtuele Machines Marketplace][vm_marketplace].
    Wanneer u een groep met VM configuratie knooppunten maakt, moet u niet alleen de grootte van de knooppunten, maar ook de **verwijzing naar de afbeelding van virtuele machine** en de Batch **knooppunt agent SKU** op de knooppunten zijn geïnstalleerd. Zie de [voorziening Linux berekenen knooppunten in Azure Batch van toepassingen](batch-linux-nodes.md)voor meer informatie over het opgeven van deze groep eigenschappen.

    **Configuratie van cloud Services** biedt dat Windows knooppunten *alleen*berekenen. Beschikbare besturingssystemen voor Cloud Services-configuratie van toepassingen worden vermeld in de [Azure Gast OS releases en SDK compatibiliteit matrix](../cloud-services/cloud-services-guestos-update-matrix.md). Wanneer u een groep met Cloudservices knooppunten maakt, moet u de grootte van het knooppunt en *OS familie*opgeven. Bij het maken van groepen van Windows berekenen knooppunten, u Cloud Services voor het meest worden gebruikt.

    - De *OS familie* ook bepaalt welke versies van .NET worden geïnstalleerd met het besturingssysteem.
    - Als u met de rollen van de werknemer binnen Cloud Services, kunt u een *Versie van het besturingssysteem* (Zie de sectie [laten weten over cloudservices](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) in de [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md)voor meer informatie over rollen werknemer,).
    - Als u met de rol van de werknemer, het is raadzaam dat u opgeeft `*` voor de *Versie van het besturingssysteem* , zodat de knooppunten automatisch bijgewerkt worden en er geen tijdelijke vereist is om geschikt zijn voor nieuwe versies uitgebracht. De primaire use-case voor het selecteren van een specifieke versie van het besturingssysteem is om ervoor te zorgen compatibiliteit van toepassingen, waarmee de compatibiliteit met eerdere versies testen om te worden uitgevoerd voordat de versie moet worden bijgewerkt. Na validatie, de *Versie van het besturingssysteem* voor de toepassingen kunnen worden bijgewerkt en de afbeelding van de nieuwe OS kan worden geïnstalleerd--eventuele actieve taken worden gestoord en opnieuw.

- **Grootte van de knooppunten**

    **Cloud Services-configuratie** berekeningscluster knooppunt grootten worden vermeld in [formaten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md). Batch ondersteunt kleine Cloud Services behalve `ExtraSmall`.

    **VM configuratie** berekenen knooppunt tekengrootten worden weergegeven in [formaten voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) en de [grootte voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Batch ondersteuning biedt voor alle Azure VM formaten, behalve `STANDARD_A0` en de premium-opslag (`STANDARD_GS`, `STANDARD_DS`, en `STANDARD_DSV2` reeks).

    Als u de grootte van een berekeningscluster knooppunt selecteert, kunt u de kenmerken en de vereisten van de toepassingen die u op de knooppunten uitvoeren wilt. Aspecten zoals of de toepassing wordt meerdere threads en hoeveel geheugen deze verbruikt kan helpen Hiermee bepaalt u de grootte van het meest geschikt en efficiënt knooppunt. Het is typisch een knooppunt grootte ervan uitgaande dat er één taak kan worden uitgevoerd op een knooppunt tegelijk selecteren. Echter, is het mogelijk om meerdere taken (en dus meerdere toepassingsexemplaren) knooppunten [parallel uitvoeren](batch-parallel-node-tasks.md) op berekenen tijdens de taakuitvoering. In dit geval is het algemene knooppunt groter zijn voor de grotere vraag van de uitvoering van parallelle taken kiezen. Zie [taakplanning beleid](#task-scheduling-policy) voor meer informatie.

    De knooppunten in een groep allemaal dezelfde grootte. Als wilt uitvoeren van toepassingen met verschillende systeemvereisten en/of laden niveaus, wordt aangeraden u afzonderlijke van toepassingen gebruiken.

- **Het aantal knooppunten doel**

    Dit is het aantal berekeningscluster knooppunten die u wilt implementeren in de groep. Dit is genoemd, een *doeltoepassing* omdat, in sommige gevallen, uw groep mogelijk niet heeft bereikt het gewenste aantal knooppunten. Een resourcegroep die mogelijk niet het gewenste aantal knooppunten heeft bereikt als dit het [quotum voor core](batch-quota-limit.md#batch-account-quotas) voor uw account Batch--bereikt of als er een automatisch schalen formule die u hebt toegepast op de toepassingen die beperkt het maximum aantal knooppunten (Zie de volgende sectie voor "Schaalbaarheid beleid").

- **Schaalbaarheid van beleid**

    Naast het opgeven van een statische aantal knooppunten, kunt u in plaats daarvan schrijven en een [formule automatisch schalen](#scaling-compute-resources) toepassen op een groep. De service Batch regelmatig evalueert de formule en past u het aantal knooppunten binnen de groep die op basis van verschillende toepassingen, functie en taakparameters die u kunt opgeven.

- **Beleid voor taakplanning**

    De optie [max taken per knooppunt](batch-parallel-node-tasks.md) configuratie bepaalt het maximum aantal taken die parallel kan worden uitgevoerd op elk knooppunt berekeningscluster in de groep.

    De standaardconfiguratie is dat één taak per keer wordt uitgevoerd op een knooppunt, maar er scenario's waarop de invoegtoepassing nuttig zijn is om meer dan één taak uitgevoerd op een knooppunt tegelijk. Zie het [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het artikel [gelijktijdige knooppunt taken](batch-parallel-node-tasks.md) om te zien hoe u kunt profiteren van meerdere taken per knooppunt.

    U kunt ook een *Opvullingstype* die bepaalt of Batch als een dubbele de taken gelijkmatig op alle knooppunten in een groep of elk knooppunt met het maximum aantal taken packs voordat u taken toewijzen aan een ander knooppunt opgeven.

- **Status van de communicatie** van knooppunten berekenen

    In de meeste gevallen taken werken onafhankelijk en hoeft niet te communiceren met elkaar. Er zijn echter enkele toepassingen waarin taken, zoals [MPI scenario's communiceren moeten](batch-mpi.md).

    U kunt een groep zodat de communicatie tussen de knooppunten binnen it--**de communicatie tussen knooppunten**configureren. Wanneer de communicatie tussen knooppunten is ingeschakeld, knooppunten in Cloud Services-configuratie van toepassingen kunnen communiceren met elkaar op poorten groter is dan 1100 en VM configuratie van toepassingen niet verkeer op een willekeurige poort beperken.

    Houd er rekening mee dat het inschakelen van de communicatie tussen knooppunten ook invloed op de positie van de knooppunten binnen clusters en het maximum aantal knooppunten in een groep vanwege implementatie beperkingen mogelijk beperken. Als uw toepassing niet de communicatie tussen knooppunten hoeft, de Batch-service kan een potentieel groot aantal knooppunten toewijzen aan de groep uit een groot aantal verschillende clusters en datacenters om in te schakelen parallelle verwerking power verhoogd.

- Voor berekeningscluster knooppunten **taak starten**

    Het optionele *taak starten* wordt uitgevoerd op elk knooppunt zoals u dat knooppunt deelneemt aan de groep en telkens wanneer een knooppunt wordt opgestart of reimaged. De starttaak is vooral handig voor het voorbereiden van berekeningscluster knooppunten voor de uitvoering van taken, zoals de toepassingen die uw taken uitgevoerd op de knooppunten berekeningscluster installeren.

- **Toepassingspakketten**

    [Toepassingspakketten](#application-packages) om te implementeren naar de knooppunten berekenen in de groep, kunt u opgeven. Toepassingspakketten bieden vereenvoudigde implementatie en versiebeheer van de toepassingen die uw taken uitvoeren. Toepassingspakketten die u voor een resourcegroep die opgeeft zijn geïnstalleerd op elk knooppunt die aan elkaar die groep koppelt en telkens wanneer er een knooppunt wordt opnieuw gestart of reimaged. Toepassingspakketten worden momenteel niet ondersteund op Linux berekeningscluster knooppunten.

- **Netwerkconfiguratie**

    De ID van een Azure [virtuele netwerk (VNet)](../virtual-network/virtual-networks-overview.md) waarin van de groep knooppunten berekenen moet worden gemaakt, kunt u opgeven. Vereisten voor het opgeven van een VNet voor uw groep u in [een resourcegroep die bij een account toevoegen vindt] [ vnet] in de Batch REST API-verwijzing.

> [AZURE.IMPORTANT] Alle Batchaccounts hebt een standaard- **quota** waardoor het aantal **cores** (en dus berekeningscluster knooppunten) in een Batch-account. U vindt de standaardquota en instructies voor het [vergroten van een quota](batch-quota-limit.md#increase-a-quota) (zoals het maximum aantal cores in uw account Batch) in [quota en -limieten voor de Batch Azure-service](batch-quota-limit.md). Als u merkt vragen "Waarom won't mijn groep hebt bereikt meer dan knooppunten X?" Dit quotum core mogelijk de oorzaak zijn.

## <a name="job"></a>Taak

Een taak is een verzameling taken. Deze beheert hoe berekening wordt uitgevoerd door de taken op de berekeningscluster knooppunten in een groep.

- De taak geeft de **groep** waarin het werk wordt uitgevoerd. U kunt een nieuwe toepassingen voor elke taak maken of een groep gebruiken voor veel taken. U kunt een resourcegroep die voor elke taak die is gekoppeld aan een taakplanning of voor alle taken die zijn gekoppeld aan een projectplanning kunt maken.

- U kunt een optionele **functie prioriteit**opgeven. Wanneer een taak wordt verzonden met een hogere prioriteit dan de taken die momenteel uitgevoerd worden, worden de taken voor de taak hogere prioriteit ingevoegd in de wachtrij ligt taken voor de taken lagere prioriteit. Taken in een lagere prioriteit taken die al worden uitgevoerd, zijn niet afgebroken.

- U kunt taak **beperkingen** gebruiken om op te geven van bepaalde limieten voor uw taken:

    U kunt een **maximum wallclock tijd**, instellen zodat als een taak wordt uitgevoerd langer dan de maximale wallclock tijd die is opgegeven, de taak en alle taken worden beëindigd.

    Batch kunt detecteren in en probeer mislukte taken. Als een beperking, inclusief of een taak is *altijd* of *nooit* een nieuwe poging gedaan, kunt u het **maximum aantal pogingen taak** opgeven. Een taak opnieuw betekent dat de taak is opnieuw als u wilt opnieuw worden uitgevoerd.

- De clienttoepassing kunt taken toevoegen aan een taak of kunt u een [manager projecttaak](#job-manager-task)opgeven. Een manager projecttaak bevat de informatie die nodig is voor het maken van de vereiste taken voor een taak, met de projecttaak manager wordt uitgevoerd op een van de knooppunten berekeningscluster in de groep. De manager projecttaak wordt verwerkt specifiek door Batch--deze is in de wachtrij zodra de taak is gemaakt en opnieuw is opgestart als dat niet werkt. Een taak van de manager is *vereist* voor taken die zijn gemaakt door een [projectplanning](#scheduled-jobs) omdat dit de enige manier om te definiëren van de taken is voordat de taak is gemaakt.

- Al dan niet standaard blijven de taken in de actieve status als alle taken binnen de taak voltooid is. U kunt dit gedrag kunt wijzigen, zodat de taak wordt automatisch beëindigd als alle taken in de taak voltooid is. Stel van de taak **onAllTasksComplete** eigenschap ([OnAllTasksComplete] [ net_onalltaskscomplete] in Batch .NET) naar *terminatejob* automatisch de taak beëindigen wanneer alle taken in de voltooide staat worden.

    Houd er rekening mee dat de Batch-service van mening een taak *zonder* taken is dat alle taken worden voltooid. Deze optie wordt daarom meest worden gebruikt met een [manager projecttaak](#job-manager-task). Als u gebruiken automatische taak beëindiging zonder de manager van een taak wilt, moet u eerst stelt u een nieuwe taak **onAllTasksComplete** eigenschap op *noaction*en vervolgens Stel deze in op *terminatejob* alleen nadat u klaar bent met taken toevoegen aan de taak.

### <a name="job-priority"></a>Prioriteit van taak

U kunt een prioriteit toewijzen aan taken die u in Batch maakt. De Batch-service gebruikt de waarde van prioriteit van de taak om te bepalen de volgorde van taakplanning binnen een account (dit is niet te verwarren met een [geplande taak](#scheduled-jobs)). De prioriteit waarden variëren van-1000 tot 1000, met-1000 de laagste prioriteit en 1000 de hoogste. U kunt de prioriteit van een taak bijwerken met behulp van het [bijwerken van de eigenschappen van een taak] [ rest_update_job] bewerking (Batch REST) of doordat de [CloudJob.Priority] [ net_cloudjob_priority] eigenschap (Batch .NET).

Binnen hetzelfde account, wordt in hogere prioriteit taken hebben de prioriteit van regels via lagere prioriteit taken plannen. Een project met de waarde van een hogere prioriteit in één account beschikt niet over de prioriteit van regels plannen via een andere taak met een lage prioriteit waarde in een ander account.

Taakplanning over toepassingen is onafhankelijk. Tussen verschillende groepen, is deze niet gegarandeerd dat er eerst een hogere prioriteit taak is gepland als de bijbehorende toepassingen dan niet-actieve knooppunten is. Taken met de dezelfde prioriteit hebt in dezelfde groep, een gelijk kans van die is gepland.

### <a name="scheduled-jobs"></a>Geplande taken

[Taak planningen] [ rest_job_schedules] kunnen maken van terugkerende taken in de Batch-service. Een taakplanning geeft aan of u taken uitvoeren en bevat de specificaties voor de taken moeten worden uitgevoerd. De duur van de planning--hoe lang en wanneer de planning van kracht-- en hoe vaak tijdens deze periode die taken moeten worden gemaakt, kunt u opgeven.

## <a name="task"></a>Taak

Een taak is een maateenheid berekening die is gekoppeld aan een taak. Deze wordt uitgevoerd op een knooppunt. Taken zijn toegewezen aan een knooppunt voor uitvoering of in de wachtrij totdat een knooppunt gratis wordt. Eenvoudig gesteld: een taak wordt uitgevoerd een of meer programma's of scripts op een berekeningscluster knooppunt om uit te voeren van het werk dat u nodig hebt gedaan.

Wanneer u een taak maakt, kunt u opgeven:

- De **opdrachtregel** van de taak. Dit is de opdrachtregel die uw toepassing of een script op het knooppunt berekeningscluster wordt uitgevoerd.

    Het is belangrijk te weten dat de opdrachtregel niet daadwerkelijk wordt uitgevoerd onder een shell. Daarom deze kan niet standaard profiteren van shellfuncties zoals [omgevingsvariabele](#environment-settings-for-tasks) uitbreiding (deze groep omvat de `PATH`). Om te profiteren van dergelijke functies, start u de shell in de opdrachtregel--bijvoorbeeld door het starten van `cmd.exe` op Windows knooppunten of `/bin/sh` op Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Als uw taken moeten uitvoeren van een toepassing of script die zich niet in van het knooppunt `PATH` of verwijzen naar omgevingsvariabelen, roepen de shell expliciet in de opdrachtregel taak.

- **Resource-bestanden** met de gegevens die moeten worden verwerkt. Deze bestanden worden automatisch gekopieerd naar het knooppunt uit in een account met **algemene** opslag van Azure-blobopslag voordat de opdrachtregel van de taak wordt uitgevoerd. Zie voor meer informatie de sectie [taak starten](#start-task) en [bestanden en mappen](#files-and-directories).

- De **omgevingsvariabelen** die nodig zijn voor uw toepassing. Zie voor meer informatie de sectie [instellingen van de omgeving voor taken](#environment-settings-for-tasks) .

- De **limieten valt** waaronder de taak moet worden uitgevoerd. Voor voorbeeld, de maximale tijd dat de taak is toegestaan om uit te voeren, het maximum aantal keren die een mislukte taak moet opnieuw worden gestart, en de maximale tijd die bestanden in de werkmap van de taak worden bewaard.

- **Toepassing-pakketten** om te implementeren naar het knooppunt berekeningscluster is waarop de taak is gepland om uit te voeren. [Toepassing-pakketten](#application-packages) bieden vereenvoudigde implementatie en versiebeheer van de toepassingen die uw taken uitvoeren. Taak op gebruikersniveau toepassingspakketten zijn met name handig in gedeelde toepassingen omgevingen, waar andere taken worden uitgevoerd op een groep en de groep wordt niet verwijderd wanneer een taak is voltooid. Als uw project minder taken dan knooppunten in de groep heeft, kunnen taak toepassing-pakketten overdracht van gegevens minimaliseren omdat uw toepassing wordt geïmplementeerd alleen op de knooppunten die taken uitvoeren.

Naast de taken die u definieert om te berekenen op een knooppunt, zijn de volgende speciale taken ook beschikbaar door de service Batch:

- [Taak starten](#start-task)
- [Projecttaak manager](#job-manager-task)
- [Voorbereiding van en release van projecttaken](#job-preparation-and-release-tasks)
- [Meerdere exemplaren taken (MPI)](#multi-instance-tasks)
- [Taakafhankelijkheden](#task-dependencies)

### <a name="start-task"></a>Taak starten

Door te koppelen een **taak starten** met een groep, kunt u de besturingsomgeving knooppunten voorbereiden. U kunt bijvoorbeeld acties bijgehouden, zoals de toepassingen die uw taken uitgevoerd installeren en starten van achtergrondprocessen uitvoeren. De begintaak wordt uitgevoerd wanneer een knooppunt wordt gestart, voor zo lang maken als deze in de groep--blijft wanneer het knooppunt de eerste keer inclusief toegevoegd aan de groep en wanneer deze wordt opgestart of reimaged.

Een belangrijk voordeel van de starttaak is dat deze de informatie die nodig is voor het configureren van een berekeningscluster knooppunt en installeer de toepassingen die vereist voor de uitvoering van taken zijn kan bevatten. Het aantal knooppunten in een groep met groter wordende dus net zo eenvoudig als de informatie die nodig is voor het configureren van de nieuwe knooppunten en deze voorbereiden voor het accepteren van taken die de nieuwe doeltoepassing knooppunt telling--Batch al aangeeft heeft.

Als met een taak Azure Batch, u een lijst met **bronbestanden** in [Azure Storage opgeven kunt][azure_storage], naast een **opdrachtregel** moet worden uitgevoerd. Batch eerst de resource-bestanden wordt gekopieerd naar het knooppunt van Azure-opslag, en voert de opdrachtregel. Voor een taak van het starten van toepassingen bevat de bestandenlijst meestal de taak-toepassing en de bijbehorende afhankelijkheden.

Dit kan echter ook referentiegegevens moet worden gebruikt door alle taken die worden uitgevoerd op het knooppunt berekeningscluster bevatten. Bijvoorbeeld van een starttaak opdrachtregel kan uitvoeren een `robocopy` bewerking toepassingsbestanden (die zijn opgegeven als bronbestanden en dan ook gedownload naar het knooppunt) vanaf het van de begintaak [werkmap](#files-and-directories) kopiëren naar de [gedeelde map](#files-and-directories)en voer vervolgens een MSI-bestand of `setup.exe`.

> [AZURE.IMPORTANT] Batch momenteel ondersteunt *alleen* het **algemene** opslag-accounttype, zoals wordt beschreven in stap 5 van [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [over Azure opslag-accounts](../storage/storage-create-storage-account.md). Uw batchtaken (inclusief standaard taken, begin taken, voorbereiding van projecttaken en projecttaken definitieve versie) moeten resource-bestanden die zich *alleen* in de **algemene** opslag accounts bevinden opgeven.

Het is meestal wenselijk voor de service Batch moet wachten om door de starttaak om te voltooien voordat het knooppunt gereed om te worden toegewezen taken, maar u kunt dit configureren.

Als een starttaak op een berekeningscluster knooppunt mislukt, klikt u vervolgens de status van het knooppunt is bijgewerkt, zodat de fout en het knooppunt niet alle taken is toegewezen. Een starttaak kan mislukken als er een probleem kopiëren van de resource-bestanden van opslag, of als het proces dat wordt uitgevoerd door de opdrachtregel geeft als resultaat een afsluitcode niet nul.

Als u toevoegen of bijwerken van de starttaak voor een *bestaande* toepassingen, moet u de knooppunten berekeningscluster voor de starttaak moeten worden toegepast om de knooppunten opnieuw opstarten.

### <a name="job-manager-task"></a>Projecttaak manager

U meestal gebruik een **manager projecttaak** om te bepalen en/of uitvoering--bijvoorbeeld bewaken maken en indienen van de taken voor een taak, bepalen aanvullende taken kunt uitvoeren en bepalen wanneer werk voltooid is. Een taak van de manager is echter niet beperkt tot deze activiteiten. Dit is een volledig zelfstandige taak die bewerkingen die vereist voor de taak zijn kunt uitvoeren. Een manager projecttaak mogelijk bijvoorbeeld downloaden van een bestand dat is opgegeven als een parameter, de inhoud van het bestand analyseren en aanvullende taken op basis van deze inhoud verzenden.

Een taak van de manager is gestart voordat alle andere taken. Deze biedt de volgende functies:

- Dit wordt automatisch verstuurd als een taak door de service Batch wanneer de taak is gemaakt.

- Moet worden uitgevoerd voordat de andere taken in een taak is gepland.

- Het knooppunt dat de bijbehorende is de laatste moeten worden verwijderd uit een groep wanneer zijn, wordt de groep wordt verkleind.

- De overeenkomst kan worden gekoppeld aan de beëindiging van alle taken in de taak.

- Een manager projecttaak krijgt de hoogste prioriteit wanneer deze moet opnieuw worden gestart. Als een niet-actieve knooppunt niet beschikbaar is, kan een van de andere actieve taken in de groep ruimte maken voor de manager projecttaak om uit te voeren door de service Batch beëindigen.

- Een taak van de manager in één taak bevat geen prioriteit boven de taken van andere taken. In projecten, worden alleen taak niveau prioriteiten waargenomen.

### <a name="job-preparation-and-release-tasks"></a>Voorbereiding van en release van projecttaken

Voorbereiding van projecttaken biedt batch voor oude uitvoering-instelling. Projecttaken release zijn geschikt voor post taak onderhoud of opruimen.

- **Voorbereiding van projecttaak**: voorbereiding van een taak wordt uitgevoerd op alle berekeningscluster knooppunten die zijn gepland taken uitvoert voordat een van de andere projecttaken worden uitgevoerd. U kunt een taak definiëren voorbereidende naar gegevens kopiëren die wordt gedeeld door alle taken, maar uniek is voor de taak, bijvoorbeeld gebruiken.
- **Projecttaak release**: wanneer een taak is voltooid, de taak van een taak vrijgeven op elk knooppunt in de toepassingen die ten minste één taak uitgevoerd wordt uitgevoerd. U kunt een taak van de release definiëren om gegevens die wordt gekopieerd door de voorbereidende projecttaak, te verwijderen of comprimeren en uploadt u diagnostische gegevens, bijvoorbeeld.

Beide voorbereiding van taak en release taken kunnen u de opdrachtregel worden uitgevoerd wanneer de taak wordt aangeroepen opgeven. Deze bieden functies beschikbaar, zoals bestand downloaden, verhoogde worden uitgevoerd, aangepaste omgevingsvariabelen, maximum execution duur, aantal nieuwe pogingen, en bestand bewaarbeleid tijd.

Voor meer informatie over het voorbereiden en release projecttaken, raadpleegt u [uitvoeren voorbereiden en voltooiing projecttaken op Azure Batch knooppunten berekenen](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Meerdere exemplaren taak

Een [taak van meerdere exemplaren](batch-mpi.md) is een taak die is geconfigureerd om uit te voeren op meer dan één berekeningscluster knooppunt tegelijk. Met meerdere exemplaren taken kunt u krachtige situaties waarvoor een groep berekeningscluster knooppunten die bij elkaar te verwerken van een enkel werkbelasting (zoals bericht Passing Interface (MPI)) worden toegewezen.

Voor gedetailleerde informatie over het uitvoeren van MPI taken in de Batch met behulp van de Batch .NET-bibliotheek, raadpleegt u [Gebruik meerdere exemplaren taken bericht Passing Interface (MPI) toepassingen in Azure Batch uit te voeren](batch-mpi.md).

### <a name="task-dependencies"></a>Taakafhankelijkheden

[Taakafhankelijkheden](batch-task-dependencies.md), zoals de naam aangeeft, kunt u opgeven dat een taak afhankelijk van de voltooiing van andere taken voor de uitvoering is. Deze functie biedt ondersteuning voor situaties waarin een 'volgende' verbruik de uitvoer van een taak "boven"-- of wanneer de taak in een boven sommige initialisatie dat nodig is door een volgende taak uitvoert. Deze functie wilt gebruiken, moet u eerst taakafhankelijkheden inschakelen voor uw taak Batch. Klik voor elke taak die afhankelijk van een andere is (of vele andere), u de taken die deze taak afhankelijk van is opgeeft.

Met taakafhankelijkheden, kunt u scenario's als volgt uit:

* *taskB* afhankelijk van de *taskA* (*taskB* execution pas begint *taskA* is voltooid).
* *taskC* , hangt af van de *taskA* en *taskB*.
* *taskD* , is afhankelijk van wat u een bereik van taken, zoals taken, *1* tot en met *10*, voordat deze wordt uitgevoerd.

Bekijk [taakafhankelijkheden in Azure Batch](batch-task-dependencies.md) en de [TaskDependencies] [ github_sample_taskdeps] voorbeeld van de code in de [azure-batch-voorbeelden] [ github_samples] GitHub-bibliotheek voor meer gedetailleerde informatie over deze functie.

## <a name="environment-settings-for-tasks"></a>Omgevingsinstellingen voor taken

Elke taak die is uitgevoerd door de service Batch heeft toegang tot omgevingsvariabelen die wordt ingesteld op berekeningscluster knooppunten. Dit geldt ook voor omgevingsvariabelen die is gedefinieerd door de Batch-service ([service gedefinieerde][msdn_env_vars]) en de aangepaste variabelen die u voor uw taken definiëren kunt. Toegang tot deze omgevingsvariabelen is tijdens het uitvoeren van de toepassingen en scripts die uw taken uitvoeren.

U kunt aangepaste omgevingsvariabelen op het niveau van de taak instellen door de eigenschap *omgevingsinstellingen* voor deze entiteiten vullen. Zie bijvoorbeeld de [een taak aan een taak toevoegen] [ rest_add_task] bewerking (Batch REST API) of de [CloudTask.EnvironmentSettings] [ net_cloudtask_env] en [CloudJob.CommonEnvironmentSettings] [ net_job_env] eigenschappen in Batch .NET.

Uw clienttoepassing of de service kunt verkrijgen van een taak-omgevingsvariabelen, zowel service gedefinieerde en aangepaste, met de [informatie over een taak] [ rest_get_task_info] bewerking (Batch REST) of via de [CloudTask.EnvironmentSettings] [ net_cloudtask_env] eigenschap (Batch .NET). Processen uitvoeren op een berekeningscluster knooppunt hebben toegang tot deze en andere variabelen op het knooppunt, bijvoorbeeld via de vertrouwde `%VARIABLE_NAME%` (Windows) of `$VARIABLE_NAME` syntaxis van de (Linux).

U kunt een volledige lijst met alle service gedefinieerde omgevingsvariabelen vinden in het [knooppunt omgevingsvariabelen berekenen][msdn_env_vars].

## <a name="files-and-directories"></a>Bestanden en mappen

Elke taak heeft een *werkmap* waaronder dat wordt gemaakt nul of meer bestanden en mappen. Deze werkmap kan worden gebruikt voor het opslaan van het programma dat wordt uitgevoerd door de taak, de gegevens die worden verwerkt en de uitvoer van de verwerking die worden uitgevoerd. Alle bestanden en mappen van een taak zijn eigendom van de taak-gebruiker.

De Batch-service stelt een gedeelte van het bestandssysteem op een knooppunt als de *hoofdmap*. Taken hebben toegang tot de hoofdmap door te verwijzen naar de `AZ_BATCH_NODE_ROOT_DIR` omgevingsvariabele. Zie voor meer informatie over het gebruik van de omgevingsvariabelen [omgevingsinstellingen voor taken](#environment-settings-for-tasks).

De hoofdmap bevat de volgende mapstructuur:

![Mapstructuur knooppunt berekenen][1]

- **gedeelde**: deze map bevat alleen-lezen/schrijven toegang tot *alle* taken die worden uitgevoerd op een knooppunt. Elke taak die wordt uitgevoerd op het knooppunt kunt maken, lezen, bijwerken en verwijderen van bestanden in deze map. Taken hebben toegang tot deze map door te verwijzen naar de `AZ_BATCH_NODE_SHARED_DIR` omgevingsvariabele.

- **opstarten**: deze map wordt gebruikt door een starttaak als de werkmap. Alle bestanden die worden gedownload naar het knooppunt door de begintaak worden hier opgeslagen. De begintaak kunt maken, lezen, bijwerken en verwijder bestanden onder deze map. Taken hebben toegang tot deze map door te verwijzen naar de `AZ_BATCH_NODE_STARTUP_DIR` omgevingsvariabele.

- **Taken**: een map wordt gemaakt voor elke taak die wordt uitgevoerd op het knooppunt. Dit wordt geopend door te verwijzen naar de `AZ_BATCH_TASK_DIR` omgevingsvariabele.

    Binnen elke taak directory, de Batch-service Hiermee maakt u een werkmap (`wd`) waarvan het unieke pad is opgegeven door de `AZ_BATCH_TASK_WORKING_DIR` omgevingsvariabele. Deze map biedt alleen-lezen/schrijven toegang tot de taak. De taak kunt maken, lezen, bijwerken en verwijder bestanden onder deze map. Deze map blijft behouden op basis van de *RetentionTime* restrictie die is opgegeven voor de taak.

    `stdout.txt`en `stderr.txt`: deze bestanden worden opgeslagen in de taakmap tijdens de uitvoering van de taak.

>[AZURE.IMPORTANT] Wanneer een knooppunt wordt verwijderd uit de groep, worden *alle* bestanden die zijn opgeslagen op het knooppunt worden verwijderd.

## <a name="application-packages"></a>Toepassingspakketten

De functie [toepassingspakketten](batch-application-packages.md) biedt eenvoudig beheer en distributie van toepassingen naar de knooppunten berekenen in uw toepassingen. U kunt uploaden en beheren van meerdere versies van de toepassingen die door uw taken, met inbegrip van hun binaire bestanden en ondersteunende bestanden worden uitgevoerd. Vervolgens kunt u automatisch een of meer van deze toepassingen naar de knooppunten berekeningscluster implementeren in uw groep.

Toepassingspakketten op het niveau van de groep en de taak, kunt u opgeven. Wanneer u de toepassing-pakketten van toepassingen opgeeft, wordt de toepassing wordt geïmplementeerd op elk knooppunt in de groep. Wanneer u de toepassing-pakketten taak opgeeft, de toepassing wordt geïmplementeerd alleen naar knooppunten die zijn gepland om uit te voeren ten minste één van de taak taken, voordat de opdrachtregel van de taak wordt uitgevoerd.

Batch omgaat met de details van het werken met Azure opslagruimte voor uw toepassingspakketten opslaan en implementeren om te berekenen van knooppunten, zodat uw code en de management realiseren kan worden vereenvoudigd.

Meer informatie over de functie voor het pakket van toepassing, raadpleegt u [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md).

>[AZURE.NOTE] Als u de toepassing-pakketten van toepassingen aan een *bestaande* toepassingen toevoegt, moet u de knooppunten berekeningscluster voor de toepassingspakketten op de knooppunten kunnen worden geïmplementeerd opnieuw opstarten.

## <a name="pool-and-compute-node-lifetime"></a>Levensduur van toepassingen en berekeningscluster knooppunt

Wanneer u uw oplossing Azure Batch ontwerpt, die u moet maken van een ontwerp beslissing over hoe en wanneer pools worden gemaakt, en hoe lang berekenen knooppunten binnen de groepen worden gehouden beschikbaar.

U kunt op één uiteinde van de verschillende, een resourcegroep die voor elke taak ingediende foutenrapporten maken en verwijderen van de groep zodra de taken na de einddatum worden uitgevoerd. Dit gemaximaliseerd gebruik omdat de knooppunten alleen toegewezen zijn wanneer nodig en afsluiten zodra ze niet actief geweest. Dit betekent dat de taak tot de knooppunten wachten moet moet worden toegewezen, is het belangrijk te weten dat taken worden gepland voor uitvoering zodra knooppunten afzonderlijk beschikbaar is, toegewezen zijn, en de begintaak is voltooid. Batch wordt *niet* wachten totdat alle knooppunten binnen een resourcegroep die beschikbaar zijn voor het toewijzen van taken aan de knooppunten. Dit zorgt ervoor dat optimaal gebruik van alle beschikbare knooppunten.

Aan de andere kant van de verschillende, als taken direct gestart met de hoogste prioriteit, kunt u maken van een resourcegroep die tijd vooraf en maak de knooppunten beschikbaar voordat taken worden verzonden. In dit scenario taken direct kunnen worden gestart, maar knooppunten mogelijk inactief tijdens het wachten totdat ze worden toegewezen.

Een gecombineerde aanpak wordt meestal gebruikt voor het afhandelen van een variabele, maar lopende, laden. Kunt u beschikken over een resourcegroep die meerdere taken worden verzonden naar, maar kunnen schalen dat het aantal knooppunten omhoog of omlaag op basis van de taak laden (Zie [schaal berekenen resources](#scaling-compute-resources) in de volgende sectie). U kunt dit doen ontwikkelingscyclus, gebaseerd op de huidige belasting of proactief, als laden kan worden voorspeld.

## <a name="scaling-compute-resources"></a>Schaalbaarheid van berekeningscluster resources

U kunt de Batch-service dynamisch aanpassen het aantal berekeningscluster knooppunten in een groep op basis van de huidige werkbelasting en resource gebruik van uw scenario berekeningscluster hebben bij [Automatische schaling](batch-automatic-scaling.md). Hiermee kunt u naar rechtsonder van de totale kosten van het uitvoeren van uw toepassing met alleen de bronnen die u nodig hebt en vrijgeven die u niet nodig hebt.

U inschakelen automatische schaling met een [Automatische schaal formule](batch-automatic-scaling.md#automatic-scaling-formulas) te typen en die formule koppelen aan een groep. De Batch-service wordt de formule gebruikt om te bepalen van het doel aantal knooppunten in de groep voor de volgende schaal interval (een interval die u kunt configureren). Wanneer u deze hebt gemaakt of het inschakelen van schalen op een resourcegroep die later, kunt u de instellingen voor Automatische schaal voor een groep opgeven. U kunt ook de schaal instellingen op een groep schaalbaarheid ingeschakelde bijwerken.

Als u bijvoorbeeld misschien een taak is vereist dat u een groot aantal taken verzendt moeten worden uitgevoerd. U kunt een schaal formule toewijzen aan de toepassingen die Hiermee past u het aantal knooppunten in de groep op basis van het huidige aantal taken in de wachtrij en het tarief weer dat voltooiing van de taken in de taak. De service Batch regelmatig evalueert de formule en het formaat van de groep, op basis van de werklast passen (knooppunten voor veel wachtrijtaken taken toevoegen en verwijderen van knooppunten voor geen taken in de wachtrij of wordt uitgevoerd) en andere instellingen die uw formule.

Een schaal formule kan worden gebaseerd op de doelstellingen van de volgende:

- **Tijd aan de doelstellingen** zijn gebaseerd op elke vijf minuten in het opgegeven aantal uren verzamelde statistieken.

- **Resource aan de doelstellingen** zijn gebaseerd op CPU gebruik, bandbreedtegebruik geheugengebruik en aantal knooppunten.

- **De doelstellingen van de taak** op basis van de taakstatus, zoals *Active* (in wachtrij), *uitgevoerd*of *voltooid*.

Als automatische schaling het aantal berekeningscluster knooppunten in een groep afneemt, moet u rekening houden met hoe u omgaat met taken die worden uitgevoerd op het moment van de bewerking verkleinen. Daarvoor biedt Batch een *knooppunt af te breken optie* die u wilt in uw formules opnemen. U kunt bijvoorbeeld opgeven dat actieve taken zijn onmiddellijk gestopt, onmiddellijk gestopt en vervolgens opnieuw voor uitvoering op een ander knooppunt of toegestaan voltooien voordat het knooppunt wordt verwijderd uit de groep.

Zie [automatisch de knooppunten in een Azure Batch van toepassingen voor het berekenen van schaal](batch-automatic-scaling.md)voor meer informatie over het automatisch schaalbaarheid van een toepassing.

> [AZURE.TIP] Instellen als wilt maximaliseren berekeningscluster Resourcegebruik, het doel aantal knooppunten nul aan het einde van een taak, maar toestaan actieve taken om te voltooien.

## <a name="security-with-certificates"></a>Beveiliging met certificaten

U meestal moet certificaten gebruiken wanneer u versleutelen of vertrouwelijke informatie voor taken, zoals de toets voor de [opslag van Azure-account]van een ontsleutelen[azure_storage]. Daarom kunt u de certificaten op knooppunten installeren. Versleutelde geheimen worden doorgegeven aan taken via opdrachtregelparameters of ingesloten in een van de bronnen voor de taak en de geïnstalleerde certificaten kunnen worden gebruikt om ze ontsleutelen.

U gebruikt het [certificaat toevoegen] [ rest_add_cert] bewerking (Batch REST) of [CertificateOperations.CreateCertificate] [ net_create_cert] methode (Batch .NET) een certificaat toevoegen aan een Batch-account. Vervolgens kunt u het certificaat naar een nieuwe of bestaande groep koppelen. Wanneer een certificaat dat is gekoppeld aan een groep is, installeert u de Batch-service het certificaat op elk knooppunt in de groep. De Batch-service is geïnstalleerd de juiste certificaten wanneer het knooppunt wordt gestart, vóór het starten van alle taken (met inbegrip van de starttaak en projecttaak manager).

Als u certificaten aan een *bestaande* toepassingen toevoegen, moet u de knooppunten berekeningscluster voor de certificaten moeten worden toegepast om de knooppunten opnieuw opstarten.

## <a name="error-handling"></a>Foutafhandeling

U kunt vinden voor het afhandelen van taak- en -toepassing fouten binnen uw oplossing Batch.

### <a name="task-failure-handling"></a>Afhandeling van taak is mislukt
Fouten vallen in deze categorieën:

- **Planning van fouten**

    Als de overdracht van bestanden die zijn opgegeven voor een taak om welke reden dan ook is mislukt, wordt een 'planning fout"is ingesteld voor de taak.

    Planning van fouten kan optreden als bronbestanden van de taak hebt verplaatst, de opslag-account is niet langer beschikbaar of een ander probleem is opgetreden waardoor het succesvolle kopiëren van bestanden naar het knooppunt.

- **Toepassingsfouten**

    Het proces dat is opgegeven met de opdrachtregel van de taak kan ook mislukken. Het proces geacht wordt te zijn mislukt wanneer u een andere waarde dan nul afsluitcode wordt geretourneerd door het proces dat door de taak is uitgevoerd (Zie *taak afsluiten codes* in het volgende gedeelte).

    Voor toepassingsfouten, kunt u de Batch als u de taak tot aan een opgegeven aantal keren automatisch opnieuw wilt configureren.

- **Beperking fouten**

    U kunt een beperking waarmee de maximale execution duur voor een taak of een taak, de *maxWallClockTime*instellen. Dit is handig voor taken "vastgelopen" beëindigen.

    Wanneer de maximale hoeveelheid tijd is overschreden, de taak is gemarkeerd als *voltooid*, maar de afsluitcode is ingesteld op `0xC000013A` en het veld *schedulingError* is gemarkeerd als `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Voor foutopsporing in toepassingsfouten

- `stderr`en`stdout`

    Tijdens de uitvoering, kan een toepassing diagnostische uitvoer die u gebruiken kunt voor het oplossen van problemen opleveren. Zoals in de eerdere sectie [bestanden en mappen](#files-and-directories)vermeld, de Batch-service schrijft standaard uitvoer en standaardfout uitvoer naar `stdout.txt` en `stderr.txt` bestanden in de map taak op het knooppunt berekeningscluster. U kunt de portal van Azure of op een van de Batch SDK's deze bestanden downloaden. Bijvoorbeeld, kunt u deze en andere bestanden voor het oplossen van problemen met behulp van [ComputeNode.GetNodeFile] ophalen[ net_getfile_node] en [CloudTask.GetNodeFile] [ net_getfile_task] in de Batch .NET-bibliotheek.

- **Taak afsluitcodes**

    Zoals eerder is vermeld, wordt een taak is gemarkeerd als is door de Batch-service is mislukt als het proces dat wordt uitgevoerd door de taak als een afsluitcode niet nul resultaat. Wanneer een taak wordt uitgevoerd van een proces, worden de Batch van de taak afsluiten code eigenschap met de *code van het proces te retourneren*. Het is belangrijk te weten dat van een taak afsluitcode **niet** bepaald door de service Batch is--dit wordt bepaald door het proces zelf of het besturingssysteem waarop het proces wordt uitgevoerd.

### <a name="accounting-for-task-failures-or-interruptions"></a>Financieel voor fouten of onderbrekingen

Taken mislukt mogelijk af en toe of worden gestoord. De taak-toepassing zelf mislukt mogelijk, het knooppunt waarop de taak wordt uitgevoerd mogelijk opnieuw worden gestart of het knooppunt mogelijk verwijderd uit de groep tijdens een bewerking formaat wijzigen van de groep af te breken beleid is ingesteld op knooppunten onmiddellijk verwijderen zonder het wachten op taken om te voltooien. In alle gevallen kan de taak worden automatisch opnieuw door Batch voor uitvoering op een ander knooppunt.

Het is ook mogelijk dat een door onregelmatige probleem oorzaak van een taak vastloopt of duurt lang uit te voeren. U kunt de maximale duur van de uitvoering van een taak instellen. Als dit wordt overschreden, onderbreekt Batch de taak-toepassing.

### <a name="connecting-to-compute-nodes"></a>Verbinding maken met knooppunten berekenen

U kunt extra foutopsporing en probleemoplossing aanmeldt bij een berekeningscluster knooppunt op afstand uitvoeren. U kunt de portal van Azure downloaden van een bestand Remote Desktop Protocol (RDP) voor Windows knooppunten en Secure Shell (SSH) verbindingsinformatie voor Linux knooppunten verkrijgen. U kunt dit ook doen met behulp van de Batch API's--bijvoorbeeld met [Batch.NET] [ net_rdpfile] of [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Als u wilt verbinden met een knooppunt via RDP of SSH, moet u eerst een gebruiker maken op het knooppunt. Hiervoor kunt u de Azure portal, [een gebruiker toevoegen aan een knooppunt] [ rest_create_user] bellen met behulp van de Batch REST API, de [ComputeNode.CreateComputeNodeUser] [ net_create_user] methode in Batch .NET of gesprek de [add_user] [ py_add_user] methode in de Batch Python-module.

### <a name="troubleshooting-bad-compute-nodes"></a>Probleemoplossing "bad" berekenen knooppunten

In situaties waarin enkele van uw taken zijn verbroken, uw Batch-clienttoepassing of service kan bekijken die de metagegevens van de mislukte taken om een vastloopt zodat knooppunt te identificeren. Elk knooppunt in een groep krijgt een unieke ID en het knooppunt waarop een taak wordt uitgevoerd in de metagegevens van de taak is opgenomen. Nadat u een probleem knooppunt hebt geïdentificeerd, kunt u verschillende acties met deze uitvoeren:

- **Start het knooppunt opnieuw op** ([REST][rest_reboot] | [.NET][net_reboot])

    Opnieuw starten van het knooppunt kunt soms wissen van latente problemen zoals zitten of vastgelopen processen. Houd er rekening mee dat als uw groep gebruikmaakt van een starttaak of uw document een voorbereidende projecttaak wordt, ze worden uitgevoerd wanneer het knooppunt opnieuw wordt gestart.

- **Het knooppunt reimage** ([REST][rest_reimage] | [.NET][net_reimage])

    Dit op het knooppunt het besturingssysteem opnieuw installeert. Net als met een knooppunt opgestart, start van taken en voorbereiding van taken worden opnieuw nadat het knooppunt heeft zijn reimaged.

- **Het knooppunt uit de groep verwijderen** ([REST][rest_remove] | [.NET][net_remove])

    Het is soms nodig het knooppunt volledig te verwijderen uit de groep.

- **Klik op het knooppunt voor taakplanning uitschakelen** ([REST][rest_offline] | [.NET][net_offline])

    Dit effectief duurt het knooppunt "offline" zodat er geen verdere taken zijn toegewezen, maar kunt het knooppunt moet blijven actief zijn en in de groep. Hiermee kunt u verdere onderzoek naar de oorzaak van de fouten uitvoeren zonder de mislukte taak gegevensverlies-- en zonder het knooppunt veroorzaakt extra fouten. U kunt bijvoorbeeld taakplanning op het knooppunt [extern aanmelden](#connecting-to-compute-nodes) bekijken van het knooppunt gebeurtenislogboeken of het oplossen van andere problemen uitschakelen. Nadat u klaar bent met uw onderzoek, kunt u vervolgens het knooppunt terug online brengen doordat taakplanning ([REST][rest_online] | [.NET][net_online]), of voert u een van de andere acties die eerder is beschreven.

> [AZURE.IMPORTANT] Met elke actie die wordt beschreven in deze sectie--start opnieuw op, reimage, verwijderen en uitschakelen taakplanning--u zich kunt opgeven hoe taken dat moment actief zijn voor het knooppunt worden afgehandeld wanneer u de actie uitvoeren. Wanneer u het plannen van taken op een knooppunt met behulp van de bibliotheek Batch .NET-client uitschakelt, kunt u bijvoorbeeld een [DisableComputeNodeSchedulingOption] opgeven[ net_offline_option] vaste-tekstwaarde om op te geven of te **beëindigen** actieve taken, **Requeue** ze voor het plannen van op andere knooppunten, of toestaan actieve taken voltooid voordat het uitvoeren van de actie (**TaskCompletion**).

## <a name="next-steps"></a>Volgende stappen

- Een voorbeeld Batch-toepassing stapsgewijze in [aan de slag met de bibliotheek Azure Batch voor .NET](batch-dotnet-get-started.md)doorlopen. Er is ook een [Python versie](batch-python-tutorial.md) van de zelfstudie die een werkbelasting op Linux berekeningscluster knooppunten wordt uitgevoerd.

- Download en maken van de [Batch Explorer] [ github_batchexplorer] steekproef project om dit te gebruiken terwijl u uw Batch-oplossingen ontwikkelen. Met de Batch Explorer, kunt u het volgende en nog veel meer uitvoeren:
  - Controleren en bewerken van toepassingen, taken en taken in uw account Batch
  - Download `stdout.txt`, `stderr.txt`, en andere bestanden van knooppunten
  - Gebruikers maken op knooppunten over en download RDP-bestanden voor externe aanmelding

- Informatie over het [maken van groepen Linux berekeningscluster knooppunten](batch-linux-nodes.md).

- Ga naar het [forum van Azure Batch] [ batch_forum] op MSDN. Het forum is een goede locatie om te vragen, vragen of u alleen leren of expert op het gebruik van de Batch zijn.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
