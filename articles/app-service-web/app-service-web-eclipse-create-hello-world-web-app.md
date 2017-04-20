<properties 
    pageTitle="Een Hallo wereld Web-App maken voor Azure in Eclips | Microsoft Azure" 
    description="Deze zelfstudie ziet u hoe u de Toolkit Azure voor Eclips gebruiken om te maken van een Hallo wereld Web App voor Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Een Hallo wereld Web-App voor Azure in Eclips maken

Deze zelfstudie leert hoe u maken en implementeren van een eenvoudige Hallo allemaal toepassing Azure als een Web-App met behulp van de [Azure Toolkit voor Eclips]. Een voorbeeld van de eenvoudige JSP voor eenvoudig wordt weergegeven, maar uiterst dezelfde stappen wordt geschikt te maken voor een servlet Java wat betreft Azure-implementatie is.

Wanneer u deze zelfstudie hebt voltooid, uw toepassing ziet er ongeveer uit de volgende afbeelding wanneer u deze in een webbrowser bekijken:

![Voorbeeld van Hallo wereld-app][01]
 
## <a name="prerequisites"></a>Vereisten voor

* Een Java ontwikkelaars Kit (JDK), v 1,8 of hoger.
* IDE Eclips voor Java EE ontwikkelaars, standaard of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.
* Een verdeling van een webserver Java gebaseerde of toepassingsserver, zoals Apache Tomcat of Jetty.
* Een Azure-abonnement die kan worden aangeschaft uit <https://azure.microsoft.com/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor Eclips. Zie de [installatie van de Azure Toolkit voor Eclips]voor meer informatie.

## <a name="to-create-a-hello-world-application"></a>Een toepassing van de wereld Hallo maken

Eerst beginnen we uitschakelen met het maken van een project Java.

1. Eclips, starten en klik op het menu **bestand**, klikt u op **Nieuw**en klik vervolgens op **Dynamische Web Project**. (Als u **Dynamische Web Project** vermeld als een beschikbare project na het klikken op **bestand** en **Nieuw**niet ziet, klikt u vervolgens het volgende doen: klik op **bestand**, klikt u op **Nieuw**, klik op **Project...**, **Web**uitvouwen, klik op **Dynamische Web Project**en klik op **volgende**.)

1. Geef het project **MyWebApp**voor toepassing van deze zelfstudie wordt. Uw scherm ziet er ongeveer als volgt uit:

    ![Een nieuwe dynamische Web Project te maken][02]

1. Klik op **Voltooien**.

1. Uitvouwen **MyWebApp**binnen de Eclips Projectverkenner weergave. Met de rechtermuisknop op **webinhoud**, klikt u op **Nieuw**en klik vervolgens op **JSP-bestand**.

1. In het dialoogvenster **Nieuw JSP-bestand** , de naam van het bestand **index.jsp**, de bovenliggende map als **MyWebApp/webinhoud**behouden en klik vervolgens op **volgende**.

1. Klik in het dialoogvenster **Selecteer JSP sjabloon** voor toepassing van deze zelfstudie selecteert u **Nieuw JSP-bestand (html)**en klik vervolgens op **Voltooien**.

1. Wanneer uw index.jsp-bestand wordt geopend in Eclips, toevoegen aan tekst om weer te geven dynamisch **Hallo wereld!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud ziet er dan het volgende voorbeeld:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Sla index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Uw toepassing aan een Container Azure Web-App implementeren

Er zijn verschillende manieren waarop u een webtoepassing Java Azure kunt implementeren. Deze zelfstudie wordt beschreven in een van de eenvoudigste: uw toepassing wordt geïmplementeerd in een Container Azure Web App - geen speciale projecttype of aanvullende hulpmiddelen voor nodig zijn. De JDK en de web container software krijgt u door Azure, zodat er geen nodig is voor het uploaden van uw eigen; hoeft u uw Java-Web-App is. Hierdoor wordt het publicatieproces voor uw toepassing seconden, niet minuten duren.

1. Projectverkenner van Eclips, met de rechtermuisknop op **MyWebApp**.

1. In het contextmenu **Azure**selecteren en klik op **publiceren als Azure Web App..**

    ![Publiceren als Azure WebApp][03]
   
    U kunt ook terwijl uw web application-project is geselecteerd in de Projectverkenner, kunt u op de knop **publiceren** vervolgkeuzelijst op de werkbalk en selecteer **publiceren als Azure Web App** vanaf hier:
   
    ![Publiceren als Azure WebApp][14]
   
1. Als u hebt nog niet aangemeld bij Azure uit Eclips, wordt u gevraagd u zich aanmeldt bij uw Azure-account:

    ![Azure dialoogvenster Aanmelden][04]
   
    Als u meerdere Azure-accounts hebt, kan enkele van de aanwijzingen tijdens het aanmelden proces worden weergegeven meer dan één keer, zelfs als ze lijken moeten overeenkomen. Als dit gebeurt, gaat u verder na het teken in de instructies.

1. Nadat u bent aangemeld bij uw Azure-account, wordt een lijst met abonnementen die zijn gekoppeld aan uw referenties weergegeven in het dialoogvenster **Abonnementen beheren** . Als er meerdere abonnementen vermeld en u wilt werken met alleen een specifiek deel van deze, kunt u de kleuren die u wilt gebruiken (optioneel) uitschakelen. Wanneer u uw abonnementen hebt geselecteerd, klikt u op **sluiten**.

    ![Dialoogvenster abonnementen beheren][05]
   
1. Als het dialoogvenster **distribueren aan Azure Web App Container** wordt weergegeven, wordt weergegeven een Web App-containers die u eerder hebt gemaakt. Als u geen containers niet hebt gemaakt, wordt de lijst niet leeg zijn.

    ![Naar het dialoogvenster Azure Web App Container implementeren][06]
   
1. Als u een Container Azure Web App voordat u niet hebt gemaakt, of als u wilt publiceren van uw toepassing aan een nieuwe container, gebruikt u de volgende stappen uit. Anders, selecteert u een bestaande Container van de Web-App en gaat u verder met stap 7 hieronder.

    1. Klik op **nieuwe...**

        ![Naar het dialoogvenster Azure Web App Container implementeren][15]

    1. Het dialoogvenster **Nieuwe Web App Container** wordt weergegeven:

        ![Het dialoogvenster Nieuw Web App Container][07a]

    1. Typ een **DNS-Label** voor de Container van uw Web-App. Hiermee wordt het label van de DNS-knooppuntniveau van de host-URL voor uw webtoepassing formulier in Azure wordt aangegeven. (Houd er rekening mee dat de naam moet beschikbaar en voldoen aan Azure Web App naamgevingsvereisten.)

    1. Selecteer de betreffende software voor uw toepassing in het vervolgkeuzemenu **Web Container** .

        Op dit moment kunt u kiezen uit Tomcat 8, Tomcat 7 of Jetty 9. Een recente verdeling van de geselecteerde software worden geleverd door Azure en deze kan worden uitgevoerd op een recente verdeling van JDK 8 gemaakt door Oracle en Azure gekregen.

    1. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement dat u wilt gebruiken voor deze installatie.

    1. Selecteer de resourcegroep die u wilt koppelen van uw Web-App in de vervolgkeuzelijst **Resourcegroep** . (Azure resourcegroepen kunt u verwante resources om samen te groeperen zodat, bijvoorbeeld ze elkaar kunnen worden verwijderd.)

        U kunt een bestaande resourcegroep (als u een hebt) en overslaan naar stap g onderstaande selecteren of gebruikt u de volgende deze stappen uit als u wilt een nieuwe resourcegroep maken:

        * Klik op **nieuwe...**

        * Het dialoogvenster **Nieuwe resourcegroep** wordt weergegeven:

            ![Het dialoogvenster nieuwe resourcegroep][08]

        * In de **het tekstvak,** Geef een naam voor de nieuwe Resource-groep.

        * In de de **regio** vervolgkeuzelijst, selecteer de juiste Azure gegevens centreren locatie voor de resourcegroep.

        * Optioneel: Standaard een recente verdeling van Java 8 wordt geïmplementeerd door Azure automatisch in uw web-app container als uw JVM. U kunt echter een andere versie- en distributiegroepen van JVM opgeven als uw Web-App vereist. Als u de JDK voor uw Web-App, klik op het tabblad **JDK** en selecteer een van de volgende opties:

            * **Deploy standaard JDK aangeboden door Azure Web Apps-service**: deze optie wordt de recente verdeling van Java 8 implementeren.

            * **Een 3e derden beschikbaar op Azure JDK Deploy**: deze optie kunt u kiezen uit de lijst met JDKs die door Microsoft Azure.

            * **Deploy mezelf JDK vanaf deze downloadlocatie**: deze optie kunt u opgeven van uw eigen JDK-verdeling gebruikt, die moet worden geleverd als een ZIP-bestand en kan worden geüpload naar een openbaar downloadlocatie of een Azure opslag-account waarvoor u toegang hebt.

            ![Het dialoogvenster Nieuw Web App Container][07b]

    * Klik op **OK**.

    1. Het vervolgkeuzemenu **App-Service plannen** bevat de app-service-abonnementen die zijn gekoppeld aan de resourcegroep die u hebt geselecteerd. (App Service-abonnementen opgeven toegangsgegevens voor de locatie van uw Web-App, de prijzen laag en de grootte van de exemplaar berekeningscluster. Een enkel App-Service plannen kan worden gebruikt voor meerdere Web-Apps, dat wil zeggen waarom deze afzonderlijk worden onderhouden van een specifieke Web App-implementatie.)

        U kunt een bestaand App-Service plannen (als u een hebt) en overslaan naar stap h onderstaande selecteren of gebruikt u de volgende deze stappen uit als u wilt maken van een nieuwe App-Service plannen:

        * Klik op **nieuwe...**

        * Het dialoogvenster **Nieuwe App-Service plannen** wordt weergegeven:

            ![Nieuw dialoogvenster voor het App-Service plannen][09]

        * In de **het tekstvak,** Geef een naam voor uw nieuwe App-Service plannen.

        * In de de **locatie** vervolgkeuzelijst, selecteer de juiste Azure gegevens centreren locatie voor het abonnement.

        * In de de **Prijzen laag** vervolgmenu, selecteert u de juiste prijs voor het abonnement. U kunt **gratis**kiezen voor testdoeleinden.

        * In de het **Exemplaar grootte** vervolgkeuzelijst, selecteer het exemplaar van de juiste grootte voor het abonnement. U kunt **kleine**kiezen voor testdoeleinden.

    1. Als u alle bovenstaande stappen hebt voltooid, ziet het dialoogvenster Nieuwe Web App Container in de volgende afbeelding:

        ![Het dialoogvenster Nieuw Web App Container][10]

    1. Klik op **OK** om te voltooien van het maken van uw nieuwe Web App-container.

        Wacht een paar seconden voor de lijst van de Web App-containers worden vernieuwd en de container gemaakte web-app zijn nu geselecteerd in de lijst.

1. U bent nu klaar om te voltooien van de eerste installatie van uw Web-App naar Azure:

    ![Naar het dialoogvenster Azure Web App Container implementeren][11]

    Klik op **OK** als u wilt uw Java-toepassing aan de geselecteerde Web App-container implementeren.

    De toepassing wordt standaard als een submap van de toepassingsserver geïmplementeerd. Als u deze worden geïmplementeerd als de hoofdmap-toepassing wilt, schakelt u het selectievakje **distribueren naar de hoofdsite** voordat u op **OK**.

1. Vervolgens ziet u de weergave **Azure activiteit Log** , die de implementatiestatus van uw Web-App wordt aangegeven.

    ![Azure activiteit Log][12]

    Het proces van het implementeren van uw Web-App naar Azure moet slechts een paar seconden duren. Wanneer de hand van de toepassing, ziet u een koppeling met de naam **Published** in de kolom **Status** . Als u de koppeling klikt, gaat deze u naar de startpagina van uw geïmplementeerd Web-App.

## <a name="updating-your-web-app"></a>Bijwerken van uw Web-App

Een snel en eenvoudig proces bijwerken van een bestaande Azure Web App actief is en u hebt twee opties voor het bijwerken van:

* U kunt de implementatie van een bestaande Java-Web-App kunt bijwerken.
* U kunt een extra Java-toepassing aan dezelfde Web App Container publiceren.

In beide gevallen wordt het proces identiek is en duurt slechts een paar seconden:

1. Met de rechtermuisknop op de Java-toepassing die u wilt bijwerken of toevoegen aan een bestaande Container van de Web-App in de Projectverkenner Eclips.

1. Wanneer het contextmenu wordt weergegeven, selecteert u **Azure** en klik vervolgens op **publiceren als Azure Web App..**

1. Aangezien u hebt al geregistreerd eerder, ziet u een lijst met uw bestaande Web App-containers. Selecteer het account dat u wilt publiceren of opnieuw publiceren naar uw Java-toepassing en klik op **OK**.

Een paar seconden later, de weergave van de **Azure activiteit Log** uw bijgewerkte implementatie als **Published** worden weergegeven en is mogelijk om te controleren of uw bijgewerkte toepassing in een webbrowser.

## <a name="stopping-an-existing-web-app"></a>Een bestaande Web-App stoppen

Stoppen met een bestaande Azure Web App container, (inclusief alle gebruikte Java-toepassingen erin), kunt u de weergave van de **Azure Verkenner** .

Als de weergave **Azure Explorer** nog niet is geopend, kunt u openen door te klikken op vervolgens **venster** in Eclips, en klik op **Weergave weergeven**en vervolgens **Overige...** **Azure**en klik op **Azure Explorer**. Als u eerder niet hebt aangemeld, worden de volgende kunt doen.

Wanneer de weergave **Azure Explorer** wordt weergegeven, gebruiken als volgt te werk om te stoppen uw Web-App: 

1. Vouw het knooppunt **Azure** .

1. Vouw het knooppunt **Web Apps** . 

1. Met de rechtermuisknop op de gewenste Web-App.

1. Als het snelmenu wordt weergegeven, klikt u op **stoppen**.

    ![Een bestaande Web-App stoppen][13]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Toolkits Azure voor Java IDEs, de volgende koppelingen:

- [Azure Toolkit voor Eclips]
  - [Installatie van de Azure Toolkit voor Eclips]
  - *Een Hallo wereld Web-App maken voor Azure in Eclips (in dit artikel)*
  - [Wat is er nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installatie van de Azure Toolkit voor IntelliJ]
  - [Een Hallo wereld Web-App maken voor Azure in IntelliJ]
  - [Wat is er nieuw in de Azure Toolkit voor IntelliJ]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie [Overzicht van de Web-Apps]voor meer informatie over het maken van Azure Web Apps.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Een Hallo wereld Web-App maken voor Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Installatie van de Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse-installation.md
[Installatie van de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Wat is er nieuw in de Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse-whats-new.md
[Wat is er nieuw in de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Overzicht van de Web-Apps]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
