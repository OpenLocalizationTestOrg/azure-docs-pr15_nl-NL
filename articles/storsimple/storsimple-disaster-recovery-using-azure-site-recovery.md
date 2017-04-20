<properties 
   pageTitle="DR voor bestandsshares op StorSimple met Azure Site herstel automatiseren | Microsoft Azure"
   description="Beschrijft de stappen en aanbevolen procedures voor het maken van een oplossing voor het herstellen van noodgevallen voor bestandsshares die worden gehost op StorSimple opslag."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Geautomatiseerde herstel-oplossing met behulp van Azure Site herstel voor bestandsshares die worden gehost op StorSimple

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple is een hybride cloud opslagoplossing die voldoet aan de complexiteit van ongestructureerde gegevens meestal dat is gekoppeld aan bestandsshares. StorSimple gebruikt cloudopslag als een uitbreiding van de on-premises implementatie-oplossing en automatisch op lagen gegevens over de opslag van de on-premises implementatie en cloudopslag. Gegevensbescherming, geïntegreerd met lokale, en cloud momentopnamen: een weids opslaginfrastructuur hoeven.

[Herstel van Azure-Site](../site-recovery/site-recovery-overview.md) is een op basis van Azure-service waarmee noodgevallen herstelmogelijkheden (DR) door orchestrating herhaling, failover en herstel van virtuele machines. Azure Site herstel ondersteunt een aantal herhaling technologieën consistente repliceren, beveiligen en naadloos samen via virtuele machines en toepassingen privé/openbaar of gehoste wolken is mislukt.

Azure sites worden hersteld, VM herhaling en StorSimple cloud momentopname mogelijkheden gebruikt, kunt u de volledige bestand server-omgeving beveiligen. In het geval van een onderbreking, kunt u één muisklik om uw bestandsshares online in Azure wordt aangegeven in slechts een paar minuten weer te geven.

In dit document wordt gedetailleerd uitgelegd hoe u kunt een noodgevallen hersteloplossing voor uw gedeelde bestanden die worden gehost op StorSimple opslag, maken en uitvoeren geplande, niet-geplande en failovers met een indeling met één klik herstel testen. In wezen wordt weergegeven hoe kunt u het Plan voor herstel in uw kluis Azure Site herstellen om StorSimple failovers tijdens het scenario's voor noodgevallen. Bovendien worden deze ondersteunde configuraties en vereisten. In dit document wordt ervan uitgegaan dat u bekend met de basisprincipes van Azure Site herstel en StorSimple architecturen bent.

## <a name="supported-azure-site-recovery-deployment-options"></a>Ondersteunde Azure Site implementatie herstelopties

Klanten kunnen bestandsservers te implementeren als fysieke servers of virtuele machines (VMs) waarop Hyper-V of VMware, en maak bestandsshares uit oppervlaktevariaties geen opslagruimte meer StorSimple. Azure Site herstel kunt fysieke en virtuele implementaties naar een secundaire site of naar Azure beveiligen. In dit document behandelt details van een DR-oplossing met Azure als de site herstel voor een bestandsserver VM die worden gehost op Hyper-V en bestandsshares op StorSimple opslag. Andere scenario's waarin de bestandsserver VM zich op een VM VMware of een fysieke machine bevindt kunnen ook worden geïmplementeerd.

## <a name="prerequisites"></a>Vereisten voor

Uitvoering van een oplossing voor het herstel van één muisklik noodgevallen die gebruikmaakt van Azure Site herstel voor bestandsshares die worden gehost op StorSimple opslag heeft de volgende vereisten:

-   On-premises implementatie Windows Server 2012 R2 bestand server VM die worden gehost op Hyper-V of VMware of een fysieke machine

-   StorSimple opslag apparaat on-premises implementatie is geregistreerd bij Azure StorSimple manager

-   StorSimple Cloud toestel die is gemaakt in de Azure StorSimple manager (dit kan worden onderhouden afsluiten)

-   Bestandsshares die worden gehost op de hoeveelheden die is geconfigureerd op de StorSimple opslag-apparaat

-   [Azure Site herstel services kluis](../site-recovery/site-recovery-vmm-to-vmm.md) die is gemaakt in een Microsoft Azure-abonnement

Bovendien als Azure uw site herstel is, voert u het [hulpprogramma Azure virtuele machines gereedheid Assessment](http://azure.microsoft.com/downloads/vm-readiness-assessment/) op VMs om ervoor te zorgen dat deze compatibel met Azure VMs en Azure Site herstelbestanden services zijn.

Om te voorkomen latentie problemen (dit kunnen leiden tot hogere kosten), zorg ervoor dat u maakt de StorSimple Cloud toestel, automatisering account en opslag (s) in dezelfde regio.

## <a name="enable-dr-for-storsimple-file-shares"></a>DR voor bestandsshares StorSimple inschakelen  

Elk onderdeel van de on-premises omgeving moet worden beveiligd als replicatie is voltooid en herstel wilt inschakelen. Deze sectie wordt beschreven hoe u:

-   Active Directory en DNS herhaling (optioneel) instellen

-   Herstel van Azure-Site gebruiken om te schakelen van beveiliging van de bestandsserver VM

-   Beveiliging van StorSimple hoeveelheden inschakelen

-   Het netwerk configureren

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Active Directory en DNS herhaling (optioneel) instellen

Als u beveiligen met de computers met Active Directory en DNS wilt, zodat ze beschikbaar op de DR-site zijn, moet u expliciet om ze te beveiligen (zodat het bestandsservers na fail via met verificatie toegankelijk zijn). Er zijn twee aanbevolen opties op basis van de complexiteit van een van de klant on-premises omgeving.

#### <a name="option-1"></a>Optie 1

Als de klant een klein aantal toepassingen heeft, één domeincontroller voor de gehele on-premises site, en over de hele site, wordt worden verbroken en vervolgens het beste Azure Site herstel herhaling gebruiken voor het domeincontroller machine repliceren naar een secundaire site (dit is van toepassing voor zowel site-naar-site en site-naar-Azure).

#### <a name="option-2"></a>Optie 2

Als de klant heeft een groot aantal toepassingen, een Active Directory-bos wordt uitgevoerd en worden worden verbroken via een paar toepassingen tegelijk en klikt u wordt aangeraden het instellen van een extra domeincontroller op de site DR (een secundaire site of in Azure).

Raadpleeg [automatisch DR-oplossing voor Active Directory en DNS met Azure Site herstel](../site-recovery/site-recovery-active-directory.md) voor instructies bij het maken van een domeincontroller beschikbaar op de DR-site. Voor de rest van dit document, wordt ervan uitgegaan dat een domeincontroller is beschikbaar op de DR-site.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Herstel van Azure-Site gebruiken om te schakelen van beveiliging van de bestandsserver VM

Deze stap is vereist dat u voorbereiden op de server-omgeving van de on-premises implementatie-bestand, maken en een kluis Azure Site herstel voorbereiden en bestandsbeveiliging van VM inschakelen.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Voor het voorbereiden van de on-premises implementatie bestand server-omgeving

1.  Stel het **Gebruikersaccountbeheer** op **nooit waarschuwen**. Dit is vereist zodat u Azure automatische scripts gebruiken kunt om verbinding maken met de iSCSI-doelen na fail door Azure sites worden hersteld.

    1.  Druk op de Windows-toets + Q en zoeken voor **Gebruikersaccountbeheer**.

    2.  Selecteer **Instellingen voor Gebruikersaccountbeheer wijzigen**.

    3.  Sleep de balk naar de onderkant richting **Nooit waarschuwen**.

    4.  Klik op **OK** en selecteer vervolgens **Ja** wanneer u wordt gevraagd.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Installeer de VM-Agent op elk van de bestandsserver VMs. Dit is vereist zodat u Azure automatische scripts op de mislukte via VMs uitvoeren kunt.

    1.  [Download de agent](http://aka.ms/vmagentwin) naar `C:\\Users\\<username>\\Downloads`.

    2.  Open Windows PowerShell in Administrator-modus (als Administrator uitvoeren) en voer de volgende opdracht uit om te navigeren naar de downloadlocatie:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] De bestandsnaam kan afwijken, afhankelijk van de versie.

1.  Klik op **volgende**.

2.  Accepteer de **Bepalingen van de overeenkomst** en klik vervolgens op **volgende**.

3.  Klik op **Voltooien**.


1.  Gedeelde bestanden via een volume oppervlaktevariaties geen opslagruimte meer StorSimple maken. Zie [gebruiken de StorSimple Manager-service voor het beheren van volumes](storsimple-manage-volumes.md)voor meer informatie.

    1.  Druk op de Windows-toets + Q en zoeken voor **iSCSI**op uw VMs on-premises implementatie.

    2.  Selecteer **iSCSI-initiator**.

    3.  Selecteer het tabblad **configuratie** en de initiatornaam kopiëren.

    4.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/).

    5.  Selecteer het tabblad **StorSimple** en selecteer vervolgens de StorSimple Manager-Service die het apparaat fysiek bevat.

    6.  Volume container(s) maken en dan volume (s). (Deze volumes zijn voor het bestand share(s) op de server VMs). De initiatornaam kopiëren en geef een naam voor de Access-besturingselement-Records bij het maken van de hoeveelheden.

    7.  Selecteer het tabblad **configureren** en noteer het IP-adres van het apparaat.

    8.  Klik op uw VMs on-premises implementatie, gaat u naar de **iSCSI-initiator** opnieuw en geef het IP-in de sectie snel verbinding maken. Klik op **Snel verbinding maken** (het apparaat moet nu zijn verbonden).

    9.  Open de Portal van Azure Management en selecteer het tabblad **hoeveelheden en apparaten** . Klik op **automatisch configureren**. Het volume dat u zojuist hebt gemaakt, moet worden weergegeven.

    10. Klik in de portal selecteert u het tabblad **apparaten** en selecteer vervolgens **maken van een nieuwe virtueelapparaat.** (Dit virtuele apparaat wordt gebruikt als een storing). Deze nieuwe virtuele apparaat kan worden bewaard in een offline status voor extra kosten voorkomen. Het virtuele apparaat offline te zetten, gaat u naar de sectie **virtuele Machines** op de Portal en afsluiten.

    11. Ga terug naar de on-premises implementatie VMs en schijf Management openen (druk op de Windows-toets + X en selecteer **Schijfopruiming Management**).

    12. Ziet u enkele extra schijven (afhankelijk van het aantal volume die u hebt gemaakt). Met de rechtermuisknop op de eerste fase, selecteer **Geïnitialiseerd schijf**en selecteer **OK**. Met de rechtermuisknop op de sectie **Unallocated** , selecteert u **Nieuw eenvoudig Volume**en hieraan een letter voltooien van de wizard.

    13. Herhaal stap l voor alle schijven. U kunt nu alle schijven zien op **Deze PC** in Windows Verkenner.

    14. Met de rol bestands- en Storage-Services te maken van bestandsshares op deze volumes.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Maken en een kluis Azure Site herstel voorbereiden

Raadpleeg de [Azure Site herstel documentatie](../site-recovery/site-recovery-hyper-v-site-to-azure.md) aan de slag met Azure Site herstel voordat de bestandsserver VM is beveiligd.

#### <a name="to-enable-protection"></a>Beveiliging inschakelen

1.  De target(s) iSCSI verbreken in de on-premises implementatie VMs die u wilt beveiligen via Azure Site herstellen:

    1.  Druk op Windows-toets + Q en zoek naar **iSCSI**.

    2.  Selecteer **iSCSI-initiator instellen**.

    3.  Het StorSimple apparaat dat u eerder verbinding verbreken U kunt ook de bestandsserver uitschakelen voor een paar minuten bij het inschakelen van beveiliging.

    > [AZURE.NOTE] Hierdoor wordt de bestandsshares tijdelijk niet beschikbaar

1.  [VM beveiliging inschakelen](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) van de bestandsserver VM vanaf de portal Azure sites worden hersteld.

2.  Wanneer de eerste synchronisatie begint, kunt u het doel opnieuw. Ga naar het beginpunt iSCSI, selecteer het apparaat StorSimple en op **verbinding maken**.

3.  Wanneer de synchronisatie voltooid is en de status van de VM **beveiligd is**, selecteert u de VM, selecteer het tabblad **configureren** en het netwerk van VM dienovereenkomstig bijwerken (dit is het netwerk die de mislukte via VM(s) een deel van is). Als het netwerk niet wordt weergegeven, betekent dit dat de synchronisatie nog steeds aan de hand is.

### <a name="enable-protection-of-storsimple-volumes"></a>Beveiliging van StorSimple hoeveelheden inschakelen

Als u de optie **een standaard back-up voor dit volume inschakelen** voor de hoeveelheden StorSimple niet hebt geselecteerd, gaat u naar de **Back-beleid** in de service StorSimple Manager en geschikt back-beleid voor alle de hoeveelheden maken. Het is raadzaam dat u de frequentie van back-ups instellen op de herstel punt doelstelling (vrijgegeven Productieorder) die u wilt zien voor de toepassing.

### <a name="configure-the-network"></a>Het netwerk configureren

Voor de bestandsserver VM netwerkinstellingen bij het herstellen van Azure Site zodanig configureren dat de netwerken VM zijn gekoppeld aan het juiste DR-netwerk na failover.

Zoals weergegeven in de volgende afbeelding, kunt u de VM selecteren in de **Cloud VMM** of de **Groep beveiliging** om de netwerkinstellingen te configureren.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Maak een herstelplan

U kunt een plan voor herstel maken in ASR om het failoverproces van de bestandsshares te automatiseren. Als een onderbreking optreedt, kunt u de bestandsshares omhoog overbrengen in een paar minuten met één klik. Als u wilt deze automatisering inschakelen, moet u een Azure automatisering-account.

#### <a name="to-create-the-account"></a>Het account maken

1.  Ga naar de Azure klassieke portal en Ga naar de sectie **automatisering** .

1.  Maak een nieuwe automatisering-account. Houd het in het hetzelfde geografische gebied waarin de StorSimple Cloud toestel en opslag-accounts zijn gemaakt.

2.  Klik op **Nieuw** &gt; **App Services** &gt; **automatisering** &gt; **Runbook** &gt; **Uit galerie** alle vereiste runbooks importeren in de automatisering-account.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  De volgende runbooks uit het deelvenster **Herstel** in de galerie toevoegen:

    -   Mislukken StorSimple volume containers

    -   Opschonen van StorSimple hoeveelheden na Test Failover (TFO)

    -   Volumes op StorSimple apparaat na failover koppelen

    -   Virtuele toestel StorSimple starten

    -   Aangepast script extensie in Azure VM verwijderen

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Alle scripts door het runbook selecteren in het account dat automatisering en gaat u naar het tabblad van de **auteur** publiceren. Na deze stap weergegeven het tabblad **Runbooks** als volgt:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  In het account dat automatisering Ga naar het tabblad **activa** , klikt u op **Toevoegen instelling** &gt; **Referentie toevoegen**, en uw Azure referenties toevoegen – name van de activa AzureCredential.

    Gebruik de Windows PowerShell-referentie. Dit moet een referentie met een organisatie-ID-gebruikersnaam en wachtwoord met toegang tot dit Azure abonnement en meervoudige verificatie is uitgeschakeld. Dit is vereist om te verifiëren namens de gebruiker tijdens de failovers en om het volume van de server bestand op de site DR weer te geven.

1.  In het account automatisering, selecteer het tabblad **activa** en klik vervolgens op **Toevoegen instelling** &gt; **variabele toevoegen** en de volgende variabelen toevoegen. U kunt deze activa versleutelen. Deze variabelen zijn herstel abonnement-specifieke. Als uw herstel van plan bent (dat u wilt maken in de volgende stap) naam TestPlan en dan uw variabelen moet TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName, enzovoort.

    -   *RecoveryPlanName* **-StorSimRegKey**: de registratie-toets voor de service StorSimple Manager.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: de naam van de Azure abonnement.

    -   *RecoveryPlanName* **-Bronnaam**: de naam van de resource StorSimple die met het apparaat dat StorSimple.

    -   *RecoveryPlanName* **-Apparaatnaam**: het apparaat waarop moet worden overgenomen.

    -   *RecoveryPlanName* **-TargetDeviceName**: het StorSimple Cloud toestel waarop de containers zijn moet worden overgenomen.

    -   *RecoveryPlanName* **-VolumeContainers**: een tekenreeks met door komma's gescheiden van volume containers aanwezig is op het apparaat die moeten worden overgenomen; bijvoorbeeld volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: de naam van de service van het doelapparaat (dit kunt u vinden in de sectie **VM** : de servicenaam van de is hetzelfde als de DNS-naam).

    -   *RecoveryPlanName* **-StorageAccountName**: de opslagaccountnaam waarin het script (deze moet worden uitgevoerd op de mislukte via VM) wordt opgeslagen. Dit is een opslag-account met enkele spatie voor het tijdelijk opslaan van het script.

    -   *RecoveryPlanName* **-StorageAccountKey**: de access-toets voor de bovenstaande opslag-account.

    -   *RecoveryPlanName* **-ScriptContainer**: de naam van de container waarin het script wordt opgeslagen in de cloud. Als de container niet bestaat, kunt u het wordt gemaakt.

    -   *RecoveryPlanName* **-VMGUIDS**: na het beveiligen van een VM Azure Site herstel elke VM een unieke ID toegewezen waarmee de details van de mislukte via VM. Als u de VMGUID, selecteer het tabblad **Herstel Services** en klik vervolgens op **Beveiligde Item** &gt; **Beveiliging groepen** &gt; **Machines** &gt; **Eigenschappen**. Als u meerdere VMs hebt, moet u vervolgens de GUID's toevoegen als een tekenreeks met door komma's gescheiden.

    -   *RecoveryPlanName* **-AutomationAccountName** – de naam van de automatisering account waarin u de runbooks en de activa hebt toegevoegd.

    Bijvoorbeeld als de naam van het abonnement herstel fileServerpredayRP is, klikt u vervolgens het tabblad **activa** moet worden weergegeven als volgt nadat u alle activa hebt toegevoegd.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Ga naar het gedeelte **Herstel Services** en selecteer de Azure Site herstel kluis die u eerder hebt gemaakt.

2.  Selecteer het tabblad **Herstel van abonnement** en maakt u een nieuw abonnement voor herstel als volgt:

    een.  Geef een naam en selecteer de gewenste **Groep beveiliging**.

    b.  Selecteer de VMs in de groep beveiliging die u wilt opnemen in het herstelproces-abonnement.

    c.  Nadat het herstelproces abonnement is gemaakt, selecteert u deze om de herstel abonnement aanpassing weergave te openen.

    d.  Selecteer **alle groepen afsluiten**, klikt u op **Script**en klikt u op **toevoegen een script op de primaire voordat alle groep afsluiten**.

    e.  Selecteer het account dat automatisering (u toegevoegd waarin de runbooks) en selecteer vervolgens het runbook **over-StorSimple-Volume-Containers mislukt** .

    f.  Klik op **groep 1: beginnen**, kiest u **virtuele Machines**en de VMs die moeten worden beveiligd in het herstelproces-abonnement toevoegen.

    g.  Klik op **groep 1: beginnen**, kiest u **Script**en alle volgende scripts in volgorde als de stappen **1 na groep** toevoegen.

    - Start-StorSimple-virtuele-toestel runbook
    - Over-StorSimple-volume-containers runbook mislukt
    - Koppeling-volumes-na-failover runbook
    - Runbook verwijderen-aangepaste-script-extensie

1.  Een handmatige actie na de bovenstaande 4 scripts toevoegen in dezelfde **groep 1: post stappen** sectie. Deze actie is het punt waarop u controleren kunt of alles correct werkt. Deze actie moet worden toegevoegd als een deel van de test failover (alleen selecteren de **Testen Failover** selectievakje).

2.  Na de handmatige actie, het opruimen script toevoegen met de procedure die u voor de andere runbooks gebruikt. Sla het herstelproces-abonnement.

    > [AZURE.NOTE] Wanneer een test failover actief is, moet u controleren of alles in de Actiestap handmatige omdat de StorSimple hoeveelheden die op de doelapparaat hebt u er zijn gekopieerd worden verwijderd als onderdeel van het opruimen nadat de handmatige actie is voltooid.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Een failover test uitvoeren

Raadpleeg de handleiding voor [Active Directory DR-oplossing](../site-recovery/site-recovery-active-directory.md) voor specifieke overwegingen met Active Directory tijdens een de overname van de test. De instelling van de on-premises implementatie is niet helemaal gestoord wanneer de test storing. De StorSimple hoeveelheden die zijn gekoppeld aan de on-premises implementatie VM zijn het StorSimple Cloud toestel op Azure gekopieerd. Een VM voor testdoeleinden van Azure is toegevoegd en de gekloonde hoeveelheden zijn gekoppeld aan de VM.

#### <a name="to-perform-the-test-failover"></a>De overname test uitvoeren

1.  Selecteer in de portal voor Azure klassieke uw site herstel kluis.

1.  Klik op het herstelproces-abonnement dat is gemaakt voor de bestandsserver VM.

2.  Klik op de **toets Failover**.

3.  Selecteer het virtuele netwerk om de test failover-proces te starten.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Wanneer de secundaire omgeving is, kunt u uw validatie kunt uitvoeren.

2.  Wanneer de validatie voltooid zijn, klikt u op **Validatie voltooid**. De failover-testomgeving zullen worden verwijderd en de bewerking TFO wordt voltooid.

## <a name="perform-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

Tijdens een een niet-geplande overname, de hoeveelheden StorSimple worden overgenomen door het virtuele apparaat, een replica VM worden gebracht omhoog op Azure en de hoeveelheden zijn gekoppeld aan de VM.

#### <a name="to-perform-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

1.  Selecteer in de portal voor Azure klassieke uw site herstel kluis.

1.  Klik op het herstelproces-abonnement dat is gemaakt voor bestandsserver VM.

2.  Klik op **Failover** en selecteer vervolgens **Niet-geplande Failover**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Selecteer het doelnetwerk en klik op het pictogram ✓ om de failoverproces te starten.

## <a name="perform-a-planned-failover"></a>Een geplande failover uitvoeren

Tijdens een een geplande overname bestand de on-premises implementatie server die VM is afsluiten en een cloud-back-momentopname volume met het op StorSimple apparaat is gemaakt. De hoeveelheden StorSimple worden overgenomen door het virtuele apparaat, een replica VM is opstarten op Azure en de hoeveelheden zijn gekoppeld aan de VM.

#### <a name="to-perform-a-planned-failover"></a>Een geplande failover uitvoeren

1.  Selecteer in de portal voor Azure klassieke uw site herstel kluis.

1.  Klik op het herstelproces-abonnement dat is gemaakt voor de bestandsserver VM.

2.  Klik op **Failover** en selecteer vervolgens **Geplande Failover**.

3.  Selecteer het doelnetwerk en klik op het pictogram ✓ om de failoverproces te starten.

## <a name="perform-a-failback"></a>Een failback uitvoeren

Tijdens een failback worden StorSimple volume containers overgenomen terug naar het apparaat fysiek nadat een back-up is die u hebt gemaakt.

#### <a name="to-perform-a-failback"></a>Een failback uitvoeren

1.  Selecteer in de portal voor Azure klassieke uw site herstel kluis.

1.  Klik op het herstelproces-abonnement dat is gemaakt voor de bestandsserver VM.

2.  Klik op **Failover** en selecteer **failover gepland** of **niet-geplande failover**.

3.  Klik op **richting wijzigen**.

4.  Selecteer de juiste gegevenssynchronisatie en opties voor het maken van VM.

5.  Klik op het pictogram ✓ om de failback-proces te starten.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="capacity-planning-and-readiness-assessment"></a>Capaciteit plannen en gereedheid assessment


#### <a name="hyper-v-site"></a>Hyper-V-site

Gebruik de [capaciteit van de gebruiker teamplanner hulpmiddel](http://www.microsoft.com/download/details.aspx?id=39057) voor het ontwerpen van de server, opslag en netwerkinfrastructuur voor uw Hyper-V replica-omgeving.

#### <a name="azure"></a>Azure

U kunt het [hulpprogramma Azure virtuele machines gereedheid Assessment](http://azure.microsoft.com/downloads/vm-readiness-assessment/) uitvoeren op VMs om ervoor te zorgen dat deze compatibel met Azure VMs en Azure Site herstel Services zijn. Het hulpprogramma voor het evaluatie van gereedheid controleert VM configuraties en wordt gewaarschuwd wanneer configuraties niet compatibel met Azure zijn. Deze problemen bijvoorbeeld een waarschuwing weergegeven als een station C: groter dan 127 GB is.


Capaciteit planning bestaat uit ten minste twee belangrijke processen:

-   Toewijzing on-premises implementatie Hyper-V VMs Azure VM grootte (zoals A6, A7, A8 en A9).

-   De vereiste internetbandbreedte nagaan.

## <a name="limitations"></a>Beperkingen

- Op dit moment alleen 1 StorSimple apparaat kan worden niet via (een enkel StorSimple Cloud toestel). Het scenario van een bestandsserver die zich uitstrekt over meerdere StorSimple apparaten wordt nog niet ondersteund.

- Als er een fout tijdens het inschakelen van beveiliging voor een VM, zorg dat u de iSCSI-doelen hebt losgekoppeld.

- Alle volume-containers die samen zijn gegroepeerd vanwege back-beleid over volume containers mislukt via samen.

- Enkel het volume van de volume-containers die u hebt gekozen via mislukt.

- Volumes die maximaal meer dan 64 TB toevoegen kunnen niet worden via is mislukt omdat de maximale capaciteit van een enkel StorSimple Cloud toestel 64 TB.

- Als de geplande/ongeplande overname mislukt en de VMs worden gemaakt in Azure wordt aangegeven, klikt u vervolgens niet opschonen het VMs. Voer in plaats daarvan een failback. Worden als u de VMs verwijderen klikt u vervolgens de on-premises implementatie VMs kunnen niet ingeschakeld opnieuw.

- Na een failover, bent u niet de hoeveelheden zien, gaat u naar de VMs, onderdeel de schijven opnieuw controleren en ze online weer.

- In sommige gevallen, de stationsletters in de site DR mogelijk anders dan de letters on-premises implementatie. Als dit gebeurt, moet u handmatig het probleem te verhelpen nadat de overname is voltooid.

- Meervoudige verificatie moet zijn uitgeschakeld voor de Azure referentie die is ingevoerd in het account automatisering als een actief. Als deze verificatie niet is uitgeschakeld, scripts kunnen niet automatisch wordt uitgevoerd en het abonnement herstel, mislukt.

- Time-out van de taak failover: het StorSimple script time-out wordt als de overname van volume containers langer duurt dan de limiet van Azure Site herstel per script (momenteel 120 minuten).

- Time-out voor back-uptaak: het StorSimple script treedt er een time-out als de back-up van hoeveelheden langer dan de limiet van Azure Site herstel per script (momenteel 120 minuten duurt).
 
    > [AZURE.IMPORTANT] Handmatig uitvoeren van de back-up van de Azure-portal en voer het herstelproces-abonnement opnieuw.

- Time-out van taak klonen: het StorSimple script treedt er een time-out als de klonen van hoeveelheden langer dan de limiet van Azure Site herstel per script (momenteel 120 minuten duurt).

- Synchronisatiefout tijd: het StorSimple scripts fouten af zeggen dat de back-ups niet lukt, zijn hoewel de back-up voltooid in de portal is. Een mogelijke oorzaak hiervoor kan zijn dat de waarde van het apparaat StorSimple tijd mogelijk niet gesynchroniseerd met de huidige tijd in de gewenste tijdzone.
 
    > [AZURE.IMPORTANT] De tijd toestel met de huidige tijd in de tijdzone synchroniseren.

- Toestel failover-fout: het StorSimple script mislukt mogelijk als er een toestel-overname is wanneer het abonnement herstel wordt uitgevoerd.
    
    > [AZURE.IMPORTANT] Nadat de overname toestel voltooid is, voert u het abonnement herstel opnieuw uit.

## <a name="summary"></a>Overzicht

Met Azure sites worden hersteld, kunt u een volledige geautomatiseerde gaan voor een bestandsserver VM maken met gedeelde bestanden die worden gehost op StorSimple opslag. U kunt de overname starten in enkele seconden vanaf elke locatie in het geval van een onderbreking en krijgen de toepassing omhoog en worden uitgevoerd in een paar minuten.
