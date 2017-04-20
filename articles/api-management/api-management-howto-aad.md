<properties 
    pageTitle="Hoe u Autoriseer ontwikkelaars accounts met Azure Active Directory in Azure API Management" 
    description="Leer hoe u gebruikers met Azure Active Directory in API Management machtigen." 
    services="api-management" 
    documentationCenter="API Management" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hoe u Autoriseer ontwikkelaars accounts met Azure Active Directory in Azure API Management


## <a name="overview"></a>Overzicht
Deze handleiding ziet u hoe u toegang tot de developer portal inschakelen voor alle gebruikers in een of meer Azure Active Directory. Deze handleiding ziet u ook hoe u groepen Azure Active Directory-gebruikers beheren door externe groepen die de gebruikers van een Azure Active Directory bevatten.

>Als de stappen in deze handleiding wilt uitvoeren, moet u eerst een Azure Active Directory voor het maken van een toepassing hebben.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Hoe u Autoriseer ontwikkelaars accounts met Azure Active Directory

Als u wilt beginnen, klikt u op **beheren** in de klassieke Azure-Portal voor uw service API-Management. Hiermee gaat u naar de publisher-API-beheerportal.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

Klik op **beveiliging** in het menu **API-beheer** aan de linkerkant en klik op **Externe identiteiten**.

![Externe identiteiten][api-management-security-external-identities]

Klik op **Azure Active Directory**. Noteer de **URL omleiden** en de schakeloptie naar uw Azure Active Directory in de klassieke Azure-Portal.

![Externe identiteiten][api-management-security-aad-new]

Klik op de knop **toevoegen** om een nieuwe Azure Active Directory-toepassing te maken en kies van **toepassing is bij het ontwikkelen van mijn organisatie toevoegen**.

![Nieuwe Azure Active Directory-toepassing toevoegen][api-management-new-aad-application-menu]

Voer een naam voor de toepassing, selecteert u **webtoepassing en/of Web API**en klik op de knop Volgende.

![Nieuwe Azure Active Directory-toepassing][api-management-new-aad-application-1]

Voer de URL in het veld eenmalige aanmelding van uw developer portal voor **eenmalige aanmelding URL**. In dit voorbeeld de **aanmelding URL** is `https://aad03.portal.current.int-azure-api.net/signin`. 

Voor de **URL van de App-ID**, geef het standaarddomein of een aangepast domein voor de Azure Active Directory en een unieke reeks toevoegen. In dit voorbeeld wordt het standaarddomein van **https://contoso5api.onmicrosoft.com** gebruikt met het achtervoegsel van **/api** opgegeven.

![Nieuwe eigenschappen van de Azure Active Directory-toepassing][api-management-new-aad-application-2]

Klik op de knop controleren om te slaan en de nieuwe toepassing te maken en Ga naar het tabblad **configureren** voor het configureren van de nieuwe toepassing.

![Nieuwe Azure Active Directory-toepassing die zijn gemaakt][api-management-new-aad-app-created]

Als meerdere Azure Active Directory's gaat moet worden gebruikt voor deze toepassing, klik op **Ja** voor de **toepassing meerdere tenant is**. De standaardinstelling is **Nee**.

![Toepassing is meerdere tenant][api-management-aad-app-multi-tenant]

Kopieer de **Omleidings-URL** van de **Azure Active Directory** -sectie van het tabblad **Externe identiteiten** in de publisher-portal en plak deze in het tekstvak **URL beantwoorden** . 

![Antwoord URL][api-management-aad-reply-url]

Schuif naar de onderkant van het tabblad configureren, selecteert u de **Machtigingen van toepassing** vervolgkeuzelijst en controleren van de **gegevens van de map lezen**.

![Toepassingsmachtigingen][api-management-aad-app-permissions]

Selecteer de vervolgkeuzelijst met **Machtigingen voor gemachtigden** en schakel **schakelen eenmalige aanmelding en profielen van gebruikers te lezen**.

![Machtigingen voor gedelegeerd][api-management-aad-delegated-permissions]

>Zie [toegang tot de API Graph][]voor meer informatie over de toepassing en gedelegeerde machtigingen.

De **Client-Id** naar het Klembord kopiëren.

![Cliënt-Id][api-management-aad-app-client-id]

Ga terug naar de publisher-portal en plakken in de **Client-Id** hebt gekopieerd uit de configuratie van de Azure Active Directory-toepassing.

![Cliënt-Id][api-management-client-id]

Ga terug naar de Azure Active Directory-configuratie, klik op de **duur Selecteer** vervolgkeuzelijst in de sectie **toetsen** en een interval opgeven. In dit voorbeeld wordt **1 jaar** gebruikt.

![Toets][api-management-aad-key-before-save]

Klik op **Opslaan** om de configuratie opslaan en de toets weer te geven. De toets naar het Klembord kopiëren.

>Noteer deze toets. Wanneer u het venster van de configuratie Azure Active Directory hebt gesloten, kan de toets niet opnieuw worden weergegeven.

![Toets][api-management-aad-key-after-save]

Ga terug naar de publisher-portal en plak de toets in het tekstvak **Geheim Client** .

![Geheim client][api-management-client-secret]

**Toegestane Tenants** geeft aan welke mappen hebben toegang tot de API's van het exemplaar van de service API Management. Geef de domeinen van de Azure Active Directory-exemplaren waartoe u toegang wilt verlenen. U kunt meerdere domeinen scheiden met nieuwe regels, spaties of komma's.

![Toegestane tenants][api-management-client-allowed-tenants]

Meerdere domeinen kunnen worden opgegeven in de sectie **Tenants toegestaan** . Voordat u elke gebruiker kan zich aanmelden vanuit een ander domein dan het oorspronkelijke domein waarvoor de aanvraag is geregistreerd, moet een globale beheerder van de verschillende domein toestemming voor de toepassing op access-gegevens voor directory verlenen. Als u wilt machtigen, moet een globale beheerder Meld u aan bij de toepassing en klik op **accepteren**. In het volgende voorbeeld `miaoaad.onmicrosoft.com` is toegevoegd aan **Toegestane Tenants** en een globale beheerder van dat domein is voor de eerste keer aanmelden.

![Machtigingen][api-management-permissions-form]

>Als de beheerder van een niet-algemene probeert aan te melden voordat u een globale beheerder machtigingen worden verleend, wordt de aanmeldingspoging mislukt en wordt een fout wordt weergegeven.

Wanneer de gewenste configuratie is opgegeven, klikt u op **Opslaan**.

![Opslaan][api-management-client-allowed-tenants-save]

Nadat u de wijzigingen die zijn opgeslagen, kunnen gebruikers in de opgegeven Azure Active Directory volgens de stappen in de [Meld u aan bij de Developer portal Azure Active Directory-account gebruikt][]zich aanmelden bij de Developer portal.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Het toevoegen van een externe Azure Active Directory-groep

Nadat u hebt toegang voor gebruikers in een Azure Active Directory inschakelt, kunt u Azure Active Directory-groepen toevoegen in het beheer van de API voor het beheren van de koppeling van de ontwikkelaars in de groep met de gewenste producten eenvoudiger.

> Voordat u een externe Azure Active Directory-groep configureren, moet de Azure Active Directory eerst worden geconfigureerd in het tabblad identiteiten volgens de procedure in de vorige sectie. 

Externe Azure Active Directory-groepen worden toegevoegd op het tabblad **zichtbaarheid** van het product waarvoor u toegang verlenen aan de groep wilt maken. Klik op **producten**en klik vervolgens op de naam van het gewenste product.

![Product configureren][api-management-configure-product]

Ga naar het tabblad **zichtbaarheid** en klikt u op **Groepen van Azure Active Directory toevoegen**.

![Groepen toevoegen][api-management-add-groups]

Selecteer de **Azure Active Directory-Tenant** in de vervolgkeuzelijst en typ de naam van de gewenste groep in de **groepen** tekstvak wordt toegevoegd.

![Selecteer groep][api-management-select-group]

De naam van deze groep vindt u in de lijst met **groepen** voor uw Azure Active Directory, zoals wordt weergegeven in het volgende voorbeeld.

![Lijst met Azure Active Directory-groepen][api-management-aad-groups-list]

Klik op **toevoegen** om te valideren van de groepsnaam en de groep toevoegen. In dit voorbeeld de **Contoso-5-ontwikkelaars** wordt externe groep toegevoegd. 

![Groep toegevoegd][api-management-aad-group-added]

Klik op **Opslaan** als de selectie van de nieuwe groep wilt opslaan.

Wanneer een Azure Azure Active Directory-groep uit één product is geconfigureerd, is deze beschikbaar moet worden gecontroleerd op het tabblad **zichtbaarheid** voor de andere producten in het exemplaar van de service API Management.

Als u wilt controleren en de eigenschappen voor externe groepen configureren nadat ze zijn toegevoegd, klikt u op de naam van de groep op het tabblad **groepen** .

![Groepen beheren][api-management-groups]

Hier kunt u de **naam** en een **Beschrijving** van de groep bewerken.

![Groep bewerken][api-management-edit-group]

Gebruikers van de geconfigureerde Azure Active Directory kunnen Meld u aan bij de portal voor ontwikkelaars en de weergave en u hierop abonneren aan groepen waartoe ze zichtbaarheid hebt volgens de instructies in de volgende sectie.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Hoe meld u aan bij de Developer portal Azure Active Directory-account gebruikt

Als u wilt Meld u aan bij de Developer portal geconfigureerd in de vorige gedeelten Azure Active Directory-account gebruikt, opent u een nieuw browservenster via de **URL voor eenmalige aanmelding** van de configuratie van de Active Directory-toepassing en **Azure Active Directory**op.

![Developer Portal][api-management-dev-portal-signin]

Voer de referenties van een van de gebruikers in uw Azure Active Directory en klikt u op **aanmelden**.

![Aanmelden][api-management-aad-signin]

U mogelijk gevraagd met een registratieformulier als eventueel aanvullende informatie vereist is. Vul het registratieformulier en klik op **aanmelden**.

![Registratie][api-management-complete-registration]

De gebruiker is nu aangemeld bij de portal voor ontwikkelaars voor uw exemplaar van de service API Management.

![Registratie voltooid][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Toegang krijgen tot de API-grafiek]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Meld u aan bij de Developer portal Azure Active Directory-account gebruikt]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

