<properties
    pageTitle="Overname in Site herstel | Microsoft Azure" 
    description="Azure Site herstel coördinaten de herhaling, failover en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of een secundaire datacenter." 
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

# <a name="failover-in-site-recovery"></a>Overname in sites worden hersteld

De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

In dit artikel vindt u informatie en instructies voor het herstellen van (meer dan en bij ontbreken terug verbroken) virtuele machines en fysieke servers die zijn beveiligd met sites worden hersteld. 

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.


## <a name="types-of-failover"></a>Soorten failover

Nadat de beveiliging is ingeschakeld voor virtuele machines en fysieke servers en ze bent repliceren kunt u failovers kunt uitvoeren, zoals nodig is. Site-herstel ondersteunt allerlei soorten failover.

**Failover** | **Wanneer moet worden uitgevoerd** | **Meer informatie** | **Proces**
---|---|---|---
**Test failover** | Uitvoeren om te valideren van uw strategie herhaling of het uitvoeren van een noodgevallen herstel meer details | Geen verlies van gegevens of downtime.<br/><br/>Geen invloed op herhaling<br/><br/>Geen invloed op uw productieomgeving | De overname starten<br/><br/>Opgeven hoe test machines worden verbonden met netwerken na failover<br/><br/>De voortgang bijhouden op het tabblad **taken** . Test machines worden gemaakt en starten in de secundaire locatie<br/><br/>Azure - verbinding maken met de computer in de portal van Azure<br/><br/>Secundaire site - toegang tot de computer op dezelfde host en cloud<br/><br/>Voltooi testen en opschonen automatisch test failover-instellingen.
**Geplande failover** | Klaar om te voldoen aan nalevingsvereisten<br/><br/>Uitvoeren voor gepland onderhoud<br/><br/>Als u wilt behouden werkbelasting uitgevoerd voor bekende bijvoorbeeld - zoals een fout verwachte power of slechte weerberichten mislukken uitvoeren<br/><br/>Uitvoeren naar failback na failover vanaf primaire naar de secundaire | Geen verlies van gegevens<br/><br/>Downtime worden gemaakt tijdens de tijd die het duurt de virtuele machine op de primaire afsluiten en weer op de secundaire locatie.<br/><br/>Virtuele machines zijn impact als doelmachines bron machines na failover. | De overname starten<br/><br/>De voortgang bijhouden op het tabblad **taken** . Bron machines worden afgesloten<br/><br/>Replica machines starten in de secundaire locatie<br/><br/>Azure - verbinding maken met de computer replica in de portal van Azure<br/><br/>Secundaire site - toegang tot de computer op dezelfde host en in de cloud dezelfde<br/><br/>De overname doorvoeren
**Niet-geplande failover** | Dit type failover uitvoert op een wijze die reactieve wanneer een primaire-site niet toegankelijk zijn vanwege een onverwachte incident, zoals een power storing of virus aanval <br/><br/> U kunt uitvoeren als een niet-geplande failover kan worden uitgevoerd, zelfs als primaire site is niet beschikbaar. | Verlies van gegevens is afhankelijk van herhaling frequentie-instellingen<br/><br/>Gegevens worden bijgewerkt overeenkomstig de laatste keer dat deze is gesynchroniseerd | De overname starten<br/><br/>De voortgang bijhouden op het tabblad **taken** . (Optioneel) kunt u virtuele machines wilt afsluiten en de meest recente gegevens synchroniseren<br/><br/>Replica machines starten in de secundaire locatie<br/><br/>Azure - verbinding maken met de computer replica in de portal van Azure<br/><br/>Toegang tot een secundaire site de computer op dezelfde host en in de cloud dezelfde<br/><br/>De overname doorvoeren


De soorten failovers die worden ondersteund, is afhankelijk van uw implementatiescenario.

**Failover richting** | **Test failover** | **Geplande failover** | **Niet-geplande failover**
---|---|---|---
Primaire VMM site naar secundaire VMM-site | Ondersteund | Ondersteund | Ondersteund 
Secundaire VMM site naar primaire VMM-site | Ondersteund | Ondersteund | Ondersteund
Cloud in de Cloud (één VMM server) |  Ondersteund | Ondersteund | Ondersteund
Azure VMM site | Ondersteund | Ondersteund | Ondersteund 
Azure naar VMM-site | Niet-ondersteunde | Ondersteund | Niet-ondersteunde 
Hyper-V site Azure | Ondersteund | Ondersteund | Ondersteund
Azure naar Hyper-V-site | Niet-ondersteunde | Ondersteund | Niet-ondersteunde
Azure VMware site | Ondersteund (enhanced-scenario)<br/><br/> Niet-ondersteunde (oudere scenario) |Niet-ondersteunde | Ondersteund
Fysiek server Azure | Ondersteund (enhanced-scenario)<br/><br/> Niet-ondersteunde (oudere scenario) | Niet-ondersteunde | Ondersteund

## <a name="failover-and-failback"></a>Failover en foutherstel

U kunt niet via virtuele machines tot een secundaire on-premises implementatie-site of tot Azure, afhankelijk van uw implementatie. Een machine die wordt overgenomen door Azure wordt gemaakt als een Azure virtuele machines. U kunt niet via een enkele virtuele machine of fysieke server of een herstel-abonnement. Herstel abonnementen bestaat uit een of meer groepen met beveiligde virtuele machines of fysieke servers hebt besteld. Deze worden gebruikt voor failover goedkeuringen van meerdere apparaten die niet op op basis van de groep die zich bevinden. [Lees meer](site-recovery-create-recovery-plans.md) over herstel abonnementen. 

Houd rekening met het volgende nadat failover is voltooid en uw machines gebruiksklaar bevinden zich op een secundaire locatie:

- Als u niet Azure, nadat de machines failover beveiligde of vermenigvuldigt niet in de primaire of secundaire locatie. Azure de virtuele machines opgeslagen in geografische gerepliceerd-opslag waarmee u tolerantie, maar niet herhaling.
- Als u een niet-geplande failover naar een secundaire site nadat de machines failover op de secundaire locatie niet zijn beveiligd of repliceren hebt gedaan.
- Als u een geplande failover naar een secundaire site na de machines failover op de secundaire locatie hebt zijn beveiligd.
 

### <a name="failback-from-secondary-site"></a>Failback van secundaire site

Failback van een secundaire site aan een primair gebruikt dezelfde stappen als failover van de primaire naar secundaire. Nadat de overname van primaire met secundaire datacenter vastgelegde en voltooid is, kunt u omgekeerde herhaling kunt starten wanneer uw primaire site beschikbaar. Replicatie tussen de secundaire site en de primaire start omgekeerde herhaling door de gegevens delta te synchroniseren. Omgekeerde herhaling levert de virtuele machines met een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie. Zodat u de primaire-site in de actieve locatie start u een geplande failover uit naar primaire, secundaire gevolgd door een andere omgekeerde herhaling.

### <a name="failback-from-azure"></a>Failback van Azure

Als u via Azure is mislukt wordt uw virtuele machines zijn beveiligd door de tolerantiefuncties Azure voor virtuele machines. Als u de oorspronkelijke primaire site in de actieve locatie wilt uitvoeren u een geplande failover. U kunt terug naar de oorspronkelijke locatie of naar een andere locatie mislukt als de oorspronkelijke site niet beschikbaar is. Als u wilt repliceren na failback naar de primaire locatie opnieuw start starten u een omgekeerde herhaling.

### <a name="failover-considerations"></a>Failover-overwegingen

- **IP-adres na failover**, standaard een mislukte via computer heeft een ander IP-adres dan de broncomputer. Als u behouden de dezelfde IP address Zie wilt: 
    - **Secundaire site**, als u bent niet via een secundaire site en u wilt bewaren van een IP-adres [lezen](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) in dit artikel. Houd er rekening mee dat u een openbare IP-adres behouden kunt als uw Internetprovider dit ondersteunt.
    - **Azure**, als u bent niet via Azure kunt u het IP-adres dat u wilt toewijzen op het tabblad **configureren** van de eigenschappen van het virtuele machine opgeven. U kunt een openbare IP-adres kan niet behouden na failover naar Azure. U kunt RFC 1918 adres spaties die worden gebruikt als interne adressen bewaren.
- **Gedeeltelijke failover**, als u wilt mislukt over gedeelte van een site in plaats van een hele site rekening met het volgende: 
    - **Secundaire site**, als u niet over gedeelte van een primaire-site op een secundaire site en u een verbinding terug naar de primaire site wilt maken, een VPN-verbinding voor site-naar-site via verbinding met mislukte toepassingen op de secundaire site onderdelen van de infrastructuur uitgevoerd op de primaire site. Als een hele subnet overgenomen kunt u het IP-adres van VM bewaren. Als u niet over een gedeeltelijke subnet niet kunt u het IP-adres van VM bewaren omdat subnetten kunnen niet worden gesplitst tussen sites.
    - **Azure**, als u via een gedeeltelijke site van Azure mislukt en terug naar de hoofdsite verbinding wilt maken, kunt u een VPN-verbinding voor de site-naar-site via de toepassing in Azure naar onderdelen van de infrastructuur uitgevoerd op de primaire site een mislukte verbinding maken. Houd er rekening mee dat als de hele subnet mislukt u kunnen het virtuele machine IP-adres bewaren. Als u niet over een gedeeltelijke subnet niet kunt u het IP-adres van VM bewaren omdat subnetten kunnen niet worden gesplitst tussen sites.
 
- **Station**: als u wilt behouden de letter op virtuele machines na failover kunt u de SAN-beleid voor de virtuele machine instellen aan **op**. VM schijven komt automatisch online. [Meer informatie](https://technet.microsoft.com/library/gg252636.aspx).
- **Route-clientaanvragen**, herstel van de Site met Azure verkeer Manager op aanvragen van de route-clients in uw toepassing na failover werkt.




## <a name="run-a-test-failover"></a>Een failover test uitvoeren

Wanneer u een test failover uitvoert wordt u gevraagd om te selecteren netwerkinstellingen voor test replica machines. Er wordt een aantal opties.  

**Test failover-optie** | **Beschrijving** | **Failover controleren** | **Meer informatie**
---|---|---|---
**Naar Azure mislukken, zonder netwerk** | Selecteer een doel Azure netwerk niet | Failover controles die VM testen start zoals verwacht in Azure wordt aangegeven | Alle test virtuele machines in een indeling herstellen worden toegevoegd in een enkel cloudservice en verbinding kunt maken met elkaar<br/><br/>Machines niet worden verbonden met een Azure netwerk na failover.<br/><br/>Gebruikers kunnen verbinding met de computers test met een openbare IP-adres
**Naar Azure mislukken, met netwerk** | Selecteer een doel Azure netwerk | Failover Hiermee wordt gecontroleerd of test machines met het netwerk is verbonden | Maak een Azure netwerk geïsoleerd uit het Azure productienetwerk en het instellen van de infrastructuur voor de gerepliceerde virtuele machine werkt zoals verwacht.<br/><br/>Het subnet van de test virtuele machine is gebaseerd op subnet waarop de mislukte via virtuele machine naar verwachting verbinding maken met als een of niet gepland storing.
**Naar de site van een secundaire VMM mislukken, zonder netwerk** | Selecteer een VM-netwerk niet | Failover Hiermee wordt gecontroleerd of test machines zijn gemaakt.<br/><br/>De test virtuele machine wordt gemaakt op dezelfde host als host waarop de replica virtuele machine bestaat. Deze niet wordt toegevoegd aan de cloud waarin de replica virtuele machine zich bevindt. | <p>De mislukte via de computer niet wordt aangesloten op een netwerk.<br/><br/>De computer kan worden verbonden met een netwerk VM nadat deze is gemaakt
**Naar de site van een secundaire VMM mislukken, met netwerk** | Selecteer een bestaand VM netwerk een | Failover Hiermee wordt gecontroleerd of virtuele machines zijn gemaakt | De test virtuele machine wordt gemaakt op dezelfde host als host waarop de replica virtuele machine bestaat. Deze niet wordt toegevoegd aan de cloud waarin de replica virtuele machine zich bevindt.<br/><br/>Maken van een netwerk VM die heeft geïsoleerd van uw productienetwerk<br/><br/>Als u gebruikmaakt van een netwerk VLAN gebaseerde raden dat u een afzonderlijke logische netwerk (niet gebruikt in productie) maken in VMM daartoe. Deze logisch netwerk gebruikt voor het maken van VM-netwerken te gebruiken voor het testen failover.<br/><br/>De logische netwerk moet worden gekoppeld aan ten minste één van de netwerkadapters van alle Hyper-V servers host voor virtuele machines.<br/><br/>Voor VLAN logische netwerken te gebruiken, moeten de netwerksites aan de logische netwerk toevoegen worden geïsoleerd.<br/><br/>Als u een Windows-netwerk Virtualization gebaseerde logisch netwerk gebruikt, wordt geïsoleerd VM netwerken automatisch gemaakt door Azure sites worden hersteld.
**Naar de site van een secundaire VMM mislukken, een netwerk maken** | Een tijdelijke testnetwerk wordt gemaakt op basis van de instelling die u in **Logische netwerk** - en de gerelateerde netwerksites opgeeft automatisch | Failover Hiermee wordt gecontroleerd of virtuele machines zijn gemaakt | Gebruik deze optie als het abonnement herstel gebruikmaakt van meer dan één VM netwerk. Als u Windows netwerk Virtualization netwerken gebruikt, kunt deze optie VM-netwerken te gebruiken met dezelfde instellingen (subnetten en IP-adres van toepassingen) automatisch maken in het netwerk van de replica virtuele machine. Deze netwerken VM worden automatisch afgewerkt nadat de overname test voltooid is.</p><p>De test virtuele machine wordt gemaakt op dezelfde host als host waarop de replica virtuele machine bestaat. Deze niet wordt toegevoegd aan de cloud waarin de replica virtuele machine zich bevindt.

>[AZURE.NOTE] Het IP-adres dat is opgegeven met een virtuele machine tijdens een test overname is gelijk aan het IP-adres dat deze wanneer ontvangt een of niet gepland failover (ervan uitgaande dat het IP-adres beschikbaar in het netwerk van de failover-test is. doen Als hetzelfde IP-adres is niet beschikbaar in het netwerk van de failover-test vervolgens ontvangt VM een ander IP-adres beschikbaar in het netwerk van de failover-test.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Een test failover uitgevoerd vanuit on-premises worden Azure

Deze procedure wordt beschreven hoe een failover test voor een abonnement herstel uitvoeren. U kunt ook de overname voor één computer uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Herstel van abonnement** > *recoveryplan_name*. Klik op **Failover** > **Test Failover**.
2. Klik op de pagina **Bevestigen Test Failover** opgeven hoe de replica machines worden verbonden met een Azure netwerk na failover.
3. Als u bent niet via Azure en gegevensversleuteling is ingeschakeld voor de cloud, schakelt u het certificaat dat is uitgegeven wanneer u gegevensversleuteling tijdens de installatie van de Provider ingeschakeld in **Versleuteling-toets** . 
4. Failover voortgang bijhouden op het tabblad **taken** . U moet mogelijk om de test replica machine in de portal van Azure weer te geven.
5. U kunt openen replica machines in Azure wordt aangegeven vanaf uw on-premises implementatie site starten een RDP-verbinding met de virtuele machine. poort 3389 moet zijn geopend op het eindpunt voor de virtuele machine.
5. Wanneer u klaar bent, als de overname bereikt de fase **voltooid testen** , klikt u op **Voltooid testen** om te voltooien.
5. Records in **notities** en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.
8. Klik op **de toets overname is voltooid** om op te schonen automatisch de testomgeving. Wanneer dit voltooid is wordt de overname test de status van de**omplete** C weergegeven.

> [AZURE.NOTE] Als een test failover zich blijft voor meer dan twee weken die deze moet worden uitgevoerd door dwingen voordoen. Alle elementen of virtuele machines automatisch gemaakt tijdens de test overname worden verwijderd.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Een failover test uitvoeren vanaf een primaire on-premises-site op een secundaire on-premises implementatie-site

U moet doen van een aantal dingen die u moet uitvoeren van een test failover, waaronder bellen van een kopie van de domeincontroller en test DHCP en DNS servers in uw testomgeving plaatsen. U kunt dit op een aantal manieren doen:

- Als u uitvoeren van een test failover gebruikmaakt van een bestaand netwerk wilt, voorbereiden u Active Directory, DHCP en DNS-in dat netwerk.
- Als u een test failover VM netwerken automatisch maken met de optie uitvoeren wilt, moet u handmatig stap vóór groep-1 toevoegen in het herstelproces is-abonnement dat u wilt gebruiken voor de overname test en vervolgens de infrastructuur-bronnen toevoegen met de automatisch gemaakte netwerk voordat u de overname test.

#### <a name="things-to-note"></a>Punten onthouden

- Wanneer worden gerepliceerd naar een secundaire site, het type netwerk door de machine replica gebruikt niet moet overeenkomen met het type logische netwerk die worden gebruikt voor test failover, maar sommige combinaties werkt mogelijk niet. Als de replica gebruikmaakt van DHCP en VLAN gebaseerde moeten worden geïsoleerd, hoeft het netwerk VM voor de replica niet een statische IP-adres-groep. Met behulp van Windows netwerk Virtualization voor de overname test goed niet omdat er geen adres van toepassingen beschikbaar zijn. Daarnaast werken test failover niet als het netwerk replica geen moeten worden geïsoleerd en de testnetwerk Windows netwerk Virtualization is. Dit is omdat het netwerk geen moeten worden geïsoleerd geen de subnetten die is vereist voor het maken van een netwerk met een Windows-netwerk Virtualization.
- De manier waarop in welke replica virtuele machines met verbonden zijn toegewezen VM netwerken nadat failover is afhankelijk van hoe de VM-netwerk is geconfigureerd in de console VMM:
    - **VM netwerk is geconfigureerd met geen moeten worden geïsoleerd of VLAN moeten worden geïsoleerd**, als DHCP is gedefinieerd voor het netwerk VM, de replica virtuele machine worden verbonden met de VLAN-ID met de instellingen die zijn opgegeven voor de netwerksite in het bijbehorende logische netwerk. De virtuele machine ontvangt het IP-adres van de beschikbare DHCP-server. U kunt een statische IP-adresgroep gedefinieerd voor de doeltoepassing VM-netwerk niet nodig hebt. Als een statische IP-adresgroep wordt gebruikt voor het netwerk VM wordt de replica virtuele machine niet worden verbonden met de VLAN-ID met de instellingen die zijn opgegeven voor de netwerksite in het bijbehorende logische netwerk. De virtuele machine ontvangt het IP-adres van de opgegeven voor het netwerk VM toepassingen. Als een statische IP-adres van toepassingen niet is gedefinieerd op het netwerk VM, toewijzing van IP-adressen, mislukt. De groep met IP-adressen moet worden gemaakt op zowel de bronsite en doelsites VMM-servers die u wilt gebruiken voor beveiliging en herstel.
    - **VM netwerk met Windows netwerk virtualization**, als een netwerk VM is geconfigureerd met deze instelling een statische groep moet worden gedefinieerd voor de doeltoepassing VM-netwerk, ongeacht of de bron VM netwerk is geconfigureerd naar gebruik DHCP of een statische IP-adres van toepassingen. Als u DHCP definieert, wordt de doelserver VMM fungeren als DHCP-server en bieden een IP-adres uit de groep die is gedefinieerd voor het netwerk VM. Als het gebruik van een resourcegroep die statische IP-adres is gedefinieerd voor de bronserver, wordt de doelserver VMM een IP-adres uit de groep toewijzen. In beide gevallen mislukt toewijzing van IP-adressen als een statische IP-adres van toepassingen niet is gedefinieerd.

#### <a name="run-test"></a>Test uitvoeren

Deze procedure wordt beschreven hoe een failover test voor een abonnement herstel uitvoeren. U kunt ook de overname voor een enkele VM of fysieke server uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Herstel van abonnement** > *recoveryplan_name*. Klik op **Failover** > **Test Failover**.
2. Klik op de pagina **Bevestigen testen Failover** opgeven hoe virtuele machines met netwerken moeten worden gekoppeld nadat de overname test.
3. Failover voortgang bijhouden op het tabblad **taken** . Wanneer de overname de fase** voltooid testen bereikt** , klikt u op **Voltooid testen** om te voltooien van de overname test.
4. Klik op **notities** als u wilt opnemen en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.
4. Controleer of de virtuele machines gestart nadat deze voltooid is.
5. Nadat u hebt gecontroleerd virtuele machines is gestart, voert u de overname testen om de geïsoleerd omgeving op te schonen. Als u ervoor kiest om automatisch te maken VM-netwerken te gebruiken, opruimen Hiermee verwijdert u alle test virtuele machines en netwerken testen.

> [AZURE.NOTE] Als een test failover zich blijft voor meer dan twee weken die deze moet worden uitgevoerd door dwingen voordoen. Alle elementen of virtuele machines automatisch gemaakt tijdens de test overname worden verwijderd.


#### <a name="prepare-dhcp"></a>DHCP voorbereiden

Als de virtuele machines betrokken bij test failover DHCP gebruiken, een test DHCP-server binnen het geïsoleerd netwerk dat wordt gemaakt voor de test failover moet worden gemaakt.


### <a name="prepare-active-directory"></a>Active Directory voorbereiden
Als u wilt een failover test voor de toepassing testen uitvoeren, moet u een kopie van de Active Directory productieomgeving in uw testomgeving. Ga via de sectie [testen failover overwegingen voor active directory](site-recovery-active-directory.md#considerations-for-test-failover) voor meer informatie. 


### <a name="prepare-dns"></a>DNS voorbereiden

Voorbereiden op een DNS-server de overname test als volgt:

- **DHCP**, als virtuele machines DHCP, het IP-adres van de test DNS moet worden bijgewerkt op de toets DHCP-server. Als u een netwerktype van Windows netwerk Virtualization gebruikt, wordt de server VMM fungeert als de DHCP-server. Daarom moet het IP-adres van de DNS-zijn bijgewerkt in het netwerk van de failover-test. In dit geval registreert de virtuele machines zelf aan de relevante DNS-Server.
- **Statisch adres**, als virtuele machines een statische IP-adres gebruikt, het IP-adres van de test DNS-server in test failover netwerk moet worden bijgewerkt. Mogelijk moet u DNS bijwerken met het IP-adres van de test virtuele machines. Hiervoor kunt u het volgende voorbeeldscript: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Een geplande failover (primair naar secundair) uitvoeren

 Deze procedure wordt beschreven hoe een geplande failover voor een abonnement herstel uitvoeren. U kunt ook de overname voor een enkele virtuele machine uitvoeren op het tabblad **virtuele Machines** .

1. Controleer voordat u begint of alle virtuele machines die u wilt mislukken eerste replicatie is voltooid.
2. Selecteer **Herstel van abonnement** > *recoveryplan_name*. Klik op **Failover** > **Failover die is gepland**. 
3. Kies de bronsite en doelsites locaties op de pagina **Gepland Failover bevestigen **. Opmerking de richting failover.

    - Als het vorige failovers werkt zoals verwacht en alle VM servers bevinden zich op de bron- of doellijst locatie, wordt de details van de richting failover zijn alleen ter informatie. 
    - Als virtuele machines actief op de bron- en doellijst locaties zijn, wordt de knop **Tekstrichting van de wijziging** wordt weergegeven. Gebruik deze knop om te wijzigen en geef de richting waarin de overname moet worden uitgevoerd.

5. Als u bent niet via Azure en gegevensversleuteling is ingeschakeld voor de cloud, schakelt u het certificaat dat is uitgegeven wanneer u gegevensversleuteling tijdens de installatie van de Provider op de server VMM ingeschakeld in **Versleuteling-toets** . 
6. Wanneer een geplande failover begint is de eerste stap het afsluiten van de virtuele machines om ervoor te zorgen er geen gegevens verloren gaan. Klik op het tabblad **taken** kunt u de voortgang failover volgen. Als er een fout optreedt in de overname (hetzij op een virtuele machine of in een script die deel van het abonnement herstel uitmaakt aan), de geplande overname van een abonnement herstel stopt. U kunt de overname opnieuw starten.
8. Nadat replica virtuele machines zijn gemaakt zijn de rijen in een doorvoeren in behandeling staat. Klik op **doorvoeren** als u wilt doorvoeren van de overname. 
9. Als de replicatie is voert u de virtuele machines opstarten op de secundaire locatie. 

## <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

Deze procedure wordt beschreven hoe een niet-geplande failover voor een abonnement herstel uitvoeren. U kunt ook de overname voor een enkele VM of fysieke server uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Herstel van abonnement** > *recoveryplan_name*. Klik op **Failover** > **niet-geplande Failover**. 
3. Kies de bronsite en doelsites locaties op de pagina **Niet-geplande Failover bevestigen **. Opmerking de richting failover.

    - Als het vorige failovers werkt zoals verwacht en alle VM servers bevinden zich op de bron- of doellijst locatie, wordt de details van de richting failover zijn alleen ter informatie. 
    - Als virtuele machines actief op de bron- en doellijst locaties zijn, wordt de knop **Tekstrichting van de wijziging** wordt weergegeven. Gebruik deze knop om te wijzigen en geef de richting waarin de overname moet worden uitgevoerd.

4. Als u bent niet via Azure en gegevensversleuteling is ingeschakeld voor de cloud, schakelt u het certificaat dat is uitgegeven wanneer u gegevensversleuteling tijdens de installatie van de Provider op de server VMM ingeschakeld in **Versleuteling-toets** . 
5. Selecteer **virtuele machines afsluiten en de meest recente gegevens synchroniseren** om op te geven dat sites worden hersteld proberen moet de beveiligde virtuele machines afsluiten en de gegevens te synchroniseren, zodat de nieuwste versie van de gegevens worden overgenomen. Als u deze optie niet selecteert of poging tot het niet mogelijk zijn de overname van de meest recente komma herstelproces is beschikbaar voor de virtuele machine.
6. Klik op het tabblad **taken** kunt u de voortgang failover volgen. Houd er rekening mee dat zelfs als er fouten optreden tijdens een een niet-geplande overname, het abonnement herstel wordt uitgevoerd totdat deze voltooid is.
7. Na de overname zijn de virtuele machines status **in behandeling doorvoeren** . Klik op **doorvoeren** als u wilt doorvoeren van de overname.
8. Als u de replicatie hebt ingesteld voor het gebruik van meerdere herstel wordt verwezen, in wijzigen herstel punt kunt u een herstelpunt die niet de meest recente gebruiken (laatste al dan niet standaard wordt gebruikt). Nadat u aanvullende doorvoeren worden herstel punten verwijderd.
9. Als de replicatie is voltooid wordt de virtuele machines opstart en op de secundaire locatie worden uitgevoerd. Ze zijn echter niet beveiligd of repliceren. Als de primaire site beschikbaar voor communicatie nogmaals met de infrastructuur voor dezelfde onderliggende is, klikt u op **Omgekeerde repliceren** om te beginnen met omgekeerde herhaling. Dit zorgt ervoor dat alle gegevens worden gerepliceerd terug naar de primaire-site en dat de virtuele machine gereed is voor failover opnieuw. Replicatie omkeren nadat een niet-geplande failover een belasting van de overdracht van gegevens bijhoudt. De overdracht gebruikt dezelfde methode die is geconfigureerd voor de eerste replicatie-instellingen voor de cloud.

## <a name="failback-from-secondary-to-primary"></a>Failback van secundaire naar primaire

 Na een failover van de primaire naar secundaire locatie gerepliceerde virtuele machines niet zijn beveiligd met herstel van de Site en de secundaire locatie nu fungeert als de primaire. Volg deze procedures mislukt terug naar de oorspronkelijke primaire site. Deze procedure wordt beschreven hoe een geplande failover voor een abonnement herstel uitvoeren. U kunt ook de overname voor een enkele virtuele machine uitvoeren op het tabblad **virtuele Machines** .

1. Selecteer **Herstel van abonnement** > *recoveryplan_name*. Klik op **Failover** > **Failover die is gepland**.
2. Kies de bronsite en doelsites locaties op de pagina **Gepland Failover bevestigen **. Opmerking de richting failover. Als de overname van primaire heeft gewerkt als verwachten en alle virtuele machines in de secundaire locatie die dit alleen ter informatie is.
3. Als u terug van Azure verbroken bent selecteert u instellingen in de **Synchronisatie van gegevens**:

    - **Gegevens voordat u failover (alleen voor synchroniseren deltawijzigingen) synchroniseren**, deze optie wordt beperkt downtime voor virtuele machines tijdens het synchroniseren van deze zonder af te sluiten. Dit gebeurt het volgende:
        - Fase 1: Maakt momentopname van de virtuele machine in Azure wordt aangegeven en gekopieerd naar de on-premises implementatie Hyper-V-host. De computer blijft uitgevoerd in Azure wordt aangegeven.
        - Fase 2: Uitgeschakeld de virtuele machine in Azure wordt aangegeven, zodat er geen nieuwe wijzigingen worden uitgevoerd. De laatste set wijzigingen worden overgebracht naar de on-premises implementatie-server en de on-premises implementatie virtuele machine wordt gestart.
    

    - **Synchroniseren gegevens tijdens een overname alleen (volledige downloaden)**, gebruikt u deze optie als u op Azure lange tijd hebt is uitgevoerd. Deze optie is sneller omdat we verwachten dat meestal de schijf is gewijzigd en we niet wilt dat tijd in controlesom berekening. Het downloaden van een van de schijf uitvoeren. Het is ook handig wanneer de on-premises virtuele machine is verwijderd.
    
    > [AZURE.NOTE] We raden u gebruik deze optie als u hebt uitgevoerd Azure een tijdje (een maand of meer) of de VM on-premises is verwijderd. Deze optie niet controlesom berekeningen uit te voeren.
    
5. Als u bent niet via Azure en gegevensversleuteling is ingeschakeld voor de cloud, schakelt u het certificaat dat is uitgegeven wanneer u gegevensversleuteling tijdens de installatie van de Provider op de server VMM ingeschakeld in **Versleuteling-toets** . 
5. Al dan niet standaard de laatste herstel komma wordt gebruikt, maar in **Wijzigen herstel punt** kunt u een herstelpunt verschillende opgeven. 
6. Klik op het vinkje om te beginnen de failback.  Klik op het tabblad **taken** kunt u de voortgang failover volgen. 
7. f die u hebt geselecteerd de optie voor het synchroniseren van de gegevens voordat u de overname, nadat de oorspronkelijke gegevenssynchronisatie voltooid is en u iets wilt afsluiten de virtuele machines in Azure wordt aangegeven, klikt u op **taken**  >  <planned failover job name> **Voltooid Failover**. Dit de Azure computer afgesloten, brengt de meest recente wijzigingen naar de on-premises implementatie virtuele machine en deze wordt gestart.
8. U kunt nu zich aanmeldt bij de virtuele machine voor het valideren van deze beschikbaar is zoals verwacht. 
9. De virtuele machine is in een doorvoeren in behandeling staat. Klik op **doorvoeren** als u wilt doorvoeren van de overname.
10. Klik nu om te voltooien de failback op **Omkeren repliceren** om te beginnen met het beveiligen van de virtuele machine in de primaire-site.



## <a name="failback-to-an-alternate-location"></a>Failback naar een andere locatie

Als u hebt geïmplementeerd beveiliging tussen een [site van Hyper-V en Azure](site-recovery-hyper-v-site-to-azure.md) u de mogelijkheid om te failback van Azure naar moet een alternatief on-premises implementatie locatie. Dit is handig als u nodig hebt voor het instellen van nieuwe on-premises implementatie hardware. Hier ziet u hoe u dit doet.

1. Als u nieuwe hardware instelt Installeer Windows Server 2012 R2 en de functie Hyper-V op de server.
2. Een schakeloptie virtueel netwerk maken met dezelfde naam die u had op de oorspronkelijke server.
3. Selecteer **Items beveiligde** -> **Groep beveiliging**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> moet worden mislukt terug en selecteer **Geplande Failover**.
4. Selecteer in de **Geplande Failover bevestigen** **maken on-premises implementatie VM als deze nog niet bestaat**. 
5. Selecteer de nieuwe Hyper-V hostserver waarop u wilt plaatsen van de virtuele machine in **Host Name** .
6. Selecteer in de synchronisatie van gegevens die wordt u aangeraden de optie **synchroniseren de gegevens voordat u de overname**. Hiermee wordt de downtime voor virtuele machines geminimaliseerd tijdens het synchroniseren van deze zonder af te sluiten. Dit gebeurt het volgende:

    - Fase 1: Maakt momentopname van de virtuele machine in Azure wordt aangegeven en gekopieerd naar de on-premises implementatie Hyper-V-host. De computer blijft uitgevoerd in Azure wordt aangegeven.
    - Fase 2: Uitgeschakeld de virtuele machine in Azure wordt aangegeven, zodat er geen nieuwe wijzigingen worden uitgevoerd. De laatste set wijzigingen worden overgebracht naar de on-premises implementatie-server en de on-premises implementatie virtuele machine wordt gestart.
    
7. Klik op het vinkje om te beginnen met de overname (failback).
8. Nadat u bent de virtuele machine in Azure afsluiten en de eerste synchronisatie is voltooid, klikt u op **taken** > <planned failover job> > **Voltooid Failover**. Hiermee de Azure computer afgesloten, brengt de meest recente wijzigingen naar de on-premises implementatie virtuele machine en start deze.
9. U kunt zich aanmelden op de on-premises implementatie virtuele machine om te controleren of dat alles werkt zoals verwacht. Klik vervolgens op **doorvoeren** om te voltooien van de overname.
10. Klik op **Omkeren repliceren** om te beginnen met het beveiligen van de on-premises implementatie virtuele machine.

    >[AZURE.NOTE] Als u de taak failback opzegt tijdens deze stap van de synchronisatie van gegevens, wordt de VM on-premises implementatie, worden in een beschadigd staat. Dit komt doordat kopieën van de portal gegevens de meest recente gegevens van Azure VM schijven naar de on-premises gegevensschijven en kun meer totdat de synchronisatie is voltooid, de gegevens mogelijk niet in een consistente status schijf. Als de On-premises VM wordt opgestart nadat de portal gegevens is geannuleerd, kan deze niet opstarten. Failover om te voltooien van de portal gegevens opnieuw geactiveerd.
 
