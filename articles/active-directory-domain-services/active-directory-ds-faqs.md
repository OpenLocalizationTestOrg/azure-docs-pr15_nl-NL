<properties
    pageTitle="Veelgestelde vragen - Azure Active Directory Domain Services | Microsoft Azure"
    description="Veelgestelde vragen over Azure Active Directory Domain Services"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Veelgestelde vragen

Deze pagina vindt u antwoorden op veelgestelde vragen over de Azure Active Directory Domain Services. Gecontroleerd regelmatig op updates.

### <a name="troubleshooting-guide"></a>De gids voor probleemoplossing
Zie onze [gids voor probleemoplossing](active-directory-ds-troubleshooting.md) voor oplossingen voor veelvoorkomende problemen bij het configureren en beheren van Azure AD Domain Services.


### <a name="configuration"></a>Configuratie

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Kan ik meerdere domeinen voor één maken Azure AD-map?
Nee. U kunt slechts één domein verwerkt door Azure AD-domeinservices voor één maken Azure AD-map.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kan ik Azure AD-domeinservices in een virtueel netwerk Azure resourcemanager inschakelen?
Nee. Azure AD-domeinservices kunnen alleen worden ingeschakeld in een klassieke Azure virtual network. U kunt het klassieke virtuele netwerk verbinding maken met een resourcemanager virtuele netwerk via virtueel netwerk peering om uw beheerde domein in een virtueel resourcemanager-netwerk te gebruiken.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kan ik ervoor Azure AD-domeinservices beschikbaar in meerdere virtuele netwerken binnen mijn abonnement?
De service zelf ondersteunt dit scenario niet direct. Azure AD-domeinservices is beschikbaar in slechts één virtueel netwerk tegelijk. U kunt echter de connectiviteit tussen meerdere virtuele netwerken om weer te geven van Azure AD-domeinservices met andere virtuele netwerken configureren. In dit artikel wordt beschreven hoe u kunt [verbinding maken met virtuele netwerken in Azure wordt aangegeven](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kan ik Azure AD-domeinservices via PowerShell inschakelen?
PowerShell/geautomatiseerde implementatie van Azure AD-domeinservices is momenteel niet beschikbaar.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Azure AD-domeinservices beschikbaar is in de nieuwe portal van Azure?
Nee. Azure AD-domeinservices kunnen worden geconfigureerd alleen in de [klassieke Azure-portal](https://manage.windowsazure.com). We verwachten om uit te breiden ondersteuning voor de [Azure-portal](https://portal.azure.com) in de toekomst.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kan ik domeincontrollers toevoegen aan een beheerde domeinservices van Azure AD-domein?
Nee. Het domein dat is verstrekt door Azure AD-domeinservices is een beheerde domein. U hoeft niet te inrichten, configureren of anderszins domeincontrollers voor dit domein - beheren deze activiteiten management dienen als een service door Microsoft. U kunt geen daarom controller (alleen-lezen-schrijven of alleen-lezen) voor het domein dat beheerde toevoegen.

### <a name="administration-and-operations"></a>Beheer en bedrijfsactiviteiten

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kan ik verbinding maken met de domeincontroller voor mijn beheerde domein met extern bureaublad?
Nee. U bent niet gemachtigd om te verbinden met domeincontrollers voor de beheerde domein via Extern bureaublad. Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen het beheerde domein met AD-beheerprogramma's zoals Active Directory beheer Center (ADAC) of AD PowerShell beheren. Deze hulpprogramma's zijn geïnstalleerd met van de functie 'Externe Server beheerprogramma's ' op een Windows-server toegevoegd aan het beheerde domein.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ik hebt Azure AD-domeinservices ingeschakeld. Welke gebruikersaccount moet ik gebruiken voor domein join-computers aan dit domein?
Gebruikers die u hebt toegevoegd aan de beheergroep (bijvoorbeeld ' AAD domeincontroller beheerders') kunnen domein-join machines. Daarnaast kunnen gebruikers in deze groep toegang extern bureaublad op computers die op het domein zijn samengevoegd.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Kan ik een domein beheerdersbevoegdheden voor het domein dat is verstrekt door Azure AD-domeinservices beleidsitems?
Nee. U geen beheerdersbevoegdheden voor het domein dat beheerde daarvoor. Bevoegdheden voor zowel ' domein beheerder ' en ' ondernemingsadministrator ' zijn niet beschikbaar voor gebruik in het domein. De beheerder van een bestaand domein of enterprise beheerder groepen binnen de adreslijst van uw Azure AD worden ook niet verleend domein/administratorbevoegdheden in het domein.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Kan ik groepslidmaatschap LDAP of andere AD beheerprogramma's gebruikt op domeinen verstrekt door Azure AD-domeinservices wijzigen?
Nee. Groepslidmaatschap kunnen niet worden gewijzigd op domeinen die worden verwerkt door Azure AD Domain Services. Dit geldt ook voor gebruikerskenmerken. U kunt echter groepslidmaatschap of gebruikerskenmerken wijzigen in Azure AD of op uw on-premises domein. Deze wijzigingen worden automatisch gesynchroniseerd met Azure AD Domain Services.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Kan ik het schema van het domein dat is verstrekt door Azure AD-domeinservices uitbreiden?
Nee. Het schema wordt beheerd door Microsoft voor het domein dat beheerde. Schema-uitbreidingen worden niet ondersteund door Azure AD Domain Services.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kan ik wijzigen of de DNS-records toevoegen aan mijn beheerde domein?
Ja. Gebruikers die deel uitmaakt van de ' AAD domeincontroller ' beheerdersgroep worden bevoegdheden voor ' DNS-beheerder ', als u wilt wijzigen van de DNS-records in de beheerde domein toegewezen. Deze gebruikers kunnen de DNS-beheer-console gebruiken op een computer met Windows Server toegevoegd aan het beheerde domein, dat DNS wordt beheerd. U gebruikt de DNS-beheer door ' DNS-Server hulpprogramma's installeert ', die deel van de 'Externe Server beheerprogramma's ' optionele functie op de server uitmaakt. Meer informatie over [functies voor beheer, controleren en problemen met DNS oplossen](https://technet.microsoft.com/library/cc753579.aspx) is beschikbaar op TechNet.


### <a name="billing-and-availability"></a>Facturering en beschikbaarheid

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Is dat Azure AD Domain Services een betaalde service?
Ja. Zie de [prijzen van de pagina](https://azure.microsoft.com/pricing/details/active-directory-ds/)voor meer informatie.

#### <a name="is-there-a-free-trial-for-the-service"></a>Is er een gratis proefversie voor de service?
Deze service is opgenomen in de gratis proefversie voor Azure. U kunt zich aanmelden voor een [gratis proefversie voor één maand van Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Kan ik Azure AD-domeinservices krijgen als onderdeel van Enterprise mobiliteit Suite (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Heb ik nodig Azure AD Premium Azure AD-domeinservices gebruiken?
Nee. Azure AD-domeinservices een pay-as-you-go Azure-service is en geen deel uitmaakt van EMS. Azure AD-domeinservices zijn beschikbaar voor alle edities van Azure AD (gratis, Basic en, Premium) en worden gefactureerd per uur worden berekend, afhankelijk van gebruik.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Welke Azure regio's is de service beschikbaar in?
Verwijzen naar de pagina [Azure Services per regio](https://azure.microsoft.com/regions/#services/) voor een overzicht van de Azure regio's waar Azure AD-domeinservices beschikbaar is.
