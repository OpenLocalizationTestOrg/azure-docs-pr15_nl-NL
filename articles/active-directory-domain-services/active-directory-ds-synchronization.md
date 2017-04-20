<properties
    pageTitle="Azure Active Directory Domain Services: Synchronisatie in beheerde domeinen | Microsoft Azure"
    description="Meer informatie over synchronisatie in een beheerde Azure Active Directory Domain Services-domein"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronisatie in een beheerde domeinservices van Azure AD-domein
In het volgende diagram ziet u hoe synchronisatie werkt in Azure AD-domeinservices beheerde domeinen.

![Topologie van de synchronisatie van Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronisatie van uw on-premises adreslijst aan bij uw Azure AD-tenant
Azure AD Connect-synchronisatie wordt gebruikt om te synchroniseren gebruikersaccounts, groeperen lidmaatschappen en referentie ervoor zorgt aan bij uw Azure AD-tenant. Kenmerken van de gebruiker accounts zoals de UPN en on-premises beveiligings-id (beveiligings-id) worden gesynchroniseerd. Als u Azure AD Domain Services gebruikt, worden ook oudere referentie hashes die zijn vereist voor NTLM als Kerberos-verificatie gesynchroniseerd naar uw Azure AD-tenant.

Als u terugschrijven configureert, worden wijzigingen in uw adreslijst Azure AD optreedt terug naar uw lokale Active Directory gesynchroniseerd. Bijvoorbeeld als u uw wachtwoord met Azure AD selfservice-wachtwoord wijzigen functies wijzigen, het gewijzigde wachtwoord wordt bijgewerkt in uw on-premises AD-domein.

> [AZURE.NOTE] Altijd de nieuwste versie van Azure AD Connect gebruiken om ervoor te zorgen er correcties voor alle bekende bugs bij te werken.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronisatie van uw Azure AD-tenant naar uw beheerde domein
Gebruikersaccounts, groepslidmaatschap en referentie hashes wordt gesynchroniseerd vanuit uw Azure AD-tenant naar uw beheerde domeinservices van Azure AD-domein. Dit synchronisatieproces wordt automatisch. U hoeft niet te configureren, controleren of dit synchronisatieproces beheren. Het synchronisatieproces is ook een / way/één richting van aard. Uw beheerde domein is grotendeels alleen-lezen, behalve voor een aangepaste OU's die u maakt. Daarom niet kunt u wijzigingen aanbrengt in gebruikerskenmerken, gebruikerswachtwoorden of groepslidmaatschap binnen het beheerde domein. Hierdoor wordt er geen omgekeerde synchronisatie van wijzigingen vanaf uw domein beheerde terug naar uw Azure AD-tenant.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronisatie van een meerdere bos on-premises omgeving
Veel hebben organisaties een vrij gecompliceerde on-premises implementatie identiteit infrastructuur die bestaat uit meerdere account forests. Azure AD Connect ondersteunt synchroniseren gebruikers, groepen en referentie ogen uit meerdere bos omgevingen van uw Azure AD-tenant.

Uw Azure AD-tenant is daarentegen een veel eenvoudiger en platte naamruimte. Als u wilt dat gebruikers naar betrouwbaar toegang tot toepassingen die zijn beveiligd met Azure AD, conflicten UPN tussen gebruikersaccounts in verschillende forests. Uw Azure AD-domeinservices beheerde domein beren sluit procesontwerp aan bij uw Azure AD-tenant. U ziet er daarom een platte OU-structuur in uw beheerde domein. Alle gebruikers en groepen worden opgeslagen in de container 'AADDC Users', ongeacht de on-premises domein of bos waaruit ze zijn gesynchroniseerd in. Hebt u een hiërarchische OU geconfigureerd structureren on-premises implementatie. Uw beheerde domein heeft echter nog steeds een eenvoudige platte OU-structuur.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Uitsluitingen - wat is niet gesynchroniseerd met uw beheerde domein
De volgende objecten of -kenmerken worden niet gesynchroniseerd naar uw Azure AD-tenant of naar uw beheerde domein:

- **Die zijn uitgesloten kenmerken:** U kunt kiezen dat u wilt uitsluiten van bepaalde kenmerken van synchroniseren met uw Azure AD-tenant uit uw on-premises domein met Azure AD Connect. Deze uitgesloten kenmerken zijn niet beschikbaar in uw beheerde domein.

- **Beleidsregels groeperen:** Groepsbeleid dat is geconfigureerd in uw on-premises domein worden niet gesynchroniseerd naar uw beheerde domein.

- **SYSVOL delen:** De inhoud van de share SYSVOL op uw on-premises domein worden ook niet gesynchroniseerd naar uw beheerde domein.

- **Computer-objecten:** Computerobjecten voor computers deel uitmaken van uw on-premises domein worden niet gesynchroniseerd naar uw beheerde domein. Deze computers niet hebben van een vertrouwensrelatie met uw beheerde domein en deel uitmaakt van uw on-premises domein. Zoek in uw domein beheerde computerobjecten alleen bestemd voor computers die u hebt expliciet domein-toegevoegd aan het beheerde domein.

- **SidHistory kenmerken voor gebruikers en groepen:** De primaire gebruiker en primaire groep beveiligings-id's uit uw on-premises domein worden gesynchroniseerd met uw beheerde domein. Bestaande SidHistory kenmerken voor gebruikers en groepen worden echter niet gesynchroniseerd vanuit uw on-premises domein naar uw beheerde domein.

- **Organisatie eenheden (OU) structuren:** Organisatie-eenheden die zijn gedefinieerd in uw on-premises domein worden niet gesynchroniseerd naar uw beheerde domein. Er zijn twee ingebouwde organisatie-eenheden in uw beheerde domein. Standaard is uw beheerde domein een platte OU-structuur. U kunt echter [een aangepaste organisatie-eenheid in uw beheerde domein](./active-directory-ds-admin-guide-create-ou.md)maken.


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Hoe specifieke kenmerken worden gesynchroniseerd naar uw beheerde domein
De volgende tabel bevat enkele algemene kenmerken en wordt beschreven hoe deze worden gesynchroniseerd naar uw beheerde domein.

| Kenmerk in uw beheerde domein | Bron | Notities |
|:---|:---|:---|
|UPN|UPN-kenmerk van de gebruiker in uw Azure AD-tenant|Het kenmerk UPN van uw Azure AD-tenant is gesynchroniseerd naar uw beheerde domein is. De meest betrouwbare manier om aan te melden bij uw beheerde domein wordt daarom uw UPN gebruiken.|
|SAMAccountName|Gebruikerswachtwoord mailNickname kenmerk in uw Azure AD-tenant of automatisch gegenereerde|De SAMAccountName-kenmerk is die afkomstig is van het kenmerk mailNickname in uw Azure AD-tenant. Als u meerdere gebruikersaccounts hebt hetzelfde mailNickname kenmerk, is de SAMAccountName automatisch gegenereerde. Als de mailNickname of UPN voorvoegsel van de gebruiker meer dan 20 tekens is, is de SAMAccountName automatisch gegenereerde om te voldoen aan de 20 tekenlimiet op SAMAccountName kenmerken.|
|Wachtwoorden|Gebruikerswachtwoord van uw Azure AD-tenant|Referentie-hashes die zijn vereist voor NTLM of Kerberos-verificatie (ook wel aanvullende referenties genoemd) wordt gesynchroniseerd vanuit uw Azure AD-tenant. Als uw Azure AD-tenant een gesynchroniseerde tenant is, deze referenties zijn die afkomstig is van uw on-premises domein.|
|Primaire gebruiker/groep beveiligings-id|Automatisch gegenereerde|De primaire beveiligings-id voor de gebruiker/groep accounts is automatisch gegenereerde in uw beheerde domein. Dit kenmerk niet overeenkomen met de primaire gebruiker/groep beveiligings-id van het object in uw on-premises AD-domein. Deze incompatibiliteit is omdat het beheerde domein een andere beveiligings-id-naamruimte dan uw on-premises domein heeft.|
|Beveiligings-ID-geschiedenis voor gebruikers en groepen|Primaire gebruiker van on-premises implementatie en groep|Het kenmerk SidHistory voor gebruikers en groepen in uw beheerde domein is ingesteld zodat deze overeenkomt met de bijbehorende primaire gebruiker of groep in uw on-premises domein. Deze functie kunt gemakkelijker lift-en-shift van on-premises implementatie-toepassingen naar de beheerde domein, aangezien u niet te opnieuw-ACL resources hoeft.|

> [AZURE.NOTE] **Meld u aan bij de beheerde domein met de UPN-opmaak:** De SAMAccountName-kenmerk mogelijk automatisch wordt gegenereerd voor sommige gebruikersaccounts in uw beheerde domein. Als meerdere gebruikers hetzelfde mailNickname kenmerk hebben of gebruikers te lange UPN voorvoegsels hebben, moet u de SAMAccountName voor deze gebruikers mogelijk automatisch gegenereerde. De SAMAccountName-indeling (bijvoorbeeld ' CONTOSO100\joeuser') dus niet altijd een betrouwbare manier om het aanmelden bij het domein. Gebruikers automatisch gegenereerde SAMAccountName kan afwijken van hun UPN-voorvoegsel. Gebruik de UPN-indeling (bijvoorbeeld 'joeuser@contoso100.com') naar betrouwbaar aanmelden bij het beheerde domein.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objecten die niet worden gesynchroniseerd met uw Azure AD-tenant vanaf uw beheerde domein
Als in een voorgaande sectie in dit artikel wordt beschreven, is er geen synchronisatie van uw domein beheerde terug naar uw Azure AD-tenant. U kunt [Een aangepaste organisatie-eenheid](./active-directory-ds-admin-guide-create-ou.md) maken in uw beheerde domein. Bovendien kunt u andere organisatie-eenheden, gebruikers, groepen of serviceaccounts binnen deze aangepaste organisatie-eenheden. Geen van de objecten die zijn gemaakt binnen aangepaste organisatie-eenheden worden gesynchroniseerd terug naar uw Azure AD-tenant. Deze objecten zijn beschikbaar voor gebruik alleen binnen uw beheerde domein. Deze objecten zijn er daarom niet zichtbaar Azure AD PowerShell-cmdlets, Azure AD Graph API of het beheer van Azure AD UI gebruiken.


## <a name="related-content"></a>Verwante onderwerpen
- [Functies - Azure AD-domein Services](active-directory-ds-features.md)

- [Scenario's voor implementatie - Azure AD-domeinservices](active-directory-ds-scenarios.md)

- [Aandachtspunten voor de netwerken voor Azure AD-domeinservices](active-directory-ds-networking.md)

- [Aan de slag met Azure AD-domeinservices](active-directory-ds-getting-started.md)
