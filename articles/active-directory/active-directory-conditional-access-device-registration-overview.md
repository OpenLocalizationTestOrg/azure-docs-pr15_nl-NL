<properties
    pageTitle="Overzicht van Azure Active Directory-apparaatregistratie | Microsoft Azure"
    description="is de basis voor apparaat gebaseerde voorwaardelijke-scenario's. Wanneer een apparaat is geregistreerd, bepalingen Azure Active Directory-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer de gebruiker zich aanmeldt."
    services="active-directory"
    keywords="apparaatregistratie, apparaatregistratie inschakelen, apparaatregistratie en MDM"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="Markvi"/>

# <a name="get-started-with-azure-active-directory-device-registration"></a>Aan de slag met Azure Active Directory-apparaatregistratie

Azure Active Directory-apparaatregistratie is de basis voor apparaat gebaseerde voorwaardelijke-scenario's. Wanneer een apparaat is geregistreerd, biedt Azure Active Directory-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer de gebruiker zich aanmeldt. Het apparaat dat geverifieerde en de kenmerken van het apparaat, kunnen vervolgens worden gebruikt om af te dwingen voorwaardelijke-beleid voor toepassingen die worden gehost in de cloud en on-premises implementatie.

Wanneer gecombineerd met een mobiel apparaat management(MDM) oplossing zoals Intune van Microsoft, wordt de kenmerken van het apparaat in Azure Active Directory worden bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u voorwaardelijke toegangsregels die toegang vanaf apparaten om te voldoen aan de standaarden voor beveiliging en naleving afdwingen maken. Zie voor meer informatie over apparaten in Microsoft Intune inschrijven, [inschrijven apparaten voor management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Scenario's die zijn ingeschakeld door apparaatregistratie van Azure Active Directory

Azure Active Directory-apparaatregistratie biedt ondersteuning voor iOS, Android en Windows apparaten. De afzonderlijke scenario's die gebruikmaken van Azure AD-apparaatregistratie wellicht meer informatie over vereisten en platform ondersteunen. Deze scenario's zijn als volgt:

- **Voorwaardelijke toegang tot uw toepassingen die zijn gehoste on-premises implementatie**: U kunt geregistreerde apparaten gebruiken met clienttoegangsbeleid voor toepassingen die zijn geconfigureerd voor gebruik van AD FS met Windows Server 2012 R2. Zie voor meer informatie over het instellen van voorwaardelijke toegang voor on-premises implementatie, [bij het instellen van On-premises voorwaardelijke toegang tot een Azure Active Directory-apparaatregistratie](active-directory-conditional-access-on-premises-setup.md).

- **Voorwaardelijke toegang voor Office 365-toepassingen met Microsoft Intune** : IT-beheerders voorwaardelijke apparaat clienttoegangsbeleid als u wilt beveiligen bedrijfsresources, terwijl op hetzelfde moment waardoor de informatiewerkers op compatibele apparaten voor toegang tot de services kunt inrichten. Zie [Voorwaardelijke Clienttoegangsbeleid van het type apparaat voor Office 365-services](active-directory-conditional-access-device-policies.md)voor meer informatie.

##<a name="setting-up-azure-active-directory-device-registration"></a>Bij het instellen van Azure Active Directory-apparaatregistratie

U moet Azure AD-apparaatregistratie in de Portal Azure inschakelen zodat mobiele apparaten de service herkent bekende DNS-records kunnen detecteren. U moet de DNS van uw bedrijf zodanig configureren dat Windows 10, Windows 8.1, Windows 7, Android en iOS-apparaten kunnen ontdekken en gebruik van de service.
U kunt weergeven en geregistreerde apparaten met behulp van de beheerder-Portal in Azure Active Directory in-/ uitschakelen.

>[AZURE.NOTE]
 Meest recente Zie voor instructies voor het instellen van automatische apparaatregistratie, [Automatische registratie van Windows-domein instellen die zijn gekoppeld apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

### <a name="enable-azure-active-directory-device-registration-service"></a>Registratie-Service van Azure Active Directory-apparaat inschakelen

1. Meld u aan bij de Microsoft Azure-portal als beheerder.
2. Selecteer in het linkerdeelvenster, **Active Directory**.
3. Selecteer het telefoonboek van uw op het tabblad **map** .
4. Selecteer het tabblad **configureren** .
5. Schuif naar de sectie **apparaten**.
6. Selecteer **alle** voor **gebruikers mogelijk WERKPLEK JOIN-apparaten**.
7. Selecteer het maximum aantal apparaten die u wilt machtigen per gebruiker.

>[AZURE.NOTE]
>Registratie met Microsoft Intune of Mobile Device Management voor Office 365 is vereist werkplek deelnemen aan. Als u een van deze services hebt geconfigureerd, alles is geselecteerd en de knop is uitgeschakeld.

Tweeledige verificatie is standaard niet ingeschakeld voor de service. Tweeledige verificatie is echter aangeraden wanneer u een apparaat registreren.

- Voordat u tweeledige verificatie vereisen voor deze service, moet u een provider tweeledige verificatie configureren in Azure Active Directory en de gebruikersaccounts configureren voor meervoudige verificatie: Zie [Meervoudige verificatie toe te voegen met Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)

- Als u AD FS gebruikt met Windows Server 2012 R2, moet u een module tweeledige verificatie configureren in AD FS, raadpleegt u [Meervoudige verificatie met Active Directory Federation Services gebruiken](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Azure Active Directory-apparaatregistratie discovery configureren
Windows 7 en Windows 8.1 apparaten wordt de apparaatregistratie-service via het combineren van de naam van de gebruikersaccount met de naam van een bekende apparaatregistratie-server vinden.

U moet een DNS CNAME-record die naar de A-record dat is gekoppeld aan uw apparaatregistratie van Azure Active Directory-service verwijst maken. De CNAME-record, moet de bekende voorvoegsel enterpriseregistration gevolgd door de UPN-achtervoegsel gebruikt door de gebruikersaccounts in uw organisatie gebruiken. Als uw organisatie gebruikmaakt van meerdere UPN-achtervoegsels, kan meerdere CNAME-records moeten worden gemaakt in DNS.

Als u twee UPN-achtervoegsels gebruiken in uw organisatie met de naam bijvoorbeeld @contoso.com en @region.contoso.com, u de volgende DNS-records maakt.

| Invoer                                     | Type  | Adres                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.Region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Apparaatobjecten in Azure Active Directory beheren en bekijken
1. In de portal Azure-beheerder kunt u bekijken, blokkeren en deblokkeren apparaten. Een apparaat dat is geblokkeerd hebben niet langer toegang tot de toepassingen die zijn geconfigureerd voor alleen geregistreerde apparaten.
2. Meld u aan bij de Microsoft Azure-Portal als beheerder.
3. Selecteer in het linkerdeelvenster, **Active Directory**.
4. Selecteer de map.
5. Selecteer het tabblad **gebruikers** . Selecteer een gebruiker zijn of haar apparaten bekijken
6. Selecteer het tabblad **apparaten** .
7. Selecteer **Geregistreerde apparaten** in de vervolgkeuzelijst.
8. Hier kunt u bekijken, blokkeren of deblokkeren van de gebruikers geregistreerde apparaten.

## <a name="additional-topics"></a>Aanvullende onderwerpen

U kunt uw apparaten met Windows 7 en Windows 8.1 domein samengevoegd met Azure AD-apparaatregistratie registreren. De volgende onderwerpen vindt u meer informatie over de vereisten en de benodigde stappen voor het configureren van apparaatregistratie op apparaten met Windows 7 en Windows 8.1.

- [Automatische apparaatregistratie met Azure Active Directory voor apparaten met Windows domein behoren](active-directory-conditional-access-automatic-device-registration.md)
- [Automatische apparaatregistratie voor apparaten met Windows 7-domein gekoppeld configureren](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Automatische apparaatregistratie voor apparaten met Windows 8.1 domein toegevoegd configureren](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische apparaatregistratie met een domein behoren Azure Active Directory voor Windows 10-apparaten](active-directory-azureadjoin-devices-group-policy.md)
