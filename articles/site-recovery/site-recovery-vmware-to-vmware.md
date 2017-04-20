<properties
    pageTitle="Repliceren on-premises implementatie VMware virtuele machines of fysieke servers aan een secundaire site | Microsoft Azure"
    description="Lees dit artikel worden gerepliceerd VMware VMs of Windows/Linux fysieke servers met een secundaire site met Azure sites worden hersteld."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>On-premises implementatie VMware virtuele machines of fysieke servers repliceren naar een secundaire site


## <a name="overview"></a>Overzicht

InMage Scout bij het herstellen van Azure Site biedt realtime replicatie tussen on-premises implementatie VMware sites. InMage Scout deel uitmaakt van Azure Site herstel service-abonnementen.


## <a name="prerequisites"></a>Vereisten voor

**Azure-account**: U moet een [Microsoft Azure](https://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.


## <a name="step-1-create-a-vault"></a>Stap 1: Maak een kluis
1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Klik op Nieuw > Beheer > back-up- en Site-herstel (OMS). U kunt ook kunt u Bladeren > herstel Services kluis > toevoegen.
3. Geef in het vak **naam** een beschrijvende naam voor de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. Maak een nieuwe resourcegroep in **resourcegroep** of Selecteer een bestaande eigenschap. Geef een Azure gebied om te voltooien verplichte velden. 
5.  Selecteer in de **locatie**, de geografische regio voor de kluis. Als u wilt controleren of er ondersteunde regio's, Zie [Azure Site herstel prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Als u wilt snel toegang wordt de kluis vanuit het Dashboard klikt u op vastmaken aan dashboard en klik vervolgens op maken.
6. De nieuwe kluis wordt weergegeven op het Dashboard > alle bronnen, en klik op de belangrijkste herstel Services kluizen blade.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Stap 2: De kluis configureren en InMage Scout onderdelen downloaden
7. Klik in het herstelproces is Services kluizen blad uw kluis en klik op instellingen.
8. In **Instellingen** > **Aan de slag** Klik op **Site-herstel** > stap 1: **Voorbereiden infrastructuur** > **beveiliging doel**.
9. Selecteer bij herstel site in **beveiliging doel** en selecteer Ja, met VMware vSphere Hypervisor. Klik vervolgens op OK.
10. Klik op software- en registratiegegevens-code van InMage Scout 8.0.1 GA downloaden om te downloaden in het **Scout-instelling**. De installatiebestanden voor alle vereiste onderdelen zijn in het gedownloade ZIP-bestand.


## <a name="step-3-install-component-updates"></a>Stap 3: Onderdeel updates installeren

Lees meer over de meest recente [updates](#updates). Installeert u de updatebestanden op servers in de volgende volgorde:

1. RX server als er een
2. Configuratieservers
3. Proces servers
3. Basispagina doelservers
4. vContinuum servers
5. Bronserver (Windows en Linux Server)

Als volgt te werk om de updates te installeren:

1. Download het bestand .zip [bijwerken](https://aka.ms/asr-scout-update4) . Dit zip-bestand bevat de volgende bestanden:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 bits voor RHEL5, OL5, OL6, SUSE 10, 11 SUSE: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Pak de ZIP-bestanden.<br>
3. **Server voor de RX**: kopie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** op de server RX en pak het bestand. Klik in de map opgehaalde **uitvoeren/installeren**.<br>
4. **Voor de server/proces configuratieserver**: kopie **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** bij de configuratieserver en processerver. Dubbelklik op uit te voeren.<br>
5. **Voor de Windows basispagina doelserver**: als u de gecombineerde agent bijwerken, **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** naar de basispagina doelserver te kopiëren. Dubbelklik op deze uit te voeren. Houd er rekening mee dat de gecombineerde-agent ook van toepassing op de bronserver is. U moet deze installeren op de bronserver en, zoals verderop in deze lijst is vermeld.<br>
7. **Voor de server vContinuum**: **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** kopiëren naar de server vContinuum.  Zorg ervoor dat u de wizard vContinuum hebt gesloten. Dubbelklik op het bestand uit te voeren.<br>
8. **Voor het Linux basispagina doelserver**: als u de gecombineerde agent bijwerken, **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** naar de basispagina doelserver kopiëren en deze extraheren. Klik in de map opgehaalde **uitvoeren/installeren**.<br>
9. **Voor de Windows server gegevensbron**: als u de gecombineerde agent bijwerken, kopieert u **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** met de bronserver. Dubbelklik op deze uit te voeren.<br>
10. **Voor het Linux bronserver**: als u de gecombineerde agent bijwerken, overeenkomstige versie van UA bestand kopiëren naar de server Linux en pak het bestand. Klik in de map opgehaalde **uitvoeren/installeren**.  Voorbeeld: RHEL 6,7 64 bits server **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** kopiëren naar de server en deze ophalen. Klik in de map opgehaalde **uitvoeren/installeren**.

## <a name="step-4-set-up-replication"></a>Stap 4: Herhaling instellen
1. Replicatie tussen de bron instellen en afstemmen VMware sites.
2. Voor hulp, gebruikt u de documentatie InMage Scout die wordt gedownload met het product. U kunt ook de documentatie als volgt openen:

    - [Releaseopmerkingen](https://aka.ms/asr-scout-release-notes)
    - [Compatibiliteit matrix](https://aka.ms/asr-scout-cm)
    - [Gebruikershandleiding](https://aka.ms/asr-scout-user-guide)
    - [De gebruikershandleiding RX](https://aka.ms/asr-scout-rx-user-guide)
    - [Handleiding voor snelle installatie](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Updates

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site herstel Scout 8.0.1 Update 4
Scout Update 4 is een cumulatieve update. Alle correcties van update1 kassa update3 en volgende correcties nieuwe en verbeterde heeft.

**Nieuwe platformondersteuning** 

- Ondersteuning is voor vCenter/vSphere 6.0, 6.1 en 6.2 toegevoegd
- Ondersteuning is toegevoegd voor volgende Linux-besturingssystemen
    - Rode rol Enterprise Linux (RHEL) 7.0, 7.1 en 7.2 
    - CentOS 7.0, 7.1 en 7.2
    - Rode rol Enterprise Linux (RHEL) 6,8
    - CentOS 6,8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-bits **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** wordt geleverd met grondtal Scout GA pakket **InMage_Scout_Standard_8.0.1 GA.zip**. Downloaden Scout GA vanuit portal zoals vermeld in [stap 1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault).

**Correcties en verbeteringen** 

- Verbeterde afsluiten afhandelen voor volgende Linux OSes en klonen om ongewenste opnieuw synchroniseren problemen te voorkomen.
    - Rode rol Enterprise Linux (RHEL) 6.x
    - Oracle Linux (Outlook) 6.x
- Voor Linux, voert u de machtigingen in geïntegreerd agent installatiemap nu alleen op de lokale gebruiker beperkt zijn toegang tot een map.
- In Windows geladen time-out probleem tijdens het algemene verdeelde consistentie adresboek markeren intensief op uitgifte gedistribueerde toepassingen zoals SQL versus netwerkshare clusters.
- Toegevoegde log gerelateerde fix in CX grondtal installer.
- VMware vCLI 6.0 downloadkoppeling wordt toegevoegd aan het doel van de Windows-outmodel grondtal installer.
- Meer controles en logboeken voor wijzigingen in het netwerk configuraties tijdens failover en DR boren toegevoegd.
- Sometime bewaarbeleid informatie is niet aan de CX gemeld.  
- Volume bewerking formaat wijzigen tot en met de wizard vContinuum is voor fysieke cluster verbroken wanneer bron volume verkleinen is er gebeurd.
- Cluster beveiliging is mislukt 'Kan de schijfhandtekening vinden' wanneer cluster PRDM schijf betreft.
- cxps transport server vastlopen vanwege uitzondering buiten het bereik. 
- De servernaam van de en IP-kolommen zijn nu grootte in de pagina voor het installeren van de wizard vContinuum push.
- RX API-uitbreidingen
    - Biedt vijf meest recente beschikbaar algemene consistentie punten (alleen gegarandeerd tags).
    - Capaciteit en details van de beschikbare ruimte biedt voor alle beveiligde apparaten.
    - Biedt Scout stuurprogramma staat op bronserver. 
    
>[AZURE.NOTE] 
>
>- **InMage_Scout_Standard_8.0.1_GA.zip** grondtal pakket heeft nu CX grondtal installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** en Windows-outmodel doelsites grondtal installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**bijgewerkt. Gebruik de nieuwe CX en Windows-outmodel doelsites GA bits voor alle nieuwe installatie.
>- Update 4 rechtstreeks kan worden toegepast op 8.0.1 GA.
>- De configuratieserver en RX updates kunnen niet worden hersteld nadat deze worden toegepast op het systeem.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site herstel Scout 8.0.1 Update 3
Update 3 bevat de volgende correcties en verbeteringen:

- De configuratieserver en RX mislukt registreren om de Site herstel wanneer ze zich achter de proxy.
- Het aantal uren dat de herstel punt doelstelling (vrijgegeven Productieorder) niet is voldaan is niet bijgewerkt in het statusrapport.
- De configuratieserver wordt niet gesynchroniseerd met RX wanneer de gegevens van ESX hardware of netwerkgegevens UTF-8 tekens bevatten.
- Windows Server 2008 R2-domeincontrollers niet kunnen worden opgestart na herstel.
- Offline synchronisatie werkt niet zoals verwacht.
- Na een virtuele machine (VM) failover herhaling paren verwijdering lange tijd in de UI CX blijft staan en gebruikers niet kunnen voltooien van de failback of hervatten.
- Algemene verbroken momentopname bewerkingen die worden uitgevoerd door de consistentie taak geoptimaliseerd zijn om te beperken toepassing zoals SQL-clients.
- De prestaties van het hulpmiddel consistentie (VACP.exe) is verbeterd doordat de geheugengebruik die is vereist voor het maken van momentopnamen in Windows.
- De push installeren service loopt vast wanneer het wachtwoord groter is dan 16 tekens.
- vContinuum is niet controleren en vragen om nieuwe vCenter referenties wanneer de referenties worden gewijzigd.
- Op Linux is basispagina doel Cachebeheer (cachemgr) niet downloaden van bestanden van de processerver, waardoor herhaling paar beperken.
- Wanneer u de volgorde van fysieke failover cluster (MSCS) schijf is niet hetzelfde zijn op alle knooppunten, wordt replicatie is niet ingesteld voor een deel van de cluster hoeveelheden.
<br/>Houd er rekening mee dat het cluster moet worden reprotected om te profiteren van deze correctie.  
- SMTP-functionaliteit werkt niet zoals verwacht nadat RX wordt bijgewerkt van Scout 7.1 naar Scout 8.0.1.
- Meer stat zijn toegevoegd in het logboek voor de bewerking ongedaan maken voor het bijhouden van de tijd die deze bevat die u hebt gemaakt om deze te voltooien.
- Ondersteuning is voor besturingssystemen Linux op de bronserver toegevoegd:
    - Rood rol Enterprise Linux (RHEL) 6 update 7
    - CentOS 6 bijwerken 7
- De CX en RX UI kunt u nu de melding voor de paar, die tot in bitmapmodus doorloopt weergeven.
- De volgende beveiligingscorrecties zijn toegevoegd in RX:

**Beschrijving van probleem**|**Implementatie procedures**
---|---
Autorisatie kan overslaan via parameter wordt geknoeid|Beperkte toegang tot de niet-toepasselijke gebruikers.
Meerdere sites verzoek voorkoming|Het concept van pagina-token, dat willekeurig wordt gegenereerd voor elke pagina geïmplementeerd. <br/>Hiermee worden er: <li> Er is slechts één aanmeldingsproblemen exemplaar voor dezelfde gebruiker.</li><li>Pagina vernieuwen werkt niet--deze worden omgeleid naar het dashboard.</li>
Schadelijk bestand uploaden|Beperkte bestanden naar bepaalde uitbreidingen. Toegestane extensies zijn: 7z, aiff, AVP, avi, bmp csv, document, docx, FLA-, FLV-, GIF-bestand, gz, gzip, jpeg, jpg-, log, deel, mov, MP3-bestand, mp4, mpc, mpeg, mpg, ods, odt, PDF-, PNG-, PowerPoint, pptx, pxd, qt, ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, SWF-, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml, en zip.
Permanente meerdere sites uitvoeren van scripts | Invoer validatie toegevoegd.


>[AZURE.NOTE]
>
>-  Alle Site-herstel-updates zijn cumulatief. Update 3 heeft alle correcties van Update 1 en 2 van de Update. Update 3 rechtstreeks kan worden toegepast op 8.0.1 GA.
>-  De configuratieserver en RX updates kunnen niet worden hersteld nadat deze worden toegepast op het systeem.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site herstel Scout 8.0.1 Update 2 (bijwerken 03 Dec 15)

Correcties in Update 2, waaronder:

- **Configuratieserver**: oplossen voor een probleem dat de functie 31 dagen gratis meting niet werkt zoals verwacht wanneer de configuratieserver is geregistreerd bij het herstellen van de Site.
- **Geïntegreerde agent**: oplossen bij een probleem in Update 1, waardoor de update niet worden geïnstalleerd op de basispagina doelserver wanneer deze versie 8.0 naar 8.0.1 is bijgewerkt.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site herstel Scout 8.0.1 Update 1

Update 1 bevat de volgende correcties en de nieuwe functies:

- 31 dagen van de gratis beveiliging per server-instantie. Hiermee kunt u testen van functionaliteit of een bewijs-van-concept in te stellen.
    - Alle bewerkingen op de server, inclusief failover en foutherstel, zijn gratis voor de eerste 31 dagen, beginnend vanaf het moment dat een server eerst met Site herstel Scout is beveiligd.
    - Vanaf de 32nd dag vanaf worden elke beveiligde server standard exemplaar snelheid voor de beveiliging van Azure Site herstel aan een site voor klanten die zelf berekend.
    - Op elk gewenst moment is het aantal beveiligde servers die zijn momenteel in rekening wordt gebracht beschikbaar op de pagina Dashboard van de kluis Azure sites worden hersteld.
- Ondersteuning toegevoegd voor vSphere opdrachtregel-Interface (vCLI) 5,5 Update 2.
- Ondersteuning voor besturingssystemen Linux op de bronserver toegevoegd:
    - RHEL 6 6 bijwerken
    - RHEL 5 11 bijwerken
    - CentOS 6 Update 6
    - CentOS 5 Update 11
- Fout worden opgelost aan de volgende kwesties:
    - Kluis registratie mislukt voor de configuratieserver of RX-server.
    - Clustervolumes worden niet weergegeven zoals verwacht wanneer geclusterde virtuele machines zijn reprotected wanneer ze weer.
    - Failback mislukt wanneer de basispagina doelserver wordt gehost op een andere ESXi-server uit de on-premises implementatie productie virtuele machines.
    - Configuratie van bestandsmachtigingen worden gewijzigd wanneer u een upgrade uitvoert naar 8.0.1, die van invloed op beveiliging en bedrijfsactiviteiten.
    - De drempelwaarde voor het opnieuw niet afgedwongen zoals verwacht, die leidt tot inconsistente herhaling gedrag.
    - De vrijgegeven Productieorder-instellingen worden niet correct weergegeven in de configuratie server-interface. De waarde niet-gecomprimeerde gegevens onjuist worden gegroepeerd ziet u de gecomprimeerde waarde.
    -  De verwijderbewerking verwijdert, wordt niet zoals verwacht in de wizard vContinuum en replicatie wordt niet verwijderd uit de configuratie server-interface.
    -  Klik in de wizard vContinuum is de schijf automatisch uitgeschakeld wanneer u op **Details** in de weergave schijf tijdens de beveiliging van MSCS virtuele machines klikt.
    - Tijdens de fysieke en de virtuele scenario (P2V) worden niet vereist HP-services, zoals CIMnotify en CqMgHost, verplaatst naar handmatig bij het herstellen van virtuele machine. Hierdoor extra opstarten.
    - Linux VM beveiliging mislukt als er meer dan 26 schijven op de basispagina doelserver.

## <a name="next-steps"></a>Volgende stappen

Vragen die u op het [forum van Azure herstel Services hebt](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.
