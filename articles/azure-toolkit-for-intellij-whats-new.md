<properties
    pageTitle="Wat is er nieuw in de Azure Toolkit voor IntelliJ | Microsoft Azure"
    description="Meer informatie over de nieuwste functies in de Toolkit Azure voor IntelliJ."
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
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Wat is er nieuw in de Azure Toolkit voor IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure Toolkit voor IntelliJ versies

In dit artikel bevat informatie over de verschillende versies en de meest recente updates voor de Toolkit Azure voor IntelliJ.

> [AZURE.NOTE] Er is ook een Toolkit Azure voor de IDE Eclips. Zie [Azure Toolkit voor Eclips]voor meer informatie.

### <a name="august-26-2016"></a>26 augustus 2016

De Toolkit Azure voor IntelliJ - augustus 2016 release bevat de volgende verbeteringen:

* **Aangepaste JDK onderzoeken**. De Toolkit Azure voor IntelliJ ondersteunt nu opgeven en implementeren van een willekeurige JDK versie aan uw Azure WebApp container:
  - Naast de JDKs verstrekt door Azure, kunt u ook kiezen uit een breed selectie van Zulu OpenJDK versies beschikbaar gemaakt op Azure door Azul Systems.
  - Als u deze als een zipbestand naar uw account opslag uploaden, kunt u ook uw eigen verdeling JDK opgeven.
* **Verbeteringen in de weergave van de Azure Verkenner**:
  - Ondersteuning voor VM management met de nieuwe Resource Manager-model van Azure: u kunt een lijst met, maken en resources op basis van een manager virtuele machines verwijderen zonder de IDE.
  - Ondersteuning voor opslag-Account blob beheren met behulp van Azure Resource Manager, dat is een aanvulling op de bestaande functionaliteit voor het beheren van "klassieke" opslag-accounts.
* **Microsoft JDBC stuurprogramma 6.0 voor SQL Server**. Deze update bevat de meest recente JDBC-stuurprogramma voor Microsoft SQL Server (v6.0), dat wil zeggen dat is opgenomen als een bibliotheek waarin u eenvoudig aan uw projecten Java toevoegen kunt, daarmee ook de oudere versie vervangen.

### <a name="june-29-2016"></a>29 juni 2016

De Toolkit Azure voor IntelliJ - juni 2016 release bevat de volgende verbeteringen:

* **Het vereiste voor Java 8**. De Toolkit Azure voor IntelliJ moet nu Java 8, hoewel deze vereiste alleen voor de toolkit geldt-uw toepassingen kunnen blijven alle versies van Java die worden ondersteund door Azure gebruiken.
* **Ondersteuning voor de meest recente Java-JDKs**. De meest recente versies van de Java-JDKs worden nu ondersteund door de Toolkit Azure voor IntelliJ.
* **Ondersteuning voor Azure SDK v2.9.1**. De meest recente versie van de Azure SDK is nu de minimale spelen voor de Toolkit Azure voor IntelliJ.
* **Geïntegreerde voorbeelden**. De Toolkit Azure voor IntelliJ biedt nu verschillende steekproeven toepassingen om ontwikkelaars aan de slag te helpen.
* **Hulpmiddel HDInsight-integratie**. Van Azure HDInsight extra zijn nu samengevoegd met de Toolkit Azure voor IntelliJ. Zie [HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ]voor meer informatie.
* **Externe foutopsporing van Java WebApps**. De Toolkit Azure voor IntelliJ ondersteunt nu foutopsporing op afstand van Java-web-apps op Azure App-Service.

### <a name="april-12-2016"></a>12 april 2016

De Toolkit Azure voor IntelliJ - 2016 in April release bevat de volgende verbeteringen:

* **Ondersteuning voor Azure SDK v2.9.0**. De meest recente versie van de Azure SDK is nu de minimale spelen voor de Toolkit Azure voor IntelliJ.
* **Diverse bruikbaarheid, serverreactie- en prestatieverbeteringen die betrekking hebben op Azure Web App-ondersteuning**. Een aantal prestatieverbeteringen in hoe de Toolkit met Azure resulteren communiceert in de gebruikersinterface van een meer heeft gereageerd.
* **Mogelijkheid om te verwijderen van een bestaande webtoepassing container in Azure wordt aangegeven uit binnen IntelliJ**. De Toolkit Azure voor IntelliJ nu kunt u een bestaande Azure Web container verwijderen zonder IntelliJ.

## <a name="see-also"></a>Zie ook ##

Zie voor meer informatie over de Toolkits Azure voor Java IDEs, de volgende koppelingen:

- [Azure Toolkit voor Eclips]
  - [Installatie van de Azure Toolkit voor Eclips]
  - [Een Hallo wereld Web-App voor Azure in Eclips maken]
  - [Wat is er nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installatie van de Azure Toolkit voor IntelliJ]
  - [Een Hallo wereld Web-App maken voor Azure in IntelliJ]
  - *Wat is er nieuw in de Azure Toolkit voor IntelliJ (in dit artikel)*

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij.md
[Een Hallo wereld Web-App voor Azure in Eclips maken]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Een Hallo wereld Web-App maken voor Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installatie van de Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse-installation.md
[Installatie van de Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Wat is er nieuw in de Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight-invoegtoepassing voor hulpmiddelen voor IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
