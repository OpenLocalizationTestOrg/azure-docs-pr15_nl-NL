<properties
    pageTitle="Prestaties Monitor oplossing in OMS netwerk | Microsoft Azure"
    description="Network prestaties Monitor Hiermee kunt u de prestaties van uw-netwerken te gebruiken in vrijwel real-permanente naar controleren detecteren en zoek naar netwerk prestatieproblemen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Prestaties Monitor (Preview)-oplossing in OMS netwerk

>[AZURE.NOTE] Dit is een [voorbeeld-oplossing](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

In dit document wordt beschreven hoe u instellen en gebruiken de oplossing Network prestaties Monitor in OMS, waarin u de prestaties van uw-netwerken te gebruiken in vrijwel real-permanente naar controleren het opsporen en zoek prestatieproblemen netwerkverbindingen. U kunt de winst- en latentie tussen twee netwerken, subnetten of servers controleren met de oplossing Network prestaties Monitor. Monitor met een netwerk prestaties vastgesteld netwerkproblemen zoals verkeer blackholing, routeren fouten en problemen die de controle methoden gebruikelijke netwerk niet kunnen detecteren. Network prestaties Monitor genereert waarschuwingen en krijgt zoals en wanneer een drempel voor de netwerkkoppeling naar wordt gekraakt. Deze drempels automatisch kunnen worden geleerd door het systeem of kunt u ze als u wilt gebruiken, aangepaste waarschuwingsregels configureren. Monitor met een netwerk prestaties zorgt ervoor dat tijdig detectie van problemen met de netwerkprestaties en de bron van het probleem aan een bepaald netwerksegment of het apparaat is vertaald.

Netwerkproblemen met het dashboard oplossing waarin samengevatte informatie over uw netwerk inclusief recente netwerk systeemstatus gebeurtenissen, beschadigd netwerkkoppelingen en subnetwerk koppelingen die die tegenover elkaar hoog pakketverlies en latentie liggen zijn wordt weergegeven, kunt u detecteren. U kunt inzoomen op een netwerkkoppeling naar de huidige status van subnetwerk koppelingen, evenals de koppelingen naar knooppunten weergeven. U kunt ook de historische trend van verlies en latentie bij het netwerk, subnetwerk en naar knooppunten niveau weergeven. U kunt tijdelijke netwerkproblemen door te bekijken van historische trendgrafieken voor pakketverlies en latentie detecteren en zoek naar netwerkknelpunten op een kaart topologie. In de grafiek interactieve topologie kunt u de netwerkroutes hop door hop visualiseren en de bron van het probleem te bepalen. Zoals elke andere oplossingen, kunt u Log zoeken naar verschillende analytics vereisten voor het maken van aangepaste rapporten op basis van de gegevens die worden verzameld door netwerk prestaties Monitor.

De oplossing wordt synthetische transacties gebruikt als primaire om naar het netwerkfouten opsporen. Zo is, kunt u deze gebruiken zonder te letten op de leverancier of het model van een specifieke netwerkapparaat. Dit werkt voor on-premises implementatie, cloud (IaaS) en hybride omgevingen. De oplossing detecteert automatisch de netwerktopologie en verschillende routes in uw netwerk.

Normale netwerk controleren producten richten op het controleren van de status van netwerk-apparaat (routers, schakelopties enz.) maar geen inzicht krijgen in de werkelijke kwaliteit van de netwerkverbindingen tussen twee punten, zoals Network prestaties Monitor.

### <a name="using-the-solution-standalone"></a>Gebruik van de zelfstandige versie van de oplossing

Als u controleren van de kwaliteit van de netwerkverbindingen tussen hun kritieke werkbelasting wilt, kunt netwerken, datacenters of office-sites en u de oplossing Network prestaties Monitor zelfstandig gebruiken om de status van de connectiviteit tussen te houden:

- meerdere datacenters of office-sites die zijn verbonden met een openbaar of privé-netwerk
- kritieke werkbelasting met bedrijfstoepassingen
- openbare cloudservices, zoals Microsoft Azure of Amazon Web Services (AWS) en on-premises implementatie-netwerken te gebruiken, als u IaaS (VM) beschikbaar hebt en u hebt gateways geconfigureerd voor communicatie tussen on-premises implementatie-netwerken te gebruiken en cloud-netwerken te gebruiken
- Azure en on-premises-netwerken te gebruiken wanneer u Express Route gebruikt

### <a name="using-the-solution-with-other-networking-tools"></a>Gebruik van de oplossing met andere netwerken hulpmiddelen

Als u controleren van een aantal zakelijke toepassingen wilt, kunt u de oplossing Network prestaties Monitor als een oplossing companion van andere netwerkhulpprogramma's. Een traag netwerk kan leiden tot traag toepassingen en netwerk prestaties kunt bekijken, kunt u onderzoek prestatieproblemen met de toepassing die worden veroorzaakt door onderliggende netwerkproblemen. Omdat de oplossing niet geen toegang hebben tot netwerkapparaten hoeft, moet de beheerder van de toepassing niet afhankelijk zijn van een team netwerken bevatten informatie over hoe het netwerk is voor de toepassingen.

Ook als u al in andere hulpprogramma's voor controle netwerk investeren, kunt klikt u vervolgens de oplossing vullen deze extra omdat meest traditionele netwerk controle kan worden uitgevoerd, niet inzicht krijgen in de prestatiegegevens end-to-end-netwerk zoals verlies en latentie leveren.  De oplossing Network prestaties Monitor kunt doorvoeren die tussenruimte.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Installeren en configureren van agenten voor de oplossing

Gebruik de basic-processen voor het installeren van agenten op [verbinding maken met Windows-computers Log Analytics](log-analytics-windows-agents.md) en [Verbinding Operations Manager naar Log Analytics](log-analytics-om-agents.md).

>[AZURE.NOTE]
U moet ten minste 2 agenten installeren om te kunnen onvoldoende gegevens om te detecteren en controleren van uw netwerk resources. Anders blijft de oplossing configureren status totdat installeert en configureert u aanvullende agenten.

### <a name="where-to-install-the-agents"></a>Waar u de agenten installeert

Voordat u agenten hebt geïnstalleerd, kunt u de topologie van uw netwerk en welke delen van het netwerk dat u wilt controleren. We raden u meer dan één agent voor elk subnet die u wilt controleren te installeren. Met andere woorden, voor elke subnet dat u wilt controleren, kiest u twee of meer servers of VMs en de agent installeren op deze.

Als u wat de topologie van uw netwerk weet, installeert u de agenten op servers met kritieke werkbelasting waar u wilt controleren van de prestaties van het netwerk. U wilt bijvoorbeeld bijhouden van een netwerkverbinding tussen een webserver en een server met SQL Server. In dit voorbeeld zou u een agent installeren op beide servers.

Agenten bewaken netwerkconnectiviteit (koppelingen) tussen hosts--niet de hosts zelf. Zo is, moet u agenten installeren op beide eindpunten van deze koppeling om de netwerkkoppeling, te houden.

### <a name="configure-agents"></a>Agenten configureren

Nadat u agenten hebt geïnstalleerd, moet u open firewallpoorten voor die computers om ervoor te zorgen dat agenten kunnen communiceren. U moet downloaden en voer het [EnableRules.ps1 PowerShell-script](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) zonder parameters in een PowerShell-venster met beheerdersbevoegdheden

Het script registersleutels staat het beeldscherm van de prestaties netwerk vereist gemaakt en dat wordt gemaakt van Windows firewallregels voor het toestaan van agenten TCP-verbindingen met elkaar te maken. De registersleutels gemaakt door het script ook opgeven of de logboeken voor foutopsporing en het pad voor het bestand logboeken aanmelden. Deze definieert ook de agent TCP-poorten gebruikt voor communicatie. De waarden voor deze toetsen worden automatisch ingesteld door het script, zodat u deze toetsen moet niet handmatig wijzigen.

De poort standaard geopend is 8084. U kunt een aangepaste poort doordat de parameter `portNumber` aan het script. Echter moet dezelfde poort worden gebruikt op alle computers waarop het script wordt uitgevoerd.

>[AZURE.NOTE] Het script EnableRules.ps1 configureert Windows firewallregels alleen op de computer waarop het script wordt uitgevoerd. Als u een netwerkfirewall hebt, moet u ervoor zorgen dat kan verkeer voor de TCP-poorten door Network prestaties Monitor wordt gebruikt.


## <a name="configuring-the-solution"></a>Configuratie van de oplossing

Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

1. De oplossing Network prestaties Monitor krijgt gegevens uit computers met Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger gebruikt, zijn de dezelfde vereisten als de Microsoft Monitoring Agent MMA ().
2. De oplossing Network prestaties Monitor toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  
  ![Symbool voor netwerk-prestaties controleren](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Klik in de portal OMS ziet u een nieuwe tegel **Network prestaties Monitor** getiteld met het bericht *oplossing is aanvullende configuratie vereist*. U moet de oplossing om toe te voegen netwerken op basis van subnetwerken en knooppunten die zijn gevonden door agenten configureren. Klik op **Netwerk prestaties controleren** om te beginnen met het configureren van de standaard-netwerk.  
  ![oplossing is aanvullende configuratie vereist.](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>De oplossing configureren met een standaardnetwerk

Klik op de configuratiepagina ziet u één netwerk met de naam **standaard**. Wanneer u geen netwerken nog niet hebt gedefinieerd, worden alle subnetten automatisch gedetecteerd in de standaard-netwerk geplaatst.

Wanneer u een netwerk maakt, wordt u er een subnet aan toevoegen en dat subnet wordt verwijderd uit de standaard-netwerk. Als u een netwerk verwijdert, worden automatisch alle bijbehorende subnetten bij het netwerk standaard geretourneerd.

De standaard-netwerk is met andere woorden, de container voor alle subnetten die zich niet in een netwerk door gebruiker gedefinieerd. U kunt geen bewerken of verwijderen van de standaard-netwerk. Het blijft altijd in het systeem. U kunt echter zoveel netwerken naar wens maken.

In de meeste gevallen wordt de subnetten in uw organisatie worden gerangschikt in meer dan één netwerk en moet u een of meer netwerken als u wilt groeperen logisch uw subnetten maken.

### <a name="create-new-networks"></a>Maken van nieuwe netwerken

Een netwerk met Network prestaties Monitor is een container voor subnetten. U kunt een netwerk maken met een naam die u wilt en subnetten aan het netwerk toevoegen. Zoals u kunt maken van een netwerk met de naam *Building1* en voegt u subnetten, of u kunt maken van een netwerk met de naam *DMZ* en voegt u alle subnetten die behoren tot gedemilitariseerde zone met dit netwerk.

#### <a name="to-create-a-new-network"></a>Een nieuw netwerk maken

1. Klik op **netwerk toevoegen** en typ de naam en beschrijving.
2.  Selecteer een of meer subnetten en klik vervolgens op **toevoegen**.
3. Klik op **Opslaan** om op te slaan de configuratie.  
  ![netwerk toevoegen](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Wacht totdat het samenvoegen van gegevens

Nadat u de configuratie voor de eerste keer hebt opgeslagen, wordt de oplossing gestart pakket verlies en latentie Netwerkgegevens verzamelen tussen de knooppunten waarop agenten zijn geïnstalleerd. Dit proces kan het enige tijd duren, soms 30 minuten. Tijdens deze status wordt de tegel Network prestaties Monitor in de overzichtspagina van een bericht weergegeven *het samenvoegen van de gegevens in het proces*.

![gegevens aggregatie Bezig](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Wanneer de gegevens is geüpload, ziet u het netwerk prestaties Monitor tegel bijgewerkt waarin gegevens.

![Tegel voor netwerk-prestaties controleren](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klik op de tegel om het netwerk prestaties Monitor dashboard weer te geven.

![Netwerk prestaties Monitor dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Controle-instellingen voor subnetten bewerken

Alle subnetten waarin ten minste één agent is geïnstalleerd, worden weergegeven op het tabblad **subnetwerken** in de configuratiepagina.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>-Of uitschakelen voor bepaalde subnetwerken bewaken

1. Selecteer of het selectievakje uit naast de **subnetwerk-ID** en klik vervolgens ervoor zorgen dat **wordt gebruikt voor controle** ingeschakeld of uitgeschakeld, passende is. U kunt selecteren of meerdere subnetten uitschakelen. Als uitgeschakeld, worden niet subnetwerken gecontroleerd, zoals de agenten wordt bijgewerkt als u wilt stoppen met het pingen andere gemachtigden.
2. Kies de knooppunten die u voor een bepaalde subnetwerk door het subnetwerk in de lijst te selecteren en de vereiste knooppunten te verplaatsen tussen de lijsten met niet-beheerde en gecontroleerde knooppunten wilt controleren.
U kunt een aangepaste **Beschrijving** als u wilt toevoegen aan het subnetwerk.
3. Klik op **Opslaan** om op te slaan de configuratie.  
  ![subnet bewerken](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Kies knooppunten om te controleren

Alle knooppunten waarvoor een agent die is geïnstalleerd op deze worden weergegeven op het tabblad **knooppunten** .

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>-Of uitschakelen voor knooppunten bewaken

1. Schakel de knooppunten die u wilt controleren of niet meer wordt gecontroleerd of uit.
2. Klik op **gebruiken voor controle**, in- of uitgeschakeld.
3. Klik op **Opslaan**.  
  ![knooppunt controle inschakelen](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Set regels bewaken

Monitor met een netwerk prestaties genereert systeemstatus gebeurtenissen over de connectiviteit tussen een paar knooppunten of subnetwerk of netwerk koppelingen wanneer een drempel wordt gekraakt. Deze drempels automatisch kunnen worden geleerd door het systeem of u ze kunt configureren, kunt u aangepaste waarschuwingsregels.

De *standaard regel* is gemaakt door het systeem en dat de drempelwaarde voor systeem geleerd wordt een gebeurtenis systeemstatus wanneer inbreuk op verlies of latentie tussen elk paar-netwerken te gebruiken of subnetwerk koppelingen gemaakt. U kunt kiezen om te schakelen van de standaardregel en aangepaste controleren regels maken

#### <a name="to-create-custom-monitoring-rules"></a>Aangepaste controle regels maken

1. Klik op **Regel toevoegen** in het tabblad **Beeldscherm** en voer de naam en beschrijving.
2. Selecteer de set netwerk- of subnetwerk koppelingen om te controleren in de lijsten.
3. Selecteert u eerst het netwerk waarin het eerste subnetwerk/s van belang is opgenomen in de vervolgkeuzelijst netwerk en selecteer vervolgens de subnetwerk (s) in de bijbehorende subnetwerk vervolgkeuzelijst.
Selecteer **alle subnetwerken** als u wilt controleren van alle subnetwerken in een netwerkkoppeling. Selecteer op dezelfde manier de andere subnetwerk/s belangrijke. En u kunt klikken op **Uitzondering toevoegen** als u wilt uitsluiten monitoring voor bepaalde subnetwerk koppelingen uit de selectie die u hebt aangebracht.
4. Als u niet wilt maken van systeemstatus-gebeurtenissen voor de items die u hebt geselecteerd, schakelt u **controle op de koppelingen bestrijkt deze regel van de gezondheid inschakelen**.
5. Kies voorwaarden cmdlets voor controle.
U kunt aangepaste drempelwaarden voor het genereren van de gebeurtenis status instellen door te typen drempelwaarden. Wanneer de waarde van de voorwaarde boven de geselecteerde drempel voor de geselecteerde netwerk/subnetwerk paar gaat, wordt een gebeurtenis status gegenereerd.
6. Klik op **Opslaan** om op te slaan de configuratie.  
  ![Aangepaste controle regel maken](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Detailgegevens van de siteverzameling

Monitor met een netwerk prestaties gebruikt TCP SYN-SYNACK-ACK handshake-pakketten voor het verzamelen van verlies en latentie informatie en traceroute wordt ook gebruikt om topologie informatie te verkrijgen.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor Network prestaties Monitor.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
| Windows |![Ja](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ja](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Nee](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Nee](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Nee](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP handshakes elke 5 seconden, gegevens verzonden elke 3 minuten |

De oplossing gebruik maakt van synthetische transacties te beoordelen van de status van het netwerk. OMS agenten geïnstalleerd op verschillende punt in het netwerk exchange TCP-pakketten met elkaar en klikt u in het proces, lees de retour-verlies van tijd en pakket, indien van toepassing. Elke agent voert regelmatig, ook een traceringsroute uit naar de andere gemachtigden om alle te vinden van de verschillende in het netwerk die moet worden getest. Deze gegevens gebruikt, zijn de agenten kunnen afleiden van het netwerklatentie en pakket verlies afbeeldingen. De tests worden herhaald elke vijf seconden en gegevens worden samengevoegd voor een periode van drie minuten door de agenten voordat u deze uploadt naar OMS.

>[AZURE.NOTE] Hoewel vaak agenten met elkaar communiceren, genereren ze geen een groot aantal netwerkverkeer tijdens de uitvoering van de test. Agenten vertrouwen alleen op TCP SYN-SYNACK-ACK handshake pakketten om te bepalen de winst- en latentie--geen gegevens pakketten worden uitgewisseld. Tijdens dit proces agenten met elkaar communiceren alleen wanneer nodig en de topologie van de communicatie agent is geoptimaliseerd voor de netwerkverkeer te verminderen.

## <a name="using-the-solution"></a>Gebruik van de oplossing

In deze sectie worden alle het dashboard functies en hoe ze worden gebruikt.

### <a name="solution-overview-tile"></a>Oplossing overzicht tegel

Wanneer u de oplossing Network prestaties Monitor hebt ingeschakeld, biedt de tegel oplossing op de pagina overzicht van de OMS een kort overzicht van de status van het netwerk. Een ringdiagram met het nummer van de orde en beschadigd subnetwerk koppelingen wordt weergegeven. Wanneer u op de tegel hebt geklikt, wordt het dashboard oplossing geopend.

![Tegel voor netwerk-prestaties controleren](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Netwerk prestaties Monitor oplossing dashboard

Het **Netwerk samenvatting** blad bevat een overzicht van de netwerken samen met de relatieve grootte. Dit wordt gevolgd door de tegels met totale aantal netwerkkoppelingen, subnet koppelingen en paden in het systeem (een pad bestaat uit de IP-adressen van twee hosts met agenten en alle hops ertussen).

Het blad **Boven netwerk systeemstatus gebeurtenissen** vindt u een lijst met meest recente systeemstatus evenementen en waarschuwingen in het systeem en de tijd sinds de gebeurtenis actief is. Een gebeurtenis systeemstatus of waarschuwing ontvangen wordt gegenereerd wanneer de pakketverlies of latentie van een netwerk of subnetwerk koppeling een drempelwaarde overschrijdt.

Het blad **Boven beschadigd netwerkkoppelingen** ziet u een lijst met netwerkkoppelingen beschadigd. Dit zijn de netwerkkoppelingen met een of meer nadelige systeemstatus gebeurtenis voor deze op het moment dat.

De **Bovenste subnetwerk koppelingen met meest verlies** en **Subnetwerk koppelingen met meest latentie** bladen weergeven respectievelijk de bovenste subnetwerk koppelingen door pakketverlies en de bovenste subnetwerk koppelingen van latentie. Hoge latentie of bepaalde hoeveelheid pakketverlies verwachting op bepaalde netwerkkoppelingen. Deze koppelingen worden weergegeven in de top tien-lijsten, maar ook niet gemarkeerd beschadigd.

Het blad **Algemene query's** bevat een reeks zoekquery's die onbewerkte netwerk rechtstreeks-gegevens bewaken ophalen. U kunt deze query's gebruiken als uitgangspunt voor het maken van uw eigen query's voor aangepaste rapporten.

![Netwerk prestaties Monitor dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Een zoomfunctie voor diepte

U kunt klikken op verschillende koppelingen op de oplossing dashboard om een zoomfunctie grondigere in een interessegebied. Bijvoorbeeld wanneer u een waarschuwing of een beschadigde netwerkkoppeling worden weergegeven op het dashboard ziet, kunt u deze verder onderzoeken. U moet nemen aan een pagina waarin alle subnetwerk koppelingen voor de koppeling bepaald netwerk. Is mogelijk om de status verlies en latentie servicestatus van elke koppeling subnetwerk weer te geven en snel zoeken welke koppelingen subnetwerk het probleem veroorzaken. U kunt **weergave knooppunt koppelingen** om te zien van alle koppelingen knooppunt voor de koppeling beschadigd subnet klik vervolgens op. Vervolgens kunt u afzonderlijke naar knooppunten koppelingen weergeven en vindt u de koppelingen beschadigd knooppunt.

U kunt klikken op **weergave topologie** om weer te geven van de topologie hop door hop routes tussen de bron- en doeltabellen knooppunten. De beschadigde routes of hops worden weergegeven in het rood zodat u snel van het probleem aan een bepaald gedeelte van het netwerk identificeren kunt.

![Inzoomen op gegevens](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trendgrafieken

Op elk niveau dat u inzoomen, ziet u de trend van verlies en latentie voor de netwerkkoppeling van een. Trendgrafieken zijn ook beschikbaar voor subnetwerk en knooppunt koppelingen. U kunt het interval voor de grafiek wilt uitzetten met behulp van het besturingselement tijd boven aan de grafiek wijzigen.

Trendgrafieken weergeven u een historische perspectief van de prestaties van een netwerkkoppeling. Sommige netwerkproblemen tijdelijke van aard zijn, en is moeilijk te onderschept alleen door te kijken in de huidige status van het netwerk. Dit komt doordat problemen kunnen snel het oppervlak en verdwijnen voordat iedereen meldingen, alleen op een later opnieuw in tijd. Deze tijdelijke problemen kunnen ook lastig voor toepassingsbeheerders van de die vaak oppervlak problemen als onverklaarbare toename van de toepassing antwoord tijd, zelfs wanneer alle toepassingsonderdelen probleemloos weergegeven.

Eenvoudig kunt u deze soorten problemen detecteren door te kijken een trend grafiek waar het probleem in het netwerklatentie of pakketverlies als een plotselinge Prikker wordt weergegeven.

![grafiek met trendlijn](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Hop door hop topologie kaart

Network prestaties Monitor ziet u de topologie hop door hop van routes tussen twee knooppunten op een kaart interactieve topologie. U kunt de kaart topologie weergeven door een koppeling knooppunt te selecteren en vervolgens te klikken op **weergave topologie**. U kunt ook de kaart topologie weergeven door te klikken op de tegel **paden** op het dashboard. Wanneer u op het dashboard op **paden** klikt, moet u de bron- en doeltabellen knooppunten Selecteer in het deelvenster linkerpagina en klik vervolgens op **Tekenen** als u wilt uitzetten de routes tussen de twee knooppunten.

De topologie-kaart wordt weergegeven hoeveel routes zijn tussen de twee knooppunten en wat paden de gegevenspakketten uitvoeren. Netwerk prestatieknelpunten zijn gemarkeerd in het rood op de kaart topologie. U kunt een defect netwerkverbinding of een netwerkapparaat defect vinden door te kijken rode gekleurde elementen op de kaart topologie.

Wanneer u een knooppunt of plaats de muisaanwijzer eroverheen Klik op de kaart topologie, ziet u de eigenschappen van het knooppunt zoals FQDN en IP-adres. Klik op een hop om te zien is het IP-adres. U kunt bepaalde routes markeren door te schakelen en alleen de routes die u wilt markeren op de kaart te selecteren. U kunt in- of uitzoomen op de kaart topologie met behulp van uw muiswieltje inzoomen.

Houd er rekening mee dat de topologie wordt weergegeven in de map layer 3 topologie is en geen laag 2-apparaten en verbindingen.

![hop door hop topologie kaart](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Foutenstructuuranalyse lokalisatie

Network prestaties Monitor wordt de netwerkknelpunten zonder verbinding maken met de netwerkapparaten gevonden. Op basis van de gegevens die worden verzameld, van het netwerk en door het toepassen van geavanceerde algoritmen op de grafiek netwerk, maakt Network prestaties Monitor een probabilistische schatting van de onderdelen van het netwerk die waarschijnlijk de bron van het probleem.

Deze methode is handig om te bepalen de netwerkknelpunten wanneer toegang tot hops is niet beschikbaar omdat deze geen gegevens die worden verzameld uit de netwerkapparaten zoals routers of schakelopties nodig. Dit is ook handig als de hops tussen twee knooppunten niet in uw beheer zijn. De hops mogelijk bijvoorbeeld Internetprovider routers.

### <a name="log-analytics-search"></a>Meld u aan analyses zoeken

Alle gegevens die wordt weergegeven grafisch via het dashboard Network prestaties Monitor en inzoomen op pagina's is ook beschikbaar native in Log Analytics zoeken. U kunt de query de gegevens met de taal van de query zoeken en aangepaste rapporten maken met de gegevens exporteren naar Excel of PowerBI. Het blad **Algemene query's** in het dashboard bevat enkele nuttige query's die u als uitgangspunt gebruiken kunt voor het maken van uw eigen query's en rapporten.

![zoekopdrachten](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>De oorzaak van een melding voor een Servicestatus onderzoeken

Nu dat u hebt gelezen over netwerk prestaties beeldscherm, sluit u een eenvoudige onderzoek naar de onderliggende oorzaak voor een gebeurtenis systeemstatus nu behandeld.

1. Klik op de pagina overzicht krijgt u een snelle momentopname van de status van uw netwerk door de naleving van het **Netwerk prestaties Monitor** tegel. U ziet dat afmelden bij de 80 subnetwerken koppelingen worden gecontroleerd, 43 beschadigd. Dit garandeert onderzoek. Klik op de tegel om weer te geven van de oplossing dashboard.  
  ![Tegel voor netwerk-prestaties controleren](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. In de onderstaande voorbeeldafbeelding ziet u dat er momenteel 4 systeemstatus-evenementen en 4 netwerkkoppelingen die beschadigd zijn. U wilt onderzoeken het probleem en klik op de koppeling voor het netwerk van **Sharepoint op het Web** om vast te stellen de hoofdmap van het probleem.  
  ![Voorbeeld van de koppeling beschadigd netwerk](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. De pagina inzoomen ziet u alle subnetwerk koppelingen in **Sharepoint op het Web** netwerk koppelen. U ziet dat voor beide de subnetwerk-koppelingen, de latentie heeft Gekruiste de drempelwaarde voor de netwerkkoppeling beschadigd maken. U kunt ook de trends latentie beide subnetwerk koppelingen bekijken. U kunt de selectie tijd besturingselement in de grafiek kunt richten op het vereiste tijdsbereik. Hier ziet u de tijd van de dag wanneer latentie de piek heeft bereikt. Later kunt u de logboeken voor deze periode het probleem kunnen onderzoeken zoeken. Klik op **weergave knooppunt koppelingen** naar een zoomfunctie verder.  
  ![Voorbeeld van beschadigde subnet koppelingen](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Net als u naar de vorige pagina, omdat de pagina inzoomen op voor de koppeling bepaalde subnetwerk vermeldt omlaag bijbehorende koppelingen onderdeel knooppunt. U kunt dezelfde acties uitvoeren hier zoals in de vorige stap. Klik op **weergave topologie** om weer te geven van de topologie tussen de 2 knooppunten.  
  ![Voorbeeld van de koppelingen beschadigd knooppunten](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. Alle paden tussen de 2 geselecteerde knooppunten worden uitgezet in de map van de topologie. U kunt de hop door hop topologie van routes tussen twee knooppunten op de kaart topologie visualiseren. Hebt u een afbeelding wissen van hoeveel routes bestaan tussen de twee knooppunten en wat paden de gegevenspakketten duurt. Netwerk prestatieknelpunten zijn gemarkeerd in rode kleur. U kunt een defect netwerkverbinding of een netwerkapparaat defect vinden door te kijken rode gekleurde elementen op de kaart topologie.  
  ![Voorbeeld van de beschadigde topologie-weergave](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. De verlies, latentie, het aantal hops in elk pad kunt u lezen in het detailvenster **Pad** . In dit voorbeeld ziet u dat er 3 beschadigd paden zoals vermeld in het deelvenster. Gebruik de schuifbalk om de details van die beschadigd paden te bekijken.  Gebruik de selectievakjes in om te selecteren een van de paden zodat de topologie voor slechts één pad worden uitgezet. U kunt uw muiswieltje in-of uitzoomen op de kaart topologie.

  In de onderstaande afbeelding dat u de oorzaak van het probleem gebieden die u wilt de specifieke sectie van het netwerk duidelijk kunt zien door te kijken de paden en hops in rode kleur. Klikken op een knooppunt in de map topologie klikt, worden de eigenschappen van het knooppunt, inclusief de FQDN-naam, en IP-adres. Klikken op een hop, ziet u het IP-adres van de hop.  
  ![beschadigde topologie - pad details voorbeeld](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords te bekijken.
