<properties
    pageTitle="Een aangepaste domeinnaam voor een web-app configureren in Azure App Service die verkeer Manager voor taakverdeling wordt gebruikt."
    description="Een aangepaste domeinnaam gebruiken voor een een WebApp in Azure-Service voor App dat verkeer Manager voor taakverdeling bevat."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Het configureren van een aangepaste domeinnaam voor een web-app in Azure App-Service met verkeer Manager

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

In dit artikel bevat algemene instructies voor het gebruik van een aangepaste domeinnaam met Azure App Service die verkeer Manager voor taakverdeling gebruiken.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Informatie over DNS-records

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Uw WebApps voor standaardmodus configureren

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Een DNS-record voor uw aangepaste domein toevoegen

> [AZURE.NOTE] Als u domein via Azure App Service Web-Apps hebt aangeschaft overslaan stappen te volgen en verwijzen naar de laatste stap van de [Web Apps-domein kopen](custom-dns-web-site-buydomains-web-app.md) artikel.

Om uw aangepaste domein koppelen aan een WebApp in Azure App-Service, moet u een nieuwe vermelding in de tabel DNS voor uw aangepaste domein toevoegen met behulp van hulpmiddelen voor gekregen van de domeinregistrar die u uit de naam van uw domein hebt gekocht. Gebruik de volgende stappen uit om te zoeken en gebruikt u de DNS-hulpmiddelen.

1. Meld u aan bij uw account bij uw domeinregistrar en zoek naar een pagina voor het beheer van DNS-records. Vindt u koppelingen of gebieden van de site die is gelabeld als **Domeinnaam**, **DNS**of **De naam van Server Management**. Vaak een koppeling naar deze pagina kan worden gevonden worden gegevens van uw account weergeven en klik vervolgens op zoek naar een koppeling zoals **My domains**.

1. Zodra u de beheerpagina voor uw domeinnaam gevonden hebt, zoekt u een koppeling waarmee u de DNS-records kunt bewerken. Dit kan worden weergegeven als een **Zone file**, **DNS-Records**, of als een koppeling van de configuratie **Geavanceerd** .

    * De pagina waarschijnlijk heeft een paar records hebt gemaakt, zoals het koppelen van een vermelding '**@**'of'\*' met een pagina 'domein parkeren'. Het kan ook records voor algemene subdomeinen, zoals **www**bevatten.
    * De pagina wordt vermelden **CNAME-records**of geef een vervolgkeuzelijst als u een recordtype. Dit kan ook andere records zoals **een records** en de **MX-records**worden vermeld. In sommige gevallen wordt de CNAME-records worden aangeroepen door de namen van andere zoals een **Alias Record**.
    * De pagina wordt ook velden waarmee u naar de **map** uit een **hostnaam** of de **domeinnaam** naar een andere domeinnaam hebben.

1. Terwijl de details van elke registrar variëren, in het algemeen u *van* toewijzen de naam van uw aangepaste domein (zoals **contoso.com**,) *naar* de verkeer Manager domeinnaam (**contoso.trafficmanager.net**) die wordt gebruikt voor uw web-app.

    > [AZURE.NOTE] Als een record al gebruikt wordt en u moet uw apps preemptively binden ernaar toevoegen, kunt u ook een extra CNAME-record maken. Bijvoorbeeld als u wilt koppelen preemptively **www.contoso.com** naar uw web-app, maken een CNAME-record van **awverify.www** naar **contoso.trafficmanager.net**. U kunt vervolgens 'www.contoso.com' toevoegen aan uw Web-App zonder te wijzigen van de 'www' CNAME-record. Zie voor meer informatie [DNS-records maken voor een web-app in een aangepast domein][CREATEDNS].

1. Wanneer u klaar bent met het toevoegen of wijzigen van de DNS-records bij uw domeinregistrar, moet u de wijzigingen opslaan.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Verkeer Manager inschakelen

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Volgende stappen

Zie het [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
