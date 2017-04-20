<properties
    pageTitle="Wachtwoordbeleidsregels en beperkingen in Azure Active Directory | Microsoft Azure"
    description="Beschrijving van de beleidsregels voor wachtwoorden in Azure Active Directory, inclusief toegestane tekens, lengte en vervaldatum"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Wachtwoordbeleidsregels en beperkingen in Azure Active Directory

In dit artikel worden de wachtwoordbeleidsregels en de complexiteitsvereisten die zijn gekoppeld aan gebruikersaccounts die zijn opgeslagen in uw adreslijst Azure AD.

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName beleidsregels voor alle gebruikersaccounts

Elke gebruikersaccount die vereist zijn voor het aanmelden bij het systeem Azure AD-verificatie moet een unieke gebruiker UPN (User Principal Name)-kenmerkwaarde die is gekoppeld aan dat account. De volgende tabel overzichten het beleid die van toepassing op beide on-premises implementatie die afkomstig zijn Active Directory-gebruikersaccounts (gesynchroniseerd met de cloud) en alleen de cloud gebruikersaccounts.

|   Eigenschap           |     UserPrincipalName vereisten  |
|   ----------------------- |   ----------------------- |
|  Tekens die zijn toegestaan    |  <ul> <li>A-Z</li> <li>een z- </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Tekens die zijn niet toegestaan  | <ul> <li>Een '@' teken dat is de gebruikersnaam van het domein niet scheiden.</li> <li>Mag een periode teken '.' vóór de '@' symbool</li></ul> |
| Lengte beperkingen  |       <ul> <li>Totale lengte mag maximaal 113 tekens bevatten</li><li>64 tekens vóór de ‘@’ symbool</li><li>48 tekens na de ‘@’ symbool</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Wachtwoordbeleidsregels die alleen voor gebruikersaccounts cloud gelden

De volgende tabel worden de beschikbare wachtwoordbeleidsinstellingen die kunnen worden toegepast op gebruikersaccounts die zijn gemaakt en beheerd in Azure AD.

|  Eigenschap       |    Vereisten          |
|   ----------------------- |   ----------------------- |
|  Tekens die zijn toegestaan   |   <ul><li>A-Z</li><li>een z- </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + [] {} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Tekens die zijn niet toegestaan   |       <ul><li>Unicode-tekens</li><li>Spaties</li><li> **Alleen sterke wachtwoorden**: mogen geen bevatten een teken stip '.' vóór de '@' symbool</li></ul> |
|   Wachtwoordbeperkingen | <ul><li>8 tekens minimum en maximum van 16 tekens</li><li>**Alleen sterke wachtwoorden**: 3 vereist afmelden bij 4 van de volgende handelingen uit:<ul><li>Kleine letters</li><li>Hoofdletters</li><li>Cijfers (0-9)</li><li>Symbolen (Zie wachtwoordbeperkingen hierboven)</li></ul></li></ul> |
| Wachtwoord verstrijken duur      | <ul><li>Standaardwaarde: **90** dagen </li><li>Waarde is geconfigureerd met de cmdlet Set-MsolPasswordPolicy van de Azure Active Directory-Module voor Windows PowerShell.</li></ul> |
| Wachtwoord verstrijken melding |  <ul><li>Standaardwaarde: **14** dagen (voordat wachtwoord verloopt)</li><li>Waarde is geconfigureerd met de cmdlet Set-MsolPasswordPolicy.</li></ul> |
| Wachtwoord verloopt |  <ul><li>Standaardwaarde: **Onwaar** dagen (geeft aan dat wachtwoord verloopt en is ingeschakeld) </li><li>Waarde kan worden geconfigureerd voor afzonderlijke gebruikersaccounts met de cmdlet Set-MsolUser. </li></ul> |
|  Wachtwoordgeschiedenis  | Laatste wachtwoord worden niet opnieuw gebruikt. |
|  Wachtwoord geschiedenis duur | Eeuwen |
|  Accountvergrendeling | Na 10 aanmeldingsproblemen pogingen (verkeerde wachtwoord), wordt de gebruiker gedurende een minuut worden vergrendeld. Verder wordt onjuiste aanmeldingsproblemen pogingen vergrendeld de gebruiker voor duur te vergroten. |


## <a name="next-steps"></a>Volgende stappen

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [Wachtwoorden beheren vanaf elke locatie](active-directory-passwords.md)
* [De werking van wachtwoordbeheer](active-directory-passwords-how-it-works.md)
* [Aan de slag met wachtwoord Mangement](active-directory-passwords-getting-started.md)
* [Wachtwoordbeheer aanpassen](active-directory-passwords-customize.md)
* [Aanbevolen werkwijzen voor wachtwoorden beheren](active-directory-passwords-best-practices.md)
* [Hoe kom ik aan operationele inzichten die met wachtwoord Management-rapporten](active-directory-passwords-get-insights.md)
* [Wachtwoordbeheer Veelgestelde vragen](active-directory-passwords-faq.md)
* [Problemen met wachtwoordbeheer](active-directory-passwords-troubleshoot.md)
* [Meer informatie](active-directory-passwords-learn-more.md)
