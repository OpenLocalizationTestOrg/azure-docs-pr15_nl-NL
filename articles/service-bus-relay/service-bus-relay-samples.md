<properties 
    pageTitle="Service Bus relay voorbeelden overzicht | Microsoft Azure"
    description="Categoriseert en Service Bus relay voorbeelden met koppelingen naar elk beschreven."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Voorbeelden van Service Bus relay

De Service Bus relay voorbeelden wordt getoond belangrijke functies in [Service Bus relay](https://azure.microsoft.com/services/service-bus/). In dit artikel wordt gecategoriseerd en wordt beschreven in de voorbeelden beschikbaar, met koppelingen naar elk.

>[AZURE.NOTE] Voorbeelden van de service Bus worden niet geïnstalleerd met de SDK. U kunt de voorbeelden downloaden van de [pagina met Azure SDK voorbeelden](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Daarnaast kunnen er is een bijgewerkte reeks Service Bus relay voorbeelden [hier](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (schrijven van dit, ze zijn niet in dit artikel beschreven).  

Raadpleeg de [Service Bus messaging voorbeelden](../service-bus-messaging/service-bus-samples.md)voor SMS-berichten voorbeelden.

## <a name="service-bus-relay"></a>Service Bus relay

In de volgende voorbeelden illustreren hoe u toepassingen die gebruikmaken van de Service Bus relayservice schrijft.

Houd er rekening mee dat in de voorbeelden relay een verbindingsreeks voor toegang tot de naamruimte van uw Service Bus vereist.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Een verbindingsreeks ophalen voor Azure Service Bus

1. Meld u aan bij de [portal van Azure](http://portal.azure.com).

1. Klik in de linkerkolom op **Service-Bus**.

1. Klik op de naam van de naamruimte in de lijst.

3. Klik in het blad naamruimte op **clienttoegangsbeleid gedeeld**.

4. Klik in het blad **clienttoegangsbeleid gedeeld** op **RootManageSharedAccessKey**.

6. De verbindingsreeks naar het Klembord kopiëren.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Een verbindingsreeks ophalen voor Service Bus voor Windows Server

1. De volgende PowerShell-cmdlet uitvoeren:

    ```
    get-sbClientConfiguration
    ```

2. Plak de verbindingsreeks in het bestand App.config voor de steekproef.

## <a name="service-bus-relay"></a>Service Bus relay

Voorbeelden die de Service Bus relay illustreren.

### <a name="getting-started"></a>Aan de slag

|De naam van de steekproef|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Doorgegeven Messaging: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Laat zien hoe een Service Bus-client en service uitvoeren op Azure. In dit voorbeeld configureert Service Bus via programmacode. Alleen-omgeving en beveiliging gegevens worden opgeslagen in de configuratiebestanden.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS verificatie: Gedeeld geheim](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Laat zien hoe een uitgever naam en de uitgever geheim gebruiken om te verifiëren met Service Bus.|1,8|Microsoft Azure-Service Bus|
|[SMS-berichten verificatie doorgegeven: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Laat zien hoe om weer te geven van een HTTP-service waarvoor geen client gebruikersverificatie vereist.|1,8|Microsoft Azure-Service Bus|
|[SMS-berichten bindingen doorgegeven: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Laat zien hoe de binding **WebHttpRelayBinding** gebruiken om terug te keren binaire gegevens via het Web programming model.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: NetTcp doorgegeven](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Laat zien hoe de binding **NetTcpRelayBinding** gebruiken.|1,8|Microsoft Azure-Service Bus|

### <a name="exploring-features"></a>Functies verkennen

Voorbeelden die diverse functies van de Service Bus relay illustreren.

|De naam van de steekproef|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Indirecte SMS verificatie: Eenvoudige WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Laat zien hoe een eenvoudige web token referentie gebruiken om te verifiëren met Service Bus. De steekproef is vergelijkbaar met een voorbeeld van de Echo, met een paar wijzigingen. In dit voorbeeld wordt specifiek, een gedrag toegevoegd in de ChannelFactory (client)-toepassingen en ServiceHost (service).|1,8|Microsoft Azure-Service Bus|
|[Indirecte Messaging: Taakverdeling](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Laat zien hoe Microsoft Azure-Service Bus gebruiken om berichten te routeren naar meerdere ontvangers. Meerdere exemplaren van een eenvoudige service communiceren met een client via de binding **NetTcpRelayBinding** worden weergegeven|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: Netto gebeurtenis](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Laat zien met de binding **NetEventRelayBinding** op Microsoft Azure-Service bevinden.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: WS2007Http sessie](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Ziet u de binding **WS2007HttpRelayBinding** gebruikt met betrouwbare sessies ingeschakeld. Ook ziet u hoe u de Service Bus referenties opgeven in het configuratiebestand in plaats van via een programma.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Laat zien hoe u de binding **WS2007HttpRelayBinding** met berichtbeveiliging end-to-end-berichten en daarbij nog steeds clients om te verifiëren met Service Bus beveiligen.|1,8|Microsoft Azure-Service Bus|
|[Indirecte Messaging: Uitwisseling van metagegevens](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Laat zien hoe om weer te geven van een metagegevens-eindpunt dat gebruikmaakt van de binding relay. **MetadataExchange** wordt ondersteund in de volgende relay bindingen: **NetTcpRelayBinding**, **NetOnewayRelayBinding** **BasicHttpRelayBinding**en **WS2007HttpRelayBinding**.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: NetTcp-Direct](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Laat zien hoe de binding **NetTcpRelayBinding** ter ondersteuning van de **hybride** verbindingsmodus eerst een indirecte verbinding tot stand brengt, en indien mogelijk, schakelt u automatisch naar een directe verbinding tussen een client en een service configureren.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: NetTcp-MsgSec gebruikersnaam](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Laat zien hoe u de binding **NetTcpRelayBinding** met berichtbeveiliging.|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: Netto enkele reis](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Laat zien hoe laten zien en gebruiken van een service-eindpunt met de binding **NetOnewayRelayBinding** .|1,8|Microsoft Azure-Service Bus|
|[Indirecte SMS bindingen: WS2007Http eenvoudig](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Laat zien met de binding **WS2007HttpRelayBinding** . Laat een eenvoudige service die geen beveiligingsopties gebruikt en niet vereist is voor clients om te verifiëren.|1,8|Microsoft Azure-Service Bus|

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor overzichten van concepten van Service Bus.

- [Overzicht van de service Bus-relay](service-bus-relay-overview.md)
- [Service-busarchitectuur](../service-bus-messaging/service-bus-architecture.md)
- [Service Bus over grondbeginselen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)