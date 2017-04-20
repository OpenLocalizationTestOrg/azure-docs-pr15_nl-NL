<properties
   pageTitle="Service-communicatie met de ASP.NET Web API | Microsoft Azure"
   description="Informatie over het implementeren van service communicatie met stapsgewijze met behulp van de ASP.NET-Web-API met OWIN hostingprovider zelf in de betrouwbare Services-API."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Aan de slag: Service stof Web API-services met OWIN selfservice hostingprovider

Wanneer u bent bepalen van de manier waarop uw services om te communiceren met gebruikers en met elkaar, Azure-Service stof de kracht plaatst in uw handen. Deze zelfstudie bevat informatie over het implementeren van service communicatie ASP.NET Web API gebruiken met Open Web-Interface voor .NET (OWIN) hostingprovider zelf in de Service stof betrouwbare Services API. We moet nauw delve naar betrouwbaar Services pluggable communicatie API. We gaan Web API ook gebruiken in een stapsgewijs voorbeeld om aan te geven het instellen van een aangepaste communicatie luisteraar ervan af.


## <a name="introduction-to-web-api-in-service-fabric"></a>Inleiding tot Web API in de Service stof

ASP.NET-webpagina API is een populaire en krachtige kader voor het samenstellen van HTTP APIs boven aan het .NET Framework. Als u niet al bekend met het kader bent, raadpleegt u [aan de slag met ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) voor meer informatie.

Web API in de Service stof is de dezelfde ASP.NET Web API-u kent en al kent. Het verschil is in hoe u *host* een Web API-toepassing. Won't u Microsoft Internet Information Services (IIS). Meer informatie over het verschil, laten we dit opsplitsen in twee delen:

 1. De Web API-toepassing (inclusief controllers en modellen)
 2. De host (de webserver, meestal IIS)

Een Web API-toepassing zelf wordt niet gewijzigd. Deze niet afwijkt van Web API-toepassingen die u hebt gemaakt in het verleden, en u moet kunnen gewoon plaatst u de meeste van de toepassingscode. Maar als u hebt hosten op IIS, waar u de toepassing host mogelijk iets anders dan wat u gewend bent te. Voordat we u om de hostingprovider deel, laten we beginnen met iets meer vertrouwde: de Web API-toepassing.


## <a name="create-the-application"></a>De toepassing maken

Beginnen met het maken van een nieuwe Service stof-toepassing met een service voor eenmalige stateless in Visual Studio-2015:

![Maak een nieuwe Service stof-toepassing](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Een Visual Studio-sjabloon voor een stateless service Web API gebruiken is beschikbaar voor u. In deze zelfstudie hebt we maken op een project Web API maken die het resultaat in wat u krijgt als u deze sjabloon hebt geselecteerd.

Selecteer een lege Stateless Service project voor meer informatie over het maken van een project Web API maken, of u kunt beginnen met de stateless service Web API-sjabloon en gewoon meelezen.  

![Een service voor eenmalige stateless maken](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

De eerste stap is om op te halen in sommige NuGet-pakketten voor Web API. Het pakket dat we wilt gebruiken is Microsoft.AspNet.WebApi.OwinSelfHost. Dit pakket bevat alle benodigde Web API-pakketten en de *host* -pakketten. Dit is belangrijk later.

![Web API maken met behulp van de NuGet Package Manager](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Nadat de pakketten zijn geïnstalleerd, kunt u building uit de basisstructuur voor Web API-project begint. Als u Web API hebt gebruikt, is de projectstructuur ziet er erg bekend voorkomen. Begin met het toevoegen van een `Controllers` directory en de controller van een eenvoudige waarden:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Voeg nu een Opstartklasse op de hoofdmap van het project te registreren de routering, formatters en eventuele andere configuratie-instellingen. Dit is ook waar Web API op aansluit naar de *host*, die later opnieuw worden herzien. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Dat is het voor de toepassingsonderdeel. Nu we hebt alleen de eenvoudige Web API project vensterindeling instelt. Dusverre deze niet mag veel er anders uitzien van Web API-projecten die u hebt gemaakt in het verleden of de eenvoudige Web API-sjabloon. Uw bedrijfslogica vindt u in de controllers en modellen zoals u gewend bent.

Nu wat we doen over het hosten van zodat we daadwerkelijk kunt uitvoeren?

## <a name="service-hosting"></a>Service hosten

In de Service stof, is uw service wordt uitgevoerd in een *service hostproces*, een uitvoerbaar bestand dat wordt uitgevoerd van uw servicecode. Wanneer u een service met behulp van de betrouwbare Services-API schrijft, worden alleen uw project service gecompileerd naar een uitvoerbaar bestand dat uw servicetype geregistreerd en wordt uitgevoerd van uw code. Dit geldt in de meeste gevallen wanneer u een service-Service stof in .NET schrijven. Wanneer u Program.cs in het project stateless service opent, ziet u het volgende:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Als die eruit ziet verdacht gedraagt het ingangspunt voor een consoletoepassing, komt dit doordat deze is.

Meer informatie over de service-hostproces en de service is geregistreerd zijn buiten het bereik van dit artikel. Maar het is belangrijk dat u weet nu dat *dat de servicecode van uw wordt uitgevoerd in een eigen proces*.

## <a name="self-host-web-api-with-an-owin-host"></a>Selfservice Web API hosten bij een OWIN-host

Gezien het feit dat de code van uw Web API-toepassing wordt gehost in een eigen proces, hoe u wijzen tot een webserver? Voer [OWIN](http://owin.org/). OWIN is gewoon een overeenkomst tussen .NET webtoepassingen en -endwebservers. Traditioneel wanneer ASP.NET (maximaal MVC 5) wordt gebruikt, wordt de webtoepassing is nauw naar IIS via System.Web verbonden. Web API implementeert echter OWIN, zodat u kunt een webtoepassing die is ontkoppelde schrijven van de webserver die als host fungeert. Reden, kunt u een *zelf gehost* OWIN webserver waarop u kunt in uw eigen proces. Hiermee past perfect bij de Service stof hostingprovider model die hierboven wordt beschreven.

In dit artikel gebruiken we Katana als host OWIN voor het Web API-toepassing. Katana is een open source OWIN host implementatie die is gebaseerd op [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) en de Windows- [API voor HTTP-Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [AZURE.NOTE] Meer informatie over Katana, gaat u naar de [site Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Zie voor een kort overzicht van het gebruik van Katana voor het hosten van selfservice Web API, [OWIN naar Self-Host ASP.NET Web API 2 gebruiken](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>De webserver instellen

De betrouwbare Services-API biedt een communicatie-ingangspunt waar u communicatie stapels waarmee gebruikers en -clients verbinding maken met de service kunt aansluiten:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

De webserver (en andere communicatie-stack die u in de toekomst, zoals WebSockets gebruiken) moeten de interface ICommunicationListener gebruiken voor het correct integreren met het systeem. De redenen worden zichtbaarder in de volgende stappen.

Maak eerst een klasse OwinCommunicationListener dat ICommunicationListener implementeert genoemd:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

De ICommunicationListener-interface biedt drie methoden voor het beheer van een communicatie luisteraar ervan af voor uw service:

 - *OpenAsync*. Beginnen met luisteren voor aanvragen.
 - *CloseAsync*. Stoppen met het luisteren naar aanvragen, einddatum tijdens een vlucht verzoeken en afsluiten.
 - *Afbreken*. Alles annuleren en direct wilt stoppen.

Toevoegen als u wilt beginnen, klassenleden voor mogelijke die de luisteraar ervan af moet werken. Deze worden geïnitialiseerd via de constructor en later wordt gebruikt bij het instellen van de luisteren URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>OpenAsync implementeren

Als u wilt instellen de webserver, moet u twee soorten informatie:

 - *Een URL-pad voorvoegsel*. Hoewel dit optioneel is, is het handig zijn voor u dit nu instellen zodat u meerdere webservices veilig in uw toepassing hosten kunt.
 - *Een poort*.

Voordat u een poort voor de webserver krijgt, is het belangrijk dat u weet dat stof Service een toepassingslaag die als buffer tussen uw toepassing en het onderliggende besturingssysteem dat deze wordt uitgevoerd biedt fungeert op. Als zodanig kunt Service stof u voor het configureren van de *eindpunten* voor de services. Service stof zorgt ervoor dat de eindpunten beschikbaar voor uw service zijn gebruiken. Op deze manier die u niet hoeft te configureren ze zelf in de onderliggende OS-omgeving. U kunt eenvoudig uw toepassing Service stof in verschillende omgevingen hosten zonder wijzigingen aanbrengen in uw toepassing. (Bijvoorbeeld, kunt u hosten dezelfde toepassing in Azure wordt aangegeven of in uw eigen datacenter.)

Een eindpunt HTTP op PackageRoot\ServiceManifest.xml configureren:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Deze stap is belangrijk omdat het hostproces voor de service wordt uitgevoerd onder beperkte referenties (Netwerkservice op Windows). Dit betekent dat uw service geen toegang tot een HTTP-eindpunt instellen op een eigen. Met de eindpuntconfiguratie, weet Service stof voor het instellen van de juiste toegangsbeheerlijst (ACL) voor de URL die de service luistert op. Service stof ook hier standaard eindpunten configureren.


Terug in OwinCommunicationListener.cs, kunt u beginnen OpenAsync implementeren. Dit is waar u de webserver begint. Eerst de informatie van het eindpunt en de URL die de service luistert op maken. De URL is verschillen afhankelijk van of de luisteraar ervan af in een stateless service of een statuscontrole service wordt gebruikt. Voor een statuscontrole service moet de luisteraar ervan af een uniek adres voor elke statuscontrole service replica die luistert het programma op maken. Voor stateless services, kan het adres niet veel eenvoudiger. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Houd er rekening mee dat "http://+" Hier wordt gebruikt. Dit is om ervoor te zorgen dat de webserver op alle beschikbare adressen luistert, inclusief localhost, FQDN en IP-computer.

De OpenAsync-implementatie is een van de belangrijkste redenen waarom de webserver (of een stapel communicatie) is geïmplementeerd als een ICommunicationListener, in plaats van dat deze rechtstreeks vanuit openen `RunAsync()` in de service. De retourwaarde van OpenAsync is het adres dat de webserver luistert op. Als dit adres aan het systeem wordt geretourneerd, markeert Hiermee u het adres met de service. Service stof biedt een API waarmee clients en andere services kunnen vervolgens vragen om dit adres door de servicenaam van de. Dit is belangrijk omdat het serviceadres niet statische is. Services worden verplaatst rond in het cluster voor resource taakverdeling en beschikbaarheid van de toepassing. Dit is de methode waarmee clients voor het oplossen van het luisteren adres van een service.

Met daarom OpenAsync Hiermee start u de webserver en geeft als resultaat het adres dat deze luistert op. Opmerking dat deze op 'http://+', maar voordat OpenAsync geeft als het adres resultaat, luistert de 'en' vervangen door het IP- of FQDN-naam van het knooppunt dat momenteel ingeschakeld is. Het adres dat wordt geretourneerd door deze methode is wat geregistreerd bij het systeem. Het is ook wat clients en andere services te zien wanneer ze om van een service-adres vragen. Voor clients correct tot stand te brengen, moeten ze een werkelijke IP- of FQDN-naam in het adres.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Houd er rekening mee dat deze verwijst naar de klas opstarten dat is doorgegeven aan de OwinCommunicationListener in de constructor. Dit exemplaar opstarten wordt gebruikt door de webserver naar de Web API-toepassing bootstrap.

De `ServiceEventSource.Current.Message()` lijn wordt weergegeven in het venster diagnostische gebeurtenissen later, wanneer u de toepassing om te bevestigen dat de webserver is gestart uitvoert.

## <a name="implement-closeasync-and-abort"></a>Implementeren CloseAsync en afbreken

Tot slot implementeren CloseAsync en afbreken als u wilt stoppen, de webserver. De webserver kan het worden gestopt door de server handgreep die is gemaakt tijdens OpenAsync verwijdering.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

In dit voorbeeld implementatie CloseAsync zowel afbreken gewoon de webserver stoppen. U kunt ervoor kiezen om de webserver netter gecoördineerde afgesloten in CloseAsync uit te voeren. De afsluiting kan bijvoorbeeld tijdens een vlucht aanvragen moet zijn voltooid voordat de return wachten.

## <a name="start-the-web-server"></a>Start de webserver

U bent nu klaar om te maken en een exemplaar van OwinCommunicationListener te starten, de webserver terug te keren. Terug in de klas Service (WebService.cs), overschrijven de `CreateServiceInstanceListeners()` methode:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Dit is waar de Web API- *toepassing* en de OWIN *host* ten slotte elkaar tegenkomen. De host (OwinCommunicationListener) krijgt een exemplaar van de *toepassing* (Web API) via de klas opstarten. Service stof beheert vervolgens de levenscyclus. Deze hetzelfde patroon kunt meestal met een communicatie-stack worden gevolgd.

## <a name="put-it-all-together"></a>Alle samenvoegen

In dit voorbeeld niet hoeft te doen de `RunAsync()` methode, zodat deze overschrijven kan alleen worden verwijderd.

De uiteindelijke service-implementatie moet heel eenvoudig. Alleen moeten worden gemaakt van de communicatie luisteraar ervan af:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

De volledige `OwinCommunicationListener` class:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Nu dat u hebt alle delen ingevoegd op hun plaats staan, worden uw project ziet er als een normale Web API-toepassing met betrouwbare Services API invoer wordt verwezen en een OWIN-host:


![Web API met betrouwbare Services API-fragment punten en OWIN host](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Uitvoeren en maak verbinding via een webbrowser

Als u dit nog niet hebt gedaan, [uw ontwikkelomgeving instellen](service-fabric-get-started.md).


U kunt nu bouwen en implementeren van uw service. Druk op **F5** in Visual Studio voor bouwen en implementeren van de toepassing. Klik in het venster diagnostische gebeurtenissen ziet u een bericht dat aangeeft dat de webserver geopend op http://localhost:8281 /.


![Visual Studio diagnostische gebeurtenissen-venster](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Als de poort al door een ander proces op uw computer is geopend, ziet u mogelijk een fout hier. Hiermee wordt aangeduid dat de luisteraar ervan af kan niet worden geopend. Als dit het geval is, kunt u met behulp van een andere poort voor de eindpuntconfiguratie van het in ServiceManifest.xml.


Zodra de service wordt uitgevoerd, open een browser en Ga naar [http://localhost:8281/api/waarden](http://localhost:8281/api/values) te testen.

## <a name="scale-it-out"></a>Dit schaal

Stateless WebApps meestal schalen betekent dat er meer machines toevoegen en de web-apps op deze draaien. Engine voor de organisatie van de service stof kan dit voor u betekenen wanneer nieuwe knooppunten worden toegevoegd aan een cluster. Wanneer u exemplaren van een stateless service maakt, kunt u het aantal exemplaren dat u wilt maken. Service stof aantal_tekens hebt opgegeven dat aantal exemplaren op knooppunten in de cluster. En dit ervoor zorgt ervoor dat u niet meer dan één exemplaar maken op een willekeurig één knooppunt. U kunt ook de opdracht geven voor Service stof altijd een exemplaar te maken op elk knooppunt door het opgeven van **-1** voor het aantal exemplaar. Dit zorgt ervoor dat wanneer u knooppunten aan de nieuwe schaal uit uw cluster toevoegen, een exemplaar van uw stateless service wordt gemaakt op de nieuwe knooppunten. Deze waarde is een eigenschap van het exemplaar van de service, zodat deze wordt ingesteld wanneer u een exemplaar van de service maakt. U kunt dit doen via PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

U kunt dit ook doen wanneer u een standaardservice in een Visual Studio stateless service project definiëren:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Zie voor meer informatie over het maken van toepassing en service-exemplaren, [Deploy een toepassing](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Volgende stappen

[Fouten opsporen in uw toepassing Service stof met behulp van Visual Studio](service-fabric-debugging-your-application.md)
