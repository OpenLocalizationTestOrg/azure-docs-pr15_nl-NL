<properties
   pageTitle="De levenscyclus van de toepassing in Service stof | Microsoft Azure"
   description="Worden ontwikkelen, implementeren, testen, een upgrade uitvoert, onderhouden en stof Service-toepassingen verwijderen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>De levenscyclus van de service stof-toepassing
Als u met andere platforms, een toepassing op Azure-Service stof meestal doorloopt de volgende stappen: ontwerp, ontwikkeling, testen, implementatie, een upgrade uitvoert, onderhoud en verwijderen. Service stof biedt eersteklas ondersteuning voor de levenscyclus van de volledige toepassing van cloud-toepassingen, van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud eventuele buiten bedrijf stellen. Het service-model kunt verschillende verschillende rollen onafhankelijk deelnemen aan de levenscyclus van de toepassing. Dit artikel bevat een overzicht van de API's en hoe ze worden gebruikt door de verschillende rollen overal in de fasen van de levenscyclus van de toepassing Service stof.

## <a name="service-model-roles"></a>Rollen van service-model
De service model rollen zijn:

- **Service ontwikkelaars**: ontwikkelt modulaire en algemene services die kunnen worden opnieuw gebruikt en gebruikt in meerdere toepassingen van de hetzelfde type of verschillende typen. Een wachtrijservice kan bijvoorbeeld worden gebruikt voor het maken van een toepassing voor kaartverkoop (helpdesk) of een e-commerce-toepassing (winkelwagen).

- **De ontwikkelaar van toepassing**: toepassingen resulteert in een verzameling services om te voldoen aan bepaalde over de vereisten of scenario's integreren. Bijvoorbeeld een e-commerce-website kan integreren met "JSON Stateless Front-End Service", "Aanbieding Stateful Service" en "Wachtrij Stateful Service" maken van een auctioning oplossing.

- **Beheerder van de toepassing**: beslissingen op de configuratie van de toepassing (de parameters van de sjabloon configuratie invullen), implementatie (toewijzing naar beschikbare resources) en kwaliteit van de service. Een beheerder van de toepassing wordt bijvoorbeeld de landinstelling taal (Engels voor de Verenigde Staten) of Japans voor Japan, bijvoorbeeld van de toepassing bepaald. Een andere geïmplementeerd toepassing kan verschillende instellingen hebben.

- **Operator**: toepassingen op basis van de configuratie van de toepassing en vereisten die zijn opgegeven door de beheerder van de toepassing implementeren. Bijvoorbeeld een operator bepalingen en implementeren van de toepassing en zorgt ervoor dat deze wordt uitgevoerd in Azure wordt aangegeven. Operatoren informatie over gezondheid en prestaties van toepassingen en bijhouden van de fysieke infrastructuur naar wens.


## <a name="develop"></a>Ontwikkelen
1. Een *service ontwikkelaars* ontwikkelt verschillende soorten mailservices via het programmeren model [Betrouwbare betrokkenen](service-fabric-reliable-actors-introduction.md) of [Betrouwbare Services](service-fabric-reliable-services-introduction.md) .
2. Een *service ontwikkelaars* beschrijft declaratieve de servicetypen ontwikkeld in een manifest servicebestand bestaande uit een of meer code, configuratie en gegevens-pakketten.
3. Een *toepassing ontwikkelaars* genereert vervolgens een toepassing via verschillende typen.
4. Het toepassingstype in het manifest van een toepassing een *toepassing ontwikkelaars* declaratieve worden door te verwijzen naar de service-manifesten van het onderdeel services correct overschrijven en parameters voorzien van verschillende instellingen voor de configuratie en implementatie van het onderdeel services.

Zie [aan de slag met betrouwbare betrokkenen](service-fabric-reliable-actors-get-started.md) en [aan de slag met betrouwbare Services](service-fabric-reliable-services-quick-start.md) voor voorbeelden.

## <a name="deploy"></a>Implementeren
1. Een *beheerder van de toepassing* aangepast, zodat deze het toepassingstype naar een specifieke toepassing om te worden geïmplementeerd op een Service stof cluster door het opgeven van de gewenste parameters van het element **ApplicationType** in manifest van de toepassing.

2. Een *operator* uploadt de toepassingspakket naar de store van de afbeelding cluster met behulp van de [methode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) of de [ **Kopie-ServiceFabricApplicationPackage** -cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Het toepassingspakket bevat manifest van de toepassing en de verzameling servicepakketten. Service stof implementeert toepassingen uit de toepassingspakket die zijn opgeslagen in de afbeelding opslaan, die een Azure blob store of de service-Service stof kan zijn.

3. De *operator* bepalingen vervolgens het toepassingstype in het doelcluster uit het geüploade toepassing-pakket met de [methode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), de [ **Register-ServiceFabricApplicationType** cmdlet](https://msdn.microsoft.com/library/azure/mt125958.aspx)of voor de [REST van de **voorziening een toepassing** bewerking](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Nadat de toepassing is geïnstalleerd, wordt een *operator* de toepassing gestart met parameters die door de *beheerder van de toepassing* met de [methode **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), de [cmdlet **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)of voor de [REST van de **Toepassing maken** bewerking](https://msdn.microsoft.com/library/azure/dn707676.aspx).

5. Nadat de toepassing is geïmplementeerd, wordt de [methode **CreateServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), de [cmdlet **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)of de [bewerking van de REST van de **Service maken** ](https://msdn.microsoft.com/library/azure/dn707657.aspx) van een *operator* gebruikt om u te maken van nieuwe exemplaren van de service voor de toepassing op basis van beschikbare servicetypen.

6. De toepassing wordt nu uitgevoerd in het cluster Service stof.

Zie [Deploy een toepassing](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="test"></a>Test
1. Na het implementeren naar het plaatselijke ontwikkeling cluster of een test cluster, wordt in een *service ontwikkelaars* het ingebouwde failover Testscenario uitgevoerd met behulp van de klassen [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) en [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) of de [cmdlet **Roep-ServiceFabricFailoverTestScenario** ](https://msdn.microsoft.com/library/azure/mt644783.aspx). Het failover-Testscenario doorloopt een opgegeven service belangrijke overgangen en failovers om ervoor te zorgen dat dit nog steeds beschikbaar en werkt is.

2. De *ontwikkelaar van* de ingebouwde chaos Testscenario met behulp van de klassen [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) en [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) of de [cmdlet **Roep ServiceFabricChaosTestScenario** ](https://msdn.microsoft.com/library/azure/mt644774.aspx)vervolgens worden uitgevoerd. Het chaos Testscenario induceert willekeurig meerdere knooppunt, code-pakket en replica fouten in het cluster.

3. De *service ontwikkelaars* [tests service-naar-service communicatie](service-fabric-testability-scenarios-service-communication.md) door authoring test-scenario's die primaire replica rond het cluster verplaatsen.

Zie [Inleiding tot de foutenstructuuranalyse analyse-Service](service-fabric-testability-overview.md) voor meer informatie.

## <a name="upgrade"></a>Upgrade
1. Een *service ontwikkelaars* werkt het onderdeel services van de geactiveerde toepassing en/of oplossingen bugs bij te werken en biedt een nieuwe versie van de servicemanifest.

2. Een *toepassing ontwikkelaars* wordt genegeerd en de configuratie en implementatie-instellingen van de consistente services parameterizes en vindt u een nieuwe versie van manifest van de toepassing. De ontwikkelaar van toepassingen vervolgens de nieuwe versies van de service-manifesten implementeren in de toepassing en vindt u een nieuwe versie van het toepassingstype in een bijgewerkte toepassing-pakket.

3. Een *beheerder van de toepassing* voor het implementeren van de nieuwe versie van het toepassingstype in de doeltoepassing instellen door de gewenste parameters bij te werken.

5. Het pakket bijgewerkte toepassing uploadt een *operator* naar de cluster afbeelding store met de [methode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) of de [ **Kopie-ServiceFabricApplicationPackage** cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Het toepassingspakket bevat manifest van de toepassing en de verzameling servicepakketten.

6. Een *operator* bepalingen de nieuwe versie van de toepassing in het doelcluster met behulp van de [methode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), de [ **Register-ServiceFabricApplicationType** cmdlet](https://msdn.microsoft.com/library/azure/mt125958.aspx)of de [REST van de **voorziening een toepassing** bewerking](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. Een *operator* upgrades de doeltoepassing naar de nieuwe versie met de [methode **UpgradeApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), de [ **Begin-ServiceFabricApplicationUpgrade** cmdlet](https://msdn.microsoft.com/library/azure/mt125975.aspx)of voor de [ **Upgrade van een toepassing** REST bewerking](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. Een *operator* controleert de voortgang van een upgrade met de [methode **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), de [cmdlet **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)of voor de [REST van de **Voortgang van toepassing upgraden krijgen** bewerking](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Indien nodig, wordt de *operator* wijzigt en de parameters van de huidige toepassing upgrade met de [methode **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), de [ **Update-ServiceFabricApplicationUpgrade** cmdlet](https://msdn.microsoft.com/library/azure/mt126030.aspx)of voor de [REST **Update toepassing upgraden** bewerking](https://msdn.microsoft.com/library/azure/mt628489.aspx)opnieuw te toegepast.

10. Klik zo nodig, wordt de *operator* ongedaan gemaakt de huidige toepassing upgrade met de [methode **RollbackApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), de [ **Begin-ServiceFabricApplicationRollback** cmdlet](https://msdn.microsoft.com/library/azure/mt125833.aspx)of voor de [REST **Terugdraaien toepassing upgraden** bewerking](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Service stof upgrades de doeltoepassing instellen in het cluster uitgevoerd zonder de beschikbaarheid van een van de onderdeel services kwijt te raken.

Zie de [toepassing upgraden zelfstudie](service-fabric-application-upgrade-tutorial.md) voor voorbeelden.

## <a name="maintain"></a>Onderhouden
1. Voor het besturingssysteem en patches, Service stof interfaces met de Azure infrastructuur te garanderen beschikbaarheid van alle toepassingen uitgevoerd in het cluster.

2. Voor upgrades en het platform Service stof patches upgrades Service stof zelf zonder beschikbaarheid van een van de toepassingen die wordt uitgevoerd op het cluster kwijt te raken.

3. Een *beheerder van de toepassing* goedkeurt het toevoegen of verwijderen van knooppunten van een cluster na het analyseren van gegevens over het gebruik historische capaciteit en verwachte toekomstige vraag.

4. Een *operator* wordt toegevoegd en wordt verwijderd van knooppunten die zijn opgegeven door de *beheerder van de toepassing*.

5. Wanneer nieuwe knooppunten worden toegevoegd aan of bestaande knooppunten worden verwijderd uit het cluster, Service stof automatisch laden-saldi de actieve toepassingen op alle knooppunten in het cluster optimale prestaties.

## <a name="remove"></a>Verwijderen
1. Een *operator* kunt u een specifiek exemplaar van een actieve service in het cluster verwijderen zonder de hele toepassing met de [methode **DeleteServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), de [cmdlet **Verwijderen-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)of voor de [REST van de **Service verwijderen** betrekking heeft](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. Een *operator* kunt ook verwijderen instantie van een toepassing en alle bijbehorende services met de [methode **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), de [cmdlet **Verwijderen-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125914.aspx)of voor de [REST van de **Toepassing verwijderen** betrekking heeft](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Zodra de toepassingen en services zijn gestopt, kan de *operator* het toepassingstype met de [methode **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), de [ **Registratie-ServiceFabricApplicationType** cmdlet](https://msdn.microsoft.com/library/azure/mt125885.aspx)of voor de [REST van de **inrichting verwijderen van een toepassing** bewerking](https://msdn.microsoft.com/library/azure/dn707671.aspx)inrichting. Hierbij het toepassingstype, wordt het toepassingspakket niet verwijderd uit de ImageStore. Het toepassingspakket moet u handmatig verwijderen.

4. Het toepassingspakket verwijdert een *operator* uit de ImageStore met de [methode **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) of de [cmdlet **Verwijderen-ServiceFabricApplicationPackage** ](https://msdn.microsoft.com/library/azure/mt163532.aspx).

Zie [Deploy een toepassing](service-fabric-deploy-remove-applications.md) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het ontwikkelen van, testen en stof Service-toepassingen en services beheren:

- [Betrouwbare betrokkenen](service-fabric-reliable-actors-introduction.md)
- [Betrouwbare Services](service-fabric-reliable-services-introduction.md)
- [Een toepassing implementeren](service-fabric-deploy-remove-applications.md)
- [Upgrade van toepassing](service-fabric-application-upgrade.md)
- [Testbaarheid overzicht](service-fabric-testability-overview.md)
- [Voorbeeld van de levenscyclus van de REST-toepassing](service-fabric-rest-based-application-lifecycle-sample.md)
