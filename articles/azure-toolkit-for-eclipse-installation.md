<properties
    pageTitle="Installatie van de Azure Toolkit voor Eclips | Microsoft Azure"
    description="Informatie over het installeren van de Azure Toolkit voor Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installatie van de Azure Toolkit voor Eclips

De Toolkit Azure voor Eclips bevat sjablonen en functies kunt u gemakkelijk maken, ontwikkelen, testen en implementeren van Azure toepassingen met de ontwikkelomgeving Eclips. De Toolkit Azure voor Eclips is een bron openen project, waarvan de broncode beschikbaar onder de MIT-licentie van de site van het project op GitHub op de volgende URL is:

<https://github.com/Microsoft/Azure-Tools-for-Java>

De volgende stappen hoe u de Toolkit Azure voor Eclips installeren.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Voor het installeren van de Azure Toolkit voor Eclips

1. Eclips starten.

1. Wanneer Eclips wordt geopend, klikt u op het menu **Help** en klik vervolgens op **Nieuwe Software installeren**, zoals wordt weergegeven in de volgende afbeelding.

    ![Installatie van de Azure Toolkit voor Eclips][01]

1. Typ in het dialoogvenster **Beschikbare Software** binnen het tekstvak **werken met** **http://dl.microsoft.com/eclipse** , gevolgd door de **Enter** -toets.

1. Klik in het deelvenster **naam** **Azure Toolkit voor Eclips**controleren en schakel **contactpersoon alle sites tijdens bijwerken om te zoeken vereiste software installeren**. Het scherm moet worden de volgende strekking weergegeven:

    ![Installatie van de Azure Toolkit voor Eclips][02]

1. Als u de **Azure Toolkit voor Eclips**uitvouwen, ziet u de volgende items:

    * **Toepassing inzichten-invoegtoepassing voor Java**: dit onderdeel kunt u het gebruik van Azure telemetrielogboek logboekregistratie en analysis services voor uw toepassingen en exemplaren van de server.
    * **Azure toegang besturingselement Services Filter**: dit onderdeel biedt ondersteuning voor verifiëren van toepassingsgebruikers met Azure ACS, eenmalige aanmelding scenario's inschakelen en logica van de identiteit van de configuratietoepassing externalizing.
    * **Azure-invoegtoepassing voor algemene**: dit onderdeel biedt de algemene functionaliteit die nodig zijn voor andere onderdelen toolkit.
    * **Azure Explorer voor Eclips**: dit onderdeel biedt de algemene functionaliteit die nodig zijn voor andere onderdelen toolkit.
    * **Azure-invoegtoepassing voor Eclips met Java**: dit onderdeel biedt ondersteuning voor het ontwikkelen van projecten die te bouwen, testen en implementeren van Java-toepassingen voor de Microsoft Azure-cloud in Eclips en via de opdrachtregel.
    * **Azure Web Apps-invoegtoepassing met Java**: dit onderdeel biedt ondersteuning voor het implementeren van Java webtoepassingen op Microsoft Azure Web App containers.
    * **Microsoft JDBC stuurprogramma 4.2 voor SQL Server**: dit onderdeel biedt JDBC API voor SQL Server en Microsoft Azure SQL-Database voor Java Platform Enterprise Edition 8.
    * **Inpakken voor Apache Qpid clientbibliotheken voor JMS**: dit onderdeel voorziet in het onderdeel van de client JMS uit het project Apache Qpid uw toepassing voor gebruik met AMQP messaging in Microsoft Azure inschakelen.
    * **Pakket voor Microsoft Azure bibliotheken voor Java**: dit onderdeel biedt API's voor het werken met Microsoft Azure-services, zoals opslag, service bus, service runtime, enzovoort.

1. Klik op **volgende**. (Als u ongebruikelijke vertragingen ondervinden bij de installatie van de toolkit, zorg ervoor dat **alle sites tijdens bijwerken contactpersoon installeren als u wilt zoeken vereiste software** uitgeschakeld is.)

1. Klik op **volgende**in het dialoogvenster **Details installeren** .

    ![Controleer de installatiedetails][03]

1. Lees de voorwaarden van de licentievoorwaarden in het dialoogvenster **Licenties controleren** . Als u akkoord met de voorwaarden van de licentievoorwaarden, klikt u op **ik ga akkoord met de voorwaarden van de licentievoorwaarden** en klik vervolgens op **Voltooien**. (De overige stappen wordt ervan uitgegaan dat u Ga akkoord met de voorwaarden van de gebruiksrechtovereenkomst. Als u de voorwaarden van de gebruiksrechtovereenkomst niet accepteert, sluit u het installatieproces.)

    ![Licenties controleren][04]

    Eclips wordt download en installeer de vereiste pakketten.

    ![Voortgang van de installatie][05]

1. Als u hierom wordt gevraagd om Eclips om installatie te voltooien opnieuw te starten, klikt u op **Ja**.

    ![Start opnieuw Prompt][06]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over de Toolkits Azure voor Java IDEs, de volgende koppelingen:

- [Azure Toolkit voor Eclips]
  - *Installatie van de Azure Toolkit voor Eclips (in dit artikel)*
  - [Een Hallo wereld Web-App voor Azure in Eclips maken]
  - [Wat is er nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installatie van de Azure Toolkit voor IntelliJ]
  - [Een Hallo wereld Web-App maken voor Azure in IntelliJ]
  - [Wat is er nieuw in de Azure Toolkit voor IntelliJ]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij.md
[Een Hallo wereld Web-App voor Azure in Eclips maken]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Een Hallo wereld Web-App maken voor Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installatie van de Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Wat is er nieuw in de Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse-whats-new.md
[Wat is er nieuw in de Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

