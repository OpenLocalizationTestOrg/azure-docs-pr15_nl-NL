<properties
    pageTitle="Problemen met Azure bestand opslag oplossen | Microsoft Azure"
    description="Opslag van Azure-bestand oplossen"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Azure-bestand opslag oplossen

In dit artikel bevat veel voorkomende problemen die betrekking op Microsoft Azure-bestandsopslag hebben wanneer u verbinding van Windows en Linux-clients maakt. Het biedt ook de mogelijke oorzaken van en oplossingen voor deze problemen.

**Algemene problemen (optreden in zowel Windows en Linux-clients)**

- [Quotum fout wanneer u een bestand te openen](#quotaerror)

- [Vertragingen wanneer u opslag van Azure-bestanden vanuit Windows of Linux openen](#slowboth)

**Problemen met Windows-client**

- [Vertragingen wanneer u Azure bestandsopslag vanuit Windows 8.1 of Windows Server 2012 R2](#windowsslow)

- [Fout 53 poging om een bestandsshare Azure koppelen](#error53)

- [Netto gebruik is geslaagd, maar ik zie niet het Azure bestand delen gekoppelde in Windows Verkenner](#netuse)

- [Mijn account opslag bevat "/" en het net gebruiken opdracht mislukt?](#slashfails)

- [Mijn toepassing/service geen toegang tot gekoppelde bestanden Azure-station.](#accessfiledrive)

- [Extra aanbevelingen optimaliseren](#additional)

**Problemen met Linux-client**

- [Fout "U kunt een bestand zijn kopiëren naar een bestemming die geen versleuteling ondersteunt" bij het uploaden van/kopiëren van bestanden naar Azure-bestanden](#encryption)

- [Fout 'Host is niet beschikbaar' op bestaand bestand deelt, of de shell loopt vast bij het uitvoeren van de lijst opdrachten op het koppelpunt](#errorhold)

- [Fout 115 koppelen tijdens een poging om mountains Azure-bestanden op de VM Linux](#error15)

- [Linux VM willekeurige vertraging in opdrachten zoals "ls"](#delayproblem)

## <a name="general-problems"></a>Algemene problemen
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Quotum fout wanneer u een bestand te openen

Klik in Windows krijg een foutbericht dat er dan ongeveer als volgt te werk:

**1816 ERROR_NOT_ENOUGH_QUOTA <>--0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Onvoldoende quotum is beschikbaar voor deze opdracht verwerken**

**Ongeldige ingang waarde GetLastError: 53**

Klik op Linux krijg een foutbericht dat er dan ongeveer als volgt te werk:

**<filename>[geweigerd]**

**Schijf is overschreden**

#### <a name="cause"></a>Oorzaak

Het probleem treedt op omdat u de bovengrens van gelijktijdige open ingangen dat is toegestaan voor een bestand hebt bereikt.

#### <a name="solution"></a>Oplossing

Beperk het aantal gelijktijdige open ingangen door sommige grepen sluiten en probeer het opnieuw. Zie [Microsoft Azure opslag prestaties en schaalbaarheid controlelijst](storage-performance-checklist.md)voor meer informatie.

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Vertragingen bij het opslaan van bestanden openen vanuit Windows of Linux

- Als u een specifieke i/o-grootte minimumvereiste niet hebt, is het raadzaam dat u als de i/o-grootte voor optimale prestaties 1 MB gebruikt.

- Als u weet dat de uiteindelijke grootte van een bestand dat u zijn uitbreiden met schrijven en uw software geen compatibiliteitsproblemen wanneer het nog niet geschreven uiteinde op het bestand met nullen, en stel vervolgens de bestandsgrootte van tevoren in plaats van elke schrijven wordt een uitbreiden schrijven.

## <a name="windows-client-problems"></a>Problemen met Windows-client
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Vertragingen bij het openen van de bestandsopslag vanuit Windows 8.1 of Windows Server 2012 R2

Voor klanten die werken met Windows 8.1 of Windows Server 2012 R2, zorg dat de hotfix [KB3114025](https://support.microsoft.com/kb/3114025) is geïnstalleerd. Deze hotfix verbetert de maken en prestaties van de greep sluiten.

U kunt het volgende script om te controleren of de hotfix is geïnstalleerd op uitvoeren:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Als hotfix is geïnstalleerd, wordt het volgende resultaat weergegeven:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Windows Server 2012 R2 afbeeldingen in Azure Marketplace hebben de hotfix KB3114025 geïnstalleerd al dan niet standaard in December 2015 starten.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Extra aanbevelingen optimaliseren

Nooit Maak of open een bestand op in de cache I/O die schrijftoegang, maar niet leestoegang aanvraagt. Dat wil zeggen, wanneer u **CreateFile()**bellen, nooit alleen **GENERIC_WRITE opgeven**, maar altijd opgeven **GENERIC_READ | GENERIC_WRITE**. Een alleen-schrijven greep kan geen lokaal, kleine schrijft cache zelfs als dit de enige geopende greep voor het bestand. Dit legt slechte prestaties ten bij het kleine schrijven. Houd er rekening mee dat de 'een' modus CRT **fopen()** wordt geopend de greep van een alleen-schrijven.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>'Fout 53' wanneer u probeert te koppelen en ontkoppelen daarvan een bestandsshare van Azure

Dit probleem kan worden veroorzaakt door de volgende voorwaarden:

#### <a name="cause-1"></a>Oorzaak 1

Systeemfout "53 is opgetreden. Toegang is geweigerd." Verbindingen met Azure bestandsshares worden veiligheidsredenen worden geblokkeerd als het communicatiekanaal is niet versleuteld en de verbinding wordt niet geprobeerd uit de dezelfde Datacenter waarop Azure bestandsshares zich bevinden. Communicatie kanaal versleuteling is niet beschikbaar als de client van de gebruiker OS geen SMB-versleuteling ondersteunt. Hiermee wordt aangegeven door een "systeem 53 is fout opgetreden. Toegang geweigerd' foutbericht wordt weergegeven wanneer een gebruiker probeert te koppelen van een bestand delen vanaf on-premises implementatie of een ander datacenter. Windows 8, Windows Server 2012 en nieuwere versies van elke negotiate-aanvraag met SMB 3.0, die versleuteling ondersteunt.

#### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

Verbinding maken vanaf een client die voldoet aan de vereisten van Windows 8, Windows Server 2012 of hogere versies of die verbinding maken vanaf een virtuele machine die zich op de dezelfde Datacenter als de opslag van Azure-account dat wordt gebruikt voor het delen van Azure-bestand.

#### <a name="cause-2"></a>Oorzaak 2

'Systeemfout 53' wanneer u koppelt een Azure bestandsshare kan optreden als poort 445 uitgaande communicatie met Azure bestanden datacenter is geblokkeerd. Klik [hier](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) om het overzicht van internetproviders die toestaan of weigeren van toegang via poort 445 weer te geven.

Comcast en sommige bedrijven IT blokkeren deze poort. Als u wilt weten of dit de reden achter het 'Systeem foutbericht 53' wordt weergegeven is, kunt u Portqry om het eindpunt TCP:445 query's mogelijk. Als het eindpunt TCP:445 wordt weergegeven als gefilterd, wordt de TCP-poort is geblokkeerd. Hier volgt een voorbeeldquery:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Als de TCP-445 wordt geblokkeerd door een regel langs het netwerkpad, ziet u het volgende resultaat:

**TCP-poort 445 (service van microsoft-ds): GEFILTERD**

Zie voor meer informatie over het gebruik van Portqry [Beschrijving van het hulpprogramma voor de opdrachtregel Portqry.exe](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Werken met uw IT-organisatie poort 445 uitgaande [Azure IP](https://www.microsoft.com/download/details.aspx?id=41653)-bereiken openen.

#### <a name="cause-3"></a>Oorzaak 3

"Systeemfout 53" kan ook worden ontvangen als NTLMv1 communicatie is ingeschakeld op de client. Ondervindt NTLMv1 ingeschakeld, wordt een client minder veilige gemaakt. Daarom worden communicatie voor Azure-bestanden geblokkeerd. Om te controleren of dit de oorzaak van de fout is, controleert u of dat de volgende registersubsleutel is ingesteld op een waarde van 3:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Zie het onderwerp [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) op TechNet voor meer informatie.

#### <a name="solution-for-cause-3"></a>Oplossing voor oorzaak 3

U lost dit probleem, stelt u de waarde LmCompatibilityLevel in de registersleutel HKLM\SYSTEM\CurrentControlSet\Control\Lsa op de standaardwaarde van 3.

Azure-bestanden ondersteunt alleen NTLMv2-verificatie. Zorg ervoor dat Groepsbeleid wordt toegepast op de clients. Hiermee wordt voorkomen dat deze fout zich voordoet. Dit wordt ook beschouwd als veiligheidsoverwegingen. Zie voor meer informatie [hoe clients NTLMv2 configureren met Groepsbeleid](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

De aanbevolen beleidsinstelling is **alleen verzenden NTLMv2-antwoord**. Dit komt overeen met een registerwaarde van 3. Clients gebruiken alleen NTLMv2-verificatie en ze NTLMv2 sessiebeveiliging gebruiken als de server dit ondersteunt. Domeincontrollers accepteren LM, NTLM en NTLMv2-verificatie.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Netto gebruik is geslaagd, maar het bestand niet ziet Azure delen gekoppelde in Windows Verkenner

#### <a name="cause"></a>Oorzaak

Standaard uitvoeren Windows Verkenner niet als Administrator. Als u **netto gebruiken** vanaf een opdrachtprompt als Administrator uitvoeren, toewijzen u de netwerkstation 'als Administrator." Aangezien toegewezen stations gebruiker centraal, wordt het gebruikersaccount waarmee is aangemeld de stations niet weergegeven als ze zijn gekoppeld onder een andere account.

#### <a name="solution"></a>Oplossing

Koppel de optie voor delen vanaf de opdrachtregel voor een niet-beheerder. U kunt ook [in dit TechNet-onderwerp](https://technet.microsoft.com/library/ee844140.aspx) als u wilt configureren de registerwaarde **EnableLinkedConnections** volgen.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Mijn account opslag bevat "/" en het net gebruiken opdracht mislukt?

#### <a name="cause"></a>Oorzaak

Wanneer de opdracht **net use** wordt uitgevoerd onder opdrachtprompt (cmd.exe), wordt deze door toe te voegen "/" als een schakeloptie geparseerd. Hierdoor wordt de toewijzing van het station mislukt.

#### <a name="solution"></a>Oplossing

U kunt een van de volgende stappen uit om het probleem te omzeilen:

• Gebruik het volgende PowerShell-opdracht:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Vanuit een batchbestand kan dit gebeuren als

`Echo new-smbMapping ... | powershell -command –`

• Plaats dubbele aanhalingstekens rond de toets om dit probleem omzeilen, tenzij "/" is het eerste teken. Als dat zo is, gebruikt u de interactieve modus en voer uw wachtwoord afzonderlijk of uw sleutels als u een sleutel die niet beginnen met het teken schuine streep (/) te genereren.

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Mijn toepassing/service geen toegang tot gekoppelde bestanden Azure-station

#### <a name="cause"></a>Oorzaak

Stations worden gekoppeld per gebruiker. Als uw toepassing of service wordt uitgevoerd onder een andere account, ziet de computer voor gebruikers niet het station.

#### <a name="solution"></a>Oplossing

Station koppelen uit dezelfde gebruikersaccount waaronder de toepassing is. U kunt dit doen met hulpprogramma's zoals gg.bat.

U kunt ook een nieuwe gebruiker die dezelfde bevoegdheden als de service of systeem netwerkaccount heeft maken en voer **cmdkey** en **netto gebruik** onder dat account. De naam van de gebruiker moet de naam van het opslag-account en wachtwoord moet de accountsleutel opslag. Er is een andere optie voor **netto gebruiken** om door te geven in de opslagaccountnaam en de sleutel in de gebruikersparameters voor gebruikersnaam en wachtwoord van de opdracht **net use** .

Wanneer u deze instructies, verschijnt het volgende foutbericht weergegeven: "systeem 1312 fout. Een sessie van de opgegeven gebruiker bestaat niet. Het is mogelijk al beëindigd' bij het uitvoeren van **netto gebruiken** voor het systeem/netwerk service-account. Als dit gebeurt, controleert u of dat de gebruikersnaam die wordt doorgegeven aan de **netto gebruik** domeininformatie bevat (bijvoorbeeld: "[opslagaccountnaam]. file.core.windows .net").

## <a name="linux-client-problems"></a>Problemen met Linux-client

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fout 'U kopieert een bestand naar een bestemming die versleuteling wordt niet ondersteund'

#### <a name="cause"></a>Oorzaak

BitLocker gecodeerde bestanden kunnen worden gekopieerd naar Azure-bestanden. De bestandsopslag ondersteunt echter niet NTFS EFS. Daarom u waarschijnlijk gebruikt EFS in dit geval. Als u bestanden die zijn versleuteld met EFS hebt, wordt een bewerking kopiëren naar de bestandsopslag kan mislukken tenzij de opdracht kopiëren is een gekopieerde bestand ontsleutelen.

#### <a name="workaround"></a>Tijdelijke oplossing

Als u wilt een bestand kopiëren naar de bestandsopslag, moet u deze eerst decoderen. U kunt dit doen met behulp van een van de volgende methoden:

• Gebruik **/d kopiëren**.

• Instellen de volgende registersleutel:

- Pad = HKLM\Software\Policies\Microsoft\Windows\System
- Waardetype DWORD =
- Naam = CopyFileAllowDecryptedRemoteDestination
- Waarde = 1

Bedenk wel dat de registersleutel instelling is van invloed op alle kopieerbewerkingen naar gedeelde netwerken.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Fout 'Host is niet beschikbaar' op bestaand bestand deelt, of de shell loopt vast wanneer u lijst opdrachten op het koppelpunt uitvoeren

#### <a name="cause"></a>Oorzaak

Deze fout treedt op de client Linux wanneer de client niet actief geweest gedurende een langere periode is. Wanneer deze fout treedt op, de client verbinding wordt verbroken en de clientverbinding treedt er een time-out.

#### <a name="solution"></a>Oplossing

Dit probleem is nu vastgezet op de kernel Linux als onderdeel van de [set wijzigen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), in behandeling backport in Linux-verdeling.

Works kunt dit probleem, de verbinding vangen en voorkomen dat een inactief, een bestand bewaren in de Azure bestandsshare die u regelmatig schrijven. Dit is een bewerking schrijven, zoals de datum die is gemaakt/gewijzigd herschrijven aan het bestand. Anders kan worden weergegeven in de cache opgeslagen resultaten en de bewerking mogelijk niet zo activeren dat de verbinding.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>'Fout 115 koppelen' wanneer u probeert te koppelen van Azure-bestanden op de VM Linux

#### <a name="cause"></a>Oorzaak

Nog bieden niet versleutelingsfunctie in SMB 3.0 ondersteuning voor Linux onderzoeken. In sommige onderzoeken wordt gebruiker een "115" foutbericht weergegeven als zij te koppelen Azure-bestanden probeert met behulp van SMB 3.0 vanwege een ontbrekende functie.

#### <a name="solution"></a>Oplossing

Als de Linux SMB-client die wordt gebruikt geen versleuteling ondersteunt, bestanden mountains Azure via SMB 2.1 uit een VM Linux op de dezelfde Datacenter als het bestand opslag-account.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux VM willekeurige vertraging in opdrachten zoals "ls"

#### <a name="cause"></a>Oorzaak

Dit kan gebeuren wanneer de optie **serverino** niet bij de koppelopdracht inbegrepen is. De opdracht ls zonder **serverino**, wordt een **stat** op elk bestand uitgevoerd.

#### <a name="solution"></a>Oplossing

Controleer de **serverino** in uw vermelding '/ enzovoort/fstab':

azureuser.File.Core.Windows.NET/WMS/Comer op/Start/sampledir type cifs (rw, nodev, relatime, ver = 2.1, sec = ntlmssp, cache = strikte, gebruikersnaam = lengte, domein = X, file_mode = 0755, dir_mode = 0755, serverino, rsize = 65536, wsize = 65536, actimeo = 1)

Als de optie **serverino** niet aanwezig is, niet ontkoppelen en dit koppelen opnieuw bestanden Azure doordat de optie **serverino** is geselecteerd.

## <a name="learn-more"></a>Meer informatie

- [Aan de slag met Azure bestandsopslag in Windows](storage-dotnet-how-to-use-files.md)

- [Aan de slag met Azure bestandsopslag op Linux](storage-how-to-use-files-linux.md)
