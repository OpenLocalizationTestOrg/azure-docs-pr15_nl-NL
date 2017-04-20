<Properties
    pageTitle="Azure Active Directory voorwaardelijke toegang | Microsoft Azure"  
    description="Gebruik voorwaardelijke toegangsbeheer in Azure Active Directory wilt controleren op specifieke voorwaarden bij de verificatie voor toegang tot toepassingen."  
    services="active-directory"
    keywords="voorwaardelijke toegang tot de apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, voorwaardelijke clienttoegangsbeleid"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Voorwaardelijke toegang in Azure Active Directory   

De mogelijkheden van het besturingselement in Azure Active Directory (Azure AD) voorwaardelijke toegang bieden eenvoudige manieren om u te helpen beveiligen resources in de cloud en on-premises implementatie. Voorwaardelijke clienttoegangsbeleid zoals meervoudige verificatie kunt beschermen tegen de kans op pesterijen gestolen en phished referenties. Andere voorwaardelijke clienttoegangsbeleid kunnen u de gegevens van uw organisatie privénotities kunt beschermen. Bijvoorbeeld naast het vereisen van referenties, wellicht een beleid dat alleen apparaten die zijn geregistreerd in een mobiel apparaat management-systeem, zoals Microsoft Intune hebben toegang tot gevoelige services van uw organisatie.


## <a name="prerequisites"></a>Vereisten voor

Azure AD voorwaardelijke toegang is een functie van [Azure Active Directory Premium](http://www.microsoft.com/identity). Elke gebruiker die toegang heeft tot een toepassing waarop voorwaardelijke clienttoegangsbeleid toegepast moet een Azure AD Premium-licentie hebben. U kunt meer informatie over licentievereisten in [zonder licentie gebruikersrapport](https://aka.ms/utc5ix).


## <a name="how-is-conditional-access-control-enforced"></a>Hoe wordt voorwaardelijke toegangsbeheer afgedwongen?  

Met voorwaardelijke toegangsbeheer op hun plaats staan, Azure AD gecontroleerd op de specifieke voorwaarden instellen voor een gebruiker voor toegang tot een toepassing. Nadat access voorwaarden is voldaan, wordt de gebruiker is geverifieerd en de toepassing kan openen.  

![Overzicht van voorwaardelijke toegang](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Voorwaarden

Dit zijn de voorwaarden die u wilt in een voorwaardelijke-beleid opnemen:

- **Groepslidmaatschap**. Een gebruikerswachtwoord toegangsbeheer op basis van lidmaatschap in een groep.

- **Locatie**. Gebruik van de locatie van de gebruiker trigger meervoudige verificatie en blokkeren besturingselementen gebruiken wanneer een gebruiker zich niet op een vertrouwde netwerk.

- **Apparaat-platform**. Het apparaatplatform, zoals iOS-, Android-, Windows Mobile- of Windows, als een voorwaarde voor het toepassen van beleid gebruiken.

- Het **apparaat is ingeschakeld**. Status van het apparaat, is of in- of uitgeschakeld, gevalideerd tijdens de evaluatie van apparaat beleid. Als u een apparaat verloren of gestolen in de adreslijst uitschakelt, kan deze niet meer voldoen aan vereisten.

- **Aanmeldingsadres en gebruikersnaam risico**. U kunt [Azure AD identiteit beveiliging](active-directory-identityprotection.md) gebruiken voor clienttoegangsbeleid voorwaardelijke risico. Voorwaardelijke risico clienttoegangsbeleid helpen geven van uw organisatie vooraf beveiliging op basis van de risico's en ongebruikelijke aanmeldingsproblemen activiteiten.


## <a name="controls"></a>Besturingselementen

Dit zijn de besturingselementen die u kunt een voorwaardelijke-beleid afdwingen:

- **Meervoudige verificatie**. U kunt sterke verificatie via meervoudige verificatie vereist. U kunt meervoudige verificatie met Azure meervoudige verificatie of met behulp van een on-premises implementatie meervoudige verificatie-provider, gecombineerd met Active Directory Federation Services (AD FS). Gebruik van meervoudige verificatie voorkomt resources door een onbevoegde gebruiker die mogelijk hebben toegang tot de referenties van een geldige gebruiker wordt geopend.

- **Blokkeren**. U kunt voorwaarden zoals de gebruikerslocatie toegang geven gebruiker toepassen. U kunt bijvoorbeeld toegang blokkeren wanneer een gebruiker zich niet op een vertrouwde netwerk.

- **Compatibele apparaten**. U kunt voorwaardelijke-beleid op het apparaatniveau van het instellen. U kunt een beleid instellen zodat alleen computers die domein behoren zijn of mobiele apparaten die zijn geregistreerd in een toepassing voor het beheer van mobiele apparaat toegankelijk bronnen van uw organisatie. U kunt bijvoorbeeld Intune nagegaan apparaat gebruikt, en vervolgens meld dit dan op Azure AD voor afdwingen wanneer de gebruiker probeert te krijgen tot een toepassing. Zie [apps beveiligen en gegevens met Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune)voor gedetailleerde informatie over het gebruik van Intune om apps en gegevens te beschermen. U kunt ook Intune gebruiken om af te dwingen gegevensbescherming voor verloren of gestolen apparaten. Raadpleeg de [Help van uw gegevens beschermen met volledige of selectief wissen op met Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)voor meer informatie.

## <a name="applications"></a>Toepassingen

U kunt een voorwaardelijke-beleid op het toepassingsniveau van de afdwingen. Toegangsniveaus voor toepassingen en services instellen in de cloud of on-premises implementatie. Het beleid wordt toegepast rechtstreeks op de website of service. Het beleid wordt afgedwongen voor toegang tot de browser, en toepassingen die toegang tot de service.


## <a name="device-based-conditional-access"></a>Voorwaardelijke toegang op basis van het apparaat

U kunt toegang tot toepassingen beperken op apparaten die zijn geregistreerd met Azure AD en die voldoen aan bepaalde voorwaarden. Voorwaardelijke toegang apparaat gebaseerde beschermen van een organisatie resources van gebruikers die proberen te krijgen tot de resources uit:

- Onbekende of niet-beheerde apparaten.
- Apparaten die niet voldoen aan de beveiligingsbeleid voor uw organisatie is ingesteld.

Beleidsregels op basis van deze vereisten is voldaan, kunt u instellen:

- **Een domein behoren apparaten**. Een beleid voor het beperken van toegang aan apparaten die zijn gekoppeld aan een on-premises Active Directory-domein en dat ook zijn geregistreerd met Azure AD ingesteld. Dit beleid is van toepassing op Windows desktopcomputers, laptops en enterprise-tablets.
Zie [Automatische registratie van Windows instellen met een domein behoren apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)voor meer informatie over het instellen van automatische registratie van een domein behoren apparaten met Azure AD.

- **Compatibele apparaten**. Een beleid voor het beperken van toegang aan apparaten die zijn gemarkeerd als **compatibele** in de systeemmap management ingesteld. Dit beleid zorgt ervoor dat alleen de apparaten die voldoen aan beveiligingsbeleid voor apparaten zoals afdwingen bestandsversleuteling op een apparaat toegang hebben. U kunt dit beleid gebruiken voor het beperken van toegang van de volgende apparaten:

    - **Windows op apparaten met een domein behoren**. Beheerd door System Center Configuration Manager (in de huidige tak) geïmplementeerd in een hybride configuratie.
    - **Windows 10 Mobile werk- of persoonlijke apparaten**. Beheerd door Intune of met een ondersteunde derden mobiele apparaat managementsysteem.
    - **iOS en Android-apparaten**. Beheerd door Intune.


Gebruikers die toegang hebben tot de toepassingen die zijn beveiligd met een apparaat op basis van beleid moet toegang tot de toepassing vanaf een apparaat die voldoet aan de vereisten van dit beleid. Toegang als geprobeerd op een apparaat die niet aan de vereisten.

Zie [voorwaardelijke-apparaat gebaseerde beleid voor Azure Active Directory - toepassingen instellen](active-directory-conditional-access-policy-connected-applications.md)voor informatie over het configureren van een apparaat gebaseerde beleid in Azure AD.

## <a name="resources"></a>Resources

Zie de volgende categorieën van de resource en de volgende artikelen voor meer informatie over het instellen van voorwaardelijke toegang voor uw organisatie.


### <a name="multi-factor-authentication-and-location-policies"></a>Beleidsregels voor meervoudige verificatie en locatie

- [Aan de slag met voorwaardelijke toegang tot Azure AD-verbonden-apps op basis van het deelvenster Groeperen, locatie en beleidsregels voor meervoudige verificatie](active-directory-conditional-access-azuread-connected-apps.md)
- [Toepassingen die worden ondersteund](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Voorwaardelijke toegang op basis van het apparaat

- [Voorwaardelijke-apparaat gebaseerde beleid voor toegangsbeheer ingesteld op Azure Active Directory-toepassingen](active-directory-conditional-access-policy-connected-applications.md)
- [Automatische registratie van Windows instellen met een domein behoren apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Probleemoplossing voor toegangsproblemen Azure Active Directory](active-directory-conditional-access-device-remediation.md)
- [Beschermen van gegevens op verloren of gestolen apparaten met behulp van Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Resources op basis van aanmeldingsproblemen risico beveiligen

-   [Beveiliging van Azure AD-identiteit](active-directory-identityprotection.md)

### <a name="next-steps"></a>Volgende stappen

- [Voorwaardelijke toegang Veelgestelde vragen](active-directory-conditional-faqs.md)
- [Technische verwijzing](active-directory-conditional-access-technical-reference.md)
