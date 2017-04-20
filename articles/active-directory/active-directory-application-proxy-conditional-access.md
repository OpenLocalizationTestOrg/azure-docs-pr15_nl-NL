<properties
    pageTitle="Voorwaardelijke toegang voor toepassingen die zijn gepubliceerd met Azure AD-toepassingsproxy"
    description="Wordt uitgelegd hoe u voorwaardelijke toegang voor toepassingen die u publiceren om te worden geopend op afstand met Azure AD-toepassingsproxy instellen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Werken met voorwaardelijke toegang

U kunt toegangsregels voor voorwaardelijke toegang verlenen tot toepassingen die zijn gepubliceerd met toepassingsproxy configureren. Hiermee kunt u:

- Meervoudige verificatie per toepassing vereisen
- Meervoudige verificatie vereisen alleen als gebruikers zich niet in de praktijk
- Gebruikers toegang krijgen tot de toepassing wanneer ze niet in de praktijk zijn blokkeren

Deze regels kunnen worden toegepast op alle gebruikers en groepen of alleen aan specifieke gebruikers en groepen. Standaard wordt de regel toepassen op alle gebruikers die toegang tot de toepassing hebben. De regel kunt echter ook worden beperkt tot gebruikers die lid van bepaalde beveiligingsgroepen zijn.  

Access regels worden geëvalueerd wanneer een gebruiker een federatieve toepassing die gebruikmaakt van OAuth 2.0, OpenID verbinden, SAML of WS-Federation opent. Bovendien worden access regels worden geëvalueerd met OAuth 2.0 en verbindt u OpenID wanneer een token vernieuwen wordt gebruikt voor het ophalen van een toegangstoken.

## <a name="conditional-access-prerequisites"></a>Voorwaardelijke toegang vereisten

- Abonnement voor Premium Azure Active Directory
- Een federatieve of beheerde Azure Active Directory-tenant
- Federatieve tenants vereisen dat meervoudige verificatie (MFA) worden ingeschakeld  
    ![Configureren toegangsregels - meervoudige verificatie vereisen](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Per toepassing meervoudige verificatie configureren
1. Meld u aan als beheerder bij de portal van Azure klassieke.
2. Ga naar Active Directory en selecteer de map waarin u wilt inschakelen toepassingsproxy.
3. Klik op **toepassingen** en schuif omlaag naar de sectie **Toegangsregels** . Het gedeelte van de regels access wordt alleen weergegeven voor toepassingen die zijn gepubliceerd met toepassingsproxy die federatieve verificatie gebruikt.
4. Schakel de regel door te **Toegangsregels inschakelen** voor **op**selecteren.
5. Geef de gebruikers en groepen aan wie de regels van toepassing. Gebruik de knop **Groep toevoegen** om te selecteren van een of meer groepen waaraan de access-regel van toepassing. Dit dialoogvenster kan ook worden gebruikt voor het geselecteerde groepen verwijderen.  Wanneer de regels toepassen op groepen zijn geselecteerd, worden de toegangsregels alleen voor gebruikers die deel uitmaakt van een van de opgegeven beveiligingsgroepen afgedwongen.  

  - Als u wilt uitsluiten expliciet beveiligingsgroepen van de regel, **behalve** controleren en geef een of meer groepen. Gebruikers die deel van een groep in de lijst behalve uitmaken niet moet uitvoeren meervoudige verificatie.  

  - Als een gebruiker is geconfigureerd met de functie van de meervoudige verificatie per gebruiker, hebben deze instelling voorrang op de regels van toepassing meervoudige verificatie. Dit betekent dat een gebruiker die is geconfigureerd voor per gebruiker meervoudige verificatie uitgevoerd om uit te voeren meervoudige verificatie moeten worden, zelfs als ze zijn uitgesloten voor meervoudige verificatie-regels van de toepassing. Meer informatie over [meervoudige verificatie en instellingen per gebruiker](../multi-factor-authentication/multi-factor-authentication.md).

6. Selecteer de access-regel die u wilt instellen:
    - **Vereisen meervoudige verificatie**: gebruikers aan wie toegangsregels zijn van toepassing moeten worden uitgevoerd naar volledige meervoudige verificatie voordat u de toepassing waarop de regel van toepassing is.
    - **Vereisen meervoudige verificatie wanneer u niet op werk**: gebruikers bij het openen van de toepassing van een vertrouwde IP-adres niet moeten om uit te voeren meervoudige verificatie. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina voor meervoudige verificatie-instellingen.
    - **Toegang als niet aan het werk blokkeren**: gebruikers bij het openen van de toepassing van buiten uw bedrijfsnetwerk hebben niet steeds toegang tot de toepassing.


## <a name="configuring-mfa-for-federation-services"></a>MFA voor federation services configureren
Voor federatieve tenants, meervoudige verificatie (MFA) kan worden uitgevoerd door Azure Active Directory of door het lokale AD FS-server. Standaard gebeurt MFA op een willekeurige pagina die worden gehost door Azure Active Directory. Voordat u configureren MFA on-premises implementatie, Windows PowerShell uitvoeren en het gebruik van de eigenschap – SupportsMFA voor het instellen van de Azure AD-module.

Het volgende voorbeeld ziet u hoe on-premises implementatie MFA inschakelen met behulp van de [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) op de contoso.com tenant:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Naast het instellen van deze vlag, moet het federatieve tenant AD FS-exemplaar worden geconfigureerd meervoudige verificatie uitvoeren. Volg de instructies voor het [implementeren van Microsoft Azure meervoudige verificatie on-premises implementatie](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).


## <a name="see-also"></a>Zie ook

- [Werken met op claims-toepassingen](active-directory-application-proxy-claims-aware-apps.md)
- [Toepassingen met toepassingsproxy publiceren](active-directory-application-proxy-publish.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)
- [Toepassingen met uw eigen domeinnaam publiceren](active-directory-application-proxy-custom-domains.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
