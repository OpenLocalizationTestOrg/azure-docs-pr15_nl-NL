<properties
    pageTitle="Azure voorwaardelijke toegang voor SaaS Apps | Microsoft Azure"
    description="Voorwaardelijke toegang in Azure AD kunt u toegangsregels per toepassing meervoudige verificatie en de mogelijkheid om te blokkeren toegang voor gebruikers niet op een vertrouwde netwerk configureren. "
    services="active-directory"
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
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Aan de slag met Azure Active Directory voorwaardelijke toegang

Azure Active Directory voorwaardelijke toegang voor [SaaS](https://azure.microsoft.com/overview/what-is-saas/) -apps en Azure AD verbonden apps kunt die u voorwaardelijke toegang op basis van het deelvenster Groeperen, locatie en toepassing gevoeligheid configureren. 

Met voorwaardelijke toegang op basis van de toepassing gevoeligheid, kunt u de toegangsregels meervoudige verificatie (MFA) per toepassing instellen. MFA per toepassing biedt de mogelijkheid toegang voor gebruikers die zich niet in een netwerk met een vertrouwde blokkeren. U kunt MFA regels toepassen op alle gebruikers die zijn toegewezen aan de toepassing, of alleen voor gebruikers binnen de opgegeven beveiligingsgroepen.  Gebruikers kunnen worden uitgesloten uit de vereiste MFA als ze toegang krijgt tot de toepassing vanaf een IP-adres dat is binnen het bedrijfsnetwerk.

Deze mogelijkheden zijn beschikbaar voor klanten die een licentie Azure Active Directory Premium hebt gekocht.

## <a name="scenario-prerequisites"></a>Scenario vereisten
* De licentie voor Premium Azure Active Directory

* Federatieve of beheerde Azure Active Directory-tenant

* Federatieve tenants werken alleen die meervoudige verificatie is ingeschakeld.

## <a name="configure-per-application-access-rules"></a>Per toepassing toegangsregels configureren

In deze sectie wordt beschreven hoe per toepassing toegangsregels configureren.

1. Meld u aan bij de Azure klassieke portal met behulp van een account dat is een globale beheerder voor Azure AD.
2. Selecteer in het linkerdeelvenster, **Active Directory**.
3. Selecteer het telefoonboek van uw op het tabblad map.
4. Selecteer het tabblad **toepassingen** .
5. Selecteer de toepassing die de regel wordt ingesteld voor.
6. Selecteer het tabblad **configureren** .
7. Schuif omlaag naar de sectie toegang regels. Selecteer de gewenste access-regel.
8. Geef de gebruikers op die de regel van toepassing.
9. Het beleid door te selecteren **ingeschakeld moet zijn op**inschakelen.

##<a name="understanding-access-rules"></a>Lidmaatschap toegangsregels

In deze sectie wordt een gedetailleerde beschrijving van de access-regels die worden ondersteund in de toepassing van Azure voorwaardelijke toegang.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Opgeven van de gebruikers toepassen de access-regels op

Het beleid wordt standaard toepassen op alle gebruikers die toegang tot de toepassing hebben. U kunt echter ook het beleid voor gebruikers die lid van de opgegeven beveiligingsgroepen zijn beperken. De knop **Groep toevoegen** wordt gebruikt om een of meer groepen van het dialoogvenster in de groep selecteren die de access-regel wordt toegepast op te selecteren. Dit dialoogvenster kan ook worden gebruikt voor het geselecteerde groepen verwijderen. Wanneer de regels toepassen op groepen zijn geselecteerd, worden de toegangsregels alleen voor gebruikers die deel uitmaakt van een van de opgegeven beveiligingsgroepen afgedwongen.

Beveiligingsgroepen kunnen ook worden expliciet uitgesloten van het beleid door de optie **behalve** te selecteren en een of meer groepen te geven. Gebruikers die lid zijn van een groep in de lijst **, behalve** worden niet onderhevig aan de vereiste meervoudige verificatie, zelfs als ze lid zijn van een groep waarvoor u de access-regel van toepassing op.
De access-regel hieronder moeten worden alle gebruikers in de groep Managers meervoudige verificatie gebruiken bij het openen van de toepassing.

![Regels voor voorwaardelijke toegang met MFA instellen](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Regels voor voorwaardelijke toegang met MFA
Als een gebruiker is geconfigureerd met de functie van de meervoudige verificatie per gebruiker, wordt deze instelling voor de gebruiker van de regels meervoudige verificatie van de app combineren. Dit betekent dat een gebruiker die is geconfigureerd voor per gebruiker meervoudige verificatie moet meervoudige verificatie uitvoeren, zelfs als ze de meervoudige verificatie-regels van toepassing zijn uitgesloten. Meer informatie over instellingen voor meervoudige verificatie en per gebruiker.

### <a name="access-rule-options"></a>Opties voor Access-regel
De volgende opties worden ondersteund:

* **Meervoudige verificatie vereisen**: de gebruikers aan wie de access-regels toepassen op, moeten worden uitgevoerd naar volledige meervoudige verificatie voordat u de toepassing die het beleid is van toepassing op.

* **Meervoudige verificatie wanneer niet op het werk vereisen**: een gebruiker die afkomstig zijn uit een vertrouwde IP-adres niet moet uitvoeren meervoudige verificatie. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina voor meervoudige verificatie-instellingen.

* **Toegang als niet aan het werk blokkeren**: een gebruiker die niet afkomstig zijn uit een vertrouwde IP-adres worden geblokkeerd. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina voor meervoudige verificatie-instellingen.

### <a name="setting-rule-status"></a>Regelstatus instellen
Statusbalk van Access-regel kunt schakelen van de regels in- of uitschakelen. Wanneer de toegangsregels uitgeschakeld zijn, wordt de vereiste meervoudige verificatie niet afgedwongen.

### <a name="access-rule-evaluation"></a>Evaluatie van regels voor Access

Access regels worden geëvalueerd wanneer een gebruiker een federatieve toepassing die gebruikmaakt van OAuth 2.0, OpenID verbinden, SAML of WS-Federation opent. Daarnaast wordt toegangsregels voor worden geëvalueerd als het OAuth 2.0 en OpenID verbinding maken met een token vernieuwen in het bezit van een toegangstoken. Als evaluatie van beleid mislukt wanneer een token vernieuwen wordt gebruikt, wordt de fout **invalid_grant** wordt geretourneerd, dit betekent dat de gebruiker moet opnieuw worden geverifieerd naar de klant.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Federation services voor de meervoudige verificatie configureren

Voor federatieve tenants, MFA kan worden uitgevoerd door Azure Active Directory of door het lokale AD FS-server.

Standaard plaatsvindt MFA aan een pagina die worden gehost door Azure Active Directory. Als u wilt configureren MFA on-premises implementatie, moet de eigenschap **– SupportsMFA** worden ingesteld op **waar** in Azure Active Directory, met behulp van de Azure AD-module voor Windows PowerShell.

Het volgende voorbeeld ziet u hoe on-premises implementatie MFA inschakelen met behulp van de [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) op de contoso.com tenant:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Naast het instellen van deze vlag, moet het federatieve tenant AD FS-exemplaar worden geconfigureerd meervoudige verificatie uitvoeren. Volg de instructies voor de [implementatie van Azure meervoudige verificatie on-premises implementatie](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Verwante artikelen

- [Toegang tot Office 365 en andere apps beveiligen verbonden met Azure Active Directory](active-directory-conditional-access.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
