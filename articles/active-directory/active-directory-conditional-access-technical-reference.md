
<properties
    pageTitle="Azure Active Directory voorwaardelijke toegang technische verwijzing | Microsoft Azure"
    description="Met voorwaardelijke toegangsbeheer controles Azure Active Directory de specifieke voorwaarden die u bij het verifiëren van de gebruiker en vóór het toestaan van toegang tot de toepassing kiezen. Zodra deze voorwaarden is voldaan, wordt de gebruiker is geverifieerd, en toegang hebben tot de toepassing."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory voorwaardelijke toegang technische verwijzing

## <a name="services-enabled-with-conditional-access"></a>Ingeschakeld met voorwaardelijke access Services
Regels voor voorwaardelijke toegang worden ondersteund tussen verschillende typen van Azure AD-toepassing. Deze lijst bevat:

- Federatieve toepassingen uit de galerie met Azure AD-toepassing
- Wachtwoord SSO-toepassingen uit de galerie met Azure AD-toepassing
- Toepassingen die zijn geregistreerd bij de toepassingsproxy van Azure
- Ontwikkeld tekstregel bedrijven en meerdere tenant-toepassingen die zijn geregistreerd bij Azure AD
- Online Visual Studio
- Azure Remote-App
-   Dynamics CRM
- Microsoft Office 365 Yammer
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (inclusief OneDrive voor bedrijven)


## <a name="enable-access-rules"></a>Toegangsregels inschakelen

Elke regel kan worden ingeschakeld of uitgeschakeld op een per grondtal van de toepassing. Wanneer de regels zijn **ON** worden ze ingeschakeld en afgedwongen voor gebruikers met toegang tot de toepassing. Wanneer ze **uit** zijn ze niet worden gebruikt en is niet van invloed op de gebruikers aanmelden ervaring.

## <a name="applying-rules-to-specific-users"></a>Regels toepassen op specifieke gebruikers
Regels kunnen worden toegepast op specifieke sets met gebruikers op basis van de beveiligingsgroep door in te stellen **Toepassen op**. **Toepassen op** kan worden ingesteld op **Alle gebruikers** of **groepen**. Als de waarde voor **Alle gebruikers** de regels wordt toegepast op alle gebruikers met toegang tot de toepassing. De optie **groepen** kunt specifieke beveiligings- en distributiegroepen mogen worden geselecteerd, regels alleen voor deze groepen worden afgedwongen.

Wanneer u een regel implementeert, wordt meestal deze een beperkt aantal gebruikers, die deel uitmaken van een groepen piloting eerst hebt toegepast. Als voltooid kan de regel worden toegepast op **Alle gebruikers**. Hierdoor wordt de regel worden afgedwongen voor alle gebruikers in de organisatie.

Selecteer groepen kunnen ook worden vrijgesteld van beleid met de optie **behalve** . Alle leden van deze groepen worden vrijgesteld, zelfs als ze worden weergegeven in een opgenomen groep.

## <a name="at-work-networks"></a>"Aan het werk" netwerken


Regels voor voorwaardelijke access met een netwerk 'op het werk", is afhankelijk van vertrouwde IP-adresbereiken die zijn geconfigureerd in Azure AD, of gebruik van de claim"binnen corpnet"van AD FS. Deze regels zijn:

- Meervoudige verificatie wanneer niet op het werk vereisen
- Toegang geven als niet aan het werk

Opties voor LDAP 'op het werk"netwerken

1. Vertrouwde IP-adresbereiken configureren op de [pagina voor meervoudige verificatie-configuratie](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Voorwaardelijke-beleid gebruikt de geconfigureerde bereiken op elke verificatieaanvraag en token regels wordt berekend. 
2. Gebruik van de binnenkant configureren corpnet claimen, deze optie kan worden gebruikt met federatieve mappen, AD FS gebruikt. [Meer informatie over de binnenkant coronet claims](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Openbare IP-adresbereiken configureren. Klik op het tabblad configureren kunt voor uw adreslijst, u openbare IP-adressen instellen. Voorwaardelijke toegang Gebruik deze zoals 'in de praktijk' IP-adressen, hierdoor aanvullende bereiken te configureren, boven de 50 IP-adres dat wordt afgedwongen door de pagina van de instelling MFA.



## <a name="rules-based-on-application-sensitivity"></a>Regels op basis van de toepassing gevoeligheid

Regels zijn geconfigureerd per toepassing zodat de services hoge waarde moet worden beveiligd zonder die invloed hebben op toegang tot andere services. Regels voor voorwaardelijke toegang kunnen worden geconfigureerd op het tabblad **configureren** van de toepassing. 

Regels die momenteel wordt aangeboden:

- **Meervoudige verificatie vereisen**
 - Alle gebruikers die dit beleid wordt toegepast op moeten worden uitgevoerd om te verifiëren via ten minste eenmaal meervoudige verificatie.
 
- **Meervoudige verificatie wanneer niet op het werk vereisen**
 - Als dit beleid wordt toegepast, moeten alle gebruikers worden uitgevoerd moet ten minste eenmaal meervoudige verificatie hebt uitgevoerd als ze toegang hebt tot de service vanaf een niet-werk externe locatie. Als ze naar een werk naar externe locatie verplaatsen, ze moet meervoudige verificatie uitvoeren tijdens het werken met de service.
 
- **Toegang geven als niet aan het werk** 
 - Wanneer gebruikers naar werk op een externe locatie verplaatsen, wordt deze geblokkeerd als het beleid "Toegang geven als niet aan het werk" is toegepast.  Ze worden opnieuw kunnen access vanuit een werklocatie.


## <a name="related-topics"></a>Verwante onderwerpen

- [Toegang tot Office 365 en andere apps beveiligen verbonden met Azure Active Directory](active-directory-conditional-access.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
