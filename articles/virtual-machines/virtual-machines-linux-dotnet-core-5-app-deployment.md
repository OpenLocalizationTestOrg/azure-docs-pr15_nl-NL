<properties
   pageTitle="Automatiseren van toepassingsimplementatie waarbij VM extensies | Microsoft Azure"
   description="Azure virtuele machines DotNet Core zelfstudie"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Toepassingsimplementatie waarbij resourcemanager Azure-sjablonen

Zodra alle Azure infrastructurele vereisten zijn geïdentificeerd en omgezet in een sjabloon voor de implementatie, moet de werkelijke toepassingsimplementatie worden toegewezen. Toepassingsimplementatie hier verwijst naar de installatie van de werkelijke toepassing binaire bestanden naar Azure resources. Voor de steekproef muziek Store, .net Core, NGINX en toezichthouder moeten worden geïnstalleerd en geconfigureerd op elke virtuele machine. De binaire muziek Store bestanden moeten worden geïnstalleerd op de virtuele machine en de muziek Store-database gemaakt vooraf.

In dit document een gedetailleerd overzicht van hoe VM extensies toepassingsimplementatie- en configuratie van Azure virtuele machines kunnen automatiseren. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor een optimale ervaring, vooraf een exemplaar van de oplossing aan uw Azure-abonnement en werk samen met de sjabloon Azure resourcemanager implementeren. De volledige sjabloon vindt u hier: [Muziek Store-implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Configuratiescript

VM extensies zijn gespecialiseerde programma's die op virtuele machines voeren te leveren configuratie automatisering. Extensies zijn beschikbaar voor veel specifieke doeleinden, zoals anti-virus uitvoert, configuratie voor logboekregistratie en Docker configuratie. Een aangepast script-extensie kan worden gebruikt voor een script uitvoeren op een virtuele machine. Met het voorbeeld van de muziek Store is het snel aan de aangepast script-extensie voor de Ubuntu virtuele machines configureren en installeren van de muziek Store-servicetoepassing.

Voordat u met informatie over hoe VM extensies zijn gedefinieerd in een sjabloon Azure resourcemanager, controleert u het script die wordt uitgevoerd. Dit script configureert de Ubuntu virtuele machine als host voor de muziek Store-servicetoepassing. Wanneer wordt uitgevoerd, wordt het script alle nodig software, de muziek store-servicetoepassing installeren vanuit het besturingselement voor gegevensbronnen, en de database voorbereiden. 

Voor meer informatie over het hosten van een .net Core van toepassing op Linux, Zie [publiceren naar een productieomgeving Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> In dit voorbeeld is ter demonstratie.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>VM Script-extensie

VM extensies kan worden uitgevoerd ten opzichte van een virtuele machine opbouwen tegelijk door de resource extensie op te nemen in de sjabloon resourcemanager Azure. De extensie kan worden toegevoegd met de wizard Resources Visual Studio toevoegen of door een geldig JSON invoegen in de sjabloon. De resource Script extensie is genest binnen de resource VM; Dit kan worden bekeken in het volgende voorbeeld.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [VM Script extensie](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Zoals u ziet de onderstaande JSON waarop het script in GitHub zijn opgeslagen. Dit script kan ook worden opgeslagen in Azure-blobopslag. Azure resourcemanager sjablonen toestaan dat het script execution tekenreeks opgesteld zodat de sjabloon parameterwaarden kunnen worden gebruikt als parameters uitvoeren van scripts. In dit geval gegevens worden weergegeven bij het distribueren van de sjablonen en deze waarden vervolgens kunnen worden gebruikt bij het uitvoeren van het script.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Zie voor meer informatie over het gebruik van de extensie aangepast script [aangepast scriptextensies met resourcemanager sjablonen](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Volgende stap

<hr>

[Meer Azure resourcemanager sjablonen verkennen](https://github.com/Azure/azure-quickstart-templates)
