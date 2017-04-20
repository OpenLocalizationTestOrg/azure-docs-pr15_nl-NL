<properties
    pageTitle="Eenmalige aanmelding met toepassingsproxy | Microsoft Azure"
    description="Wordt uitgelegd hoe u eenmalige aanmelding met Azure AD-toepassingsproxy bieden."
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Eenmalige aanmelding met toepassingsproxy

Eenmalige aanmelding is een belangrijk element van Azure AD-toepassingsproxy. De beste gebruikerservaring biedt de volgende stappen uit:

1. Een gebruiker zich aanmeldt bij de cloud  
2. Alle beveiliging validatie optreden in de cloud (vooraf-verificatie)  
3. Wanneer het verzoek wordt verzonden naar de on-premises-toepassing, vertegenwoordigt de toepassing Proxy verbindingslijn de gebruiker. De backend-toepassing denkt dat dit is een gewone gebruiker die afkomstig zijn uit een apparaat domein behoren.

![Access-diagram van de eindgebruiker, bij toepassingsproxy, met het bedrijfsnetwerk](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Azure AD-toepassingsproxy kunt u een eenmalige aanmelding (SSO)-ervaring te bieden voor uw gebruikers. Gebruik de volgende instructies voor het publiceren van uw apps Eenmalige aanmelding van:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>Eenmalige aanmelding voor on-premises geïntegreerde Windows-apps met behulp van KCD met toepassingsproxy
Voor uw toepassingen met geïntegreerde Windows-verificatie door middel van een toepassing Proxy verbindingslijnen machtiging in Active Directory uitgeven voor gebruikers, en verzenden en ontvangen van tokens namens hen kunt u eenmalige aanmelding inschakelen.


### <a name="network-diagram"></a>Netwerkdiagram

In dit diagram wordt de stroom uitgelegd wanneer een gebruiker probeert voor toegang tot een on-premises-toepassing die gebruikmaakt van uitgebreid.

![Microsoft AAD verificatie gegevensstroom-diagram](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. De gebruiker voert de URL voor toegang tot de on-premises-toepassing via toepassingsproxy.
2. Toepassingsproxy leidt het verzoek om naar Azure AD-verificatieservices om te worden. Azure AD van toepassing op dit moment voor elke toepassing verificatie en autorisatie beleidsregels, zoals meervoudige verificatie. Als de gebruiker is gevalideerd, wordt Azure AD Hiermee maakt u een token en verzonden naar de gebruiker.
3. De gebruiker wordt het token aan toepassingsproxy doorgegeven.
4. Toepassingsproxy is gevalideerd met het token haalt de UPN (User Principal Name) van dit en stuurt de aanvraag, de UPN en de Service Principal naam (Name) naar de verbindingslijn via een zowel geverifieerde beveiligd kanaal.
5. De verbindingslijn uitvoert Kerberos beperkt delegeren (KCD)-onderhandelingen met de on-premises AD, zich voordoen als de gebruiker als u een Kerberos-token met de toepassing.
6. Active Directory verzendt de Kerberos-token voor de toepassing op de verbindingslijn.
7. De verbindingslijn wordt de oorspronkelijke aanvraag verzonden naar de toepassingsserver, met de Kerberos-token die het ontvangen van AD.
8. De toepassing stuurt de reactie naar de verbindingslijn die vervolgens wordt geretourneerd aan de service toepassingsproxy en ten slotte aan de gebruiker.

### <a name="prerequisites"></a>Vereisten voor

Controleer voordat u aan de slag met eenmalige aanmelding voor de Proxy-toepassing, of dat uw omgeving is klaar met de volgende instellingen en configuraties:

- Uw apps, zoals SharePoint-Web-apps, zijn ingesteld op geïntegreerde Windows-verificatie gebruiken. Zie [Ondersteuning voor Kerberos-verificatie inschakelen](https://technet.microsoft.com/library/dd759186.aspx)voor meer informatie of Zie [voor Kerberos-verificatie in SharePoint 2013 plannen](https://technet.microsoft.com/library/ee806870.aspx)voor SharePoint.

- Al uw apps hebben Service Principal namen.

- De server de verbindingslijn en de server met de app zijn domein toegevoegd en een gedeelte van hetzelfde domein of vertrouwde domeinen. Zie voor meer informatie over domein join, [deelnemen aan een Computer met een domein](https://technet.microsoft.com/library/dd807102.aspx).

- De server met de verbindingslijn heeft toegang tot de TokenGroupsGlobalAndUniversal lezen voor gebruikers. Dit is een standaardinstelling die mogelijk is beïnvloed door beveiliging versterking van de omgeving. Meer hulp met dit in [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Active Directory-configuratie

De Active Directory-configuratie varieert, afhankelijk van of de verbindingslijn van de Proxy-toepassing en de gepubliceerde server zich in hetzelfde domein of niet bevinden.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Verbindingslijn en gepubliceerde server in hetzelfde domein

1. In Active Directory, gaat u naar **Extra** > **gebruikers en Computers**.
2. Selecteer de server met de verbindingslijn.
3. Klik met de rechtermuisknop en selecteer **Eigenschappen** > **delegeren**.
4. Selecteer **deze computer mag overdragen aan de opgegeven services alleen** en klik onder **Services waaraan dit account kan referenties overdragen**toevoegen de waarde voor de identiteit name van de toepassingsserver.
5. Hiermee worden de verbindingslijn van de Proxy-toepassing met uitgeven voor gebruikers in AD ten opzichte van de toepassingen die is gedefinieerd in de lijst.

![Eigenschappen van een verbindingslijn SVR venster schermafbeelding](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Verbindingslijn en gepubliceerde server in verschillende domeinen

1. Zie voor een lijst met vereisten voor het werken met KCD voor domeinen, [Kerberos beperkt delegeren tussen domeinen](https://technet.microsoft.com/library/hh831477.aspx).
2. In Windows 2012 R2, gebruikt u de `principalsallowedtodelegateto` eigenschap op de server verbindingslijn om in te schakelen van de toepassingsproxy aan gemachtigde met voor de server verbindingslijn, waarbij de gepubliceerde server is `sharepointserviceaccount` en de delegerende server is `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`is de computeraccount SP's of een serviceaccount waarmee de toepassingen SP's wordt uitgevoerd.


### <a name="azure-classic-portal-configuration"></a>Configuratie van Azure klassieke portal

1. Uw toepassing volgens de instructies die worden beschreven in [de toepassingen publiceren met toepassingsproxy](active-directory-application-proxy-publish.md)publiceren. Zorg ervoor dat **Azure Active Directory** als de **Vooraf-verificatie-methode**.
2. Nadat uw toepassing in de lijst met toepassingen wordt weergegeven, selecteert u deze en klik op **configureren**.
3. Stel onder **Eigenschappen**, **Interne verificatiemethode** **geïntegreerde Windows**-verificatie.  
  ![Geavanceerde configuratie](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Voer de **Interne toepassing name** van de toepassingsserver. In dit voorbeeld is de name voor onze gepubliceerde toepassing http/lob.contoso.com.  

>[AZURE.IMPORTANT] Als uw UPN on-premises implementatie en de UPN in Azure Active Directory niet identiek zijn, moet u voor het configureren van de [gedelegeerde login identiteit](#delegated-login-identity) om vooraf-verificatie om te werken.

|  |  |
| --- | --- |
| Interne verificatiemethode | Als u Azure AD voor vooraf-verificatie gebruikt, kunt u instellen dat een interne verificatiemethode zodat uw gebruikers kunnen profiteren van eenmalige aanmelding (SSO) naar deze toepassing. <br><br> Selecteer **Geïntegreerde Windows-verificatie** (uitgebreid) als uw toepassing gebruikmaakt van uitgebreid en kunt u Kerberos beperkt delegeren (KCD) om in te schakelen van eenmalige aanmelding van deze toepassing. Toepassingen die gebruikmaken van uitgebreid moeten worden geconfigureerd met KCD, anders toepassingsproxy is niet mogelijk te publiceren van deze toepassingen. <br><br> Selecteer **geen** als uw toepassing geen uitgebreid gebruikt. |
| Interne toepassing name | Dit is de Service-Principal-naam (Name) van de interne toepassing zoals geconfigureerd in de on-premises Azure AD. De name wordt gebruikt door de toepassing Proxy verbindingslijn om op te halen Kerberos-tokens voor de toepassing die KCD. |


## <a name="sso-for-non-windows-apps"></a>Eenmalige aanmelding voor niet-Windows-apps
De stroom van de overdracht Kerberos in Azure AD-toepassingsproxy gestart wanneer Azure AD wordt geverifieerd door de gebruiker in de cloud. Zodra het verzoek on-premises implementatie binnenkomt, wordt in de Azure AD-toepassing Proxy verbindingslijn een Kerberos-tickets namens de gebruiker oplossen door de interactie met de lokale Active Directory. Dit proces wordt aangeduid als Kerberos beperkt delegeren (KCD). In de volgende fase, een aanvraag verzonden naar de backend-toepassing met deze Kerberos-tickets. Er zijn een aantal protocollen die hoe bepalen u deze verzoeken verzendt. De meeste niet-Windows-servers verwachten Negotiate/SPNego dat nu wordt ondersteund door Azure AD-toepassingsproxy.

![Niet-Windows SSO-diagram](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Gedelegeerd login identiteit
Gedelegeerd login identiteit kunt u twee verschillende login-scenario's afhandelen:

- Niet-Windows-toepassingen die meestal aan gebruikers-id in de vorm van een gebruikersnaam of SAM-accountnaam, niet in een e-mailadres (username@domain).
- Alternatieve login configuraties waar de UPN in Azure AD en de UPN in uw lokale Active Directory verschillen.

Met toepassingsproxy, kunt u kiezen welke identiteit u met de Kerberos-tickets verkrijgt. Deze instelling is per toepassing. Sommige van deze opties zijn geschikt voor systemen die e-mailadres opmaak niet accepteren, anderen zijn ontworpen voor alternatieve login.

![Schermafbeelding van gedelegeerde login identiteit parameter](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Als gedelegeerd login identiteit wordt gebruikt, de waarde mogelijk niet uniek zijn voor alle domeinen of forests in uw organisatie. U kunt dit probleem voorkomen door deze toepassingen twee keer met twee verschillende verbindingslijn groepen publiceren. Aangezien elke toepassing een andere gebruiker publiek heeft, kunt u de verbindingslijnen kunt deelnemen aan een ander domein.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Werken met eenmalige aanmelding bij het on-premises en cloud identiteiten niet identiek zijn
Tenzij anderszins is geconfigureerd, toepassingsproxy wordt ervan uitgegaan dat gebruikers precies dezelfde identiteit in de cloud en on-premises hebben. U kunt configureren, voor elke toepassing, welke identiteit moet worden gebruikt bij het uitvoeren van eenmalige aanmelding.  

Deze functie kunt veel organisaties die verschillende on-premises en cloud identiteiten hebben om eenmalige aanmelding vanuit de cloud te on-premises apps moeten zonder de gebruikers om in te voeren verschillende gebruikersnamen en wachtwoorden opnieuw instellen. Dit geldt ook voor organisaties die:

- Intern meerdere domeinen heb (joe@us.contoso.com, joe@eu.contoso.com) en één domein in de cloud (joe@contoso.com).

- Niet-omgeleid domeinnaam intern hebt (joe@contoso.usa) en juridische één in de cloud.

- Gebruik domeinnamen niet op intern (Jan)

- Gebruik van verschillende aliassen on-premises en in de cloud. Bijvoorbeeld joe-johns@contoso.comVS.joej@contoso.com  

Ook kunt met toepassingen die adressen in de vorm van een e-mailadres, dat wil een veelvoorkomende scenario voor niet-Windows backend-servers zeggen niet accepteren.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Eenmalige aanmelding instellen voor verschillende cloud en on-premises identiteiten

1. Azure AD Connect-instellingen configureren zodat de belangrijkste identiteit het e-mailadres (e). Dit gebeurt als onderdeel van het proces aanpassen door te wijzigen van het veld **Gebruikersnaam principe** in de synchronisatie-instellingen. Opmerking dat deze instellingen bepalen ook hoe gebruikers Meld u aan bij Office365, Windows10 apparaten, en andere toepassingen die gebruikmaken van Azure AD als hun identiteit store.  
  ![Schermafbeelding van de gebruikers - User Principal Name vervolgkeuzelijst identificeren](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Selecteer in de toepassingsconfiguratie-instellingen voor de toepassing die u wilt wijzigen, de **Gedelegeerde Login identiteit** moet worden gebruikt:
  - Principe gebruikersnaam:joe@contoso.com  
  - Alternatieve principe gebruikersnaam:joed@contoso.local  
  - Deel van de gebruikersnaam van principe gebruikersnaam: Jan  
  - Deel van de gebruikersnaam van alternatieve principe gebruikersnaam: joed  
  - On-premises implementatie SAM-accountnaam: afhankelijk on-premises domeinconfiguratie

  ![Gedelegeerd login identiteit vervolgmenu schermafbeelding](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Eenmalige aanmelding voor probleemoplossing voor verschillende identiteiten
Als er een fout in het proces voor eenmalige aanmelding van wordt deze weergegeven in het gebeurtenissenlogboek van verbindingslijn machine zoals wordt uitgelegd in [Probleemoplossing](active-directory-application-proxy-troubleshoot.md).
Maar in sommige gevallen, de aanvraag is verzonden naar de backend-toepassing terwijl deze toepassing wordt beantwoorden in diverse andere HTTP-antwoorden. Problemen oplossen die gevallen moet beginnen door te bekijken van de gebeurtenis getal 24029 op de computer van de verbindingslijn in het gebeurtenissenlogboek van toepassingsproxy sessie. De identiteit van de gebruiker die is gebruikt voor overdracht wordt weergegeven in het veld "gebruiker" binnen de details van een gebeurtenis. Als u op log sessie, selecteert u **Logboeken en foutopsporing analytische weergeven** in het geval viewer menu Beeld.


## <a name="see-also"></a>Zie ook

- [Toepassingen met toepassingsproxy publiceren](active-directory-application-proxy-publish.md)
- [Problemen die u met toepassingsproxy ondervindt](active-directory-application-proxy-troubleshoot.md)
- [Werken met op claims-toepassingen](active-directory-application-proxy-claims-aware-apps.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
