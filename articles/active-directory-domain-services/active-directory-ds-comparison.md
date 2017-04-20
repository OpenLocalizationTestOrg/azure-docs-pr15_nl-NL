<properties
    pageTitle="Azure AD-domeinservices: Azure AD-domein vergelijken Services zelf domein controller | Microsoft Azure"
    description="Vergelijking van Azure Active Directory Domain Services zelf domein controller"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Bepalen als Azure AD Domain Services is geschikt voor uw gebruiksvoorbeeld
Azure AD-domeinservices kunt u de werkbelasting in Azure-infrastructuurservices, zonder te hoeven maken over het beheren van de infrastructuur van uw identiteit implementeren. Deze beheerde service verschilt van een typisch Windows Server Active Directory-implementatie die u implementeren en beheren in uw eigen. De service is bedoeld voor toegankelijkheid-van-implementatie, geautomatiseerde statuscontrole en remediation en een eenvoudige identiteit infrastructuur voor de cloud. We zijn steeds in de service om toe te voegen ondersteuning voor algemene situaties ontwikkeling.

Te bepalen of gebruikmaken van Azure AD-domeinservices of kringvelden omhoog en beheren van uw eigen (doe) in Azure AD-infrastructuur:

- Bekijk de lijst met [functies door Azure AD-domeinservices worden aangeboden](active-directory-ds-features.md).

- Bekijk veelvoorkomende [scenario's voor implementatie voor Azure AD Domain Services](active-directory-ds-scenarios.md).

- Tot slot [Azure AD-domeinservices naar een doe AD-optie vergelijken](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure AD-domeinservices naar zelf AD-domein in Azure vergelijken
De volgende tabel kunt u bepalen tussen Azure AD-domeinservices gebruiken en beheren van uw eigen AD-infrastructuur in Azure wordt aangegeven.

|**Functie**|**Azure AD-domeinservices**|**'Doe' AD in Azure VMs**|
|---|:---:|:---:|
|[**Beheerde service**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x en met 2715;**|
|[**Beveiligde implementaties**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|De beheerder moet de implementatie secure.|
|[**DNS-server**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (beheerde service)|**& #x 2713;**|
|[**Domein of Enterprise beheerdersbevoegdheden**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x en met 2715;**|**& #x 2713;**|
|[**Aan domein toevoegen**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Domeinverificatie NTLM en Kerberos gebruiken**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Aangepaste OU-structuur**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Schema-uitbreidingen**](active-directory-ds-comparison.md#schema-extensions)|**& #x en met 2715;**|**& #x 2713;**|
|[**AD-domein vertrouwt**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x en met 2715;**|**& #x 2713;**|
|[**LDAP lezen**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Veilige LDAP (leesbaar TEKSTFORMAAT)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**LDAP schrijven**](active-directory-ds-comparison.md#ldap-write)|**& #x en met 2715;**|**& #x 2713;**|
|[**Groepsbeleid**](active-directory-ds-comparison.md#group-policy)|Eenvoudige|Volledige|
|[**Geografische distributed implementaties**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x en met 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Beheerde service
Azure AD-domeinservices domeinen worden beheerd door Microsoft. U hoeft niet te bang patches, updates, controle, back-ups, en ervoor zorgen dat de beschikbaarheid van uw domein. Deze beheertaken kunnen uitvoeren als een service met Microsoft Azure voor uw beheerde domeinen.

#### <a name="secure-deployments"></a>Beveiligde implementaties
Het beheerde domein is veilig vergrendeld aan de hand van Microsoft beveiliging aanbevolen procedures voor AD-implementaties. Deze aanbevolen procedures voortvloeien uit het AD-productteam tientallen jaren te horen van ervaring engineering en ondersteunende AD-implementaties. Voor doe implementaties moet u specifieke implementatiestappen uitvoeren om te vergrendelen omlaag/secure uw implementatie.

#### <a name="dns-server"></a>DNS-server
Een beheerde domein van Azure AD Domain Services omvat beheerde DNS-services. Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen DNS in het beheerde domein wordt beheerd. Leden van deze groep krijgen volledige DNS-beheer bevoegdheden voor het domein dat beheerde. DNS-beheer kan worden uitgevoerd met de 'beheer van DNS-console' opgenomen in de externe Server Administration Tools (RSAT)-pakket.

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domein of Ondernemingsadministrator bevoegdheden
Deze verhoogde bevoegdheden zijn niet beschikbaar op een beheerde AAD-DS-domein. Toepassingen die deze bevoegdheden moet zijn geïnstalleerd/uitvoeren nodig kunnen niet worden uitgevoerd ten opzichte van beheerde domeinen. Een subset van beheerdersbevoegdheden is beschikbaar voor leden van de gedelegeerd beheergroep 'AAD domeincontroller beheerders' genoemd. Deze bevoegdheden opnemen bevoegdheden DNS configureren, Groepsbeleid configureren, krijgen beheerdersbevoegdheden op domein behoren computers, enzovoort.

#### <a name="domain-join"></a>Aan domein toevoegen
U kunt virtuele machines deelnemen aan de beheerde domein dat lijkt op hoe u computers aan een AD-domein toevoegen.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Domeinverificatie NTLM en Kerberos gebruiken
Met Azure AD-domeinservices, kunt u uw bedrijfsreferenties gebruiken om te verifiëren met de beheerde domein. Referenties worden gesynchroniseerd met uw Azure AD-tenant. Voor gesynchroniseerde tenants, Azure AD Connect zorgt ervoor dat wijzigingen in de referenties die zijn aangebracht on-premises implementatie zijn gesynchroniseerd met Azure AD. Met een zelf domein-instelling moet u mogelijk een vertrouwensrelatie domein met een on-premises implementatie account bos voor gebruikers om te verifiëren met hun bedrijfsreferenties instellen. Mogelijk moet u ook AD replicatie instellen om ervoor te zorgen dat gebruikerswachtwoorden naar uw Azure domeincontroller virtuele machines synchroniseren.

#### <a name="custom-ou-structure"></a>Aangepaste OU-structuur
Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen aangepaste-eenheden binnen het beheerde domein maken.

#### <a name="schema-extensions"></a>Schema-uitbreidingen
U kunt het grondtal schema van een beheerde domeinservices van Azure AD-domein niet uitbreiden. Toepassingen die afhankelijk van uitbreidingen AD-schema (bijvoorbeeld nieuwe kenmerken in het gebruikersobject zijn) kunnen niet worden opgeheven en daarom verschoven naar AAD-DS-domeinen.

#### <a name="ad-domain-or-forest-trusts"></a>AD-domein of bos vertrouwensrelaties
Beheerde domeinen worden niet geconfigureerd voor het instellen van vertrouwensrelaties (inkomende/uitgaande poort) met andere domeinen. Scenario's zoals resource bos implementaties of zaken waar u liever geen wachtwoorden naar Azure AD synchroniseren niet daarom Azure AD Domain Services gebruiken.

#### <a name="ldap-read"></a>LDAP-gelezen
Het beheerde domein ondersteunt LDAP werkbelasting lezen. Daarom kunt u toepassingen die LDAP gelezen ten opzichte van het domein dat beheerde bewerkingen implementeren.

#### <a name="secure-ldap"></a>Veilige LDAP
U kunt Azure AD domein Services beveiligde LDAP toegang bieden tot uw beheerde domein, inclusief via internet configureren.

#### <a name="ldap-write"></a>LDAP schrijven
Het beheerde domein is alleen-lezen voor gebruikersobjecten. Toepassingen die LDAP schrijven ten opzichte van de kenmerken van het gebruikersobject bewerkingen werken dus niet in een beheerde domein. Daarnaast kunnen kunnen niet gebruikerswachtwoorden worden gewijzigd in het beheerde domein. Een ander voorbeeld is de wijziging van de groepslidmaatschappen of Groepkenmerken in het beheerde domein, dat niet is toegestaan. Echter een wordt gewijzigd in de gebruikerskenmerken of wachtwoorden die zijn aangebracht in Azure AD (via PowerShell/Azure portal) of on-premises AD worden gesynchroniseerd met de beheerde AAD-DS-domein.

#### <a name="group-policy"></a>Groepsbeleid
Geavanceerde Groepsbeleid constructies worden niet ondersteund in de beheerde AAD-DS-domein. Bijvoorbeeld, kunt u maken en implementeren van afzonderlijke GPO's voor elke aangepaste OU in het domein of WMI filteren voor 1U doelgroepen gebruiken. Er is een ingebouwde GPO elke voor de containers 'AADDC Computers' en 'AADDC Users', die kan worden aangepast voor het configureren van Groepsbeleid.

#### <a name="geo-dispersed-deployments"></a>Geografische afwijken implementaties
Azure AD-domeinservices beheerde domeinen zijn beschikbaar in één virtual netwerk in Azure wordt aangegeven. Voor scenario's waarvoor domeincontrollers kan worden gecontroleerd in meerdere Azure regio's kant van de wereld zitten, stellen domeincontrollers in Azure IaaS VMs mogelijk beter alternatief.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Doe' (DIY) AD-distributieopties
U moet mogelijk implementatie gebruik-zaken waar u moet enkele van de mogelijkheden die door de installatie van een Windows Server AD worden aangeboden. In dat geval kunt u een van de volgende opties voor doe (DIY):

- **Zelfstandige cloud domein:** U kunt een zelfstandig product 'cloud domein' met Azure virtuele machines die zijn geconfigureerd als domeincontroller instellen. Deze infrastructuur is geen interactie tussen uw on-premises AD-omgeving. Deze optie moet een andere set 'cloud referenties' VMs in de cloud login/beheren.

- **Resource bos implementatie:** U kunt een domein instellen in de topologie van de bos resource met Azure virtuele machines geconfigureerd als domeincontroller. Vervolgens kunt u een AD-vertrouwensrelatie configureren met uw on-premises AD-omgeving. Domein-join computers (Azure VMs) kunt u naar deze resource bos in de cloud. Gebruikersverificatie gebeurt via een VPN-/ ExpressRoute verbinding met uw on-premises adreslijst.

- **Breid uw on-premises domein naar Azure:** Kunt u een Azure virtuele netwerk verbinden met uw on-premises netwerk via een VPN-/ ExpressRoute-verbinding zodat Azure VMs kunnen worden gekoppeld aan uw on-premises AD. Een ander alternatief is replicadomeincontrollers van uw domein on-premises implementatie in Azure wordt aangegeven als een VM bevorderen. U kunt vervolgens deze moet instellen om via een VPN-/ ExpressRoute-verbinding voor uw on-premises adreslijst repliceren. Uw on-premises domein breidt deze modus implementatie effectief naar Azure.

> [AZURE.NOTE] U kan vaststellen dat beter is voor uw implementatie gebruik-situaties een optie voor zelf geschikt is. Houd rekening met het [delen van feedback](active-directory-ds-contact-us.md) om ons begrijpen hoe functies kunt te helpen u ervoor hebt gekozen Azure AD-domeinservices in de toekomst. Deze feedback kan wij de service beter aan uw behoeften voor implementatie en gebruik-dozen ontwikkelen.

We hebben [richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx) waarmee gemakkelijker zelf installaties die zijn gepubliceerd.


## <a name="related-content"></a>Verwante onderwerpen
- [Functies - Azure AD-domein Services](active-directory-ds-features.md)

- [Scenario's voor implementatie - Azure AD-domeinservices](active-directory-ds-scenarios.md)

- [Richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
