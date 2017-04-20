<properties
    pageTitle="Azure-toepassingen in Eclips foutopsporing"
    description="Meer informatie over de Azure voor foutopsporing in toepassingen met behulp van de Azure-Toolkit voor Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Azure-toepassingen in Eclips foutopsporing #

Met de Toolkit Azure voor Eclips, kunt u uw toepassingen foutopsporing, ongeacht of ze worden uitgevoerd in Azure wordt aangegeven of in de emulator berekenen als u Windows als uw besturingssysteem gebruikt. De volgende afbeelding ziet u de eigenschappen van het **Foutopsporing** dialoogvenster gebruikt voor foutopsporing op afstand inschakelen:

![][ic719504]

Deze zelfstudie wordt ervan uitgegaan dat u al een toepassing die is gemaakt, en worden vertrouwd met de emulator berekenen en implementeren voor Azure.

We gebruiken de toepassing uit de handleiding voor het [gebruik van de Azure Service Runtime Library in JSP][] als uitgangspunt voor dit onderwerp. Voordat u verder gaat, die toepassing te maken als u dit nog niet hebt gedaan.

## <a name="to-debug-your-application-while-running-in-azure"></a>Voor foutopsporing van uw toepassing tijdens het uitvoeren van in Azure wordt aangegeven ##

>[AZURE.WARNING] Huidige ondersteuning van de toolkit voor foutopsporing op afstand Java is hoofdzakelijk voor implementaties uitgevoerd in de emulator Azure berekeningscluster bedoeld. Omdat de verbinding foutopsporing niet veilig is, moet u niet inschakelen foutopsporing op afstand in productie implementaties. Als u nodig hebt voor een toepassing wordt uitgevoerd in de cloud Azure foutopsporing, gebruikt u de implementatie van een tijdelijk opslaan, maar en zich daarna realiseert dat onbevoegde toegang tot uw sessie voor foutopsporing mogelijk als iemand bekend is het IP-adres van uw cloud-implementatie, is zelfs als dit een tijdelijk opslaan implementatie is.

1. Uw project voor het testen van in de emulator maakt: In Eclips Projectverkenner met de rechtermuisknop op **MyAzureProject**, klikt u op **Eigenschappen**klikt u op **Azure**en instellen **voor opbouwen** aan **implementatie naar cloud**.
1. Opnieuw opbouwen van uw project: Klik in het menu Eclips op **Project**en klik op **Alle maken**.
1. De toepassing *waarin u tijdelijk opslaat* in Azure implementeren.
    >[AZURE.IMPORTANT] Bovengenoemde, wordt het ten zeerste aanbevolen dat u fouten in de emulator berekenen in de meeste gevallen opsporen en klik in de testomgeving foutopsporing alleen als extra foutopsporing is vereist. Het wordt aanbevolen geen fouten opsporen in de productieomgeving.
1. Krijgen wanneer uw implementatie klaar in Azure wordt aangegeven is, de DNS-naam voor de implementatie van de [Azure-beheerportal][]. Een tijdelijk opslaan implementatie heeft een DNS-naam in de vorm van http://*&lt;guid&gt;*. cloudapp.net, waar * &lt;guid&gt; * een GUID-waarde door Azure toegewezen.
1. In de Eclips Projectverkenner met de rechtermuisknop op **WorkerRole1** **Azure**op en klik vervolgens op **Foutopsporing**.
1. Klik in het dialoogvenster **Eigenschappen voor WorkerRole1 foutopsporing** :
    1. Controleer **Foutopsporing op afstand voor deze rol inschakelen.**
    1. Voor **invoer eindpunt te gebruiken**, gebruikt u **Foutopsporing (openbare: 8090, privé: 8090)**.
    1. Controleer of **Dat JVM starten in onderbroken modus, wachten op een verbinding foutopsporing** is uitgeschakeld.
        >[AZURE.IMPORTANT] De optie **Start JVM in onderbroken modus, wachten op een verbinding foutopsporing** is bedoeld voor geavanceerde scenario's in de berekeningscluster emulator alleen foutopsporing (niet voor cloud-implementaties). Als de optie **Start JVM in onderbroken modus, wachten op een foutopsporing verbinding** wordt gebruikt, wordt deze van de server opstartproces onderbreken totdat de foutopsporing Eclips is verbonden met de JVM. Terwijl u deze optie voor een sessie met de emulator berekeningscluster voor foutopsporing gebruiken kunt, gebruik deze niet voor een sessie voor foutopsporing in een cloud-implementatie. Initialisatie van een server wordt uitgevoerd in een taak Azure opstarten en de Azure cloud maakt geen openbare eindpunten beschikbaar totdat de opstarten taak is voltooid. Daarom een opstartproces niet worden voltooid als deze optie is ingeschakeld in een cloud-implementatie, omdat deze niet kunnen voor het ontvangen van een verbinding uit een externe Eclips-client.
1. Klik op **Foutopsporing configuraties maken**.
1. Klik in het dialoogvenster **Azure foutopsporing configuratie** :
    1. Voor **Java project om op te lossen**, selecteert u het project **MyHelloWorld** .
    1. Voor het **configureren voor foutopsporing**, raadpleegt u **Azure cloud (tijdelijke)**.
    1. Controleer of **dat Azure berekenen emulator** is uitgeschakeld.
    1. Voor **Host**, voert u de DNS-naam van de gefaseerde implementatie, maar zonder de voorgaande **http://**. Bijvoorbeeld (Gebruik uw GUID in plaats van de GUID Hier wordt getoond): **4e616d65-6f6e-6-d 65-6973-526f62657274.cloudapp.net**
1. Klik op **OK** om de **Configuratie van Azure fouten opsporen in** dialoogvenster te sluiten.
1. Klik op **OK** om de **Eigenschappen voor WorkerRole1 foutopsporing** dialoogvenster te sluiten.
1. Als u geen een onderbrekingspunt in index.jsp zijn ingesteld, stelt u deze:
    1. Binnen de Eclips Projectverkenner **MyHelloWorld**uitvouwen, **webinhoud**uitvouwen en dubbelklik op **index.jsp**.
    1. Binnen index.jsp, klik met de rechtermuisknop op de blauwe balk aan de linkerkant van uw Java-code en klik op de **Wisselknop onderbrekingspunten**, zoals wordt weergegeven in het volgende: ![][ic551537]
1. Klik op **uitvoeren** en klik vervolgens op **Fouten opsporen in configuraties**binnen Eclips van menu.
1. Klik in het dialoogvenster **Foutopsporing configuraties** uitvouwen **Externe Java-toepassing** in het linkerdeelvenster, selecteer **Azure Cloud (WorkerRole1)**en op **fouten opsporen in**.
1. Uw browser, Voer uw gefaseerde toepassing, **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, wordt vervangen door de GUID van uw DNS-naam voor * &lt;guid&gt;*. Als u hierom wordt gevraagd een dialoogvenster voor **Bevestigen perspectief veranderen** , klikt u op **Ja**. Uw sessie voor foutopsporing moet nu worden uitgevoerd op de regel met code waar het onderbrekingspunt werd ingesteld.

>[AZURE.NOTE] Als u probeert te starten van een externe wordt verbinding met een implementatie met meerdere exemplaren die rol actief zijn, kunt u geen momenteel beheren welke exemplaar foutopsporing foutopsporing in eerste instantie verbonden met het, zoals de Azure taakverdeling wordt een exemplaar willekeurig kiezen. Nadat u met die instantie verbonden bent, wordt u echter gewoon voor foutopsporing in hetzelfde exemplaar. Opmerking ook als er een periode van inactiviteit van meer dan 4 minuten (bijvoorbeeld wanneer u bent gestopt bij een onderbrekingspunt te lang), Azure mogelijk sluit de verbinding.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Voor foutopsporing in een exemplaar van de specifieke rol in een meerdere exemplaren-implementatie ##

Wanneer uw implementatie wordt uitgevoerd in de cloud, uitvoert u waarschijnlijk deze in meerdere berekeningscluster, of rol, exemplaren. Hiermee kunt u profiteren van garanties voor de beschikbaarheid van Azure 99.95% en aan de nieuwe schaal out van uw toepassing.

In deze scenario's moet u mogelijk op afstand fouten opsporen in uw Java-toepassing in het exemplaar van een specifieke rol. Als u alleen een gewone invoer eindpunt voor foutopsporing inschakelt, wordt de Azure taakverdeling nog wel deze vrijwel waardoor u foutopsporing verbinden met een specifieke rol exemplaar. In plaats daarvan wordt deze verbonden met een exemplaar van de rol die deze willekeurig hervat.

Dit is het type scenario waar profiteren van de invoer eindpunten exemplaar wordt eenvoudiger kunt fouten opsporen in een exemplaar van de specifieke rol.

Stel dat u van plan bent om uit te voeren maximaal 5 rol exemplaren van uw implementatie. De pagina van de eigenschap **eindpunten** in het eigenschappenvenster van de rol gebruikt, een exemplaar invoer eindpunt maken en hieraan een bereik van openbare poorten, in plaats van een getal met één poort. Geef bijvoorbeeld **81-85**in het vak Invoerbereik **openbare poort** .

Nadat u uw toepassing met dit exemplaar eindpunt implementeert, worden Azure een uniek poortnummer uit dit bereik toewijzen aan elk van de rol exemplaren. Klik om te weten welke exemplaar welke poortnummer hebt toegewezen, kunt u de omgevingsvariabele *InstanceEndpointName***_PUBLICPORT** (indien *InstanceEndpointName* de naam die u is bij het maken van het eindpunt exemplaar toegewezen) automatisch geconfigureerd door de Toolkit gebruiken in uw implementatie (bijvoorbeeld door te retourneren de waarde in de voettekst van een webpagina, zodat u deze lezen kan wanneer u naar deze bladeren).

Zodra u weet welke openbare poortnummer in dat exemplaar is toegewezen, kunt u deze verwijzing in de configuratie van uw foutopsporing in Eclips, door het aanbrengen van deze naar de hostnaam van de van uw service. Hiermee schakelt u de foutopsporing Eclips verbinding maken met die specifieke exemplaar, en niet een van de andere exemplaren.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Alleen Windows: voor uw toepassing tijdens het uitvoeren van in de emulator berekeningscluster foutopsporing ##

>[AZURE.NOTE] De Azure emulator is alleen beschikbaar voor Windows. Dit gedeelte overslaan als u een besturingssysteem dan Windows gebruikt. 

1. Uw project voor het testen van in de emulator maakt: In Eclips Projectverkenner met de rechtermuisknop op **MyAzureProject**, klikt u op **Eigenschappen**klikt u op **Azure**en **voor opbouwen** instellen om te **testen in emulator**.
1. Opnieuw opbouwen van uw project: Klik in het menu Eclips op **Project**en klik op **Alle maken**.
1. In de Eclips Projectverkenner met de rechtermuisknop op **WorkerRole1** **Azure**op en klik vervolgens op **Foutopsporing**.
1. Klik in het dialoogvenster **Eigenschappen voor WorkerRole1 foutopsporing** :
    1. Controleer **Foutopsporing op afstand voor deze rol inschakelen.**
    1. Voor **invoer eindpunt te gebruiken**, gebruikt u de standaardeindpunt dat automatisch wordt gegenereerd door de toolkit, vermeld als **Foutopsporing (openbare: 8090, privé: 8090)**.
    1. Controleer of dat de optie **Start JVM in onderbroken modus, wachten op een verbinding foutopsporing** is uitgeschakeld.
        >[AZURE.IMPORTANT] De optie **Start JVM in onderbroken modus, wachten op een verbinding foutopsporing** is bedoeld voor geavanceerde scenario's in de berekeningscluster emulator alleen foutopsporing (niet voor cloud-implementaties). Als de optie **Start JVM in onderbroken modus, wachten op een foutopsporing verbinding** wordt gebruikt, wordt deze van de server opstartproces onderbreken totdat de foutopsporing Eclips is verbonden met de JVM. Terwijl u deze optie voor een sessie met de emulator berekeningscluster voor foutopsporing gebruiken kunt, gebruik deze niet voor een sessie voor foutopsporing in een cloud-implementatie. Initialisatie van een server wordt uitgevoerd in een taak Azure opstarten en de Azure cloud maakt geen openbare eindpunten beschikbaar totdat de opstarten taak is voltooid. Daarom een opstartproces niet worden voltooid als deze optie is ingeschakeld in een cloud-implementatie, omdat deze niet kunnen voor het ontvangen van een verbinding uit een externe Eclips-client.
1. Klik op **Foutopsporing configuraties maken**.
1. Klik in het dialoogvenster **Azure foutopsporing configuratie** :
    1. Voor **Java project om op te lossen**, selecteert u het project **MyHelloWorld** .
    1. Voor het **configureren voor foutopsporing**, raadpleegt u **Azure emulator berekenen**.
1. Klik op **OK** om de **Configuratie van Azure fouten opsporen in** dialoogvenster te sluiten.
1. Klik op **OK** om de **Eigenschappen voor WorkerRole1 foutopsporing** dialoogvenster te sluiten.
1. Stel een onderbrekingspunt in index.jsp:
    1. Binnen de Eclips Projectverkenner **MyHelloWorld**uitvouwen, **webinhoud**uitvouwen en dubbelklik op **index.jsp**.
    1. Binnen index.jsp, klik met de rechtermuisknop op de blauwe balk aan de linkerkant van uw Java-code en klik op de **Wisselknop onderbrekingspunten**, zoals wordt weergegeven in het volgende: ![][ic551537]

       Een onderbrekingspunt is ingesteld als u een pictogram onderbrekingspunt binnen de blauwe balk aan de linkerkant van uw Java-code wordt weergegeven.
1. Start de toepassing in de emulator berekenen door te klikken op de knop **uitvoeren in Azure Emulator** op de werkbalk Azure.
1. Klik op **uitvoeren** en klik vervolgens op **Fouten opsporen in configuraties**binnen Eclips van menu.
1. Klik in het dialoogvenster **Foutopsporing configuraties** uitvouwen **Externe Java-toepassing** in het linkerdeelvenster, selecteer **Azure Emulator (WorkerRole1)**en op **fouten opsporen in**.
1. Nadat de emulator berekeningscluster wordt aangegeven dat uw toepassing wordt uitgevoerd, klikt u in uw browser **http://localhost:8080/MyHelloWorld**uitvoeren.
    Als u hierom wordt gevraagd een dialoogvenster voor **Bevestigen perspectief veranderen** , klikt u op **Ja**.
    Uw sessie voor foutopsporing moet nu worden uitgevoerd op de regel met code waar het onderbrekingspunt werd ingesteld.

Dit hebt u geleerd hoe u fouten opsporen in de emulator berekeningscluster; het volgende gedeelte ziet u hoe u fouten opsporen in een toepassing geïmplementeerd in Azure.

## <a name="debugging-notes"></a>Voor foutopsporing in notities ##

* Na het foutopsporing, kunt u het perspectief van **fouten opsporen in** **Java** schakelen via te klikken op Eclips van menu- **venster** **Openen vanuit het perspectief**op en selecteer van het perspectief die u wilt gebruiken.
* Als u wilt inschakelen voor foutopsporing op afstand in GlassFish, gebruik geen externe van de Configuratiefunctie voor foutopsporing van de Azure-Toolkit voor Eclips. In plaats daarvan GlassFish handmatig configureren. Vanwege de manier waarop die glassfish Java opties vooraf gedefinieerd in de omgevingsvariabelen wordt verwerkt, van de toolkit externe foutopsporing Configuratiefunctie werkt niet goed met GlassFish. Als de toolkit van externe configuratie van de functie voor foutopsporing is ingeschakeld, kan deze verhinderen dat GlassFish starten.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Gebruik van de Azure-Service Runtime Library in JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
