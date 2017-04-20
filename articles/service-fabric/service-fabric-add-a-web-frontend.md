<properties
   pageTitle="Maken van een front-end van web voor uw toepassing met ASP.NET Core | Microsoft Azure"
   description="Uw Service stof-toepassing op het web weergeven met behulp van een ASP.NET Core Web API-project en tussen antwoordgroepservice communicatie via ServiceProxy."
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
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Een web-service-front-end voor uw toepassing met ASP.NET Core maken

Azure-Service configuratieservices bieden standaard geen een openbare interface op het web. Als u wilt laten zien de functionaliteit van de toepassing op HTTP-clients, moet u een project web fungeren als een beginpunt en klik vervolgens delen vanaf hier uw afzonderlijke services.

In deze zelfstudie we verdergaan waar we gebleven in deze zelfstudie [maken van de eerste toepassing in Visual Studio was](service-fabric-create-your-first-application-in-visual-studio.md) en een webservice vóór de service statuscontrole item toevoegen. Als u dit nog niet hebt gedaan, moet u terug te gaan en deze zelfstudie eerst stapsgewijs.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Een service van ASP.NET Core toevoegen aan uw toepassing

ASP.NET-Core is een kader voor het ontwikkelen van licht, platforms web die u gebruiken kunt om te maken van moderne web UI en API's met webonderdelen. Laten we een ASP.NET Web API-project toevoegen aan de bestaande toepassing.

>[AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u [.NET Core 1.0 installeren][dotnetcore-install].

1. Klik in Solution Explorer met de rechtermuisknop op de **Services** binnen het toepassingsproject en kies **toevoegen > nieuwe Service stof Service**.

    ![Een nieuwe service toevoegen aan een bestaande toepassing][vs-add-new-service]

2. Klik op de pagina **maken een Service** **ASP.NET Core** kiezen en geef deze een naam.

    ![ASP.NET-webservice kiezen in het nieuwe dialoogvenster service][vs-new-service-dialog]

3. De volgende pagina biedt een aantal ASP.NET Core projectsjablonen. Houd er rekening mee dat deze dezelfde sjablonen die u zien zijn wilt als u een project ASP.NET Core buiten een toepassing voor de Service stof gemaakt. Voor deze zelfstudie kiezen we **Web API**. U kunt echter dezelfde concepten toepassen tot het bouwen van een volledige webtoepassing.

    ![ASP.NET-projecttype kiezen][vs-new-aspnet-project-dialog]

    Als uw project Web API is gemaakt, hebt u twee services in uw toepassing. Terwijl u eraan uw toepassing maken, voegt u meer services op precies dezelfde manier. Elk kunnen afzonderlijk versienummer en een bijgewerkte zijn.

>[AZURE.TIP] Meer informatie over het samenstellen van ASP.NET Core services, raadpleegt u de [Documentatie van ASP.NET-Core](https://docs.asp.net).

## <a name="run-the-application"></a>Voer de toepassing

Als u een idee van wat we al hebt gedaan, laten we u de nieuwe toepassing te implementeren en gaat u naar het standaardgedrag vindt u de ASP.NET Core Web API-sjabloon.

1. Druk op F5 in Visual Studio om fouten opsporen in de app.

2. Wanneer implementatie voltooid is, wordt de browser naar de hoofdmap van de service ASP.NET Web API--bijvoorbeeld http://localhost:33003 gestart in Visual Studio. Het poortnummer willekeurig worden toegewezen en kan afwijken op uw computer. De sjabloon ASP.NET Core Web API biedt geen standaardgedrag voor de hoofdsite en zodat u een fout in de browser krijgt.

3. Toevoegen `/api/values` naar de locatie in de browser. Hiermee wordt aangeroepen de `Get` methode op de ValuesController in de Web API-sjabloon. Het standaardantwoord die is verstrekt door de sjabloon--een JSON-matrix met twee tekenreeksen wordt geretourneerd:

    ![Standaardwaarden geretourneerd van ASP.NET Core Web API-sjabloon][browser-aspnet-template-values]

    Aan het einde van de zelfstudie, wordt hebben we vervangen deze waarden met de meest recente itemwaarde van onze statuscontrole service.


## <a name="connect-the-services"></a>Verbinding maken met de services

Service stof flexibel voltooid hoe u betrouwbare services communiceert. U wellicht binnen één toepassing, services die toegankelijk zijn via TCP, andere diensten die toegankelijk via een HTTP REST API zijn en nog steeds andere diensten die toegankelijk via web sockets zijn. Achtergrondinformatie over de beschikbare opties en welke compromissen nodig zijn voor het nodig zijn, raadpleegt u [Communicating met services](service-fabric-connect-and-communicate-with-services.md). In deze zelfstudie we Voer een van de eenvoudiger manieren en gebruik de `ServiceProxy` / `ServiceRemotingListener` klassen die beschikbaar zijn in de SDK.

In de `ServiceProxy` benadering (gebaseerd op RPC of RPC's), u een interface als de opdracht voor de service definiëren. Vervolgens kunt u die interface genereren van een proxyklasse voor de communicatie met de service.


### <a name="create-the-interface"></a>De interface maken

We zijn verzonden door de interface fungeert als de overeenkomst tussen de statuscontrole-service en de clients, met inbegrip van het project ASP.NET Core maken.

1. Met de rechtermuisknop op de oplossing in Solution Explorer en kies **toevoegen** > **Nieuw Project**.

2. Kies het **Visual C#** -fragment in het linkernavigatiedeelvenster en selecteert u de sjabloon **Klassenbibliotheek** . Zorg ervoor dat de .NET Framework-versie is ingesteld op **4.5.2**.

    ![Een project interface voor uw statuscontrole service maken][vs-add-class-library-project]

3. Om een interface worden gebruikt door `ServiceProxy`, moet deze zijn afgeleid van de IService-interface. Deze interface wordt opgenomen in een van de Service stof NuGet-pakketten. Het pakket toevoegen, met de rechtermuisknop op het nieuwe project van de klasse-bibliotheek en kies **NuGet-pakketten beheren**.

4. Zoek voor het pakket **Microsoft.ServiceFabric.Services** en installeer deze.

    ![Het pakket Services NuGet toevoegen][vs-services-nuget-package]

5. In de bibliotheek class, maakt u een interface met één methode: `GetCountAsync`, en de interface van IService uitbreiden.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Implementeer de interface in uw statuscontrole service

Nu we de interface hebt gedefinieerd, moet deze in de statuscontrole service omzetten.

1. In uw statuscontrole service, toevoegen een verwijzing naar de bibliotheek lesproject waarin de interface.

    ![Een verwijzing naar de bibliotheek lesproject toevoegen in de statuscontrole service][vs-add-class-library-reference]

2. Zoek de klasse die van overneemt `StatefulService`, zoals `MyStatefulService`, en deze willen implementeren uitbreiden de `ICounter` interface.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Nu implementeren de één-methode die is gedefinieerd in de `ICounter` interface, `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Laten zien van de statuscontrole service met een externe toegang van de luisteraar ervan af

Met de `ICounter` interface geïmplementeerd, de laatste stap bij het inschakelen van de statuscontrole service moeten aanroepbare van andere services is een communicatiekanaal openen. Statuscontrole-services, Service stof biedt een overschrijfbare methode genoemd `CreateServiceReplicaListeners`. Met deze methode kunt u een of meer communicatie listeners, op basis van het type communicatie die u met uw service wilt inschakelen.

>[AZURE.NOTE] De overeenkomstige methode voor het openen van een communicatiekanaal naar stateless services wordt aangeroepen `CreateServiceInstanceListeners`.

In dit geval we wordt vervangen door de bestaande `CreateServiceReplicaListeners` methode en geef een exemplaar van `ServiceRemotingListener`, waarbij een eindpunt RPC die automatisch is aanroepbare van clients via `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Gebruik van de klas ServiceProxy voor interactie met de service

Onze statuscontrole service is nu klaar voor het ontvangen van andere services. Blijft is zodat het de code voor communicatie met deze uit de ASP.NET-webservice toevoegen.

1. Klik in uw project ASP.NET toevoegen een verwijzing naar de klassebibliotheek waarin het `ICounter` interface.

2. Open in het menu **Build** en de **Configuration Manager**. Worden er ongeveer zo uitziet:

    ![Configuration manager waarin klassenbibliotheek als/platform][vs-configuration-manager]

    Houd er rekening mee dat de bibliotheek lesproject, **MyStatefulService.Interface**, is geconfigureerd voor Any CPU opbouwen. Als u wilt goed werkt met Service stof, moet deze worden expliciet gericht op x64. Klik op de vervolgkeuzelijst Platform en kies de optie **Nieuw**en maken van een x64 platformconfiguratie.

    ![Maken van nieuwe platform voor klassenbibliotheek][vs-create-platform]

3. Net zoals u als voor de bibliotheek lesproject eerder, moet u het pakket Microsoft.ServiceFabric.Services toevoegen aan het project ASP.NET. Hierdoor krijgt de `ServiceProxy` class.

4. Open in de map **Controllers** de `ValuesController` class. Houd er rekening mee dat het `Get` methode momenteel alleen geeft als resultaat een vastgelegde tekenreeksmatrix van "waarde1" en "waarde2"--die overeenkomt met wat hebt gezien eerder in de browser. Deze implementatie vervangen door de volgende code:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    De eerste regel met code is de belangrijkste. Als u wilt de proxy ICounter bij de statuscontrole service hebt gemaakt, moet u twee soorten informatie: een partition-ID en de naam van de service.

    U kunt gebruiken met schaal statuscontrole services partitioneren door splitsen hun status in verschillende categorieën, op basis van een sleutel die u, zoals een klant-ID of postcode definieert. In ons alledaagse-toepassing heeft de statuscontrole service slechts één partition, zodat de sleutel maakt niet uit. Een toets die u opgeeft leidt tot de dezelfde partition. Meer informatie over het partitioneren services, raadpleegt u [hoe u partition betrouwbare configuratieservices Service](service-fabric-concepts-partitioning.md).

    De servicenaam van de is een URI van het formulier stof: /&lt;toepassingsnaam&gt;/&lt;servicenaam&gt;.

    Met deze twee soorten informatie aanduiding Service stof de computer waarop aanvragen moeten worden verzonden naar unieke. De `ServiceProxy` class verwerkt ook naadloos wanneer de computer waarop de statuscontrole service partition gehost mislukt en een andere computer moet worden gepromoveerd ter vervanging. Deze abstraction zorgt ervoor dat de clientcode wilt omgaan met andere services aanzienlijk eenvoudiger te schrijven.

    Als we de proxy hebben, roepen we gewoon de `GetCountAsync` methode en formuleresultaten terug te keren.

5. Druk op F5 om opnieuw uit te voeren van de gewijzigde toepassing. Als wordt voordat, Visual Studio automatisch starten de browser naar de hoofdmap van het webproject. Voeg het pad "api/waarden" en ziet u de huidige itemwaarde als resultaat gegeven.

    ![De waarde van de statuscontrole teller weergegeven in de browser][browser-aspnet-counter-value]

    Vernieuw de browser regelmatig om te zien van de waarde van de teller bijwerken.


>[AZURE.WARNING] De ASP.NET Core webserver die beschikbaar zijn in de sjabloon, bekend als Kestrel, is [momenteel niet ondersteund voor het afhandelen van directe internetverkeer](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Voor productie scenario's, kunt u bovendien de eindpunten ASP.NET Core achter [API Management] hostingprovider[ api-management-landing-page] of een ander internetgerichte gateway. Houd er rekening mee dat Service stof niet wordt ondersteund voor implementatie in IIS.


## <a name="what-about-actors"></a>Hoe zit het met betrokkenen?

Deze zelfstudie gericht over het toevoegen van een front-end van web die uitgewisseld met een statuscontrole-service. U kunt echter een vergelijkbaar model wie u betrokkenen kunt volgen. Ja, is het iets eenvoudiger.

Wanneer u een project acteur maakt, worden een interface-project door Visual Studio automatisch voor u gegenereerd. U kunt die interface genereert een acteur-proxy in het webproject om te communiceren met de acteur. Het communicatiekanaal wordt automatisch weergegeven. Dus u niet hoeft te ondernemen die gelijk is aan het tot stand brengen van een `ServiceRemotingListener` zoals u in deze zelfstudie hebben gedaan voor de statuscontrole service.

## <a name="how-web-services-work-on-your-local-cluster"></a>De werking van webservices op uw lokale cluster

U kunt in het algemeen, implementeren precies dezelfde Service stof toepassing aan een meerdere machines cluster die u op uw lokale cluster geïmplementeerd en ten zeerste zeker dat het werkt zoals u zou verwachten. Dit is omdat uw lokale cluster gewoon een configuratie vijf knooppunt dat is samengevouwen tot één computer.

Wanneer u naar webservices gaat, is er echter een belangrijke aspect. Wanneer uw cluster bevindt zich achter een taakverdeling, net zoals in Azure wordt aangegeven geldt, moet u ervoor zorgen dat uw webservices worden geïmplementeerd op elke computer sinds de taakverdeling gewoon round robin verkeer wordt op de computers. U kunt dit doen door in te stellen de `InstanceCount` voor de service aan de speciale waarde van-'1'.

De service wordt daarentegen uitgevoerd wanneer u een webservice lokaal uitvoert, moet u ervoor zorgen dat slechts één exemplaar van. U wordt anders conflicten ondervindt uit meerdere processen die op het pad en de poort luisteren. Het aantal web service exemplaar moet Hierdoor kunnen worden ingesteld op '1' voor lokale implementaties.

Als u wilt weten hoe u verschillende waarden voor verschillende omgeving configureren, raadpleegt u [beheren toepassingsparameters voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Volgende stappen

- [Maak een cluster in Azure wordt aangegeven voor het implementeren van uw toepassing in de cloud](service-fabric-cluster-creation-via-portal.md)
- [Meer informatie over het communiceren met services](service-fabric-connect-and-communicate-with-services.md)
- [Meer informatie over het partitioneren statuscontrole services](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
