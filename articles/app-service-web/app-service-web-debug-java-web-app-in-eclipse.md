<properties 
    pageTitle="Fouten opsporen in een Java-Web-App op Azure in Eclips | Microsoft Azure" 
    description="Deze zelfstudie wordt getoond hoe u met de Toolkit Azure voor Eclips fouten opsporen in een Java-Web-App op Azure uitgevoerd." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Fouten opsporen in een Java-Web-App op Azure in Eclips

Deze zelfstudie leert hoe u fouten opsporen in een Java-Web-App op Azure met behulp van de [Azure Toolkit voor Eclips]uitgevoerd. Omdat dit eenvoudiger, zult u een voorbeeld van de eenvoudige Java Server pagina (JSP) gebruiken voor deze zelfstudie, maar de stappen zou hetzelfde moeten zijn voor een Java-servlet wanneer u foutopsporing op Azure.

Wanneer u deze zelfstudie hebt voltooid, uw toepassing ziet er ongeveer uit de volgende afbeelding wanneer u deze in Eclips foutopsporing:

![][01]
 
## <a name="prerequisites"></a>Vereisten voor

* Een Java ontwikkelaars Kit (JDK), v 1,8 of hoger.
* IDE Eclips voor Java EE ontwikkelaars, Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.
* Een verdeling van een webserver Java gebaseerde of toepassingsserver, zoals Apache Tomcat of Jetty.
* Een Azure-abonnement die kan worden aangeschaft uit <https://azure.microsoft.com/en-us/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor Eclips. Zie de [installatie van de Azure Toolkit voor Eclips]voor meer informatie.
* Een dynamische Web Project hebt gemaakt en geïmplementeerd in Azure App Service; Zie bijvoorbeeld [maken een Hallo wereld Web App voor Azure in Eclips].

## <a name="to-debug-a-java-web-app-on-azure"></a>Voor foutopsporing van een Java-Web-App op Azure

Als u wilt deze stappen in deze sectie hebt voltooid, kunt u een bestaand Project voor het maken van dynamische Web die u al hebt geïmplementeerd als een Java-Web-App op Azure, u kunt downloaden van een [Steekproef dynamische Web Project] en volg de stappen in [een Hallo wereld Web App maken voor Azure in Eclips] te implementeren op Azure. 

1. Open Eclips.

1. Configureer time-outs voor foutopsporing op afstand:

    1. Klik op het **Windows** -menu in Eclips en klik vervolgens op **Voorkeuren**.
    1. Vouw het knooppunt **Java** uit en selecteer **fouten opsporen in**.
    1. Instellingen configureren voor zowel de **time-out voor foutopsporing ([ms)** en **time-out ([ms) starten** naar `120000`.

        ![][02]

    1. Klik op **OK** om het dialoogvenster **Voorkeuren** te sluiten.

1. Klik in de weergave van Eclips Projectverkenner met de rechtermuisknop op de dynamische Web Project dat u hebt geïmplementeerd in Azure. Wanneer het contextmenu wordt weergegeven, selecteert u **Fouten opsporen in als**en klik op **Azure Web App**.

    ![][03]

1. Als dit de eerste keer dat u uw dynamische Web Project fouten wilt opsporen, wordt het dialoogvenster **Foutopsporing configuraties** wordt geopend. u kunt de standaardwaarden die zijn opgegeven door de Toolkit op het tabblad **verbinding maken met** accepteren. Klik op het tabblad **bron** Klik op **toevoegen**en vervolgens **Java project**, selecteert u **Dynamische Web Project**en klik vervolgens op **OK**. Als u deze stappen hebt voltooid, klikt u op **fouten opsporen in**.

    ![][04]

1. Wanneer u wordt gevraagd naar **Foutopsporing op afstand inschakelen in de externe Web-App nu?**, klikt u op **OK**.

1. Wanneer u wordt gevraagd die **uw web-app is nu klaar voor foutopsporing op afstand**, klik op **OK**.

    ![][05]

1. Wanneer het dialoogvenster **Foutopsporing configuraties** opnieuw wordt weergegeven, klikt u op **fouten opsporen in**.

1. Een Windows-opdrachtprompt of Unix-shell opent en nodig verbinding voor foutopsporing; voorbereiden u moet wachten totdat de verbinding met uw externe Java-Web-app geslaagd, is voordat u verdergaat. Als u Windows gebruikt, eruitziet deze in de volgende afbeelding.

    ![][06]

1. Een punt einde invoegen in uw pagina JSP en opent u de URL voor uw Java-Web-App in een browser:

    1. Openen van **Azure Explorer** in Eclips.
    1. Navigeer naar de **Web Apps** en de Java Web-App die u wilt opsporen.
    1. Klik met de rechtermuisknop op de Web-App en klik op **openen in Browser**.
    1. Eclips treedt nu in de foutopsporingsmodus.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie [Overzicht van de Web-Apps]voor meer informatie over het maken van Azure Web Apps.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse.md
[Installatie van de Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse-installation.md
[Een Hallo wereld Web-App voor Azure in Eclips maken]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Voorbeeld dynamische Web van Project]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Overzicht van de Web-Apps]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
