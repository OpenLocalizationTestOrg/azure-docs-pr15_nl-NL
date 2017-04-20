<properties 
    pageTitle="Handleiding voor ontwikkelaars van Azure overheid" 
    description="Dit vindt u een vergelijking van functies en informatie over het ontwikkelen van toepassingen voor de overheid van Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Handleiding voor ontwikkelaars van Microsoft Azure overheid 

<p> Microsoft Azure overheid is een fysiek en netwerk geïsoleerd exemplaar van Microsoft Azure.  Deze handleiding voor ontwikkelaars, vindt u meer informatie over de verschillen die softwareontwikkelaars en beheerders nodig hebt om te communiceren en werken met deze afzonderlijk gebieden van Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>In dit onderwerp


+ [Overzicht](#Overview)
+ [Richtlijnen voor ontwikkelaars](#Guidance)
+ [Functies die momenteel beschikbaar in Microsoft Azure overheid](#Features)
+ [Eindpunt toewijzing](#Endpoint)
+ [Volgende stappen](#next)


## <a name="Overview"></a>Overzicht

Microsoft Azure overheid is een afzonderlijk exemplaar van de Microsoft Azure-service adressering van de behoeften beveiliging en naleving van Amerikaanse federale overheidsinstanties helpt, provincie en lokale overheden en hun providers oplossingen. Azure overheid biedt fysiek en moeten worden geïsoleerd van niet-Amerikaanse overheid implementaties netwerk en biedt medewerkers van de VS gecontroleerd. 

Microsoft biedt een aantal hulpmiddelen voor het maken en implementeren van cloud-toepassingen naar de globale Microsoft Azure-service ("globale Service") en de overheid van Microsoft Azure services van Microsoft.

Bij het maken en implementeren van toepassingen op de ontwikkelaars Azure overheid Services, in plaats van de globale Service, moet u moet weten van de belangrijkste verschillen tussen de twee services.  Specifiek rond instellen en configureren van hun programming omgeving, configureren van eindpunten toepassingen schrijven en deze implementeert als services naar Azure overheid.

De informatie in dit document bevat een overzicht van deze verschillen en is een aanvulling van de gegevens die beschikbaar zijn op de site van de(http://www.azure.com/gov "Azure overheid") [Azure overheid]en de [Technische bibliotheek voor Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") op MSDN. Officiële gegevens ook beschikbaar zijn op vele andere locaties, zoals de [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ ' Microsoft Azure Trust Center "/), [Azure documentatie beheercentrum](https://azure.microsoft.com/documentation/) en in [Azure Blogs] (https://azure.microsoft.com/blog/"Azure Blogs"/). 

Deze inhoud is bedoeld voor partners en ontwikkelaars die op Microsoft Azure overheid implementeert.



## <a name="Guidance"></a>Richtlijnen voor ontwikkelaars
Omdat het grootste deel van de technische inhoud die is momenteel beschikbaar wordt ervan uitgegaan dat toepassingen worden ontwikkeld voor de globale Service en niet voor Microsoft Azure overheid, is het is belangrijk om ervoor te zorgen dat ontwikkelaars bekend zijn met belangrijke verschillen voor toepassingen die zijn ontwikkeld om te worden gehost in Azure overheid.

- Eerst er services en Functieverschillen, betekent dit dat bepaalde functies die in bepaalde gebieden van de globale Service zijn mogelijk niet beschikbaar in Azure overheid.

- Tweede, voor de functies die worden aangeboden in Azure overheid, zijn er verschillen van de configuratie van de globale Service.  U moet daarom uw voorbeeldcode, configuraties en stappen om ervoor te zorgen dat u bouwen en vanuit de Azure overheid Cloud Services-omgeving uitvoeren controleren.


## <a name="Features"></a>Functies die momenteel beschikbaar in Microsoft Azure overheid
Azure overheid heeft de volgende services beschikbaar in ons BEURS IOWA zowel ons BEURS VIRGINIA regio's:

- Virtuele Machines
- Cloudservices
- Opslag
- Active Directory
- Planner
- Virtuele netwerken
- SQL-Database
- Azure-bestanden
- Mediaservices
- Verkeer Manager
- Service Bus
- StorSimple
- Bestand Vgx Cache.
- Azure back-up maken
- Automatisering
- ExpressRoute
- enzovoort.

Andere services beschikbaar zijn, en meer services, worden toegevoegd op doorlopend.  Voor de meest recente lijst met services, raadpleegt u de [pagina van de regio's](https://azure.microsoft.com/regions/#services) die worden gemarkeerd elke beschikbare regio en hun services.  

ONS BEURS Iowa en ons BEURS Virginia zijn momenteel de datacenters ondersteunende Azure overheid.  Raadpleeg de regio's, boven voor huidige datacenters en services beschikbaar.

## <a name="Endpoint"></a>Eindpunt toewijzing

Gebruik de volgende tabel om u te helpen bij het toewijzen van openbare Microsoft Azure en SQL-Database eindpunten naar Azure overheid specifieke eindpunten.


Servicetype|Azure openbare|Azure overheid
---|---|---
Management Portal|Manage.windowsazure.com|Manage.windowsazure.us
Algemene|*. windows.net|*. usgovcloudapi.net
Core|*. core.windows.net|*. core.usgovcloudapi.net
Berekenen|*. cloudapp.net|*. usgovcloudapp.net
-Blobopslag|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Wachtrij opslag|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Table Storage|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Servicebeheer|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL-Database|*. database.windows.net|*. database.usgovcloudapi.net
Op ARM laden verdeeld eindpunt|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* Op ARM-verificatie via Azure AD, vindt u over [Verificatie Azure resourcemanager aanvragen](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Volgende stappen

Als u geïnteresseerd bent in zal het leren van meer en over Azure overheid neemt gebruikmaken van enkele van de onderstaande koppelingen.

- **[Registreren voor een proefabonnement](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Bij het verkrijgen van en toegang krijgen tot het Azure overheid](http://azure.com/gov)**

- **[Overzicht van de Azure overheid](/azure-government-overview)**

- **[Azure Government-Blog](http://blogs.msdn.com/b/azuregov/)**

- **[Naleving van Azure](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
