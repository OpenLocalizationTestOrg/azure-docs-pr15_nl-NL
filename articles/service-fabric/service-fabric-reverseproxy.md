<properties
   pageTitle="Service-stof Reverse-Proxy | Microsoft Azure"
   description="De Service stof reverse-proxy gebruiken voor communicatie met microservices van binnen en buiten het cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Service stof Reverse-Proxy

De Service stof Reverse-proxy is een reverse-proxy service ingebouwd in stof waarmee adressering van microservices in de service stof cluster, waarbij u kennismaakt HTTP-eindpunten.

## <a name="microservices-communication-model"></a>Microservices communicatiemodel

Microservices in service stof meestal worden uitgevoerd op een subset van de VM in het cluster en één VM kan overstappen naar een andere om verschillende redenen. Zodat de eindpunten voor microservices dynamisch wijzigen kunnen. Het normale patroon te communiceren met de microservice is hieronder missen oplossen

1. De locatie van de eerste instantie door de Naming Service oplossen.
2. Verbinding maken met de service.
3. De oorzaak van verbindingsfouten en opnieuw kunt oplossen door de servicelocatie indien nodig.

In het algemeen hierbij moet aan de clientzijde communicatie-bibliotheken in een lus opnieuw dat de service-resolutie en probeer het opnieuw beleid implementeert voor tekstterugloop.
Zie [communiceren met services](service-fabric-connect-and-communicate-with-services.md)voor meer informatie over dit onderwerp.

### <a name="communicating-via-sf-reverse-proxy"></a>Communiceren via EB reverse-proxy
Service stof reverse-proxy wordt uitgevoerd op alle knooppunten in het cluster. Dit kunt u het hele service resolutie proces uitvoeren namens een client en stuurt de aanvraag van de client. Dus kunt clients op het cluster alleen geen aan de clientzijde HTTP communicatie bibliotheken Neem contact op met de doelservice via de EB reverse-proxy lokaal op hetzelfde knooppunt worden uitgevoerd.

![Interne communicatie][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Kunnen bereiken Microservices van buiten het cluster
Het model van de externe communicatie standaard voor microservices is **aanmelden** waar elke service al dan niet standaard rechtstreeks vanuit externe klanten kan niet worden gebruikt. De [Taakverdeling Azure](../load-balancer/load-balancer-overview.md) is een netwerk begrenzing tussen microservices en externe-clients, NAT uitvoert en externe aanvragen doorgestuurd naar de interne **IP: poort** eindpunten. Om te kunnen maken van een microservice eindpunt rechtstreeks toegankelijk zijn voor externe klanten, moet de verdeling van de Azure belasting eerst worden geconfigureerd verkeer wilt doorsturen naar elke poort gebruikt door de service in het cluster. Bovendien de meeste microservices (esp. statuscontrole microservices) niet live op alle knooppunten van het cluster en ze kunnen verplaatsen tussen knooppunten op failover, zodat in dat geval de taakverdeling Azure effectief het doelknooppunt van de replica's kan niet bepalen bevinden zich doorsturen van het verkeer naar.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Microservices via de EB reverse-proxy van buiten het cluster kunnen bereiken

In plaats van afzonderlijke service poorten configureren in de azure taakverdeling, kan alleen de EB omkeren proxypoort worden geconfigureerd in de verdeling laden van de Azure. Hiermee kan clients buiten het cluster services binnen het cluster via de reverse-proxy zonder een extra configuraties hebt bereikt.

![Externe communicatie][0]

>[AZURE.WARNING] Configureren van de reverse-proxy poort op de taakverdeling, zorgt ervoor dat alle micro services in het cluster die een http-eindpunt, moeten addressible van buiten het cluster tonen.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>URI opmaak voor de adressering van services aan via de reverse-proxy

Een specifieke URI-notatie de Reverse-proxy gebruikt om aan te geven welke service partition de binnenkomende aanvraag moet worden doorgestuurd naar:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http (s):** De reverse-proxy kan worden geconfigureerd voor het accepteren van HTTP of HTTPS-verkeer is toegestaan. Voor het geval HTTPS-verkeer is toegestaan treedt SSL-beëindiging op de reverse-proxy. Aanvragen die worden doorgestuurd door de reverse-proxy naar services in het cluster zijn via http.
 - **Cluster FQDN | interne IP:** Voor externe klanten, kan de reverse-proxy worden geconfigureerd zodat deze bereikbaar via het clusterdomein (bijvoorbeeld mycluster.eastus.cloudapp.azure.com is). Al dan niet standaard die de reverse-proxy op elk knooppunt wordt uitgevoerd, dus voor interne verkeer deze kan worden bereikt op localhost of op een interne knooppunt IP-(bijvoorbeeld 10.0.0.1).
 - **Poort:** De poort die is opgegeven voor de reverse-proxy. Bijvoorbeeld: 19008.
 - **ServiceInstanceName:** Dit is de naam van de volledig gekwalificeerde geïmplementeerd service exemplaar van de service die u probeert te bereiken de "stof: / ' kleurenschema. Om bijvoorbeeld te laten bereiken service *stof: / Mijntoep/MijnService/*, gebruikt u *MijnToep/MijnService*.
 - **Achtervoegsel pad:** Dit is de werkelijke URL-pad voor de service die u wilt verbinden. Voorbeeld: *myapi/waarden/toevoegen/3*
 - **PartitionKey:** Voor een gepartitioneerde service is dit de partitiesleutel berekende van de partition die u wilt bereiken. Opmerking: dit is *niet* de partition-ID-GUID. Deze parameter is niet vereist voor mailservices via de singleton partition kleurenschema.
 - **PartitionKind:** De service partition kleurenschema. Dit is 'Int64Range' of 'Met de naam'. Deze parameter is niet vereist voor mailservices via de singleton partition kleurenschema.
 - **Time-out:**  Hiermee geeft u de time-out voor de HTTP-aanvraag die door de reverse-proxy bij de service namens de klant-aanvraag is gemaakt. De standaardinstelling voor deze is 60 seconden. Dit is een optionele parameter.

### <a name="example-usage"></a>Voorbeeld van syntaxis

Als u bijvoorbeeld eens service **stof: / Mijntoep/MyService** die wordt geopend een HTTP luisteraar ervan af op de volgende URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Met de volgende bronnen:

 - `/index.html`
 - `/api/users/<userId>`

Als het partitieschema singleton wordt gebruikt door de service, de *PartitionKey* en *PartitionKind* tekenreeks queryparameters zijn niet vereist en de service is bereikbaar via de gateway als:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Intern:`http://localhost:19008/MyApp/MyService`

Als het partitieschema Uniform Int64 wordt gebruikt door de service, moeten de *PartitionKey* en *PartitionKind* tekenreeks queryparameters worden gebruikt om een partition van de service hebt bereikt:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Als u wilt de resources die worden aangeboden door de service hebt bereikt, plaatst u het pad van de resource na de servicenaam in de URL:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

De gateway stuurt vervolgens deze aanvragen op van de service-URL:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Speciaal voor het delen van poort services

De Gateway-toepassing probeert opnieuw het adres van een service en probeer het verzoek wanneer een service kan niet worden bereikt. Dit is een van de belangrijkste voordelen van de gateway, zoals clientcode niet hoeft te implementeren zijn eigen resolutie service en lus oplossen.

In het algemeen wanneer een service kan niet worden bereikt betekent dit dat het exemplaar van de service of replica is verplaatst naar een ander knooppunt als onderdeel van de levenscyclus van de normale. Als dit gebeurt, wordt de gateway een netwerk verbinding foutbericht waarin wordt aangegeven dat een eindpunt is niet langer openen in het oorspronkelijk opgelost adres.

Echter replica's of service exemplaren een hostproces kunt delen en mogelijk ook delen van een poort wanneer die worden gehost door een HTTP-webserver, waaronder:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET-Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In dit geval is het mogelijk dat de webserver is beschikbaar in het hostproces en reageren op vergaderverzoeken, maar de opgelost exemplaar of een replica niet langer beschikbaar op de host is. De gateway ontvangt in dit geval een HTTP 404-antwoord van de webserver. Hierdoor heeft een HTTP 404 twee afzonderlijke betekenis:

 1. De service-mailadres juist is, maar de resource aangevraagd door de gebruiker niet bestaat.
 2. Het adres van de is onjuist en de resource aangevraagd door de gebruiker mogelijk daadwerkelijk bestaat op een ander knooppunt.

In het eerste geval is is dit een normale HTTP 404, wordt beschouwd als een gebruiker is opgetreden. In het tweede geval de gebruiker heeft een resource die bestaat aangevraagd, hoewel de gateway is niet vinden omdat de service zelf is verplaatst, zodat de gateway moet opnieuw kunt oplossen door het adres en probeer het opnieuw.

De gateway moet dus te onderscheiden tussen deze twee gevallen. Zodat u dit onderscheid, is een tip van de server vereist.

 - Standaard wordt met de Gateway-toepassing wordt ervan uitgegaan hoofdletters/kleine letters #2 en probeert te te zetten en opnieuw het verzoek verzenden.
 - Als u wilt aanduiden hoofdletters/kleine letters #1 tot de Gateway-toepassing, moet de volgende HTTP-tekenarcering worden geretourneerd door de service:

`X-ServiceFabric : ResourceNotFound`

Deze koptekst HTTP-antwoord geeft aan dat een normale HTTP 404 situatie waarin de gevraagde resource niet bestaat, en de gateway wordt niet opnieuw kunt oplossen door het adres van de.

## <a name="setup-and-configuration"></a>Installatie en configuratie
De service stof Reverse-proxy kan worden ingeschakeld voor het cluster via de [resourcemanager Azure-sjabloon](./service-fabric-cluster-creation-via-arm.md).

Nadat u de sjabloon voor het cluster die u wilt implementeren hebt (uit de voorbeeldsjablonen of via een aangepaste resource manager sjabloon) de Reverse-proxy in de sjabloon kan worden ingeschakeld door de volgende stappen uit.

1. Hiermee definieert u een poort voor de reverse-proxy in de [sectie Parameters](../resource-group-authoring-templates.md) van de sjabloon.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geef de poort voor elk van de objecten nodetype in de **Cluster** [de sectie type Resource](../resource-group-authoring-templates.md)

    Voor de apiVersion vóór ' 2016-09-01' de poort wordt aangegeven door de parameter ***httpApplicationGatewayEndpointPort*** een naam geven

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Voor apiVersion van op of na ' 2016-09-01' de poort wordt aangegeven door de parameter ***reverseProxyEndpointPort*** een naam geven

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Als u wilt het adres van de reverse-proxy van buiten het azure cluster, de **azure laden de verdeling van regels** voor de poort die is opgegeven in stap 1-instelling.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Als u wilt configureren SSL-certificaten op de poort voor de Reverse-proxy, kunt u het certificaat toevoegen aan de eigenschap httpApplicationGatewayCertificate de **Cluster** [de sectie type Resource](../resource-group-authoring-templates.md)

    Voor de apiVersion vóór ' 2016-09-01' het certificaat wordt aangegeven door de parameter ***httpApplicationGatewayCertificate*** een naam geven

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Voor apiVersion van op of na ' 2016-09-01' het certificaat wordt aangegeven door de parameter ***reverseProxyCertificate*** een naam geven
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Volgende stappen
 - Zie een voorbeeld van HTTP communicatie tussen services in een [voorbeeld van project op GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [RPC met betrouwbare Services externe toegang](service-fabric-reliable-services-communication-remoting.md)

 - [Web API die gebruikmaakt van OWIN in betrouwbare Services](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-communicatie met betrouwbare Services](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
