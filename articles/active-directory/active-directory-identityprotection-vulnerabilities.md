<properties
    pageTitle="Problemen die worden gedetecteerd door Azure Active Directory identiteitsbeveiliging | Microsoft Azure"
    description="Overzicht van de problemen gedetecteerd door Azure Active Directory identiteitsbeveiliging."
    services="active-directory"
    keywords="beveiliging in de Azure active directory-identiteit, cloud-app discovery,-toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid beheren"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Problemen die worden gedetecteerd door Azure Active Directory identiteitsbeveiliging 

Problemen zijn weaknesses in uw omgeving die kan worden gebruikt door onbevoegden. Het is raadzaam dat u deze oplost om te verbeteren de houding beveiliging van uw organisatie en voorkomen dat onbevoegden misbruik van deze.
<br><br>
![problemen](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

De volgende secties vindt u een goed beeld van de problemen gemeld door identiteitsbeveiliging.

## <a name="multi-factor-authentication-registration-not-configured"></a>Meervoudige verificatie registratie niet geconfigureerd 

Hierdoor kunt u de implementatie van Azure meervoudige verificatie in uw organisatie beheren. 

Azure meervoudige verificatie biedt een tweede laag van beveiliging voor gebruikersverificatie. Deze helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden. Sterke verificatie via een bereik van eenvoudig controleopties levert, telefoongesprek, SMS-bericht of mobiele app melding of verificatie-code en 3e partijen EED tokens.

Het is raadzaam dat u Azure meervoudige verificatie voor aanmelding invoegtoepassingen voor gebruiker vereist. Meervoudige verificatie speelt een belangrijke rol in risico gebaseerde voorwaardelijke clienttoegangsbeleid beschikbaar via de identiteitsbeveiliging.

Zie voor meer informatie, [Wat Azure meervoudige verificatie is?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Onbeheerde cloud-apps

Hierdoor kunt u bepalen onbeheerde cloud-apps in uw organisatie.
 
In moderne ondernemingen zijn de IT-afdelingen vaak niet op de hoogte van alle cloud-toepassingen die gebruikers in hun organisatie worden gebruikt om te kunnen werken. Het is eenvoudig om te zien waarom beheerders zou vragen hebt over onbevoegd toegang krijgen tot de zakelijke gegevens, mogelijke gegevens lekkage en andere beveiligingsrisico's. 

Het is raadzaam dat uw organisatie implementeert Cloud-App Discovery kunnen onbeheerde cloud-toepassingen vinden, en deze met Azure Active Directory-toepassingen beheren.

Zie [onbeheerde cloud-toepassingen met de Cloud App Discovery zoeken](active-directory-cloudappdiscovery-whatis.md)voor meer informatie.



##<a name="security-alerts-from-privileged-identity-management"></a>Beveiligingsmeldingen van bevoegdheden identiteitsbeheer

Hierdoor kunt u opsporen en oplossen van waarschuwingen over bevoegdheden identiteiten in uw organisatie.  

Als u wilt dat gebruikers bevoegdheden verrichten, organisaties nodig gebruikers tijdelijk of permanent bevoegdheden om toegang te verlenen in Azure AD Azure- of Office 365 resources of andere SaaS-apps. Elk van deze gebruikers met bevoegdheden Hiermee verhoogt u de aanvallen van uw organisatie. Hierdoor kunt u gebruikers bepalen met onnodige bevoegdheden toegang en passende maatregelen over het verlagen of het risico dat zij vormen. 

Het is raadzaam dat uw organisatie gebruikmaakt van Azure AD bevoegdheden identiteitsbeheer om te beheren, wilt instellen en monitor identiteiten en hun toegang tot bronnen in Azure AD, evenals andere Microsoft online services zoals Office 365 of Microsoft Intune bevoegdheden.

Zie [Azure AD bevoegdheden identiteitsbeheer](active-directory-privileged-identity-management-configure.md)voor meer informatie. 



## <a name="see-also"></a>Zie ook

 - [Beveiliging van Azure Active Directory-identiteit](active-directory-identityprotection.md)
