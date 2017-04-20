<properties 
   pageTitle="Azure SDK voor .NET 2.6 releaseopmerkingen" 
   description="Azure SDK voor .NET 2.6 releaseopmerkingen" 
   services="app-service/web" 
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

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK voor .NET 2.6 releaseopmerkingen

Dit document bevat de releaseopmerkingen voor Azure SDK voor .NET 2.6 release. 

U kunt met Azure SDK 2.6 cloud-servicetoepassingen (PaaS) .NET 4.5.2 of .NET 4.6 doelgroepen, mits u het doel .NET Framework handmatig op de rol van de Service Cloud installeren ontwikkelen. Zie [.NET op een rol Cloud-Service installeren](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Bus service-updates

- Gebeurtenis Hubs: 

    - Nu kunt gerichte toegangsbeheer bij het verzenden van gebeurtenissen door door te geven van extra publisher-eindpunt voor gebeurtenis Hubs.
    - Aanvullende stabiliteit en verbetering toegevoegd aan de gebeurtenis Hubs functie.
    - Ondersteuning van Amqp protocol via WebSocket toe te voegen voor messaging en gebeurtenis Hubs.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools voor Visual Studio-updates

- **IntelliSense schaduweffecten**: externe metagegevens suggestie

    HDInsight Tools voor Visual Studio ondersteunt nu ophalen externe metagegevens wanneer u uw script component bewerkt. Bijvoorbeeld, typt u * *Selecteer* FROM** en alle tabelnamen worden weergegeven. De kolomnamen wordt ook weergegeven nadat u een tabel hebt opgegeven.

- **HDInsight emulator ondersteuning**

    Nu HDInsight Tools voor Visual Studio ondersteuning verbinding maakt met HDInsight emulator, zodat u kan uw scripts component lokaal ontwikkelt zonder introductie van toepassing kosten, klikt u vervolgens deze scripts tegen HDInsight clusters uitvoeren. 

    Raadpleeg [deze handleiding](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)voor meer informatie.

- **HDInsight Tools voor Visual Studio ondersteuning voor algemene Hadoop clusters** (Preview)

    Nu ondersteuning HDInsight Tools voor Visual Studio algemene Hadoop kolomgroepen zodat u HDInsight Tools voor Visual Studio gebruiken kunt om het volgende te doen:

    - verbinding maken met uw cluster, 
    - Component query met verbeterde ondersteuning voor IntelliSense/automatisch aanvullen, schrijven 
    - alle taken in uw cluster met een intuïtieve gebruikersinterface weergeven. 

    Raadpleeg [deze handleiding](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409)voor meer informatie.

##<a name="in-role-cache-updates"></a>In de rol Cache-updates

- **In de rol Cache** is bijgewerkt **Microsoft Azure opslag SDK** versie 4.3 gebruiken. Tot op heden, is de **Cache In rol** Azure opslag SDK versie 1.7 gebruiken.

    Klanten gebruiken Azure SDK 2,5 of hieronder moet bijwerken naar Azure SDK 2.6 en verplaatsen naar de nieuwe versie van Azure opslag SDK. 

    Azure opslagcapaciteit versie 2011-08-18 is op dit moment gepland 1 augustus 2016 zijn verwijderd. Een migratie van tot rol Cache van Azure SDK 2,5 of hieronder moeten naar 2.6 zijn voltooid op dit moment. Zie voor meer informatie over de buitengebruikstelling van Azure opslagcapaciteit versie 2011-08-18, [Update over het verwijderen van de versie van Microsoft Azure opslag Service: extensie naar 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]We bent de buitengebruikstelling 30 November 2016, voor Azure-Service beheerd Cache en Azure In rol Cache aankondigen. Het is raadzaam om te migreren naar Azure bestand Vgx. Cache voorbereiding op deze buitengebruikstelling. Zie voor meer informatie over datums en migratie richtlijnen, [waarin Azure Cache aanbod is geschikt voor mij?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Hulpmiddelen voor Azure App-Service

De volgende items zijn bijgewerkt in de versie van Azure SDK 2,6 af.

- Azure publiceren uitgebreid met Azure API Apps als implementatiedoel.
- API-Apps inrichting functionaliteit waarmee gebruikers met functionaliteit voor het maken en de inrichting van API-App.
- Server Explorer aangepast aan de nieuwe App Service knooppunt met Web, Mobile en API apps die zijn gegroepeerd per resourcegroep.
- Azure API App-Client beweging toegevoegd aan de meeste C#-projecten waarmee automatisch genereren van Swagger ingeschakelde API Apps uitgevoerd in Azure abonnement van een gebruiker toevoegen.
- Tooling API-Apps en App Service knooppunten in Server Explorer zijn alleen beschikbaar in Visual Studio-2013. 

##<a name="azure-resource-manager-tools-updates"></a>Azure resourcemanager hulpmiddelen voor updates

De manager Azure resource tools hebt bijgewerkt met sjablonen voor virtuele Machines, netwerken en opslag. De JSON bewerken ervaring heeft bijgewerkt met een nieuwe overzichtsweergave voor sjablonen en de mogelijkheid om te bewerken van de JSON fragmenten met sjablonen. Sjablonen die zijn geïmplementeerd in Visual Studio gebruik een PowerShell-script voorzien van het project, zodat alle wijzigingen in het script worden gebruikt door Visual Studio.

##<a name="diagnostics-improvements-for-cloud-services"></a>Verbeteringen in de diagnostische hulpprogramma's voor Cloudservices

Azure SDK 2.6 Hiermee terug ondersteuning voor het verzamelen van diagnostische logboeken in de emulator Azure berekenen en overbrengt naar ontwikkeling opslag. Een diagnostisch hulpprogramma Logboeken (met inbegrip van de toepassing trace Logboeken, gebeurtenissen traceren van Windows (ETW)-Logboeken, prestatie-items, infrastructuur logboeken en windows gebeurtenislogboeken) gegenereerd wanneer de toepassing wordt uitgevoerd in de emulator kan worden doorgeschakeld naar ontwikkeling opslag om te bevestigen dat uw gegevens diagnostische gegevens vastleggen op uw lokale computer werkt. 

Het hulpprogramma's voor diagnose opslag-account kan nu worden opgegeven in de service (.cscfg) configuratiebestand wordt eenvoudiger andere hulpprogramma's voor diagnose opslag rekeningen gebruiken voor verschillende omgevingen. Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in Azure SDK 2,4 en Azure SDK 2.6 heeft gewerkt. Zie [Diagnostische hulpprogramma's configureren voor Azure Cloud Services](http://go.microsoft.com/fwlink/?LinkID=532784)voor meer informatie over het gebruik van de verbinding diagnostisch hulpprogramma opslag tekenreeks en hoe dit van invloed op uw projecten.

##<a name="breaking-changes"></a>Wijzigingen verbreken

###<a name="azure-resource-manager-tools"></a>Azure resourcemanager hulpmiddelen 

- Het projecttype **Cloud implementatieprojecten** beschikbaar in de Azure SDK 2,5 is gewijzigd in **Azure resourceveld groep**.
- Projecten die zijn gemaakt in de Azure SDK 2,5 **Cloud implementatieprojecten** type kan worden gebruikt in 2.6 maar implementeren van de sjabloon Visual Studio, mislukt. Echter werken implementeert met de PowerShell-script nog niet eerder.  Voor meer informatie over het gebruik van Lees **Projecten van Cloud-implementatie** in 2.6 deze [posten](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Bekende problemen

- Diagnostische logboeken in de emulator verzamelt vereist een 64-bits besturingssysteem wordt uitgevoerd. Diagnostische logboeken worden geen verzameld wanneer u zich in een 32-bits besturingssysteem wordt uitgevoerd. Dit geldt niet voor alle andere emulator-functionaliteit. 

- Azure SDK 2.6 uitgebracht op 29-4-2015 had twee problemen: 

    - Universele App kan niet worden geladen in Visual Studio-2015 wanneer Azure SDK 2,6 af op de computer is geïnstalleerd.
    - Voor foutopsporing in een Cloudservice project zou mislukt in Visual Studio 2013 en Visual Studio-2015 waar Visual Studio niet meer reageert en loopt vast bij het weergeven van een dialoogvenster met het bericht "Configureren diagnostische hulpprogramma's voor emulator".
    
    Een update van Azure SDK 2.6 is uitgebracht op 18-5-2015. De bijgewerkte versie is 2.6.30508.1601; de presentatie bevat oplossingen voor twee problemen hierboven is beschreven. U kunt het bouwen van de SDK via het Configuratiescherm Identificeer -> programma's en functies-Microsoft Azure-hulpmiddelen > voor Microsoft Visual Studio-2013-v 2,6 af. De kolom versie, wordt het versienummer weergegeven.

    Als u nog steeds die tegenover elkaar de bovenstaande problemen liggen zijn, moet u de nieuwste versie van de Azure 2.6 SDK installeren voor [tegenover 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [tegenover 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) of [tegenover 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Zie ook

[Ondersteuning en buitengebruikstelling informatie voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
