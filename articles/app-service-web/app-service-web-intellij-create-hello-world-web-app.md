<properties 
    pageTitle="Een Hallo wereld Web-App maken voor Azure in IntelliJ | Microsoft Azure" 
    description="Deze zelfstudie ziet u hoe u de Toolkit Azure voor IntelliJ gebruiken om te maken van een Hallo wereld Web App voor Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Een Hallo wereld Web-App maken voor Azure in IntelliJ

Deze zelfstudie leert hoe u maken en implementeren van een eenvoudige Hallo allemaal toepassing Azure als een Web-App met behulp van de [Azure Toolkit voor IntelliJ]. Een voorbeeld van de eenvoudige JSP voor eenvoudig wordt weergegeven, maar uiterst dezelfde stappen wordt geschikt te maken voor een servlet Java wat betreft Azure-implementatie is.

Wanneer u deze zelfstudie hebt voltooid, uw toepassing ziet er ongeveer uit de volgende afbeelding wanneer u deze in een webbrowser bekijken:

![][01]
 
## <a name="prerequisites"></a>Vereisten voor

* Een Java ontwikkelaars Kit (JDK), v 1,8 of hoger.
* IntelliJ IDEE Ultimate Edition. Dit kan worden gedownload van <https://www.jetbrains.com/idea/download/index.html>.
* Een verdeling van een webserver Java gebaseerde of toepassingsserver, zoals Apache Tomcat of Jetty.
* Een Azure-abonnement die kan worden aangeschaft uit <https://azure.microsoft.com/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor IntelliJ. Zie de [installatie van de Azure Toolkit voor IntelliJ]voor meer informatie.

## <a name="to-create-a-hello-world-application"></a>Een toepassing van de wereld Hallo maken

Eerst beginnen we uitschakelen met het maken van een project Java.

1. IntelliJ, starten en klik op het menu **bestand**en klik op **Nieuw**en klik vervolgens op **Project**.

   ![][02]

1. Klik in het dialoogvenster Nieuw Project Selecteer **Java**, klikt u vervolgens **Webtoepassing**, en klik op **volgende**.

   ![][03a]

   Als u hierom wordt gevraagd om door te gaan met geen SDK die zijn toegewezen, klikt u op **Ja**.

   ![][03b]

1. Naam van het project **Java-Web-App-op-Azure**voor toepassing van deze zelfstudie, en klik vervolgens op **Voltooien**.

   ![][04]

1. Binnen de IntelliJ Projectverkenner weergave, vouw **Java-Web-App-op-Azure**, en vervolgens uitvouwen **web**en dubbelklik vervolgens op **index.jsp**.

   ![][05]

1. Wanneer uw index.jsp-bestand wordt geopend in IntelliJ, toevoegen aan tekst om weer te geven dynamisch **Hallo wereld!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud ziet er dan het volgende voorbeeld:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Sla index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Uw toepassing aan een Container Azure Web-App implementeren

Er zijn verschillende manieren waarop u een webtoepassing Java Azure kunt implementeren. Deze zelfstudie wordt beschreven in een van de eenvoudigste: uw toepassing wordt geïmplementeerd in een Container Azure Web App - geen speciale projecttype of aanvullende hulpmiddelen voor nodig zijn. De JDK en de web container software krijgt u door Azure, zodat er geen nodig is voor het uploaden van uw eigen; hoeft u uw Java-Web-App is. Hierdoor wordt het publicatieproces voor uw toepassing seconden, niet minuten duren.

1. Projectverkenner van IntelliJ, met de rechtermuisknop op het project **Java-Web-App-op-Azure** . Wanneer het contextmenu wordt weergegeven, selecteert u **Azure**en klik op **publiceren als Azure Web App..**

   ![][06]

1. Als u hebt nog niet aangemeld bij Azure uit IntelliJ, wordt u gevraagd u zich aanmeldt bij uw Azure-account:

   ![][07]

   Opmerking: Als u meerdere Azure-accounts hebt, enkele van de aanwijzingen tijdens het aanmelden proces kan worden weergegeven meer dan één keer, zelfs als ze lijken moeten overeenkomen. Als dit gebeurt, gaat u verder na het teken in de instructies.

1. Nadat u bent aangemeld bij uw Azure-account, wordt een lijst met abonnementen die zijn gekoppeld aan uw referenties weergegeven in het dialoogvenster **Abonnementen beheren** . Als er meerdere abonnementen vermeld en u wilt werken met alleen een specifiek deel van deze, kunt u de kleuren die u wilt gebruiken (optioneel) uitschakelen. Wanneer u uw abonnementen hebt geselecteerd, klikt u op **sluiten**.

   ![][08]

1. Als het dialoogvenster **distribueren aan Azure Web App Container** wordt weergegeven, wordt weergegeven een Web App-containers die u eerder hebt gemaakt. Als u geen containers niet hebt gemaakt, wordt de lijst niet leeg zijn.   

   ![][09]

1. Als u een Container Azure Web App voordat u niet hebt gemaakt, of als u wilt publiceren van uw toepassing aan een nieuwe container, gebruikt u de volgende stappen uit. Anders, selecteert u een bestaande Container van de Web-App en gaat u verder met stap 6 hieronder.

  1. Klik op**+**

        ![][10]

  1. Het dialoogvenster **Nieuwe Web App Container** verschijnt, dat u wilt gebruiken voor de volgende enkele stappen uitvoeren.

        ![][11]

  1. Typ een **DNS-Label** voor de Container van uw Web-App. Hiermee wordt het label van de DNS-knooppuntniveau van de host-URL voor uw webtoepassing formulier in Azure wordt aangegeven. Opmerking: De naam moet beschikbaar en voldoen aan Azure Web App naamgevingsvereisten.

  1. Selecteer de betreffende software voor uw toepassing in het vervolgkeuzemenu **Web Container** .

        Op dit moment kunt u kiezen uit Tomcat 8, Tomcat 7 of Jetty 9. Een recente verdeling van de geselecteerde software worden geleverd door Azure en deze kan worden uitgevoerd op een recente verdeling van JDK 8 gemaakt door Oracle en Azure gekregen.

  1. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement dat u wilt gebruiken voor deze installatie.

  1. Selecteer de resourcegroep die u wilt koppelen van uw Web-App in de vervolgkeuzelijst **Resourcegroep** .

        Opmerking: Azure resourcegroepen kunt u groeperen verwante resources zodat, bijvoorbeeld ze elkaar kunnen worden verwijderd.

        U kunt een bestaande resourcegroep (als u een hebt) en overslaan naar stap g onderstaande selecteren of gebruikt u de volgende deze stappen uit als u wilt een nieuwe resourcegroep maken:

      * Klik op **nieuwe...**

      * Het dialoogvenster **Nieuwe resourcegroep** wordt weergegeven:

            ![][12]

      * In de **het tekstvak,** Geef een naam voor de nieuwe Resource-groep.

      * In de de **regio** vervolgkeuzelijst, selecteer de juiste Azure gegevens centreren locatie voor de resourcegroep.

      * Klik op **OK**.

  1. Het vervolgkeuzemenu **App-Service plannen** bevat de app-service-abonnementen die zijn gekoppeld aan de resourcegroep die u hebt geselecteerd.

        Opmerking: Een App-Service plannen geeft informatie zoals de locatie van uw Web-App, de prijzen laag en de grootte van de exemplaar berekeningscluster. Een enkel App-Service plannen kan worden gebruikt voor meerdere Web-Apps, dat wil zeggen waarom deze afzonderlijk worden onderhouden van een specifieke Web App-implementatie.

        U kunt een bestaand App-Service plannen (als u een hebt) en overslaan naar stap h onderstaande selecteren of gebruikt u de volgende deze stappen uit als u wilt maken van een nieuwe App-Service plannen:

      * Klik op **nieuwe...**

      * Het dialoogvenster **Nieuwe App-Service plannen** wordt weergegeven:

            ![][13]

      * In de **het tekstvak,** Geef een naam voor uw nieuwe App-Service plannen.

      * In de de **locatie** vervolgkeuzelijst, selecteer de juiste Azure gegevens centreren locatie voor het abonnement.

      * In de de **Prijzen laag** vervolgmenu, selecteert u de juiste prijs voor het abonnement. U kunt **gratis**kiezen voor testdoeleinden.

      * In de het **Exemplaar grootte** vervolgkeuzelijst, selecteer het exemplaar van de juiste grootte voor het abonnement. U kunt **kleine**kiezen voor testdoeleinden.

  1. Als u alle bovenstaande stappen hebt voltooid, ziet het dialoogvenster Nieuwe Web App Container in de volgende afbeelding:

        ![][14]

  1. Klik op **OK** om te voltooien van het maken van uw nieuwe Web App-container.

        Wacht een paar seconden voor de lijst van de Web App-containers worden vernieuwd en de container gemaakte web-app zijn nu geselecteerd in de lijst.

1. U bent nu klaar om te voltooien van de eerste installatie van uw Web-App naar Azure; Klik op **OK** als u wilt uw Java-toepassing aan de geselecteerde Web App-container implementeren.

    ![][15]

    Opmerking: Standaard uw toepassing wordt geïmplementeerd als een submap van de toepassingsserver. Als u deze worden geïmplementeerd als de hoofdmap-toepassing wilt, schakelt u het selectievakje **distribueren naar de hoofdsite** voordat u op **OK**.

1. Vervolgens ziet u de weergave **Azure activiteit Log** , die de implementatiestatus van uw Web-App wordt aangegeven.

    ![][16]

    Het proces van het implementeren van uw Web-App naar Azure moet slechts een paar seconden duren. Wanneer de hand van de toepassing, ziet u een koppeling met de naam **Published** in de kolom **Status** . Als u de koppeling klikt, deze gaat u naar de startpagina van uw geïmplementeerd Web-App of kunt u de stappen in de volgende sectie om te bladeren naar uw web-app.

## <a name="browsing-to-your-web-app-on-azure"></a>Bladeren naar uw Web-App op Azure

Naar de browser naar uw Web-App op Azure, kunt u de weergave van de **Azure Verkenner** .

Als de weergave **Azure Explorer** nog niet is geopend, kunt u deze openen door te klikken op vervolgens menu **Beeld** in IntelliJ, en vervolgens klikt u op **Hulpprogramma Windows**en klik op **Service Explorer**. Als u eerder niet hebt aangemeld, worden de volgende kunt doen.

Wanneer de weergave **Azure Explorer** wordt weergegeven, gebruiken als volgt te werk om te stoppen uw Web-App: 

1. Vouw het knooppunt **Azure** .

1. Vouw het knooppunt **Web Apps** . 

1. Met de rechtermuisknop op de gewenste Web-App.

1. Als het snelmenu wordt weergegeven, klikt u op **openen in Browser**.

    ![][17]

## <a name="updating-your-web-app"></a>Bijwerken van uw Web-App

Een snel en eenvoudig proces bijwerken van een bestaande Azure Web App actief is en u hebt twee opties voor het bijwerken van:

* U kunt de implementatie van een bestaande Java-Web-App kunt bijwerken.
* U kunt een extra Java-toepassing aan dezelfde Web App Container publiceren.

In beide gevallen wordt het proces identiek is en duurt slechts een paar seconden:

1. Met de rechtermuisknop op de Java-toepassing die u wilt bijwerken of toevoegen aan een bestaande Container van de Web-App in de Projectverkenner IntelliJ.

1. Wanneer het contextmenu wordt weergegeven, selecteert u **Azure** en klik vervolgens op **publiceren als Azure Web App..**

1. Aangezien u hebt al geregistreerd eerder, ziet u een lijst met uw bestaande Web App-containers. Selecteer het account dat u wilt publiceren of opnieuw publiceren naar uw Java-toepassing en klik op **OK**.

Een paar seconden later, de weergave van de **Azure activiteit Log** uw bijgewerkte implementatie als **Published** worden weergegeven en is mogelijk om te controleren of uw bijgewerkte toepassing in een webbrowser.

## <a name="starting-or-stopping-an-existing-web-app"></a>Starten of stoppen van een bestaande Web-App

Als u wilt starten of stoppen van een bestaande Azure Web App-container, (inclusief alle gebruikte Java-toepassingen erin), kunt u de weergave van de **Azure Verkenner** .

Als de weergave **Azure Explorer** nog niet is geopend, kunt u deze openen door te klikken op vervolgens menu **Beeld** in IntelliJ, en vervolgens klikt u op **Hulpprogramma Windows**en klik op **Service Explorer**. Als u eerder niet hebt aangemeld, worden de volgende kunt doen.

Wanneer de weergave **Azure Explorer** wordt weergegeven, gebruiken als volgt te werk om te starten of stoppen uw Web-App: 

1. Vouw het knooppunt **Azure** .

1. Vouw het knooppunt **Web Apps** . 

1. Met de rechtermuisknop op de gewenste Web-App.

1. Als het snelmenu wordt weergegeven, klikt u op **starten** of **stoppen**. Houd er rekening mee dat de menuopties weet context-, zodat u kunt alleen een actieve WebApp stoppen of starten van een web-app dat niet actief is.

    ![][18]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Toolkits Azure voor Java IDEs, de volgende koppelingen:

- [Azure Toolkit voor Eclips]
  - [Installatie van de Azure Toolkit voor Eclips]
  - [Een Hallo wereld Web-App voor Azure in Eclips maken]
  - [Wat is er nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installatie van de Azure Toolkit voor IntelliJ]
  - *Een Hallo wereld Web-App maken voor Azure in IntelliJ (in dit artikel)*
  - [Wat is er nieuw in de Azure Toolkit voor IntelliJ]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie [Overzicht van de Web-Apps]voor meer informatie over het maken van Azure Web Apps.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij.md
[Een Hallo wereld Web-App voor Azure in Eclips maken]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Installatie van de Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse-installation.md
[Installatie van de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Wat is er nieuw in de Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse-whats-new.md
[Wat is er nieuw in de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Overzicht van de Web-Apps]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
