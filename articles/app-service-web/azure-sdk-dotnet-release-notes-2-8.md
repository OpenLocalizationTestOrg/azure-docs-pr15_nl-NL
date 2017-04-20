
<properties 
   pageTitle="Azure SDK voor .NET 2,8 releaseopmerkingen" 
   description="Azure SDK voor .NET 2,8 releaseopmerkingen" 
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
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK voor .NET 2,8, punt 2.8.1 en punt 2.8.2

##<a name="overview"></a>Overzicht
 
In dit artikel bevat de releaseopmerkingen (met bekende problemen en recente wijzigingen) voor de SDK Azure voor .NET 2,8, punt 2.8.1 en punt 2.8.2 loslaat. 

Zie de aankondiging [Azure SDK 2,8 voor Visual Studio 2013 en Visual Studio-2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) voor de volledige lijst met nieuwe functies en updates die zijn aangebracht in deze release. 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK voor .NET 2,8

### <a name="download-azure-sdk-for-net-28"></a>Azure SDK voor .NET 2,8 downloaden

[Azure SDK voor .NET 2,8 voor Visual Studio-2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK voor .NET 2,8 voor Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>Ondersteuning voor .NET 4.5.2 

####<a name="known-issues"></a>Bekende problemen

Azure .NET SDK 2,8 kunt u .NET 4.5.2 maken Cloudservice-pakketten. Echter 4.5.2 .NET framework wordt niet geïnstalleerd op de standaard Gast OS afbeeldingen tot januari 2016 Gast OS loslaat. Voordat u deze, .NET 4.5.2 framework zijn beschikbaar via een afzonderlijke Gast OS releaseversie – November 2015-02. Ga naar de pagina [Azure Gast OS Releases en SDK compatibiliteit Matrix](../cloud-services/cloud-services-guestos-update-matrix.md) om bij te houden wanneer de afbeelding wordt uitgebracht.  Zodra de November 2015-02-afbeelding is uitgebracht kunt u de afbeelding te gebruiken die door het hulpprogramma voor het bestand (.cscfg) van de Cloudservice-configuratiebestand bij te werken. In de serviceconfiguratie bestand het kenmerk Versie_besturing van het element ServiceConfiguration ingesteld op de tekenreeks "WA-GAST-OS-4.26_201511-02 '. Als u wilt gebruiken in deze afbeelding en vervolgens u niet langer automatische updates van het besturingssysteem Gast krijgt deelnemen. Over de automatische updates de Versie_besturing moet zijn ingesteld op "*" en .NET 4.5.2 zijn alleen beschikbaar via Automatische updates in januari 2016.

###<a name="azure-data-factory"></a>Azure gegevens Factory

####<a name="known-issues"></a>Bekende problemen 

Tijdens een project **Factory gegevenssjabloon** maken met voorbeeldgegevens, mislukken azure power-shellscript als azure power shell versie op de computer zijn geïnstalleerd na 0.9.8.

Om te kunnen dit type project maken, moet u [azure power shell versie 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)installeren.


### <a name="azure-resource-manager-tools"></a>Azure resourcemanager hulpmiddelen 

####<a name="breaking-changes"></a>Wijzigingen verbreken

De PowerShell-script verstrekt door het project Azure resourcegroep is bijgewerkt in deze release voor gebruik met de nieuwe Azure PowerShell-cmdlets versie 1.0.  Deze nieuwe script werkt niet uit met Visual Studio wanneer u een versie van de SDK voordat u 2,8 gebruikt.  

Projecten die zijn gemaakt in eerdere versies van de SDK scripts wordt niet uitvoeren vanuit Visual Studio bij gebruik van de 2,8 SDK.  Alle scripts blijft werken buiten Visual Studio met de juiste versie van de Azure PowerShell-cmdlets.  

De 2,8 SDK vereist versie 1.0 van de Azure PowerShell-cmdlets.  Alle andere versies van de SDK vereisen versie 0.9.8 van de Azure PowerShell-cmdlets.  Zie voor meer informatie [in dit](http://go.microsoft.com/fwlink/?LinkID=623011) blog.

###<a name="web-tools-extensions"></a>Web Tools extensies

####<a name="known-issues"></a>Bekende problemen

De volgende bekende problemen wordt in de volgende versie opgelost.

- App Service gerelateerde Cloud en Server Explorer beweging voor niet-productieomgevingen (zoals Azure China of Azure stapel klanten) werken niet. Het profiel publiceren downloaden uit de portal van Azure kunt publicerende mogelijkheid voor klanten in deze risico gebieden. Een toekomstige release wordt bewegingen zoals "Foutopsporing bijvoegen" en "Weergave Streaming logboeken" herstel van Azure China en stapel klanten. 
- Klanten ziet mogelijk fouten tijdens het App-Service gemaakt wanneer de App-inzichten exemplaar waarin ze implementeert een gebied dan Oost VS wordt. In deze situaties worden maken van een App-Service in de portal en downloaden van het profiel publiceren publicerende scenario's mogelijk. 

###<a name="azure-hdinsight-tools"></a>Hulpmiddelen voor Azure HDInsight

####<a name="new-updates"></a>Nieuwe updates

- U kunt uw query component uitvoeren in het cluster via HiveServer2 met vrijwel geen realiseren en dat de taak Logboeken in realtime.
- Met de nieuwe component taak kan worden uitgevoerd weergave kunt u uw werk grondigere nader, daarna nog meer informatie vinden en mogelijke problemen.

Zie [Azure SDK 2,8 voor Visual Studio 2013 en Visual Studio-2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)voor informatie. 

## <a name="azure-sdk-for-net-281"></a>Azure SDK voor .NET punt 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekende problemen voor Visual Studio 2013 en Visual Studio-2015
 
1. Geactiveerde WebJob worden gepubliceerd naar sleuven kan weergeven en de fout en won't instellen een planning, maar deze wordt push de WebJob naar Azure. Klanten die worden een geplande taak moeten kunnen vervolgens de Azure-Portal gebruiken voor het instellen van de planning voor de WebJob. 
2. Python klanten kunnen foutopsporing problemen. Service-team is implementeren van een oplossing voor dit, maar als klanten worden beïnvloed, kunt Microsoft weten op de forums of in de blog aankondiging of vrijgeven van de sectie Opmerkingen notities. 
3. Klanten in bepaalde gebieden (zoals Zuid-India) ervaart App Service inrichting van fouten. Dit komt overeen met de portal en klanten die dit probleem doet zich de portal van Azure toegang tot het publiceren naar deze geografische regio aanvragen kunnen gebruiken. Zodra ze toegang tot deze regio's met aanvragen moet de portal inrichting Azure werken. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK voor .NET punt 2.8.2

Na de installatie van het punt 2.8.2 hulpmiddelen voor klanten waarnemen het volgende probleem.         

- Als u Windows 10 gebruikt en Internet Explorer niet hebt geïnstalleerd, krijgt u mogelijk een fout "Internet Explorer niet gevonden".
Dit probleem, installeert u Internet Explorer met het dialoogvenster Windows-onderdelen toevoegen/verwijderen.

Als u dit probleem ziet, moet u de Send a smile-functie gebruiken om dit te melden.

Zie voor meer informatie [Dit](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) posten.
##<a name="other-updates"></a>Andere updates

Zie voor andere updates, [Azure SDK 2,8 aankondiging posten](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Zie ook

[Azure SDK 2,8 aankondiging bericht](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Ondersteuning en buitengebruikstelling informatie voor de Azure SDK voor .NET en API 's](https://msdn.microsoft.com/library/azure/dn479282.aspx)

