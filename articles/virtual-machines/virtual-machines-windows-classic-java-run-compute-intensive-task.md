<properties
    pageTitle="Computerintensieve Java-toepassing op een VM | Microsoft Azure"
    description="Informatie over het maken van een Azure virtuele machine die wordt uitgevoerd als een computerintensieve Java-toepassing die door een andere Java-toepassing kan worden gecontroleerd."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Het uitvoeren van een taak computerintensieve in Java op een virtuele machine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Met Azure, kunt u een virtuele machine worden afgehandeld computerintensieve taken. Bijvoorbeeld kunt een virtuele machine afhandelen, taken en resultaten overbrengen op clientcomputers verbinding of mobiele toepassingen. Lees dit artikel en hebt u weet hoe een virtuele machine die wordt uitgevoerd als een computerintensieve Java-toepassing die kan worden gecontroleerd door een andere Java-toepassing maken.

Deze zelfstudie wordt ervan uitgegaan dat u weten hoe u Java-consoletoepassingen maakt, bibliotheken kunt importeren in uw Java-toepassing en een Java-archief (oppervlak) kunt genereren. Er wordt geen kennis van Microsoft Azure uitgegaan.

U leert:

* Het maken van een virtuele machine met een Java Development Kit (JDK) al is geïnstalleerd.
* Hoe u extern Meld u aan bij uw VM.
* Het maken van een service bus naamruimte.
* Het maken van een Java-toepassing waarmee een taak computerintensieve.
* Het maken van een Java-toepassing die de voortgang van de taak computerintensieve bewaakt.
* Het uitvoeren van de Java-toepassingen.
* Hoe stop de Java-toepassingen.

Deze zelfstudie wordt het probleem Traveling verkoper gebruikt voor de taak computerintensieve. Hier volgt een voorbeeld van het uitvoeren van de taak computerintensieve Java-toepassing.

![Traveling verkoper probleem Oplosser][solver_output]

Hier volgt een voorbeeld van de controle van de taak computerintensieve Java-toepassing.

![Traveling verkoper probleem-client][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2. Klik op **Nieuw**op **berekenen**, klikt u op **VM**, en klik vervolgens op **Uit de galerie**.
3. Selecteer in het dialoogvenster **VM afbeelding selecteren** **JDK 7 Windows Server 2012**.
Houd er rekening mee dat **JDK 6 Windows Server 2012** beschikbaar is voor het geval u oudere toepassingen die nog niet kan worden uitgevoerd in JDK 7 hebt.
4. Klik op **volgende**.
4. Klik in het dialoogvenster **configuratie VM** :
    1. Geef een naam voor de virtuele machine.
    2. De grootte wilt gebruiken voor de virtuele machine opgeven.
    3. Voer een naam voor de beheerder in het veld **Gebruikersnaam in te voeren** . Onthoud dat deze naam en het wachtwoord dat u het volgende wilt invoeren, u kunt ze worden gebruikt wanneer u extern Meld u aan bij de virtuele machine.
    4. Voer in het veld **Nieuw wachtwoord** een wachtwoord en voer deze opnieuw in het veld **bevestigen** . Dit is het accountwachtwoord beheerder.
    5. Klik op **volgende**.
5. Klik in het volgende dialoogvenster voor **de configuratie VM** :
    1. Voor **Cloudservice binnenkomt**, gebruikt u de standaard **maken een nieuwe cloudservice**.
    2. De waarde voor **de naam van de DNS-service Cloud** moet over cloudapp.net uniek zijn. Indien nodig, wordt deze waarde wijzigen zodat deze Azure wordt aangegeven dat deze uniek is.
    2. Geef een regio, affiniteit groep of virtuele netwerk. Geef een gebied zoals **West Amerikaans**voor toepassing van deze zelfstudie wordt.
    2. Selecteer **een account automatisch gegenereerde opslag gebruiken**voor **Opslag-Account**.
    3. Voor **Beschikbaarheid instellen**, selecteert u **(geen)**.
    4. Klik op **volgende**.
5. In het uiteindelijke in het dialoogvenster **configuratie VM** :
    1. Accepteer de standaard-eindpunt-vermeldingen.
    2. Klik op **Voltooien**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Op afstand aan te melden uw VM

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die u aanmelden wilt bij.
4. Klik op **verbinding maken**.
5. Reageer op de aanwijzingen naar wens verbinding maken met de virtuele machine. Wanneer u wordt gevraagd voor de gebruikersnaam en wachtwoord, gebruikt u de waarden die u hebt opgegeven toen u de virtuele machine hebt gemaakt.

Houd er rekening mee dat de functionaliteit voor Azure Service Bus is vereist voor het certificaat Baltimore CyberTrust Root zijn geïnstalleerd als onderdeel van van uw JRE **cacerts** store. Dit certificaat in de Java Runtime omgeving (JRE) die wordt gebruikt door deze zelfstudie automatisch opgenomen. Als u geen dit certificaat in uw JRE **cacerts** store, raadpleegt u [een certificaat voor de de opslag van Java CA certificaat toe te voegen] [ add_ca_cert] voor informatie over het toevoegen van deze (evenals informatie over het weergeven van de certificaten in de winkel cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Het maken van een service bus naamruimte

Als u wilt gebruiken Service Bus wachtrijen in Azure wordt aangegeven, moet u eerst de naamruimte van een service maken. De naamruimte van een service biedt een scope container voor de adressering van Service Bus bronnen binnen uw toepassing.

Een Servicenaamruimte maken:

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2.  Klik in het deelvenster navigatie linksonder van de Azure klassieke portal op **Service No, toegangsbeheer & cache**.
3.  Klik in het deelvenster linksboven van de Azure klassieke portal het knooppunt **Service Bus** op en klik vervolgens op de knop **Nieuw** .  
    ![Schermafbeelding van de service Bus knooppunt][svc_bus_node]
4.  In het dialoogvenster **maken een nieuwe Service Namespace** , voert u een **Namespace**en klik vervolgens op de knop **Beschikbaarheid controleren** om ervoor te zorgen dat het een unieke.  
    ![Een nieuwe Namespace schermafbeelding maken][create_namespace]
5.  Wanneer de naamruimtenaam van de ervoor te zorgen dat beschikbaar is, kiest u het land of de regio waarin uw naamruimte moeten worden uitgevoerd en klik op de knop **Namespace maken** .  

    De naamruimte die u hebt gemaakt, worden vervolgens weergegeven in de portal van Azure klassieke en duurt even te activeren. Wacht totdat de status **actief** is voordat u verdergaat met de volgende stap.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>De Management standaard referenties voor de naamruimte verkrijgen

Om te kunnen uitvoeren management bewerkingen uitvoeren, zoals het maken van een wachtrij, klik op de nieuwe naamruimte, moet u de management referenties voor de naamruimte verkrijgen.

1.  Klik op het knooppunt **Service Bus** om weer te geven van de lijst met beschikbare naamruimten in het linkernavigatiedeelvenster.
    ![Schermafbeelding van beschikbare naamruimten][avail_namespaces]
2.  Selecteer de naamruimte die u zojuist hebt gemaakt in de lijst weergegeven.
    ![Schermafbeelding van Namespace lijst][namespace_list]
3.  Het deelvenster aan de rechterkant **Eigenschappen** bevat de eigenschappen voor de nieuwe naamruimte.
    ![Eigenschappen van deelvenster schermafbeelding][properties_pane]
4.  De **Standaard-sleutel** is verborgen. Klik op de knop **weergave** om de beveiligingsreferenties weer te geven.
    ![Schermafbeelding van de standaard-toets][default_key]
5.  Maak een notitie van de **Standaard uitgever** en de **Toets standaard** zoals u kunt deze gegevens hieronder bewerkingen uitvoeren met de naamruimte.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Het maken van een Java-toepassing waarmee een taak computerintensieve

1. Op uw computer ontwikkeling (dat niet hoeft te worden de virtuele machine die u hebt gemaakt) downloaden van de [Azure SDK for Java](https://azure.microsoft.com/develop/java/).
2. Maak een Java-console-toepassing met de voorbeeldcode aan het einde van deze sectie. In deze zelfstudie gebruiken we **TSPSolver.java** als de naam van het Java-bestand. Wijzig de **uw\_service\_bus\_naamruimte**, **uw\_service\_bus\_eigenaar**, en **uw\_service\_bus\_sleutel** tijdelijke aanduidingen voor gebruik van uw service bus **naamruimte**, **Standaard uitgever** en **Standaard** sleutelwaarden, respectievelijk.
3. Na het kleurcodering, de toepassing naar een runnable Java-archief (oppervlak) exporteren en pakket inpakken vereiste bibliotheken in het gegenereerde oppervlak. In deze zelfstudie gebruiken we **TSPSolver.jar** als de gegenereerde oppervlak-naam.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Het maken van een Java-toepassing die de voortgang van de taak computerintensieve bewaakt

1. Maak een Java-console-toepassing met de voorbeeldcode aan het einde van deze sectie op uw computer ontwikkeling. In deze zelfstudie gebruiken we **TSPClient.java** als de naam van het Java-bestand. Zoals eerder, wijzig de **uw\_service\_bus\_naamruimte**, **uw\_service\_bus\_eigenaar**, en **uw\_service\_bus\_sleutel** tijdelijke aanduidingen voor gebruik van uw service bus **naamruimte**, **Standaard uitgever** en **Standaard** sleutelwaarden, respectievelijk.
2. De toepassing naar een runnable oppervlak exporteren en pakket inpakken vereiste bibliotheken in het gegenereerde oppervlak. In deze zelfstudie gebruiken we **TSPClient.jar** als de gegenereerde oppervlak-naam.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Het uitvoeren van de Java-toepassingen
Voer de toepassing computerintensieve, klikt u eerst naar de wachtrij maken, klikt u vervolgens om het probleem Saleseman reizen, waarin de huidige beste route wordt toegevoegd aan de service bus wachtrij op te lossen. Terwijl de toepassing computerintensieve actief (of achteraf), de client om weer te geven resultaten van de service bus wachtrij uitvoeren.

### <a name="to-run-the-compute-intensive-application"></a>De toepassing computerintensieve uit te voeren

1. Meld u aan bij uw VM.
2. Maak een map waarin u de toepassing wordt uitgevoerd. Bijvoorbeeld: **c:\TSP**.
3. **TSPSolver.jar** naar **c:\TSP**, kopiëren
4. Een bestand met de naam **c:\TSP\cities.txt** met de volgende inhoud maken.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Bij een opdrachtprompt door mappen te wijzigen in c:\TSP.
6. Controleer of de map bin van de JRE op de omgevingsvariabele PATH.
7. U moet maken van de service bus wachtrij voordat u de TSP Oplosser permutaties uitvoert. Voer de volgende opdracht om de service bus wachtrij te maken.

        java -jar TSPSolver.jar createqueue

8. Nu dat de wachtrij is gemaakt, kunt u de TSP Oplosser permutaties kunt uitvoeren. Bijvoorbeeld, voer de volgende opdracht Oplosser voor 8 steden uitvoeren.

        java -jar TSPSolver.jar 8

 Als u een nummer niet opgeeft, wordt deze uitgevoerd voor 10 steden. Terwijl de Oplosser huidige kortste routes gevonden, wordt deze ze toevoegen aan de wachtrij.

> [AZURE.NOTE]
> Des te groter het nummer dat u hebt opgegeven, hoe langer de Oplosser worden uitgevoerd. Bijvoorbeeld uitgevoerd gedurende 14 steden enkele minuten duren kunnen en uitvoeren voor 15 steden kan enkele uren duren. Vergroten naar 16 of meer steden kan resulteren in dagen van runtime (uiteindelijk weken, maanden en jaren). Dit is vanwege de snelle stijging van het aantal permutaties die door Oplosser worden geëvalueerd als het aantal steden toeneemt.

### <a name="how-to-run-the-monitoring-client-application"></a>Het uitvoeren van de controle-clienttoepassing
1. Meld u aan bij de computer waarop u de clienttoepassing wordt uitgevoerd. Dit hoeft geen moeten dezelfde computer uitvoeren van de toepassing **TSPSolver** , hoewel het kan zijn.
2. Maak een map waarin u de toepassing wordt uitgevoerd. Bijvoorbeeld: **c:\TSP**.
3. **TSPClient.jar** naar **c:\TSP**, kopiëren
4. Controleer of de map bin van de JRE op de omgevingsvariabele PATH.
5. Bij een opdrachtprompt door mappen te wijzigen in c:\TSP.
6. Voer de volgende opdracht.

        java -jar TSPClient.jar

    (Optioneel) Geef het aantal minuten slaapstand bij het controleren van de wachtrij door doorgeven in opdrachtregelargumenten. De standaard naar bed gaan periode voor het controleren van de wachtrij is 3 minuten, die wordt gebruikt als er geen opdrachtregel argument is opgegeven bij **TSPClient**. Als u een andere waarde voor het interval naar bed gaan gebruiken wilt, bijvoorbeeld een minuut, voer de volgende opdracht.

        java -jar TSPClient.jar 1

    De client uitgevoerd totdat deze een bericht wachtrij "Volledig ziet". Houd er rekening mee dat als u meerdere exemplaren van de Oplosser uitgevoerd zonder het uitvoeren van de client, u moet mogelijk meerdere keren volledig leegmaken de wachtrij voor de client uitvoeren. U kunt ook de wachtrij verwijderen en opnieuw maken. De wachtrij wilt verwijderen, voer de volgende opdracht uit **TSPSolver** (niet **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    De Oplosser uitgevoerd totdat het voltooid is alle routes onderzoeken.

## <a name="how-to-stop-the-java-applications"></a>Hoe stop de Java-toepassingen
Voor Oplosser zowel clienttoepassingen, kunt u druk op **Ctrl + C** om af te sluiten als u wilt beëindigen vóór normaal is voltooid.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
