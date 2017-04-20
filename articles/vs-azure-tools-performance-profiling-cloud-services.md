<properties 
   pageTitle="De prestaties van een cloudservice testen | Microsoft Azure"
   description="De prestaties van een cloudservice met de Visual Studio-profiler testen"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>De prestaties van een cloudservice testen 

##<a name="overview"></a>Overzicht

U kunt de prestaties van een cloudservice testen op de volgende manieren:

- Diagnostisch hulpprogramma Azure gebruikt voor het verzamelen van informatie over het aanvragen en verbindingen en moet worden gereviseerd statistieken voor websites die weergeven hoe de service kunt uitvoeren vanuit het klantperspectief van een. Zie [Diagnostisch hulpprogramma configureren voor Azure-Cloudservices en virtuele Machines]( http://go.microsoft.com/fwlink/p/?LinkId=623009)om te beginnen met.

- Gebruik de profiler Visual Studio om een uitgebreide analyse van de rekenkundige aspecten van hoe de service wordt uitgevoerd. Als dit onderwerp wordt beschreven, kunt u de profiler prestaties meten, zoals een service wordt uitgevoerd in Azure wordt aangegeven. Zie [testen van de prestaties van een Azure Cloud Service lokaal in het berekenen Emulator met de Visual Studio-Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845)voor informatie over het gebruik van de profiler prestaties meten, zoals een service lokaal wordt uitgevoerd in een berekeningscluster emulator.



## <a name="choosing-a-performance-testing-method"></a>Een methode testen prestaties kiezen

###<a name="use-azure-diagnostics-to-collect"></a>Gebruik Azure diagnostische hulpprogramma's voor het verzamelen van:###

- Statistieken op webpagina's of services, zoals aanvragen en verbindingen.

- Statistieken over rollen, zoals hoe vaak een rol opnieuw is opgestart.

- Algemene informatie over geheugengebruik, zoals het percentage van de tijd die de garbagecollector duurt of het geheugen instellen van een actieve rol.

###<a name="use-the-visual-studio-profiler-to"></a>Gebruik de Visual Studio profiler naar:###

- Hiermee bepaalt u welke functies kunt u de meeste tijd.

- Hoe lang duurt voor elk onderdeel van een rekenkracht programma meten.

- Vergelijk de van gedetailleerde prestatierapporten voor twee versies van een service.

- Geheugentoewijzing uitgebreider dan het niveau van afzonderlijke geheugentoewijzingen analyseren.

- Gelijktijdigheidsproblemen bij met meerdere threads code analyseren.

Wanneer u de profiler gebruikt, kunt u gegevens verzamelen wanneer een cloudservice wordt uitgevoerd lokaal of in Azure wordt aangegeven.

###<a name="collect-profiling-data-locally-to"></a>Verzamelen profileringsgegevens lokaal naar:###

- Test de prestaties van een deel van een cloudservice, zoals het uitvoeren van bepaalde werknemer rol waarvoor een realistische gesimuleerd laden niet.

- Test de prestaties van een cloudservice in moeten worden geïsoleerd, gecontroleerd.

- De prestaties van een cloudservice testen voordat u deze bij Azure implementeert.

- Test de prestaties van een cloudservice privé, zonder de bestaande implementaties storen.

- Test de prestaties van de service zonder kosten voor het uitvoeren van in Azure wordt aangegeven.

###<a name="collect-profiling-data-in-azure-to"></a>Verzamelen profileringsgegevens in Azure wordt aangegeven naar:###

- Test de prestaties van een cloudservice onder een gesimuleerd of reële laden.

- Gebruik de methode instrumentation verzamelen profileringsgegevens, zoals verderop in dit onderwerp wordt beschreven.

- Test de prestaties van de service in dezelfde omgeving als wanneer de service wordt uitgevoerd in productie.

U meestal simuleren een werklast voor test-cloudservices onder normaal of belasting.

## <a name="profiling-a-cloud-service-in-azure"></a>Profielen van een cloudservice in Azure wordt aangegeven

Wanneer u uw cloudservice van Visual Studio publiceert, kunt u de service profiel en de profielbeleid instellingen waarmee u de informatie die u wilt opgeven. Een profielbeleid sessie is voor elk exemplaar van een rol gestart. Zie [publiceren naar een Cloudservice Azure van Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)voor meer informatie over het publiceren van uw service uit Visual Studio.

Als u wilt weten over meer informatie over prestaties profielen in Visual Studio, raadpleegt u [Beginners handleiding voor het profiel van prestaties](https://msdn.microsoft.com/library/azure/ms182372.aspx) en [Prestaties van toepassingen analyseren met hulpprogramma's voor profielen gebruiken](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] U kunt IntelliTrace of profiel wanneer u uw cloudservice publiceert inschakelen. U kunt niet beide inschakelen.

###<a name="profiler-collection-methods"></a>Profiler siteverzameling methoden

U kunt verschillende methoden gebruiken voor het samenstellen van profiel, op basis van uw prestatieproblemen:

- **CPU steekproeven** - met deze methode verzamelt-statistieken die handig voor het eerste analyse van CPU-gebruik problemen zijn. CPU steekproeven is de voorgestelde methode voor het starten van de meeste prestaties onderzoeken. Er is een lage invloed op de toepassing waarin u een beoordeling zijn wanneer u CPU steekproeven gegevens verzamelen.

- **Instrumentation** -deze methode verzamelt gegevens over gedetailleerde tijdsinstellingen is handig voor focus analyse en voor het analyseren van invoer/uitvoer prestatieproblemen. De methode instrumentation vastgelegd elke vermelding, afsluiten en functie-aanroep van de functies in een module tijdens een profiel uitvoeren. Deze methode is handig voor het verzamelen van tijdsinstellingen voor gedetailleerde informatie over een sectie van de code en voor de invloed van invoer- en uitvoerbereik bewerkingen van de toepassingsprestaties. Deze methode is uitgeschakeld voor een computer waarop een 32-bits besturingssysteem wordt uitgevoerd. Deze optie is alleen beschikbaar wanneer u de cloudservice in Azure wordt aangegeven, niet lokaal in de emulator berekeningscluster uitvoeren.

- **Geheugentoewijzing .NET** - met deze methode verzamelt .NET Framework geheugen toegewezen gegevens met behulp van de methode profielen steekproeven. De verzamelde gegevens bevat het aantal en het formaat van de toegewezen objecten.

- **Bij gelijktijdigheid** - met deze methode verzamelt resourcegegevens conflict en proces en thread execution-gegevens die is nuttig zijn bij het analyseren van meerdere threads toestaan en met meerdere processen-toepassingen. De methode gelijktijdigheid verzamelt gegevens voor elke gebeurtenis dat blokken uitvoering van de code, zoals wanneer een thread wacht voor toegang tot de bron van een toepassing dat u beschikbaar wilt vergrendeld. Deze methode is handig voor het analyseren van toepassingen met meerdere threads.

- U kunt ook uitschakelen **Laag interactie profielen**, waarmee u meer informatie over de tijden voor de uitvoering van synchroon ADO.NET-oproepen in de functies van meerlagige toepassingen die met een of meer databases communiceren. U kunt gegevens van de laag interactie verzamelen met een van de profielbeleid methoden. Zie [Laag Interacties weergeven](https://msdn.microsoft.com/library/azure/dd557764.aspx)voor meer informatie over het samenstellen van laag interactie profiel.

## <a name="configuring-profiling-settings"></a>Profielbeleid instellingen configureren

De volgende afbeelding ziet hoe u uw profielbeleid instellingen in het dialoogvenster publiceren Azure-toepassing configureren.

![Instellingen voor profielen configureren](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Als u wilt dat het selectievakje **inschakelen profiel** , moet u de profiler geïnstalleerd op de lokale computer die u gebruikt voor het publiceren van uw cloudservice hebben. De profiler is standaard geïnstalleerd tijdens de installatie van Visual Studio.

### <a name="to-configure-profiling-settings"></a>Profielbeleid instellingen configureren

1. Open het snelmenu voor uw project Azure in Solution Explorer en kies **publiceren**. Zie [publiceren van een wolk service met de Azure hulpmiddelen](http://go.microsoft.com/fwlink/p?LinkId=623012)voor meer informatie over het publiceren van een cloudservice.

1. Klik in het dialoogvenster **Publiceren Azure-toepassing** voor het tabblad **Advanced Settings** kiest.

1. Als u een beoordeling, selecteert u het selectievakje **inschakelen profiel** .

1. Als u wilt uw profielbeleid instellingen hebt geconfigureerd, kiest u de hyperlink **Instellingen** . Het dialoogvenster Instellingen voor profielen wordt weergegeven.

1. Kies het type van het profiel dat u moet het keuzerondje **welke methode van het profiel wilt u gebruiken** .

1. De laag interactie profielbeleid om gegevens te verzamelen, schakel het selectievakje **Profielen interactie met laag inschakelen** uit.

1. Als u wilt opslaan in de instellingen, kies de knop **OK** .

    Wanneer u deze toepassing publiceert, wordt deze instellingen worden gebruikt om de profielbeleid sessie voor elke rol te maken.

## <a name="viewing-profiling-reports"></a>Profielen van rapporten weergeven

Een profielbeleid sessie is voor elk exemplaar van een rol in de cloudservice gemaakt. Als u wilt uw profielbeleid rapporten van elke sessie vanuit Visual Studio weergeven, kunt u het venster Server Explorer weergeven en kies vervolgens het knooppunt Azure berekenen om te selecteren van een exemplaar van een rol. Vervolgens kunt u het profielbeleid rapport bekijken, zoals wordt weergegeven in de volgende afbeelding.

![Weergave profielen rapport van Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profielbeleid rapporten weergeven

1. Kies om weer te geven het Server Verkenner-venster in Visual Studio, in het menu weergave, Server Explorer.

1. Kies het knooppunt Azure berekenen en kies vervolgens het knooppunt Azure-implementatie voor de cloudservice die u hebt geselecteerd voor profiel wanneer u in Visual Studio gepubliceerd.

1. Profielbeleid om rapporten te bekijken voor een exemplaar, kiest u de rol in de service, opent u het snelmenu voor een specifiek exemplaar en kiest u **Een beoordeling View-rapport**.

    Het rapport, een bestand .vsp wordt nu gedownload van Azure en de status van de download weergegeven in het gebeurtenissenlogboek van Azure. Wanneer het downloaden is voltooid, het profielbeleid rapport wordt weergegeven op een tabblad in de editor voor Visual Studio met de naam <Role name> _<Instance Number>_ <identifier>.vsp. Samengevatte gegevens voor het rapport wordt weergegeven.

1. Kies het type weergave die u wilt om verschillende weergaven van het rapport, in de lijst huidige weergave. Zie [Extra rapportweergaven profielen](https://msdn.microsoft.com/library/azure/bb385755.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Voor foutopsporing in Cloudservices](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publiceren naar een Azure Cloudservice van Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

