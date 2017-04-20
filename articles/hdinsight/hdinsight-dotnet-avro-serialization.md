<properties
    pageTitle="Gegevens met de Microsoft Avro Library serialiseren | Microsoft Azure"
    description="Informatie over het gebruik van Avro groot gegevens serialiseren in Azure HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Gegevens in Hadoop met het Microsoft Avro Library serialiseren

In dit onderwerp ziet u het gebruik van de <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro bibliotheek</a> serialiseren van objecten en andere gegevensstructuren in streams om te kunnen ze geheugen, een database of een bestand te behouden en ook hoe u ze als u wilt herstellen van de oorspronkelijke objecten terugconverteren.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
De <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro bibliotheek</a> implementeert het systeem Apache Avro gegevens serialisatie voor de Microsoft.NET-omgeving. Apache Avro biedt een compacte binaire gegevens interchange-indeling voor serialisatie. <a href="http://www.json.org" target="_blank">JSON</a> wordt gebruikt om een schema taal-agnostic die taal interoperabiliteit schadequoten te definiëren. Gegevens in één taal serienummer kunnen worden gelezen in een andere. C, C++, C#, Java, PHP, Python en Ruby worden momenteel ondersteund. In de <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Specificatie van Apache Avro</a>vindt u gedetailleerde informatie over de notatie. Houd er rekening mee dat de huidige versie van het Microsoft Avro Library het deel van de RPC oproepen (RPC's) in deze specificatie niet ondersteunt.

De serienummer weergave van een object in het systeem Avro bestaat uit twee delen: schema en een werkelijke waarde. Het schema Avro beschrijving van het gegevensmodel van taal-onafhankelijke serienummer gegevens met JSON. Is het presenteren-elkaar met een binaire weergave van gegevens. Met het schema los van de binaire weergave toestemming geeft dat elk object met geen algemene per-waarde, waardoor u serialisatie snel en de weergave kleine worden geschreven.

##<a name="the-hadoop-scenario"></a>Het Hadoop-scenario
De Apache Avro serialisatie-indeling wordt veel gebruikt in Azure HDInsight en andere Apache Hadoop-omgevingen. Avro is een handige manier om aan te geven van complexe gegevensstructuren binnen een project Hadoop MapReduce. De opmaak van Avro-bestanden (Avro object container-bestand) is ontworpen ter ondersteuning van het verdeelde MapReduce programming model. De spil waarmee de verdeling is dat de bestanden "splittable' in de zin dat een kunt een willekeurige plaats in een bestand te zoeken en lezen vanuit een bepaald tekstblok starten.

##<a name="serialization-in-avro-library"></a>Serialisatie in Avro-bibliotheek
De bibliotheek .NET voor Avro ondersteunt serialiseren objecten op twee manieren:

- **weerspiegeling** - de JSON-schema voor de typen wordt automatisch samengesteld uit de gegevens contract kenmerken van de typen .NET moet worden toegepast.
- **algemene record** - A JSON schema expliciet is opgegeven in een record dat wordt aangeduid door de klasse [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) wanneer geen .NET-typen zijn om te beschrijven van het schema voor de gegevens worden presenteren.

Wanneer het gegevensschema naar de schrijver en de lezer van de stream bekend is, kunnen de gegevens zonder een schema worden verzonden. Wanneer een Avro object container-bestand wordt gebruikt, wordt het schema in gevallen opgeslagen in het bestand. Andere parameters, zoals de codec voor gegevenscompressie, kunnen worden opgegeven. Deze scenario's zijn uit die in meer detail beschreven en wordt weergegeven in de onderstaande codevoorbeelden.


## <a name="install-avro-library"></a>Avro bibliotheek installeren

Hier volgen vereist voordat u de bibliotheek installeren:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 of later)

Houd er rekening mee dat de afhankelijkheid Newtonsoft.Json.dll wordt automatisch gedownload met de installatie van de Microsoft Avro Library. De procedure hiervoor is opgegeven in de volgende sectie.


De Microsoft Avro Library wordt gedistribueerd als een NuGet-pakket dat vanuit Visual Studio kan worden geïnstalleerd via de volgende procedure:

1. Selecteer het tabblad **Project** -> **Beheren NuGet-pakketten...**
2. Zoeken naar 'Microsoft.Hadoop.Avro' in het vak **Zoeken op Online** .
3. Klik op de knop **installeren** naast **Microsoft Azure HDInsight Avro bibliotheek**.

Houd er rekening mee dat het Newtonsoft.Json.dll (> = 6.0.4) afhankelijkheid wordt ook automatisch gedownload met het Microsoft Avro Library.

U kunt de <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft-Avro bibliotheekpagina start</a> als u wilt lezen van de huidige releaseopmerkingen te bezoeken.


De Microsoft Avro Library-broncode is beschikbaar op de <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">startpagina van Microsoft Avro bibliotheek</a>.

##<a name="compile-schemas-using-avro-library"></a>Compilatie van schema's met behulp van Avro-bibliotheek

De Microsoft Avro Library bevat een hulpprogramma voor het genereren van code waarmee C#-typen automatisch op basis van de eerder gedefinieerde JSON-schema maken. De code generatie utility niet als een binaire uitvoerbaar is verdeeld, maar kan eenvoudig worden opgebouwd via de volgende procedure:

1. Download het ZIP-bestand met de meest recente versie van HDInsight SDK broncode vanuit <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK voor Hadoop</a>. (Klik op het pictogram **downloaden** , niet het tabblad **Downloads** .)

2. Pak de SDK HDInsight naar een map op de computer met .NET Framework 4 hebt geïnstalleerd en verbinding met Internet voor het downloaden van nodig afhankelijkheid NuGet-pakketten. Hieronder wordt ervan uitgegaan dat de broncode naar C:\SDK is opgehaald.

3. Ga naar de map C:\SDK\src\Microsoft.Hadoop.Avro.Tools en build.bat uitvoeren. (Het bestand wordt uit de 32-bits-verdeling van .NET Framework MSBuild belt. Als u wilt de 64-bits versie wilt gebruiken, bewerk build.bat, de opmerkingen in het bestand te volgen.) Zorg ervoor dat de build geslaagd is. (Op sommige systemen MSBuild kan leiden tot waarschuwingen. Deze waarschuwingen hebben geen invloed op het hulpprogramma zo lang maken als er geen opbouwfouten.)

4. Het hulpprogramma gecompileerd bevindt zich in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Als u vertrouwd met de syntaxis van de opdrachtregel, voer de volgende opdracht uit de map waarin het code generatie hulpprogramma zich bevindt:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Als u wilt testen het hulpprogramma, kunt u C#-klassen genereren van de steekproef JSON-schemabestand de broncode voorzien. Voer de volgende opdracht uit:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Dit moet om twee C#-bestanden in de huidige map te maken: SensorData.cs en Location.cs.

Als u wilt weten over de logica waarin de code generatie utility wordt gebruikt bij het converteren van het schema JSON naar C#-typen, raadpleegt u het bestand GenerationVerification.feature in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Houd er rekening mee dat naamruimten worden opgehaald uit het schema JSON, met de logica uit het bestand dat wordt vermeld in de vorige alinea. Naamruimten opgehaald uit het schema van de voorrang op wat is opgegeven met de parameter /n in de opdrachtregel hulpprogramma. Als u overschrijven de naamruimten opgenomen in het schema wilt, gebruikt u de parameter /nf. Bijvoorbeeld als alle naamruimten uit de SampleJSONSchema.avsc wilt my.own.nspace wijzigen, voert u de volgende opdracht uit:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Voorbeelden
Zes voorbeelden die beschikbaar zijn in dit onderwerp ziet u verschillende scenario's die worden ondersteund door de Microsoft Avro Library. De Microsoft Avro Library is ontworpen voor gebruik met een stream. In deze voorbeelden wordt de gegevens via geheugen streams in plaats van bestand streams of databases voor eenvoudig en consistentie gemanipuleerd. De methode die u hebt gemaakt in een productieomgeving, is afhankelijk van de exacte scenario vereisten, gegevensbron en volume, prestaties beperkingen en andere factoren.

De eerste twee voorbeelden wordt getoond hoe u serialiseren en terugconverteren van gegevens in het geheugen stream buffers met behulp van weerspiegeling en algemene records. Het schema in beide gevallen wordt uitgegaan van worden gedeeld tussen de lezers en schrijvers out-van-band.

De derde en vierde voorbeelden wordt het serialiseren en terugconverteren van gegevens met behulp van de bestanden Avro object container. Wanneer u gegevens in een Avro container-bestand is opgeslagen, wordt altijd het schema met deze opgeslagen omdat het schema voor deserialisatie moet worden gedeeld.

Monster, de eerste vier voorbeelden die kan worden gedownload van de <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">voorbeelden van Azure code</a> -site.

Het vijfde voorbeeld wordt getoond hoe u het gebruik van een aangepaste compressie-codec voor Avro object containerbestanden. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload van de <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">voorbeelden van Azure code</a> -site.

Het zesde voorbeeld ziet hoe u met Avro serialisatie gegevens uploaden naar Azure-blobopslag en deze vervolgens analyseren met behulp van component met een cluster HDInsight (Hadoop). Worden kan gedownload van de <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">voorbeelden van Azure code</a> -site.

Hier vindt koppelingen naar de zes steekproeven besproken in het onderwerp:

 * <a href="#Scenario1">**Serialisatie met reflectie**</a> - de JSON-schema voor typen worden wordt automatisch samengesteld uit de gegevens contract kenmerken.
 * <a href="#Scenario2">**Serialisatie met algemene record**</a> - de JSON schema is expliciet is opgegeven in een record als er geen .NET-type beschikbaar voor weerspiegeling is.
 * <a href="#Scenario3">**Serialisatie object containerbestanden met weerspiegeling gebruiken**</a> - de JSON schema is automatisch gemaakt en gedeeld samen met de serienummer gegevens via een Avro object container-bestand.
 * <a href="#Scenario4">**Serialisatie object containerbestanden met algemene record gebruiken**</a> - de JSON schema is expliciet opgegeven voordat de serialisatie en gedeeld samen met de gegevens via een Avro object container-bestand.
 * <a href="#Scenario5">**Serialisatie met object containerbestanden met een aangepaste compressie-codec**</a> - met het voorbeeld ziet hoe u een Avro object container-bestand maken met een aangepaste .NET-implementatie van de Deflate gegevens compressiecodec.
 * <a href="#Scenario6">**Avro gebruiken voor het uploaden van gegevens voor de service Microsoft Azure HDInsight**</a> - met het voorbeeld ziet u hoe Avro serialisatie moet samenwerken met de service HDInsight. Een actieve Azure-abonnement en toegang tot een cluster Azure HDInsight zijn vereist om uit te voeren in dit voorbeeld.

###<a name="Scenario1"></a>Voorbeeld 1: Serialisatie met weerspiegeling

De JSON-schema voor de typen kan door worden automatisch opgebouwd Microsoft Avro Library via weerspiegeling van de gegevens contract kenmerken van de objecten C# moet worden toegepast. Hiermee maakt u de Microsoft Avro Library een [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) om aan te geven van de velden moet worden toegepast.

In dit voorbeeld objecten (een **SensorData** klasse met een lid **locatie** structuur) worden omgezet naar een geheugenstroom en deze stroom is beurtelings gedeserialiseerd. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar om te bevestigen dat het object **SensorData** is hersteld, gelijk aan de oorspronkelijke is.

Het schema in dit voorbeeld wordt uitgegaan van moet worden gedeeld tussen de lezers en schrijvers, zodat de Avro object container-indeling niet vereist is. Zie voor een voorbeeld van het serialiseren en terugconverteren van gegevens in het geheugenbuffers met behulp van weerspiegeling met de indeling van de container object wanneer het schema moet worden gedeeld met de gegevens, <a href="#Scenario3">serialisatie object containerbestanden met weerspiegeling gebruiken</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Voorbeeld 2: Serialisatie met een algemene record

Een schema JSON kan expliciet worden opgegeven in een algemene record wanneer weerspiegeling kan niet worden gebruikt, omdat de gegevens via .NET klassen met een gegevenscontract kan niet worden weergegeven. Deze methode is gewoonlijk langzamer dan met behulp van reflectie. In dat geval het schema voor de gegevens ook dynamische, dat wil zeggen worden kunnen niet bekend bij compilatie. Gegevens die worden weergegeven als door komma's gescheiden waarden (CSV) bestanden waarvan het schema onbekend is totdat deze wordt omgezet naar de indeling Avro tijdens runtime is een voorbeeld van dit type dynamische scenario.

In dit voorbeeld ziet u het maken en gebruiken van een [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) expliciet opgeven een schema JSON, hoe u deze in te vullen met de gegevens en klikt u vervolgens hoe serialiseren en deze terugconverteren. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar om te bevestigen dat de record hersteld gelijk aan de oorspronkelijke is.

Het schema in dit voorbeeld wordt uitgegaan van moet worden gedeeld tussen de lezers en schrijvers, zodat de Avro object container-indeling niet vereist is. Zie voor een voorbeeld van het serialiseren en terugconverteren van gegevens in het geheugenbuffers met behulp van een algemene record met de indeling van de container object wanneer het schema toegevoegd aan de serienummer gegevens worden moet, in het voorbeeld <a href="#Scenario4">serialisatie object containerbestanden met algemene record gebruiken</a> .


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Voorbeeld 3: Serialisatie object containerbestanden en serialisatie gebruiken met weerspiegeling

In dit voorbeeld is vergelijkbaar met het scenario in het <a href="#Scenario1">eerste voorbeeld</a>, waarin het schema impliciet is opgegeven met weerspiegeling. Het verschil is dat hier, wordt het schema niet uitgegaan bekend zijn met de lezer die deze deserializes. De objecten **SensorData** moet worden toegepast en hun impliciet opgegeven schema worden opgeslagen in een Avro object container-bestand dat wordt aangeduid door de klasse [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) .

De gegevens wordt omgezet in dit voorbeeld met [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) en gedeserialiseerde met [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Het resultaat wordt vervolgens vergeleken met de oorspronkelijke exemplaren om ervoor te zorgen identiteit.

De gegevens in het object container-bestand is gecomprimeerd via het standaard [**Deflate**] [ deflate-100] compressiecodec in .NET Framework 4. Zie het <a href="#Scenario5">vijfde voorbeeld</a> in dit onderwerp voor meer informatie over het gebruik van een bovenliggende en meer recente versie van de [**Deflate**] [ deflate-110] compressiecodec is beschikbaar in .NET Framework 4.5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Voorbeeld 4: Serialisatie object containerbestanden en serialisatie gebruiken met algemene record

In dit voorbeeld is vergelijkbaar met het scenario in het <a href="#Scenario2">tweede voorbeeld</a>, waarin het schema expliciet is opgegeven met JSON. Het verschil is dat hier, wordt het schema niet uitgegaan bekend zijn met de lezer die deze deserializes.

De gegevensset test is die worden verzameld in een lijst met [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objecten via een expliciet gedefinieerde JSON schema en vervolgens worden opgeslagen in een container-bestand van het object dat wordt aangeduid door de klasse [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) . Dit containerbestand Hiermee maakt u een schrijver die wordt gebruikt voor de gegevens, niet-gecomprimeerd, op een geheugen gegevensstroom die vervolgens naar een bestand is opgeslagen. De [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) -parameter wordt gebruikt voor het maken van de lezer wordt aangegeven dat deze gegevens worden niet gecomprimeerd.

De gegevens worden vervolgens lezen van het bestand en gedeserialiseerd in een verzameling objecten. Deze verzameling wordt vergeleken met de oorspronkelijke lijst met Avro records om te bevestigen dat ze identiek zijn.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Voorbeeld 5: Serialisatie object containerbestanden gebruiken met een aangepaste compressie-codec

Het vijfde voorbeeld wordt getoond hoe u het gebruik van een aangepaste compressie-codec voor Avro object containerbestanden. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload van de [voorbeelden van Azure code](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) -site.

De [Specificatie van Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) kunt gebruik van een optioneel compressiecodec (naast het **Null** en **Deflate** standaardwaarden). In dit voorbeeld is niet het implementeren van een volledig nieuwe codec zoals Snappy (wordt vermeld als een ondersteunde optioneel codec in de [Specificatie van Avro](http://avro.apache.org/docs/current/spec.html#snappy)). Dit wordt uitgelegd hoe u de .NET Framework 4.5 uitvoering van de [**Deflate**] [ deflate-110] codec, waarmee een betere compressiealgoritme op basis van de bibliotheek [zlib](http://zlib.net/) compressie dan de standaardversie van .NET Framework 4.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Voorbeeld 6: Avro gebruiken voor het uploaden van gegevens voor de service Microsoft Azure HDInsight

Het zesde voorbeeld ziet u enkele programming technieken die betrekking hebben op de interactie met de service Azure HDInsight. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload van de [voorbeelden van Azure code](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) -site.

De steekproef gebeurt het volgende:

* Maakt verbinding met een bestaand HDInsight service cluster.
* Serialiseert verschillende CSV-bestanden en uploadt het resultaat met Azure-blobopslag. (Het CSV-bestanden worden gedistribueerd samen met de steekproef en een uittreksel uit bijlage Aandelengrafiek historische gegevens gedistribueerd door [Infochimps](http://www.infochimps.com/) voor de periode 1970-2010 vertegenwoordigen. De steekproef leest gegevens uit een CSV-bestand, de records converteert naar exemplaren van de klasse **voorraad** en ze vervolgens serialiseert met behulp van weerspiegeling. Voorraad typedefinitie wordt gemaakt van een schema JSON via het hulpprogramma Microsoft Avro Library code generatie.
* Hiermee maakt u een nieuwe externe tabel genaamd **voorraden** in component en koppelingen deze met de gegevens in de vorige stap hebt geüpload.
* Hiermee voert een query met behulp van component via de tabel **voorraden** .

Daarnaast wordt bij de steekproef een procedure voor opschoning uitvoeren voor en na het uitvoeren van primaire bewerkingen. Tijdens de opschoning, alle gerelateerde gegevens van Azure Blob en mappen worden verwijderd en de tabel component is verbroken. U kunt ook de procedure opschonen vanaf de opdrachtregel steekproef aanroepen.

Het voorbeeld heeft de volgende vereisten:

* Een actieve Microsoft Azure-abonnement en de abonnement-ID.
* Een certificaat management voor het abonnement met de bijbehorende persoonlijke sleutel. Het certificaat moet worden geïnstalleerd in de huidige gebruiker persoonlijke opslag op de computer gebruikt voor het uitvoeren van de steekproef.
* Een actieve HDInsight cluster.
* Een opslag van Azure-account dat is gekoppeld aan het cluster HDInsight uit de vorige vereiste, samen met de bijbehorende primaire of secundaire access-sleutel.

Alle gegevens uit de vereisten moeten worden ingevoerd in het voorbeeld configuratiebestand voordat de steekproef wordt uitgevoerd. Er zijn twee manieren doen:

* Bewerk het bestand app.config in de hoofdmap van de steekproef en dit vervolgens de steekproef
* Eerst de steekproef maken en klik AvroHDISample.exe.config bewerken in de adreslijst opbouwen

In beide gevallen alle bewerkingen aangegeven moeten worden uitgevoerd de **<appSettings>** gedeelte instellingen. Volg de opmerkingen in het bestand.
De steekproef wordt uitgevoerd vanaf de opdrachtregel door de volgende opdracht uit te voeren (waar het ZIP-bestand met het voorbeeld is uitgegaan naar C:\AvroHDISample; uitgepakt als anders gebruiken voor de relevante bestandspad):

    AvroHDISample run C:\AvroHDISample\Data

Als u wilt opruimen het cluster, voert u de volgende opdracht uit:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
