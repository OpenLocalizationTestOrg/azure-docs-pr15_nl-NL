<properties
   pageTitle="Azure Security Management en het monitoren van overzicht | Microsoft Azure"
   description=" Azure biedt beveiliging methoden als hulpmiddel bij het beheren en controleren van Azure cloudservices en virtuele machines.  Dit artikel bevat een overzicht van deze core beveiligingsfuncties en -services. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Azure Security Management en het monitoren van overzicht

Azure biedt beveiliging methoden als hulpmiddel bij het beheren en controleren van Azure cloudservices en virtuele machines. Dit artikel bevat een overzicht van deze core beveiligingsfuncties en -services. Koppelingen worden gegeven naar artikelen met details van elk geeft zodat u kunt meer informatie.

De beveiliging van uw Microsoft-cloudservices is een verbinding en gedeelde verantwoordelijkheid tussen u en Microsoft. Gedeelde verantwoordelijkheid betekent dat Microsoft is verantwoordelijk voor de Microsoft Azure en fysieke beveiliging van de gegevens worden gecentreerd (met behulp van de beveiligingsinstellingen zoals vergrendelde badge vermelding deuren, fences en schermen). Daarnaast biedt Azure sterke niveaus van cloud beveiliging tegen de laag software die voldoet aan de behoeften voor beveiliging, privacy en naleving van de veeleisende klanten.

U eigenaar bent van uw gegevens en identiteiten, verantwoordelijk voor het beschermen van deze, de beveiliging van uw lokale resources en de beveiliging van onderdelen van cloud waarover u controle hebt. Microsoft biedt u met de besturingselementen voor beveiliging en mogelijkheden om te helpen u uw gegevens en toepassingen beveiligen. Uw verantwoordelijkheid voor beveiliging is gebaseerd op het type cloudservice.

De volgende tabel bevat een overzicht van het saldo van verantwoordelijk voor zowel Microsoft als de klant.

![Gedeelde verantwoordelijkheid][1]

Zie voor een grondigere Kennismaking in het beveiligingsbeheer van de, [beveiliging management in Azure wordt aangegeven](azure-security-management.md).

Hier volgen de core-functies om te worden beschreven in dit artikel:

- Toegangsbeheer op basis van rollen
- Antimalware
- Meervoudige verificatie
- ExpressRoute
- Virtuele netwerkgateways
- Bevoegdheden identiteitsbeheer
- Identiteitsbeveiliging
- Beveiligingscentrum

## <a name="role-based-access-control"></a>Toegangsbeheer op basis van rollen

Rolgebaseerd Access besturingselement (RBAC) biedt fijnmazige toegangsbeheer voor Azure resources. RBAC gebruikt, kunt u verlenen personen alleen het bedrag van access die ze nodig hebben om uit te voeren hun taken.  RBAC kunt u ervoor zorgen dat als mensen de organisatie verlaten ze geen toegang meer tot bronnen in de cloud.

Meer informatie:

- [Active Directory-teamblog op RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

Met Azure kunt u de software antimalware van primaire beveiliging leveranciers zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky u kunt uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere threats.

Microsoft Antimalware biedt u de mogelijkheid een agent antimalware voor zowel PaaS rollen en virtuele machines installeren. Op basis van systeem Center Endpoint Protection, kunt deze functie u beproefde on-premises implementatie beveiligingsbeheerder in de cloud.

We bieden bovendien de naadloze integratie voor de Trend [Uitgebreide beveiliging](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ en [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™-producten in de Azure platform. DeepSecurity is een antivirussoftware oplossing en SecureCloud is een oplossing versleuteling. DeepSecurity wordt geïmplementeerd binnen VMs met een extensiemodel. De UI-portal en PowerShell gebruikt, kunt u DeepSecurity binnen nieuwe VMs die zijn wordt of omhoog of bestaande VMs die al zijn geïmplementeerd gebruiken.

Symantec eindpunt beveiliging (SEP) wordt ook ondersteund op Azure. Via de integratie van de portal hebben klanten de mogelijkheid om op te geven dat ze wilt gebruiken SEP binnen een VM. SEP kan worden geïnstalleerd op een gloednieuwe VM via de Portal Azure of kan worden geïnstalleerd op een bestaande VM via PowerShell.

Meer informatie:

- [Implementatie van oplossingen Antimalware op Azure virtuele Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft Antimalware voor Azure Cloudservices en virtuele Machines](../security/azure-security-antimalware.md)
- [Het installeren en configureren van Trend Micro uitgebreide Security als een Service op een Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Het installeren en configureren van Symantec Endpoint Protection op een Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nieuwe Antimalware opties voor het beschermen van Azure virtuele Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Meervoudige verificatie

Azure meervoudige verificatie (MFA) is een methode verificatie dat nodig is van het gebruik van meer dan één verificatiemethode en een kritieke tweede laag van beveiliging toegevoegd aan de gebruiker aanmeldingen en transacties. MFA helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden. Levert sterke verificatie via een bereik van opties voor verificatie, telefoongesprek, SMS-bericht of mobiele app melding of verificatie-code en 3e partijen EED tokens.

Meer informatie:

- [Meervoudige verificatie](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Wat Azure meervoudige verificatie is?](../multi-factor-authentication/multi-factor-authentication.md)
- [De werking van Azure meervoudige verificatie](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute kunt u uw on-premises implementatie-netwerken uitbreiden naar het Microsoft cloud via een speciale privé verbinding vergemakkelijkt door een provider connectivity. U kunt verbindingen met Microsoft cloudservices, zoals Microsoft Azure, Office 365 en CRM Online tot stand te brengen met ExpressRoute. Connectiviteit kan afkomstig zijn uit een de toepassing te (VPN IP)-netwerk, een Ethernet-netwerk met een punt of een virtuele cross-verbinding via een provider connectivity in de inrichting van een collega locatie. ExpressRoute verbindingen gaan niet via de openbare Internet. Hierdoor ExpressRoute verbindingen om te bieden meer betrouwbaarheid, snellere snelheden lagere vertragingstijden en hoger beveiliging dan de normale verbindingen via Internet.

Meer informatie:

- [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuele netwerkgateways

VPN Gateways, ook wel genoemd Azure virtuele netwerkgateways, worden gebruikt om netwerkverkeer tussen virtuele netwerken en on-premises implementatie locaties te verzenden. Ze ook gebruikt voor het verzenden van verkeer tussen meerdere virtuele netwerken binnen Azure (VNet VNet).  VPN gateways verzorgen secure cross-premises tussen Azure en de infrastructuur van uw.

Meer informatie:

- [Over VPN gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Overzicht van de Azure netwerk-beveiliging](security-network-overview.md)

## <a name="privileged-identity-management"></a>Bevoegdheden identiteitsbeheer

Soms moeten gebruikers verrichten bevoegdheden in Azure resources of andere SaaS-toepassingen. Dit betekent vaak organisaties hebben deze permanente bevoegdheden om toegang te geven in Azure Active Directory (Azure AD). Dit is een groeiende beveiligingsrisico voor resources cloud gehoste omdat organisaties voldoende niet bewaken wat die gebruikers mee bezig zijn met hun bevoegdheden toegang.
Ook als een gebruikersaccount met bevoegdheden toegang is is gehackt, die één niet naleven kan van invloed zijn op de algehele cloud-beveiliging. Azure AD rechten identiteitsbeheer helpt dit risico oplossen door de tijd weergeven van bevoegdheden verlagen en zicht op gebruik met groter wordende.  

Bevoegdheden identiteitsbeheer maakt u kennis met het concept van een tijdelijke beheerder voor een rol of 'alleen in tijd"beheerderstoegang, dat wil een gebruiker die moet zeggen worden voltooid van een activeringsproces voor die rol toegewezen. Het activeringsproces voor verandert de toewijzing van de gebruiker aan een rol in Azure AD uit inactief in actief, gedurende een bepaalde tijd period zoals 8 uur.

Meer informatie:

- [Azure AD-rechten identiteitsbeheer](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Aan de slag met Azure AD bevoegdheden identiteitsbeheer](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging

Azure Active Directory (AD) identiteitsbeveiliging biedt een gecombineerde weergave van verdachte aanmeldingsproblemen activiteiten en mogelijke problemen beveiligen van uw bedrijf. Identiteitsbeveiliging worden gedetecteerd verdachte activiteiten voor gebruikers en rechten (admin) identiteiten, op basis van signalen zoals gewelddadige aanvallen, meer referenties en aanmeldingen van onbekende locaties en apparaten geïnfecteerd.

Door meldingen en aanbevolen remediation, helpt identiteit bescherming tegen risico's in realtime te beperken. Deze gebruiker risico ernst berekent en u kunt op basis van het risico beleidsregels om automatisch te beschermen-toepassing openen vanuit toekomstige threats configureren.

Meer informatie:

- [Beveiliging van Azure Active Directory-identiteit](../active-directory/active-directory-identityprotection.md)
- [Kanaal 9: Azure AD en identiteit weergeven: identiteit beveiliging Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Beveiligingscentrum

Azure Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats en vindt dat u meer inzicht in, en controle over, de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

Beveiligingscentrum kunt optimaliseren en controleren van de beveiliging van uw Azure resources door:

- Zodat u kunt het definiëren van beleidsregels voor uw abonnement op Azure-resources op basis van de behoeften van de beveiliging van uw bedrijf en het type van toepassingen of gevoeligheid van de gegevens in elk abonnement.
- De status van uw Azure virtuele machines controle, netwerken en toepassingen.
- Een lijst met prioriteit beveiligingsmeldingen, met inbegrip van meldingen van geïntegreerde partneroplossingen, samen met de gegevens die u nodig hebt om te snel kunnen onderzoeken en aanbevelingen over het herstellen van een aanval leveren.

Meer informatie:

- [Inleiding tot Azure Beveiligingscentrum](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
