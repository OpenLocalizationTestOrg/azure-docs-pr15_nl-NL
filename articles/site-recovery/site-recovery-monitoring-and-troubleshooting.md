<properties
    pageTitle="Controleren en problemen met beveiliging voor virtuele machines en fysieke servers | Microsoft Auzre" 
    description="Azure Site herstel coördinaten de herhaling, failover en herstel van virtuele machines die zijn opgeslagen op servers van de on-premises implementatie Azure of een secundaire datacenter. Lees dit artikel om te controleren en problemen met beveiliging VMM of Hyper-V-Site." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Controleren en problemen met beveiliging voor virtuele machines en fysieke servers

Deze controle- en Troubleshooting Guide kunt u meer informatie over het bijhouden van de status herhaling en technieken voor probleemoplossing voor Azure sites worden hersteld.

## <a name="understanding-the-components"></a>Informatie over de onderdelen

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>Site-implementatie VMware/fysieke voor replicatie tussen on-premises en Azure.
Voor het instellen van DR tussen on-premises implementatie VMware/fysieke machine; Configuratieserver, doel van de basispagina en proces moet geconfigureerd. Tijdens het inschakelen van beveiliging voor de bronserver installeert Azure Site herstel mobiliteit service. Bericht on-premises implementatie storing zodra de bronserver mislukt op Azure, klanten behoeften voor het instellen van een proces-Server in Azure wordt aangegeven en een basispagina doelserver on-premises als u wilt beveiligen met de bronserver terug naar gegenereerde on-premises implementatie. 

![VMware/fysieke Site implementatie voor replicatie tussen on-premises implementatie en Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>Site-implementatie VMM voor replicatie tussen on-premises-site.

Als onderdeel van het instellen van DR tussen twee on-premises locatie; Azure Site herstel Provider moet worden gedownload en geïnstalleerd op de server VMM. Provider moet internetconnectiviteit om ervoor te zorgen dat alle bewerkingen die vanaf de Portal van Azure geactiveerd wordt omgezet in on-premises implementatie bewerkingen zoals het inschakelen van beveiliging afsluiten primaire virtuele machines als onderdeel van failovers enzovoort.

![Site-implementatie VMM voor replicatie tussen on-premises-site](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>Site-implementatie VMM voor replicatie tussen on-premises implementatie en Azure.

Als onderdeel van het instellen van DR tussen on-premises implementatie en Azure; Azure Site herstel Provider moet worden gedownload en geïnstalleerd op de server VMM samen met Azure herstel Services Agent die moet worden geïnstalleerd op elke Hyper-V-host. Raadpleeg [Lidmaatschap Site Azure beveiliging](./site-recovery-understanding-site-to-azure-protection.md) voor meer informatie.

![Site-implementatie VMM voor replicatie tussen on-premises implementatie en Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Implementatie van Hyper-V Site replicatie tussen on-premises implementatie en Azure

Dit is dezelfde als die van VMM-implementatie: alleen verschil wordt Provider & Agent wordt geïnstalleerd op de host van Hyper-V zelf. Raadpleeg [Lidmaatschap Site Azure beveiliging](./site-recovery-understanding-site-to-azure-protection.md) voor meer informatie.

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Configuratie, beveiliging en herstel bewerkingen controleren

Alle bewerkingen van ASR wordt gecontroleerd en klik op het tabblad "Taken" wordt bijgehouden. Ga naar het tabblad taken in het geval van eventuele configuratie, beveiliging of herstel fout en Zie als er fouten zijn.

![Configuratie, beveiliging en herstel bewerkingen controleren](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Als u fouten optreden bij de weergave taken hebt gevonden, selecteert u de taak en klikt u op FOUTDETAILS voor die taak.

![Configuratie, beveiliging en herstel bewerkingen controleren](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Meer informatie over de fout beter kunt herkennen mogelijke oorzaak en de aanbeveling voor het probleem.

![Configuratie, beveiliging en herstel bewerkingen controleren](media/site-recovery-monitoring-and-troubleshooting/image5.png)

In de bovenstaande zaak lijkt er een andere bewerking die wordt uitgevoerd vanwege waarin beveiliging configuratie is verbroken. Zorg ervoor dat u het probleem aan de hand van de aanbeveling – daar na klikt u op RESART opnieuw starten met de bewerking oplost.

![Configuratie, beveiliging en herstel bewerkingen controleren](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Optie voor het opnieuw starten is niet beschikbaar voor alle bewerkingen – voor degenen die geen de optie opnieuw starten Ga terug naar het object en opnieuw de bewerking opnieuw uitvoeren. Elke taak kan worden geannuleerd op een willekeurig moment in uitvoering met de knop Annuleren.

![Configuratie, beveiliging en herstel bewerkingen controleren](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Replicatie servicestatus voor virtuele machine controleren

ASR provider centraal & externe controle via de Portal Azure voor elk van de beveiligde entiteiten. Navigeer naar de ITEMS beveiligde daar na select VMM WOLKEN of beveiliging groepen. Tabblad VMM WOLKEN geldt alleen voor VMM gebaseerd implementaties en alle andere scenario's hebben de beveiligde entiteiten onder het tabblad Beveiliging groepen.

![Replicatie servicestatus voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Hier na selecteren de beveiligde entity onder de desbetreffende cloud of de groep beveiliging. Wanneer u de beveiligde entity alle toegestaan worden bewerkingen in het onderste deelvenster weergegeven.

![Replicatie servicestatus voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Zoals hierboven in geval de virtuele machine dat status kritieke – is kunt u de knop meer informatie over fout onderaan om de fout weer te geven. Op basis van de 'mogelijke oorzaken' en 'Aanbeveling' genoemd los het probleem.

![Replicatie servicestatus voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Replicatie servicestatus voor virtuele machine controleren](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Opmerking: Als er een actieve bewerkingen die in uitvoering of mislukte Ga naar de weergave taken zoals eerder is vermeld om weer te geven van de taak specifieke fout.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Problemen met on-premises implementatie Hyper-V oplossen

Verbinding maken met de beheerconsole van on-premises implementatie Hyper-V en selecteer de virtuele machine raadpleegt u de status herhaling.

![Problemen met on-premises implementatie Hyper-V oplossen](media/site-recovery-monitoring-and-troubleshooting/image12.png)

*Replicatie-status* wordt in dit geval wordt aangegeven als kritiek – *Weergave herhaling systeemstatus* om de details weer te geven.

![Problemen met on-premises implementatie Hyper-V oplossen](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Voor situaties waarin replicatie is onderbroken voor de virtuele machine, met de rechtermuisknop op Select *herhaling*->*cv herhaling*
![problemen oplossen met de on-premises implementatie Hyper-V problemen](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Geval VM een nieuwe Hyper-V-host (binnen het cluster of een zelfstandige computer), die is geconfigureerd door ASR migreert, wouldn't herhaling voor de virtuele machine worden beïnvloed. Zorg ervoor dat de nieuwe Hyper-V host voldoet aan alle de per-materiaal en is geconfigureerd met ASR.

### <a name="event-log"></a>Gebeurtenislogboek

| Bronnen van gebeurtenissen                | Meer informatie                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Toepassingen en logboeken/Microsoft/VirtualMachineManager/Server/servicebeheerder** (VMM Server)   |  Hierdoor handig logboekregistratie voor veel verschillende VMM oplossen. |
| **Toepassingen en Service logboeken/MicrosoftAzureRecoveryServices/herhaling** (Host hyper-V)   | Hierdoor handig logboekregistratie voor het oplossen van problemen met veel Microsoft Azure herstel Services Agent. <br/> ![Bron van gebeurtenis voor host van Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Toepassingen en Service logboeken/Microsoft/Azure Site herstel/Provider/operationele** (Host hyper-V)   | Deze biedt handige logboekregistratie voor het oplossen van problemen met veel Microsoft Azure Site herstel-Service. <br/> ![Bron van gebeurtenis voor host van Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Toepassingen en logboeken/Microsoft/Windows/Hyper-V-VMMS/servicebeheerder** (Host hyper-V) | Hierdoor handig logboekregistratie voor het oplossen van problemen met veel Hyper-V VM versiebeheer. <br/> ![Bron van gebeurtenis voor host van Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Opties voor logboekregistratie van Hyper-V herhaling

Alle gebeurtenissen die betrekking hebben op Hyper-V Replica worden geregistreerd in het Hyper-V-VMMS\\beheerder log bevindt zich onder **Logboeken toepassingen en Services\\Microsoft\\Windows**. Een analytisch log kunt bovendien Hyper-V-VMMS zijn ingeschakeld. Schakel dit logboek door Zorg eerst de logboeken analysen en foutopsporing kan worden bekeken in de logboeken. Open Logboeken, klikt u vervolgens in het **menu Beeld**, klikt u op **analytische weergeven en logboeken voor foutopsporing**.

![Problemen met on-premises implementatie Hyper-V oplossen](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Een analytisch log is zichtbaar onder Hyper-V-VMMS

![Problemen met on-premises implementatie Hyper-V oplossen](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Klik in het deelvenster **Acties** op **Logboek inschakelen**. Eenmaal is ingeschakeld, locaties in **Performance Monitor** tijdens een sessie voor het traceren van gebeurtenis bevindt zich onder **verzamelen gegevenssets.**

![Problemen met on-premises implementatie Hyper-V oplossen](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Als u de informatie die worden verzameld, eerst de traceringssessie beëindigen door te schakelen van het logboek en slaat u het logboek en opnieuw openen in de logboeken of andere hulpmiddelen gebruiken om deze te converteren naar wens.



## <a name="reaching-out-for-microsoft-support"></a>Voor Microsoft Support brengen

### <a name="log-collection"></a>Log siteverzameling

Raadpleeg [ASR Log siteverzameling ondersteuning Diagnostics Platform (SDP) hulpmiddel gebruiken](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) om de vereiste logboeken verzamelen voor VMM Site beveiliging.

Voor de beveiliging van Hyper-V Site, het [hulpmiddel](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) downloaden en uitvoeren op de host Hyper-V voor het verzamelen van de logboeken.

Raadpleeg voor VMware/fysieke scenario's, [Azure herstel Log siteverzameling voor de beveiliging van VMware en fysieke site](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) om de vereiste logboeken verzamelen.

De logboeken lokaal onder een willekeurige naam submap onder **%LocalAppData%\ElevatedDiagnostics** die worden verzameld

![Voorbeeld van de stappen die worden weergegeven van Hyper-V site beveiliging.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Een ondersteuningsticket openen

Als u wilt verhogen ondersteuningsticket voor ASR, bereiken ter ondersteuning van Azure via de URL op <http://aka.ms/getazuresupport>

## <a name="kb-articles"></a>KB-artikelen

-   [Hoe u de letter voor beveiligde virtuele machines die zijn overgenomen of gemigreerd naar Azure behouden](http://support.microsoft.com/kb/3031135)
-   [Het beheren van on-premises implementatie naar Azure beveiliging netwerkbandbreedte gebruikt](https://support.microsoft.com/kb/3056159)
-   [ASR: "de resource cluster niet gevonden" fout wanneer u probeert beveiliging voor een virtuele machine inschakelen](http://support.microsoft.com/kb/3010979)
-   [Begrijpen en problemen met Hyper-V Replica gids](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Veelvoorkomende fouten in ASR en hun oplossingen

Hieronder vindt u de veelvoorkomende fouten die u mogelijk op en hun oplossingen. Elk van de fout wordt beschreven in een afzonderlijk wikipagina.

### <a name="general"></a>Algemene
-   <span style="color:green;">Nieuw</span> [Taken verbroken met fout "Er is een bewerking is uitgevoerd." Fout 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Nieuw</span> [Taken verbroken met fout 'De Server is niet verbonden met Internet'. Fout 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Setup
-   [De server VMM kan niet worden geregistreerd vanwege een interne fout. Raadpleeg de weergave taken in de Portal-Site herstellen voor meer informatie over de fout. Voer Setup opnieuw registreren van de server uit.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Een verbinding kan niet worden gemaakt om de Hyper-V herstel Manager. Controleer de proxy-instellingen of probeer het later opnieuw.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Configuratie
-   [Kan niet worden gemaakt, groep beveiliging: Er is een fout opgetreden bij het ophalen van de lijst met servers.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Hyper-V host cluster met ten minste één statische netwerkadapter of geen verbonden adapters zijn geconfigureerd voor gebruik van DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM is niet gemachtigd om een actie te voltooien](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Het account opslag binnen het abonnement tijdens het configureren van beveiliging selecteren niet](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Beveiliging
- <span style="color:green;">Nieuw</span> [Beveiliging inschakelen mislukken met foutbericht "beveiliging kan niet worden geconfigureerd voor de virtuele machine". Fout 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Nieuw</span> [Beveiliging inschakelen mislukken met foutbericht "beveiliging kan niet worden ingeschakeld voor de virtuele machine." Fout 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Nieuw</span> [Live migratiefout 23848 - de virtuele machine dient moeten worden verplaatst met type Live. Hiermee kan de status van de beveiliging herstel van de virtuele machine verbreken.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Beveiliging is mislukt omdat Agent is niet geïnstalleerd op host machine inschakelen](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Een geschikte host voor de replica virtuele machine kan niet worden gevonden - vanwege laag berekenen resources](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Een geschikte host voor de replica virtuele machine kan niet worden gevonden - vanwege geen logische netwerk gekoppeld](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Kan geen verbinding maken met de computer van de replica host - kan geen verbinding worden gemaakt](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Herstel
- VMM kan de bewerking host - niet voltooien
    -   [Naar het geselecteerde herstelpunt voor virtuele machine mislukken: algemene fout: toegang geweigerd.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Hyper-V is mislukt mislukt boven aan de geselecteerde herstel komma voor VM: bewerking afgebroken probeert een recentere herstelpunt. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Een verbinding met de server kan niet worden waarop deze is opgezet (0x00002EFD)
        -   [Hyper-V kan geen omgekeerde herhaling voor virtuele machine inschakelen](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Hyper-V kan geen herhaling voor VM virtuele machine inschakelen](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Failover voor VM kan niet worden doorgevoerd.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Het herstelplan bevat virtuele machines die niet gereed voor geplande failover zijn](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [De virtuele machine niet gereed is voor geplande failover](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuele machine niet wordt uitgevoerd en wordt niet uitgeschakeld](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Verouderd band bewerking is er gebeurd op een virtuele machine en doorvoeren failover is mislukt](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Failover testen
    -   [Failover kan niet worden gestart, aangezien test failover uitgevoerd wordt](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Nieuw</span>  Failover treedt er een time-out met PreFailoverWorkflow taak WaitForScriptExecutionTaskTimeout vanwege de configuratie-instellingen op de beveiligingsgroep van netwerk dat is gekoppeld aan de virtuele Machine of het subnet waaraan de computer behoort. Verwijzen naar ['PreFailoverWorkflow taak WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) voor meer informatie.


### <a name="configuration-server-process-server-master-target"></a>Configuratie-Server, processerver, outmodel doel
Configuratieserver (CS), processerver (PS), outmodel Targer (MT)
-   [De ESXi host waarop de PS/CS wordt gehost als een VM mislukt met een paarse scherm.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Extern bureaublad oplossen na failover
-   Veel klanten hebt problemen via VM in Azure verbinding maken met de mislukte geconfronteerd. [Gebruik en het oplossen van problemen document RDP in de VM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Het toevoegen van een openbare IP-adres voor een resource manager virtuele machine
Als de knop **koppelen** in de portal lichter gekleurd wordt weergegeven en u geen verbinding is met Azure via een Express Route of Site-naar-Site VPN-verbinding, die u wilt maken en uw VM toewijzen een openbare IP-adres voordat u RDP/SSH kunt gebruiken. Volg de onderstaande stappen voor het toevoegen van een openbare IP-adres op de netwerkinterface van de virtuele machine.  

![Het toevoegen van een openbare IP-adres op het netwerkinterface van is mislukt via virtuele machines](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)