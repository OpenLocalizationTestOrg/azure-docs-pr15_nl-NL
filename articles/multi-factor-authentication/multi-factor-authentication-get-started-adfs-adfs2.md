<properties
    pageTitle="Gebruik van Azure MFA Server met AD FS 2.0 | Microsoft Azure"
    description="Dit is de pagina van de Azure meervoudige verificatie die wordt beschreven hoe u aan de slag met Azure MFA en AD FS 2.0."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Cloud en on-premises bronnen Azure meervoudige verificatieserver gebruikt met AD FS 2.0 beveiligen

Dit artikel is bedoeld voor organisaties die zijn gekoppeld aan Azure Active Directory en wilt beveiligen van resources die on-premises implementatie zijn of in de cloud. Uw resources beveiligen door de Server Azure meervoudige verificatie en configureren om te werken met AD FS zodat verificatie in twee stappen voor de eindpunten hoge waarde wordt geactiveerd.

Deze documentatie wordt beschreven voor het gebruik van de Server Azure meervoudige verificatie met AD FS 2.0.  Meer informatie over [beveiligen cloud en on-premises bronnen met Azure meervoudige verificatieserver met Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)krijgen.


## <a name="secure-ad-fs-20-with-a-proxy"></a>AD FS 2.0 beveiligen met een proxy
Als u wilt beveiligen AD FS 2.0 met een proxy, de Server Azure meervoudige verificatie installeren op de ADFS-proxyserver en configureren van de Server.

### <a name="configure-iis-authentication"></a>IIS-verificatie configureren

1. Klik op het pictogram **IIS-verificatie** in het linkermenu binnen de Azure meervoudige verificatie-Server.
2. Klik op het tabblad **Op basis van een formulier** .
3. Klik op de **toevoegen...** knop.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Gebruikersnaam, wachtwoord en domein variabelen automatisch bepalen, voert u de aanmeldings-URL (zoals https://sso.contoso.com/adfs/ls) in het dialoogvenster Auto-Configure Form-Based Website en klik op OK.
5. Schakel het selectievakje **Azure meervoudige verificatie vereisen gebruiker overeenkomen met** als alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor verificatie in twee stappen. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van verificatie in twee stappen, laat u het vak uitgeschakeld. Zie voor meer informatie over deze functie, het help-bestand.
6. Als de variabelen pagina kunnen niet automatisch worden gedetecteerd, klikt u op de **Opgeven handmatig...** knop in het dialoogvenster Auto-Configure Form-Based-Website.
7. In het dialoogvenster Add Form-Based Website, voert u de URL naar de aanmeldingspagina ADFS in het veld URL verzenden (zoals https://sso.contoso.com/adfs/ls) en voer een naam van een toepassing (optioneel). De naam van de toepassing wordt weergegeven in Azure meervoudige verificatie-rapporten en kan worden weergegeven in SMS of Mobile-App verificatie-berichten. Zie het help-bestand voor meer informatie op de URL indienen.
8. Instellen van de indeling van het verzoek naar "Posten of KRIJG."
9. Voer de gebruikersnaam (ctl00$ ContentPlaceHolder1$ UsernameTextBox) en wachtwoord variabele (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Als uw op formulieren gebaseerde aanmeldingspagina wordt een tekstvak domein weergegeven, voert u ook de variabele van het domein. Mogelijk moet u navigeren naar de aanmeldingspagina in een webbrowser, met de rechtermuisknop op de pagina en selecteer **Bron weergeven** om de namen van de invoervakken binnen de aanmeldingspagina.
10. Schakel het selectievakje **Azure meervoudige verificatie vereisen gebruiker overeenkomen met** als alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor verificatie in twee stappen. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van verificatie in twee stappen, laat u het vak uitgeschakeld.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klik op de **Geavanceerde...** knop moet worden gereviseerd geavanceerde instellingen. U kunt instellingen waaronder de mogelijkheid om te selecteren van een bestand van de pagina aangepaste weigering succesvolle authenticatie naar de website van het gebruik van cookies in cache en selecteert u hoe u de primaire referenties verifiëren.
12. Aangezien de AD FS-proxyserver niet kunnen worden toegevoegd aan het domein is, kunt u LDAP verbinding maken met uw domeincontroller voor het importeren van de gebruiker en verificatie vooraf. Klik in het dialoogvenster Advanced Form-Based Website klikt u op het tabblad **Primaire verificatie** en selecteer **LDAP afhankelijk maken** voor het type verificatie vooraf-verificatie.
13. Als alles compleet is, klikt u op de knop **OK** om terug te keren naar het dialoogvenster Add Form-Based-Website. Zie het help-bestand voor meer informatie over de geavanceerde instellingen.
14. Klik op de knop **OK** om het dialoogvenster te sluiten.
15. Zodra de URL's en pagina-variabelen zijn gevonden of hebt ingevoerd, wordt de websitegegevens weergegeven in het deelvenster op basis van een formulier.
16. Klik op het tabblad **Systeemeigen Module** en selecteert u de server, de website waarop de AD FS-proxy onder (zoals "Standaardwebsite") wordt uitgevoerd of de AD FS-proxy-toepassing (zoals "ls" onder 'adfs') om in te schakelen van de invoegtoepassing IIS op het gewenste niveau.
17. Klik in het vak **inschakelen IIS-verificatie** boven aan het scherm.
18. De IIS-verificatie is nu ingeschakeld.

### <a name="configure-directory-integration"></a>Directory-integratie configureren

U IIS-verificatie hebt ingeschakeld, maar de oude naar uw AD (Active Directory) via LDAP-verificatie moet u de LDAP-verbinding met de domeincontroller configureren.

1. Klik op het pictogram **Directory-integratie** .
2. Selecteer het keuzerondje **specifieke LDAP-configuratie gebruiken** op het tabblad instellingen.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Klik op de **bewerken...** knop.
4. Klik in het dialoogvenster bewerken LDAP-configuratie de velden in te vullen met de gegevens die nodig is om te verbinden met de domeincontroller AD. Beschrijvingen van de velden zijn opgenomen in de onderstaande tabel. Deze informatie is ook in het help-bestand van Azure meervoudige verificatieserver opgenomen.
5. De LDAP-verbinding testen door te klikken op de knop **testen** .
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Als de LDAP-verbindingstest geslaagd is, klikt u op de knop **OK** .

### <a name="configure-company-settings"></a>Bedrijfsinstellingen configureren

1. Vervolgens klikt u op het pictogram **Instellingen van het bedrijf** en selecteer het tabblad **Gebruikersnaam resolutie** .
2. Selecteer het keuzerondje **Gebruik LDAP unieke id-kenmerk voor overeenkomende gebruikersnamen** .
3. Als gebruikers hun gebruikersnaam in "domein\gebruikersnaam"-indeling, wordt de Server moet kunnen verwijderen van het domein uit de gebruikersnaam bij het maken van de LDAP-query. Die kunnen worden uitgevoerd via een registerinstelling.
4. Open de Register-editor en gaat u naar HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/positieve netwerken/PhoneFactor op een 64-bits-server. Als u op een 32-bits-server, duren voordat de "Wow6432Node" Afmelden bij het pad. Maak een DWORD-registersleutel 'UsernameCxz_stripPrefixDomain' genoemd en stel de waarde op 1. Azure meervoudige verificatie is nu de beveiliging van de AD FS-proxy.

Zorg ervoor dat gebruikers zijn geïmporteerd uit Active Directory in de Server. Zie de [sectie vertrouwde IP -adressen](#trusted-ips) als u "witte" lijst interne IP-adressen wilt zodat verificatie in twee stappen niet vereist is tijdens het aanmelden bij de website van deze locaties.

<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 rechtstreekse zonder proxy

U kunt beveiligen AD FS wanneer de AD FS-proxy niet wordt gebruikt. De Server Azure meervoudige verificatie op de AD FS-server installeren en configureren van de Server per de volgende stappen uit:

1. Klik op het pictogram **IIS-verificatie** in het linkermenu binnen de Azure meervoudige verificatie-Server.
2. Klik op het tabblad **HTTP** .
3. Klik op de **toevoegen...** knop.
4. Voer in het dialoogvenster toevoegen basis-URL de URL voor de ADFS-website waar HTTP-verificatie wordt uitgevoerd (zoals https://sso.domain.com/adfs/ls/auth/integrated) naar het veld basis-URL. Voer vervolgens een naam van een toepassing (optioneel). De naam van de toepassing wordt weergegeven in Azure meervoudige verificatie-rapporten en kan worden weergegeven in SMS of Mobile-App verificatie-berichten.
5. Desgewenst aanpassen de inactiviteit en Maximum sessie tijden.
6. Schakel het selectievakje **Azure meervoudige verificatie vereisen gebruiker overeenkomen met** als alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor verificatie in twee stappen. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van verificatie in twee stappen, laat u het vak uitgeschakeld. Zie voor meer informatie over deze functie, het help-bestand.
7. Schakel het selectievakje van de cache cookie desgewenst.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klik op de knop **OK** .
9. Klik op het tabblad **Systeemeigen Module** en selecteert u de server, de website (zoals "Standaardwebsite") of de ADFS-toepassing (zoals "ls" onder 'adfs') om in te schakelen van de invoegtoepassing IIS op het gewenste niveau.
10. Klik in het vak **inschakelen IIS-verificatie** boven aan het scherm. ADFS is nu de beveiliging van Azure meervoudige verificatie.

Zorg ervoor dat gebruikers zijn geïmporteerd uit Active Directory in de Server. Zie het gedeelte vertrouwde IP-adressen als u "witte" lijst interne IP-adressen wilt zodat verificatie in twee stappen niet vereist is tijdens het aanmelden bij de website van deze locaties.


## <a name="trusted-ips"></a>Vertrouwde IP-adressen
Vertrouwde IP-adressen gebruikers, worden omzeild Azure meervoudige verificatie voor de website aanvragen die afkomstig zijn van specifieke IP-adressen of subnetten toestaan. Zoals u mogelijk wilt vrijgestelde gebruikers uit verificatie in twee stappen wanneer ze zich vanuit de office aanmelden. Hiervoor geeft u het office-subnet als de invoer in een vertrouwde IP-adressen.

### <a name="to-configure-trusted-ips"></a>Vertrouwde IP-adressen configureren


1. Klik op het tabblad **Vertrouwde IP -adressen** in de sectie IIS-verificatie.
1. Klik op de **toevoegen...** knop.
1. Wanneer het dialoogvenster vertrouwde IP's toevoegen wordt weergegeven, selecteert u een van de keuzerondjes **Één IP**, **IP-bereik**of **Subnet** .
1. Voer het IP-adres, bereik van IP-adressen of subnetten die whitelisted worden moet. Als een subnet invoeren, selecteert u het juiste netmasker en klik op **OK** . De vertrouwde IP is nu toegevoegd.


<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
