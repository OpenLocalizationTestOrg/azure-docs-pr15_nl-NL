<properties
    pageTitle="Voorwaardelijke-apparaatbeleid voor Office 365-services | Microsoft Azure"
    description="Meer informatie over hoe apparaat op basis van voorwaarden toegang tot Office 365-services beheren. Hoewel informatiewerkers (kenniswerkers) toegang tot Office 365-services, zoals Exchange en SharePoint Online op het werk of school vanaf hun persoonlijke apparaten, wil hun IT-beheerder de toegang tot worden secure.IT beheerders voorwaardelijke apparaat clienttoegangsbeleid als u wilt beveiligen bedrijfsresources, terwijl op hetzelfde moment kenniswerkers toestaan op compatibele apparaten voor toegang tot de services kunt inrichten."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Voorwaardelijke-apparaatbeleid voor Office 365-services

De term, "voorwaardelijke toegang" heeft vele voorwaarden die zijn gekoppeld aan dit zoals meervoudige geverifieerde gebruiker, geverifieerd apparaat, compatibele apparaat enzovoort. In dit onderwerp wordt hoofdzakelijk focusses van apparaat gebaseerde voorwaarden toegang tot Office 365-services te beheren. Hoewel informatiewerkers (kenniswerkers) toegang tot Office 365-services, zoals Exchange en SharePoint Online op het werk of school vanaf hun persoonlijke apparaten, wil hun IT-beheerder de toegang tot veilig. IT-beheerders kunt inrichten van voorwaardelijke apparaat clienttoegangsbeleid als u wilt beveiligen bedrijfsresources, terwijl op hetzelfde moment kenniswerkers toestaan op compatibele apparaten voor toegang tot de services. Voorwaardelijke toegangsbeleid naar Office 365 kan worden geconfigureerd via Microsoft Intune voorwaardelijke access-portal.

Azure Active Directory wordt afgedwongen voorwaardelijke clienttoegangsbeleid beveiligde toegang tot Office 365-services. Een tenant-beheerder kunt maken van een voorwaardelijke-beleid dat kan een gebruiker op een niet-compatibele apparaat toegang krijgen tot een O365-service. De gebruiker moet voldoen aan apparaat van bedrijfsbeleid voordat toegang kan worden verleend met de service. U kunt ook kunt de beheerder ook een beleid dat, gebruikers alleen hun apparaat te registreren moeten toegang tot een O365-service maken. Beleid mogelijk worden toegepast op alle gebruikers van een organisatie, of beperkt tot een paar doelgroepen en enhanced na verloop van tijd om op te nemen aanvullende doelgroepen.

Vereist voor het afdwingen van beleid voor het apparaat is voor gebruikers hun apparaten registreren met apparaatregistratie van Azure Active Directory-service. U kunt ervoor kiezen om in te schakelen meervoudige verificatie (MFA) voor het registreren van apparaten met apparaatregistratie van Azure Active Directory-service. MFA geschikt is voor apparaatregistratie van Azure Active Directory-service. Wanneer MFA is ingeschakeld, worden gebruikers hun apparaten registreren met apparaatregistratie van Azure Active Directory-service voor de tweede factor verificatie gecontroleerd.

##<a name="how-does-conditional-access-policy-work"></a>Hoe werkt voorwaardelijke-beleid?

Wanneer een gebruiker aanvragen toegang tot O365-service van een platform ondersteund apparaat, Azure Active Directory wordt geverifieerd door de gebruiker en apparaat waaruit de gebruiker de aanvraag; Start en verleent toegang tot de service alleen wanneer de gebruiker aan het beleid instellen voor de service voldoet. Gebruikers die ik heb geen diens apparaat geregistreerd krijgt corrigerende instructies voor het registreren en compatibel voor toegang tot zakelijke O365-services. Gebruikers op iOS en Android-apparaten moeten hun apparaat met bedrijfsportal-toepassing te registreren. Wanneer een gebruiker zich haar apparaat inschrijft, is het apparaat dat geregistreerd bij Azure Active Directory, en voor Apparaatbeheer en naleving geregistreerd. Klanten moeten de apparaatregistratie van Azure Active Directory-service gebruiken in combinatie met Microsoft Intune om in te schakelen van mobile device management voor Office 365-service. Apparaatregistratie is spelen voor gebruikers met toegang tot Office 365-services wanneer het beleid voor het apparaat, worden afgedwongen.

Wanneer een gebruiker zich haar apparaat succes inschrijft, wordt het apparaat vertrouwde. Azure Active Directory biedt één eenmalige aanmelding naar access bedrijfstoepassingen en worden toegepast in voorwaardelijke-beleid om toegang tot een service niet alleen de eerste keer dat de gebruiker toegang, maar elke keer dat de gebruiker aanvraagt verlengen toegang aanvraagt. De gebruiker wordt toegang tot services geweigerd bij het aanmelden referenties zijn gewijzigd, apparaat wordt verbroken/gestolen of het beleid niet op het moment van de aanvraag voor verlenging is voldaan.

## <a name="deployment-considerations"></a>Aandachtspunten voor de implementatie:
Apparaatregistratie van Azure Active Directory-service voor apparaatregistratie moet worden gebruikt.

Wanneer gebruikers moeten worden geverifieerd on-premises, is Active Directory Federation Services (AD FS) (1,0 en hoger) vereist. Meervoudige verificatie voor het bedrijf deelnemen, mislukt wanneer de identiteitsprovider niet kan meervoudige verificatie. AD FS 2.0 is bijvoorbeeld niet meervoudige verificatie uitvoeren. Tenant-beheerder moet ervoor zorgen dat de lokale AD FS is MFA staat en een geldig MFA-methode is ingeschakeld, voordat u MFA inschakelt op apparaatregistratie van Azure Active Directory-service. AD FS op Windows Server 2012 R2 bevat bijvoorbeeld mogelijkheden voor MFA. Voordat u MFA van Azure Active Directory-apparaatregistratie service inschakelt, moet u ook een extra geldige (MFA) verificatiemethode op de AD FS-server inschakelen. Zie voor meer informatie over ondersteunde MFA methoden voor AD FS extra verificatiemethoden configureren voor AD FS.

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ)

V: Wat is het standaardbeleid voor uitsluiting voor platforms voor niet-ondersteunde apparaten?

A: op dit moment gelden voorwaardelijke clienttoegangsbeleid selectief voor de gebruikers op iOS en Android-apparaten. Toepassingen op andere platforms apparaat zijn standaard niet beïnvloed door het beleid voorwaardelijke toegang voor iOS en Android-apparaten. Tenant-beheerder kan echter kiezen om te negeren het globaal beleid als u wilt toestaan van toegang tot gebruikers op niet-ondersteunde platforms.
Het is op de routekaart voor het uitbreiden van voorwaardelijke-beleid aan gebruikers op andere platforms, met inbegrip van Windows.

V: wanneer wordt voorwaardelijke-beleid naar Office 365-services uitgebreid tot op basis van Browser-apps (bijvoorbeeld OWA, SharePoint op basis van browser).

A: op dit moment is voorwaardelijke toegang tot Office 365-services beperkt tot uitgebreide toepassingen op apparaat. Het is op de routekaart voor het uitbreiden van voorwaardelijke-beleid aan gebruikers toegang krijgen tot de services in browsers.
