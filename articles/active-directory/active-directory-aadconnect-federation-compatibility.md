<properties
    pageTitle="Azure AD Federatie compatibiliteit lijst"
    description="Deze pagina bevat niet-Microsoft-identiteitsprovider die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure AD Federatie compatibiliteit lijst
Azure Active Directory biedt eenmalige aanmelding op en beveiliging in access toepassing voor Office 365 en andere Microsoft Online services voor hybride en alleen de cloud implementaties enhanced zonder een niet-Microsoft-oplossing. Office 365, zoals het grootste deel van de Microsoft Online services is geïntegreerd met Azure Active Directory voor adreslijstservices en verificatiegegevens. Azure Active Directory biedt ook eenmalige aanmelding tot duizenden SaaS-toepassingen en on-premises implementatie van webtoepassingen. Raadpleeg de galerie met Azure Active Directory-toepassing voor ondersteunde SaaS-toepassingen.

Organisaties die hebben geïnvesteerd in niet-Microsoft federation oplossingen, bevat dit onderwerp richtlijnen voor het configureren van eenmalige aanmelding voor hun Windows Server Active Directory-gebruikers met Microsoft Online services met behulp van niet-Microsoft-identiteitsprovider van de 'Azure Active Directory federation compatibiliteit onderstaande". 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Groep Oxford-Computer](http://oxfordcomputergroup.com/), een derde partij, namens Microsoft, getest deze ervaringen voor eenmalige aanmelding met niet-Microsoft-identiteitsprovider ten opzichte van een reeks gebruik gevallen algemene met Azure Active Directory.

Voor informatie over hoe u uw identiteitsprovider van derden Hier wordt vermeld gaat, neem contact op met Oxford Computer Group op [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] De groep Computer Oxford getest alleen de functionaliteit voor Federatie van deze eenmalige aanmelding scenario's. Groep van de Computer Oxford heeft geen uitgevoerd voor elke testen van de synchronisatie, tweeledige verificatie, enzovoort onderdelen van deze eenmalige aanmelding scenario's.

>Gebruik van aanmeldingsproblemen door alternatieve ID naar UPN is ook niet getest in dit programma.



- [Azure Active Directory](#azure-active-directory)
- [Optimale IDM virtuele identiteit Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli federatieve identiteitsbeheer 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401) 
- [BIG-IP met Access beleid Manager BIG-IP ver., 11, lid 3 x – 11.6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware werkruimte Portal versie 2.1](#vmware-workspace-portal-version-21) 
- [Meld u aan en gaat u 5.3](#signampgo-53) 
- [IceWall Federatie versie 3.0](#icewall-federation-version-30) 
- [CA Secure Cloud](#ca-secure-cloud) 
- [Dell één identiteitsbeheer Cloud Access v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil één aanmeldingsgegevens 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Aangezien die producten van derden, biedt Microsoft geen ondersteuning voor de implementatie, configuratie, probleemoplossing, aanbevolen procedures, enzovoort problemen en vragen met betrekking tot deze identiteitsprovider. Voor ondersteuning en vragen met betrekking tot deze identiteitsprovider, rechtstreeks contact op met de ondersteunde derden.

>Deze identiteitsprovider van derden zijn getest op compatibiliteit met Microsoft cloudservices WS-Federation en WS-Trust protocollen alleen gebruiken. Testen bevat geen gebruik van de SAML-protocol.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory kunt gebruikers verifiëren door het samenbrengen met Active Directory in uw on-premises implementatie of zonder een federatieserver on-premises implementatie met behulp van Wachtwoordsynchronisatie. 

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor aanmelding. 


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|
|Moderne toepassingen met ADAL zoals Office 2016| Ondersteund|Geen|

Zie voor meer informatie over het gebruik van Azure Active Directory met AD FS [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Zie voor meer informatie over het gebruik van Azure Active Directory met Wachtwoordsynchronisatie [Azure AD Connect](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimale IDM virtuele identiteit Server Federation Services 
Optimale IDM virtuele identiteit Server Federation Services kunnen gebruikers die zich in actieve adreslijsten van klanten on-premises implementatie bevinden verifiëren.

Hieronder ziet u het scenario ondersteunen matrix deze functionaliteit voor eenmalige aanmelding.


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Voor meer informatie over clienttoegang beleid Zie [beperken van toegang tot Office 365-Services is gebaseerd op de locatie van de Client.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 implementeert de gebruikte WS Federation identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen (eerdere versies moeten een upgrade uitvoeren naar 6.11|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Voor de PingFederate-instructies voor het configureren van dit STS op te geven van de functionaliteit voor eenmalige aanmelding voor de Active Directory-gebruikers, het PDF-bestand downloaden [hier.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7.2 
PingFederate 7.2 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor de PingFederate instructies voor het configureren van deze STS op te geven van de functionaliteit voor eenmalige aanmelding aan uw Active Directory-gebruikers, [hier.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor de PingFederate instructies voor het configureren van deze STS op te geven van de functionaliteit voor eenmalige aanmelding aan uw Active Directory-gebruikers, [hier.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify helpt een federatieve eenmalige aanmelding ervaring beschikken voor Office 365 zonder de vereiste van een on-premises implementatie federatieserver hostingprovider.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Client-toegangsbeheer wordt niet ondersteund. 

Zie voor meer informatie over Centrify, [hier.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli federatieve identiteitsbeheer 6.2.2 
IBM Tivoli federatieve identiteitsbeheer 6.2.2 met IBM beveiliging Access Manager voor Microsoft-toepassingen 1.4 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over IBM Tivoli federatieve identiteitsbeheer [IBM beveiliging Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een functionaliteit voor eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over SecureAuth, [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 cumulatieve Release 4
CA SiteMinder Federatie 12.52 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over CA SiteMinder, [CA SiteMinder Federatie.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne Cloud Federatie Service (CFS) 3.0 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over RadiantOne CFS, [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 


| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie is vereist installatie van extra webserver en Okta-toepassing.|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over Okta, [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin zoals getest in mei 2014 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over OneLogin, [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |* Kerberos contracten worden ondersteund|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

* NetIQ Kerberos-verificatie via de configuratie van een Kerberos Contract ondersteunen.  Neem contact op met NetIQ of bekijken van de installatiehandleiding voor hulp bij deze configuratie kunt. Zie voor meer informatie over NetIQ Access Manager [NetIQ Access Manager.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP met Access beleid Manager BIG-IP ver. 11, lid 3 x – 11.6 x 
De BIG-IP met Access beleid Manager, (APM) ver. BIG-IP 11,3 x – 11.6 x implementeert de gebruikte SAML identiteit standaard om een functionaliteit voor eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding. 

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Niet ondersteund |Niet ondersteund|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over BIG-IP Access beleid Manager [BIG-IP Access beleid Manager.](https://f5.com/products/modules/access-policy-manager) 

Voor de BIG-IP Access beleid Manager-instructies voor het configureren van dit STS op te geven van de functionaliteit voor eenmalige aanmelding aan uw Active Directory-gebruikers, het PDF-bestand downloaden [hier.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>VMware werkruimte Portal versie 2.1 
VMware werkruimte Portal versie 2.1 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Voor meer informatie over VMware werkruimte Portal versie 2.1, downloadt u het PDF-bestand [hier.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Meld u aan en gaat u 5.3 
Meld u aan en ga 5.3 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard een eenmalige aanmelding en kenmerk exchange framework te geven.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Kerberos-contracten worden ondersteund |
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|


Aanmelden en ga 5.3 ondersteunt Kerberos-verificatie via de configuratie van een Kerberos Contract.  Voor hulp bij deze configuratie, neemt u contact opnemen met Ilex of bekijken van de installatiehandleiding voor [hier.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>IceWall Federatie versie 3.0 
IceWall Federatie versie 3.0 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over IceWall Federatie, [hier](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) en [hier.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA Secure Cloud 

CA Secure Cloud implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over CA Secure Cloud, [CA Secure Cloud.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell één identiteitsbeheer Cloud Access v7.1 
Dell één identiteit Cloud Access Manager implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geen|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM |  Ondersteund |Geen|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|

Zie voor meer informatie over Dell één Cloud Access identiteitsbeheer [Dell één Cloud Access identiteitsbeheer.](http://software.dell.com/products/cloud-access-manager)

 Zie de instructies voor het configureren van deze STS op te geven van de functionaliteit voor eenmalige aanmelding aan uw Office 365-gebruikers [Office 365-gebruikers configureren.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil één aanmeldingsgegevens 4.5 
AuthAnvil eenmalige aanmelding op 4.5 implementeert de gebruikte WS Federatie/WS-Trust identiteit standaard om een eenmalige aanmelding en kenmerk exchange framework.

Hieronder ziet u de matrix scenario ondersteuning voor deze functionaliteit voor eenmalige aanmelding.

| Client |Ondersteuning  |Uitzonderingen|
| --------- | --------- |--------- |
| Webclients zoals Exchange Web Access en SharePoint Online | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| RTF-clienttoepassingen zoals Lync, Office-abonnement, CRM | Ondersteund |Geïntegreerde Windows-verificatie wordt niet ondersteund.|
| E-RTF-mailclients zoals Outlook en ActiveSync |  Ondersteund |Geen|


Zie voor meer informatie [AuthAnvil Single Sign On.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
