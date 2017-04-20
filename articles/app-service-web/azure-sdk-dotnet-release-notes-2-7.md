
<properties 
   pageTitle="Azure SDK voor .NET 2.7 en .NET 2.7.1 releaseopmerkingen" 
   description="Azure SDK voor .NET 2.7 en .NET 2.7.1 releaseopmerkingen" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK voor .NET 2.7 en .NET 2.7.1 releaseopmerkingen

##<a name="overview"></a>Overzicht

Dit document bevat de releaseopmerkingen voor Azure SDK voor .NET 2.7 release. 

Het document bevatten ook de releaseopmerkingen voor Azure SDK voor .NET 2.7.1 loslaat.

Azure SDK 2.7 wordt alleen ondersteund in Visual Studio-2015 en Visual Studio-2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) is dat de laatste SDK ondersteund voor Visual Studio 2012.

Zie voor gedetailleerde informatie over deze release, [Azure SDK 2.7 aankondiging posten](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) en [Azure SDK 2.7.1 aankondiging posten](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK voor .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Meld u aan verbeteringen voor Visual Studio-2015

Azure SDK 2.7 voor Visual Studio-2015 ondersteunt de nieuwe identiteit beheerfuncties in Visual Studio-2015.  Dit geldt ook voor ondersteuning voor accounts toegang krijgen tot het Azure via rol gebaseerd toegangsbeheer, Cloud oplossing Providers, DreamSpark en andere typen account en abonnementen.

De wordt geleverd bij Azure SDK 2.7 verbeteringen in het vak Aanmeldingsadres zijn alleen beschikbaar in Visual Studio-2015. Ondersteuning voor Visual Studio 2013 is opgenomen in Azure SDK 2.7.1.


###<a name="mobile-sdk"></a>Mobiele SDK

**Mobile-Apps** sjablonen om weer te geven van de nieuwste [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) en configuratie van proces bijgewerkt.

###<a name="service-bus"></a>Service Bus 

Algemene correcties en verbeteringen. Raadpleeg de releaseopmerkingen van de meest recente [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/)voor details over updates en functies.

###<a name="hdinsight-tools"></a>Hulpmiddelen voor HDInsight 

In deze release zijn de volgende updates zijn aangebracht. Deze updates zijn in de proefversie. Zie voor meer informatie [in dit blog](http://go.microsoft.com/fwlink/?LinkId=619108).

- Grafieken voor component component op Tez taken
- Volledige component DML IntelliSense-ondersteuning
- Varken sjabloon ondersteuning
- Storm sjablonen voor Azure services

####<a name="breaking-changes"></a>Wijzigingen verbreken

- Oude **Storm** project moet worden bijgewerkt wanneer u deze versie van de hulpmiddelen gebruikt. Zie voor meer informatie [in dit blog](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express niet meer wordt ondersteund. Zie voor meer informatie [in dit blog](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Hulpmiddelen voor Azure App-Service

In deze release zijn de volgende updates zijn aangebracht in Web hulpmiddelen voor uitbreidingen. Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blog. 

- Ondersteuning voor DreamSpark accounts die zijn toegevoegd
- Volledige wijziging Azure-hulpprogramma's die zijn aangebracht ter ondersteuning van de nieuwe Azure Management API's voor Resource
- Ondersteuning voor Azure App-Service is toegevoegd aan de [Cloud Explorer](#cloud_explorer)

####<a name="known-issues"></a>Bekende problemen

Web App-implementatie slot knooppunten worden niet weergegeven in het knooppunt sleuven in Server Explorer en Web App-implementatie slot onderliggende knooppunten onder Cloud Explorer niet geladen. Dit probleem is opgelost en voorbereid voor de volgende SDK-versie. 


###<a name="cloud_explorer"></a>Cloud Explorer voor Visual Studio-2015

Azure SDK 2.7 bevat Cloud Explorer voor Visual Studio-2015 waarmee u uw Azure resources bekijken, hun eigenschappen controleren en belangrijke ontwikkelaars acties uitvoeren vanuit in Visual Studio. 

Cloud explorer ondersteunt het volgende:

- Resourcegroep en Type Resource weergaven van uw Azure resources 
- Zoeken naar resources met hun naam weergeven (beschikbaar in de weergave van het Type Resource)
- Ondersteuning voor abonnementen en resources met rol op basis van Access besturingselement RBAC () toegepast 
- Geïntegreerde actie Configuratiescherm waarin ontwikkelaars gerichte acties specifiek zijn voor resources geselecteerd. Bijvoorbeeld: externe foutopsporing koppelen voor het weergeven van virtuele Machines die zijn gemaakt met de stapel resourcemanager diagnostisch hulpprogramma gegevens voor virtuele Machines enzovoort.
- Deelvenster met een geïntegreerde eigenschappen waarin wordt getoond ontwikkelaars gerichte eigenschappen vaak nodig tijdens ontwikkelaar/testen 
- Snelle overschakelen van het account wilt gebruiken wanneer u resources opsommen (opdracht instellingen op de werkbalk gebruiken) 
- Filteren van abonnementen op het gebruik bij het opsommen resources (opdracht instellingen op de werkbalk gebruiken) 
- Uitgebreide koppelingen naar de Portal Azure voor beheer van resources en resourcegroepen 
 
 
###<a name="azure-resource-manager-tools"></a>Azure resourcemanager hulpmiddelen 

De hulpmiddelen van Azure resourcemanager zijn bijgewerkt als u wilt werken met rol op basis van Access besturingselement RBAC () en nieuwe abonnement typen.  Deze wijzigingen is inbegrepen, is de mogelijkheid om te gebruiken van nieuwe opslag-accounts, naast klassieke opslag, voor de opslag van onderdelen tijdens de implementatie.  

Als u een resourcegroep Azure-project uit een eerdere versie van de SDK met de 2.7 SDK gebruikt, wordt een nieuwe implementatiescript is nodig om u te implementeren met een nieuwe opslag-account in plaats van klassieke opslag.  U wordt gevraagd voordat wijzigingen worden aangebracht in uw project nieuwe script toevoegen.  Het oude script worden gewijzigd en moet u handmatig Breng eventuele wijzigingen aan het nieuwe script.
 
 
###<a name="storage-explorer-tools"></a>Extra opslagruimte-Explorer 

- Ondersteuning voor het weergeven van BLOB's toevoegen. Meer informatie in [dit blogbericht](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Ondersteuning voor het weergeven van de Premium-opslag accounts via Server Explorer. Server Explorer worden alleen weergegeven voor BLOB van pagina's voor premium opslag-accounts zoals ze de enige ondersteunde type voor premium opslag-accounts zijn.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory Tools voor Visual Studio 

Inleiding tot **Azure Data Factory Tools** voor Visual Studio. Hieronder ziet u welke functies zijn ingeschakeld. Zie [Dit blog](http://go.microsoft.com/fwlink/?LinkId=617530) voor meer informatie.

- **Sjabloon gebaseerd authoring**: Selecteer gebruik geïntegreerd op basis van sjablonen, gegevens verkeer sjablonen of gegevensverwerking sjablonen kunt implementeren van een oplossing end-to-end-integratie en aan de slag praktische snel met gegevens Factory. 
- **Integratie met Explorer oplossing voor het ontwerpen en implementeren van gegevens Factory entiteiten**: maken en implementeren van pijpleidingen en gerelateerde entiteiten als Visual Studio-projecten. 
- **Integratie met Diagram weergeven voor visuele interactie tijdens het ontwerpen**: visueel auteur pijpleidingen en gegevenssets met hulpmiddel uit de diagramweergave te klikken. 
- **Integratie met Server explorer voor het bekijken en interactie met al geïmplementeerd entiteiten**: gebruikmaken van de Server Explorer om te bladeren die al geïmplementeerd gegevens factory's en de corresponderende entiteiten. Een factory gedistribueerde gegevens of een entiteit (verkooppijplijn, gekoppelde Service, gegevenssets) importeren in uw project. 
- **JSON bewerken met schema gegevensvalidatie en uitgebreide intellisense**: efficiënt configureren en bewerken van JSON documenten van gegevens Factory entiteiten met uitgebreide intellisense en schemabestanden gegevensvalidatie 
- **Publiceren op meerdere omgeving**: publiceren pijpleidingen op ontwikkelaar, test of productcatalogus omgeving geschreven door afzonderlijk configuratiebestanden voor elke omgeving te maken.
- **Varken, component en .net gebaseerd gegevensverwerking ondersteuning**: ondersteuning voor varken en component Scripts in Data Factory-project. Ondersteuning voor verwijst naar C#-Project voor het beheren van .net activiteit.

##<a name="azure-sdk-for-net-271"></a>Azure SDK voor .NET 2.7.1

De volgende sectie bevat updates die zijn opgenomen met de SDK Azure voor .NET 2.7.1 loslaat.
###<a name="hdinsight-tools"></a>Hulpmiddelen voor HDInsight 

Zie [Dit blog](http://go.microsoft.com/fwlink/?LinkId=623831)voor gedetailleerde informatie over updates voor hulpmiddelen voor HDInsight.

- Component taak Operator View (een nieuwe functie)

    Om u te helpen u inzicht krijgen in uw query component beter, is de functie component Operator weergave toegevoegd. Als u wilt zien van alle operatoren in een hoekpunt, dubbelklikt u op de hoekpunten van de taak-grafiek. Als u meer details van een bepaalde operator, plaats u de muisaanwijzer op die operator.
- Markering voor component fout (een nieuwe functie)

    Zodat u kunt het weergeven van de grammaticafouten direct, is de functie component fout markering toegevoegd. Ook foutberichten zijn enhanced en nu ziet u gedetailleerde grammaticafouten direct (totdat deze release, moest u een script component aan het cluster indienen en wacht enige tijd voordat u meer informatie over het foutbericht wordt weergegeven).  
- Storm topologie Graph (een nieuwe functie)

    Zomersportevenementen is heel belangrijk als u zien wilt als uw topologie werkt zoals verwacht. In deze release toegevoegd we visualisatie voor Storm grafieken. U kunt de doelstellingen van de belangrijke visualiseren voor uw topologie (bijvoorbeeld een kleur geeft weer een bepaalde rol is "bezet" al dan niet). U kunt ook dubbelklikken de bout/Spout als u meer details.

- Ondersteuning voor HDInsight clusters die zijn gemaakt in de Portal Azure (een fout fix)

    U kunt nu Visual Studio gebruiken om te bekijken en taken verzenden naar alle HDInsight clusters bekijken, ongeacht waar het cluster zijn gemaakt.

- Meer IntelliSense-ondersteuning & sneller component bij metagegevens (een verbetering) laden

    We hebben de IntelliSense verbeterd door toe te voegen meer gebruiker beschrijvende suggesties. Bijvoorbeeld kan tabelalias nu ook worden voorgesteld in IntelliSense zodat u kunt uw query eenvoudiger te schrijven. Bovendien hebben we de component metagegevens laden zodat het duurt slechts enkele seconden voor een overzicht van alle databases, tabellen en kolommen van uw metastore component verbeterd.

Zie [Dit blog](http://go.microsoft.com/fwlink/?LinkId=623831)voor gedetailleerde informatie over updates voor hulpmiddelen voor HDInsight.

###<a name="improvements-in-visual-studio-2013"></a>Verbeteringen in Visual Studio 2013

- Azure SDK 2.7.1 kunt Visual Studio 2013 voor toegang tot Azure accounts en abonnementen via rol gebaseerd toegangsbeheer, Cloud oplossing Providers en Dreamspark.
- Met Azure SDK 2.7.1, het nieuwe Cloud Explorer hulpmiddel venster is nu ook beschikbaar in Visual Studio-2013.

###<a name="known-issues"></a>Bekende problemen

Installatie van de Azure SDK 2,6 af of 2.7.1 voor Visual Studio Community 2013 op een niet - Engelse OS er een waarschuwing weergegeven dat de Engelse en niet-Engelstalige bronnen van Visual Studio misschien overeen. Deze waarschuwing mogelijk veilig gesloten. Dit gebeurt alleen als de computer bevat geen van Visual Studio Community 2013 zijn geïnstalleerd en u de SDK op een niet - Engelse OS installeert. De waarschuwing wordt weergegeven nadat het taalpakket de resources RTM geldt voor Visual Studio, maar voordat u deze Update 4 is van toepassing. Sluiten van de waarschuwing, kan het taalpakket om te gaan en het toepassen van de Update 4-versie van de language pack-inhoud te voltooien.

LightSwitch projecten zijn niet compatibile in deze versie. Dit probleem is opgelost met de volgende SDK-versie.

##<a name="also-see"></a>Zie ook
[Azure SDK 2.7.1 aankondiging bericht](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 aankondiging bericht](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Ondersteuning en buitengebruikstelling informatie voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
