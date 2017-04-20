<properties
    pageTitle="Problemen met toepassingsproxy | Microsoft Azure"
    description="Wordt uitgelegd hoe u fouten corrigeren in Azure AD-toepassingsproxy."
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
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Problemen met toepassingsproxy

Als er fouten optreden bij het openen van een toepassing voor gepubliceerde of in de publicatie-toepassingen, controleert u de volgende opties om te zien als Microsoft Azure AD-toepassingsproxy goed werkt:

- Open de Windows-Services-console en controleer of de service **Microsoft AAD toepassing Proxy Connector** is ingeschakeld en uitgevoerd. U kunt ook kijkt u naar de pagina toepassingsproxy service eigenschappen, zoals wordt weergegeven in de volgende afbeelding:  
  ![Schermafbeelding van Microsoft AAD-toepassing Proxy Connector Properties-venster](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Open Logboeken en zoek naar toepassingsproxy verbindingslijn gebeurtenissen in **Logboeken toepassingen en Services** > **Microsoft** > **AadApplicationProxy** > **verbindingslijn** > **beheerder**.
- Indien nodig, is meer gedetailleerde logboeken zijn beschikbaar door in te schakelen analytics en voor foutopsporing in Logboeken en het inschakelen van het logboek toepassingsproxy verbindingslijn sessie.

## <a name="the-page-is-not-rendered-correctly"></a>De pagina is niet correct weergegeven

Als u niet een specifiek foutbericht krijgt, kan u nog steeds problemen hebt met uw toepassing weergeven of onjuist functioneren. Dit kan gebeuren als u het artikel pad gepubliceerd, maar de toepassing inhoud die zich buiten dat pad vereist.

Bijvoorbeeld als u het pad https://yourapp/app publiceren, maar de toepassing afbeeldingen in https://yourapp/media u belt, won't ze worden weergegeven. Zorg ervoor dat u de toepassing met het hoogste niveau pad, moet u alle relevante inhoud bevatten publiceren. In dit voorbeeld is het http://yourapp/ worden.

Als u uw pad als u wilt opnemen waarnaar wordt verwezen inhoud, maar nog steeds nodig gebruikers op de koppeling naar een grondigere in het pad wijzigt, raadpleegt u het blogbericht [de juiste koppeling voor toepassingsproxy toepassingen in het deelvenster van Azure AD-toegang en het startprogramma voor Office 365 instellen](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Algemene fouten

Fout | Beschrijving | Resolutie
--- | --- | ---
Deze zakelijke app kan niet worden gebruikt. U bent niet gemachtigd voor toegang tot deze toepassing. Verificatie mislukt. Zorg ervoor dat de gebruiker met access toewijzen aan deze toepassing. | Mogelijk hebt u de gebruiker van deze toepassing niet toegewezen. | Ga naar het tabblad- **toepassing** en klik onder **gebruikers en groepen**, deze gebruiker of groep toewijzen aan deze toepassing.
Deze zakelijke app kan niet worden gebruikt. U bent niet gemachtigd voor toegang tot deze toepassing. Verificatie mislukt. Zorg ervoor dat de gebruiker een licentie voor Azure Active Directory Premium of Basic heeft. | Uw gebruikers mogelijk dit foutbericht krijgt wanneer bij het openen van de app die u hebt gepubliceerd als ze zijn niet expliciet met een Premium/Basic-licentie door de beheerder van de abonnee toegewezen. | Ga naar het tabblad van de Active Directory- **licenties** van de abonnee en zorg ervoor dat deze gebruiker of groep Premium of eenvoudige licentie is toegewezen.


## <a name="connector-troubleshooting"></a>Connector problemen oplossen
Als registratie tijdens de installatie van de wizard verbindingslijn mislukt, zijn er twee manieren om te bekijken van de reden voor de fout. U kunt zoeken in het gebeurtenislogboek onder **toepassingen en Services Logs\Microsoft\AadApplicationProxy\Connector\Admin**of Voer de volgende Windows PowerShell-opdracht.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Fout | Beschrijving | Resolutie |
| --- | --- | --- |
| Registratie van de verbindingslijn is mislukt: Zorg ervoor dat u toepassingsproxy in de beheerportal Azure en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: 'een of meer fouten opgetreden." | U het venster registratie zonder uit te voeren, meld u aan bij Azure AD gesloten. | De wizard Connector opnieuw uitvoeren en de verbindingslijn registreren. |
| Registratie van de verbindingslijn is mislukt: Zorg ervoor dat u toepassingsproxy in de beheerportal Azure en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: ' AADSTS50001: Resource `https://proxy.cloudwebappproxy.net /registerapp` is uitgeschakeld.' | Toepassingsproxy is uitgeschakeld.  | Zorg ervoor dat u toepassingsproxy in de portal van Azure klassieke inschakelen voordat u probeert te registreren van de verbindingslijn. Zie voor meer informatie over het inschakelen van toepassingsproxy [toepassingsproxy inschakelen services](active-directory-application-proxy-enable.md). |
| Registratie van de verbindingslijn is mislukt: Zorg ervoor dat u toepassingsproxy in de beheerportal Azure en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: 'een of meer fouten opgetreden." | Als de registratie-venster wordt geopend en klik vervolgens onmiddellijk wordt gesloten zonder dat u zich aanmelden, krijgt u waarschijnlijk deze fout. Deze fout treedt op als er een fout netwerken op uw systeem. | Zorg ervoor dat het is mogelijk verbinding maakt via een browser met een openbare website en dat de poort geopend zoals opgegeven in [toepassingsproxy vereisten zijn](active-directory-application-proxy-enable.md). |
| Registratie van de verbindingslijn is mislukt: Zorg ervoor dat de computer is verbonden met Internet. Fout: "Er is luistert geen eindpunt op `https://connector.msappproxy.net :9090/register/RegisterConnector` die het bericht kan accepteren. Dit is vaak veroorzaakt door een onjuist adres of SOAP-actie. Zie InnerException, als deze aanwezig zijn, voor meer informatie.' | Als u niet aanmelden via uw Azure AD-gebruikersnaam en wachtwoord maar klikt u vervolgens deze fout wordt weergegeven, is het mogelijk dat alle poorten boven 8081 worden geblokkeerd. | Zorg ervoor dat de benodigde poorten geopend zijn. Zie [toepassingsproxy vereisten](active-directory-application-proxy-enable.md)voor meer informatie. |
| Wissen fout wordt weergegeven in het venster registratie. Kan niet – niet doorgaan alleen het venster te sluiten. | U het verkeerde gebruikersnaam of wachtwoord hebt ingevoerd. | &Opnieuw. |
| Registratie van de verbindingslijn is mislukt: Zorg ervoor dat u toepassingsproxy in de beheerportal Azure en u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: ' AADSTS50059: er worden geen gegevens tenant identificeren gevonden op een van de aanvraag of geïmpliceerd door een opgegeven referenties en zoeken op service-principe URI is mislukt. | U probeert aan te melden met een Microsoft-Account en niet een domein die deel uitmaakt van de organisatie-ID van de map die u probeert te openen. | Zorg ervoor dat de beheerder maakt deel uit van dezelfde domeinnaam als de tenant-domein, bijvoorbeeld als het Azure AD-domein contoso.com, de beheerder moet zijn admin@contoso.com. |
| Het huidige execution beleid voor het uitvoeren van de PowerShell-scripts ophalen is mislukt. | Als de installatie van de Connector mislukt, controleert u of dat PowerShell execution beleid niet is uitgeschakeld. | Open de Groepsbeleid-Editor. Ga naar de **Computerconfiguratie** > **Administratieve sjablonen** > **Windows-onderdelen** > **Windows PowerShell** en dubbelklik op het **uitvoeren van scripts inschakelen**. Dit kan worden ingesteld op **Niet geconfigureerd** of **ingeschakeld**. Als op **ingeschakeld**ingesteld, zorg ervoor dat onder Opties voor het beleid is ingesteld op een **lokale scripts toestaan en externe ondertekend scripts** of toe te **staan dat alle scripts**. |
| Connector niet worden gedownload van de configuratie. | Clientcertificaat van de verbindingslijn, die wordt gebruikt voor verificatie, verlopen. Dit kan ook gebeuren als u de verbindingslijn geïnstalleerd achter een proxy hebt. In dit geval de verbindingslijn geen toegang tot Internet en is niet mogelijk te bieden toepassingen voor externe gebruikers. | Verlengen vertrouwen handmatig met behulp van de `Register-AppProxyConnector` cmdlet in Windows PowerShell. Als de verbindingslijn achter een proxy bevindt is, komt dat moet worden verleend, internettoegang tot de verbindingslijn accounts "netwerkservices" en "lokaal systeem." Dit kunt u doen door ze toegang verlenen aan de Proxy of door ze worden omzeild de proxy instellen. |
| Registratie van de verbindingslijn is mislukt: Controleer of u een globale beheerder van uw Active Directory registreren van de verbindingslijn. Fout: ' de registratieaanvraag is geweigerd.' | De alias die u probeert aan te melden met een beheerder in dit domein niet. De verbindingslijn is altijd geïnstalleerd voor de map die eigenaar is van het domein van de gebruiker. | Zorg ervoor dat de beheerder u probeert aan te melden als globale machtigingen om de Azure AD-tenant heeft.|


## <a name="kerberos-errors"></a>Kerberos-fouten

| Fout | Beschrijving | Resolutie |
| --- | --- | --- |
| Het huidige execution beleid voor het uitvoeren van de PowerShell-scripts ophalen is mislukt. | Als de installatie van de Connector mislukt, controleert u of dat PowerShell execution beleid niet is uitgeschakeld. | Open de Groepsbeleid-Editor. Ga naar de **Computerconfiguratie** > **Administratieve sjablonen** > **Windows-onderdelen** > **Windows PowerShell** en dubbelklik op het **uitvoeren van scripts inschakelen**. Dit kan worden ingesteld op **Niet geconfigureerd** of **ingeschakeld**. Als op **ingeschakeld**ingesteld, zorg ervoor dat onder Opties voor het beleid is ingesteld op beide **toestaan lokale scripts en externe ondertekend scripts** of toe te **staan dat alle scripts**. |
| 12008 - azure AD overschreden het maximale aantal toegestane Kerberos-verificatiepogingen naar de backend-server. | Deze gebeurtenis mogelijk onjuiste configuratie tussen Azure AD en de backend-toepassingsserver of een probleem in de configuratie van de datum en tijd op beide computers. | Endserver geweigerd de Kerberos-tickets gemaakt door Azure AD. Controleer of die Azure AD en de backend-toepassingsserver correct zijn geconfigureerd. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de backend-toepassingsserver worden gesynchroniseerd. |
| 13016 - azure AD kan een Kerberos-tickets namens de gebruiker niet ophalen omdat er geen UPN in het token rand of in de access-cookie. | Er is een probleem met de configuratie STS. | Problemen met de configuratie van de claim UPN in de STS oplossen. |
| 13019 - azure AD kan een Kerberos-tickets namens de gebruiker niet ophalen vanwege de volgende algemene API-fout. | Deze gebeurtenis mogelijk onjuiste configuratie tussen Azure AD en de domeincontroller-server, of een probleem in de configuratie van de datum en tijd op beide computers. | De domeincontroller geweigerd de Kerberos-tickets gemaakt door Azure AD. Controleer of die Azure AD en de backend-toepassingsserver zijn geconfigureerd, met name de configuratie name. Controleer of dat de Azure AD is toegevoegd aan het hetzelfde domein als de domeincontroller om ervoor te zorgen dat de domeincontroller tot stand vertrouwen met Azure AD brengt domein. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de domeincontroller worden gesynchroniseerd. |
| 13020 - azure AD kan een Kerberos-tickets namens de gebruiker niet ophalen omdat endserver name niet is gedefinieerd. | Deze gebeurtenis mogelijk onjuiste configuratie tussen Azure AD en de domeincontroller-server, of een probleem in de configuratie van de datum en tijd op beide computers. | De domeincontroller geweigerd de Kerberos-tickets gemaakt door Azure AD. Controleer of die Azure AD en de backend-toepassingsserver zijn geconfigureerd, met name de configuratie name. Controleer of dat de Azure AD is toegevoegd aan het hetzelfde domein als de domeincontroller om ervoor te zorgen dat de domeincontroller tot stand vertrouwen met Azure AD brengt domein. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de domeincontroller worden gesynchroniseerd. |
| 13022 - azure AD kan de gebruiker niet verifiëren omdat endserver moet op Kerberos-verificatiepogingen met een HTTP 401-fout reageren. | Deze gebeurtenis mogelijk onjuiste configuratie tussen Azure AD en de backend-toepassingsserver of een probleem in de configuratie van de datum en tijd op beide computers. | Endserver geweigerd de Kerberos-tickets gemaakt door Azure AD. Controleer of die Azure AD en de backend-toepassingsserver correct zijn geconfigureerd. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de backend-toepassingsserver worden gesynchroniseerd. |
| De pagina weergeven niet. | Uw gebruikers mogelijk dit foutbericht krijgt wanneer bij het openen van de app die u als de toepassing een uitgebreid is gepubliceerd. De gedefinieerde name van deze toepassing is mogelijk onjuist. | Voor geïntegreerde Windows-apps: Zorg ervoor dat de name die is geconfigureerd voor deze toepassing correct is. |
| De pagina weergeven niet. | Uw gebruikers mogelijk dit foutbericht krijgt wanneer bij het openen van de app die u als de toepassing een OWA is gepubliceerd. Dit kan worden veroorzaakt door een van de volgende opties: <br> -De gedefinieerde name van deze toepassing is onjuist. <br> -De gebruiker die heeft geprobeerd te krijgen tot de toepassing wordt gebruikt een Microsoft-account in plaats van de juiste bedrijfsaccount aan te melden of de gebruiker is een gastgebruiker. <br> -De gebruiker die heeft geprobeerd te krijgen tot de toepassing wordt niet goed gedefinieerd voor deze toepassing aan de on-premises-kant. | De stappen te verminderen dienovereenkomstig gewijzigd: <br> -Controleer of de name die is geconfigureerd voor deze toepassing correct is. <br> -Controleer of de gebruiker zich aan met hun bedrijfsaccount die overeenkomt met het domein van de gepubliceerde toepassing. Gebruikers van Microsoft-Account en als gast geen toegang tot de geïntegreerde Windows-toepassingen. <br> -Zorg ervoor dat deze gebruiker de juiste machtigingen heeft, zoals gedefinieerd voor deze backend-toepassing op de computer van de on-premises. |
| Deze zakelijke app kan niet worden gebruikt. U bent niet gemachtigd voor toegang tot deze toepassing. Verificatie mislukt. Zorg ervoor dat de gebruiker met access toewijzen aan deze toepassing. | Uw gebruikers mogelijk dit foutbericht krijgt wanneer bij het openen van de app die u hebt gepubliceerd als ze met Microsoft-accounts in plaats van hun zakelijke account voor aanmelden. Deze fout kunnen ook openen door gastgebruikers. | Gebruikers van Microsoft-Account en gasten geen toegang tot de geïntegreerde Windows-toepassingen. Zorg ervoor dat de gebruiker zich aanmeldt met behulp van hun bedrijfsaccount die overeenkomt met het domein van de gepubliceerde toepassing. |
| Deze zakelijke app kan niet nu worden geopend. Probeer het later opnieuw... De verbindingslijn timed-out. | Uw gebruikers mogelijk deze fout krijgen wanneer u probeert voor toegang tot de app die u als ze niet correct zijn gedefinieerd voor deze toepassing aan de on-premises gepubliceerd. | Zorg ervoor dat uw gebruikers de juiste machtigingen hebben, zoals gedefinieerd voor deze backend-toepassing op de computer van de on-premises. |


## <a name="see-also"></a>Zie ook

- [Toepassingsproxy voor Azure Active Directory inschakelen](active-directory-application-proxy-enable.md)
- [Toepassingen met toepassingsproxy publiceren](active-directory-application-proxy-publish.md)
- [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
