## <a name="receive-messages-with-apache-storm"></a>Berichten met Apache Storm ontvangen

[**Apache Storm**](https://storm.incubator.apache.org) is een gedistribueerde realtime berekening-systeem die betrouwbare verwerking van niet-gebonden stromen van gegevens vereenvoudigen. In deze sectie wordt uitgelegd hoe u een gebeurtenis Hubs Storm spout die u voor het ontvangen van gebeurtenissen van gebeurtenis Hubs. Apache Storm gebruikt, kunt u gebeurtenissen splitsen in meerdere processen die worden gehost in verschillende knooppunten. De gebeurtenis Hubs-integratie met Storm eenvoudiger gebeurtenis verbruik door transparant plaatst controlepunten de voortgang van Storm Zookeeper installatie, het beheren van permanente controlepunten en parallel ontvangt van gebeurtenis Hubs.

Voor meer informatie over de gebeurtenis Hubs patronen ontvangen, raadpleegt u de [gebeurtenis Hubs overzicht][].

Deze zelfstudie gebruikt een installatie [HDInsight Storm][] , waarbij de gebeurtenis Hubs spout al beschikbaar wordt geleverd.

1. Volg de procedure [HDInsight Storm - aan de slag](../articles/hdinsight/hdinsight-storm-overview.md) kunt u een nieuw HDInsight cluster maken en verbinden via Extern bureaublad.

2. Kopie de `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` bestand op uw lokale ontwikkelomgeving. Dit bevat gebeurtenissen-storm-spout.

3. Gebruik de volgende opdracht uit het pakket in het lokale archief voor Maven installeren. Hiermee kunt u deze toevoegen als een verwijzing in het project Storm in een volgende stap.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. Maak een nieuw project voor Maven in Eclips, (Klik op **bestand**, klik **Nieuw** **Project**).

    ![][12]

5. Selecteer **de werkruimte standaardlocatie gebruiken**en klik op **volgende**

6. Selecteer de archetype **maven-archetype-quickstart** en klik op **volgende**

7. Een **groeps-id** en **ArtifactId**invoegen en klik vervolgens op **Voltooien**

8. In **pom.xml**, toevoegen de volgende afhankelijkheden in de `<dependency>` knooppunt.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Klik in de map **src** maken van een bestand met de naam van **Config.properties** en kopieer de volgende inhoud, wordt vervangen door de volgende waarden:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    De waarde voor **eventhub.receiver.credits** bepaalt hoeveel gebeurtenissen batch zijn verwerkt voordat u ze naar de pijplijn Storm loslaat. In dit voorbeeld wordt omdat dit eenvoudiger, deze waarde ingesteld op 10. In de productie worden dit meestal ingesteld op hogere waarden; bijvoorbeeld: 1024.

10. Maak een nieuwe klasse **LoggerBolt** met de volgende code:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Deze bout Storm Logboeken de inhoud van de ontvangen gebeurtenissen. Hiermee kan eenvoudig worden uitgebreid om op te slaan tupels in een storage-service. De [HDInsight sensor analyse zelfstudie] wordt deze dezelfde methode gebruikt voor de opslag van gegevens in HBase.

11. Maak een klasse **LogTopology** aangeroepen met de volgende code:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Deze klasse Hiermee maakt u een nieuwe gebeurtenis Hubs spout, met de eigenschappen in de configuratie opbergen naar instatiate. Het is belangrijk te weten die in dit voorbeeld zoveel spouts taken als het aantal partities in de Hub gebeurtenis gemaakt om te kunnen gebruiken de maximale parallellisme toegestaan door de Hub van die gebeurtenis.

<!-- Links -->
[Overzicht van de Hubs gebeurtenissen]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight Storm]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight sensor analyse zelfstudie]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png