<properties
    pageTitle="Laden uw toepassing testen met behulp van Visual Studio Team Services | Microsoft Azure"
    description="Leer hoe testen uw stof van Azure-Service-toepassingen met behulp van Visual Studio Team Services."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Laden uw toepassing testen met behulp van Visual Studio Team Services

In dit artikel leest hoe u Microsoft Visual Studio laden test functies gebruiken om te testen een toepassing. Een Azure-Service stof statuscontrole service back-end en een front-end van het web stateless service wordt gebruikt. De voorbeeld-toepassing waarmee het hier is een vliegtuig locatie simulator. U opgeeft een vliegtuig-ID, vertrektijd en bestemming. Het aanvragen van de toepassing back-end worden verwerkt, en de front-end worden weergegeven op een kaart het vliegtuig die voldoet aan de criteria.

In het volgende diagram ziet u de Service stof-toepassing die u moet worden testen.

![Diagram van de toepassing voorbeeld vliegtuig locatie][0]

## <a name="prerequisites"></a>Vereisten voor
Voordat u aan de slag, moet u als volgt te werk:

- Een Visual Studio Team Services-account aanschaffen. U opent een gratis voor [Visual Studio Team Services](https://www.visualstudio.com).
- Ophalen en installeert u Visual Studio 2013 of Visual Studio-2015. In dit artikel Visual Studio 2015 Enterprise edition gebruikt, maar Visual Studio 2013 en andere versies op dezelfde manier moeten werken.
- Uw toepassing naar een testomgeving implementeren. Lees [hoe u een extern cluster met Visual Studio-toepassingen implementeren](service-fabric-publish-app-remote-cluster.md) voor meer informatie hierover.
- Van uw toepassing gebruikspatroon te begrijpen. Deze informatie wordt gebruikt voor het patroon laden simuleren.
- Meer informatie over het doel voor het laden testen. Hiermee kunt u interpreteren en analyseren de testresultaten laden.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Maken en uitvoeren van het project webprestaties en laden testen

### <a name="create-a-web-performance-and-load-test-project"></a>Een project webprestaties en laden Test maken

1. Open Visual Studio-2015. Kies **bestand** > **Nieuw** > **Project** op de menubalk om het dialoogvenster **Nieuw Project** te openen.

2. Vouw het knooppunt **Visual C#** en selecteer **Test** > **webprestaties en laden Test project**. Geef een naam op voor het project en kies vervolgens de knop **OK** .

    ![Schermafbeelding van het dialoogvenster Nieuw Project][1]

    Hier ziet u een nieuw webprestaties en laden Test project in Solution Explorer.

    ![Schermafbeelding van de oplossing Explorer met het nieuwe project][2]

### <a name="record-a-web-performance-test"></a>Een web prestaties toets opnemen

1. Open het project .webtest.

2. Kies het pictogram **Opname toevoegen** aan een opnamesessie starten in uw browser.

    ![Schermafbeelding van het pictogram opname toevoegen in een browser][3]

    ![Schermafbeelding van de knop opnemen in een browser][4]

3. Blader naar de Service stof-toepassing. De webaanvragen moet worden weergegeven in het deelvenster aan de opname.

    ![Schermafbeelding van het webaanvragen in het deelvenster opname][5]

4. Een reeks acties die u verwacht dat de gebruikers uitvoeren uitvoeren. Deze acties worden gebruikt als een patroon te genereren van het selectievakje laden.

5. Wanneer u klaar bent, kiest u de knop **stoppen** om te stoppen met het opnemen.

    ![Schermafbeelding van de knop stoppen][6]

    Het project .webtest in Visual Studio, moet een reeks aanvragen hebt vastgelegd. Dynamische parameters worden automatisch vervangen. Nu kunt u extra, herhaalde afhankelijkheid verzoeken die geen deel uitmaken van uw testscenario verwijderen.

6. Sla het project en kies vervolgens de opdracht **Uitvoeren testen** om de test van de web-prestaties lokaal uitvoeren en controleer of dat alles correct werkt.

    ![Schermafbeelding van de opdracht Test uitvoeren][7]

### <a name="parameterize-the-web-performance-test"></a>De web prestatietest voorzien

U kunt de web prestatietest voorzien door deze te converteren naar een gecodeerde web prestaties-toets en vervolgens de code te bewerken. Als alternatief, kunt u de web prestatietest verbinden met een lijst met gegevens zodat de test doorlopen in de gegevens. Zie [genereren en een gecodeerde web prestatietest uitvoeren](https://msdn.microsoft.com/library/ms182552.aspx) voor meer informatie over hoe u de web prestatietest converteert naar een gecodeerde test. Zie [toevoegen een gegevensbron om de prestaties van een webpagina te testen](https://msdn.microsoft.com/library/ms243142.aspx) voor informatie over het koppelen van gegevens om een web prestaties te testen.

In dit voorbeeld wordt we de web prestatietest converteren naar een gecodeerde toets, zodat u kunt de ID van de vliegtuig door een gegenereerde GUID vervangen en meer aanvragen voor het verzenden van vluchten naar verschillende locaties toevoegen.

### <a name="create-a-load-test-project"></a>Een laden testproject maken

Een project van de test laden bestaat uit een of meer scenario's beschreven door de web prestaties testen en testen van de eenheden, samen met extra opgegeven laden test-instellingen. De volgende stappen uit weergeven het maken van een project van de test laden:

1. Kies in het snelmenu van uw project webprestaties en laden Test, de optie **Add** > **Laden Test**. Kies de knop **volgende** op de toets-instellingen configureren in de wizard **Laden testen** .

2. Kies of u wilt een constant gebruiker laden of een stap laden, die begint met enkele gebruikers en verhogen van de gebruikers na verloop van tijd in de sectie **Laden** .

    Als u een goede schatting van de hoeveelheid gebruiker laden en wilt u zien hoe de huidige systeem uitvoert, kiest u **Constante laden**. Als uw doel is om te leren of het systeem voortdurend onder verschillende laadtijd uitvoert, kiest u **Stap laden**.

3. In de sectie **Mix testen** , klikt u op de knop **toevoegen** en selecteer vervolgens de test die u wilt opnemen in de test laden. U kunt de **verdeling** kolom gebruiken om op te geven het percentage van totaal tests die zijn uitgevoerd voor elke test.

4. Geef de duur van de test laden in het gedeelte **Instellingen uitvoeren** .

    >[AZURE.NOTE] De optie **Iteraties testen** is alleen beschikbaar wanneer u een lokaal gebruik van Visual Studio laden-test uitvoeren.

5. Geef in het gedeelte **locatie** van **Instellingen uitvoeren**, de locatie waar laden test verzoeken gegenereerd. De wizard mogelijk gevraagd aan te melden met uw Team Services-account. Meld u aan en kies vervolgens een geografische locatie. Wanneer u klaar bent, kiest u de knop **Voltooien** .

6. Nadat de test laden is gemaakt, opent u het project .loadtest en kiest u de huidige instelling, zoals **Instellingen uitvoeren**uitvoeren > **Settings1 uitvoeren [actieve]**. Hiermee wordt de uitvoeren instellingen in het venster **Eigenschappen** geopend.

7. In het gedeelte van de **resultaten** van het eigenschappenvenster voor **Instellingen uitvoeren** , moet de instelling **Tijdsinstellingen Details opslag** **geen** hebben als de standaardwaarde. Verander deze waarde in **Alle afzonderlijke Details** voor meer informatie over het selectievakje laden testresultaten. Zie [Laden testen](https://www.visualstudio.com/load-testing.aspx) voor meer informatie over het verbinding maken met Visual Studio Team Services en voer een test laden.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>De test laden uitvoeren met behulp van Visual Studio Team Services

Kies de opdracht **Uitvoeren laden testen** om te start de test uitvoeren.

![Schermafbeelding van de opdracht laden Test uitvoeren][8]

## <a name="view-and-analyze-the-load-test-results"></a>Bekijken en analyseren de testresultaten laden

Als het selectievakje laden testen vordert, de informatie over de prestaties grafiek wilt weergeven. U ziet er ongeveer uitzien als in de volgende grafiek.

![Schermafbeelding van prestaties graph voor testresultaten laden][9]

1. Kies de koppeling **downloadrapport** boven aan de pagina. Nadat het rapport is gedownload, klikt u op de knop **View-rapport** .

    Klik op het tabblad **grafiek** kunt u grafieken voor verschillende items zien. Klik op het tabblad **Samenvatting** de algehele testresultaten worden weergegeven. Het tabblad **tabellen** ziet u het totale aantal doorgegeven en mislukte laden tests.

2. Kies het nummer koppelingen op de **toets** > **mislukt** en de **fouten** > **aantal** kolommen om meer informatie over fout weer te geven.

    Het tabblad **detailsectie** bevat virtuele gebruiker en test scenario voor mislukte aanvragen. Deze gegevens kunnen zijn handig als de test laden meerdere scenario's bevat.

[Resultaten voor het laden van testen analyseren in de diagrammen weergave van de laden testen Analyzer](https://www.visualstudio.com/load-testing.aspx) bekijken voor meer informatie over het weergeven van testresultaten laden.

## <a name="automate-your-load-test"></a>Uw test laden automatiseren

Visual Studio Team Services laden Test biedt API's waarmee u laden tests beheren en te analyseren van resultaten in een Team Services-account. Zie [Cloud laden testen Rest-API's](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
- [Cmdlets voor controle en services in de instellingen voor een lokale computer ontwikkeling diagnose](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
