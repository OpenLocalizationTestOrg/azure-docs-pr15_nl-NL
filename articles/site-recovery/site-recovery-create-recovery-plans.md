<properties
    pageTitle="Herstel abonnementen maken | Microsoft Azure" 
    description="Maak herstel abonnementen met Azure Site herstel mislukken en herstellen van groepen virtuele machines en fysieke servers." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Herstel abonnementen maken

De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md).


## <a name="overview"></a>Overzicht

In dit artikel vindt u informatie over het maken en aanpassen van herstel-abonnementen. 

Herstel abonnementen bestaan uit een of meer geordende groepen met virtuele machines of fysieke servers die u samenwerken wilt via mislukt. Herstel abonnementen volgt als:

- Groepen van machines die mislukken en klik vervolgens bij elkaar opstart definiëren.
- Model afhankelijkheden tussen computers door ze te groeperen in een groep herstel-abonnement. Voor voorbeeld als u wilt mislukken en weer een specifieke toepassing u zou de virtuele machines voor die toepassing in dezelfde herstel abonnement groep groep.
- Automatiseren en failover uitbreiden. U kunt een test, gepland, of niet-geplande failover uitvoeren op een herstel-abonnement. U kunt herstel abonnementen met scripts, Azure automatisering en handmatige acties aanpassen.

Herstel abonnementen worden weergegeven op de **Herstel abonnementen** in de portal-Site herstellen.


Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="before-you-start"></a>Voordat u begint

Houd rekening met het volgende:

- Een plan voor herstel niet mag VMs combineren met enkele of meerdere netwerkadapters. Dit is omdat deze VMs mengen niet is toegestaan in een Azure-cloudservice.
- U kunt herstel abonnementen met scripts en handmatige acties uitbreiden. Houd rekening met het volgende:
    - Het schrijven van scripts met Windows PowerShell.
    - Zorg ervoor dat scripts proberen variabel blokken gebruiken zodat de uitzonderingen zonder problemen worden afgehandeld. Als er een uitzondering in het script deze loopt vast en de taak wordt weergegeven als mislukt.  Als een fout optreedt, worden alle resterende deel van het script niet uitgevoerd. Als dit gebeurt wanneer u een niet-geplande failover worden uitgevoerd, blijft het abonnement herstel. Als dit gebeurt wanneer u een geplande failover uitvoert, wordt het abonnement herstelproces is gestopt. Als dit gebeurt, het script oplossen, zorg ervoor dat deze wordt uitgevoerd zoals verwacht en voer het herstelproces-abonnement opnieuw.
    - De opdracht Write-Host werkt niet in een script herstel-abonnement en het script, mislukt. Als u maken van uitvoer wilt, een proxyscript maken waarmee uw belangrijkste script op zijn beurt wordt uitgevoerd en zorg ervoor dat alle uitvoer is omgeleide via de >> opdracht.
    - Het script treedt er een time-out als dit niet binnen 600 seconden.
    - Als er is geschreven naar STDERR, wordt het script worden geclassificeerd als mislukt. Deze informatie wordt weergegeven in de details van script worden uitgevoerd.
    - Houd rekening met het volgende als u VMM in uw implementatie gebruikt:

        - Scripts in een plan voor herstel worden uitgevoerd in de context van de VMM serviceaccount. Zorg ervoor dat dit account leesmachtigingen heeft voor de externe share waarop het script zich bevindt en het script uitvoeren op het niveau VMM service account bevoegdheidsniveau testen.
        - VMM cmdlets worden bezorgd in een Windows PowerShell-module. De VMM Windows PowerShell-module is geïnstalleerd tijdens de installatie van de VMM-console. De module VMM in uw script met de volgende opdracht in het script kan worden geladen: Import-Module-naam virtualmachinemanager. [Meer informatie](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Controleer of dat u ten minste één bibliotheek server hebt in uw VMM-implementatie. Het pad van de bibliotheek delen voor een server VMM bevindt zich standaard lokaal op de server VMM met de naam van de map MSCVMMLibrary.
        - Als uw bibliotheek delen pad externe (of lokale maar niet met MSCVMMLibrary gedeeld, de optie voor delen als volgt configureren (met \\libserver2.contoso.com\share\ als voorbeeld):
            - Open de Register-Editor en navigeer naar HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration.
            -  De waarde ScriptLibraryPath bewerken en plaats de waarde als \\libserver2.contoso.com\share\. opgeven van de volledige FQDN-naam. Geef machtigingen voor de gedeelde locatie.
            -  Zorg ervoor dat u het script met een gebruikersaccount met dezelfde machtigingen als de serviceaccount VMM om ervoor te zorgen dat zelfstandige geteste scripts worden uitgevoerd op dezelfde manier dat u geen in herstel-pakketten testen. Stel op de server VMM het beleid kan worden uitgevoerd, worden omzeild als volgt:
                -  Open de 64-bits Windows PowerShell-console met verhoogde bevoegdheden.
                -  Type: **Set-uitvoeringsbeleid kan overslaan**. [Meer informatie](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Maak een herstelplan

De manier waarop waarin u een abonnement herstel maken, is afhankelijk van uw Site herstel-implementatie.

- **Repliceren VMware VMs en fysieke servers**, u een plan opstellen en replicatiegroepen met virtuele machines en fysieke servers naar een abonnement herstel toevoegen.
- **Repliceren Hyper-V VMs (in VMM wolken)**, u een plan opstellen en beveiligde Hyper-V virtuele machines vanuit een cloud VMM toevoegen aan een herstel-abonnement.
- **Repliceren Hyper-V VMs (niet in VMM wolken)**, een plan opstellen en beveiligde Hyper-V virtuele machines uit een groep beveiliging toevoegen aan een herstel-abonnement.
- **SAN herhaling**, een planning maken en toevoegen van een groep herhaling met virtuele machines naar het abonnement herstel. U selecteert u een groep herhaling in plaats van specifieke virtuele machines omdat alle virtuele machines in een groep herhaling samen moet mislukken (storing op de opslaglaag eerst).


Hiermee maakt u een abonnement herstel als volgt:

1. Klik op tabblad **Herstel van abonnement** > **Herstel plannen maken**.
Geef een naam voor het abonnement waarop herstel, en een bronsite en doelsites. De bronserver moet virtuele machines die zijn ingeschakeld voor failover- en herstelbestanden hebben.

    - Als u bent VMM gerepliceerd naar VMM Selecteer **Type bron** > **VMM**en de bronsite en doelsites VMM-servers. Klik op **Hyper-V** als u wilt zien van wolken die zijn geconfigureerd voor gebruik van Hyper-V Replica. 
    - Als u bent VMM gerepliceerd naar VMM SAN select **Brontype**met > **VMM**en de bronsite en doelsites VMM-servers. Klik op **SAN** als u wilt zien van wolken die zijn geconfigureerd voor SAN herhaling.
    - Als u bent VMM gerepliceerd naar Azure Selecteer **Type bron** > **VMM**.  Selecteer de bronserver VMM en **Azure** als het doel.
    - Als u vanaf een site Hyper-V repliceren bent Selecteer **Type bron** > **Hyper-V-site**. Selecteer de site als de bron- en **Azure **als het doel.
    - Als u bent VMware of een fysieke on-premises-server gerepliceerd naar Azure, selecteert u een configuratieserver als de bron- en **Azure** als doel

2. Selecteer de virtuele machines (of groep voor replicatie) die u wilt toevoegen aan de standaardgroep (groep 1) in het abonnement herstel in **virtuele machines selecteren** .

## <a name="customize-recovery-plans"></a>Herstel abonnementen aanpassen

Nadat u hebt toegevoegd beveiligd virtuele machines of herhaling groepen aan de standaardgroep voor het plannen van herstel en gemaakt van het abonnement dat kunt u deze aanpassen:

- **Nieuwe groepen toevoegen**, kunt u aanvullende herstel abonnement groepen toevoegen. Groepen die u toevoegt, worden genummerd in de volgorde waarin u deze toevoegen. U kunt maximaal zeven groepen toevoegen. U kunt meer computers of herhaling groepen toevoegen aan deze nieuwe groepen. Houd er rekening mee dat een virtuele machine of replicatiegroep, kan alleen worden opgenomen in één herstel abonnement groep.
- **Een script toevoegen **, kunt u scripts die voor of na een herstel plannen van de groep toevoegen. Wanneer u een script toevoegen wordt een nieuwe set acties voor de groep toegevoegd. Bijvoorbeeld een reeks oude stappen voor 1 van de groep wordt gemaakt met de naam: groep 1: oude stappen. Alle stappen van de oude worden, vermeld in deze waarde. Houd er rekening mee dat u alleen een script op de primaire-site toevoegen kunt als u een VMM server geïmplementeerd.
- **Een handmatige actie toevoegen**, kunt u handmatig acties laten voor of na een herstel abonnement groep uitvoeren toevoegen. Wanneer het abonnement herstel wordt uitgevoerd, wordt op het punt waar u de handmatige actie ingevoegd stopgezet en een dialoogvenster wordt gevraagd om op te geven dat de handmatige actie is voltooid.

## <a name="extend-recovery-plans-with-scripts"></a>Herstel abonnementen met scripts uitbreiden

U kunt een script toevoegen aan uw abonnement herstel:

- Als u een bronsite VMM u kunt ook een script op de server VMM maken en deze opnemen in uw herstel-abonnement hebt.
- Als u naar Azure repliceren bent kunt u Azure automatisering runbooks integreren in uw abonnement herstel

### <a name="create-a-vmm-script"></a>Een script VMM maken


Als volgt te werk om het script te maken:

1. Een nieuwe map maken in de bibliotheek delen, bijvoorbeeld \<VMMServerName > \MSSCVMMLibrary\RPScripts. Plaats deze op de bron en afstemmen VMM-servers.
2. Maak het script (bijvoorbeeld RPScript) en controleer dat deze werkt zoals verwacht.
3. Het script in de locatie te plaatsen \<VMMServerName > \MSSCVMMLibrary op de bronsite en doelsites VMM-servers.

### <a name="create-an-azure-automation-runbook"></a>Een runbook Azure automatisering maken

U kunt uw abonnement herstel uitbreiden door een runbook Azure automatisering uit te voeren als onderdeel van het abonnement. [Meer informatie](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Aangepaste instellingen toevoegen aan een herstel-abonnement

1. Open het herstelproces is abonnement waarop dat u wilt aanpassen.
2. Klik om toe te voegen een virtuele machines of nieuwe groep.
3. Een script of handmatig toevoegen actie klikt u op een item in de lijst van **stap** en klik vervolgens op **Script** of **Handmatige actie**. Opgeven of het script of de actie voor of na het geselecteerde item wilt toevoegen. Gebruik de knoppen **Omhoog** en **Omlaag** naar de positie van het script omhoog of omlaag verplaatsen.
4. Als u een script VMM toevoegt, selecteert u **Failover naar VMM script**en typ het relatieve pad naar de optie voor delen in **Scriptpad** in. Dat het geval is, in ons voorbeeld waarin de optie voor delen zich bevindt op \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, het pad opgeven: \RPScripts\RPScript.PS1.
5. Als u een Azure automatisering boek uitvoeren toevoegt, geef het **Azure automatisering Account** waarin het runbook zich bevindt en selecteert u het juiste **Azure Runbook Script**.
5. Voer een overname van het abonnement herstellen om ervoor te zorgen dat het script werkt zoals verwacht.


## <a name="next-steps"></a>Volgende stappen

U kunt verschillende soorten failovers herstel abonnement, inclusief een failover testen om te controleren van uw omgeving, en of niet gepland failovers uitvoeren. [Meer informatie](site-recovery-failover.md).


 
