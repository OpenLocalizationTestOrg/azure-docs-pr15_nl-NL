<properties
    pageTitle="Plannen capaciteit voor het beschermen van virtuele machines en fysieke servers bij het herstellen van Azure Site | Microsoft Azure"
    description="Azure Site herstel coördinaten de herhaling, failover en herstel van virtuele machines en fysieke servers in on-premises naar Azure of naar een secundaire on-premises-site bevinden." 
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
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Capaciteit voor het beschermen van virtuele machines en fysieke servers bij het herstellen van Azure Site plannen

Het hulpprogramma Azure Site herstel capaciteit teamplanner helpt u bij het bepalen uw capaciteitsvereisten voor het beschermen van Hyper-V VMs, VMware VMs en Windows/Linux fysieke servers met Azure sites worden hersteld.


## <a name="overview"></a>Overzicht

Gebruik de Site herstel capaciteit Planner kunt analyseren uw bronomgeving en de werkbelasting en duidelijk bandbreedte behoeften, u op de bronlocatie moet serverbronnen en de resources (virtuele machines en opslag enzovoort), die u nodig hebt in uw doellocatie. 

U kunt het hulpprogramma uitvoeren op een aantal modi:

- **Snelle planning**: uitvoeren het hulpmiddel in deze modus om netwerk- en prognoses op basis van een gemiddelde aantal VMs, schijven, opslag en de snelheid wijzigen.
- **Gedetailleerd planning**: het hulpprogramma uitvoeren in deze modus en geef de details van elke werkbelasting op VM niveau. VM compatibiliteit analyseren en u netwerk- en prognoses.

## <a name="before-you-start"></a>Voordat u begint

Voordat u het hulpprogramma uitvoeren:

1. Verzamel informatie over uw omgeving, inclusief VMs, schijven per VM, opslagruimte per schijf.
2. De snelheid van uw dagelijkse wijzigen (lospeuteren) voor gerepliceerde gegevens identificeren. Dit wilt doen:

    - Als u bent repliceren Hyper-V VMs downloaden van het [hulpmiddel capaciteit Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) om de snelheid wijzigen. [Meer informatie](site-recovery-capacity-planning-for-hyper-v-replication.md) over dit hulpprogramma. Het is raadzaam om dat u dit hulpprogramma gedurende een week om vast te leggen gemiddelden uitvoert.
    - Als u VMware virtuele machines repliceren bent, gebruikt u de [capaciteit van vSphere toestel plannen](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) om te bepalen het tarief weer dat lospeuteren.
    - Als u bezig bent met het repliceren van fysieke servers moet u handmatig schatting.

## <a name="run-the-quick-planner"></a>De snelle Planner uitvoeren
1.  Download en open het hulpprogramma [Azure Site herstel capaciteit teamplanner](http://aka.ms/asr-capacity-planner-excel) . U moet uitvoeren van macro's selecteren om te bewerken en inschakelen van inhoud wanneer u wordt gevraagd. 
2.  Selecteer in het **Selecteer een type teamplanner** **Snelle teamplanner** in de keuzelijst.

    ![Aan de slag](./media/site-recovery-capacity-planner/getting-started.png)

3.  Voer de vereiste informatie in het werkblad **Capaciteit teamplanner** . U moet alle velden met een rode in de onderstaande schermafbeelding cirkel invullen.

    - Kies in het **selecteren van uw scenario** **Hyper-V Azure** of **VMware/fysieke naar Azure**.
    - U verzamelen met het [hulpmiddel capaciteit Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) of de [capaciteit van vSphere toestel planning](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance)in het **gemiddelde dagelijkse gegevens wijzigen percentage (%)** in de gegevens hebt opgeslagen.  
    - **Compressie** geldt alleen voor compressie aangeboden als VMware VMs of fysieke servers worden gerepliceerd naar Azure. We verwachten 30% of meer, maar u kunt de instelling desgewenst wijzigen. U kunt een derde partij toestel zoals Riverbed gebruiken voor het Hyper-V VMs repliceren naar Azure compressie. 
    -  Opgeven hoe lang replica's moeten worden bewaard in **Bewaarbeleid invoeritems** . Als u bezig bent met het repliceren van VMware of fysieke servers de waarde in dagen worden ingevoerd. Als u bezig bent met het repliceren van Hyper-V de tijd opgeven in uren.
    -  **Aantal uren in welke eerste replicatie voor de batch van virtuele machines moet worden voltooid** en het **aantal virtuele machines per eerste replicatie batch** mydomain instellingen die worden gebruikt voor het berekenen van de eerste replicatievereisten.  Wanneer Site herstel wordt geïmplementeerd kan de hele gegevensset aanvankelijke moeten worden geüpload. 

    ![Ingangen](./media/site-recovery-capacity-planner/inputs.png)

2.  Nadat u hebt opgeslagen in de waarden voor de bronomgeving, wordt weergegeven uitvoer bevat:

    - **Bandbreedte nodig is voor delta replicatie** (MB/sec). Netwerkbandbreedte voor delta replicatie wordt berekend van de snelheid gemiddelde dagelijkse gegevens wijzigen.
    - **Bandbreedte vereist voor de eerste replicatie** (MB/sec). Netwerkbandbreedte voor de eerste replicatie wordt berekend van de eerste replicatie-waarden die u hebt opgeslagen in. 
    - **Opslag vereist (in GB)** , wordt de totale Azure opslag vereist.
    - **Totale IO's / s op standaard opslag-accounts** is berekend op basis van 8 kB IO's / s eenheidsgrootte op de totale opslagcapaciteit van de standaard-accounts.  Voor de snelle Planner die het getal wordt berekend op basis van de bron VMs schijven en dagelijkse gegevens wijzigen rente. Voor de gedetailleerde Planner die het getal wordt berekend op basis van het totale aantal VMs die zijn toegewezen aan de standaard Azure VMs en gegevens wijzigen rente op deze VMs. 
    - **Aantal standaard opslag accounts** biedt het totale aantal standaard opslag accounts die nodig zijn voor de VMs beveiligen. Houd er rekening mee dat een standaard-opslag-account tot 20.000 IO's / s over alle VMs in een standaard-opslag houdt kunt en maximaal 500 IO's / s ondersteund per schijf. 
    - **Aantal blob schijven vereist** geeft het aantal schijven die worden gemaakt op Azure opslag.
    - **Aantal premium opslag accounts vereist** , vindt u het totale aantal premium opslag accounts die nodig zijn voor de VMs beveiligen. Houd er rekening mee dat een bron VM met hoge IO-'s (groter dan 20000) / s een premium-opslag-account moet. Een account van de opslagruimte premium kan maximaal 80000 IO's / s bevatten.
    - **Totale IO's / s op premium opslag** is berekend op basis van 256 K IO's / s eenheidsgrootte op de totale premium opslag-accounts.  Voor de snelle Planner die het getal wordt berekend op basis van de bron VMs schijven en dagelijkse gegevens wijzigen rente. Voor de gedetailleerde Planner die het getal wordt berekend op basis van het totale aantal VMs die zijn toegewezen aan premium Azure VM (Active Directory en GS reeksenas) en de gegevens wijzigen rente op deze VMs. 
    - **Aantal configuratieservers vereist** ziet u hoeveel configuratieservers zijn vereist voor de implementatie (1)
    - **Aantal aanvullende CMYK-servers vereist** , ziet u of er aanvullende CMYK-servers nodig naast de processerver dat geconfigureerd op de configuratieserver al dan niet standaard zijn.
    - **100% extra opslagruimte op de bron** wordt weergegeven of extra opslagruimte is vereist in de bronlocatie.
            
    ![Uitvoer](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>De gedetailleerde Planner uitvoeren


1.  Download en open het hulpprogramma [Azure Site herstel capaciteit teamplanner](http://aka.ms/asr-capacity-planner-excel) . U moet uitvoeren van macro's selecteren om te bewerken en inschakelen van inhoud wanneer u wordt gevraagd. 
2.  Selecteer in het **Selecteer een type teamplanner** **Gedetailleerde teamplanner** in de keuzelijst.

    ![Aan de slag](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  Voer de vereiste informatie in het werkblad **Werkbelasting voor hoger onderwijs** . U moet de gemarkeerde velden invullen.

    - Geef het totale aantal cores op een bronserver in **processorcores** .
    - Geef de RAM grootte van een bronserver in **geheugentoewijzing in MB** . 
    - Het **Getal van NIC** Geef het aantal netwerkadapters op een bronserver. 
    -  Geef de totale grootte van de VM-opslag in de **totale opslagcapaciteit (in GB)** . Bijvoorbeeld als de bronserver heeft 3 schijven met 500 GB, de grootte van de totale opslagcapaciteit is 1500 GB.
    -  Geef het totale aantal schijven van een bronserver in **aantal schijven die zijn gekoppeld** .
    -  Geef het gemiddeld gebruik in **gebruik van de capaciteit Schijfopruiming** .
    -  Geef dat de dagelijkse gegevens tarief weer dat een bronserver wijzigen in **dagelijks wijzigen percentage (%)** .
    -  Grootte van de **Toewijzing Azure** Voer hetzij Azure VM grootte die u wilt toewijzen. Klik op de**IaaS VMs berekenen**als u niet wilt u dit wilt doen handmatig. Houd er rekening mee dat als u een handmatige instelling invoer en klik vervolgens op berekenen IaaS VMs uw handmatige instelling mogelijk worden overschreven omdat het proces berekeningscluster automatisch geeft aan het meest geschikt is van de grootte van de Azure VM wat.

    ![Werkbelasting voor hoger onderwijs](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Als u klikt op luidt acties **IaaS VMs berekenen** hier als volgt:

    - De invoer verplicht is gevalideerd.
    - Berekent de IO's / s en stappen worden voorgesteld de beste Azure VM aize vergelijken voor elke VMs dat daarvoor in aanmerking komt voor replicatie naar Azure. Als de juiste grootte van Azure VM kan niet worden gedetecteerd dat een fout is uitgegeven. Bijvoorbeeld als het aantal bijgevoegde in 65 een fout schijven is problemen sinds de hoogste grootte is Azure VM 64.
    - Stappen worden voorgesteld een opslag-account dat kan worden gebruikt voor een VM Azure.
    - Berekent het totale aantal premium opslag rekeningen is vereist voor de werkbelasting en standaard opslag. Schuif omlaag aan de rechterkant type Azure opslag en het opslag-account dat kan worden gebruikt voor een bronserver weergeven
    - Is voltooid en de rest van de tabel op basis van de vereiste opslagtype (standaard of premium) toegewezen voor een VM en het aantal schijven gekoppeld gesorteerd. Dit rapport toont Ja gedurende alle VMs die voldoen aan de vereisten voor een reservekopie op Azure, kolom A (Is VM gekwalificeerd?). Als een VM kan geen back-maximaal wordt Azure een fout weergegeven.

Kolommen AA naar AE worden uitgevoerd en geef de informatie voor elke VM.

![Werkbelasting voor hoger onderwijs](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Voorbeeld
Als u bijvoorbeeld voor zes VMs met de waarden uit de tabel, het hulpmiddel en overige planningsfactoren het meest geschikt is Azure VM en de Azure opslagvereisten.

![Werkbelasting voor hoger onderwijs](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- Houd rekening met het volgende in de uitvoer voor het voorbeeld:
    
    - De eerste kolom is een kolom validatie voor de VMs, schijven en lospeuteren.
    - Twee standaard opslag accounts en één premium opslag-account nodig hebt voor vijf VMs. 
    -  VM3 niet in aanmerking voor de beveiliging omdat een of meer schijven meer dan 1 TB zijn.
    -  VM1 en VM2 kunt het eerste account is standaard opslag gebruiken
    -  VM4 kunt het tweede standaard opslag-account gebruiken.
    -  VM5 en VM6 een premium-opslag-account nodig en gebruiken allebei één account.

    >[AZURE.NOTE]  IO's / s op standard en premium opslag worden berekend op het niveau VM en niet op schijf niveau. Een standaard virtuele machine kan maximaal 500 IO's / s per schijf verwerken. Als IO's / s voor een schijf groter dan 500 zijn moet u premium-opslag. Echter als IO's / s voor een schijf groter is dan 500 zijn, maar IO's / s voor de totale VM schijven binnen de limieten voor ondersteuning Azure VM standaard (VM grootte, het aantal schijven, het aantal netwerkadapters, CPU, geheugen) klikt u vervolgens de planner maken hervat een standaard VM en niet de DS of GS reeks. U moet de toewijzing Azure grootte cel handmatig bijwerken met de juiste DS of GS reeks VM.

5. Nadat u alle gegevens hebt ingevoerd op hun plaats staan, klikt u op **gegevens verzenden naar de knop teamplanner** om te openen van de **Capaciteit teamplanner** werkbelasting om weer te geven of ze in aanmerking komen voor beveiliging of niet zit zijn gemarkeerd.


### <a name="submit-data-in-the-capacity-planner"></a>Gegevens in de planning capaciteit indienen

1.  Bij het openen van het werkblad **Capaciteit teamplanner** wordt deze gevuld op basis van de instellingen die u hebt opgegeven. Het woord 'Werkbelasting' wordt weergegeven in de cel **Infra invoeritems bron** om weer te geven dat de invoer **Werkbelasting voor hoger onderwijs** -werkblad. 
2.  Als u wijzigen wilt, u moet het werkblad **Werkbelasting voor hoger onderwijs** wijzigen en klikt u nogmaals op de gegevens verzenden naar de knop teamplanner.  

    ![Capaciteit teamplanner](./media/site-recovery-capacity-planner/capacity-planner.png)


