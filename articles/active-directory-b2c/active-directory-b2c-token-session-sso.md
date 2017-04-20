<properties
    pageTitle="Azure Active Directory-B2C: Token, sessie en configuratie voor eenmalige aanmelding | Microsoft Azure"
    description="Token, sessie en configuratie van eenmalige aanmelding in Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory-B2C: Token, sessie en configuratie voor eenmalige aanmelding

Deze functie kunt u fijnmazige bepalen, [per beleid soort_jaar](active-directory-b2c-reference-policies.md), van:
 
1. Levensduur van beveiligingstokens verstrekt door B2C Azure Active Directory (Azure AD).
2. Levensduur van web application-sessies beheerd door Azure AD B2C.
3. Eenmalige aanmelding (SSO) de gedrag op meerdere apps en beleidsregels in uw tenant B2C.

U kunt deze functie gebruiken in uw tenant B2C als volgt:

1. Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
2. Klik op **aanmelden beleid**. *Opmerking: U kunt deze functie gebruiken op een willekeurig beleidstype, niet alleen op* *Beleid voor het aanmelden***.
3. Open een beleid door erop te klikken. Bijvoorbeeld, klik op **B2C_1_SiIn**.
4. Klik op **bewerken** aan de bovenkant van het blad.
5. Klik op **Token, sessie en eenmalige aanmelding config**.
6. Breng de gewenste wijzigingen aan. Meer informatie over de beschikbare eigenschappen in de volgende secties.
7. Klik op **OK**.
8. Klik boven aan het blad op **Opslaan** .

![Schermafbeelding van token, sessie en config voor eenmalige aanmelding](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Token levensduur configuratie

Azure AD B2C ondersteunt het [OAuth 2.0 autorisatie protocol](active-directory-b2c-reference-protocols.md) voor beveiligde toegang bieden tot beveiligde bronnen. Als u wilt deze ondersteuning implementeren, genereert Azure AD B2C verschillende [beveiligingstokens](active-directory-b2c-reference-tokens.md). Hierna ziet u de eigenschappen die u gebruiken kunt voor het beheren van de levensduur van beveiligingstokens verstrekt door Azure AD B2C:

- **Access & ID token levensduur (minuten)**: de levensduur van de 2.0 OAuth-dragertoken gebruikt voor toegang tot een beveiligde bron. Azure AD B2C problemen alleen ID tokens op dit moment. Deze waarde wilt toepassen op access tokens, ook als we ondersteuning voor deze toevoegt.
   - Standaard = 60 minuten.
   - Minimum (inclusief) = 5 minuten.
   - Maximum (inclusief) = 1440 minuten.
- **Levensduur van tokens vernieuwen (dagen)**: de maximale tijdsperiode waarvóór een token vernieuwen kan worden gebruikt om een nieuwe access en ID token aan te schaffen (en desgewenst een nieuwe vernieuwen token, als uw toepassing is verleend de `offline_access` bereik).
   - Standaard = 14 dagen.
   - Minimum (inclusief) = 1 dag.
   - Maximum (inclusief) = 90 dagen.
- **Vernieuwen token schuiven venster levensduur (dagen)**: nadat deze periode is verstreken van de gebruiker wordt gedwongen om te verifiëren opnieuw, ongeacht de geldigheidsperiode van de meest recente vernieuwen token opgehaald door de toepassing. Dit kan alleen worden verstrekt als de schakeloptie is ingesteld op **dat wordt begrensd**. Deze moet groter is dan of gelijk is aan de **levensduur van tokens vernieuwen (dagen)** waarde. Als de schakeloptie is ingesteld op **Unbounded**, niet kunt u een bepaalde waarde opgeven.
   - Standaard = 90 dagen.
   - Minimum (inclusief) = 1 dag.
   - Maximum (inclusief) = 365 dagen.

Dit zijn een paar gebruik zaken die u kunt inschakelen met deze eigenschappen:

- Een gebruiker toestaan te laten staan, aangemeld bij een mobiele toepassing blijven zo lang maken als hij of zij continu actief op de toepassing is. U kunt dit doen door in te stellen de **vernieuwen schuifregelaar venster levensduur van tokens (dagen)** overschakelen naar **Unbounded** in uw beleid aanmelden.
- Voldoen aan uw bedrijfstak van de beveiliging en nalevingsvereisten door in te stellen van de juiste-token levensduur.

## <a name="session-configuration"></a>Configuratie van de sessie

Azure AD B2C ondersteunt het [protocol voor verificatie OpenID verbinding maken](active-directory-b2c-reference-oidc.md) voor het inschakelen van secure bij de aanmelding bij webtoepassingen. Hierna ziet u de eigenschappen die u gebruiken kunt voor het beheren van web application sessies:

- **Sessie levensduur (minuten) in de browser**: de levensduur van Azure AD B2C sessiecookie die is opgeslagen op de browser van de gebruiker na verificatie is geslaagd.
   - Standaard = 1440 minuten.
   - Minimum (inclusief) = 15 minuten.
   - Maximum (inclusief) = 1440 minuten.
- **Web app-sessietime**: als deze schakeloptie is ingesteld op **Absolute**, wordt de gebruiker gedwongen opnieuw worden geverifieerd na de periode die is opgegeven door de **Web-app sessie levensduur (minuten)** is verstreken. Als deze schakeloptie is ingesteld op **rollend** (de standaardinstelling), blijft de gebruiker aangemeld zo lang maken als de gebruiker in uw webtoepassing continu actief is.

Dit zijn een paar gebruik zaken die u kunt inschakelen met deze eigenschappen:

- Voldoen aan vereisten voor de beveiliging en naleving van van uw bedrijfstak door in te stellen van de juiste web application-sessie levensduur.
- Verplicht tot nieuwe verificatie na een periode tijd instellen tijdens de interactie met een deel van de beveiliging van uw webtoepassing. 

## <a name="single-sign-on-sso-configuration"></a>Configuratie van eenmalige aanmelding (SSO)

Als u meerdere toepassingen en beleid in uw tenant B2C hebt, kunt u de gebruikersinteracties over deze met de eigenschap **configuratie voor eenmalige aanmelding** beheren. U kunt de eigenschap instellen op een van de volgende instellingen:

- **Tenant**: dit is de standaardinstelling. Met deze instelling kan meerdere toepassingen en beleidsregels in uw tenant B2C om de dezelfde gebruikerssessie te delen. Bijvoorbeeld wanneer een gebruiker zich in een toepassing, Contoso winkelen hij of zij kan ook naadloos zich aanmelden bij een andere één, Contoso-faculteit, na het openen van deze.
- **Toepassing**: Hiermee kunt u voor het behoud van een gebruikerssessie uitsluitend voor een toepassing weer te geven, onafhankelijk van andere toepassingen. Bijvoorbeeld als u wilt dat de gebruiker te melden bij Contoso faculteit (met dezelfde referenties), zelfs als hij of zij is al aangemeld bij Contoso winkelen, een andere toepassing op dezelfde B2C tenant. 
- **Beleid**: Hiermee kunt u voor het behoud van een gebruikerssessie uitsluitend voor een beleid, ongeacht de toepassingen gebruiken. Bijvoorbeeld als de gebruiker heeft al aangemeld en een stap meerdere factor verificatie (MFA) voltooid, kunt hij of zij toegang krijgen tot betere beveiliging delen van meerdere toepassingen zolang de sessie die is gekoppeld aan het beleid niet verlopen.
- **Uitgeschakeld**: dit zorgt ervoor dat de gebruiker om uit te voeren via de hele gebruiker reis op elke uitvoering van het beleid. Bijvoorbeeld Hierdoor kunnen meerdere gebruikers te registreren in uw toepassing (in een gedeeld bureaublad scenario), zelfs terwijl u één gebruiker blijft aangemeld tijdens de hele periode.
