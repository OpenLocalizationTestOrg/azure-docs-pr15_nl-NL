<properties
   pageTitle="Project-maken Vervolgstappen service stof | Microsoft Azure"
   description="In dit artikel bevat koppelingen naar een reeks taken core Service stof"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Uw Service stof-toepassing en de volgende stappen
Uw Azure-Service stof-toepassing is gemaakt. In dit artikel worden de mate van uw project en mogelijke volgende stappen.

## <a name="your-application"></a>Uw toepassing
Elke nieuwe toepassing bevat een toepassingsproject. Er zijn mogelijk een of twee extra projecten, afhankelijk van het type service gekozen.

### <a name="the-application-project"></a>De toepassingsproject
De toepassingsproject bestaat uit:

- Een reeks verwijzingen naar de services waaruit de toepassing.

- Twee publiceren profielen (lokaal en Cloud) die u voor het behoud van voorkeuren kunt voor het werken met verschillende omgevingen--zoals voorkeuren die zijn gerelateerd aan een eindpunt cluster en of u wilt uitvoeren van de upgrade implementaties al dan niet standaard.

- Twee parameter toepassingsbestanden (lokaal en Cloud) die u kunt voor het behoud van toepassingsconfiguraties van de omgeving / regiospecifieke-, zoals het aantal partities die u wilt maken voor een service.

- Een implementatiescript waarbij u kunt uw toepassing vanaf de opdrachtregel of als onderdeel van een geautomatiseerde continue integratie en implementatie verkooppijplijn implementeren.

- Manifest van de toepassing, waarin de toepassing. U vindt het manifest onder de map ApplicationPackageRoot.

### <a name="stateless-service"></a>Stateless service
Wanneer u een nieuwe stateless service hebt toegevoegd, Visual Studio een service-project toegevoegd aan uw oplossing waarin een type afgeleid van `StatelessService`. De service wordt een lokale variabele in een item verhoogd.

### <a name="stateful-service"></a>Statuscontrole-service
Wanneer u een nieuwe statuscontrole service hebt toegevoegd, Visual Studio een service-project toegevoegd aan uw oplossing waarin een type afgeleid van `StatefulService`. De service wordt verhoogd een item in de `RunAsync` methode en slaat u het resultaat in een `ReliableDictionary`.

### <a name="actor-service"></a>Acteur-service
Wanneer u een nieuwe betrouwbare acteur hebt toegevoegd, Visual Studio twee projecten toegevoegd aan uw oplossing: een project acteur en een interface-project.

Het project acteur biedt methoden voor het instellen en aan de waarde van een item dat het betrouwbaar blijft behouden in van de acteur staat. Het project interface biedt een interface die andere services gebruiken kunnen om aan te roepen de acteur.

### <a name="stateless-web-api"></a>Stateless Web API
Het stateless Web API-project biedt een eenvoudige webservice die u gebruiken kunt om uw toepassing naar externe klanten te openen. Het project gestructureerd, Zie voor meer informatie over het [Service stof Web API-services met OWIN hostingprovider zelf](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.NET-core

De Service stof SDK bevat dezelfde reeks ASP.NET Core sjablonen die beschikbaar zijn voor de zelfstandige versie van ASP.NET Core projecten: leegmaken, [Web API][aspnet-webapi], en [Webtoepassing][aspnet-webapp].

## <a name="next-steps"></a>Volgende stappen
### <a name="create-an-azure-cluster"></a>Een Azure cluster maken
De Service stof SDK biedt een lokale cluster voor ontwikkelen en testen. Zie voor informatie over het maken van een cluster in Azure wordt aangegeven [bij het instellen van een Service stof cluster van de Azure portal][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Probeert u de implementatie in Azure gratis met partijen clusters

Als u implementeren en beheren van toepassingen in Azure proberen zonder bij het instellen van uw eigen clusters wilt, kunt u de gratis [partijen cluster-service](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>De toepassing Azure publiceren
U kunt uw toepassing rechtstreeks vanuit de Visual Studio aan een Azure cluster publiceren. Voor meer informatie hierover leest u [de toepassing Azure publiceren][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Service stof Explorer gebruiken om te visualiseren van uw cluster
Service stof Explorer biedt een eenvoudige manier om te visualiseren uw cluster, waaronder gebruikte toepassingen en fysieke lay-out. Meer informatie raadpleegt u [uw cluster met behulp van de Service stof Explorer Zomersportevenementen][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Versie en bijwerken van uw services
Service stof kunt onafhankelijke versiebeheer en het upgraden van onafhankelijke services in een toepassing. Meer informatie raadpleegt u [versiebeheer en een upgrade van uw services][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Continue integratie met Visual Studio Team Services configureren
Meer informatie over hoe kunt u een integratieproces continue instellen voor uw Service stof-toepassing, raadpleegt u [configureren continue integratie met Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
