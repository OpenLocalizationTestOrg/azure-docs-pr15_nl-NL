<properties
   pageTitle="Interactief werken met Service stof clusters met CLI | Microsoft Azure"
   description="Het gebruik van Azure CLI om te communiceren met een cluster Service stof"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Gebruik van de Azure CLI om te communiceren met een Service stof Cluster

U kunt werken met Service stof cluster met Linux machines met de CLI Azure op Linux.

De eerste stap is get de nieuwste versie van de CLI uit de verkoper cijfer en stelt u dit in het pad met de volgende opdrachten:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Voor elke opdracht, wordt ondersteund, kunt u de naam van de opdracht voor de help voor deze opdracht typen. Automatisch aanvullen wordt ondersteund voor de opdrachten. De volgende opdracht krijgt bijvoorbeeld u help voor alle opdrachten in de toepassing. 

```sh
 azure servicefabric application 
```

Verder kunt u de help voor specifieke opdracht filteren als het volgende voorbeeld wordt getoond:

```sh
 azure servicefabric application  create
```

Als u automatisch aanvullen in de CLI, voert u de volgende opdrachten:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

De volgende opdrachten verbinding maken met het cluster en leert u de knooppunten in het cluster:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Als u benoemde parameters gebruiken en vinden wat ze zijn, kunt u typen--helpen na een opdracht. Bijvoorbeeld:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Wanneer u verbinding maakt met een meerdere machines cluster vanaf een computer **die geen deel uitmaakt van het cluster**, gebruikt u de volgende opdracht uit:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Vervang de tag PublicIPorFQDN door de reële IP- of FQDN zo nodig. Wanneer u verbinding maakt met een meerdere machines cluster vanaf een computer **dat wil zeggen een deel van het cluster**, gebruikt u de volgende opdracht uit:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

U kunt PowerShell of CLI om te communiceren met uw Cluster Linux Service stof gemaakt via de portal van Azure. 

**Let op:** Deze clusters niet is beveiligd, dus u mogelijk worden openen van uw account en-klare door het openbare IP-adres toe te voegen in het manifest cluster.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Gebruik van de Azure CLI verbinding maken met een Service stof Cluster

De volgende opdrachten van Azure CLI wordt beschreven hoe verbinding maken met een beveiligde cluster. De details van het certificaat moeten overeenkomen met een certificaat knooppunten.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Als uw certificaat heeft certificeringsinstanties (CA's), moet u de parameter--ca-certificaat-path zoals in het volgende voorbeeld toevoegen: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Als u meerdere certificeringsinstanties hebt, gebruikt u een puntkomma als scheidingsteken.
 
Als de naam van de algemene in het certificaat niet overeenkomen met het verbindingseindpunt, kunt u de parameter `--strict-ssl` , worden de verificatie omzeild, zoals wordt weergegeven in de volgende opdracht uit: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Als u de verificatie CA overslaan wilt, zou u de--parameter negeren onbevoegde zoals wordt getoond in de volgende opdracht uit: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Nadat u verbinding maakt, moet u mogelijk zijn andere opdrachten CLI om te communiceren met het cluster kunt uitvoeren. 

## <a name="deploying-your-service-fabric-application"></a>Uw Service stof-toepassing implementeren

De volgende opdrachten kopiëren, registreren en starten van de servicetoepassing voor stof uitvoeren:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Een upgrade van uw toepassing

Het proces is vergelijkbaar met het [proces dat in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Maken, kopiëren, registreren en uw toepassing maken vanaf de hoofdmap project. Als uw toepassingsexemplaar stof heet: / MySFApp en het type is MySFApp, de opdrachten zou als volgt:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Breng de wijzigingen aan in uw toepassing en de gewijzigde-service opnieuw.  Bijwerken van de gewijzigde service manifest-bestand (ServiceManifest.xml) met de bijgewerkte versies voor de Service (en Code of Config of gegevens zo nodig). Ook wijzigen van de toepassing manifest (ApplicationManifest.xml) met de bijgewerkte versienummer voor de toepassing en de gewijzigde-service.  Nu, kopiëren en registreren van uw bijgewerkte toepassing met de volgende opdrachten:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

U kunt nu de toepassing upgrade starten met de volgende opdracht uit:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

U kunt nu de upgrade van de toepassing met SFX controleren. In een paar minuten zou de toepassing zijn bijgewerkt.  U kunt ook een bijgewerkte app met een fout te en de functionaliteit voor het ongedaan maken van auto in service stof.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopiëren van het toepassingspakket is niet mogelijk

Controleer als `openssh` is geïnstalleerd. Standaard Ubuntu bureaublad niet wel hebt gedaan. Installeren met behulp van de volgende opdracht uit:

```
 sudo apt-get install openssh-server openssh-client**
```

Als het probleem zich blijft voordoen, schakelt u PAM voor ssh door te wijzigen van het **sshd_config** -bestand met de volgende opdrachten:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Als het probleem nog steeds blijft optreden, vergroot het aantal ssh sessies door de volgende opdrachten uit te voeren:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Met toetsen voor ssh verificatie (in plaats van wachtwoorden) wordt niet nog ondersteund (Aangezien het platform worden gebruikt voor ssh pakketten kopiëren), dus wachtwoordverificatie in plaats hiervan te gebruiken.


## <a name="next-steps"></a>Volgende stappen

De ontwikkelomgeving installeren en implementeren van een toepassing voor de Service stof aan een cluster Linux.
