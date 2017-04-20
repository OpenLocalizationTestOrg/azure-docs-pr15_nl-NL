<properties 
   pageTitle="Azure SDK voor .NET 2.5.1 releaseopmerkingen" 
   description="Azure SDK voor .NET 2.5.1 releaseopmerkingen" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK voor .NET 2.5.1 releaseopmerkingen

Dit document bevat de releaseopmerkingen voor Azure SDK voor .NET 2.5.1 loslaat. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK voor .NET 2.5.1 releaseopmerkingen

Hier volgen nieuwe functies en -updates in de SDK Azure voor .NET 2.5.1.

- Nieuwe features\scenarios die betrekking hebben op **Web hulpmiddelen voor uitbreidingen**. 

    - Azure Websites is Azure App-Service gewijzigd. Zie voor meer informatie, [Azure App-Service en bestaande Azure-Services](app-service-changes-existing-services.md).
    - Azure API-Apps (Preview)-ondersteuning is toegevoegd zodat klanten kunnen ASP.NET-projecten als API-Apps publiceren en gebruik vervolgens de toevoegen > Azure API App-Client beweging in C# projecten om code op basis van de structuur van de geïmplementeerd API-App te genereren. 
    - Het knooppunt Websites in Server Explorer is afgeschaft in plaats van het knooppunt Azure App-Service met ondersteuning voor de Resource groep gebaseerde groepering van Azure API-Apps, Mobile-Apps en Web Apps.
    - Mobile-Apps controllers Azure Mobile-Apps (Preview)-ondersteuning is toegevoegd zodat klanten kunnen maken van nieuwe projecten gebruikt Mobile-Apps, toevoegen, de projecten publiceren en extern foutopsporing-toepassingen.
    - Toevoegen > Azure API App-Client beweging ondersteunt nu lokale Swagger JSON-bestanden, zodat ontwikkelaars Web API derden NuGets zoals Swashbuckle te genereren Swagger of auteur deze handmatig kunnen gebruiken. Op deze manier client ontwikkelaars kunnen de code-generatie-functies gebruiken een eindpunt Swagger in C#-projecten in beslag neemt. 
    - Web App en API App publicerende dialoogvensters zijn verbeterd ter ondersteuning van het concept Azure-Portal van resources groeperen en selectie/maken van Azure-resourcegroepen en abonnementen van App-Service worden aangegeven in het nieuwe Web App en API App inrichten dialoogvenster. 
    - Azure API App Server Explorer knooppunten bieden koppelingen naar de uitgebreide API Apps-koppeling in de Portal Azure, evenals andere functies zoals Log Streaming en foutopsporing op afstand.

    Voor bekende problemen en huidige beperkingen in Azure SDK .NET 2.5.1 [in dit](app-service-release-notes.md#known_issues_2_5_1) gedeelte hierna.


- Nieuwe features\scenarios die betrekking hebben op **Hulpmiddelen voor HDInsight** in Visual Studio zijn in deze release ingeschakeld. 
    - Lokale validatie van component scripts. Klik op de knop valideren script op de werkbalk om te kijken of er fouten in het script. 
    - Verbeterde foutopsporing component taken. Nu kunt u fouten opsporen component taken via garens Logboeken in Visual Studio. Als uw toepassing prestatieproblemen bevat, wordt onderzocht garens Logboeken, vindt u nuttige informatie...
    - (Openbare Preview) Ondersteuning voor component van een trefwoord automatisch aanvullen en IntelliSense. Om te helpen u creëert component scripts, HDInsight Tools voor Visual Studio toegevoegd trefwoord automatisch aanvullen en IntelliSense ondersteuning voor component.
    - Ondersteuning voor storm. U kunt nu HDInsight Tools voor Visual Studio Storm topologieën/Spouts/Bouten in C# ontwikkelen. U kunt vervolgens de ontwikkeld topologie aan een cluster Storm indienen en de status van de topologie/bout/spout bekijken. U kunt systeemlogboeken en klant-Logboeken gebruiken om op te lossen uw Storm topologieën/bouten/Spouts. U kunt bestaande JAVA-elementen ook gebruiken in Storm op HDInsight.
    
    Zie [aan de slag met HDInsight Hadoop Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)voor meer informatie.



##<a id="known_issues_2_5_1"></a>Azure SDK voor .NET 2.5.1 bekende problemen en beperkingen

- Azure API-Apps wordt weergegeven als een implementatiedoel voor Mobile-Apps. Web Apps moet de bestemming van de enige voor Mobile-Apps totdat een latere versie. 
- Azure inrichting van API-App kan resulteren in success maar af en toe aan het bijwerken van de voortgang in het venster Azure App serviceactiviteit is mislukt. Tijdelijke oplossing is om te controleren van de status van de nieuwe Azure API-App in de Portal Azure. 
- Bestand > Nieuw Project > API-App > F5 ervaring resulteert in een HTTP-fout, omdat er geen default/index.html. Tijdelijke oplossing is om handmatig naar de URL /api/values. 
- Pictogrammen van Server Explorer weergegeven en met tussenpozen afgeplatte. Opnieuw starten van de VS lost dit probleem. 
- Als een uitzondering is opgetreden tijdens de inrichting van Web App of API-App (zoals quotum overschreden voor fouten of de naam van de gateway dubbele Azure API App), wordt in de fouten onbewerkte JSON tekst weergeven. 
- Door onregelmatige maken van projecten problemen wanneer inzichten van toepassing op de aanmaaktijd van een project is ingeschakeld.
- Soms de gegenereerde Azure API App-Client-code ontbreekt naamruimten, ze moeten worden opgenomen handmatig (of die automatisch worden geïmporteerd via Visual Studio hints) voor de code compileren. 
- Mobile-App projecten moeten worden gepubliceerd naar de WebApps, maar u moet een site die u hebt gemaakt als een Mobile-App in de Portal Azure omdat Mobile-App projecten is vereist voor een database kiezen. 
- De startpagina voor Mobile-Apps gebruikt de term "mobile service" in plaats van 'mobiele apps' 
- Mobile-App maken van het project kan minuten duren om te maken. 
- Tijdens de API App terugkomt inrichting (in sommige gevallen) van een fout van de Azure-API na te denken dat de machtigingen niet correct kunnen worden ingesteld terwijl de API-App correct is ingericht en gereed voor gebruik is. U kunt machtigingen met behulp van de Portal Azure handmatig instellen.
- Inzichten van de toepassing wordt niet ondersteund op API-App-sjablonen en Mobile-App-sjablonen.
- API-App projecten kunnen niet worden gebruikt in combinatie met Cloudservice projecten.
- API-App project-sjablonen zijn alleen beschikbaar in C#.
- API-App verbruik via het contextmenu "Toevoegen Azure API App Client" wordt alleen ondersteund in C#.

 
