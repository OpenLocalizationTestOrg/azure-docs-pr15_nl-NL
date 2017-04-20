<properties
   pageTitle="Upgrade van service stof App via PowerShell | Microsoft Azure"
   description="In dit artikel worden doorlopen de ervaring van een toepassing voor de Service stof implementeert, het wijzigen van de code en implementeren van een upgrade via PowerShell."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="service-fabric-application-upgrade-using-powershell"></a>Service stof toepassing upgrade via PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

De meestgebruikte en aanbevolen upgrade benadering is de gecontroleerde lopende upgrade.  Azure-Service stof Hiermee wordt de status van de toepassing wordt bijgewerkt op basis van een reeks beleidsregels van de servicestatus. Nadat een update domain (per) wordt bijgewerkt, Service stof evalueert de toepassingsstatus en gaat door naar de volgende update domain of de upgrade afhankelijk van het beleid systeemstatus mislukt.

Een upgrade gecontroleerde toepassing kan worden uitgevoerd met de beheerde systeemeigen API's, PowerShell of REST. Zie [een upgrade van uw gebruik van Visual Studio-toepassing](service-fabric-application-upgrade-tutorial.md)voor instructies over het uitvoeren van een upgrade gebruik van Visual Studio.

Met lopende upgrades Service stof gecontroleerd, kan de beheerder van de toepassing het beleid evaluatie die Service stof wordt gebruikt om te bepalen of de toepassing correct configureren. Bovendien kunt de beheerder de actie moet worden uitgevoerd wanneer de evaluatie van de servicestatus mislukt (bijvoorbeeld een automatisch herstel doen.) configureren In deze sectie worden doorlopen een gecontroleerde upgrade voor een van de SDK voorbeelden waarin PowerShell.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Stap 1: Bouwen en implementeren van de steekproef visuele objecten


Maken en publiceren van de toepassing door met de rechtermuisknop op de toepassingsproject **VisualObjectsApplication,** en de opdracht **publiceren** te selecteren.  Zie [Service stof toepassing zelfstudie upgraden](service-fabric-application-upgrade-tutorial.md)voor meer informatie.  U kunt ook PowerShell gebruiken om te implementeren van uw toepassing.

> [AZURE.NOTE] Voordat u een van de Service stof-opdrachten kunnen worden gebruikt in PowerShell, moet u eerst verbinding maken met het cluster via de `Connect-ServiceFabricCluster` cmdlet. Daarnaast wordt ervan uitgegaan dat het Cluster al is ingesteld op uw lokale computer. Zie het artikel over het [instellen van uw Service stof ontwikkelomgeving](service-fabric-get-started.md).

Na het samenstellen van het project in Visual Studio, kunt u de PowerShell-opdracht **Kopiëren-ServiceFabricApplicationPackage** het toepassingspakket kopiëren naar de ImageStore. De volgende stap is om de toepassing voor de Service stof runtime met de **Register-ServiceFabricApplicationPackage** -cmdlet te registreren. De laatste stap is een exemplaar van de toepassing te starten met behulp van de cmdlet **New-ServiceFabricApplication** .  Deze drie stappen zijn vergelijkbaar met het gebruik van de menuoptie **implementeren** in Visual Studio.

U kunt nu de [Service stof Verkenner om het cluster en de toepassing te geven](service-fabric-visualizing-your-cluster.md). De toepassing heeft een webservice die u kunt navigeren naar in Internet Explorer door te typen [http://localhost: 8081/visualobjects](http://localhost:8081/visualobjects) in de adresbalk.  Hier ziet u enkele zwevende visuele objecten navigeren in het scherm.  Bovendien kunt u **Get-ServiceFabricApplication** de toepassingsstatus van de controleren.

## <a name="step-2-update-the-visual-objects-sample"></a>Stap 2: De steekproef visuele objecten bijwerken

U ervaart mogelijk dat met de versie die is geïmplementeerd in stap 1, de visuele objecten niet draaien. Laten we upgrade van deze toepassing naar een waar de visuele objecten ook draaien.

Selecteer het project VisualObjects.ActorService binnen de oplossing VisualObjects en open het bestand StatefulVisualObjectActor.cs. Navigeren in dat bestand, met de methode `MoveObject`, opmerking af `this.State.Move()`, en verwijder de opmerkingen bij `this.State.Move(true)`. Deze wijziging Hiermee draait u de objecten nadat de service wordt bijgewerkt.

Ook moeten we de *ServiceManifest.xml* -bestand (onder PackageRoot) van het project **VisualObjects.ActorService**bijwerken. Werk de *CodePackage* en de serviceversie 2.0 en de bijbehorende regels in het bestand *ServiceManifest.xml* .
U kunt de optie Visual Studio *Bestandenlijst bestanden bewerken* nadat u met de rechtermuisknop op de oplossing manifest bestandswijzigingen aan te brengen.


Nadat de wijzigingen zijn aangebracht, het manifest ziet er als volgt te werk (wijzigingen gemarkeerde gedeelten weergeven):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Het *ApplicationManifest.xml* -bestand (gevonden onder het project **VisualObjects** onder de oplossing **VisualObjects** ) is nu bijgewerkt naar versie 2.0 van het project **VisualObjects.ActorService** . Versie van de toepassing wordt ook bijgewerkt naar 2.0.0.0 van 1.0.0.0. De *ApplicationManifest.xml* ziet er als het codefragment van de volgende:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Nu maken op het project door te selecteren alleen in het **ActorService** -project en klik vervolgens met de rechtermuisknop op en de optie **maken** in Visual Studio. Als u **Alles opnieuw opbouwen selecteert**, moet u de versie voor alle projecten, bijwerken, omdat de code zou is gewijzigd. Volgende, laten we de bijgewerkte toepassing inpakken met de rechtermuisknop op ***VisualObjectsApplication***, selecteert u het Menu Service stof en **pakket**te kiezen. Deze actie Hiermee maakt u een toepassingspakket dat kan worden geïmplementeerd.  Uw bijgewerkte toepassing kan worden geïmplementeerd.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Stap 3: Bepalen op servicestatus beleidsregels en parameters upgraden

Vertrouwd raken met de [toepassing upgraden parameters](service-fabric-application-upgrade-parameters.md) en het [upgradeproces](service-fabric-application-upgrade.md) om een goed inzicht in de verschillende parameters voor het bijwerken, time-outs en systeemstatus criterium toegepast. Voor deze procedure het criterium service systeemstatus evaluatie is ingesteld op de standaardwaarde (en aanbevolen) waarden, wat betekent dat alle services en exemplaren moeten zijn _in orde_ na de upgrade.  

Laten we vergroten echter de *HealthCheckStableDuration* tot 60 seconden (zodat de services zijn in orde voor ten minste 20 seconden voordat de upgrade naar de volgende update-domein verloopt).  Ook instellen laten we de *UpgradeDomainTimeout* moeten 1200 seconden en de *UpgradeTimeout* moeten 3000 seconden.

Ten slotte ook instellen de *UpgradeFailureAction* te draaien. Deze optie moet Service stof terug te draaien de toepassing naar de vorige versie als er problemen tijdens de upgrade wordt aangetroffen. Dus bij het starten van de upgrade (in stap 4), worden de volgende parameters opgegeven:

FailureAction = ongedaan maken

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Stap 4: Toepassing voor de upgrade voorbereiden

De toepassing is nu-en-klare en klaar voor de upgrade. Als u een PowerShell-venster als beheerder en type **Get-ServiceFabricApplication**opent, moet deze u laten weten dat dit toepassingstype 1.0.0.0 van **VisualObjects** die wordt geïmplementeerd is.  

Het toepassingspakket worden opgeslagen onder het volgende relatieve pad waar u de Service stof SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*gecomprimeerd. U moet een map 'Pakket' vinden in die map, waarin het toepassingspakket is opgeslagen. Controleer de tijdstempels om ervoor te zorgen dat dit de meest recente versie is (mogelijk moet u de paden correct ook wijzigen).

Nu we het pakket bijgewerkte toepassing kopiëren naar de Service stof ImageStore (waar de toepassingspakketten worden opgeslagen door de Service stof). De parameter *ApplicationPackagePathInImageStore* wordt geïnformeerd Service stof waar deze het toepassingspakket kunt vinden. We hebben de bijgewerkte toepassing plaatsen "VisualObjects\_V2 ' met de volgende opdracht uit (mogelijk moet paden opnieuw correct wijzigen).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

De volgende stap is het registreren van deze toepassing bij Service stof, die kan worden uitgevoerd met de volgende opdracht:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Als de voorgaande opdracht niet slaagt, is het waarschijnlijk dat u moet zijn opgebouwd alle services. Zoals vermeld in stap 2, is het wellicht de WebService nieuwste versie.

## <a name="step-5-start-the-application-upgrade"></a>Stap 5: De toepassing-upgrade starten

Nu kunnen we verder te doen de upgrade van de toepassing starten met behulp van de volgende opdracht uit:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


De naam van toepassing is hetzelfde als deze is beschreven in het bestand *ApplicationManifest.xml* . Deze naam van de service stof wordt gebruikt om aan te geven welke toepassing wordt ophalen bijgewerkt. Als u de time-outs te kort worden ingesteld, kunt u een foutbericht dat het probleem tegenkomen. Raadpleeg de sectie voor probleemoplossing of de time-outs vergroten.

Nu, als de upgrade opbrengst van toepassing, u kunt deze Service stof Verkenner te volgen of met behulp van de volgende PowerShell-opdracht: **Get-ServiceFabricApplicationUpgrade stof: / VisualObjects**.

In een paar minuten, dat u hebt ontvangen met behulp van de voorgaande PowerShell-opdracht, zou de status dat alle update domeinen zijn bijgewerkt (voltooid). En u ziet dat de visuele objecten in het browservenster hebt gestart draaien!

Een upgrade van versie 2 versie 3, of van versie 2 versie 1 als oefening, kunt u proberen. Overstappen van versie 2 naar versie 1, wordt dit ook beschouwd als een upgrade. Spelen met time-outs en systeemstatus beleid naar uzelf vertrouwd met hen maken. Wanneer u aan een Azure cluster implementeert, moeten de parameters correct zijn ingesteld. Het is handig voor het instellen van de time-outs records.


## <a name="next-steps"></a>Volgende stappen

[Een upgrade van uw toepassing gebruik van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u door middel van de upgrade van een toepassing gebruik van Visual Studio.

Bepalen hoe uw toepassing upgrades met behulp van [parameters upgraden](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibele maken door te leren hoe u [gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md)gebruiken.

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [Geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in de [toepassing-upgrades voor probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
