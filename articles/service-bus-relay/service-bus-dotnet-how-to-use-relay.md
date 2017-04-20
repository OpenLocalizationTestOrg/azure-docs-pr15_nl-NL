<properties
    pageTitle="Het gebruik van de Service Bus relay met .NET | Microsoft Azure"
    description="Informatie over het gebruik van de Azure Service Bus relay-service om verbinding maken met twee toepassingen die worden gehost op verschillende locaties."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Het gebruik van de relayservice Azure Service Bus

In dit artikel wordt beschreven hoe de relayservice Service Bus gebruiken. In de voorbeelden zijn geschreven in C# en het gebruik van de Windows Communication Foundation (WCF) API met extensies die zijn opgenomen in de Service Bus constructie. Zie voor meer informatie over de Service Bus relay overview [Service Bus doorgegeven messaging](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Wat is de Service Bus relay?

De service [Service Bus *relay* ](service-bus-relay-overview.md) kunt u hybride toepassingen die worden uitgevoerd in een datacenter van de Azure zowel uw eigen enterprise on-premises omgeving te maken. De Service Bus relay vergemakkelijkt dit doordat u veilig worden Windows Communication Foundation (WCF)-services die zich binnen een zakelijke enterprise-netwerk met de openbare cloud bevinden zonder te hoeven openen van een firewallverbinding of storende wijzigingen moeten de infrastructuur van een bedrijfsnetwerk.

![Relay concepten](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

De Service Bus relay kunt u host WCF-services vanuit uw bestaande enterprise-omgeving. U kunt vervolgens delegeren luisteren naar binnenkomende sessies en aanvragen voor deze naar de Service Bus service wordt uitgevoerd binnen Azure WCF-services. Hiermee kunt u om weer te geven van deze services programmacode uitgevoerd in Azure wordt aangegeven of mobiele medewerkers of extranet partner-omgevingen. Service Bus kunt u veilig bepalen wie toegang tot deze services op een fijnmazige niveau. Deze biedt een krachtige en veilige manier om te laten zien van de functies en gegevens van uw bestaande bedrijfsoplossingen en uw voordeel doen met dit vanuit de cloud.

In dit artikel wordt beschreven hoe de Service Bus relay gebruiken om te maken van een WCF-webservice die wordt weergegeven met een TCP-kanaal binding, waarmee een veilige uitwisseling tussen twee partijen.

## <a name="create-a-service-namespace"></a>Een Servicenaamruimte maken

Als u wilt beginnen met het gebruik van de Service Bus relay in Azure, moet u eerst een naamruimte maken. Een naamruimte bevat een scope container voor de adressering van Service Bus bronnen binnen uw toepassing.

Een Servicenaamruimte maken:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Het pakket Service Bus NuGet ophalen

De [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om de Service Bus API en voor het configureren van uw toepassing met alle de Service Bus afhankelijkheden. Ga als volgt te werk als u wilt het pakket NuGet installeert in uw project:

1.  Klik in Solution Explorer met de rechtermuisknop op **verwijzingen**en klik **NuGet-pakketten beheren**.
2.  Zoeken naar 'Service Bus' en selecteer het item **Microsoft Azure Service Bus** . Klik op **installeren** om de installatie te voltooien en sluit het volgende dialoogvenster:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Service Bus gebruiken om te laten zien en gebruiken van een SOAP-webservice met TCP

Als u wilt laten zien voor externe verbruik van een bestaande SOAP WCF-webservice, moet u wijzigingen aanbrengen in de servicebindingen en adressen. Dit kan vragen om wijzigingen in het configuratiebestand of kan codewijzigingen, afhankelijk van hoe u hebt ingesteld en geconfigureerd uw WCF-services vereist. Houd er rekening mee dat WCF u meerdere netwerkeindpunten via dezelfde service kunt, zodat u de bestaande interne eindpunten behouden kunt bij het toevoegen van Service Bus eindpunten voor externe toegang op hetzelfde moment.

In deze taak u bouwen van een eenvoudige WCF-service en een Service Bus luisteraar ervan af aan toe te voegen. Deze oefening wordt ervan uitgegaan basisinformatie over Visual Studio en daarom worden niet Doorloop de details van een project te maken. In plaats daarvan de nadruk op de code.

Voordat u begint de volgende stappen uit, voer de volgende procedure voor het instellen van uw omgeving:

1.  Maak een consoletoepassing die twee projecten 'Client' en 'Service', binnen de oplossing bevat in Visual Studio.
2.  Het pakket Microsoft Azure-Service Bus NuGet aan beide projecten toevoegen. Dit pakket voegt alle benodigde constructie-verwijzingen naar uw projecten.

### <a name="how-to-create-the-service"></a>Het maken van de service

U maakt eerst de service zelf. Een WCF-service bestaat uit ten minste drie onderdelen:

-   De definitie van een nieuw contract waarin wordt beschreven welke berichten worden uitgewisseld en wat zijn bewerkingen moet worden gestart.
-   Uitvoering van deze contract.
-   Host die fungeert als host de WCF-service en beschrijft de verschillende eindpunten.

De codevoorbeelden in deze sectie adres elk van deze onderdelen.

Het contract definieert één bewerking, `AddNumbers`, die twee getallen worden opgeteld en wordt het resultaat. De `IProblemSolverChannel` interface kunnen de client om eenvoudiger de levensduur proxy. Maken van een interface wordt aanbevolen een. Dit is een goed idee om de definitie van deze overeenkomst in een afzonderlijk bestand zetten, zodat u kunt verwijzen naar dat bestand uit uw "Client" en "Service" projecten, maar u ook de code naar beide projecten kopiëren kunt.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Met de overeenkomst op hun plaats staan is de implementatie eenvoudig.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Een ServiceHost via een programma configureren

Met de overeenkomst en de implementatie op hun plaats staan, kunt u nu de service hosten. Hostingprovider plaatsvindt binnen een object [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) zorgt voor het beheren van exemplaren van de service en host de eindpunten die voor berichten luisteren. De volgende code configureert de service met zowel een normale lokaal eindpunt en een eindpunt Service Bus om te laten zien het uiterlijk, naast elkaar worden geïnstalleerd, van interne en externe eindpunten. Vervang de tekenreeks *naamruimte* door de naamruimtenaam en *yourKey* met de toets SA's die u in de vorige instellingsstap hebt gekregen.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

In het voorbeeld maakt u twee eindpunten die zich op de dezelfde contract-implementatie. Een lokale is en een geprojecteerd via Service-Bus. De belangrijke verschillen tussen de notitieblokken zijn de bindingen; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) voor het lokale nummer en [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) voor het eindpunt van de Service Bus en de adressen. Het lokale eindpunt heeft een adres lokale netwerk met een afzonderlijke poort. Het eindpunt van de Service Bus heeft een eindpuntadres van bestaat uit de tekenreeks `sb`, de naamruimtenaam van de en het pad "Oplosser." Dit resulteert in de URI `sb://[serviceNamespace].servicebus.windows.net/solver`, het service-eindpunt verifiëren met een volledig gekwalificeerde externe DNS-naam of een Service Bus TCP-eindpunt. Als u de code worden vervangen door de tijdelijke aanduidingen in het `Main` functie van **de servicetoepassing,** treedt er een functionele service. Als u uw service wilt moeten beluisteren uitsluitend op Service bevinden, verwijdert u de declaratie van de lokale eindpunt.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Een ServiceHost in het bestand App.config configureren

U kunt ook de host met behulp van het bestand App.config configureren. De code in dit geval hostingprovider service wordt weergegeven in het volgende voorbeeld.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

De definities van het eindpunt verplaatsen naar het bestand App.config. Het pakket NuGet heeft een bereik van definities al aan het bestand App.config, dat het vereiste configuratie selectievakje extensies voor Service Bus zijn toegevoegd. Het volgende voorbeeld, dat wil het exacte equivalent van de vorige code zeggen, direct onder het element **traceerbron** worden weergegeven. In dit voorbeeld wordt ervan uitgegaan dat de naamruimte van uw project C# naam **Service**.
Vervang de tijdelijke aanduidingen door uw Service Bus Servicenaamruimte en SA's-toets.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Nadat u deze wijzigingen hebt aangebracht, de service wordt gestart als voorheen, maar met twee live eindpunten: één plaatselijke en één met gebruikers zonder licentie in de cloud.

### <a name="create-the-client"></a>Maken van de client

#### <a name="configure-a-client-programmatically"></a>Een client via een programma configureren

De service in beslag neemt, kunt u een WCF-client met behulp van een object [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) maken. Service-Bus gebruikt een token gebaseerde beveiliging-model dat is geïmplementeerd met SA's. De klasse [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) geeft een token beveiligingstokenprovider met ingebouwde factory-methoden die resulteren in sommige bekende token providers. Het volgende voorbeeld wordt de methode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) u omgaat met de overname van het juiste SA's token. De naam en de toets komen uit de portal opgehaald, zoals wordt beschreven in de vorige sectie.

Eerst overzicht of kopieer het `IProblemSolver` de code van de service contracten in uw project client.

Vervang vervolgens de code in de `Main` methode van de client, opnieuw worden vervangen door de tijdelijke aanduiding voor tekst met uw Service Bus naamruimte en SA's-toets.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

U kunt nu maken op de client en de service, laat u ze (de service eerst uitgevoerd) en de client belt de service en wordt afgedrukt **9**. U kunt de client en server op verschillende computers, uitvoeren zelfs over netwerken en communicatie werken nog. De clientcode kan ook worden uitgevoerd in de cloud of lokaal.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Een client in het bestand App.config configureren

De volgende code ziet hoe u de client met behulp van het bestand App.config configureren.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

De definities van het eindpunt verplaatsen naar het bestand App.config. Het volgende voorbeeld, dat hetzelfde als de code die eerder is vermeld is, moet worden weergegeven direct onder het element **traceerbron** . Hier als voorheen, moet u vervangen door de tijdelijke aanduidingen voor uw Service Bus naamruimte en SA's-toets.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisprincipes van de Service Bus relayservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

- [Service Bus doorgegeven SMS-overzicht](service-bus-relay-overview.md)
- [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Voorbeelden van de Service Bus downloaden van [Azure voorbeelden][] of raadpleegt u het [overzicht van de Service Bus voorbeelden][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Voorbeelden van Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [overzicht van de Service Bus voorbeelden]: ../service-bus-messaging/service-bus-samples.md