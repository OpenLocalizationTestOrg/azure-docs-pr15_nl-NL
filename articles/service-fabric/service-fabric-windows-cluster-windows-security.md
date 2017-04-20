<properties
   pageTitle="Een cluster met behulp van Windows-beveiliging Windows Secure | Microsoft Azure"
   description="Leer hoe u beveiliging naar knooppunten en client tot knooppunt configureren op een zelfstandige cluster met Windows met Windows-beveiliging."
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


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Secure een zelfstandige cluster op Windows met behulp van Windows-beveiliging

Om onbevoegde toegang tot een cluster Service stof voorkomen moet u deze, beveiligen met name wanneer er productie werkbelasting u erop. In dit artikel wordt beschreven hoe u naar knooppunten en client tot knooppunt beveiliging op basis van Windows-beveiliging in het bestand *ClusterConfig.JSON* en komt overeen met de stap van de beveiliging configureren in [een zelfstandige cluster maken waarop Windows](service-fabric-cluster-creation-for-windows-server.md). Zie voor meer informatie over het gebruik van Windows-beveiliging in Service stof, [Cluster beveiliging scenario's](service-fabric-cluster-security.md).

>[AZURE.NOTE]
U moet zorgvuldig uw selectie beveiliging voor knooppunt naar waardepapier, aangezien er geen upgrade cluster van één beveiliging keuze naar de andere is. De selectie beveiliging wordt gewijzigd, moet een volledig cluster opnieuw opbouwen.

## <a name="configure-windows-security"></a>Windows-beveiliging configureren
Het voorbeeldbestand voor de configuratie van *ClusterConfig.Windows.JSON* gedownload met de [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) zelfstandig cluster pakket bevat een sjabloon voor het configureren van Windows-beveiliging.  Windows-beveiliging is geconfigureerd in de sectie **Eigenschappen** :

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Configuratie-instelling**|**Beschrijving**|
|-----------------------|--------------------------|
|ClusterCredentialType|Windows-beveiliging is ingeschakeld door de parameter **ClusterCredentialType** bij *Windows*.|
|ServerCredentialType|Windows-beveiliging voor clients is ingeschakeld door de parameter **ServerCredentialType** bij *Windows*. Dit betekent dat de clients van het cluster en de cluster zelf, worden uitgevoerd binnen een Active Directory-domein.|
|WindowsIdentities|Bevat de identiteiten cluster en clientcomputers.|
|ClusterIdentity|Hiermee configureert u naar knooppunten beveiliging. Een lijst door komma's gescheiden van de groep beheerde serviceaccounts of namen van de computer.|
|ClientIdentities|Client-naar-knooppunt beveiliging configureert. Een matrix van gebruikersaccounts van de client.|
|Identiteit|De client-identiteit, een domeingebruiker.|
|IsAdmin|Waar geeft aan dat de domeingebruiker beheerderstoegang client ONWAAR voor clienttoegang van de gebruiker heeft.|

[Knooppunt naar knooppunt beveiliging](service-fabric-cluster-security.md#node-to-node-security) is geconfigureerd door in te stellen met **ClusterIdentity**. Om te kunnen samenstelt vertrouwensrelaties tussen knooppunten, moeten ze worden uitgevoerd op de hoogte van elkaar. Dit kan op twee manieren doen: de groep beheerde serviceaccount waarin alle knooppunten in het cluster opgeven of het domein knooppunt identiteiten van alle knooppunten in het cluster. Het is raadzaam met behulp van de [Groep beheerde serviceaccount (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) benadering, met name voor grotere clusters (meer dan 10 knooppunten) of voor clusters die waarschijnlijk groter of kleiner.
Deze methode kunt knooppunten moeten worden toegevoegd of verwijderd uit de gMSA, zonder wijzigingen in het manifest cluster. Deze methode is niet vereist voor het maken van een domeingroep waarvoor clusterbeheerders toegangsrechten toevoegen en verwijderen van leden hebt gekregen. Zie [Aan de slag met de groep beheerde Service Accounts](http://technet.microsoft.com/library/jj128431.aspx)voor meer informatie.

[Client naar knooppunt beveiliging](service-fabric-cluster-security.md#client-to-node-security) is geconfigureerd met **ClientIdentities**. Om vast te stellen vertrouwensrelatie tussen een client en het cluster, moet u het cluster als u wilt weten welke client identiteiten die kunnen worden vertrouwd configureren. Dit kunt doen in twee verschillende manieren: opgeven domein groepsgebruikers die kunnen verbinding maken, of de gebruikers van het domein-knooppunt dat verbinding kunnen maken. Service stof twee typen voor verschillende access-besturingselementen ondersteunt voor clients die met een cluster Service stof verbonden zijn: beheerder en gebruiker. Toegangsbeheer biedt de mogelijkheid om de cluster-beheerder om te beperken van toegang tot bepaalde soorten clusterbewerkingen voor verschillende groepen gebruikers, het cluster veiliger maken.  Beheerders hebben volledige toegang tot beheermogelijkheden (inclusief mogelijkheden voor alleen-lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen toepassingen en -services.

De volgende sectie voor de voorbeeld- **beveiliging** configureert Windows-beveiliging en geeft aan dat de machines in *ServiceFabric/clusterA.contoso.com* onderdeel van het cluster zijn dat *CONTOSO\usera* beheerder clienttoegang heeft:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Volgende stappen

Na het configureren van Windows-beveiliging in het bestand *ClusterConfig.JSON* , Ga verder met het maakproces cluster in [een zelfstandige cluster maken waarop Windows](service-fabric-cluster-creation-for-windows-server.md).

Zie [Cluster beveiliging-scenario's](service-fabric-cluster-security.md)voor meer informatie over het knooppunt naar beveiliging, client tot knooppunt beveiliging en Rolgebaseerd toegangsbeheer.

Zie [verbinding maken met een beveiligde cluster](service-fabric-connect-to-secure-cluster.md) voor voorbeelden over het verbinding maken met PowerShell of FabricClient.
