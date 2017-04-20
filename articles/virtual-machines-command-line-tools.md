<properties
    pageTitle="Azure CLI-opdrachten in de modus voor servicebeheer | Microsoft Azure"
    description="Azure opdrachtregelinterface ()-opdrachten in de modus voor servicebeheer implementaties in het implementatiemodel klassieke beheren"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure CLI-opdrachten in de modus voor beheer van Azure-Service (asm)

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]U kunt ook [Lees meer over de opdrachten voor de model resourcemanager](virtual-machines/azure-cli-arm-commands.md), en het gebruik van de CLI om te [migreren van resources](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) uit het klassieke aan het model resourcemanager.

Dit artikel worden de syntaxis en opties voor Azure CLI-opdrachten die u vaak kunt maken en beheren van Azure resources in het implementatiemodel klassieke. U kunt deze opdrachten openen door de CLI uit te voeren in de modus voor beheer van Azure-Service (asm). Dit is geen volledige informatie over en uw versie CLI kan weergeven als iets anders opdrachten of parameters. 

Om gestart, eerst [de CLI Azure installeren](xplat-cli-install.md) en [verbinden met uw Azure-abonnement](xplat-cli-connect.md).

Typ voor huidige opdrachtsyntaxis van de en de opties voor de opdrachtregel `azure help` of, weer te geven van de help voor specifieke opdracht `azure help [command]`. CLI voorbeelden in de documentatie ook vinden voor het maken en beheren van specifieke Azure services.

Optionele parameters worden weergegeven in de vierkante haken (bijvoorbeeld `[parameter]`). Alle andere parameters zijn vereist.

Naast de opdracht / regiospecifieke optionele parameters hier beschreven, zijn er drie optionele parameters die kunnen worden gebruikt om gedetailleerde uitvoer zoals verzoek opties en statuscodes weer te geven. De `-v` parameter biedt uitgebreide uitvoer, en de `-vv` parameter geeft zelfs meer uitgebreide uitvoer. De `--json` optie wordt het resultaat in onbewerkte json-indeling.

## <a name="setting-asm-mode"></a>Instelling asm modus

Gebruik de volgende opdracht opdrachten voor het beheer van Azure-Service voor CLI-modus inschakelen.

    azure config mode asm

>[AZURE.NOTE] Van de CLI Azure resourcemanager modus en beheer van Azure-Service zijn elkaar wederzijds uitsluiten. Dat wil zeggen niet uit de andere modus resources die zijn gemaakt in de ene beheren.

## <a name="manage-your-account-information-and-publish-settings"></a>Gegevens van uw account beheren en publiceren van instellingen
Een manier die de CLI kunt verbinden met uw account is met behulp van uw op Azure-abonnementsgegevens. (Zie [verbinding maken met een Azure-abonnement van de Azure CLI](xplat-cli-connect.md) voor andere opties.) Deze informatie kan worden opgehaald uit de Azure klassieke portal in een instellingenbestand publiceren die hier staan beschreven. U kunt het instellingenbestand publiceren importeren zoals een permanente lokale configuratie instellen dat de CLI wordt gebruikt voor verdere bewerkingen. U hoeft slechts eenmaal importeren van uw publicatie eenmaal-instellingen.

**account downloaden [opties]**

Deze opdracht Hiermee start u een browser om uw .publishsettings-bestand downloaden via de portal van Azure klassieke.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**account importeren [opties] &lt;bestand >**


Deze opdracht geïmporteerd uit een publishsettings bestand of het certificaat, zodat deze kan worden gebruikt door het hulpprogramma in toekomstige sessies.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Het bestand publishsettings kan bevatten details (dat wil zeggen de naam van abonnement en ID) van meer dan één abonnement. Wanneer u het bestand publishsettings importeert, wordt het eerste abonnement wordt gebruikt als de standaardbeschrijving van de. Als u wilt gebruiken in een ander abonnement, voert u de volgende opdracht uit:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**account wissen [opties]**

Deze opdracht Hiermee verwijdert u de opgeslagen publishsettings die zijn geïmporteerd. Gebruik deze opdracht als u klaar bent met het hulpprogramma voor het op deze computer en wilt zorgen dat het hulpmiddel met uw account in toekomstige sessies kan niet worden gebruikt.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**lijst met accounts [opties]**

Lijst van de geïmporteerde abonnementen

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**account instellen [opties] &lt;abonnement&gt;**

Het huidige abonnement instellen

###<a name="commands-to-manage-your-affinity-groups"></a>Opdrachten voor het beheren van uw groepen affiniteit

**lijst met accounts affiniteit-groep [opties]**

Deze opdracht bevat uw Azure affiniteit groepen.

Affiniteit groepen kan worden ingesteld wanneer een groep van virtuele machines meerdere fysieke machines. De groep affiniteit aangeeft dat de fysieke machines moeten worden zo dicht bij elkaar mogelijk netwerklatentie verkleinen.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**account affiniteit-groep maken [opties] &lt;naam&gt;**

Deze opdracht Hiermee maakt u een groep affiniteit

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**account affiniteit-groep [opties] weergeven &lt;naam&gt;**

Deze opdracht ziet u de details van de groep affiniteit

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**account affiniteit-groep verwijderen [opties] &lt;naam&gt;**

Deze opdracht verwijderen de opgegeven affiniteit-groep

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Opdrachten voor het beheren van uw account-omgeving

**lijst met accounts envelop [opties]**

Lijst met de account-omgevingen

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**account envelop [opties] [omgeving] weergeven**

Account omgeving details weergeven

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**account envelop toevoegen [opties] [omgeving]**

Een omgeving met deze opdracht toegevoegd aan het account

**account envelop [opties] [omgeving] instellen**

Deze opdracht stelt u de account-omgeving

**account envelop [opties] [omgeving] verwijderen**

Deze opdracht verwijderd de opgegeven omgeving uit het account

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Opdrachten voor het beheren van uw klassieke virtuele machines
In het volgende diagram ziet u hoe klassieke Azure virtuele machines worden gehost in de productieomgeving voor de implementatie van een Azure-cloudservice.

![Azure technische Diagram](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Nieuw** Hiermee maakt u het station in blobopslag (dat wil zeggen e:\ in het diagram); een schijf al gemaakt, maar niet-vastgemaakte **bijvoegen** is gekoppeld aan een virtuele machine.

**VM maken [opties] &lt;dns-naam > &lt;afbeelding > &lt;gebruikersnaam > [wachtwoord]**

Deze opdracht maakt een Azure virtuele machines. Standaard wordt elke virtuele machine (vm) gemaakt in een eigen-cloudservice. U kunt opgeven dat een virtuele machine moet worden toegevoegd aan een bestaande cloudservice tot en met gebruik van de optie - c zoals hier beschreven.

De vm opdracht, zoals de Azure klassieke portal maken, maakt alleen virtuele machines in de productieomgeving voor implementatie. Er is geen optie voor het maken van een virtuele machine in de testomgeving voor de implementatie van een cloudservice. Als uw abonnement geen een bestaand Azure opslag-account heeft, de opdracht gemaakt.

U kunt een locatie opgeven via de--locatieparameter, of kunt u een groep affiniteit via de--parameter affiniteit-groep. Als geen van beide is opgegeven, wordt u gevraagd op te geven in een lijst met geldige locaties.

Het opgegeven wachtwoord moet 8-123 tekens bevatten en voldoen aan de wachtwoordvereisten complexiteit van het besturingssysteem dat u voor deze virtuele machine gebruikt.

Als u wilt gaan SSH gebruiken voor het beheren van een uitgevouwen Linux virtuele machine (zoals gewoonlijk de hoofdletters/kleine letters is), moet u SSH inschakelen via de -e-optie wanneer u de virtuele machine maakt. Dit kan niet worden ingeschakeld SSH nadat de virtuele machine is gemaakt.

Windows virtuele machines kunt RDP later inschakelen door poort 3389 toe te voegen als een eindpunt.

De volgende optionele parameters worden ondersteund voor deze opdracht:

**- c,--verbinding** de virtuele machine binnen een reeds gemaakte implementatie maken in een hostingservice. Als - vmname met deze optie niet wordt gebruikt, wordt automatisch de naam van de nieuwe virtuele machine gegenereerd.<br />
**-n,--vm-naam** Geef de naam van de virtuele machine. Deze parameter wordt standaard hostingprovider servicenaam. Als niet - vmname is opgegeven, de naam voor de nieuwe virtuele machine wordt gegenereerd als &lt;servicenaam >&lt;id >, waarbij &lt;id > is het aantal bestaande virtuele machines in de service plus 1. Als u deze opdracht een virtuele machine toevoegen aan een hostingservice MijnService met één bestaande virtuele machine gebruikt, is de nieuwe virtuele machine bijvoorbeeld MyService2 naam.<br />
**-u,--blob-url** Geef de doel-URL blob storage waarvoor de schijf VM maken. <br />
**-z,--vm-grootte** De grootte van de virtuele machine opgeven. Geldige waarden zijn: "ExtraSmall", "Kleine", "Medium", "Grote", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". De standaardwaarde is 'Klein'. <br />
**-r** RDP-connectiviteit toegevoegd aan een virtuele Windows-computer. <br />
**-e,--ssh** SSH connectivity toegevoegd aan een virtuele Windows-computer. <br />
**-t,--ssh-certificaat** Hiermee geeft u het certificaat SSH. <br />
**-s** Het abonnement <br />
**-o,--community** De opgegeven afbeelding is een afbeelding van de community <br />
**-w** Het virtuele netwerknaam <br/>
**-l,--locatie** geeft de locatie (bijvoorbeeld "Noord Centraal US"). <br />
Hiermee geeft u de groep affiniteit **- een,--affiniteit-groep** .<br />
**-w,--virtuele-netwerknaam** Geef het virtuele netwerk waarop u wilt toevoegen van de nieuwe virtuele machine. Virtuele netwerken kunnen worden ingesteld en beheerd met de portal van Azure klassieke.<br />
**-b,--subnet-namen** Hiermee geeft u de subnetnamen de virtuele machine toewijzen.

In dit voorbeeld is MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB een afbeelding die is verstrekt door het platform. Zie voor meer informatie over besturingssysteem afbeeldingen, vm afbeelding van de lijst.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**VM maken van &lt;dns-naam > &lt;rol-bestand >**

Deze opdracht maakt een Azure virtuele machines uit een bestand van de rol JSON.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**VM lijst [opties]**

Deze opdracht bevat Azure virtuele machines. De optie--json geeft aan dat de resultaten worden geretourneerd in de onbewerkte JSON-indeling.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**lijst met locaties VM [opties]**

Deze opdracht bevat alle beschikbare Azure-account locaties.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**VM [opties] weergeven &lt;naam >**

Deze opdracht ziet u meer informatie over een Azure virtuele machines. De optie--json geeft aan dat de resultaten worden geretourneerd in de onbewerkte JSON-indeling.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**VM verwijderen [opties] &lt;naam >**

Deze opdracht verwijderd een Azure virtuele machines. Standaard worden deze opdracht het Azure blob waaruit de schijf besturingssysteem en de gegevensschijf worden gemaakt niet verwijderd. Als u wilt verwijderen de blob en de virtuele machine waarop deze is gebaseerd, geef de optie -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**VM [startopties] &lt;naam >**

Deze opdracht start een Azure virtuele machines.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**VM opnieuw [opties] &lt;naam >**

Deze opdracht opnieuw is opgestart een Azure virtuele machines.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**VM afsluiten [opties] &lt;naam >**

Deze opdracht is afgesloten van een Azure virtuele machines. U kunt de optie -p om op te geven dat de resource berekeningscluster niet zijn uitgebracht op afsluiten.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**VM vastleggen &lt;vm-naam > &lt;doel-afbeelding-name >**

Deze opdracht wordt vastgelegd voor een afbeelding Azure virtuele machines.

Afbeelding van een virtuele machine kan alleen worden vastgelegd als de status van de virtuele machine **gestopt is**. De virtuele machine afgesloten voordat u verdergaat.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**VM exporteren [opties] &lt;vm-naam > &lt;bestandspad >**

Deze opdracht exporteert een afbeelding Azure virtuele machines naar een bestand

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Opdrachten voor het beheren van uw eindpunten Azure virtuele machines
In het volgende diagram ziet u de architectuur van een typisch implementatie van meerdere exemplaren van een klassieke virtuele machine. In dit voorbeeld is poort 3389 geopend op elke virtuele machine (voor RDP toegang). Er is ook een interne IP-adres (bijvoorbeeld 168.55.11.1) op elke virtuele machine die wordt gebruikt door de verdeling van de belasting voor route-verkeer naar de virtuele machine. Dit interne IP-adres kan ook worden gebruikt voor de communicatie tussen virtuele machines.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Externe aanvragen voor virtuele machines leest u over een taakverdeling. Reden worden niet aanvragen ten opzichte van een bepaalde virtuele machine op implementaties met meerdere virtuele machines opgegeven. Voor implementaties met meerdere virtuele machines is poorttoewijzing geconfigureerd tussen de virtuele machines (vm-poort) en de verdeling van de belasting (kg-poort).

**VM eindpunt maken &lt;vm-naam > &lt;kg-poort > [vm-poort]**

Deze opdracht maakt een virtuele machine-eindpunt. U kunt ook -u of--inschakelen-direct-server-return om op te geven of om in te schakelen directe server terug op dit eindpunt, standaard uitgeschakeld.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**vm eindpunt maken-veelvoud [opties] &lt;vm-naam > &lt;kg-poort > [:&lt;vm-poort > [:&lt;protocol > [:&lt;inschakelen-direct-server-return > [:&lt;kg-set-name > [:&lt;test-protocol > [:&lt;test-poort > [:&lt;test-pad > [:&lt;interne-kg-name >]]] {1 -*}**

Meerdere vm eindpunten maken.

**VM eindpunt verwijderen [opties] &lt;vm-naam > &lt;naam van het eindpunt >**

Deze opdracht Hiermee verwijdert u een virtuele machine-eindpunt.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**VM eindpuntlijst &lt;vm-naam >**

Deze opdracht bevat alle VM eindpunten. De optie--json geeft aan dat de resultaten worden geretourneerd in de onbewerkte JSON-indeling.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**VM eindpunt update [opties] &lt;vm-naam > &lt;naam van het eindpunt >**

Een eindpunt vm deze opdracht bijgewerkt naar de nieuwe waarden voor het gebruik van deze opties.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**VM eindpunt [opties] weergeven &lt;vm-naam >**

Deze opdracht ziet u de details van de eindpunten op een vm

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Opdrachten voor het beheren van uw afbeeldingen Azure virtuele machines

VM afbeeldingen zijn schermopnames van al geconfigureerde virtuele machines die kunnen worden gerepliceerd zoals vereist.

**lijst van de afbeelding VM [opties]**

Deze opdracht ontvangt een lijst met afbeeldingen VM. Er zijn drie soorten afbeeldingen: afbeeldingen die zijn gemaakt door Microsoft, die worden voorafgegaan door 'MSFT', afbeeldingen die zijn gemaakt door derde partijen, die worden voorafgegaan door de naam van de leverancier, en u afbeeldingen maken. Als u wilt maken van afbeeldingen, kunt u een bestaande VM vastleggen of maken van een afbeelding van een aangepaste VHD geüpload naar blobopslag. Zie voor meer informatie over het gebruik van een aangepaste VHD vm-afbeelding maken.
De optie--json geeft aan dat de resultaten worden geretourneerd in de onbewerkte JSON-indeling.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**VM afbeelding weergeven [opties] &lt;naam >**

Deze opdracht ziet u de details van de afbeelding van een virtuele machine.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**verwijderen van de afbeelding VM [opties] &lt;naam >**

Deze opdracht Hiermee verwijdert u de afbeelding van een virtuele machine.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**afbeelding van de VM maken &lt;naam > [bron-pad]**

Deze opdracht Hiermee maakt u de afbeelding van een virtuele machine. De aangepaste VHD-bestanden worden geüpload om blob storage en vervolgens de afbeelding VM vanaf hier is gemaakt. Klik u in deze afbeelding VM maakt een virtuele machine. De locatie en OS parameters zijn vereist.

>[AZURE.NOTE]Deze opdracht ondersteunt momenteel alleen uploaden vaste VHD-bestanden. Als u wilt een dynamische VHD-bestand uploaden, gebruikt u de [Azure VHD hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go).

Sommige systemen opleggen per proces beschrijving bestandsgrootten. Als deze limiet is overschreden, wordt in het hulpmiddel een bestand beschrijving limiet-fout weergegeven. U kunt de opdracht opnieuw met de -p uitvoeren &lt;getal > parameter kunt beperken het maximum aantal parallelle uploads. Het standaard maximum aantal parallelle uploads is 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Opdrachten voor het beheren van uw Azure virtuele machines gegevensschijven

Gegevensschijven zijn VHD-bestanden in-blobopslag die kunnen worden gebruikt door een virtuele machine. Voor meer informatie over hoe u zijn gegevensschijven geïmplementeerd voor blob storage, raadpleegt u het Azure technische diagram eerder weergegeven.

De opdrachten voor het koppelen van gegevensschijven (azure vm schijf bijvoegen en azure vm schijf bijvoegen-nieuwe) een logische eenheid (LUN) toewijzen aan de gegevensschijf gekoppelde, zoals vereist door de SCSI-protocol. De eerste gegevensschijf die is gekoppeld aan een virtuele machine LUN 0 is toegewezen, is de volgende toegewezen LUN 1, enzovoort.

Wanneer u een gegevensschijf met de schijf azure vm loskoppelen loskoppelen van de opdracht, voert u de &lt;lun&gt; parameter om aan te geven welke schijf loskoppelen.

>[AZURE.NOTE] U moet altijd in omgekeerde volgorde, gegevensschijven loskoppelen beginnen met het hoogste nummer LUN dat is toegewezen. De laag Linux SCSI biedt geen ondersteuning voor een lagere nummers LUN loskoppelen terwijl een hogere nummers LUN nog steeds is gekoppeld. U moet bijvoorbeeld niet LUN 0 ontkoppelen als LUN 1 nog steeds is gekoppeld.

**VM schijf weergeven [opties] &lt;naam >**

Deze opdracht ziet u meer informatie over een Azure schijf.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**VM Schijflijst [opties] [vm-naam]**

Deze opdracht lijsten Azure schijven of schijven die zijn bijgevoegd bij een opgegeven virtuele machine. Als deze is uitgevoerd met de parameter voor een virtuele machine, is het resultaat alle schijven die zijn bijgevoegd bij de virtuele machine. LUN 1 wordt gemaakt met de virtuele machine en eventuele andere vermelden schijven afzonderlijk zijn gekoppeld.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Deze opdracht zonder VM naamparameter wordt uitgevoerd, retourneert alle schijven.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**VM schijf verwijderen [opties] &lt;naam >**

Deze opdracht verwijdert een Azure schijf uit een persoonlijke bibliotheek. De schijf moet worden losgekoppeld van de virtuele machine voordat deze wordt verwijderd.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**VM schijf maken &lt;naam > [bron-pad]**

Deze opdracht uploads en registreert een Azure schijf. --blob-url,--locatie, of--affiniteit-groep moet worden opgegeven. Als u deze opdracht met [bron-pad] gebruikt, wordt het opgegeven VHD-bestand wordt geüpload en een afbeelding is gemaakt. U kunt deze afbeelding vervolgens koppelen aan een virtuele machine met behulp van vm schijf bijvoegen.

Sommige systemen opleggen per proces beschrijving bestandsgrootten. Als deze limiet is overschreden, wordt in het hulpmiddel een bestand beschrijving limiet-fout weergegeven. U kunt de opdracht opnieuw met de -p uitvoeren &lt;getal > parameter kunt beperken het maximum aantal parallelle uploads. Het standaard maximum aantal parallelle uploads is 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**VM schijf upload [opties] &lt;bron-pad > &lt;blob-url > &lt;opslag accountsleutel >**

Deze opdracht kunt u een schijf vm uploaden

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**VM schijf bijvoegen &lt;vm-naam > &lt;naam van schijf image >**

Een bestaande schijf in blobopslag koppelt deze opdracht aan een bestaande VM geïmplementeerd in een cloudservice.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**VM schijf bijvoegen-nieuwe &lt;vm-naam > &lt;grootte in gb > [blob-url]**

Deze opdracht koppelt een gegevensschijf aan een Azure virtuele machines. In dit voorbeeld is 20 de grootte van de nieuwe schijf, klikt u in gigabytes, om te worden gekoppeld. Desgewenst kunt u de URL van een blob als het laatste argument expliciet opgeven de blob doel maken. Als u de URL van een blob niet opgeeft, wordt er automatisch een object blob gegenereerd.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**VM schijf loskoppelen &lt;vm-naam > &lt;lun >**

Deze opdracht ontkoppelen een schijf met gegevens die zijn bijgevoegd bij een Azure virtuele machines &lt;LUN > de schijf losgemaakt aangeeft. Als u een lijst met schijven die is gekoppeld aan een schijf voordat u deze hebt losgekoppeld, gebruikt u vm schijf-lijst &lt;vm-naam >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Opdrachten voor het beheren van uw cloudservices Azure

Azure cloudservices zijn toepassingen en services die worden gehost op web rollen en werknemer rollen. De volgende opdrachten kunnen worden gebruikt voor het beheren van Azure cloudservices.

**service maken [opties] &lt;servicenaam >**

Deze opdracht maakt u een cloudservice

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**[Service afspeelopties] &lt;servicenaam >**

Deze opdracht ziet u de details van een Azure cloudservice

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**lijst met Services [opties]**

Deze opdracht bevat Azure cloudservices.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**service verwijderen [opties] &lt;naam >**

Deze opdracht verwijderd een Azure-cloudservice.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Als u wilt dat de verwijdering, gebruikt u de `-q` parameter.


## <a name="commands-to-manage-your-azure-certificates"></a>Opdrachten voor het beheren van uw Azure certificaten

Azure-service certificaten zijn SSL-certificaten die zijn verbonden met uw Azure-account. Zie voor meer informatie over Azure certificaten [Certificaten beheren](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**lijst met Service-certificaat [opties]**

Deze opdracht bevat Azure certificaten.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Service-certificaat maken &lt;dns-voorvoegsel > &lt;bestand > [wachtwoord]**

Deze opdracht uploadt een certificaat. Laat het wachtwoord wordt gevraagd lege voor certificaten die niet beveiligd met een wachtwoord zijn.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Service certificaat verwijderen [opties] &lt;vingerafdruk >**

Deze opdracht Hiermee verwijdert u een certificaat.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Opdrachten voor het beheren van uw WebApps

Een Azure web-app is een webconfiguratie toegankelijke URI. WebApps worden gehost in virtuele machines, maar u hoeft niet te denken over de details van maken en implementeren van de virtuele machine zelf. Deze gegevens worden afgehandeld voor u door Azure.

**sitelijst [opties]**

Deze opdracht bevat uw Webapps.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**site [opties] [naam] instellen**

Deze opdracht ingesteld configuratieopties voor uw web-app [naam]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**site-deploymentscript [opties]**

Deze opdracht genereert een aangepaste implementatiescript

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**site maken [opties] [naam]**

Deze opdracht Hiermee maakt u een WebApp en de lokale map.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] De naam van de site moet uniek zijn. U kunt geen een site maken met de DNS-naam van een bestaande site.

**site bladeren [opties] [naam]**

Deze opdracht opent uw web-app in een browser.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**site weergeven [opties] [naam]**

Deze opdracht ziet details voor een web-app.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**site verwijderen [opties] [naam]**

Deze opdracht Hiermee verwijdert u een web-app.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **site omwisselen [opties] [naam]**

Deze opdracht verwisselt twee web app-sleuven.

Deze opdracht ondersteunt de volgende aanvullende optie:

**- q of **--stille **: niet vragen om bevestiging. Gebruik deze optie in geautomatiseerde scripts.


**begin van de site [opties] [naam]**

Deze opdracht start een web-app.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**site stoppen [opties] [naam]**

Deze opdracht drukt, stopt een web-app.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**site opnieuw starten [opties] [naam]**

Deze opdracht stopt en start vervolgens een opgegeven web-app.

Deze opdracht ondersteunt de volgende aanvullende optie:

**--slot** &lt;slot >: de naam van de slot om opnieuw te starten.


**locatie sitelijst [opties]**

Deze opdracht bevat uw app weblocaties.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Opdrachten voor het beheren van de instellingen van de web-app-toepassing

**appsetting sitelijst [opties] [naam]**

Deze opdracht bevat de instelling van de app toegevoegd aan de web-app.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**site appsetting toevoegen [opties] &lt;keyvaluepair > [naam]**

Deze opdracht voegt de instelling voor een app aan uw web-app als een paar sleutelwaarde.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**site appsetting verwijderen [opties] &lt;toets > [naam]**

Deze opdracht Hiermee verwijdert u de instelling van de opgegeven app via de web-app.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**[site appsetting afspeelopties] &lt;toets > [naam]**

Deze opdracht Hiermee worden details van de opgegeven app-instelling

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Opdrachten voor het beheren van uw web-app certificaten

**certificaat sitelijst [opties] [naam]**

Deze opdracht bevat een overzicht van de certificaten voor het web app.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**site-certificaat toevoegen [opties] &lt;certificaat-pad > [naam]**

**site-certificaat verwijderen [opties] &lt;vingerafdruk > [naam]**

**site-certificaat weergeven [opties] &lt;vingerafdruk > [naam]**

Deze opdracht ziet u de details van het certificaat

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Opdrachten voor het beheren van uw web app verbindingstekenreeksen

**connectionstring sitelijst [opties] [naam]**

**site connectionstring toevoegen [opties] &lt;verbindingsnaam > &lt;waarde > &lt;type > [naam]**

**site connectionstring verwijderen [opties] &lt;verbindingsnaam > [naam]**

**[site connectionstring afspeelopties] &lt;verbindingsnaam > [naam]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Opdrachten voor het beheren van de standaarddocumenten van uw web-app

**defaultdocument sitelijst [opties] [naam]**

**site-defaultdocument toevoegen [opties] &lt;document > [naam]**

**site-defaultdocument verwijderen [opties] &lt;document > [naam]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Opdrachten voor het beheren van uw web app-implementaties

**implementatie sitelijst [opties] [naam]**

**site-implementatie [opties] weergeven &lt;commitId > [naam]**

**site-implementatie in dat geval [opties] &lt;commitId > [naam]**

**site-implementatie github [opties] [naam]**

**implementatie sitegebruiker [opties] [gebruikersnaam] [keer] instellen**

###<a name="commands-to-manage-your-web-app-domains"></a>Opdrachten voor het beheren van uw web app-domeinen

**domein sitelijst [opties] [naam]**

**site-domein toevoegen [opties] &lt;dn > [naam]**

**site domein verwijderen [opties] &lt;dn > [naam]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Opdrachten voor het beheren van uw web app-Handlertoewijzingen

**-handler sitelijst [opties] [naam]**

**site-handler toevoegen [opties] &lt;extensie > &lt;processor > [naam]**

**site-handler verwijderen [opties] &lt;extensie > [naam]**

###<a name="commands-to-manage-your-web-jobs"></a>Opdrachten voor het beheren van uw Web-taken

**site-takenlijst [opties] [naam]**

Deze opdracht lijst met alle taken web onder een web-app.

Deze opdracht ondersteunt de volgende extra opties:

+ **--taaktype** &lt;taaktype >: optioneel. Het type van de webjob. Geldige waarde is "geactiveerde" of "doorlopend". Standaard retourneren webjobs van alle typen.
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

**site-taak weergeven [opties] &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht ziet u de details van een specifieke web-taak.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam >: vereist. De naam van de webjob.
+ **--taaktype** &lt;taaktype >: vereist. Het type van de webjob. Geldige waarde is "geactiveerde" of "doorlopend".
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

**site verwijderen [opties] &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht verwijderd uit de opgegeven web-taak.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam > vereist. De naam van de webjob.
+ **--taaktype** &lt;taaktype > vereist. Het type van de webjob. Geldige waarde is "geactiveerde" of "doorlopend".
+ **-q** of **--stille**: niet vragen om bevestiging. Gebruik deze optie in geautomatiseerde scripts.
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

**site taak upload [opties] &lt;taaknaam > &lt;taaktype > <jobFile> [naam]**

Deze opdracht verwijderd uit de opgegeven web-taak.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam >: vereist. De naam van de webjob.
+ **--taaktype** &lt;taaktype >: vereist. Het type van de webjob. Geldige waarde is "geactiveerde" of "doorlopend".
+ **--taak-bestand** &lt;taak-bestand >: vereist. De taakbestand.
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

**begin van de taak [opties] site &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht start de opgegeven web-taak.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam >: vereist. De naam van de webjob.
+ **--taaktype** &lt;taaktype >: vereist. Het type van de webjob. Geldige waarde is "geactiveerde" of "doorlopend".
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

**einde van de taak site [opties] &lt;taaknaam > &lt;taaktype > [naam]**

Deze opdracht drukt, stopt de opgegeven web-taak. Alleen continue taken kunnen het worden gestopt.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam >: vereist. De naam van de webjob.
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

###<a name="commands-to-manage-your-web-jobs-history"></a>Opdrachten voor het beheren van de geschiedenis van uw Web-taken

**taak geschiedenis sitelijst [opties] [taaknaam] [naam]**

Deze opdracht geeft een geschiedenis van de uitgevoerd van de opgegeven web-taak.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam >: vereist. De naam van de webjob.
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

**site-werkervaring [opties] [taaknaam] [runId] [naam] weergeven**

Deze opdracht krijgt de details van de taak voor de taak opgegeven web uitgevoerd.

Deze opdracht ondersteunt de volgende extra opties:

+ **--Taaknaam** &lt;taaknaam >: vereist. De naam van de webjob.
+ **--uitvoeren-id** &lt;uitvoeren-id >: optioneel. De id van de uitvoeren geschiedenis. Als u niet opgeeft, weergeven u de meest recente uitvoeren.
+ **--slot** &lt;slot >: de naam van de slot om opnieuw te starten.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Opdrachten voor het beheren van uw web-app diagnostische gegevens

**site-logboek downloadopties [] [naam]**

Een ZIP-bestand met diagnostische hulpprogramma's van uw web-app downloaden.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**site log uiteinde [opties] [naam]**

Deze opdracht wordt uw terminal verbonden met de log streaming-service.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**site log [opties] [naam] instellen**

Deze opdracht zorgt ervoor dat de diagnostische opties voor uw web-app.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Opdrachten voor het beheren van uw web-app opslagplaatsen

**site opslagplaats tak [opties] &lt;tak > [naam]**

**site opslagplaats verwijderen [opties] [naam]**

**site-bibliotheek synchroniseren [opties] [naam]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Opdrachten voor het beheren van de schaalbaarheid van uw web app

**site schaalmodus [opties] &lt;modus > [naam]**

**site-schaal exemplaren [opties] &lt;exemplaren > [naam]**


## <a name="commands-to-manage-azure-mobile-services"></a>Opdrachten voor het beheren van Azure Mobile Services

Azure Mobile Services bij elkaar brengt een aantal Azure services waarmee backend mogelijkheden voor uw apps. Mobile-Services-opdrachten zijn onderverdeeld in de volgende categorieën:

+ [Opdrachten voor het beheer van mobiele service-exemplaren](#Mobile_Services)
+ [Opdrachten voor het beheer van mobiele service te configureren](#Mobile_Configuration)
+ [Opdrachten voor het beheer van mobiele-tabellen](#Mobile_Tables)
+ [Opdrachten voor het beheer van mobiele-scripts](#Mobile_Scripts)
+ [Opdrachten voor het beheren van geplande taken](#Mobile_Jobs)
+ [Opdrachten aan de nieuwe schaal een mobiele service](#Mobile_Scale)

De volgende opties zijn van toepassing op de meeste Mobile Services-opdrachten:

+ **-h** of **--help**: informatie over het gebruik van weergave-uitvoer.
+ **-s `<id>` ** of **--abonnement `<id>` **: gebruik een bepaald abonnement, opgegeven als `<id>`.
+ **-v** of **--uitgebreide**: uitgebreide uitvoer schrijven.
+ **--json**: schrijven JSON-uitvoer.

### <a name="Mobile_Services"></a>Opdrachten voor het beheer van mobiele service-exemplaren

**mobiele locaties [opties]**

Deze opdracht bevat geografische locaties die worden ondersteund door mobiele Services.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobile maken [opties] [servicenaam] [sqlAdminUsername] [sqlAdminPassword]**

Deze opdracht Hiermee maakt u een mobiele service samen met een SQL-Database en de server.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **- r `<sqlServer>` ** of **--SQL Server `<sqlServer>` **: gebruik een bestaande SQL-Database-server, opgegeven als `<sqlServer>`.
+ **-d `<sqlDb>` ** of **--sqlDb `<sqlDb>` **: gebruik bestaande SQL-database, opgegeven als `<sqlDb>`.
+ **-l `<location>` ** of **--locatie `<location>` **: de service maken in een specifieke locatie, opgegeven als `<location>`. Azure mobiele locaties om te krijgen van de beschikbare locaties worden uitgevoerd.
+ **--sqlLocation `<location>` **: de SQL server maken in een specifiek `<location>`; standaardwaarden naar de locatie van de mobiele service.

**mobiele verwijderen [opties] [servicenaam]**

Deze opdracht verwijderd een mobiele service samen met de SQL-Database en de server.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **+ d** of **--deleteData**: alle gegevens verwijderen uit deze mobiele service van de database.
+ **-a** of **--deleteAll**: Verwijder de SQL-Database en de server.
+ **-q** of **--stille**: niet vragen om bevestiging. Gebruik deze optie in geautomatiseerde scripts.

**mobiele lijst [opties]**

Deze opdracht bevat uw mobiele services.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**mobiele weergeven [opties] [servicenaam]**

Deze opdracht weergegeven details over een mobiele service.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**mobiele opnieuw starten [opties] [servicenaam]**

Deze opdracht opnieuw is opgestart een exemplaar van mobiele service.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**mobiele log [opties] [servicenaam]**

Deze opdracht het resultaat mobiele service Logboeken, het wegfilteren van alle log typen, maar `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **- r `<query>` ** of **--query `<query>` **: wordt de opgegeven logboek query uitgevoerd.
+ **-t `<type>` ** of **--type `<type>` **: de resulterende Logboeken filteren op invoer `<type>`, die kan zijn `information`, `warning`, of `error`.
+ **-k `<skip>` ** of **--overslaan `<skip>` **: Hiermee worden overgeslagen het aantal rijen dat is opgegeven door `<skip>`.
+ **-p `<top>` ** of **--boven `<top>` **: geeft als resultaat een bepaald aantal rijen, opgegeven door `<top>`.

> [AZURE.NOTE] De **--query** parameter voorrang op **--type**, **--overslaan**, en **--boven**.

**mobiele herstellen [opties] [unhealthyservicename] [healthyservicename]**

Deze opdracht herstelt een beschadigde mobiele service door dit te verplaatsen naar een correct mobiele service in een andere regio.

Deze opdracht ondersteunt de volgende aanvullende optie:

**-q** of **--stille**: de prompt voor bevestiging van herstel.

**mobiele sleutel genereren [opties] [servicenaam] [type]**

Deze opdracht genereert u deze opnieuw de toepassingstoets telefoonprovider.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Typen sleutels zijn `master` en `application`.

> [AZURE.NOTE] Wanneer u opnieuw toetsen genereren, is het mogelijk dat clients die gebruikmaken van de oude sleutel geen toegang tot uw telefoonprovider. Wanneer u de toepassingstoets genereren, moet u uw app bijwerken met de nieuwe sleutelwaarde.

**mobiele toets [opties] [servicenaam] [type] [waarde] instellen**

Deze opdracht Hiermee stelt u de mobiele service-toets op een bepaalde waarde.


### <a name="Mobile_Configuration"></a>Opdrachten voor het beheer van mobiele service te configureren

**lijst met mobiele zoekconfiguraties [opties] [servicenaam]**

Deze opdracht lijsten configuratieopties voor een mobiele service.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**mobiele config krijgen [opties] [servicenaam] [toets]**

Deze opdracht krijgt een optie voor specifieke configuratie voor een mobiele service, in dit geval dynamische schema.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**mobiele config [opties] [servicenaam] [toets] [waarde] instellen**

Deze opdracht stelt u een specifieke configuratie-optie voor een mobiele service, in dit geval dynamische schema.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Opdrachten voor het beheer van mobiele-tabellen

**mobiele Tabellijst [opties] [servicenaam]**

Deze opdracht bevat alle tabellen in uw telefoonprovider.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**mobiele tabel [opties] [servicenaam] [tabelnaam] weergeven**

Geeft als resultaat informatie over een bepaalde tabel ziet u deze opdracht.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**mobiele tabel [opties] [servicenaam] [tabelnaam] maken**

Deze opdracht maakt een tabel.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Deze opdracht ondersteunt de volgende aanvullende optie:

+ **-p `&lt;permissions>` ** of **--machtigingen `&lt;permissions>` **: door komma's gescheiden lijst met `<operation>` = `<permission>` paren, waar `<operation>` is `insert`, `read`, `update`, of `delete` en `&lt;permissions>` is `public`, `application` (standaard), `user`, of `admin`.

**mobiele gegevens lezen [opties] [servicenaam] [tabelnaam] [query]**

Deze opdracht leest gegevens van een tabel.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-k `<skip>` ** of **--overslaan `<skip>` **: Hiermee worden overgeslagen het aantal rijen dat is opgegeven door `<skip>`.
+ **-t `<top>` ** of **--boven `<top>` **: geeft als resultaat een bepaald aantal rijen, opgegeven door `<top>`.
+ **-l** of **--lijst**: haalt gegevens op in een lijstindeling.

**mobiele tabel bijwerken [opties] [servicenaam] [tabelnaam]**

Deze opdracht wijzigt machtigingen voor het verwijderen van een tabel alleen voor beheerders.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-p `&lt;permissions>` ** of **--machtigingen `&lt;permissions>` **: door komma's gescheiden lijst met `<operation>` = `<permission>` paren, waar `<operation>` is `insert`, `read`, `update`, of `delete` en `&lt;permissions>` is `public`, `application` (standaard), `user`, of `admin`.
+ **--deleteColumn `<columns>` **: door komma's gescheiden lijst met kolommen die u wilt verwijderen, als `<columns>`.
+ **-q** of **--stille**: Hiermee verwijdert u kolommen zonder waarin u wordt gevraagd om bevestiging.
+ **--addIndex `<columns>` **: door komma's gescheiden lijst met kolommen die u wilt opnemen in de index.
+ **--deleteIndex `<columns>` **: door komma's gescheiden lijst met kolommen die u wilt uitsluiten van de index.

**mobiele tabel [opties] [servicenaam] [tabelnaam] verwijderen**

Een tabel met deze opdracht verwijderd.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Geef de parameter - q als de tabel zonder bevestiging wilt verwijderen. Werkwijze om te voorkomen dat het blokkeren van automatische scripts.

**mobiele gegevens afkappen [opties] [servicenaam] [tabelnaam]**

Deze opdracht Hiermee verwijdert u alle rijen met gegevens uit de tabel.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Opdrachten voor het beheren van scripts

Opdrachten in deze sectie worden gebruikt voor het beheren van de serverscripts die deel uitmaakt van een mobiele service. Voor meer informatie raadpleegt u [werken met de server-scripts in Mobile-Services](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**mobiele Scriptlijst [opties] [servicenaam]**

Deze opdracht bevat geregistreerde scripts, inclusief zowel de tabel en de scheduler scripts.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**mobiele script downloaden [opties] [servicenaam] [scriptnaam]**

Deze opdracht het script invoegen uit de tabel TodoItem is gedownload naar een bestand met de naam `todoitem.insert.js` in de `table` submap.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-p `<path>` ** of **--pad `<path>` **: de locatie in het bestand waarin het script opslaan waar de huidige werkmap de standaardwaarde is.
+ **-f `<file>` ** of **--bestand `<file>` **: de naam van het bestand op waarin het script opslaan.
+ **-o** of **--overschrijven**: een bestaand bestand overschrijven.
+ **-c** of **--console**: het script aan de console in plaats van naar een bestand te schrijven.

**mobiele script upload [opties] [servicenaam] [scriptnaam]**

Deze opdracht uploads een script met de naam `todoitem.insert.js` uit de `table` submap.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

De naam van het bestand moet bestaan uit de tabel en bewerking namen. Dit moet zich bevinden in de tabel submap ten opzichte van de locatie waar de opdracht wordt uitgevoerd. U kunt ook de **-f `<file>` ** of **--bestand `<file>` ** parameter om op te geven van een andere bestandsnaam en pad naar het bestand met het script om te registreren.


**mobiele script [opties] [servicenaam] [scriptnaam] verwijderen**

Deze opdracht Hiermee verwijdert u het bestaande invoegen script uit de tabel TodoItem.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Opdrachten voor het beheren van geplande taken

Opdrachten in deze sectie worden gebruikt voor het beheren van geplande taken die deel uitmaakt van een mobiele service. Zie [taken plannen](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx)voor meer informatie.

**mobiele takenlijst [opties] [servicenaam]**

Deze opdracht bevat de geplande taken.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**mobiele taak maken [opties] [servicenaam] [taaknaam]**

Deze opdracht maakt u een taak met de naam `getUpdates` die is gepland per uur.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-i `<number>` ** of **--interval `<number>` **: het taakinterval, als een geheel getal. De standaardwaarde is `15`.
+ **-u `<unit>` ** of **--intervalUnit `<unit>` **: de indeling van de eenheid voor het _interval_, die een van de volgende waarden:
    + **minuut** (standaard)
    + **uur**
    + **dag**
    + **maand**
    + **geen** (op aanvraag taken)
+ **-t `<time>`** **--starttijd `<time>` ** De begintijd van de eerste uitvoeren voor het script in ISO-indeling. De standaardwaarde is `now`.

> [AZURE.NOTE] Nieuwe taken worden gemaakt in de fase uitgeschakeld, omdat een script nog steeds moet worden geüpload. Gebruik de opdracht **mobiele script uploaden** om een script en de opdracht **mobiele taak bijwerken** om te schakelen van de taak te uploaden.

**mobiele taak update [opties] [servicenaam] [taaknaam]**

De volgende opdracht wordt de uitgeschakeld `getUpdates` taak.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **-i `<number>` ** of **--interval `<number>` **: het taakinterval, als een geheel getal. De standaardwaarde is `15`.
+ **-u `<unit>` ** of **--intervalUnit `<unit>` **: de indeling van de eenheid voor het _interval_, die een van de volgende waarden:
    + **minuut** (standaard)
    + **uur**
    + **dag**
    + **maand**
    + **geen** (op aanvraag taken)
+ **-t `<time>`** **--starttijd `<time>` ** De begintijd van de eerste uitvoeren voor het script in ISO-indeling. De standaardwaarde is `now`.
+ **- een `<status>` ** of **--status `<status>` **: de taakstatus, kan `enabled` of `disabled`.

**mobiele taak [opties] [servicenaam] [taaknaam] verwijderen**

Deze opdracht verwijdert de geplande taak getUpdates uit de takenlijst van server.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Verwijderen van een taak ook Hiermee verwijdert u de geüploade script.

### <a name="Mobile_Scale"></a>Opdrachten aan de nieuwe schaal een mobiele service

Opdrachten in deze sectie worden gebruikt om de schaal van een mobiele service. Zie de [schaal van een mobiele service](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx)voor meer informatie.

**mobiele schaal [opties] [servicenaam] weergeven**

Deze opdracht weergegeven schaal informatie, inclusief huidige berekeningscluster modus en het aantal exemplaren.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**mobiele schaal [opties] [servicenaam] wijzigen**

Deze opdracht verandert de schaal van de mobiele service van gratis premium-modus.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **- c `<mode>` ** of **--computeMode `<mode>` **: de modus berekeningscluster moet een `Free` of `Reserved`.
+ **-i `<count>` ** of **--numberOfInstances `<count>` **: het aantal exemplaren gebruikt wanneer u gereserveerde afspeelt.

> [AZURE.NOTE] Wanneer u een berekeningscluster modus instelt op `Reserved`, al uw mobiele services in hetzelfde gebied uitgevoerd in de premium-modus.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Opdrachten voor de preview-functies inschakelen voor uw Mobile-Service

**mobiele preview lijst [opties] [servicenaam]**

Deze opdracht geeft de preview-functies die beschikbaar zijn op de opgegeven service en of ze zijn ingeschakeld.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**mobiele preview [opties] [servicenaam] [onderdeel] inschakelen**

Deze opdracht kunt de opgegeven voorbeeldfunctie voor een mobiele service. Eenmaal ingeschakeld, kunnen geen preview-functies worden uitgeschakeld voor een mobiele service.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Opdrachten voor het beheren van uw mobiele service API 's

**mobiele api lijst [opties] [servicenaam]**

Deze opdracht geeft een lijst mobiele service aangepaste API's die u hebt gemaakt voor uw mobiele service.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**mobiele api maken [opties] [servicenaam] [apiname]**

Hiermee maakt u een aangepaste mobiele service-API

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Deze opdracht ondersteunt de volgende aanvullende optie:

**-p** of **--machtigingen** &lt;machtigingen >: een door komma's gescheiden lijst met &lt;methode > =&lt;machtiging > paren.

**mobiele api update [opties] [servicenaam] [apiname]**

Deze opdracht de opgegeven mobiele service aangepaste API bijgewerkt.

Deze opdracht ondersteunt de volgende aanvullende optie:

Deze opdracht ondersteunt de volgende extra opties:

+ **-p** of **--machtigingen** &lt;machtigingen >: een door komma's gescheiden lijst met &lt;methode > =&lt;machtiging > paren.
+ **-f** of **--afdwingen**: overschrijft eventuele wijzigingen die u naar het bestand dat machtigingen metagegevens.

**mobiele api [opties] [servicenaam] [apiname] verwijderen**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Deze opdracht verwijderd uit de opgegeven mobiele service aangepaste API.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Opdrachten voor het beheren van uw mobiele app toepassingsinstellingen

**mobiele appsetting lijst [opties] [servicenaam]**

Deze opdracht geeft u de instellingen voor de mobiele toepassing-app voor de opgegeven service.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**mobiele appsetting toevoegen [opties] [servicenaam] [naam] [waarde]**

Deze opdracht voegt u een aangepaste toepassingsinstelling voor uw mobiele service.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**mobiele appsetting verwijderen [opties] [servicenaam] [naam]**

Deze opdracht Hiermee verwijdert u de instelling van de opgegeven toepassing voor uw telefoonprovider.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**mobiele appsetting [opties] [servicenaam] [naam] weergeven**

Deze opdracht Hiermee verwijdert u de instelling van de opgegeven toepassing voor uw telefoonprovider.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Hulpmiddel lokale instellingen beheren

Instellingen voor lokale zijn uw abonnements-ID en de standaard-Opslagaccountnaam.

**lijst met zoekconfiguraties [opties]**

Deze opdracht weergegeven configuratie-instellingen.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**config [opties] instellen &lt;naam&gt;,&lt;waarde&gt;**

Deze opdracht wijzigt een instelling voor de zoekconfiguratie.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Opdrachten voor het beheren van Service Bus

Deze opdrachten gebruiken voor het beheren van uw Service Bus-account

**SB naamruimte selectievakje [opties] &lt;naam >**

Controleer of de naamruimte van een service bus juridische en beschikbaar is.

**SB naamruimte maken &lt;naam > &lt;locatie >**

Hiermee maakt u een naamruimte Service Bus.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**SB naamruimte verwijderen &lt;naam >**

Verwijder een naamruimte.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**SB naamruimte lijst**

Lijst met alle naamruimten hebt gemaakt voor uw account.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**lijst met locaties van SB naamruimte**

Een lijst met alle beschikbare naamruimte locaties weergegeven.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**SB naamruimte weergeven &lt;naam >**

Meer informatie over een specifieke naamruimte weergeven.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**SB naamruimte verifiëren &lt;naam >**

Controleer of de naamruimte beschikbaar is.

## <a name="commands-to-manage-your-storage-objects"></a>Opdrachten voor het beheren van uw opslag-objecten

###<a name="commands-to-manage-your-storage-accounts"></a>Opdrachten voor het beheren van uw opslag-accounts

**lijst met accounts van opslag [opties]**

Deze opdracht geeft de accounts van de opslag van uw abonnement.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**opslag account weergeven [opties]<name>**

Deze opdracht wordt informatie over het opgegeven opslag-account met inbegrip van de eigenschappen URI en -account.

**opslag-account maken [opties]<name>**

Deze opdracht maakt een opslag-account op basis van de opgegeven opties.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **e -** of **--label** &lt;label >: het label voor de opslag-account.
+ **-d** of **--Beschrijving** &lt;beschrijving >: de beschrijving opslag-account.
+ **-l** of **--locatie** &lt;naam >: de geografische regio waarin u de opslag-account kunt maken.
+ **-a** of **--affiniteit-groep** &lt;naam >: de groep affiniteit waaraan het account opslag gekoppeld. 
+ **--type**: aangegeven welk type account dat u wilt maken: een standaard-opslag met redundantie optie (LRS/ZRS/GRS/RAGRS) of Premium opslag (PLRS).

**opslag-account instellen [opties]<name>**

Deze opdracht bijgewerkt het opgegeven opslag-account.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Deze opdracht ondersteunt de volgende extra opties:

+ **e -** of **--label** &lt;label >: het label voor de opslag-account.
+ **-d** of **--Beschrijving** &lt;beschrijving >: de beschrijving opslag-account.
+ **-l** of **--locatie** &lt;naam >: de geografische regio waarin u de opslag-account kunt maken.
+ **--type**: geeft aan dat het nieuwe type account: een standaard-opslag met redundantie optie (LRS/ZRS/GRS/RAGRS) of Premium opslag (PLRS).

**opslag account verwijderen [opties]<name>**

Deze opdracht Hiermee verwijdert u het opgegeven opslag-account.

Deze opdracht ondersteunt de volgende aanvullende optie:

**-q** of **--stille**: niet vragen om bevestiging. Gebruik deze optie in geautomatiseerde scripts.

###<a name="commands-to-manage-your-storage-account-keys"></a>Opdrachten voor het beheren van uw opslagruimte account sleutels

**[opties] voor de opslag account toetsen van de lijst<name>**

Deze opdracht bevat de primaire en secundaire sleutels voor het opgegeven opslag-account.

**opslag account toetsen verlengen [opties]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Opdrachten voor het beheren van uw container opslag

**lijst van de container opslag [opties] [voorvoegsel]**

Deze opdracht geeft de lijst van de container opslagruimte voor een opgegeven opslag-account. Het account opslag is opgegeven met de verbindingsreeks of de naam en account-toets voor de account voor opslagruimte.

Deze opdracht ondersteunt de volgende extra opties:

+ **-p** of **-voorvoegsel** &lt;voorvoegsel >: voorvoegsel van de container opslag.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslag wordt uitgevoerd in de foutopsporingsmodus voor.

**opslag container weergeven [opties] [container]**
**opslag container maken [opties] [container]**

Deze opdracht Hiermee maakt u een container opslag voor het opgegeven opslag-account. Het account opslag is opgegeven met de verbindingsreeks of de naam en account-toets voor de account voor opslagruimte.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-p** of **-voorvoegsel** &lt;voorvoegsel >: voorvoegsel van de container opslag.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag
+ **--Foutopsporing**: de opdracht opslag wordt uitgevoerd in de foutopsporingsmodus voor.

**opslag container verwijderen [opties] [container]**

Deze opdracht Hiermee verwijdert u de container opgegeven opslag. Het account opslag is opgegeven met de verbindingsreeks of de naam en account-toets voor de account voor opslagruimte.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-p** of **-voorvoegsel** &lt;voorvoegsel >: voorvoegsel van de container opslag.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslag wordt uitgevoerd in de foutopsporingsmodus voor.

**opslag container [opties] [container] instellen**

Deze opdracht ingesteld toegangsbeheerlijst voor de container opslag. Het account opslag is opgegeven met de verbindingsreeks of de naam en account-toets voor de account voor opslagruimte.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-p** of **-voorvoegsel** &lt;voorvoegsel >: voorvoegsel van de container opslag.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslag wordt uitgevoerd in de foutopsporingsmodus voor.

###<a name="commands-to-manage-your-storage-blob"></a>Opdrachten voor het beheren van uw blob Storage

**lijst van de blob Storage [opties] [container] [voorvoegsel]**

Deze opdracht geeft als resultaat een lijst met de BLOB storage's in de container opgegeven opslag.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-p** of **-voorvoegsel** &lt;voorvoegsel >: voorvoegsel van de container opslag.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslag wordt uitgevoerd in de foutopsporingsmodus voor.

**opslag blob [opties] [container] [blob] weergeven**

Deze opdracht weergegeven de details van de opgegeven opslag-label.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-p** of **-voorvoegsel** &lt;voorvoegsel >: voorvoegsel van de container opslag.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslagruimte in foutopsporing wordt uitgevoerd.

**opslag blob [opties] [container] [blob] verwijderen**

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-b** of **--blob** &lt;blobName >: de naam van de blob storage te verwijderen.
+ **-q** of **--stille**: verwijderen van de opgegeven blob Storage zonder bevestiging.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslagruimte in foutopsporing wordt uitgevoerd.

**opslag blob upload [opties] [bestand] [container] [blob]**

Deze opdracht wordt het opgegeven bestand uploaden naar de specified\ opslag blob.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-b** of **--blob** &lt;blobName >: de naam van de blob storage te uploaden.
+ **-t** of **--blobtype** &lt;blobtype >: het type van de blob storage: pagina of blok.
+ **-p** of **--Eigenschappen** &lt;eigenschappen >: de eigenschappen van de blob storage voor geüploade bestand. Eigenschappen zijn toets = waarde paar s en gescheiden met plaats. Beschikbare eigenschappen zijn contentType, contentEncoding contentLanguage en cacheControl.
+ **-m** of **--metagegevens** &lt;metagegevens >: de metagegevens van de blob storage voor geüploade bestand. Metagegevens volgen belangrijke = waardeparen een d gescheiden door puntkomma (;).
+ **--concurrenttaskcount** &lt;concurrenttaskcount >: het maximum aantal gelijktijdige upload aanvragen.
+ **-q** of **--stille**: de opgegeven blob Storage zonder bevestiging overschrijven.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslagruimte in foutopsporing wordt uitgevoerd.

**downloaden van de blob Storage [opties] [container] [blob] [doel]**

Deze opdracht downloads van de opgegeven opslag blob.

Deze opdracht ondersteunt de volgende extra opties:

+ **--container** &lt;container >: de naam van de container opslag maken.
+ **-b** of **--blob** &lt;blobName >: de naam van de blob storage.
+ **-d** of **--bestemming** [bestemming]: de bestemming bestand of map pad voor het downloaden.
+ **-m** of **--checkmd5**: het selectievakje md5sum voor het gedownloade bestand.
+ **--concurrenttaskcount** &lt;concurrenttaskcount > het maximum aantal gelijktijdige upload aanvragen
+ **-q** of **--stille**: het doelbestand zonder bevestiging overschrijven.
+ **-a** of **--accountnaam** &lt;accountnaam >: de naam van het opslag-account.
+ **-k** of **--accountsleutel** &lt;accountKey >: de accountsleutel opslag.
+ **-c** of **--verbindingsreeks** &lt;connectionString >: de verbindingsreeks opslag.
+ **--Foutopsporing**: de opdracht opslagruimte in foutopsporing wordt uitgevoerd.

## <a name="commands-to-manage-sql-databases"></a>Opdrachten voor het beheren van de SQL-Databases

Deze opdrachten gebruiken voor het beheren van uw Azure SQL-Databases

###<a name="commands-to-manage-sql-servers"></a>Opdrachten voor het beheren van de SQL-Servers.

Deze opdrachten gebruiken voor het beheren van uw SQL-Servers

**SQL server maken &lt;administratorLogin > &lt;administratorPassword > &lt;locatie >**

Maken van een databaseserver

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**SQL server weergeven &lt;naam >**

Serverdetails weergeven.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**SQL server-lijst**

De lijst met servers krijgen.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL server verwijderen &lt;naam >**

Hiermee verwijdert u een server

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Opdrachten voor het beheren van de SQL-Databases

Deze opdrachten gebruiken voor het beheren van uw SQL-Databases.

**SQL-db maken [opties] &lt;servernaam > &lt;databasenaam > &lt;administratorPassword >**

Hiermee wordt een exemplaar van de database gemaakt

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL-db [opties] weergeven &lt;servernaam > &lt;databasenaam > &lt;administratorPassword >**

Informatie over de database weergeven.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**lijst met SQL-db [opties] &lt;servernaam > &lt;administratorPassword >**

Lijst met de databases.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL-db verwijderen [opties] &lt;servernaam > &lt;databasenaam > &lt;administratorPassword >**

Hiermee verwijdert u een database.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Opdrachten voor het beheren van uw SQL Server-firewallregels

Deze opdrachten voor het beheren van uw SQL Server-firewallregels gebruiken

**SQL-firewallrule maken [opties] &lt;servernaam > &lt;ruleName > &lt;startIPAddress > &lt;endIPAddress >**

Een firewallregel maken voor een SQL Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL-firewallrule [opties] weergeven &lt;servernaam > &lt;ruleName >**

Firewall regeldetails weergeven.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**lijst met SQL-firewallrule [opties] &lt;servernaam >**

Lijst met de firewallregels.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL-firewallrule verwijderen [opties] &lt;servernaam > &lt;ruleName >**

Deze opdracht Hiermee verwijdert u een firewallregel.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Opdrachten voor het beheren van uw virtuele netwerken

Deze opdrachten gebruiken voor het beheren van uw virtuele netwerken

**netwerk vnet maken [opties] &lt;locatie >**

Maak een virtueel netwerk.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**vnet weergeven netwerk &lt;naam >**

Details van een virtueel netwerk weergeven.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**lijst met netwerken-vnet**

Lijst met alle bestaande virtuele netwerken.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**verwijderen van netwerk vnet &lt;naam >**

Hiermee verwijdert u het opgegeven virtuele netwerk.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**netwerk exporteren [bestandspad]**

U kunt uw netwerkconfiguratie lokaal exporteren voor geavanceerde configuratie. De geëxporteerde netwerkconfiguratie bevat DNS server settings, virtuele netwerkinstellingen, site-instellingen voor lokale netwerk en andere instellingen.

**netwerk importeren [bestandspad]**

Een lokale netwerkconfiguratie importeren.

**netwerk-DNS-server registreren [opties] &lt;dnsIP >**

Een DNS-server die u wilt gebruiken voor naamresolutie in uw netwerkconfiguratie registreren.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**lijst met netwerken-DNS-server**

Lijst met alle DNS-servers in uw netwerkconfiguratie geregistreerd.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**netwerk-DNS-server unregister [opties] &lt;dnsIP >**

Hiermee verwijdert de invoer in een DNS-server uit de netwerkconfiguratie.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

