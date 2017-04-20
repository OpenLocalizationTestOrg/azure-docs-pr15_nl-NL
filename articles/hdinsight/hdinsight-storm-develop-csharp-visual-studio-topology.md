<properties
   pageTitle="Apache Storm topologieën met Visual Studio en C# | Microsoft Azure"
   description="Informatie over het maken van Storm topologieën in C# met het maken van de topologie van een eenvoudige word tellen in Visual Studio met behulp van de hulpmiddelen HDInsight voor Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Ontwikkel C# topologieën voor Apache Storm op HDInsight met Hadoop tools voor Visual Studio

Informatie over het maken van de topologie van een C# Storm met behulp van de hulpmiddelen HDInsight voor Visual Studio. Deze zelfstudie worden doorlopen het proces van het maken van een nieuw Storm project in Visual Studio, lokaal testen en het implementeren naar een Storm Apache op HDInsight cluster.

Ook leert u hoe hybride topologieën met C# en Java-onderdelen maakt.

> [AZURE.IMPORTANT] Terwijl de stappen in dit document, is afhankelijk van een Windows-ontwikkelomgeving met Visual Studio, kan het gecompileerd project worden verzonden naar een Linux- of Windows gebaseerde HDInsight cluster. Alleen Linux gebaseerde clusters gemaakt na 10/28/2016 ondersteuning SCP.NET topologieën.
>
> Als u wilt een C#-topologie met een cluster Linux gebaseerde gebruikt, moet u het pakket Microsoft.SCP.Net.SDK NuGet gebruikt door uw project naar versie 0.10.0.6 of hoger bijwerken. De versie van het pakket moet ook overeenkomen met de primaire versie van Storm geïnstalleerd op HDInsight. Bijvoorbeeld Storm voor HDInsight versies 3.3 en 3.4 Storm versie gebruiken 0.10.x, terwijl de HDInsight 3.5 met Storm 1.0.x.
> 
> C# topologieën op Linux gebaseerde clusters moeten .NET 4.5 gebruik en zwart gebruiken om uit te voeren op het cluster HDInsight. De meeste dingen werken, echter checkt u het document [Zwart compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/) mogelijke compatibiliteitsproblemen.

## <a name="prerequisites"></a>Vereisten voor

- Een van de volgende versies van Visual Studio

    - Visual Studio 2012 met [4 bijwerken](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 met [4 bijwerken](http://www.microsoft.com/download/details.aspx?id=44921) of [Visual Studio-2013-Community](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio-2015 of [Visual Studio 2015-Community](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 of hoger

- HDInsight Tools voor Visual Studio: Zie [aan de slag met HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) installeren en configureren van de hulpmiddelen HDInsight voor Visual Studio.

    > [AZURE.NOTE] HDInsight Tools voor Visual Studio worden niet ondersteund op Visual Studio Express

-   Apache Storm op HDInsight cluster: Zie [aan de slag met Apache Storm op HDInsight](hdinsight-apache-storm-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een cluster.

## <a name="templates"></a>Sjablonen

De hulpmiddelen HDInsight voor Visual Studio bieden de volgende sjablonen::

| Projecttype | Laat zien |
| ------------ | ------------- |
| Storm-toepassing | Een leeg Storm topologie project |
| Storm Azure SQL-schrijver steekproef | Schrijven met Azure SQL-Database |
| Voorbeeld van storm DocumentDB Reader | Lezen van Azure DocumentDB |
| Storm DocumentDB schrijver steekproef | Hoe ernaar te schrijven Azure DocumentDB |
| Voorbeeld van storm EventHub Reader | Lezen van Azure gebeurtenis Hubs |
| Storm EventHub schrijver steekproef | Schrijven met Azure gebeurtenis Hubs |
| Voorbeeld van storm HBase Reader | Lezen van HBase op HDInsight clusters |
| Storm HBase schrijver steekproef | Hoe u het schrijven naar HBase op HDInsight clusters |
| Voorbeeld van de hybride storm | Het gebruik van een Java-component |
| Voorbeeld van storm | De topologie van een eenvoudige word tellen |

> [AZURE.NOTE] De HBase reader en schrijver voorbeelden gebruiken de HBase REST API om te communiceren met een HBase op HDInsight cluster, niet de HBase Java-API.

In de stappen in dit document, kunt u het eenvoudige Storm project toepassingstype wilt gebruiken voor het maken van een nieuwe topologie.

## <a name="create-a-c-topology"></a>Een C#-topologie maken

1. Als u de nieuwste versie van de hulpmiddelen HDInsight hebt niet voor Visual Studio hebt geïnstalleerd, raadpleegt u [aan de slag met HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Visual Studio openen, selecteert u **bestand** > **Nieuw**en klik vervolgens op **Project**.

3. Vouw in het scherm **Nieuw Project** **geïnstalleerde** > **sjablonen**en selecteer **HDInsight**. Selecteer in de lijst met sjablonen, **Storm-toepassing**. In de linkerbenedenhoek van het scherm, geeft u **WordCount** als de naam van de toepassing.

    ![afbeelding](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Nadat u het project hebt gemaakt, kunt u de volgende bestanden nodig hebt:

    - **Program.cs**: Hiermee definieert u de topologie voor uw project. Houd er rekening mee dat een standaard-topologie die uit één spout en één bout bestaat al dan niet standaard wordt gemaakt.

    - **Spout.cs**: een voorbeeld spout die Hiermee plaatst u willekeurige getallen.

    - **Bolt.cs**: een voorbeeld bout die een telling van getallen dat door de spout bewaart.

    Als onderdeel van het maken van projecten, wordt de meest recente [SCP.NET-pakketten](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) van NuGet worden gedownload.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

In de volgende secties wijzigt u dit project in een eenvoudige WordCount-toepassing.

### <a name="implement-the-spout"></a>De spout implementeren

1. Open **Spout.cs**. Spouts worden gebruikt voor het lezen van gegevens in een topologie van een externe bron. De belangrijkste onderdelen voor een spout zijn:

    - **NextTuple**: stormenderhand wordt aangeroepen wanneer de spout uitzenden van nieuwe tupels is toegestaan.

    - **ACK** (alleen transacties topologie): bevestigingen gestart door andere onderdelen in de topologie voor tupels verzonden vanuit deze spout verwerkt. Een tupel zijn bevestigd, kunt de spout weten dat deze is verwerkt door de volgende onderdelen.

    - **Mislukt** (alleen transacties topologie): omgaat met tupels die zijn fail-verwerking andere onderdelen in de topologie. Hier vindt u de mogelijkheid om het opnieuw verzenden van de tupel zodat deze opnieuw kan worden verwerkt.

2. De inhoud van de klasse **Spout** vervangen door het volgende. Hiermee maakt u een spout die willekeurig een zin in de topologie genereert.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Neem even de tijd om door de opmerkingen voor meer informatie over de werking van deze code te lezen.

### <a name="implement-the-bolts"></a>De bouten implementeren

1. Verwijder de bestaande **Bolt.cs** -bestand uit het project.

2. Klik in **Solution Explorer**met de rechtermuisknop op het project en selecteer **toevoegen** > **Nieuw item**. In de lijst Selecteer **Storm bout**en **Splitter.cs** invoeren als de naam. Herhaal dit als u wilt maken van een tweede bolt benoemde **Counter.cs**.

    - **Splitter.cs**: een bout die wordt gesplitst zinnen in afzonderlijke woorden en genereert een nieuwe stroom van woorden implementeert.

    - **Counter.cs**: een bout dat elk woord telt en genereert een nieuwe stroom woorden en het aantal voor elk woord dat wordt geïmplementeerd.

    > [AZURE.NOTE] Deze bouten gewoon lezen en schrijven naar stromen, maar u kunt ook een bout gebruiken om te communiceren met bronnen, zoals een database of de service.

3. Open **Splitter.cs**. Opmerking dat er slechts één methode al dan niet standaard: **uitvoeren**. Dit heet terwijl de bout een tupel voor verwerking ontvangt. Hier vindt u verwerken tupels binnenkomende en uitgaande tupels verzenden.

4. De inhoud van de klas **splitser** vervangen door de volgende code:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Neem even de tijd om door de opmerkingen voor meer informatie over de werking van deze code te lezen.

5. Open **Counter.cs** en de inhoud class vervangen door het volgende:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Neem even de tijd om door de opmerkingen voor meer informatie over de werking van deze code te lezen.

### <a name="define-the-topology"></a>De topologie definiëren

Spouts en bouten worden in een grafiek, die wordt gedefinieerd hoe de gegevens stroomt onderdelen gerangschikt. Voor deze topologie is in de grafiek als volgt uit:

![afbeelding van de manier waarop onderdelen zijn gerangschikt](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Zinnen zijn geeft de spout, die zijn verdeeld over exemplaren van de splitser bout. De splitser bout opgesplitst de zinnen in woorden, die zijn verdeeld over de teller bout.

Omdat het aantal woorden is die lokaal zijn opgeslagen in het exemplaar van de teller, horen we om ervoor te zorgen dat specifieke woorden flow op dezelfde teller bout instantie, zodat er slechts één exemplaar van het bijhouden van een specifiek woord. Maar voor de bout splitser het echt maakt niet uit welke bout ontvangt welke zin, zodat we gewoon wilt laden balance '-zinnen over die exemplaren.

Open **Program.cs**. De belangrijke methode is **GetTopologyBuilder**, die wordt gebruikt om de zoektopologie die is verzonden naar Storm definiëren. De inhoud van **GetTopologyBuilder** vervangen door de volgende code willen implementeren van de topologie die eerder is beschreven:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Neem even de tijd om door de opmerkingen voor meer informatie over de werking van deze code te lezen.

## <a name="submit-the-topology"></a>De topologie indienen

1. In **Solution Explorer**met de rechtermuisknop op het project en selecteer de optie **verzenden naar Storm op HDInsight**.

    > [AZURE.NOTE] Als u wordt gevraagd, voert u de aanmeldingsreferenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de database met uw Storm op HDInsight cluster.

2. Selecteer uw Storm op HDInsight cluster in de vervolgkeuzelijst **Storm Cluster** en selecteer vervolgens **verzenden**. U kunt controleren als de indiening geslaagd is met behulp van **het uitvoervenster** .

3. Wanneer de topologie is verzonden, wordt de **Storm topologieën** voor het cluster moet worden weergegeven. Selecteer de topologie **WordCount** in de lijst om informatie over de actieve topologie te bekijken.

    > [AZURE.NOTE] U kunt ook **Storm topologieën** van **Server Explorer**weergeven: uitvouwen **Azure** > **HDInsight**, met de rechtermuisknop op een Storm op HDInsight cluster en selecteer vervolgens **Weergave Storm topologieën**.

    Gebruik de koppelingen voor de spouts of bouten informatie over deze onderdelen kunnen bekijken. Een nieuw venster wordt geopend voor elk item is geselecteerd.

4. Klik op **verwijderen** als u wilt stoppen de topologie in de weergave **Overzicht van de topologie** .

    > [AZURE.NOTE] Storm topologieën blijven uitgevoerd totdat ze zijn geactiveerd of het cluster wordt verwijderd.

## <a name="transactional-topology"></a>Transacties topologie

De vorige topologie is niet. De onderdelen binnen de topologie Implementeer een functionaliteit voor het afspelen van berichten als verwerking door een onderdeel in de topologie mislukt. Voor een transacties topologie, een nieuw project maken en selecteer **Storm steekproef** als het projecttype.

Transacties topologieën implementeren de volgende handelingen uit om te ondersteunen opnieuw afspelen van gegevens:

- **Caching van metagegevens**: de spout moet worden opgeslagen in de metagegevens over de gegevens die zijn gegenereerd zodat de gegevens kunnen worden opgehaald en opnieuw wordt gegenereerd als er een fout optreedt. Omdat de gegevens die is verstrekt door de steekproef klein is, wordt de onbewerkte gegevens voor elke tupel wordt opgeslagen in een woordenlijst voor opnieuw afspelen.

- **ACK**: elke bout in de topologie kunt bellen `this.ctx.Ack(tuple)` naar ack dat er is een tupel verwerkt. Wanneer alle bouten bevestigd de tupel hebt, de `Ack` methode van de spout is geactiveerd. Hierdoor wordt de spout verwijderen in de cache opgeslagen gegevens voor opnieuw afspelen omdat de gegevens volledig is verwerkt.

- **Mislukt**: elke bout kunt bellen `this.ctx.Fail(tuple)` om aan te geven dat de verwerking voor een tupel is mislukt. De fout is doorgevoerd in de `Fail` methode van de spout, waar de tupel kan worden cookies met behulp van metagegevens: in cache opgeslagen.

- **Volgorde-ID**: bij het genereren van een tupel, een reeks-ID kan worden opgegeven. Dit moet een waarde die aangeeft van de tupel voor de verwerking van opnieuw afspelen (Ack en Fail). De spout in het project **Storm voorbeeld** gebruikt, bijvoorbeeld de volgende handelingen uit bij het genereren van gegevens:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Dit Hiermee plaatst u een nieuwe tupel met een zin die u wilt de standaardstream, met de waarde sequentie-ID in **lastSeqId**. In dit voorbeeld wordt de **lastSeqId** gewoon verhoogd voor elke tupel gegenereerd.

Zoals u in het project **Storm voorbeeld** , kan of een onderdeel is transacties worden gesteld tijdens de uitvoering op basis van de configuratie.

## <a name="hybrid-topology"></a>Hybride topologie

HDInsight tools voor Visual Studio kunnen ook worden gebruikt om te maken van hybride topologieën, waarbij bepaalde onderdelen zijn C# en anderen Java zijn.

Klik voor een hybride topologie, een nieuw project maken en selecteer **Storm hybride steekproef**. Hiermee maakt u een volledig opmerkingen steekproef met verschillende topologieën die het volgende illustreren:

- **Java spout** en **C# bout**: gedefinieerd in **HybridTopology_javaSpout_csharpBolt**

    - Een transacties versie is gedefinieerd in **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** en **Java bout**: gedefinieerd in **HybridTopology_csharpSpout_javaBolt**

    - Een transacties versie is gedefinieerd in **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] In deze versie is ook laat zien hoe Clojure code uit een tekstbestand als een Java-component gebruiken.

Als u wilt schakelen tussen de zoektopologie die worden gebruikt wanneer u het project is verzonden, verplaatst de `[Active(true)]` instructie aan de topologie die u gebruiken wilt voordat u deze aan het cluster verzendt.

> [AZURE.NOTE] Alle Java-bestanden die vereist zijn worden als onderdeel van dit project in de map **JavaDependency** gegeven.

Houd rekening met het volgende bij het maken en verzenden van de topologie van een hybride:

- **JavaComponentConstructor** moet worden gebruikt voor het maken van een nieuw exemplaar van de Java-klasse voor een spout of bout.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** moet worden gebruikt om gegevens in of afmelden bij Java-onderdelen van Java-objecten naar JSON serialiseren.

- Bij het verzenden van de topologie op de server, moet u de optie **Extra configuraties** gebruiken om op te geven van de **Java bestandspaden**. Het opgegeven pad moet de map waarin de oppervlak-bestanden die uw Java-klassen bevatten.

### <a name="azure-event-hubs"></a>Azure gebeurtenis Hubs

SCP.Net versie 0.9.4.203 maakt u kennis met een nieuwe klasse en de methode specifiek voor het werken met de gebeurtenis Hub Spout (een Java-spout die van gebeurtenis Hub wordt gelezen.) Wanneer u een zoektopologie die gebruikmaakt van deze spout maakt, moet u de volgende methoden gebruiken:

- **EventHubSpoutConfig** class: maakt een object dat de configuratie voor het onderdeel spout bevat

- **TopologyBuilder.SetEventHubSpout** methode: wordt het onderdeel gebeurtenis Hub Spout toegevoegd aan de topologie

> [AZURE.NOTE] Terwijl deze eenvoudiger kunt werken met de gebeurtenis Hub Spout dan andere Java-onderdelen, moet u nog steeds de CustomizedInteropJSONSerializer serialiseren gegevens die zijn gemaakt met de spout gebruiken.

## <a id="configurationmanager"></a>ConfigurationManager gebruiken

Niet ConfigurationManager gebruikt voor waarden van de systeemconfiguratie ophalen uit bout en spout onderdelen; Dit leidt tot een uitzondering null-aanwijzer. In plaats daarvan is de configuratie voor uw project doorgegeven aan de topologie Storm als de combinatie van een sleutel/waarde in de context van de topologie. Elk onderdeel dat is afhankelijk van de configuratiewaarden moet ze ophalen van de context tijdens initialisatie.

De volgende code ziet het ophalen van deze waarden:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Als u een `Get` methode om te retourneren van een exemplaar van uw component, moet u ervoor zorgen dat dit wordt doorgegeven beide de `Context` en `Dictionary<string, Object>` parameters voor de constructor. Het volgende voorbeeld is een basic `Get` methode die deze waarden correct worden doorgegeven:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Het bijwerken van SCP.NET

Recente versies van SCP.NET ondersteuning pakket upgrade tot en met NuGet. Wanneer u een nieuwe update beschikbaar is, ontvangt u een melding voor de upgrade. Als u wilt handmatig controleren voor een upgrade, moet u deze stappen uitvoeren:

1. In **Solution Explorer**met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren**.

2. Selecteer de **Updates**van de manager pakket. Als een update beschikbaar is, kunt u deze wordt weergegeven. Klik op de knop **bijwerken** voor het pakket om het te installeren.

> [AZURE.IMPORTANT] Als uw project is gemaakt met een van de eerdere versie van SCP.NET die NuGet voor pakket updates niet hebt gebruikt, moet u de volgende stappen uit om te werken naar de nieuwe versie uitvoeren:
>
> 1. In **Solution Explorer**met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren**.
> 2. Gebruik **het zoekveld** , zoeken en vervolgens toevoegt, **Microsoft.SCP.Net.SDK** aan het project.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="null-pointer-exceptions"></a>Null-aanwijzer uitzonderingen

Wanneer u een C#-topologie met een cluster Linux gebaseerde HDInsight, bout en spout van onderdelen die ConfigurationManager gebruiken om te lezen configuratie-instellingen gedurende runtime null aanwijzer uitzonderingen kunnen retourneren. Dit gebeurt omdat de configuratie voor het domein dat geladen, is niet van de vergadering die uw project bevat.

De configuratie voor uw project in de topologie Storm wordt doorgegeven als de combinatie van een sleutel/waarde in de context van de topologie en kan worden opgehaald uit de woordenlijstobject dat is doorgegeven aan uw onderdelen wanneer ze worden geïnitialiseerd.

Het volgende voorbeeld bevat de configuratiewaarden van de context van de topologie laden, raadpleegt u de sectie [ConfigurationManager](#configurationmanager) van dit document.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Wanneer u een C#-topologie met een cluster Linux gebaseerde HDInsight, kunt u de volgende fout kan optreden:

    System.TypeLoadException: Failure has occurred while loading a type.

Dit meestal occurrs wanneer u een binair getal gebruikt die niet compatibel is met de versie van .NET die ondersteuning biedt voor zwart.

Voor Linux gebaseerde HDInsight kolomgroepen moet u ervoor zorgen dat uw project binaire bestanden gecompileerd .NET 4.5 gebruikt.


### <a name="test-a-topology-locally"></a>Een topologie lokaal testen

Hoewel kunt u eenvoudig een topologie implementeren naar een cluster in sommige gevallen, moet u mogelijk een topologie lokaal testen. Gebruik de volgende stappen uitvoeren en testen van de topologie voorbeeld in deze zelfstudie lokaal in uw ontwikkelomgeving.

> [AZURE.WARNING] Lokale testen werkt alleen voor eenvoudige, C# alleen topologieën. Gebruik niet lokale testen voor hybride topologieën of topologieën met meerdere streams, zoals u fouten ontvangt.

1. Klik in **Solution Explorer**met de rechtermuisknop op het project en selecteer **Eigenschappen**. In de Projecteigenschappen van het door het **type uitvoer** naar **Console-toepassing**te wijzigen.

    ![Uitvoertype](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Onthoud dat het **type uitvoer** terug naar **Klassenbibliotheek** wijzigen voordat u de topologie dashboard naar een cluster implementeren.

2. Klik in **Solution Explorer**met de rechtermuisknop op het project, en selecteer vervolgens **toevoegen** > **Nieuw Item**. Selecteer **Class** en **LocalTest.cs** invoeren als de klassenaam. Klik tot slot op **toevoegen**.

3. Open **LocalTest.cs** en de volgende instructie met **behulp van** toevoegen aan de bovenkant:

        using Microsoft.SCP;

4. Gebruik de volgende handelingen uit als de inhoud van de klas **LocalTest** :

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Neem even de tijd om door de codeopmerkingen te lezen. Deze code **LocalContext** gebruikt voor het uitvoeren van de onderdelen in de ontwikkelomgeving en blijft de gegevensstroom tussen onderdelen naar tekstbestanden op het lokale station bestaan.

5. Open **Program.cs** en de volgende manieren te werk toevoegen aan de methode **Main** :

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Sla de wijzigingen en klikt u op **F5** of selecteer **fouten opsporen in** > **Foutopsporing starten** om het project te starten. Een consolevenster moet worden weergegeven, en meld u status als de voortgang van de tests. Wanneer u **klaar bent met het Tests** wordt weergegeven, drukt u op een toets om het venster te sluiten.

7. Zoek de map waarin uw project, bijvoorbeeld via **Windows Verkenner** **C:\Users\<gebruikersnaam > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. In deze map, open **opslaglocatie**en klik vervolgens op **fouten opsporen in**. Ziet u de tekstbestanden die zijn gemaakt wanneer de tests hebt uitgevoerd: sentences.txt, counter.txt en splitter.txt. Elk tekstbestand openen en de gegevens controleren.

    > [AZURE.NOTE] Tekenreeksgegevens blijft behouden als een matrix van decimale waarden in deze bestanden. Bijvoorbeeld \[[97,103,111]] in de **splitter.txt** het bestand is van het woord 'en'.

Hoewel een eenvoudige word testen tellen is toepassing lokaal heel eenvoudig; de reële waarde wordt opgehaald wanneer er een complexe topologie die communiceert met externe gegevensbronnen of complexe gegevensanalyses uitvoeren. Wanneer u bijvoorbeeld een project werkt, moet u mogelijk in uw onderdelen te isoleren problemen onderbrekingspunten en stappen van de code instellen.

> [AZURE.NOTE] Zorg ervoor dat het **projecttype** in te stellen op **Klassenbibliotheek** voordat u de implementatie in een Storm op HDInsight cluster.

### <a name="log-information"></a>Logboekgegevens

U kunt eenvoudig gegevens zich bij uw zoektopologieonderdelen met behulp van `Context.Logger`. Bijvoorbeeld het volgende wordt informatie log fragmenten maken:

    Context.Logger.Info("Component started");

Geregistreerde gegevens kan worden bekeken vanuit het **Logboek Hadoop-Service**, dat in **Server Explorer**is gevonden. Vouw het fragment voor uw Storm op HDInsight cluster uit en vouw vervolgens **Logboek Hadoop-Service**. Selecteer ten slotte het logboekbestand om weer te geven.

> [AZURE.NOTE] De logboeken worden opgeslagen in de opslag van Azure-account dat wordt gebruikt door uw cluster. Als u een ander abonnement dan de tabel die u bent aangemeld bij met Visual Studio, moet u aanmelden bij het abonnement dat met het account opslag om deze informatie weer te geven.

### <a name="view-error-information"></a>Fout-informatie weergeven

Om fouten te bekijken die zijn opgetreden in een actieve topologie, gebruikt u de volgende stappen:

1. Vanuit **Server Explorer**, met de rechtermuisknop op de Storm op HDInsight cluster en selecteer **weergave Storm topologieën**.

2. De **Laatste fout** kolom heeft voor de **Spout** - en **Bolts**, informatie over de laatste fout opgetreden.

3. Selecteer de **Spout-Id** of **Bout-Id** voor de component met een fout weergegeven. Op de detailpagina dat wordt weergegeven, wordt de extra informatie over fouten worden vermeld in de sectie **fouten** onder aan de pagina.

4. Als u meer informatie, selecteert u een **poort** in de sectie **Executors** van de pagina om het logboek met Storm werknemer voor de laatste minuten weer te geven.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u ontwikkelen en implementeren van Storm topologieën vanuit HDInsight tools voor Visual Studio wordt uitgelegd hoe u om [gebeurtenissen](hdinsight-storm-develop-csharp-event-hub-topology.md)te verwerken van Azure gebeurtenis Hub met Storm op HDInsight.

Zie voor een voorbeeld van een C#-topologie die stream gegevens worden gesplitst in meerdere streams, [C# Storm voorbeeld](https://github.com/Blackmist/csharp-storm-example).

Als u wilt ontdekken meer informatie over het maken van C# topologieën, gaat u naar [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Zie de volgende onderwerpen voor meer manieren om te werken met HDInsight en meer Storm op HDinsight voorbeelden:

**Microsoft SCP.NET**

* [SCP programming handleiding](hdinsight-storm-scp-programming-guide.md)

**Apache Storm op HDInsight**

- [Implementeren en topologieën met Apache Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology.md)

- [Voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop op HDInsight**

- [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

- [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

- [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase op HDInsight**

- [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started.md)
