<properties
    pageTitle="Windows 10-apparaten gebruiken in uw bedrijf | Microsoft Azure"
    description="Biedt een momentopname van mogelijkheden voor gebruikers en IT, contrasterende van de verschillende manieren een apparaat kan worden deze is ingericht en gebruikt in een onderneming met Windows 10."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Windows 10-apparaten gebruiken in uw bedrijf

Van toepassing op: pc's voor Windows 10

Windows 10 biedt drie modellen voor organisaties die de gebruikers toegang hebben tot werkresources in een beveiligde en gemakkelijke manier inschakelen.

- **Azure Active Directory deelnemen** (Azure AD Join), voor werknemers die toegang bronnen zoals Office 365 hoofdzakelijk in de cloud tot. Azure AD-Join is selfservice werk inrichting ervaring die is er nieuw in Windows 10.
- **Domein join**, voor organisaties die de investeringen in on-premises implementatie-apps en resources. Domein deelnemen aan biedt een verbeterde ervaring in Windows 10 indien verbonden met Azure AD.
- **Een nieuwe eenvoudigere functionaliteit voor BYOD**, voor gebruikers die u wilt toevoegen van een werkaccount of school aan Windows en eenvoudig toegang tot bronnen op persoonlijke apparaten.

De volgende tabel vindt een momentopname van mogelijkheden voor gebruikers en IT-beheerders, contrasterende van de verschillende manieren een apparaat kan worden deze is ingericht en gebruikt in een onderneming met Windows 10:

|                                                                                                                                                                 | Aan domein toevoegen     | Azure AD-Join | Persoonlijk apparaat |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows-apparaat-aanmelding voor werk of school-accounts.                                                                                                                      | Ja             | Ja           | Nee              |
| Gebruiker eenmalige aanmelding (SSO) Apps voor Office 365 en Azure AD. Eenmalige aanmelding is de mogelijkheid slechts eenmaal aanmelden voor toegang tot organisatiebronnen. | Ja             | Ja           | Ja             |
| Gebruiker SSO Kerberos/NTLM-apps.                                                                                                                                  | Ja             | Beperkt       | Ja, via VPN         |
| Sterke autorisatie en handige aanmelden voor werk of school-accounts met Microsoft Passport en Windows Hallo.                                                                   | Ja             | Ja           | Ja             |
| Toegang tot enterprise Windows Store met een werk- of schoolaccount (niet een Microsoft-account).                                                                                    | Ja             | Ja           | Ja             |
| Enterprise-compatibele roaming van gebruikersinstellingen op apparaten met accounts voor werk- of schoolaccount.                                                                                 | Ja             | Ja           | Ja             |
| De mogelijkheid om het beperken van toegang tot organisatie-apps naar apparaten die compatibel met beleid voor het organisatie-apparaat zijn.                                                      | Ja             | Ja           | Ja             |
| Gebruiker zelf de inrichting van apparaten voor "werken vanaf elke locatie".                                                                                                | Nee              | Ja           | Ja             |
| De mogelijkheid om apparaten te beheren.                                                                                                                                       | Ja, via 1U/SCCM | Ja           | Ja             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Gebruik werk eigendom apparaten met Azure AD Join en domein deelnemen in Windows 10

Windows 10 biedt twee manieren voor apparaten met een werk-eigenaar voor toegang tot werkresources:

- Azure AD-Join
- Aan domein toevoegen

 Beide kunnen geldige opties, afhankelijk van de behoeften en de vereisten van een organisatie zijn. In sommige gevallen nuttig organisaties inschakelen van beide methoden voor implementatie.

## <a name="when-to-use-azure-active-directory-join"></a>Wanneer gebruikt u Azure Active Directory deelnemen

Azure AD-Join is een nieuw werk selfservice-ervaring in Windows 10 inrichten.  Dit is gericht op werknemers die toegang werkresources zoals Office 365 hoofdzakelijk in de cloud tot. Dit is een lightweight manier voor het configureren van computers, tablets en -telefoons voor de onderneming. Apparaten worden beheerd via een mobiel apparaat management, met behulp van consistente besturingselementen voor Windows-platforms.

**Gebruik Azure AD deelnemen naar een of meer van de volgende redenen**:

- U wilt inschakelen de selfservice provisioning van apparaten voor werknemers onderweg.
- U geeft gebruikers werk eigendom mobiele apparaten zoals tablets en -telefoons.
- U wilt een set gebruikers beheren in Azure AD in plaats van in Active Directory, zoals seizoensgebonden werknemers, aannemers of leerlingen/studenten.
- U wilt voorzien van gekoppelde mogelijkheden voor werknemers in externe filialen beperkt on-premises implementatie-infrastructuur.
- U beschikt niet over een lokale Active Directory.

Sommige organisaties gebruikt Azure AD deelnemen aan de primaire manier om te implementeren werk eigendom apparaten, met name als ze of bijna alle hun resources naar de cloud migreren. Hybride organisaties met Active Directory en Azure AD kunt ook een methode of een ander afhankelijk van de gebruiker of afdeling implementeren.

Scholen en universiteit bijvoorbeeld mogelijk beheren personeel in Active Directory en leerlingen/studenten in Azure AD. Sommige bedrijven eventueel externe kantoren of verkoop afdelingen in Azure AD beheren. Azure AD Join zowel domein join methoden kunnen worden gebruikt binnen de organisatie van een hybride implementatie. Azure AD-Join kan zijn een uitstekende aanvulling om deel te nemen domein voor het implementeren van apparaten in een werkomgeving.

**Als de meest gangbare toegang tot ondernemingsresources via de cloud is, uw organisatie kan profiteren van bijkomende voordelen als**:

- Afhankelijkheden van on-premises implementatie identiteit infrastructuur, kunt u verwijderen.
- U kunt uw apparaten implementatiemodel, vereenvoudigen aan weg van afbeeldingen oplossingen doordat selfservice-configuratie.
- Mobiele apparaten beheren kunt u al uw apparaten beheren op andere platforms.

Zie voor meer informatie over het deelnemen aan Azure AD, [uitbreiden cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Wanneer gebruikt u domein join (of behouden gebruiken)

Voor de afgelopen 15 jaar, hebben veel organisaties domein join gebruikt werk apparaten te sluiten. Deze kan gebruikers Meld u aan bij zijn of haar apparaten met hun werk Active Directory- of schoolaccounts. Domein join kunt ook IT centraal en volledig deze apparaten beheren. Organisaties meestal zijn afhankelijk van de afbeeldingen methoden voor het inrichten apparaten en vaak System Center Configuration Manager (SCCM) of groep beleid (1U) gebruiken om u te beheren.

**Uw onderneming moet gebruiken join van het domein (of behouden gebruiken) naar een of meer van de volgende redenen**:

- U hebt Win32-apps die zijn geïmplementeerd op deze apparaten die NTLM/Kerberos gebruiken.
- U moet 1U of SCCM/DCM apparaten beheren.
- U wilt blijven gebruiken afbeeldingen oplossingen apparaten configureren voor uw werknemers.

**Domein-join in Windows 10 kunt u ook de volgende voordelen wanneer verbinding hebt met Azure AD**:

- Sterke apparaat gebonden verificatie en handige aanmelden voor werk of school-accounts met Microsoft Passport en Windows Hallo.
- Toegang tot de onderneming Windows Store voor apparaten die gebruiken werk- of schoolaccounts (geen Microsoft-account vereist).
- Enterprise-compatibele roaming van gebruikersinstellingen op apparaten met accounts voor werk of school (geen Microsoft-account vereist).
- De mogelijkheid om het beperken van toegang tot organisatie-apps naar apparaten die compatibel met beleid voor het organisatie-apparaat zijn.

Zie voor meer informatie over het deelnemen aan Azure AD, [uitbreiden cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Deelnemen aan van persoonsgegevens eigendom apparaten voor werk of school inschakelen

Als u wilt BYOD wordt ondersteund in de onderneming, geeft Windows 10 de gebruiker de mogelijkheid een werk- of schoolaccount-account toevoegen aan hun computer, tablet of telefoon. Nadat de gebruiker een werk- of schoolaccount-account toevoegt, wordt het apparaat is geregistreerd bij Azure AD, en eventueel geregistreerd in het systeem voor het beheer van mobiele apparaat die de organisatie is geconfigureerd. De map, wordt deze apparaten als 'Geregistreerd' versus 'Azure AD die zijn gekoppeld' weergegeven. IT-administraors verschillende beleid op basis van deze informatie die een lichtere aanraakfunctionaliteit op apparaten persoonlijk eigendom dan op apparaten werk eigendom desgewenst wilt toepassen.

Gebruikers kunnen toevoegen een werk of schoolaccount zeer snel naar hun persoonlijke apparaat. Ze kunnen dit bij het openen van een werktoepassing voor de eerste keer, of ze deze handmatig kunnen doen via het menu instellingen. Dit account, vindt u eenmalige aanmelding naar organisatiebronnen.

Zie [verbinding maken met een domein behoren apparaten om over te Azure AD voor Windows 10-ervaringen](active-directory-azureadjoin-devices-group-policy.md)voor meer informatie over het deelnemen aan Azure AD.

## <a name="enable-domain-join-or-azure-ad-join"></a>Domein join of Azure AD Join inschakelen

Alle methoden hierboven (domein-join, Azure AD bijwonen en toevoegen werken of schoolaccount) hebt invoer wordt verwezen in de gebruikerservaring voor Windows 10. Alle vereisen echter een IT-beheerder om te schakelen van de functionaliteit in de infrastructuur voordat de ervaring werkt.

## <a name="requirements-for-deploying-azure-ad-join"></a>Vereisten voor de implementatie van Azure AD deelnemen

Als u wilt deelnemen aan Azure AD implementeren voor elke set gebruikers hebt u het volgende nodig:

- Een Azure AD-abonnement.
- Een Azure AD Premium-abonnement, zoals mobiele apparaat management automatisch inschrijven, als u meer mogelijkheden vereist.
- Mobile device management--voor voorbeeld, een Microsoft-Intune-abonnement, mobile device management voor Office 365 of een van de partner mobiele apparaat management leveranciers met Azure AD integreren. (Zie het [gedeelte Veelgestelde vragen](#frequently-asked-questions) aan het einde van dit artikel voor meer informatie).

Als de faciliteiten hybride, raden we dat u Azure AD Connect om uit te breiden van de on-premises adreslijst naar Azure AD implementeert.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Vereisten voor het gebruik van domein deelnemen via Azure AD

Domein join blijft werken zoals altijd heeft. Echter als u de voordelen van Azure AD moet u het volgende:

- Een Azure AD-abonnement.
- Een implementatie van Azure AD Connect uit te breiden de on-premises adreslijst Azure AD.
- Een beleid waarmee apparaten voorwaardelijke toegang heeft tot Azure AD domein behoren.
- Een beleid voor toegang tot apparaten 'domein behoren' als u kunnen wilt beperken van toegang voor sommige apparaten.
- System Center Configuration Manager versie 1509 voor Technical Preview, om in te schakelen van regels voor het vereisen van compatibele apparaten. (Zie de documentatie van TechNet en blogbericht).

Zie voor meer informatie over de aan domein toevoegen in Windows 10, [verbinding maken met een domein behoren apparaten om over te Azure AD voor Windows 10-ervaringen](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Vereisten voor het gebruik van BYOD en 'Een werk- of schoolaccount-account toevoegen'

Om in te schakelen "doen om uw eigen apparaat" (BYOD) met accounts voor werk of school, moet u de volgende handelingen uit:

- Een Azure AD-abonnement.
- Een Azure AD Premium-abonnement, zoals mobiele apparaat management automatisch inschrijven, als u meer mogelijkheden vereist.

## <a name="requirements-for-using-microsoft-passport"></a>Vereisten voor het gebruik van Microsoft Passport

Als u wilt Microsoft Passport inschakelt, moet u het volgende:

- Een openbare sleutel sleutelinfrastructuur voor certificaatverificatie ondersteuning in Microsoft Passport gebruikt.
- Een abonnement Intune voor certificaatverificatie ondersteuning die gebruikmaakt van Microsoft Passport voor het deelnemen aan Azure AD en accounts voor werk- of schoolaccount.
- System Center Configuration Manager versie 1509 voor Technical Preview (Zie de documentatie van TechNet en blogbericht) voor certificaatverificatie ondersteuning die Microsoft Passport voor join domein wordt gebruikt.
- Een beleid voor het inschakelen van Microsoft Passport in de organisatie.

Als alternatief voor het gebruik van een PKI, kunt u Microsoft Passport op basis van sleutel inschakelen door het volgende te doen:

- Implementeren 2016 voor Windows Server "Productie Preview 1" DCs (niet dat u een domein of bos functionele niveaus; verschillende DCs redundantie halen die elke Active Directory-site is voldoende).
- Stel Microsoft Passport is ingeschakeld in de organisatie.

Zie < link-to-MS-Passport-and-Windows-Hello-document > voor meer informatie over Microsoft Passport en Windows Hallo in Windows 10.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Welke producten partner mobiele apparaat management integreren met Azure AD?

De volgende leveranciersproducten integreren met Azure AD voor geïntegreerd registratie en voorwaardelijke toegang in Windows 10:

- AirWatch door VMware
- Citrix Xenmobile
- Lightspeed Mobile Manager
- SOTI on-premises implementatie mobiele apparaten beheren
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Hoe zit het met werkplek deelnemen in Windows 10?
Bedrijf Join is gebruikt in Windows 8.1 BYOD inschakelen. Klik in Windows 10 is BYOD ingeschakeld via "Toevoegen van een werk- of schoolaccount" zoals eerder in dit document. Voor organisaties die hun mobiele apparaten beheren niet met Azure AD integreren, kunnen gebruikers het apparaat inschrijven in het beheer van handmatig via **Instellingen** > **Accounts** > **werk access**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Kunnen gebruikers verbinding maken met hun Microsoft-account op de domeinaccount in Windows 10?
Niet in Windows 10. In Windows 8.1, gebruikers van een domein behoren apparaten kunnen "verbinding met' hun Microsoft-account (bijvoorbeeld Hotmail, Live, Outlook, Xbox, enzovoort) hun domeinaccount om in te schakelen voor bepaalde ervaringen zoals SSO naar Live-services, met gebruik van de Windows Store en roaming van gebruikersinstellingen op apparaten. Klik in Windows 10 is het Microsoft-account 'koppelen' functionaliteit teruggetrokken. De gebruiker kan een of meer Microsoft-accounts toevoegen als extra accounts om te schakelen van eenmalige aanmelding voor consumenten-mailservices, zoals de Windows Store. Dit doet u in **Instellingen** > **Accounts** > **uw account**.

Gebruikers die een upgrade uitvoert voor Windows 8.1 domein behoren apparaten heeft, en die hun Microsoft-account koppelt, krijgen automatisch hun verbonden Microsoft-account hebt toegevoegd aan de lijst met extra accounts die ze gebruiken.


## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
