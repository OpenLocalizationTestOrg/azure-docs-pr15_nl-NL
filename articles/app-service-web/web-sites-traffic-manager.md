<properties
    pageTitle="Azure web app-verkeer met Azure verkeer Manager bepalen"
    description="Dit artikel bevat overzichtsinformatie voor Azure verkeer Manager met betrekking tot Azure-Webapps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Azure web app-verkeer met Azure verkeer Manager bepalen

> [AZURE.NOTE] Dit artikel bevat een samenvatting van de gegevens voor Microsoft Azure verkeer Manager met betrekking tot Azure App Service Web Apps. Via de koppelingen aan het einde van dit artikel vindt u meer informatie over Azure verkeer Manager zelf.

## <a name="introduction"></a>Inleiding
U kunt Azure verkeer Manager gebruiken om te bepalen hoe aanmeldingsaanvragen van webclients zijn verdeeld over WebApps in Azure App-Service. Wanneer de eindpunten van web-app zijn toegevoegd aan een profiel Azure verkeer Manager, Azure verkeer Manager houdt van de status van uw WebApps (uitgevoerd, gestopt of verwijderde) zodat deze kunt bepalen welke van deze eindpunten verkeer moet ontvangen.

## <a name="load-balancing-methods"></a>Laden van taakverdeling methoden
Azure verkeer Manager gebruikt drie verschillende laden taakverdeling methoden. Deze worden beschreven in de volgende lijst als ze Azure-WebApps betrekking hebben.

* **Failover**: als u web app klonen in verschillende regio's hebt, kunt u deze methode te gebruiken voor het configureren van een WebApp af te handelen, alle web clientverkeer en configureren van een andere WebApp in een ander gebied aan dat verkeer service geval de eerste web-app niet meer beschikbaar.

* **Round Robin**: als u web app klonen in verschillende regio's hebt, kunt u deze methode gebruiken om te verkeer gelijkmatig verdelen over de web-apps in verschillende regio's.

* **Prestaties**: prestaties van de methode verkeer op basis van de kortste retourtijd met clients distribueert. De methode prestaties kan worden gebruikt voor web-apps binnen dezelfde regio of in verschillende regio's.

##<a name="web-apps-and-traffic-manager-profiles"></a>WebApps en verkeer Manager profielen
Als u wilt configureren de besturing van web app-verkeer is toegestaan, kunt u een profiel maken in Azure verkeer Manager dat wordt gebruikt een van de drie taakverdeling methoden hierboven laden, en voegt u vervolgens de eindpunten (in dit geval WebApps) waarvoor u wilt bepalen van verkeer naar het profiel toe. Uw web app-status (gestart, gestopt of verwijderd) wordt regelmatig doorgegeven aan het profiel dat Azure verkeer Manager kunt verkeer dienovereenkomstig gewijzigd.

Wanneer u Azure verkeer Manager met Azure, houd er rekening mee de volgende punten:

* Voor web app alleen implementaties binnen dezelfde regio biedt WebApps al failover en round robin functionaliteit zonder web app-modus.

* Voor implementaties in hetzelfde gebied die Web Apps in combinatie met een andere Azure-cloudservice gebruiken, kunt u beide soorten eindpunten naar het inschakelen van scenario's voor hybride combineren.

* U kunt slechts één web app-eindpunt per regio in een profiel opgeven. Wanneer u een web-app als een eindpunt voor één regio selecteert, worden de resterende web-apps in dat gebied niet beschikbaar voor selectie voor dat profiel.

* De eindpunten van de web-app die u in een profiel Azure verkeer Manager opgeeft wordt weergegeven onder de sectie **Domain Names** op de pagina configureren voor de web-app in het profiel, maar kunnen niet worden geconfigureerd er.

* Nadat u een web-app aan een profiel toevoegen, kan de **URL van de Site** op het Dashboard van de web-app-portalpagina het aangepaste domein-URL van de web-app wordt weergegeven als u deze hebt ingesteld. Alle andere gevallen de URL voor het profiel van verkeer Manager wordt weergegeven (bijvoorbeeld `contoso.trafficmgr.com`). Zowel de directe domeinnaam van de web-app en de URL van verkeer Manager is zichtbaar is op de web-app configureren pagina onder de sectie **Domain Names** .

* De namen van uw aangepaste domein werkt zoals verwacht, maar naast deze toe te voegen aan uw WebApps, moet u ook de DNS-plattegrond zodat deze verwijzen naar de URL van verkeer Manager configureren. Zie [configureren van een aangepaste domeinnaam voor een Azure website](web-sites-custom-domain-name.md)voor informatie over het instellen van een aangepast domein voor een Azure web-app.

* U kunt alleen WebApps die in de standaardmodus aan een profiel Azure verkeer Manager zijn toevoegen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht conceptuele en technische van Azure verkeer Manager [Verkeer Manager overzicht](../traffic-manager/traffic-manager-overview.md).

Zie dat de blogberichten [Using Azure verkeer Manager met Azure websites](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) en [Azure verkeer Manager kunt nu integreren met Azure websites](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/)voor meer informatie over het gebruik van de Manager verkeer met Web Apps.
