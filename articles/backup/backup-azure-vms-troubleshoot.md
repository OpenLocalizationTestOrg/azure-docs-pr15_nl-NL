<properties
    pageTitle="Problemen met back-up van Azure virtuele machines | Microsoft Azure"
    description="Problemen met back-up en herstellen van Azure virtuele machines"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problemen met back-up van Azure virtuele machines

> [AZURE.SELECTOR]
- [Herstel services kluis](backup-azure-vms-troubleshoot.md)
- [Back-kluis](backup-azure-vms-troubleshoot-classic.md)

Fouten tijdens het gebruik van Azure back-up maken met gegevens weergegeven in de onderstaande tabel, kunt u oplossen.

## <a name="backup"></a>Back-up maken

| Back-up | Meer informatie over fout | Tijdelijke oplossing |
| -------- | -------- | -------|
|Back-up maken|    Kan de bewerking niet uitvoeren, zoals VM niet langer bestaat. -Stoppen VM beveiligen zonder back-gegevens te verwijderen. Meer informatie bij http://go.microsoft.com/fwlink/?LinkId=808124   |Dit gebeurt wanneer de primaire VM is verwijderd, maar het back-beleid blijft zien voor een VM back-up uitvoeren. Deze fout herstellen: <ol><li> Maak opnieuw de virtuele machine met dezelfde naam en dezelfde resource groepsnaam [cloud servicenaam]<br>(OR)</li><li> Stoppen met het beveiligen van virtuele machine met of zonder back-gegevens te verwijderen. [Meer informatie](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Back-up maken|Kan niet communiceren met de VM-agent voor momentopname status. -Zorg ervoor dat VM over internettoegang. Bovendien de VM-agent werk volgens de die worden genoemd in de gids voor probleemoplossing bij http://go.microsoft.com/fwlink/?LinkId=800034 |Deze fout wordt gegenereerd als er een probleem met de VM-Agent of netwerktoegang tot de Azure-infrastructuur is geblokkeerd in sommige manier. Lees meer informatie over foutopsporing omhoog VM momentopname problemen.<br> Als de VM-agent is niet wordt veroorzaakt door problemen, start u de VM. Soms kan een onjuiste VM status problemen veroorzaken en opnieuw starten van de VM herstelt deze 'niet goed'|
|Back-up maken|    Herstel services extensie bewerking is mislukt. -Zorg dat de meest recente VM agent zich in de virtuele machine bevindt en agent-service wordt uitgevoerd. Back-bewerking opnieuw en als dit mislukt, neem dan contact op met de ondersteuning van Microsoft.|   Deze fout wordt gegenereerd wanneer VM-agent verlopen is. Zie "De Agent VM bijwerken" hieronder de VM-agent bijwerken.|
|Back-up maken |VM bestaat niet. -Controleer of dat die virtuele machine bestaat of Selecteer een andere virtuele machine. | Dit gebeurt wanneer de primaire VM is verwijderd, maar het back-beleid blijft zien voor een VM back-up uitvoeren. Deze fout herstellen: <ol><li> Maak opnieuw de virtuele machine met dezelfde naam en dezelfde resource groepsnaam [cloud servicenaam]<br>(OR)<br></li><li>Stoppen met het beveiligen van virtuele machine zonder back-gegevens te verwijderen. [Meer informatie](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Back-up maken |De opdracht uitvoeren is mislukt. -Een andere bewerking is momenteel uitgevoerd op dit item. Wacht totdat de vorige bewerking is voltooid en probeer |Een bestaande back-up of terugzettaak voor VM wordt uitgevoerd en een nieuwe taak kan niet worden gestart, terwijl de bestaande taak wordt uitgevoerd.|
| Back-up maken | VHD's kopiëren uit back-kluis time-out - probeer de bewerking in een paar minuten. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support. | Dit gebeurt als er te veel gegevens wilt kopiëren. Controleer of als er minder dan 16 gegevensschijven. |
| Back-up maken | Back-up is mislukt met een interne fout: probeer de bewerking in een paar minuten. Als het probleem zich blijft voordoen, neemt u contact Microsoft Support | U kunt deze fout krijgt vanwege de 2: <ol><li> Er is een tijdelijk probleem bij het openen van VM opslag. Controleer [De Status van de Azure](https://azure.microsoft.com/en-us/status/) om te zien als er een voortdurende probleem met betrekking tot berekeningscluster/opslag/netwerk in de regio is. Neem opnieuw proberen het probleem met de back-bericht beperkt. <li>De oorspronkelijke VM is verwijderd en kunnen daarom back-up kan niet worden geplaatst. Als u wilt de back-upgegevens voor een verwijderde VM behouden, maar dat de back-fouten, de VM beveiliging en kies de optie om de gegevens te houden. Hiermee stopt u de back-planning en ook de terugkerende foutberichten. |
| Back-up maken | Installatie van de Azure herstel Services is mislukt-extensie voor het geselecteerde item - VM-Agent is spelen voor Azure herstel Services-extensie. Installeer de Azure VM-agent en de registratie-bewerking opnieuw starten | <ol> <li>Controleer als de VM-agent juist is geïnstalleerd. <li>Zorg ervoor dat de vlag op de zoekconfiguratie VM correct is ingesteld.</ol> [Lees meer](#validating-vm-agent-installation) over VM agent installatie en hoe u de installatie van de agent VM valideren. |
| Back-up maken | Extensie voor installatie is mislukt met de fout 'COM + niet kon Neem contact op met de Microsoft Distributed transactiecoördinator | Dit betekent meestal dat de COM + service niet wordt uitgevoerd. Neem contact op met Microsoft ondersteuning voor hulp bij dit probleem op te lossen. |
| Back-up maken | Momentopname bewerking is mislukt de VSS bewerking "dit station is vergrendeld door BitLocker station versleuteling. U moet dit station via het Configuratiescherm ontgrendelen. | BitLocker uitschakelen voor alle stations op de VM en toekijken als het VSS-probleem is opgelost |
| Back-up maken | Virtuele machines is virtual vaste schijven die zijn opgeslagen op de Premium-opslag ondervindt worden niet ondersteund voor back-up maken | Geen |
| Back-up maken | Azure virtuele machines niet gevonden. | Dit gebeurt wanneer de primaire VM is verwijderd, maar het back-beleid blijft zien voor een VM back-up uitvoeren. Deze fout herstellen: <ol><li>Maak opnieuw de virtuele machine met dezelfde naam en dezelfde resource groepsnaam [cloud servicenaam] <br>(OR) <li> Beveiliging voor deze VM uitschakelen zodat back-taken worden niet worden gemaakt </ol> |
| Back-up maken | VM-agent is niet aanwezig is op de virtuele machine: Installeer de vereiste oude vereiste VM agent en de bewerking opnieuw starten. | [Lees meer](#vm-agent) over VM agent installatie en hoe u de installatie van de agent VM valideren. |

## <a name="jobs"></a>Taken

| Bewerking | Meer informatie over fout | Tijdelijke oplossing |
| -------- | -------- | -------|
| Taak annuleren | Annulering wordt niet ondersteund voor dit taaktype - wacht totdat de taak is voltooid. | Geen |
| Taak annuleren | De taak is niet in een Annuleerbare staat - wacht totdat de taak is voltooid. <br>OF-BEWERKING<br> De geselecteerde taak is niet in een Annuleerbare staat - wacht totdat de taak is voltooid.| De taak is waarschijnlijk vrijwel voltooid; Wacht totdat de taak is voltooid |
| Taak annuleren | De taak niet worden annuleren, omdat deze niet uitgevoerd wordt-annulering wordt alleen ondersteund voor taken die uitgevoerd worden. Annuleer van poging op de voortgang van een in project. | Dit gebeurt vanwege een lange staat. Wacht totdat de minuten en probeer het opnieuw annuleren |
| Taak annuleren | De taak annuleren - een ogenblik geduld totdat de taak is voltooid is mislukt. | Geen |


## <a name="restore"></a>Herstellen
| Bewerking | Meer informatie over fout | Tijdelijke oplossing |
| -------- | -------- | -------|
| Herstellen | Herstellen is mislukt met de Cloud interne fout | <ol><li>Cloudservice waarnaar u wilt herstellen is geconfigureerd met DNS-instellingen. U kunt controleren <br>$deployment = "Servicenaam" get-AzureDeployment - servicenaam-Slot "Productie" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Als er is een adres dat is geconfigureerd, betekent dit dat de DNS-instellingen zijn geconfigureerd.<br> <li>Cloudservice waarnaar u wilt herstellen is geconfigureerd met optie en bestaande VMs in cloudservice zijn gestopt.<br>Een cloudservice heeft IP gereserveerd door het volgende powershell-cmdlets gebruiken, kunt u controleren:<br>$deployment = "servicenaam" get-AzureDeployment - servicenaam-Slot "Productie" $DEP ReservedIPName <br><li>U probeert een virtuele machine met de volgende speciale netwerkconfiguraties in herstellen naar dezelfde cloudservice. <br>-Virtuele machines onder configuratie van de verdeling voor laden (interne en externe)<br>-Virtuele machines met meerdere gereserveerde IP-adressen<br>-Virtuele machines met meerdere NIC 's<br>Selecteer een nieuwe cloudservice in de gebruikersinterface of Zie [overwegingen herstellen](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) voor VMs met speciale netwerkconfiguraties</ol> |
| Herstellen | De naam van de geselecteerde DNS wordt al gebruikt: Geef een andere naam voor de DNS- en probeer het opnieuw. | Als u hier de naam van de DNS-verwijst naar de naam van de cloud-service (meestal dat eindigt. cloudapp.net). Dit moet uniek zijn. Als u dit foutbericht krijgt, moet u een andere naam voor de VM kiezen tijdens het terugzetten. <br><br> Houd er rekening mee dat deze fout wordt weergegeven alleen voor gebruikers van de Azure-portal. De bewerking voor terugzetten via PowerShell wordt mislukt omdat dit alleen herstelt u de schijven en niet de VM maken. De fout wordt worden geconfronteerd wanneer de VM expliciet door u gemaakt wordt nadat de bewerking voor de schijf terugzetten. |
| Herstellen | De configuratie van de opgegeven virtueel netwerk niet juist is - Geef de configuratie van een ander virtueel netwerk en probeer het opnieuw. | Geen |
| Herstellen | Een gereserveerde IP-adres, die niet overeenkomen met de configuratie van de virtuele machine wordt teruggezet: Geef een andere cloudservice die geen van gereserveerde IP gebruikmaakt, of kies een ander herstelpunt terugzetten op basis van de opgegeven cloudservice gebruikt. | Geen |
| Herstellen | Cloudservice heeft bereikt op aantal invoer eindpunten - bewerking opnieuw uitvoeren door op te geven van een andere cloudservice of met behulp van een bestaand eindpunt. | Geen |
| Herstellen | Back-kluis en doelsites opslag-account zijn in twee verschillende gebieden - ervoor zorgen dat het opgegeven herstellen betrekking heeft opslag-account in hetzelfde Azure gebied, als de back-kluis is. | Geen |
| Herstellen | Opslag Account die is opgegeven voor het terugzetten niet is-ondersteund - alleen Basic/standaard opslag accounts met lokaal overtollige of geografische overtollige replicatie-instellingen worden ondersteund. Selecteer een ondersteunde opslag-account | Geen |
| Herstellen | Type opslag-Account die zijn opgegeven voor de bewerking voor terugzetten is niet online: Zorg ervoor dat het opgegeven herstellen betrekking heeft opslag-account online is | Dit kan gebeuren vanwege een tijdelijke fout in Azure Storage of vanwege een storing. Kies een ander opslag-account. |
| Herstellen | Resourcegroep quotum is bereikt: Verwijder sommige resourcegroepen uit Azure-portal of contactpersoon Azure ondersteuning om uit te breiden de limieten. | Geen |
| Herstellen | Geselecteerde subnet bestaat niet - Selecteer op een subnet dat bestaat | Geen |


## <a name="policy"></a>Beleid
| Bewerking | Meer informatie over fout | Tijdelijke oplossing |
| -------- | -------- | -------|
| Beleid maken | Maken van het beleid - Verklein de bewaarbeleid opties om door te gaan met de configuratie van het beleid is mislukt. | Geen |


## <a name="vm-agent"></a>VM-Agent

### <a name="setting-up-the-vm-agent"></a>Bij het instellen van de VM-Agent
De VM-Agent is meestal al aanwezig zijn in VMs die zijn gemaakt vanuit de galerie met Azure. Virtuele machines die worden gemigreerd van on-premises implementatie datacenters moet echter niet de VM-Agent is geïnstalleerd. Voor deze VMs moet de VM-Agent expliciet zijn geïnstalleerd. Meer informatie over het [installeren van de VM-agent op een bestaande VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Voor Windows VMs:

- Download en installeer de [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Moet u beheerdersrechten om de installatie te voltooien.
- [De eigenschap VM bijwerken](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de-agent is geïnstalleerd.

Voor Linux VMs:

- Meest recente [Linux-agent](https://github.com/Azure/WALinuxAgent) installeert via github.
- [De eigenschap VM bijwerken](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de-agent is geïnstalleerd.


### <a name="updating-the-vm-agent"></a>De VM-Agent bijwerken
Voor Windows VMs:

- Bijwerken van de VM-Agent is net zo eenvoudig als het [VM Agent binaire bestanden](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)opnieuw te installeren. Echter, moet u ervoor zorgen dat er geen back-up wordt uitgevoerd, terwijl de VM-Agent wordt bijgewerkt.

Voor Linux VMs:

- Volg de instructies op [Linux VM-Agent bijwerken](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>VM Agent installatie valideren
Het controleren op de VM Agent-versie van Windows VMs:

1. Meld u aan bij de Azure virtuele machine en navigeer naar de map *C:\WindowsAzure\Packages*. U moet de huidige WaAppAgent.exe-bestand hebt gevonden.
2. Met de rechtermuisknop op het bestand, gaat u naar **Eigenschappen**en selecteer vervolgens het tabblad **Details** . Het veld productversie moet 2.6.1198.718 of hoger

## <a name="troubleshoot-vm-snapshot-issues"></a>Problemen met VM momentopname oplossen
VM back-up is afhankelijk van de opdracht momentopname met onderliggende storage. Geen toegang hebben tot opslag of de begintijd vertragen in uitvoering van de taken van momentopname, kan de back-up mislukken. De volgende kunnen leiden tot momentopname taak is mislukt.

1. Netwerktoegang tot opslag is geblokkeerd met behulp van NSG<br>
   Meer informatie over het [inschakelen van netwerktoegang](backup-azure-vms-prepare.md#2-network-connectivity) met behulp van een van beide WhiteListing van IP-adressen Storage of proxyserver.
2.  VMs met Sql Server back-up is geconfigureerd, kunnen leiden tot momentopname taak vertraging <br>
    Back-up maken problemen VSS volledige back-up op Windows VMs al dan niet standaard VM. Op VMs welke Sql-Servers worden uitgevoerd en Sql Server back-up is geconfigureerd, kan dit ertoe leiden dat vertraging in momentopname worden uitgevoerd. Stel volgende registersleutel als u back-fouten vanwege momentopname problemen ondervindt.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  VM status juist omdat VM afgesloten in RDP is.  <br>
    Als u de virtuele machine in RDP hebt afgesloten, Controleer terug in de portal dat VM status correct worden weerspiegeld. Als dat niet zo is, sluit de VM in de portal met behulp van de optie 'Afsluiten' in VM dashboard.
4.  Als u meer dan vier VM delen dezelfde service cloud, kunt u meerdere back-beleid voor het voorbereiden van de back-ups dus geen dat back-ups van meer dan vier VM zijn gestart op hetzelfde moment configureren. Probeer te verspreiden van de back-up begintijden spreiden tussen beleidsregels uren. 
5.  VM wordt uitgevoerd bij hoge CPU/geheugen.<br>
    Als de virtuele machine wordt uitgevoerd bij hoge CPU usage(>90%) of geheugen, momentopname taak is in de wachtrij, vertraagd en wordt uiteindelijk time-out krijgt. Probeer op aanvraag back-up in deze situaties.

<br>

## <a name="networking"></a>Netwerken
Als alle extensies moet back-up-extensie toegang tot openbare internet om te werken. Geen toegang hebben tot openbare internet, kan de bestandenlijst zelf niet in tal van manieren:

- De installatie van de extensie kan mislukken
- De back-bewerkingen (zoals schijf momentopname) kunnen mislukken
- Weergeven van de status van de back-bewerking kan mislukken

Heeft zijn dat u voor het oplossen van openbare internetadressen gelede [hier](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). U moet de DNS-configuraties voor de VNET controleren en zorg ervoor dat de Azure-URI's opgelost worden kunnen.

Als de naamresolutie juist is ingevuld, moet toegang tot de Azure IP's ook worden opgegeven. Als u wilt deblokkeren toegang tot de Azure infrastructuur, volgt u een van de volgende stappen uit:

1. "Witte" lijst het datacenter van de Azure IP-bereiken gebruikt.
    - De lijst met [Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) moeten whitelisted krijgen.
    - Het IP-adressen met de cmdlet [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) de blokkering opheffen. Deze cmdlet binnen de VM Azure uitvoeren in een verhoogde PowerShell-venster (als Administrator uitvoeren).
    - Regels toevoegen aan de NSG (als er een op hun plaats staan) toegang tot het IP-adressen.
2. Maak een pad voor HTTP-verkeer
    - Als er implementeren enkele beperkingen netwerk op hun plaats staan (een netwerk beveiligingsgroep, bijvoorbeeld) een HTTP-proxy-server om te leiden van het verkeer. Stappen voor het implementeren van een HTTP-Proxy-server vindt u [hier](backup-azure-vms-prepare.md#2-network-connectivity).
    - Regels toevoegen aan de NSG (als er een op hun plaats staan) voor het toestaan van toegang met INTERNET uit de HTTP-Proxy.

>[AZURE.NOTE] DHCP moet zijn ingeschakeld in de Gast IaaS VM back-up werkt.  Als u een statische privé IP nodig hebt, moet u deze configureren via het platform. De optie DHCP binnen de VM moet worden ingeschakeld blijft.
Meer informatie over het [instellen van een statische IP voor interne privé](../virtual-network/virtual-networks-reserved-private-ip.md)weergeven.
