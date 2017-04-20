<properties
    pageTitle="Azure API Management Veelgestelde vragen over | Microsoft Azure"
    description="Informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures in Azure API Management."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API Management Veelgestelde vragen

De antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure API Management.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

-   [Hoe kan ik het team van Microsoft Azure API Management een vraag?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Wat houdt het wanneer een functie in de Preview-versie?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Hoe kan ik de verbinding tussen de API Management gateway en mijn services back-enddatabase beveiligen?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Hoe kopieer ik mijn exemplaar van de service API Management naar een nieuw exemplaar?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Kan ik mijn exemplaar API Management via programmacode beheren?](#can-i-manage-my-api-management-instance-programmatically)
-   [Hoe voeg ik een gebruiker aan de groep Administrators?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Waarom is het beleid dat ik niet beschikbaar in de editor wilt toevoegen?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Hoe gebruik ik API versiebeheer in API Management?](#how-do-i-use-api-versioning-in-api-management)
-   [Hoe stel ik meerdere omgevingen in één API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Kan ik SOAP met API Management gebruiken?](#can-i-use-soap-with-api-management)
-   [De constante API Management gateway IP-adres is? Kan ik deze in firewallregels gebruiken?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Kan ik een OAuth 2.0 autorisatie-server configureren met AD FS-beveiliging?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Welke methode gebruikt API Management in implementaties naar verschillende geografische locaties?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Kan ik een resourcemanager Azure-sjabloon gebruiken om te maken van een exemplaar van de service API Management?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Kan ik een zelfondertekend SSL-certificaat voor back-enddatabase gebruiken?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Waarom krijg ik een verificatie mislukt wanneer ik wil een cijfer opslagplaats klonen?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Werkt API Management met Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Kan ik een API Management-service uit één abonnement naar de andere verplaatsen?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Hoe kan ik het team van Microsoft Azure API Management een vraag?

U kunt contact met ons opnemen via een van de volgende opties:

-   Posten naar uw vragen onze [API Management MSDN-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Een e-mail sturen naar <apimgmt@microsoft.com>.
-   Stuur ons een verzoek voor een functie in het [Feedbackforum van Azure](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Wat houdt het wanneer een functie in de Preview-versie?

Als een functie in de proefversie is, betekent dit dat we feedback over hoe werkt de functie om actief zoek bent naar. Een functie in de Preview-versie is functioneel voltooid, maar het is mogelijk dat we een recente wijzigen in antwoord op feedback van klanten maken. Het is raadzaam dat u een functie die in het voorbeeld in uw productieomgeving is niet afhankelijk. Als u eventuele feedback preview-functies hebt, laat het ons weten via een van de opties voor contactpersonen in [hoe kan ik het team van Microsoft Azure API Management Stel een vraag?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Hoe kan ik de verbinding tussen de API Management gateway en mijn services back-enddatabase beveiligen?

U hebt verschillende opties voor het beveiligen van de verbinding tussen de API Management gateway en uw back-enddatabase-services. U kunt:

-   HTTP-basisverificatie gebruiken. Zie [Instellingen voor API configureren](api-management-howto-create-apis.md#configure-api-settings)voor meer informatie.
- SSL onderlinge verificatie gebruikt zoals is beschreven in [het beveiligen van back-enddatabase services met behulp van verificatie via clientcertificaat in Azure API Management](api-management-howto-mutual-certificates.md).
- Gebruik de IP-whitelisting van uw service back-enddatabase. Als u een standaard- of Premium laag API Management exemplaar hebt, is het IP-adres van de gateway ongewijzigd. U kunt instellen dat uw "witte" lijst toe te staan dat dit IP-adres. U kunt het IP-adres van uw exemplaar van de API Management krijgen op het Dashboard in de portal van Azure.
- Uw instantie API Management verbinden met een Azure Virtual Network. Lees [hoe u het VPN-verbindingen in Azure API Management instellen](api-management-howto-setup-vpn.md)voor meer informatie.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Hoe kopieer ik mijn exemplaar van de service API Management naar een nieuw exemplaar?

U hebt verschillende opties als u wilt een exemplaar API Management kopiëren naar een nieuw exemplaar. U kunt:

-   Gebruik van de back-up en herstellen van de functie in API-Management. Lees [hoe u implementeren herstel met behulp van de service back-up en herstellen in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md)voor meer informatie.
-   Uw eigen back-up maken en de functie herstellen via de [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Gebruik de REST API op te slaan en de entiteiten uit het exemplaar van de service die u wilt herstellen.
-   Downloaden van de service te configureren met behulp van cijfer en uploadt dit naar een nieuw exemplaar. Lees [hoe u opslaan en de API Management service-configuratie met behulp van cijfer configureren](api-management-configuration-repository-git.md)voor meer informatie.

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kan ik mijn exemplaar API Management via programmacode beheren?

Ja, kunt u de API Management via programmacode beheren met behulp van:

-   De [REST API voor API-beheer](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   De [Microsoft Azure ApiManagement Service Management bibliotheek SDK](http://aka.ms/apimsdk).
-   De [Service-implementatie](https://msdn.microsoft.com/library/mt619282.aspx) - en [Servicebeheer](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Hoe voeg ik een gebruiker aan de groep Administrators?

Hier ziet u hoe u een gebruiker kunt toevoegen aan de beheerdersgroep:

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Ga naar de resourcegroep waarop het API Management exemplaar dat u wilt bijwerken.
3. Beheer van API toewijzen de rol **Api Management Inzender** aan de gebruiker.

De toegevoegde Inzender kan nu Azure PowerShell- [cmdlets](https://msdn.microsoft.com/library/mt613507.aspx)gebruiken. Als volgt aan te melden als beheerder:

1. Gebruik de `Login-AzureRmAccount` cmdlet aan te melden.
2. Stel in de context op het abonnement waarop de service met behulp van `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Een URL voor eenmalige aanmelding verkrijgen met behulp van `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. De URL voor toegang tot de beheerportal gebruiken.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Waarom is het beleid dat ik niet beschikbaar in de editor wilt toevoegen?

Als het beleid dat u wilt toevoegen wordt grijs weergegeven of arcering in de editor, zorgen dat u in het juiste bereik voor het beleid worden weergegeven. Elke verklaring omtrent het informatiebeheerbeleid is ontworpen voor gebruik in specifieke bereiken en beleid secties. Het beleid secties en bereiken voor een beleid, Zie sectie voor het gebruik van in [API beleidsregels](https://msdn.microsoft.com/library/azure/dn894080.aspx)van het beleid.


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Hoe gebruik ik API versiebeheer in API Management?

Hebt u een paar opties API versiebeheer in API Management gebruiken:

-   U kunt beheer van API API's bevatten verschillende versies te configureren. U kunt bijvoorbeeld twee verschillende API's, MyAPIv1 en MyAPIv2 hebt. Een ontwikkelaar kunt kiezen de versie die de ontwikkelaar wil gebruiken.
-   U kunt ook uw API configureren met de URL van een service die niet zijn opgenomen in een versiesegment, bijvoorbeeld https://my.api. Configureer vervolgens een versiesegment op van elke bewerking [URL herschrijven](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) sjabloon. U kunt bijvoorbeeld een bewerking hebben een [URL-sjabloon](api-management-howto-add-operations.md#url-template) /resource en een [URL herschrijven](api-management-howto-add-operations.md#rewrite-url-template) sjabloon /v1/Resource genoemd. U kunt de waarde van het segment versie voor elke bewerking afzonderlijk wijzigen.
-   Als u wilt een "standaard" versiesegment behouden in de URL van de API's, klikt u op geselecteerde bewerkingen instellen een beleid waarin het beleid voor het [instellen van de backend-service](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) voor het wijzigen van het pad van de back-enddatabase aanvraag wordt gebruikt.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hoe stel ik meerdere omgevingen in één API?

Als u wilt instellen van meerdere omgevingen, bijvoorbeeld een testomgeving en een productieomgeving, in één API, hebt u twee opties. U kunt:

-   Host verschillende API's in dezelfde tenant.
-   De dezelfde API's op verschillende tenants hosten.

### <a name="can-i-use-soap-with-api-management"></a>Kan ik SOAP met API Management gebruiken?

Ondersteuning voor [SOAP Pass Through-query](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) is nu beschikbaar. Beheerders kunnen de WSDL van hun SOAP-service importeren en Azure API Management maakt u een SOAP-front-end. Portal documentatie voor ontwikkelaars, test-console, beleid en analyses zijn alle beschikbare voor SOAP-services.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>De constante API Management gateway IP-adres is? Kan ik deze in firewallregels gebruiken?

Aan de standaardweergave en de Premium-niveaus is het openbare IP-adres (VIP) van de tenant API Management statische voor de levensduur van de tenant, met een paar uitzonderingen. De IP-adres wijzigingen in deze omstandigheden:

-   De service is verwijderd en vervolgens opnieuw gemaakt.
-   Het serviceabonnement is geschorst (bijvoorbeeld voor nonpayment) en klik vervolgens opnieuw ingesteld.
-   U toevoegen of verwijderen van Azure Virtual Network (kunt u virtuele netwerk alleen in de Premium-laag).

Voor meerdere landen/regio-implementaties, het regionale adres verandert als het gebied is leeggemaakt en vervolgens hersteld (u kunt meerdere landen/regio-implementatie alleen in de Premium-laag).

Premium laag tenants die zijn geconfigureerd voor meerdere landen/regio-implementatie zijn toegewezen één openbare IP-adres per regio.

U kunt uw IP-adres (of adressen, in een meerdere landen/regio-implementatie) op de pagina tenant in de portal van Azure krijgen.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kan ik een OAuth 2.0 autorisatie-server configureren met AD FS-beveiliging?

Zie informatie over het configureren van een OAuth 2.0 autorisatie-server met Active Directory Federation Services (AD FS) beveiliging, [ADFS gebruiken in API-Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Welke methode gebruikt API Management in implementaties naar verschillende geografische locaties?

De [prestaties verkeer routeren methode](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) API Management gebruikt in implementaties naar verschillende geografische locaties. Binnenkomende verkeer wordt doorgestuurd naar de dichtstbijzijnde API-gateway. Als één regio offline gaat, wordt automatisch binnenkomende verkeer naar de volgende eerstvolgende gateway gerouteerd. Meer informatie over methoden voor het doorsturen in het [verkeer Manager routering methoden](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kan ik een resourcemanager Azure-sjabloon gebruiken om te maken van een exemplaar van de service API Management?

Ja. Overzicht van de [Azure API-beheerservice](http://aka.ms/apimtemplate) QuickStart-sjablonen.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kan ik een zelfondertekend SSL-certificaat voor back-enddatabase gebruiken?

Ja. Hier volgt een zelfondertekend certificaat voor Secure Sockets Layer (SSL) gebruiken voor back-enddatabase:

1. Maak een [back-end](https://msdn.microsoft.com/library/azure/dn935030.aspx) -entiteit met behulp van API-Management.
2. Stel de eigenschap **skipCertificateChainValidation** op **true**.
3. Als u niet langer toestaan dat zelfondertekend certificaten wilt, de Backend-entiteit verwijderen of stel de eigenschap **skipCertificateChainValidation** op **Onwaar**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Waarom krijg ik een verificatie mislukt wanneer ik wil een cijfer opslagplaats klonen?

Als u cijfer Referentiebeheer gebruikt, of als u probeert te klonen een cijfer opslagplaats met behulp van Visual Studio, kunt u een bekend probleem bij het dialoogvenster van de Windows-referenties kan optreden. Het dialoogvenster bestandsgrootten wachtwoordlengte 127 tekens en het wachtwoord Microsoft gegenereerde worden afgekapt. We werken aan het wachtwoord te verkorten. Gebruik cijfer we vaker doen om uw bibliotheek cijfer klonen voorlopig.

### <a name="does-api-management-work-with-azure-expressroute"></a>Werkt API Management met Azure ExpressRoute?

Ja. API Management werkt met Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Kan ik een API Management-service uit één abonnement naar de andere verplaatsen?

Ja. Voor meer informatie hierover leest u [resources aan een nieuwe resourcegroep of het abonnement te verplaatsen](../resource-group-move-resources.md).
