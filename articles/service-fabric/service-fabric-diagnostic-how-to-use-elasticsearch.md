<properties
   pageTitle="Elasticsearch gebruiken als een Service stof toepassing doelcellen winkel | Microsoft Azure"
   description="Wordt beschreven hoe stof Service-toepassingen Elasticsearch en Kibana gebruiken kunnen om te slaan, index, en doorzoeken toepassing sporen (Logboeken)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Gebruik Elasticsearch als een Service stof toepassing spoor opslaan
## <a name="introduction"></a>Inleiding
In dit artikel wordt beschreven hoe [Stof van Azure-Service](https://azure.microsoft.com/documentation/services/service-fabric/) -toepassingen **Elasticsearch** en **Kibana** voor de opslagruimte van de toepassing doelcellen gebruiken kunnen, indexering en zoeken. [Elasticsearch](https://www.elastic.co/guide/index.html) is een open source, gedistribueerde en scalable realtime zoeken en de analyse-engine die is zeer geschikt voor deze taak. Dit kan worden geïnstalleerd op Windows en Linux virtuele machines uitgevoerd in Microsoft Azure. Elasticsearch kan *gestructureerde* sporen geproduceerd technologieën zoals **Gebeurtenis Aanwijzen voor Windows (ETW)**gebruiken efficiënt verwerken.

ETW wordt gebruikt door de Service stof runtime diagnostische informatie van gegevensbronnen (sporen). Dit is de aanbevolen methode voor stof Service-toepassingen te hun diagnostische gegevens over de gegevensbron. Via de methode kan voor correlatie tussen runtime geleverde en toepassing opgegeven sporen, en wordt probleemoplossing te vereenvoudigen. Project-sjablonen in Visual Studio Service stof bevatten een logboekregistratie API (gebaseerd op de klas .NET **EventSource** ) die ETW sporen al dan niet standaard genereert. Zie voor een algemeen overzicht van de Service stof toepassing tracering ETW, [controle- en services in de instellingen voor een lokale computer ontwikkeling diagnose](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Voor de traces moet verschijnen in Elasticsearch, die ze moeten worden geregistreerd bij Service stof knooppunten in realtime (terwijl de toepassing wordt uitgevoerd) en verzonden naar een Elasticsearch-eindpunt. Er zijn twee belangrijkste opties voor het vastleggen van doelcellen:

+ **Proces doelcellen vastleggen**  
De toepassing of nog nauwkeuriger serviceproces, is verantwoordelijk voor het verzenden van de diagnostische gegevens naar de store trace (Elasticsearch).

+ **Out-van-process doelcellen vastleggen**  
Een afzonderlijk-agent is traces van de serviceproces of processen vastleggen en stuur deze naar de store doelcellen.

Hieronder wordt beschreven hoe Elasticsearch op Azure hebt ingesteld, de professionals bespreken en nadelen voor beide opties vastleggen en wordt uitgelegd hoe u een service-Service stof om gegevens te verzenden naar Elasticsearch configureren.


## <a name="set-up-elasticsearch-on-azure"></a>Elasticsearch op Azure instellen
De eenvoudigste manier om in te stellen van de service Elasticsearch op Azure is via [**Azure resourcemanager sjablonen**](../azure-resource-manager/resource-group-overview.md). Een volledig [Quickstart Azure resourcemanager sjabloon voor Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) is van Azure Quickstart sjablonen opslagplaats beschikbaar. Deze sjabloon worden gescheiden op te slaan accounts voor schaaleenheden (groepen van knooppunten). Dit kan ook afzonderlijk client en serverknooppunten met verschillende configuraties en verschillende aantallen gegevensschijven gekoppeld inrichten.

Hier we een andere sjabloon, **ES-MultiNode** aangeroepen vanuit de [bibliotheek van Azure diagnostische hulpprogramma's](https://github.com/Azure/azure-diagnostics-tools)gebruiken. Deze sjabloon is eenvoudig te gebruiken, maar er een Elasticsearch cluster beveiligd met HTTP-basisverificatie gemaakt. Voordat u verdergaat, downloadt u de bibliotheek van GitHub naar uw computer (door ze te klonen van de bibliotheek of downloaden van een zipbestand). De sjabloon ES-MultiNode bevindt zich in de map met dezelfde naam.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Een machine Elasticsearch installatiescripts worden uitgevoerd voorbereiden
De eenvoudigste manier om de sjabloon ES-MultiNode gebruiken is via een meegeleverde Azure PowerShell-script genoemd `CreateElasticSearchCluster`. Als u wilt deze script gebruikt, moet u PowerShell-modules en een hulpmiddel **openssl**installeren. De laatste nodig is voor het maken van een sleutel SSH die kan worden gebruikt voor het beheren van uw Elasticsearch-cluster op afstand.

`CreateElasticSearchCluster`script is bedoeld voor gebruiksgemak met de sjabloon ES-MultiNode van een Windows-computer. Het is mogelijk met de sjabloon op een niet-Windows-computer, maar dat scenario valt buiten het bereik van dit artikel.

1. Als u dit nog niet hebt ze al hebt geïnstalleerd, installeert u [**Azure PowerShell modules**](http://aka.ms/webpi-azps). Wanneer u wordt gevraagd, klikt u op **uitvoeren**, klikt u vervolgens **installeren**. Azure PowerShell 1.3 of hoger is vereist.

2. Het hulpprogramma **openssl** is opgenomen in de verdeling van [**Cijfer voor Windows**](http://www.git-scm.com/downloads). Als u dit nog niet hebt gedaan, nu [Cijfer voor Windows](http://www.git-scm.com/downloads) installeren. (De standaardopties voor installatie zijn in orde.)

3. Ervan uitgaande dat cijfer heeft is geïnstalleerd, maar niet opgenomen in het systeempad, opent u een Microsoft Azure PowerShell-venster en voer de volgende opdrachten:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Vervang de `<Git installation folder>` met het cijfer locatie op uw computer; de standaardinstelling is **"C:\Program Files\Git"**. Opmerking de puntkomma aan het begin van het eerste pad.

4. Zorg ervoor dat u bent aangemeld bij Azure (via [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet) en dat u het abonnement waaraan moet worden gebruikt voor het maken van uw cluster elastische zoeken hebt geselecteerd. U kunt controleren of dat het juiste abonnement is geselecteerd met `Get-AzureRmContext` en `Get-AzureRmSubscription` cmdlets.

5. Als u dit nog niet hebt gedaan, kunt u de huidige map wijzigen naar de map ES-MultiNode.

### <a name="run-the-createelasticsearchcluster-script"></a>De CreateElasticSearchCluster-script uitvoeren
Voordat u het script uitvoeren, opent u de `azuredeploy-parameters.json` archiveren en controleer of het opgeven van waarden voor de scriptparameters. De volgende parameters zijn beschikbaar:

|Parameternaam           |Beschrijving|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |De naam die wordt gebruikt voor het maken van de openbaar zichtbaar DNS-naam voor het cluster elastische zoeken (door het domein Azure regio toe te voegen aan de opgegeven naam). Als de parameterwaarde van deze is 'myBigCluster' en de door u gekozen Azure regio West Amerikaans is, is de resulterende DNS-naam voor het cluster bijvoorbeeld myBigCluster.westus.cloudapp.azure.com. <br /><br />Deze naam wordt ook gebruikt als de naam van een hoofdmap voor veel onderdelen die is gekoppeld aan het cluster elastische zoeken, zoals gegevens knooppuntnamen.|
|adminUsername           |De naam van het beheerdersaccount voor het beheer van het cluster elastische zoeken (bijbehorende SSH sleutels worden automatisch gegenereerd).|
|dataNodeCount           |Het aantal knooppunten in het cluster elastische zoeken. De huidige versie van het script wordt geen onderscheid gemaakt tussen gegevens en query knooppunten; alle knooppunten afspelen beide rollen. Standaard is 3 knooppunten.|
|dataDiskSize            |De grootte van gegevensschijven (in GB) die is toegewezen voor elk gegevensknooppunt. Elk knooppunt ontvangt 4 gegevensschijven, uitsluitend zijn specifiek voor elastische Search-service.|
|regio                  |De naam van Azure regio waar het cluster elastische zoeken moet zich bevinden.|
|esUserName              |De gebruikersnaam van de gebruiker die is geconfigureerd voor toegang hebben tot ES cluster (onderhevig aan HTTP basisverificatie). Het wachtwoord maakt geen deel uit van de parameterbestand en moet worden voorzien wanneer `CreateElasticSearchCluster` script wordt aangeroepen.|
|vmSizeDataNodes         |De grootte van de Azure virtuele machines voor knooppunten elastische zoeken. De standaardinstelling Standard_D2.|

U bent nu klaar het script uitvoeren. Geef de volgende opdracht:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

waar 

|Parameternaam script    |Beschrijving|
|-----------------------  |--------------------------|
|`<es-group-name>`        |De naam van de Azure resourcegroep waarin alle elastische zoeken cluster resources worden weergegeven.|
|`<azure-region>`         |De naam van de Azure regio waar het cluster elastische zoeken moet worden gemaakt.|         
|`<es-password>`          |Het wachtwoord voor de gebruiker elastische zoeken.|

>[AZURE.NOTE] Als u een NullReferenceException van de cmdlet Test-AzureResourceGroup krijgen, u bent vergeten aanmelden bij Azure (`Add-AzureRmAccount`).

Als u een foutmelding krijg het script uitvoeren en u bepalen dat de fout wordt veroorzaakt door een parameterwaarde verkeerde sjabloon, corrigeer de parameterbestand en het script opnieuw uitvoeren met de naam van een andere resource-groep. U kunt ook de naam van de dezelfde resource-groep opnieuw en het script opschonen van het oude account door toe te voegen de `-RemoveExistingResourceGroup` -parameter voor de script-aanroep.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Resultaat van de CreateElasticSearchCluster-script uitvoeren
Nadat u hebt uitgevoerd de `CreateElasticSearchCluster` script, de volgende onderdelen van de belangrijkste wordt gemaakt. In dit voorbeeld we wordt ervan uitgegaan dat u 'myBigCluster' hebt gebruikt als de waarde van de `dnsNameForLoadBalancerIP` parameter en dat de regio waar u de cluster gemaakt West Amerikaans is.

|Onderdeel|Naam, locatie en opmerkingen|
|----------------------------------|----------------------------------|
|SSH-toets voor extern beheer |myBigCluster.key-bestand (in de map van waaruit de CreateElasticSearchCluster wordt uitgevoerd). <br /><br />Dit bestand kan worden gebruikt voor verbinding naar het knooppunt beheerder en (via het knooppunt beheerder) naar gegevensknooppunten in het cluster.|
|Beheerder knooppunt                        |myBigCluster-admin.westus.cloudapp.azure.com <br /><br />Een speciale VM voor extern Elasticsearch Clusterbeheer--de enige die verbindingen met externe SSH toestaat. Deze wordt uitgevoerd op de desbetreffende virtuele netwerk de Elasticsearch knooppunten, maar niet alle services Elasticsearch uitgevoerd.|
|Gegevensknooppunten                        |myBigCluster1... myBigCluster*N* <br /><br />Gegevensknooppunten die Elasticsearch en Kibana services worden uitgevoerd. U kunt verbinding maken via SSH voor elk knooppunt, maar alleen via het knooppunt beheerder.|
|Elasticsearch cluster             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Het primaire eindpunt voor het Elasticsearch cluster (Let op het achtervoegsel /es). Het is beveiligd met eenvoudige HTTP-verificatie (de referenties zijn de opgegeven esUserName/esPassword parameters van de sjabloon ES-MultiNode). Het cluster heeft ook het hoofd invoegtoepassing geïnstalleerd (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) voor het Clusterbeheer van de eenvoudige.|
|Kibana-service                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />De Kibana-service is geconfigureerd om gegevens uit het gemaakte Elasticsearch cluster te geven. Het is beveiligd met dezelfde verificatiereferenties als het cluster zelf.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>In-proces versus out-van-process doelcellen vastleggen
In de inleiding gezegd twee fundamentele manier diagnostische gegevens te verzamelen: in- en out-van-proces. Elk heeft sterke en zwakke punten.

Het **proces doelcellen vastleggen** voordelen:

1. *Eenvoudige configuratie en implementatie*

    * De configuratie van diagnostische gegevens verzamelen is slechts een deel van de configuratie van de toepassing. Het is eenvoudig te altijd blijven "synchroon' met de rest van de toepassing.

    * Per toepassing of per service-configuratie is eenvoudig haalbaar.

    * Out-van-process doelcellen vastleggen vereist meestal een aparte implementatie en configuratie van de diagnostische agent, namelijk een extra administratieve taak en een mogelijke bron van fouten. De technologie bepaalde agent kunt vaak slechts één exemplaar van de agent per virtuele machine (knooppunt). Dit betekent dat de configuratie voor de siteverzameling van de diagnostische configuratie wordt gedeeld in alle toepassingen en services worden uitgevoerd op dat knooppunt.

2. *Flexibiliteit*

    * De toepassing kunt de gegevens verzenden waar deze nodig hebt om te gaan, zo lang maken als er is een clientbibliotheek met ondersteuning voor de gerichte systeem voor gegevensopslag. Nieuwe sinks kunnen worden toegevoegd naar wens.

    * Complexe vastleggen, filteren en gegevens-aggregatie regels kunnen worden geïmplementeerd.

    * Een out-van-process doelcellen vastleggen is vaak beperkt door de gegevens gootstenen die ondersteuning biedt voor de agent. Er zijn enkele agenten extensible.

3. *Toegang tot interne toepassingsgegevens en context*

    * De diagnostische subsysteem uitgevoerd binnen de toepassing/service-proces kan de traces met contextuele informatie eenvoudig verbeteren.

    * In de methode out-van-process, moet de gegevens worden verzonden naar een agent via sommige om communicatie tussen processen, zoals Event Tracing voor Windows. Deze methode kan extra beperkingen opleggen.

Het **vastleggen van out-van-process doelcellen** voordelen:

1. *De mogelijkheid om te controleren van de toepassing en vastlopen verzamelen wordt*

    * In het proces doelcellen vastleggen wordt mogelijk niet voltooid als de toepassing kan niet worden gestart of loopt vast. Onafhankelijke agent heeft nog veel meer kans essentieel voor probleemoplossing gegevens vastleggen.<br /><br />

2. *Vervaldatum, stabiliteit en prestaties van beproefde*

    * Een agent die zijn ontwikkeld door een leverancier platform (zoals een Microsoft Azure diagnostische gegevens-agent) is onderhevig aan strikte testen en strijd-beveiliging.

    * Met aanmeldingsproces trace blijkt vastleggen, Wees voorzichtig om ervoor te zorgen dat de activiteit om diagnostische gegevens van het proces van een toepassing niet de belangrijkste taken van de toepassing storen of tijdsinstellingen of prestaties problemen introduceren. Een afzonderlijk actieve agent is minder vatbaar voor deze problemen en is afgestemd op de gevolgen voor het systeem te beperken.

Het is mogelijk om te combineren en profiteren van beide methoden. Daadwerkelijk, kan het zijn de beste oplossing voor veel toepassingen.

We gebruiken hier de **Microsoft.Diagnostic.Listeners bibliotheek** en de in het proces tracering vastleggen om gegevens te verzenden vanuit een toepassing Service stof aan een cluster Elasticsearch.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Diagnostische gegevens naar Elasticsearch verzenden via de bibliotheek Listeners
De bibliotheek Microsoft.Diagnostic.Listeners maakt deel uit van PartyCluster steekproef Service stof toepassing. Gebruiken:

1. Download [de steekproef PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) vanuit GitHub.

2. Kopieer de Microsoft.Diagnostics.Listeners en Microsoft.Diagnostics.Listeners.Fabric-projecten (hele mappen) uit de adreslijst van de steekproef PartyCluster naar de oplossingenmap van de toepassing die u moet de gegevens te verzenden naar Elasticsearch.

3. Open de doel-oplossing, met de rechtermuisknop op het knooppunt oplossing in de Verkenner oplossing en kiest u **Bestaand Project toevoegen**. Het project Microsoft.Diagnostics.Listeners toevoegen aan de oplossing. Herhaal dezelfde voor het project Microsoft.Diagnostics.Listeners.Fabric.

4. Een overzicht van het project van project of de service-projecten toevoegen aan de twee toegevoegde projecten. (Elke service die u moet gegevens te verzenden naar Elasticsearch moet verw Microsoft.Diagnostics.EventListeners en Microsoft.Diagnostics.EventListeners.Fabric).

    ![Projectverwijzingen tot Microsoft.Diagnostics.EventListeners en Microsoft.Diagnostics.EventListeners.Fabric-bibliotheken beheren][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Beschikbaarheid van de service stof algemeen release en Microsoft.Diagnostics.Tracing Nuget pakket
Toepassingen die zijn gemaakt met de beschikbaarheid van de Service stof algemeen versie (2.0.135, vrijgegeven 31 maart 2016) afstemmen **.NET Framework 4.5.2**. Deze versie is de laatste versie van .NET Framework ondersteund door Azure op het moment van de versie GA. Deze versie van het kader ontbreekt helaas bepaalde EventListener APIs die de bibliotheek Microsoft.Diagnostics.Listeners nodig heeft. Omdat EventSource (het onderdeel dat de basis vormt van API's logboekregistratie in stof toepassingen) en EventListener nauw verbonden zijn, moet elk project die de bibliotheek Microsoft.Diagnostics.Listeners gebruikt een alternatieve implementatie van EventSource gebruiken. Deze implementatie is opgegeven door de **Microsoft.Diagnostics.Tracing Nuget pakket**, geschreven door Microsoft. Het pakket is volledig achterwaarts compatibel met EventSource opgenomen in het kader, zodat er geen codewijzigingen nodig dan waarnaar wordt verwezen naamruimte wijzigingen moeten worden weergegeven.

Als u wilt gaan gebruiken de Microsoft.Diagnostics.Tracing uitvoering van de klas EventSource, volg deze stappen voor elke service project die nog moet worden gegevens naar Elasticsearch verzenden:

1. Met de rechtermuisknop op de service-project en kies **Nuget-pakketten beheren**.

2. Overschakelen naar de bron nuget.org-pakket (als dit nog niet is gebeurd) en zoek naar '**Microsoft.Diagnostics.Tracing**'.

3. Installeer de `Microsoft.Diagnostics.Tracing.EventSource` -pakket (en de bijbehorende afhankelijkheden).

4. Open het bestand **ServiceEventSource.cs** of **ActorEventSource.cs** in uw project service en vervang het `using System.Diagnostics.Tracing` richtlijn boven aan het bestand met de `using Microsoft.Diagnostics.Tracing` richtlijn.

Deze stappen meer niet nodig zodra de **.NET Framework 4.6** wordt ondersteund door Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch luisteraar ervan af Activeringsformulier- en configuratieservices
De laatste stap voor het verzenden van diagnostische gegevens naar Elasticsearch is het opzetten van een exemplaar van `ElasticSearchListener` en configureren met de Elasticsearch verbindingsgegevens. De luisteraar ervan af wordt automatisch vastgelegd voor alle gebeurtenissen verheven via EventSource klassen die zijn gedefinieerd in het project service. Dit moet leven gedurende de levensduur van de service, zodat de beste plaats om deze te maken in de servicecode initialisatie is. Hier ziet u hoe de initialisatiecode voor een stateless service kan uitzien na de noodzakelijke wijzigingen (toevoegingen duidelijk in opmerkingen beginnen met `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch verbindingsgegevens moeten worden geplaatst in een aparte sectie in het configuratiebestand service (**PackageRoot\Config\Settings.xml**). De naam van de sectie moet overeenkomen met de waarde doorgegeven aan de `FabricConfigurationProvider` constructor, bijvoorbeeld:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
De waarden van `serviceUri`, `userName` en `password` parameters komen overeen met de Elasticsearch cluster eindpuntadres, Elasticsearch-gebruikersnaam en wachtwoord, respectievelijk. `indexNamePrefix`is het voorvoegsel voor Elasticsearch indexen; de bibliotheek Microsoft.Diagnostics.Listeners maakt dagelijks u een nieuwe index voor de gegevens.

### <a name="verification"></a>Verificatie
Dat is alles. Nu wanneer de service wordt uitgevoerd, begint sporen verzenden naar de Elasticsearch-service dat is opgegeven in de configuratie. U kunt controleren of dit door de Kibana UI te openen dat is gekoppeld aan het exemplaar van de Elasticsearch doel. In ons voorbeeld is het adres van de pagina http://myBigCluster.westus.cloudapp.azure.com/. Controleer of met de naamvoorvoegsel gekozen indexeert voor de `ElasticSearchListener` exemplaar daadwerkelijk zijn gemaakt en gevuld met gegevens.

![Kibana waarin PartyCluster toepassingsgebeurtenissen][2]

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het opsporen en een Service stof-service controleren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
