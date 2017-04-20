<properties
    pageTitle="Wat is de SDK van Azure .NET"
    description="Informatie over de mogelijkheden van de Azure .NET SDK."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Wat is de SDK Azure voor .NET?

## <a name="overview"></a>Overzicht

De SDK Azure voor .NET is een reeks hulpmiddelen voor Visual Studio, opdrachtregel hulpmiddelen, runtime binaire bestanden en clientbibliotheken die u helpen ontwikkelen, testen en implementeren van de apps die worden uitgevoerd in Azure. Dit artikel wordt uitgelegd wat u krijgt wanneer u de SDK installeren. U kunt de SDK downloaden van de [pagina Downloads van Azure](https://azure.microsoft.com/downloads/).

De SDK Azure voor .NET omvat ook [clientbibliotheken voor in beslag nemen Azure-services](http://go.microsoft.com/fwlink/?LinkId=510472). Deze bibliotheken zijn geïnstalleerd NuGet afzonderlijk bij met.

##<a id="included"></a>Wat opgenomen in de SDK Azure voor .NET

De SDK Azure voor .NET installeert de volgende producten:

- [Visual Studio-Community-editie 2015](#vwd)
- [Microsoft Azure opslag Emulator](#stgemulator)
- [Hulpmiddelen voor Microsoft Azure-opslag](#stgtools)
- [Microsoft Azure Authoring hulpmiddelen](#auth)
- [Microsoft Azure Emulator](#emulator)
- [HDInsight Tools voor Visual Studio en Microsoft Component ODBC-stuurprogramma](#hdinsight)
- [Microsoft Azure-bibliotheken voor .NET](#libraries)
- [Microsoft Azure mobiele App SDK](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Azure-hulpprogramma's voor Microsoft Visual Studio](#tools)
- [Microsoft ASP.NET- en Web Tools voor Visual Studio](#wte)
- [Microsoft Azure-gegevens Lake Tools voor Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio-Community-editie 2015

Als u geen Visual Studio op uw computer, wordt de SDK [Visual Studio Community Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs)geïnstalleerd.

###<a id="stgemulator"></a>Microsoft Azure opslag Emulator

De [Azure opslag Emulator](http://msdn.microsoft.com/library/hh403989.aspx) maakt gebruik van een SQL Server-instantie en het lokale bestandssysteem simuleren Azure Storage (wachtrijen, tabellen, BLOB's), zodat u lokaal kunt testen.

###<a id="stgtools"></a>Hulpmiddelen voor Microsoft Azure-opslag

Hiermee installeert u [AzCopy](http://aka.ms/AzCopy), een hulpprogramma voor de opdrachtregel u gebruiken kunt om gegevens te brengen in- en afmelden bij een Azure Storage-account.

###<a id="auth"></a>Microsoft Azure Authoring hulpmiddelen

Deze groep omvat de volgende:

* Het [hulpprogramma voor de CSPack](http://msdn.microsoft.com/library/gg432988.aspx) voor het maken van de implementatiepakketten.
* het [hulpprogramma voor de CSEncrypt](http://msdn.microsoft.com/library/hh404001.aspx) voor het coderen van wachtwoorden die worden gebruikt voor toegang tot exemplaren van de rol cloud service via een verbinding met extern bureaublad.
* Binaire bestanden runtime die serviceprojecten cloud is vereist voor communicatie met hun runtimeomgeving en voor diagnostische gegevens. Deze binaire bestanden zijn niet beschikbaar in NuGet-pakketten.

###<a id="emulator"></a>Microsoft Azure Emulator

De [Azure Emulator](http://msdn.microsoft.com/library/dn339018.aspx) overeenkomt met de cloud-service-omgeving zodat u cloud serviceprojecten lokaal op uw computer testen kunt voordat u deze bij Azure implementeert.

###<a id="hdinsight"></a>HDInsight Tools voor Visual Studio en Microsoft Component ODBC-stuurprogramma

HDInsight hulpmiddelen in Server Explorer kunt u navigeren component databases en gekoppelde opslag accounts voor HDInsight clusters, tabellen, maken en maken en indienen component query's. Zie [aan de slag met HDInsight Hadoop Tools for Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md)voor meer informatie.

###<a id="libraries"></a>Microsoft Azure-bibliotheken voor .NET

Deze groep omvat de volgende:

* NuGet-pakketten voor Azure opslag, Service Bus en cache die zijn opgeslagen op uw computer, zodat Visual Studio serviceprojecten nieuwe cloud terwijl maken kunt offline.
* Een Visual Studio invoegtoepassing waarmee [In rol Cache](http://msdn.microsoft.com/library/dn386103.aspx) projecten lokaal uitvoeren in Visual Studio.

###<a id="mobile"></a>Microsoft Azure mobiele App SDK

Hulpprogramma's voor het werken met [Azure App Service Mobile-Apps](app-service-mobile/app-service-mobile-value-prop.md).

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShell kunt u [automatiseren Azure-omgeving maken en implementeren](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Microsoft Azure-hulpprogramma's voor Microsoft Visual Studio

Hiermee kunt u werken met Azure resources, hoofdzakelijk Cloudservices en virtuele Machines:

* [Maken, openen, en cloud serviceprojecten publiceren](cloud-services/cloud-services-dotnet-get-started.md).
* [De implementatie-pakketten maken voor cloud serviceprojecten](http://msdn.microsoft.com/library/ff683672.aspx).
* [Azure virtuele Machines tijdens het maken van nieuwe webprojecten maken](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Maak PowerShell-scripts tijdens het maken van nieuwe virtuele machines](http://msdn.microsoft.com/library/dn642480.aspx).
* [Weergeven en beheren van cloud service-instellingen voor project in Visual Studio-Projecteigenschappen windows](http://msdn.microsoft.com/library/ee405486.aspx).
* Weergeven en beheren van [cloudservices](http://msdn.microsoft.com/library/ff683675.aspx), [virtuele machines](http://msdn.microsoft.com/library/jj131259.aspx)en [Service Bus](http://msdn.microsoft.com/library/jj149828.aspx) in Server Explorer.
* Worden [uitgevoerd in de foutopsporingsmodus op afstand voor cloudservices en virtuele machines](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatiseren resource inrichten met Azure Resource groepsprojecten implementatie](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Hulpmiddelen voor de App-Service van Microsoft voor Visual Studio

Hiermee kunt u werken met Azure-Websites:

* [Publiceren webprojecten naar Azure-Websites](app-service-web/web-sites-dotnet-get-started.md).
* [Publiceren console-toepassing projecten naar Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Azure-Website maken en SQL-Database resources tijdens het maken van een nieuw webproject- of tijdens het publiceren van een webproject](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [PowerShell maken implementatie-scripts tijdens het maken van nieuwe Websites](http://msdn.microsoft.com/library/dn642480.aspx).
* [Beheren en problemen met Azure Websites in Server Explorer](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* Worden [uitgevoerd in de foutopsporingsmodus op afstand voor Websites en WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] U hoeft te installeren van de Azure SDK voor .NET gebruik van deze functies; ze worden ook opgenomen in Visual Studio-Updates.

##<a id="datalake"></a>Microsoft Azure-gegevens Lake Tools voor Visual Studio

Zie voor meer informatie [Zelfstudie: I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Wat is niet inbegrepen tijdens de installatie van de Azure-SDK voor .NET

Er zijn enkele dingen die u kunt voor de ontwikkeling van Azure die niet opgenomen zijn tijdens de installatie van de SDK. De belangrijkste van deze artikelen zijn de volgende:

* [Client-bibliotheken](http://go.microsoft.com/fwlink/?LinkId=510472).

    De SDK Azure bevat clientbibliotheken voor het gebruik van Azure services, maar niet allemaal zijn geïnstalleerd tijdens de installatie van de SDK. Als uw toepassing een clientbibliotheek waarin de SDK wordt niet geïnstalleerd moet, kunt u deze krijgen van [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Als uw toepassing gebruikmaakt van een clientbibliotheek waarin de SDK wordt geïnstalleerd, is een goede gewoonte deze bij te werken met de huidige versie op NuGet.org.

    **Lokale kopieën van clientbibliotheken.** De SDK Azure voor .NET wordt gekopieerd naar uw computer de NuGet-pakketten voor sommige Azure clientbibliotheken, zoals opslag, Service Bus en cache. Deze clientbibliotheken worden automatisch opgenomen in de nieuwe projecten gebruikt cloud-service, zodat de lokale NuGet-pakketten Visual Studio inschakelen maken van projecten, zelfs als u geen verbinding met Internet. Clientbibliotheken worden gewoonlijk vaker dan nieuwe SDK versies worden uitgebracht, bijgewerkt zodat de clientbibliotheken op NuGet.org vaak zijn huidige dan wat u met de SDK.

    **Project-sjablonen die clientbibliotheken bevatten.** Alleen [Azure-Cloudservice](cloud-services/cloud-services-dotnet-get-started.md) en Azure-Service voor App [Mobile-Apps](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) projectsjablonen worden automatisch enkele clientbibliotheken toevoegen. Voor andere bibliotheken of andere sjablonen, installeert u de [client bibliotheek NuGet-pakketten](http://go.microsoft.com/fwlink/?LinkId=510472) die u nodig hebt.

* [Mobile-Apps projectsjablonen](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobile-Apps-sjablonen zijn alleen beschikbaar in Visual Studio 2013 Update 2 en hoger. Ze zijn niet beschikbaar in Visual Studio 2012 of eerdere versies, en niet in Visual Studio 2013 Update 1 of eerder, zelfs als u de SDK Azure voor .NET installeren.

##<a id="faq"></a>Veelgestelde vragen

- [Vele Azure functies zijn al beschikbaar in Visual Studio. Heb ik nodig bij het installeren van de Azure-SDK voor .NET?](#azinvs)
- [Ik wil een clientbibliotheek. Heb ik de SDK Azure voor .NET om dit te installeren?](#clientlib)
- [Waar vind ik oudere versies van de Azure-SDK voor .NET?](#olderversions)
- [Wat is de levenscyclus van beleid voor de versies van de Azure SDK voor .NET?](#lifecycle)
- [Welke versies van de OS Gast de SDK Azure voor .NET compatibel is met?](#guestos)
- [Hoe kan ik de SDK Azure voor .NET verwijderen?](#uninstall)

###<a id="azinvs"></a>Vele Azure functies zijn al beschikbaar in Visual Studio. Heb ik nodig bij het installeren van de Azure-SDK voor .NET?

Het is een goede gewoonte de SDK installeren als u wilt laten groeien voor Azure met de meest recente hulpmiddelen. Als u de SDK zou liever niet geïnstalleerd, kunt u dit doen als de volgende voorwaarden voldaan wordt:

* U kunt de meest recente [Update voor Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)hebt geïnstalleerd.
* U ontwikkelt alleen voor Azure Websites of Mobile-Services, niet voor cloudservices of virtuele Machines.
* Opslag, geen gebruikt in uw toepassing of site gebruikmaakt van opslag, maar u niet nodig hebt de Emulator opslag of het hulpmiddel AzCopy.

###<a id="clientlib"></a>Ik wil een clientbibliotheek. Heb ik de SDK Azure voor .NET om dit te installeren?

De SDK installeert clientbibliotheken alleen zodat u serviceprojecten cloud maken kunt zelfs als u geen verbinding met Internet. Huidige clientbibliotheken zijn beschikbaar in NuGet-pakketten bij [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Zie [Wat is niet inbegrepen tijdens de installatie van de Azure SDK voor .NET](#notincluded) eerder in dit document voor meer informatie.

###<a id="olderversions"></a>Waar vind ik oudere versies van de Azure-SDK voor .NET?

Voor oudere versies Zie dat de [SDK van Azure voor .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -pagina downloads.

###<a id="lifecycle"></a>Wat is de levenscyclus van beleid voor de versies van de Azure SDK voor .NET?

Zie [Microsoft Azure-Cloudservices Support Lifecycle-beleid](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Welke versies van de OS Gast de SDK Azure voor .NET compatibel is met?

Zie [Azure Gast OS Releases en SDK compatibiliteit Matrix](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Hoe kan ik de SDK Azure voor .NET verwijderen?

Elk item weergegeven in dit artikel onder de [mogelijkheden van de Azure SDK voor .NET](#included) is een ander programma in de lijst met geïnstalleerde programma's in het Configuratiescherm van Windows- **programma's en onderdelen**.  Er is geen manier om ze te verwijderen als een groep; u moet elk programma afzonderlijk verwijderen.

Wanneer u de SDK Azure voor .NET al geïnstalleerd hebt en u een nieuwe versie installeren, is er in het algemeen hoeft het oude account verwijderen. In de meeste gevallen de installatie SDK bijgewerkt een bestaand programma in plaats van een nieuwe toevoegt en het oude account verlaten.

Echter als u verwijderen van de toepassing niet langer-nodig van een eerdere versie wilt, verwijdert u alleen de programma's die het oudere versienummer opgeven, en alleen verwijderen als hetzelfde programma met een nieuwere versie aanwezig is. Bijvoorbeeld na het bijwerken van 2,5 naar 2.6 ziet u mogelijk 2.5 en 2.6 versies van 'Microsoft Azure hulpmiddelen voor Microsoft Visual Studio 2013', en kunt u de 2,5 versie verwijderen. Maar ziet u mogelijk alleen de 2,5 versie van 'Microsoft Azure Authoring's ' en het zou veilige wilt verwijderen die niet.

Houd er rekening mee dat versienummers in programma titels weergegeven in **programma's en onderdelen** misleidende kunnen zijn.  SDK versie 2.6 bevat bijvoorbeeld 'Microsoft Azure mobiele App SDK V1.0' als u de SDK voor Visual Studio 2013 en "Microsoft Azure mobiele App SDK V2.0" voor Visual Studio-2015 installeren; de versie niet in dit geval de SDK-versie, maar een aanduiding van de plaats waar Visual Studio versie het programma is van toepassing op.

In dit artikel niet wordt vermeld in elke programma dat elke eerdere versie van de Azure SDK opgenomen; Er zijn andere programma's die u uit eerdere versies van SDK verwijderen kunt, omdat de eerdere versies van SDK soms opgenomen programma's die zijn weggelaten uit nieuwere versies. Bijvoorbeeld versie 2.5 installeert 'Azure Resource Manager Tools voor Visual Studio', maar die niet in de lijst van dit artikel is omdat deze niet langer weergegeven als een afzonderlijk programma in **programma's en onderdelen**.  In dit artikel bevat alleen de programma's die van de Azure-SDK voor .NET versie 2.6 uitmaken deel.

> **Notitie:** Sommige programma's die de SDK bevat ook in een andere context afzonderlijk kan zijn geïnstalleerd en mogelijk zijn er nodig zelfs als u niet nodig voor de volledige SDK hebt. Het is mogelijk dat dezelfde waar van programma's die in eerdere versies van SDK zijn geïnstalleerd maar uit hogere SDK versies zijn weggelaten. Wanneer u programma's verwijdert, worden pas op dat te voorkomen dat iets dat nodig is nog steeds op uw computer verwijderd.



##<a id="versions"></a>Versies

Zie de [SDK van Azure voor .NET-versiegeschiedenis](https://azure.microsoft.com/downloads/archive-net-downloads/) pagina om te zien welke versie is het huidige of oudere versies te downloaden.

##<a id="resources"></a>Resources

Ga naar de [pagina Downloads van Azure](https://azure.microsoft.com/downloads/)als u wilt downloaden van de huidige Azure-SDK voor .NET of een clientbibliotheek.

Zie voor de SDK Azure voor .NET-broncode, met inbegrip van clientbibliotheken, [GitHub.com/Azure](https://github.com/azure/).

Zie [Azure .NET verwijzing](https://azure.microsoft.com/documentation/api/)voor Azure client bibliotheek verwijzing documentatie.
