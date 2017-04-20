<properties
   pageTitle="Verbinding maken met een cluster Azure Container-Service | Microsoft Azure"
   description="Verbinding maken met een cluster Azure Container-Service met behulp van een tunnel SSH."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, domeincontroller/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Verbinding maken met een cluster Azure Container-Service

De domeincontroller/OS en Docker Swarm clusters die zijn geïmplementeerd door Azure Container Service tonen REST eindpunten. Deze eindpunten zijn echter niet openen voor het externe netwerk. Als u wilt deze eindpunten worden beheerd, moet u een tunnel Secure Shell (SSH). Na een SSH is tunnel gebracht, kunt u opdrachten voor de eindpunten cluster uitgevoerd en het cluster UI via een browser bekijken op uw eigen systeem. In dit document begeleidt u bij het maken van een tunnel SSH uit Linux, OS X en Windows.

>[AZURE.NOTE] U kunt een sessie SSH maken met een cluster managementsysteem. We raden echter niet aan dit. Werkt u rechtstreeks aan een systeem voor projectmanagement beschrijft het risico voor per ongeluk worden configuratiewijzigingen.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Een tunnel SSH maken op Linux of OS X

Het eerste wat dat u volgt te werk wanneer u een tunnel SSH maakt op Linux of OS X is om te zoeken de naam van de openbare DNS-modellen verdeeld. Klik hiertoe uitvouwen resourcegroep zodat elke resource wordt weergegeven. Zoek en selecteer het openbare IP-adres van het model. Dit wordt geopend een blade met informatie over het openbare IP-adres, waaronder de DNS-naam. Sla deze naam voor later gebruik. <br />


![Openbare DNS-naam](media/pubdns.png)

Nu een shell opent en voer de volgende opdracht waar:

**Poort** is de poort van het eindpunt dat u wilt laten zien. Voor Swarm is dit 2375. Voor domeincontroller/OS, gebruikt u poort 80.  
**Gebruikersnaam** is de naam van de gebruiker die u hebt ontvangen wanneer u het cluster geïmplementeerd.  
**DNSPREFIX** is de DNS-voorvoegsel dat u hebt opgegeven toen u het cluster geïmplementeerd.  
**Regio** is de regio waarin uw resourcegroep zich bevindt.  
**PATH_TO_PRIVATE_KEY** [Optioneel] is het pad naar de persoonlijke sleutel die met de openbare sleutel die u hebt opgegeven overeenkomt toen u de Container Service cluster gemaakt. Gebruik deze optie met de i - markeren.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> De SSH verbinding poort is 2200--niet de standaardpoort 22.

## <a name="dcos-tunnel"></a>Domeincontroller/OS tunnel

Als u wilt een tunnel naar de eindpunten domeincontroller/OS-gerelateerde hebt geopend, kunt u een opdracht die vergelijkbaar is met de volgende uitvoeren:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

U kunt nu toegang tot de eindpunten domeincontroller/OS-gerelateerde voor:

- DOMEINCONTROLLER/BESTURINGSSYSTEEM:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

U kunt ook de rest API's voor elke toepassing via deze tunnel bereiken.

## <a name="swarm-tunnel"></a>Swarm tunnel

U opent een tunnel naar het eindpunt Swarm door uitvoeren een opdracht die er ongeveer als volgt uit:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nu kunt u uw omgevingsvariabele DOCKER_HOST als volgt instellen. U kunt uw Docker opdrachtregel-interface (CLI) gebruiken als normale blijven.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Een tunnel SSH maken in Windows

Er zijn meerdere opties voor het maken van SSH tunnels in Windows. In dit document wordt beschreven hoe stopverf gebruikt u dit wilt doen.

Stopverf downloaden naar uw Windows-bestandssysteem en voert u de toepassing.

Voer een hostnaam die uit de cluster beheerder-gebruikersnaam en de naam van het eerste model in het cluster bestaat. De **Hostnaam** ziet er als volgt: `adminuser@PublicDNS`. Voer 2200 voor de **poort**.

![Stopverf configuratie 1](media/putty1.png)

Selecteer **SSH** en **verificatie**. Uw bestand met persoonlijke sleutel voor verificatie toevoegen.

![Stopverf configuratie 2](media/putty2.png)

Selecteer **Tunnels** en configureren van de volgende poorten doorgestuurd:
- **Bronpoort:** Uw voorkeur--gebruikt 80 voor domeincontroller/OS of 2375 voor Swarm.
- **Bestemming:** Gebruik localhost:80 voor domeincontroller/OS of localhost:2375 voor Swarm.

Het volgende voorbeeld is geconfigureerd voor domeincontroller/OS, maar ziet er ongeveer uit voor Docker Swarm.

>[AZURE.NOTE] Poort 80 mag niet worden gebruikt wanneer u deze tunnel maakt.

![Stopverf configuratie 3](media/putty3.png)

Wanneer u klaar bent, sla de configuratie van de verbinding en de stopverf sessie verbinden. Wanneer u verbinding maakt, kunt u de poortconfiguratie van de in het gebeurtenissenlogboek van stopverf bekijken.

![Stopverf gebeurtenislogboek](media/putty4.png)

Wanneer u de tunnel voor domeincontroller/OS hebt geconfigureerd, kunt u het gerelateerde eindpunt bij openen:

- DOMEINCONTROLLER/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Wanneer u de tunnel hebt geconfigureerd voor het Docker Swarm, kunt u het cluster Swarm openen via de CLI Docker. U moet eerst een Windows-omgevingsvariabele configureren `DOCKER_HOST` met een waarde van ` :2375`.

## <a name="next-steps"></a>Volgende stappen

Implementeren en containers met domeincontroller/OS of Swarm beheren:

- [Werken met Azure Container Service en domeincontroller/OS](container-service-mesos-marathon-rest.md)
- [Werken met de Container Azure-Service en Docker Swarm](container-service-docker-swarm.md)
