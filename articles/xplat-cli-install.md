<properties
    pageTitle="Installeren van de Azure opdrachtregel | Microsoft Azure"
    description="Installeren van de Azure-Interface met opdrachtregel (CLI) voor Mac, Linux en Windows Azure services gebruiken"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Installeren van de Azure CLI

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

Snel de opdrachtregel Azure (Azure CLI) als u wilt een reeks open source shell-opdrachten gebruiken voor het maken en beheren van bronnen in Microsoft Azure installeren. Hebt u verschillende opties voor het installeren van deze platforms hulpprogramma's op uw computer: 

* **npm-pakket** - npm (het pakket manager voor JavaScript) de meest recente Azure CLI-pakket installeert op uw Linux verdeling of OS uitvoeren. Vereist node.js en npm op uw computer.
* **Installer** - Download een installatieprogramma voor een eenvoudige installatie op de Mac of Windows.
* **Docker container** - gebruik van de meest recente CLI in een kant-en-klaar Docker container. Vereist Docker host op uw computer.
    
Zie voor meer opties en achtergrond, het project-bibliotheek op [GitHub](https://github.com/azure/azure-xplat-cli). 

Zodra de CLI Azure is geïnstalleerd, [sluit u dit aan uw Azure-abonnement](xplat-cli-connect.md) en de opdrachten **azure** vanaf een interface voor de opdrachtregel (we vaker doen, Terminal opdrachtprompt en dergelijke) uitvoeren als u wilt werken met uw Azure resources.



## <a name="option-1-install-an-npm-package"></a>Optie 1: Een pakket met npm installeren

Als u wilt de CLI installeren vanaf een pakket met npm, zorg ervoor dat u hebt gedownload en geïnstalleerd de [meest recente Node.js en npm](https://nodejs.org/en/download/package-manager/). Voer vervolgens uit **npm installeren** om het pakket azure-cli te installeren: 

    npm install -g azure-cli

Klik op Linux onderzoeken moet u mogelijk **sudo** gebruiken om uit te voeren de opdracht __npm__ als volgt:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Als u wilt installeren of Node.js en npm op uw Linux verdeling of OS bijwerken, wordt u aangeraden dat u de meest recente versie van Node.js LTS (4.x) installeren. Als u een oudere versie gebruikt, krijgt u mogelijk fouten bij de installatie. 

Als u liever, downloadt u de meest recente Linux [tar bestand] [ linux-installer] voor het pakket npm lokaal. Vervolgens installeert u de gedownloade npm-pakket als volgt (op Linux onderzoeken moet u mogelijk **sudo**gebruiken):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Optie 2: Een installatieprogramma gebruiken

Als u een Mac of Windows-computer gebruikt, zijn de volgende CLI installatieprogramma downloaden:

* [Het installatieprogramma van Mac OS X][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]In Windows, kunt u ook het [Installatieprogramma van de Web-Platform](https://go.microsoft.com/?linkid=9828653) om te kunnen installeren van de CLI downloaden. Dit installatieprogramma kunt u de optie voor het installeren van extra Azure SDK en opdrachtregel extra na de installatie van de CLI. 


## <a name="option-3-use-a-docker-container"></a>Optie 3: Een container Docker gebruiken

Als u uw computer als host [Docker](https://docs.docker.com/engine/understanding-docker/) hebt ingesteld, kunt u de meest recente Azure CLI uitvoeren in een container Docker. Voer de volgende opdracht (op Linux onderzoeken moet u mogelijk **sudo**gebruiken):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Azure CLI opdrachten uitvoeren
Nadat de CLI Azure is geïnstalleerd, moet u de opdracht **azure** uitvoeren vanaf de opdrachtregel gebruikersinterface (we vaker doen, Terminal, opdrachtprompt, enzovoort). Bijvoorbeeld als u wilt de help-opdracht uitvoert, typt u het volgende:

```
azure help
```
> [AZURE.NOTE]Klik op sommige onderzoeken Linux u mogelijk een foutbericht vergelijkbaar met `/usr/bin/env: ‘node’: No such file or directory`. Deze fout afkomstig zijn uit recente installaties van Node.js wordt geïnstalleerd bij /usr/bin/nodejs. U lost dit op door een symbolic koppeling maken naar /usr/bin/node door deze opdracht uit te voeren:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Als u wilt zien van de versie van de Azure CLI die u hebt geïnstalleerd, typt u het volgende:

```
azure --version
```

U bent nu klaar! Voor toegang tot de CLI-opdrachten voor het werken met uw eigen resources, [verbinding maken met uw Azure abonnement van de Azure CLI](xplat-cli-connect.md).

>[AZURE.NOTE] Wanneer u eerst Azure CLI gebruikt, ziet u een bericht waarin wordt gevraagd of u toestaan van Microsoft wilt voor het verzamelen van informatie over het gebruik. Deelname is niet verplicht. Als u deelnemen wilt, kunt u stoppen op elk gewenst moment door te voeren `azure telemetry --disable`. Als u deelname op elk gewenst moment, voert u `azure telemetry --enable`.


## <a name="update-the-cli"></a>De CLI bijwerken

Microsoft brengt vaak bijgewerkte versies van de Azure CLI. De CLI met behulp van het installatieprogramma voor uw besturingssysteem opnieuw te installeren en uitvoeren van de meest recente Docker container. Of, als u hebt de meest recente Node.js en npm is geïnstalleerd, werkt u met de volgende (op Linux onderzoeken moet u mogelijk **sudo**gebruiken).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Tabblad voltooiing inschakelen

Tabblad aanvulling van CLI opdrachten wordt ondersteund voor Mac of Linux.

Als u deze in zsh, voert u het:

```
echo '. <(azure --completion)' >> .zshrc
```

Als u deze in we vaker doen, voert u het:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Volgende stappen 

* [Verbinding maken uit de CLI aan uw Azure-abonnement](xplat-cli-connect.md) maken en beheren van Azure resources.

* Meer informatie over de CLI Azure, broncode, problemen rapporteren, downloaden of bijdragen aan het project, gaat u naar de [bibliotheek GitHub voor de CLI Azure](https://github.com/azure/azure-xplat-cli).

* Als u vragen over het gebruik van de Azure CLI of Azure hebt, bezoekt u de [Azure-Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Als u wilt, kunt u ook de Python gebaseerde [Azure CLI 2.0 Preview](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
