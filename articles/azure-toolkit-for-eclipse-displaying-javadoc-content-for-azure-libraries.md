<properties
    pageTitle="Javadoc inhoud in Eclips weer te geven voor het pakket Azure bibliotheken voor Java"
    description="Hoe u de inhoud van de Javadoc weergeven voor de bibliotheken Azure in Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Javadoc inhoud in Eclips weer te geven voor het pakket Azure bibliotheken voor Java #

De inhoud van de Javadoc voor de bibliotheken Azure voor Java kan worden weergegeven in uw omgeving Eclips door te koppelen van de inhoud Javadoc tot de Azure-bibliotheken beheren voor Java. De volgende stappen hoe u deze functionaliteit binnen Eclips gebruiken.

Deze procedure wordt ervan uitgegaan dat u de bibliotheek Azure voor Java al hebt toegevoegd aan uw pad opbouwen.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Javadoc inhoud in Eclips weergeven voor de bibliotheken Azure voor Java ##

* Open in de sectie **Waarnaar wordt verwezen bibliotheken** van uw project, binnen de Eclips Projectverkenner, het contextmenu voor de bibliotheek Azure voor Java JAR. Bijvoorbeeld: **microsoft-windowsazure-api-0.1.0.jar** (het versienummer mogelijk verschillende, afhankelijk van welke versie u hebt geïnstalleerd).
* Klik op **Eigenschappen**.
* Klik op **Javadoc locatie**in het dialoogvenster **Eigenschappen** , klikt u in het linkerdeelvenster. Het dialoogvenster **Javadoc locatie** wordt weergegeven.
* U kunt een **Javadoc URL**of een **Javadoc in archief**.
    * Als u besluit om een **Javadoc URL**opgeeft, gebruikt u de URL's zoals **http://dl.windowsazure.com/javadoc** of **http://dl.windowsazure.com/storage/javadoc**.
    * Als u besluit om **Javadoc in archief**gebruiken, kunt u een extern bestand of een werkruimtebestand opslaan.
    Maak uw keuze en zo nodig bladeren/valideren. Het volgende voorbeeld wordt de bibliotheken Azure voor Java met de bijbehorende Javadoc JAR die lokaal naar een map genaamd **c:\MyAzureJARs**is gedownload.
    ![][ic553487]
* *Optioneel*: klik op **valideren**. Mogelijke problemen met de Javadoc JAR kunnen hier worden weergegeven.
* Klik op **OK**.

Wanneer is gekoppeld aan de bibliotheek, moet de Javadoc-inhoud binnen uw IDE Eclips weergegeven. Bijvoorbeeld als `blob` is gedefinieerd van het type `CloudBlockBlob` uw code, is de volgende handelingen uit een voorbeeld van Javadoc inhoud die wordt weergegeven wanneer u typt `blob.acquireLease` in code:

![][ic553488]

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png