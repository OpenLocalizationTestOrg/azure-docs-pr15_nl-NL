<properties
   pageTitle="Problemen met uw lokale Service stof cluster mailconfiguratie oplossen | Microsoft Azure"
   description="In dit artikel komen een set met suggesties voor probleemoplossing van uw cluster plaatselijke ontwikkeling"
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

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Problemen met de plaatselijke ontwikkeling cluster configuratie

Als u optreden problemen tijdens de interactie met uw lokale Azure-Service stof ontwikkeling cluster, raadpleegt u de volgende suggesties voor mogelijke oplossingen.

## <a name="cluster-setup-failures"></a>Cluster setup-fouten

### <a name="cannot-clean-up-service-fabric-logs"></a>Logboeken aan de Service stof niet kunt opschonen

#### <a name="problem"></a>Probleem

Tijdens het uitvoeren van het script DevClusterSetup, ziet u een foutbericht zoals dit:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Oplossing

Het huidige PowerShell-venster sluiten en open een nieuw venster PowerShell als beheerder. Nu moet u kunt het script uitvoeren.

## <a name="cluster-connection-failures"></a>Cluster verbindingsfouten

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service stof PowerShell-cmdlets worden niet herkend in Azure PowerShell

#### <a name="problem"></a>Probleem

Als u probeert uit te voeren een van de Service stof PowerShell-cmdlets, zoals `Connect-ServiceFabricCluster` in een venster Azure PowerShell, mislukt, zeggen de cmdlet wordt niet herkend. De reden hiervoor is dat Azure PowerShell gebruikt de 32-bits-versie van Windows PowerShell (zelfs op 64-bits besturingssysteemversies), terwijl de cmdlets Service stof alleen in 64-bits-omgevingen werken.

#### <a name="solution"></a>Oplossing

Altijd uitgevoerd Service stof cmdlets rechtstreeks vanuit Windows PowerShell.

>[AZURE.NOTE] De nieuwste versie van Azure PowerShell maakt geen een speciale snelkoppeling, zodat dit niet meer moet worden uitgevoerd.

### <a name="type-initialization-exception"></a>Type initialisatie uitzondering

#### <a name="problem"></a>Probleem

Wanneer u verbinding met het cluster in PowerShell maakt, ziet u de fout TypeInitializationException voor System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Oplossing

Uw padvariabele is niet correct ingesteld tijdens de installatie. Meld u af bij Windows en meld u weer aan. Hiermee worden uw pad volledig vernieuwd.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Cluster-verbinding mislukt met "Object is gesloten"

#### <a name="problem"></a>Probleem

Een oproep naar Connect-ServiceFabricCluster mislukt en wordt een foutbericht zoals dit:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Oplossing

Het huidige PowerShell-venster sluiten en open een nieuw venster PowerShell als beheerder. U moet nu mogelijk verbinding kunt maken.

### <a name="fabric-connection-denied-exception"></a>Stof verbinding geweigerd uitzondering

#### <a name="problem"></a>Probleem

Als foutopsporing in Visual Studio, krijgt u een fout FabricConnectionDeniedException.

#### <a name="solution"></a>Oplossing

Deze fout treedt meestal wanneer u uitproberen om te proberen te starten van een ServiceHost handmatig verwerken in plaats van de Service stof runtime om hem te starten voor u toestaan.

Zorg ervoor dat u geen serviceprojecten instellen als opstarten projecten in uw oplossing. Alleen Service stof toepassing projecten worden ingesteld als opstarten projecten.

>[AZURE.TIP] Als volgt instellen, uw lokale cluster begint zich gedraagt afgesloten, kunt u het opnieuw met behulp van de lokale cluster manager systeem systeemvak toepassing instellen. Hiermee wordt het bestaande cluster verwijderen en instellen van een nieuwe record. Houd er rekening mee dat alle toepassingen geïmplementeerd en bijbehorende gegevens worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

- [Begrijpen en problemen met uw cluster met statusrapporten systeem](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Uw cluster met Service stof Explorer visualiseren](service-fabric-visualizing-your-cluster.md)
