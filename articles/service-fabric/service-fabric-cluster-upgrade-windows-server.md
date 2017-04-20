<properties
   pageTitle="Upgraden van een zelfstandige Service stof cluster op Windows Server | Microsoft Azure"
   description="Upgrade van de code-Service stof en/of de configuratie die wordt uitgevoerd als een zelfstandig product Service stof cluster, waaronder cluster updatemodus instellen"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Upgrade van uw cluster zelfstandige Service stof op Windows Server

> [AZURE.SELECTOR]
- [Azure Cluster](service-fabric-cluster-upgrade.md)
- [Zelfstandige Cluster](service-fabric-cluster-upgrade-windows-server.md)

Voor elk moderne systeem is ontwerpen voor kunnen sleutel bij het bereiken van langdurige succes van uw product. Een Service stof cluster is een resource dat u eigenaar bent. In dit artikel wordt beschreven hoe u kunt ervoor zorgen dat het cluster altijd wordt uitgevoerd ondersteunde versies van de service-stof code en configuraties.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De stof-versie die wordt uitgevoerd op uw Cluster bepalen

U kunt instellen dat uw cluster kunnen downloaden van de service-updates stof wanneer geeft een nieuwe versie van Microsoft of kiest u een ondersteunde stof-versie die u wilt dat uw cluster op te selecteren. 

U doen dit door in te stellen van de configuratie van de cluster "fabricClusterAutoupgradeEnabled" true of false.


>[AZURE.NOTE] Zorg ervoor dat uw cluster altijd met een ondersteunde Service stof versie behouden. Als en dat we de versie van een nieuwe versie van service stof aankondigen, betekent dit dat de vorige versie zich na ten minste 60 dagen vanaf die datum is gemarkeerd voor een einde van de ondersteuning. De nieuwe versies zijn aangekondigde [op het teamblog van service stof](https://blogs.msdn.microsoft.com/azureservicefabric/ ). De nieuwe versie is beschikbaar voor kies vervolgens. 


U kunt uw cluster upgraden naar de nieuwe versie alleen als u een stijl van een productie knooppuntconfiguratie, waarbij elk knooppunt Service stof op een aparte fysieke of virtuele machine is toegewezen. Als er een cluster ontwikkeling, wanneer er meer dan één service stof knooppunten op een enkele fysieke of virtuele machine zijn, moet u uw cluster verwijderen en opnieuw te maken met de nieuwe versie.


Er zijn twee afzonderlijke werkstromen voor het upgraden van uw cluster naar de meest recente of een ondersteunde service voor stof-versie. Presentatie voor clusters die verbinding hebt met het automatisch downloaden van de meest recente versie en de tweede presentatie voor clusters die helemaal geen verbinding met het downloaden van de meest recente versie van de Service stof zijn.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Upgrade van de clusters met de verbinding met het downloaden van de meest recente code en configuratie 

Gebruik van deze stappen voor het bijwerken van uw cluster naar een ondersteunde versie, als uw knooppunten internetverbinding met [http://download.microsoft.com hebt](http://download.microsoft.com) 

Voor kolomgroepen die verbinding met [http://download.microsoft.com hebt](http://download.microsoft.com)controleert we regelmatig op de beschikbaarheid van nieuwe service stof versies.


Wanneer een nieuwe versie van de service-stof beschikbaar is, is het pakket lokaal dan ook gedownload naar het cluster en deze is ingericht voor de upgrade. Ook als u wilt de klant informeren over deze nieuwe versie, aantal_tekens het systeem hebt opgegeven een expliciete cluster systeemstatus-waarschuwing ongeveer als volgt uit:

"De huidige cluster versie [versie #dezerij] ondersteuning loopt af [datum].", nadat het cluster actief is de nieuwste versie, de waarschuwing is verdwenen.


#### <a name="cluster-upgrade-workflow"></a>Cluster het upgraden van de werkstroom.
 
Als u de cluster systeemstatus waarschuwing ziet, moet u als volgt te werk:

1. Verbinding maken met het cluster vanaf een computer met beheerderstoegang tot alle computers die worden weergegeven als knooppunten in het cluster. De computer waarop dit script wordt uitgevoerd op heeft geen deel uitmaken van het cluster

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. De lijst met service stof-versies die u naar bijwerken kunt ophalen

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    u moet een uitvoer ongeveer als volgt ophalen:

    ![stof versies ophalen][getfabversions]

3. Een upgrade cluster naar een van de versies die beschikbaar zijn voor het gebruik van de [begin-ServiceFabricClusterUpgrade PowerShell cmd](https://msdn.microsoft.com/library/mt125872.aspx) starten

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
U kunt de voortgang van de upgrade voor Service stof explorer of door met de volgende power shell-opdracht controleren

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nadat u de problemen waardoor het terugdraaien hebt gecorrigeerd, moet u de upgrade opnieuw te starten door de stappen hierboven te volgen.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Upgrade van de clusters met <U>geen connectivity</u> downloaden van de meest recente code en configuratie

Gebruik deze stappen voor het bijwerken van uw cluster naar een ondersteunde versie, als uw cluster knooppunten **Ik heb geen** internetverbinding met [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Als u een cluster dat is niet verbonden met internet uitvoert, moet u controleren van de service stof teamblog naar een melding ontvangen van een nieuwe versie. Het systeem **niet** plaats cluster systeemstatus waarschuwing om u ervan te waarschuwen.  

1. Wijzig de configuratie van uw cluster de volgende eigenschap wilt instellen op onwaar.

        "fabricClusterAutoupgradeEnabled": false,

en een upgrade van de configuratie starten. Raadpleeg [Start-ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) voor details van gebruik. De cluster manifest versie is de versie die u in de clusterConfig.JSON hebt. Zorg ervoor dat deze voordat u verwijderen uit de upgrade configuratie.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Cluster het upgraden van de werkstroom.
 


1. Download de nieuwste versie van het pakket uit [maken service stof cluster voor windows server](service-fabric-cluster-creation-for-windows-server.md) -document 


1. Verbinding maken met het cluster vanaf een computer met beheerderstoegang tot alle computers die worden weergegeven als knooppunten in het cluster. De computer waarop dit script wordt uitgevoerd op heeft geen deel uitmaken van het cluster 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopieer het gedownloade pakket in het archief van de afbeelding cluster.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. De gekopieerde pakket registreren 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Een upgrade cluster naar een van de versies die beschikbaar is starten. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
U kunt de voortgang van de upgrade voor Service stof explorer of door met de volgende power shell-opdracht controleren

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nadat u de problemen waardoor het terugdraaien hebt gecorrigeerd, moet u de upgrade opnieuw te starten door de stappen hierboven te volgen.



## <a name="next-steps"></a>Volgende stappen
- Informatie over het aanpassen van enkele van de [service stof cluster stof-instellingen](service-fabric-cluster-fabric-settings.md)
- Leer hoe u [uw cluster in-en uitfaden schaal](service-fabric-cluster-scale-up-down.md)
- Meer informatie over de [toepassingsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
