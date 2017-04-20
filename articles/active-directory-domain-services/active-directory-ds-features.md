<properties
    pageTitle="Azure Active Directory Domain Services: Functies | Microsoft Azure"
    description="Functies van Azure Active Directory Domain Services"
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
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD-domeinservices

## <a name="features"></a>Functies
De volgende functies zijn beschikbaar in Azure AD-domeinservices beheerde domeinen.

- **Eenvoudige implementatie-ervaring:** Azure AD Domain Services voor uw Azure AD-tenant met een paar muisklikken, kunt u ook weer inschakelen. Ongeacht of uw Azure AD-tenant is een cloud-tenant of gesynchroniseerd met uw on-premises adreslijst, kunt u snel uw beheerde domein ingericht.

- **Ondersteuning voor domein-join:** U kunt eenvoudig domein-join-computers in het Azure virtuele netwerk dat van uw domein beheerde is beschikbaar in. De domein-join-ervaring op Windows-client en Server besturingssystemen werkt naadloos tegen domeinen die worden verwerkt door Azure AD Domain Services. U kunt ook geautomatiseerde domein join gereedschap ten opzichte van deze domeinen gebruiken.

- **Exemplaar van een domein per Azure AD-map:** U kunt één Active Directory-domein voor elke Azure AD-map maken.

- **Maken van domeinen met aangepaste namen:** U kunt domeinen met aangepaste namen (bijvoorbeeld ' contoso100.com') met Azure AD-domeinservices maken. U kunt een van beide geverifieerd of niet-geverifieerde domeinnamen. (Optioneel) u ook een domein kunt maken met het achtervoegsel ingebouwde domein (dat wil zeggen ' *. onmicrosoft.com') door het telefoonboek van uw Azure AD worden aangeboden.

- **Geïntegreerd met Azure AD:** U hoeft niet te configureren of replicatie Azure AD Domain Services beheren. Gebruikersaccounts, groepslidmaatschap en gebruikersreferenties (wachtwoorden) uit de adreslijst van uw Azure AD zijn automatisch beschikbaar van Azure AD Domain Services. Nieuwe gebruikers, groepen of wijzigingen in de kenmerken van de Azure AD-tenant of uw on-premises adreslijst worden automatisch gesynchroniseerd met Azure AD Domain Services.

- **NTLM als Kerberos-verificatie:** Met ondersteuning voor NTLM als Kerberos-verificatie, kunt u de toepassingen die afhankelijk van Windows-verificatie zijn implementeren.

- **Gebruiken van uw bedrijf referenties/wachtwoorden:** Wachtwoorden opnieuw instellen voor gebruikers in uw Azure AD-tenant werken met Azure AD Domain Services. Gebruikers kunnen hun bedrijfsreferenties voor domein-join-computers gebruiken, aanmelden interactief of via Extern bureaublad en verifiëren met het domein dat beheerde.

- **LDAP-binding & LDAP lezen ondersteuning:** U kunt de toepassingen die afhankelijk zijn van de LDAP-bindingen om gebruikers in domeinen die worden verwerkt door Azure AD Domain Services te verifiëren. Bovendien kunnen ook toepassingen die kenmerken van de gebruiker/computer opvragen gelezen LDAP-bewerkingen uit de adreslijst gebruiken werken ten opzichte van Azure AD Domain Services.

- **Veilige LDAP (leesbaar TEKSTFORMAAT):** U kunt toegang tot de adreslijst inschakelen via veilige LDAP (LDAPS). Beveiligde LDAP toegang is beschikbaar in het virtuele netwerk al dan niet standaard. U kunt echter ook desgewenst secure LDAP-toegang inschakelen via internet.

- **Groepsbeleid:** U kunt een enkel ingebouwde GPO elke gebruiken voor de gebruikers en computers containers om af te dwingen naleving beveiligingsbeleid voor apparaten vereist voor gebruikersaccounts en computers domein behoren.

- **DNS wordt beheerd:** Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen DNS beheren voor uw beheerde domein met de vertrouwde DNS-beheerprogramma's zoals de DNS-beheer van de module.

- **Maken (aangepaste organisatie eenheden):** Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen maken van aangepaste organisatie-eenheden in de beheerde domein. Deze gebruikers krijgen volledige beheerdersrechten via aangepaste organisatie-eenheden, zodat u ze kunnen toevoegen/verwijderen serviceaccounts, computers, groepen enzovoort binnen deze aangepaste organisatie-eenheden.

- **Beschikbaar in meerdere Azure regio's:** Ga naar de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) informatie van de Azure regio's waarin Azure AD-domeinservices beschikbaar is.

- **Beschikbaarheid:** Azure AD-domeinservices biedt beschikbaarheid voor uw domein. Deze functie biedt de garantie van hoger actieve tijdsduur van service en flexibiliteit op fouten. Ingebouwde servicestatus bewaken aanbiedingen automatische remediation van fouten door te draaien nieuwe exemplaren voor het vervangen van mislukte exemplaren en op te geven doorlopende service voor uw domein.

- **Vertrouwde beheerprogramma's gebruiken:** U kunt vertrouwde Windows Server Active Directory beheerprogramma's zoals het Beheerderscentrum voor Active Directory of Active Directory-PowerShell beheerde domeinen beheren.
