<properties 
    pageTitle="Fouten opsporen in een Java-Web-App op Azure in IntelliJ | Microsoft Azure" 
    description="Deze zelfstudie wordt getoond hoe u met de Toolkit Azure voor IntelliJ fouten opsporen in een Java-Web-App op Azure uitgevoerd." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Fouten opsporen in een Java-Web-App op Azure in IntelliJ

Deze zelfstudie leert hoe u fouten opsporen in een Java-Web-App uitvoeren op Azure met behulp van de [Azure Toolkit voor IntelliJ]. Omdat dit eenvoudiger, zult u een voorbeeld van de eenvoudige Java Server pagina (JSP) gebruiken voor deze zelfstudie, maar de stappen zou hetzelfde moeten zijn voor een Java-servlet wanneer u foutopsporing op Azure.

Wanneer u deze zelfstudie hebt voltooid, uw toepassing ziet er ongeveer uit de volgende afbeelding wanneer u deze in IntelliJ foutopsporing:

![][01]
 
## <a name="prerequisites"></a>Vereisten voor

* Een Java ontwikkelaars Kit (JDK), v 1,8 of hoger.
* IntelliJ IDEE Ultimate Edition. Dit kan worden gedownload van <https://www.jetbrains.com/idea/download/index.html>.
* Een verdeling van een webserver Java gebaseerde of toepassingsserver, zoals Apache Tomcat of Jetty.
* Een Azure-abonnement die kan worden aangeschaft uit <https://azure.microsoft.com/en-us/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor IntelliJ. Zie de [installatie van de Azure Toolkit voor IntelliJ]voor meer informatie.
* Een dynamische Web Project hebt gemaakt en geïmplementeerd in Azure App Service; Zie bijvoorbeeld [maken een Hallo wereld Web App voor Azure in IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Voor foutopsporing van een Java-Web-App op Azure

Als u wilt deze stappen in deze sectie hebt voltooid, kunt u een bestaand Project voor het maken van dynamische Web die u al hebt geïmplementeerd als een Java-Web-App op Azure, u kunt downloaden van een [Steekproef dynamische Web Project] en volg de stappen in [een Hallo wereld Web App maken voor Azure in IntelliJ] te implementeren op Azure. 

1. Open het project Java-Web-App waaraan u geïmplementeerd in Azure in IntelliJ.

1. Klik op het menu **uitvoeren** en klik vervolgens op **Configuraties bewerken**.

    ![][02]

1. Wanneer het dialoogvenster **Uitvoeren/foutopsporing configuraties** wordt geopend: 

    1. Selecteer **Azure WebApp**.
    1. Klik op **+** om toe te voegen een nieuwe configuratie.
    1. Geef een **naam** voor de configuratie.
    1. Accepteer de resterende standaardwaarden die worden voorgesteld door de Toolkit Azure en klik vervolgens op **OK**.

        ![][03]

1. Selecteer de configuratie van de Azure Web App-foutopsporing die u zojuist hebt gemaakt op de juiste bovenaan in het menu en klikt u op **fouten opsporen**

    ![][04]

1. Wanneer u wordt gevraagd naar **Foutopsporing op afstand inschakelen in de externe Web-App nu?**, klikt u op **OK**.

1. Wanneer u wordt gevraagd die **uw web-app is nu klaar voor foutopsporing op afstand**, klik op **OK**.

    ![][05]

1. Selecteer de configuratie van de Azure Web App-foutopsporing die u zojuist hebt gemaakt op de juiste bovenaan in het menu en klik vervolgens op in **fouten opsporen in**.

1. Een Windows-opdrachtprompt of Unix-shell opent en nodig verbinding voor foutopsporing; voorbereiden u moet wachten totdat de verbinding met uw externe Java-Web-app geslaagd, is voordat u verdergaat. Als u Windows gebruikt, eruitziet deze in de volgende afbeelding:

    ![][06]

1. Een punt einde invoegen in uw pagina JSP en opent u de URL voor uw Java-Web-App in een browser:

    1. Openen van **Azure Explorer** in IntelliJ.
    1. Navigeer naar de **Web Apps** en de Java Web-App die u wilt opsporen.
    1. Klik met de rechtermuisknop op de Web-App en klik op **openen in Browser**.
    1. IntelliJ treedt nu in de foutopsporingsmodus.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie [Overzicht van de Web-Apps]voor meer informatie over het maken van Azure Web Apps.

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij.md
[Installatie van de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Een Hallo wereld Web-App maken voor Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Voorbeeld dynamische Web van Project]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Overzicht van de Web-Apps]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
