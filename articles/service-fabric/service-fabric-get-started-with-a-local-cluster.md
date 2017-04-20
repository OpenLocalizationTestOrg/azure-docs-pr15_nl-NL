<properties
   pageTitle="Aan de slag met het implementeren van en upgraden apps op uw lokale cluster | Microsoft Azure"
   description="Een lokale Service stof cluster instelt, implementeren van een bestaande toepassing ernaar toevoegen en vervolgens een upgrade van die toepassing."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Aan de slag met distribueren en toepassingen op uw lokale cluster upgraden
De SDK van Azure-Service stof bevat een volledige lokale ontwikkelomgeving die u snel aan de slag kunt met toepassingen op een lokale cluster implementeren en beheren. In dit artikel u maakt een lokale cluster, implementeren van een bestaande toepassing toe, en vervolgens een upgrade uitvoeren naar een nieuwe versie van Windows PowerShell van die toepassing.

> [AZURE.NOTE] In dit artikel wordt ervan uitgegaan dat u al [uw ontwikkelomgeving instellen](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Een lokale cluster maken
Een Service stof cluster vertegenwoordigt een reeks hardwarebronnen die u toepassingen kunt implementeren. Meestal een cluster bestaat uit een willekeurige plaats vijf tot duizenden machines. De Service stof SDK bevat echter een clusterconfiguratie die kan worden uitgevoerd op één computer.

Het is belangrijk om te begrijpen dat het lokale cluster Service stof niet een emulator of simulator is. Dezelfde platform code die zich op meerdere machines clusters wordt uitgevoerd. De enige verschil is dat deze de platform-processen die gewend zijn verdeeld over vijf machines op één computer wordt uitgevoerd.

De SDK zijn er twee manieren voor het instellen van een lokale cluster: een Windows PowerShell-script en de lokale Cluster Manager systeem systeemvak-app. In deze zelfstudie gebruiken we de PowerShell-script.

> [AZURE.NOTE] Als u al een lokale cluster door het implementeren van een toepassing Visual Studio hebt gemaakt, kunt u dit gedeelte overslaan.


1. Start een nieuw PowerShell-venster als een beheerder.

2. De cluster setup-script uit de map SDK uitvoeren:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Cluster setup gaat even. Nadat de setup is voltooid, ziet u de uitvoer is vergelijkbaar met:

    ![Cluster setup uitvoer][cluster-setup-success]

    U bent nu klaar om te proberen uw cluster van een toepassing implementeren.

## <a name="deploy-an-application"></a>Een toepassing implementeren
De Service stof SDK bevat een uitgebreide set kaders en gereedschap voor het maken van toepassingen ontwikkelaars. Als u geïnteresseerd leren hoe bent u toepassingen maken in Visual Studio, raadpleegt u [uw eerste stof Service-toepassing in Visual Studio maken](service-fabric-create-your-first-application-in-visual-studio.md).

In deze zelfstudie we een bestaande steekproef-toepassing (WordCount genoemd) gebruiken zodat we op de management aspecten van het platform richten kunt--inclusief implementatie, bewaken en bijwerken.


1. Start een nieuw PowerShell-venster als een beheerder.

2. Importeer de Service stof SDK PowerShell-module.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Maak een map voor het opslaan van de toepassing die u kunt downloaden en implementeren, zoals C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [De toepassing WordCount downloaden](http://aka.ms/servicefabric-wordcountapp) naar de locatie u hebt gemaakt.  Opmerking: de browser Microsoft Edge slaat het bestand met *de extensie* .  Wijzig de bestandsextensie in *.sfpkg*.

5. Verbinding maken met de lokale cluster:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Een nieuwe toepassing maken met de opdracht implementatie van de SDK met een naam en een pad naar het toepassingspakket.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Als alles goed gaat, ziet u de volgende uitvoer:

    ![Een toepassing naar de lokale cluster implementeren][deploy-app-to-local-cluster]

7. Als u wilt de toepassing in actie zien, Open de browser en Ga naar [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). U ziet het volgende:

    ![Gedistribueerde toepassing UI][deployed-app-ui]

    De toepassing WordCount is heel eenvoudig. Het bevat JavaScript-code aan de clientzijde om te genereren willekeurig vijf tekens 'woorden', die vervolgens aan de toepassing via ASP.NET Web API doorgegeven. Een statuscontrole service wordt bijgehouden met het aantal woorden in de berekening opgenomen. Deze worden ondergebracht op basis van het eerste teken van het woord. U vindt de broncode voor de app WordCount in de [voorbeelden aan de slag](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    De toepassing die wordt geïmplementeerd bevat vier partities. Zodat woorden die beginnen met A tot en met G zijn opgeslagen in de eerste partition, worden woorden die beginnen met H tot en met N zijn opgeslagen in de tweede partition, enzovoort.

## <a name="view-application-details-and-status"></a>Details van de toepassing weergeven en de status
Nu dat we de toepassing hebt geïmplementeerd, eens enkele van de appdetails van de in PowerShell.

1. Alle gebruikte toepassingen op het cluster query:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Ervan uitgaande dat u hebt alleen de app WordCount geïmplementeerd, ziet u een vergelijkbare naar:

    ![Alle gebruikte toepassingen in PowerShell query][ps-getsfapp]

2. Ga naar een hoger niveau via query's in de reeks services die van de toepassing WordCount uitmaken deel.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Lijst met services voor de toepassing in PowerShell][ps-getsfsvc]

    De toepassing bestaat uit twee services--de front-end van de web en de statuscontrole-service die worden beheerd door de woorden.

3. Ten slotte gaat u naar de lijst met partities voor WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Bekijk de service-partities in PowerShell][ps-getsfpartitions]

    De reeks opdrachten die u hebt gebruikt, zoals alle Service stof PowerShell-opdrachten zijn beschikbaar voor een cluster die u kunt verbinding met lokale of externe.

    Voor een meer visuele manier om te communiceren met het cluster, kunt u het hulpmiddel Service stof Explorer op het web door te schuiven naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer) in de browser.

    ![Details van de toepassing weergeven in Service stof Explorer][sfx-service-overview]

    > [AZURE.NOTE] Meer informatie over de Service stof Explorer, raadpleegt u [uw cluster met Service stof Explorer visualiseren](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Upgraden van een toepassing
Service stof biedt geen downtime upgrades door te controleren van de status van de toepassing, zoals deze af in het cluster uitgevouwen. Laten we een eenvoudige upgrade van de toepassing WordCount uitvoert.

De nieuwe versie van de toepassing nu telt alleen woorden die met een klinkers beginnen. Tijdens de upgrade uitgevouwen af, zien we twee wijzigingen in gedrag van de toepassing. Eerst moet de snelheid waarmee het aantal groeit vertraagd raken, aangezien minder woorden worden worden geteld. Tweede, aangezien het eerste partition twee klinkers (A en E heeft) en alle andere partities slechts één elke bevatten, wordt het aantal moet uiteindelijk was de anderen beginnen.

1. [Het pakket WordCount v2 downloaden](http://aka.ms/servicefabric-wordcountappv2) op dezelfde locatie waar u het pakket v1 hebt gedownload.

2. Ga terug naar uw PowerShell-venster en de upgrade van de SDK-opdracht gebruiken om u te registreren van de nieuwe versie in het cluster. Breng de gewenste wijzigingen bijwerken van de stof: / WordCount-toepassing.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Hier ziet u uitvoer in PowerShell die er ongeveer als volgt uit als de upgrade begint.

    ![De upgrade voortgang in PowerShell][ps-appupgradeprogress]

3. Tijdens de upgrade verder gaat is, is het wellicht handiger om de status van Service stof Explorer de te houden. Een browservenster starten en Ga naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer). **Toepassingen** in de structuur aan de linkerkant uitbreiden en kies vervolgens **WordCount**, en ten slotte **stof: / WordCount**. In het tabblad essentials ziet u de status van de upgrade terwijl deze van het cluster upgrade domeinen doorlopen.

    ![Voortgang in Service stof Explorer bijwerken][sfx-upgradeprogress]

    Tijdens de upgrade is doorlopen van elk domein, worden uitgevoerd om ervoor te zorgen dat de toepassing correct gedrag.

4. Als u de eerdere query voor het instellen van services in de stof opnieuw uitvoeren: / WordCount-toepassing, u ziet dat de versie van WordCountService gewijzigd, maar de versie van WordCountWebService niet:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Query toepassingsservices na de upgrade][ps-getsfsvc-postupgrade]

    Dit wordt gemarkeerd hoe de toepassingsupgrades voor het beheer van Service stof. Deze raakt alleen de set met services (of code/configuratie-pakketten binnen deze services) die zijn gewijzigd, waardoor het verwerken van de upgrade sneller en meer betrouwbare.

5. Ten slotte, Ga terug naar de browser om het gedrag van de nieuwe toepassingsversie toekijken. Zoals verwacht, het aantal langzamer verloopt, en wordt het eerste partition met iets meer van de volume.

    ![De nieuwe versie van de toepassing in de browser bekijken][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Opschonen

Voordat u in het kort, is het belangrijk te weten dat het lokale cluster reële. Toepassingen blijven op de achtergrond uitgevoerd totdat u deze verwijderen.  Afhankelijk van de aard van uw apps duurt een actieve app maximaal aanzienlijk resources op uw computer. Hebt u verschillende opties voor het beheren van toepassingen en het cluster:

1. Als u wilt een afzonderlijke-toepassing en alle bijbehorende gegevens verwijderen, voert u het volgende:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Of de toepassing verwijderen uit het menu **Acties** van Service stof Explorer of het contextmenu in de lijstweergave toepassing van het linkerdeelvenster.

    ![Verwijderen van een toepassing is Service stof Explorer][sfe-delete-application]

2. De toepassing uit het cluster verwijdert, kunt u de registratie van versie 1.0.0 en 2.0.0 van het type van de toepassing WordCount ongedaan maken. Verwijderen Hiermee verwijdert u de toepassingspakketten, met inbegrip van de code en configuratie van van het cluster afbeelding store.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Of, in Service stof Explorer kiezen **Inrichting verwijderen** voor de toepassing.

3. Wilt afsluiten het cluster maar de gegevens van toepassingen en -sporen, klikt u op de **Lokale Cluster stoppen** in het systeem systeemvak-app.

4. Als u wilt het cluster geheel verwijderen, klikt u op de **Lokale Cluster verwijderen** in het systeem systeemvak-app. Deze optie resulteert in een andere traag implementatie de volgende keer dat u op F5 drukt in Visual Studio. Verwijder het lokale cluster alleen als u niet wilt gebruiken voor het enige tijd of als u nodig hebt om resources vrij te maken.

## <a name="1-node-and-5-node-cluster-mode"></a>1 knooppunt en 5 knooppunt clustermodus

Tijdens het werken met de lokale cluster ontwikkelen van toepassingen, u vaak vinden zelf doen snelle herhalingen van programmacode schrijven, foutopsporing en wijzigen van code, foutopsporing enzovoort. Dit proces optimaliseren, zodat het lokale cluster kan worden uitgevoerd in twee modi: 1 knooppunt of 5 knooppunt. Beide modi cluster hebben hun voordelen.
5 knooppunt clustermodus kunt u werken met een reële cluster. U kunt testen failover-scenario's, werken met meer exemplaren en kopieën van uw services.
1 knooppunt clustermodus is geoptimaliseerd voor snelle distributie en registratie van services, kunt u snel kunt valideren code met de Service stof runtime doen.

Zowel 1 knooppunt cluster modus en 5 knooppunt clustermodus is niet een emulator of simulator. Dezelfde platform code die zich op meerdere machines clusters wordt uitgevoerd.

> [AZURE.NOTE] Deze functie is beschikbaar in SDK versie 5,2 en hoger.

Als u wilt wijzigen in de clustermodus een 1 cluster knooppunten, gebruikt u de Service stof lokaal Cluster Manager of PowerShell de volgende manier:

1. Start een nieuw PowerShell-venster als een beheerder.

2. De cluster setup-script uit de map SDK uitvoeren:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Cluster setup gaat even. Nadat de setup is voltooid, ziet u de uitvoer is vergelijkbaar met:
    
    ![Cluster setup uitvoer][cluster-setup-success-1-node]

Als u de Service stof lokaal Cluster Manager gebruikt:

![Schakeloptie clustermodus][switch-cluster-mode]

> [AZURE.WARNING] Bij het wijzigen van clustermodus, het huidige cluster wordt verwijderd uit uw systeem en een nieuw cluster wordt gemaakt. De gegevens die u moet hebt opgeslagen in het cluster, worden verwijderd wanneer u clustermodus wijzigen.

## <a name="next-steps"></a>Volgende stappen
- Nu dat u hebt geïmplementeerd en sommige vooraf gedefinieerde toepassingen bijgewerkt, kunt u [proberen samenstellen van uw eigen in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
- Alle acties uitgevoerd op het lokale cluster in dit artikel kunnen worden uitgevoerd op een [Azure cluster](service-fabric-cluster-creation-via-portal.md) ook.
- De upgrade wordt uitgevoerd in dit artikel is eenvoudige. Zie de [documentatie upgraden](service-fabric-application-upgrade.md) voor meer informatie over de kracht en flexibiliteit van Service stof upgrades.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
