<properties
   pageTitle="Azure Container container servicebeheer via het web UI | Microsoft Azure"
   description="Containers dashboard implementeren naar een Azure-Service voor Container cluster-service met behulp van het web Marathon UI."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Containerbeheer van de via de gebruikersinterface van het web

Domeincontroller/OS biedt een omgeving voor de implementatie en schaalbaarheid van gegroepeerd werkbelastingen, terwijl de onderliggende hardware samenvatten. Boven aan de domeincontroller/OS, moet u er een kader die worden beheerd plannen en uitvoeren van berekeningscluster werkbelasting is.

Hoewel kaders beschikbaar voor veel populaire werkbelasting zijn, wordt dit document wordt beschreven hoe u kunt maken en de schaal van implementaties van de container met Marathon aanpassen. Voordat u deze voorbeelden moet u een domeincontroller/OS cluster die is geconfigureerd in Azure Container-Service. U moet ook externe connectiviteit met dit cluster zijn. Zie de volgende artikelen voor meer informatie over deze items:

- [Een cluster Azure Container-Service implementeren](container-service-deployment.md)
- [Verbinding maken met een cluster Azure Container-Service](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>De domeincontroller/OS UI verkennen

Met een Secure Shell (SSH) tunnel tot stand gebracht, blader naar http://localhost/. Hiermee wordt de domeincontroller/OS web UI geladen en informatie over het cluster, zoals gebruikte resources, actieve agenten en met services.

![GEBRUIKERSINTERFACE VAN DE DOMEINCONTROLLER/OS](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Kennismaken met de Marathon UI

Als u wilt zien van de gebruikersinterface Marathon, blader naar http://localhost/Marathon. In dit scherm kunt u een nieuwe container of een andere toepassing starten op het cluster Azure Container Service domeincontroller/OS. U ziet ook informatie over het uitvoeren van containers en toepassingen.  

![Marathon UI](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Een container Docker-indeling implementeren

Een nieuwe container met behulp van Marathon implementeren, klikt u op de knop **Toepassing maken** en voer de volgende gegevens in het formulier:

Veld           | Waarde
----------------|-----------
ID              | nginx
Afbeelding           | nginx
Netwerk         | Overbrugd
Host poort       | 80
Protocol        | TCP

![Nieuwe toepassing UI--algemeen](media/dcos/dcos4.png)

![Nieuwe toepassing UI--Docker Container](media/dcos/dcos5.png)

![Nieuwe toepassing UI--poorten en -Service Discovery](media/dcos/dcos6.png)

Als u de container-poort statisch toewijzen aan een poort op de agent wilt, moet u JSON-modus gebruiken. Hiervoor door de wizard nieuwe toepassing naar **JSON-modus** met behulp van de wisselknop te schakelen. Voer de volgende handelingen uit onder het `portMappings` gedeelte van de toepassingsdefinitie. In dit voorbeeld wordt poort 80 van de container aan poort 80 van de domeincontroller/OS-agent. Nadat u deze wijziging hebt aangebracht, kunt u deze wizard afmelden bij de JSON-modus schakelen.

```none
"hostPort": 80,
```

![Nieuwe toepassing UI--poort 80-voorbeeld](media/dcos/dcos13.png)

Het cluster domeincontroller/OS wordt geïmplementeerd met set persoonlijke en openbare agenten. Voor het cluster kunnen toepassingen toegang via Internet, moet u de toepassingen naar de agent van een openbare implementeren. Selecteer het tabblad **optioneel** van de wizard nieuwe toepassing hiertoe en voer **slave_public** voor de **Resource rollen geaccepteerd**.

![Nieuwe toepassing UI--openbare agent instellen](media/dcos/dcos14.png)

Klik op de hoofdpagina Marathon ziet u de implementatiestatus van de container.

![Marathon hoofdpagina UI--container Implementatiestatus](media/dcos/dcos7.png)

Wanneer u overschakelt naar de domeincontroller/OS web UI (http://localhost/), ziet u dat een taak (in dit geval een container Docker-indeling) wordt uitgevoerd op de domeincontroller/OS cluster.

![Domeincontroller/OS web UI--taak op de cluster](media/dcos/dcos8.png)

U kunt ook het knooppunt dat de taak wordt uitgevoerd op bekijken.

![Domeincontroller/OS web UI--taak clusterknooppunt](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>De schaal van uw containers aanpassen

U kunt de gebruikersinterface Marathon gebruiken om het aantal exemplaar van een container. Navigeer naar de pagina **Marathon** hiertoe, selecteert u de container die u wilt schalen en klik op de knop **schaal** . Klik in het dialoogvenster **Schaal toepassing** Voer het aantal exemplaren van container die u wilt en selecteer **Schaal doeltoepassing**.

![Marathon UI--dialoogvenster schaal-toepassing](media/dcos/dcos10.png)

Nadat u de schaal bewerking is voltooid, ziet u meerdere exemplaren van dezelfde taak verdeeld over domeincontroller/OS agenten.

![Domeincontroller/OS web UI dashboard--verspreiding over agenten van taak](media/dcos/dcos11.png)

![Domeincontroller/OS web UI--knooppunten](media/dcos/dcos12.png)

## <a name="next-steps"></a>Volgende stappen

- [Werken met domeincontroller/OS en de Marathon-API](container-service-mesos-marathon-rest.md)

Uitgebreide kennismaking op de Container Azure-Service met Mesos

> [AZURE. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos VIDEO]]
