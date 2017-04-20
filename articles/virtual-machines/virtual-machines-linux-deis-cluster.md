<properties
   pageTitle="Een 3-knooppunt implementeren cluster Deis | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe u een 3-knooppunt cluster op Azure met een sjabloon van Azure resourcemanager Deis"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Een 3-knooppunt implementeren Deis cluster

In dit artikel begeleidt u bij de inrichting van een [Deis](http://deis.io/) cluster op Azure. Dit omvat alle stappen van de benodigde certificaten om te implementeren en schaalbaarheid van een toepassing voor de steekproef **gaat u** op het nieuw ingerichte cluster maken.

In het volgende diagram ziet u de architectuur van het gedistribueerde systeem. Een systeembeheerder worden beheerd door het gebruik van de cluster Deis hulpprogramma's zoals **deis** en **deisctl**. Verbindingen worden gemaakt door een Azure taakverdeling, die de verbindingen naar een van de lid knooppunten op het cluster doorstuurt. De toegang clients geïmplementeerd toepassingen via de verdeling van de belasting ook. In dit geval de taakverdeling doorgestuurd het verkeer naar een Deis router mesh, die verder routs verkeer naar de bijbehorende Docker containers die worden gehost op het cluster.

  ![Architectuurdiagram van geïmplementeerd Desis cluster](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Om te kunnen door de volgende stappen uitvoert, hebt u het volgende nodig:

 * Een actieve Azure-abonnement. Als u deze niet hebt, kunt u een gratis spoor verkrijgen op [azure.com](https://azure.microsoft.com/).
 * Een werk- of schoolaccount id Azure resourcegroepen kunt gebruiken. Als u een persoonlijk account en een logboek met een Microsoft-id hebt, moet u [een werk-id van uw persoonlijke kleur maken](virtual-machines-windows-create-aad-work-id.md).
 * Een--afhankelijk van uw clientbesturingssysteem--de [Azure PowerShell](../powershell-install-configure.md) of de [Azure CLI voor Mac, Linux, en Windows](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL wordt gebruikt om de benodigde certificaten.
 * Een cijfer-client zoals [Cijfer we vaker doen](https://git-scm.com/).
 * Als u wilt testen van de steekproef-toepassing, moet u eveneens een DNS-server. U kunt alle DNS-servers of services die ondersteuning jokertekens A records bieden.
 * Een computer om uit te voeren Deis clienthulpprogramma's. U kunt een lokale computer of een virtuele machine gebruiken. U kunt deze hulpprogramma's uitvoeren op vrijwel elk Linux-verdeling, maar de volgende instructies Ubuntu gebruiken.

## <a name="provision-the-cluster"></a>Het cluster inrichten

In deze sectie gebruikt u een sjabloon [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) uit de bron openen opslagplaats [azure-quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates). U moet eerst kopiëren naar beneden de sjabloon. Klik, maakt u nieuwe SSH combinatie van een sleutel voor verificatie. En vervolgens, hebt u een nieuwe id voor u cluster configureren. En ten slotte, gebruikt u de shellscript of de PowerShell-script om het cluster inrichten.

1. De bibliotheek klonen: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Ga naar de map met sjablonen:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Nieuwe SSH combinatie van een sleutel met ssh-keygen maken:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Een certificaat dat met de bovenstaande persoonlijke sleutel genereren:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Ga naar [https://discovery.etcd.io/new](https://discovery.etcd.io/new) om een nieuwe cluster token, welke er bijvoorbeeld te genereren:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Elk cluster CoreOS moet een unieke token van deze gratis service. Zie [CoreOS documentatie](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) voor meer informatie.

6. Wijzig de **cloud-config.yaml** -bestand om de bestaande **discovery** -token vervangen door het nieuwe token:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Wijzigen van het bestand **azuredeploy-parameters.json** : Open het certificaat dat u hebt gemaakt in stap 4 in een teksteditor. Kopieer alle tekst tussen `----BEGIN CERTIFICATE-----` en `-----END CERTIFICATE-----` in de **sshKeyData** -parameter (u moet verwijderen van alle nieuwe regeltekens).

8. Wijzig de parameter **newStorageAccountName** . Dit is het account opslag voor VM OS schijven. De naam van dit account heeft moet uniek zijn.

9. Wijzig de parameter **publicDomainName** . Hiermee wordt een onderdeel van de DNS-naam die is gekoppeld aan het laden de verdeling van openbare IP. De uiteindelijke FQDN hebben de indeling van _[waarde van deze parameter]_. _[regio]_. cloudapp.azure.com. Als u de naam als deishbai32 opgeven en de resourcegroep is geïmplementeerd in de regio West Amerikaans, wordt klikt u vervolgens de uiteindelijke FQDN naar uw taakverdeling bijvoorbeeld deishbai32.westus.cloudapp.azure.com.

10. Sla het parameterbestand. En vervolgens inrichten van het cluster via Azure PowerShell:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  of Azure CLI:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Nadat de resourcegroep is ingericht, kunt u alle resources in de groep weergeven op Azure klassieke portal. In de volgende schermafbeelding ziet de resourcegroep een virtueel netwerk met drie VMs, die zijn gekoppeld aan dezelfde beschikbaarheid van de set bevat. De groep bevat ook een taakverdeling, waarvoor een gekoppeld openbare IP.

  ![De ingerichte resourcegroep op Azure klassieke-portal](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>De client installeren

Moet u **deisctl** aan een besturingselement voor uw cluster Deis. Hoewel deisctl wordt automatisch geïnstalleerd in de knooppunten, is het verstandig om te deisctl op een aparte administratieve machine gebruiken. Bovendien omdat alle knooppunten met alleen privé IP-adressen zijn geconfigureerd, moet u SSH tunnel via de verdeling van de belasting, waarvoor een openbare IP-adres, verbinding maken met de computers knooppunt gebruiken. Hier volgen de stappen voor het instellen van deisctl op een aparte Ubuntu fysieke of virtuele machine.

1. Installatie deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Uw persoonlijke sleutel ssh-agent toevoegen:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Deisctl configureren:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

De sjabloon bepaalt binnenkomende NAT-regels die 2223 waarvan u een exemplaar van 1, 2224 op exemplaar 2 en 2225 op exemplaar 3 toewijzen. Hierdoor redundantie met de functie deisctl. U kunt deze regels op Azure klassieke portal controleren:

![NAT-regels op de taakverdeling](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] De sjabloon ondersteunt momenteel alleen 3 knooppunten bevatten. Dit is vanwege een beperking in Azure resourcemanager sjabloon NAT regel definiëren, die de syntaxis van de lus niet ondersteunt.

## <a name="install-and-start-the-deis-platform"></a>Installeer en start de Deis platform

Nu kunt u deisctl Installeer en start het platform Deis:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Vanaf het platform duurt (zoveel 10 minuten). Met name, het starten van de opbouwfunctie voor service erg lang duren. En soms duurt meerdere pogingen doen te kunnen uitvoeren: als de bewerking lijkt vastloopt, kunt u typen `ctrl+c` aan het einde van de uitvoering van de opdracht en probeer het opnieuw.

U kunt `deisctl list` om te controleren of alle services worden uitgevoerd:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Gefeliciteerd! Nu u een voorlopig hebt Deis clsuter op Azure! Vervolgens we implementeren van een steekproef-toepassing start het cluster in actie zien.

## <a name="deploy-and-scale-a-hello-world-application"></a>Implementeren en schaal van een toepassing Hallo allemaal.

De volgende stappen weergeven het implementeren van een 'Hallo wereld"toepassing aan het cluster gaan. De stappen zijn gebaseerd op [Deis documentatie](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Voor de routering mesh werkt alleen naar behoren, moet u een jokerteken A-record voor uw domein aan te wijzen op het openbare IP-adres van de taakverdeling hebt. De volgende schermafbeelding ziet u de A-record voor de registratie van een steekproef-domein op GoDaddy:

    ![Godaddy-A-record](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Installatie deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Maak een nieuwe SSH-sleutel en voegt u de openbare sleutel toe aan GitHub (uiteraard kunt u kunt ook opnieuw gebruiken uw bestaande sleutels). Als u wilt maken van nieuwe SSH combinatie van een sleutel, gebruiken:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Id_rsa.pub of de openbare sleutel van uw keuze wordt toevoegen aan GitHub. U kunt dit doen met behulp van de belangrijkste toevoegen SSH-knop in het scherm van de SSH toetsen configuratie:

  ![Github-toets](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Een nieuwe gebruiker hebt geregistreerd:

        deis register http://deis.[your domain]
<p />
6. De sleutel SSH toevoegen:

        deis keys:add [path to your SSH public key]
  <p />      
7. Maken van een toepassing.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.De push cijfer activeert Docker-afbeeldingen worden gemaakt en geïmplementeerd, die wordt een paar minuten duren. Vanuit Mijn ervaring, soms vastlopen stap 10 (Pushing afbeelding naar persoonlijke bibliotheek). Als dit gebeurt, kunt u het proces beëindigen, het gebruik van de toepassing verwijderen ' apps deis: destroy: een <application name> ` to remove the application and try again. You can use `deis apps:list' om vast te stellen de naam van uw toepassing. Als alles werkt, worden er ongeveer als volgt te werk aan het einde van de uitvoer van de opdracht:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Controleer of de toepassing nu wel werkt:

        curl -S http://[your application name].[your domain]
  U ziet het volgende:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. De schaal van de toepassing in 3-exemplaren aanpassen:

        deis scale cmd=3
<p />
11. Desgewenst kunt u info als u wilt onderzoeken details van uw toepassing deis. De uitvoer van de volgende zijn van mijn toepassingsimplementatie:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Volgende stappen

In dit artikel doorlopen u alle stappen voor het inrichten van een nieuw cluster op Azure met een sjabloon van Azure resourcemanager Deis. De sjabloon ondersteunt redundantie in gereedschap verbindingen, evenals van taakverdeling voor gebruikte toepassingen. De sjabloon vermijdt ook het openbare IP-adressen op lid knooppunten, waardoor wordt opgeslagen belangrijk openbare IP-resources en biedt een meer beveiligde omgeving naar hosttoepassingen gebruiken. Meer informatie raadpleegt u de volgende artikelen:

[Azure resourcemanager-overzicht] [resource-group-overview]  
[Het gebruik van de Azure CLI] [azure-command-line-tools]  
[Via Azure PowerShell met Azure resourcemanager] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
