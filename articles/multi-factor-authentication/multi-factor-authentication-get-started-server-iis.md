<properties 
    pageTitle="IIS-verificatie en Azure meervoudige verificatie-Server"
    description="Dit is de pagina van de Azure meervoudige verificatie die helpt bij het implementeren van IIS-verificatie en Azure meervoudige verificatie-Server."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="iis-authentication"></a>IIS-verificatie

De sectie IIS-verificatie van de Azure meervoudige verificatie-Server kunt u inschakelen en configureren van IIS-verificatie voor integratie met Microsoft IIS-webtoepassingen. De Server Azure meervoudige verificatie een invoegtoepassing die u kunt filteren aanvragen verwijzingen naar de webserver IIS om te kunnen toevoegen Azure meervoudige verificatie geïnstalleerd. De invoegtoepassing IIS biedt ondersteuning voor formulieren gebaseerde verificatie en geïntegreerde Windows HTTP-verificatie. Vertrouwde dat IP-adressen kan ook worden geconfigureerd met vrijgestelde interne IP-adressen van tweeledige verificatie.


![IIS-verificatie](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Gebruik van IIS formulieren gebaseerde verificatie met Azure meervoudige verificatie-Server

Als u wilt beveiligen een IIS-webtoepassing op formulieren gebaseerde verificatie, de Server Azure meervoudige verificatie installeren op de webserver IIS en configureren van de Server per de volgende procedure.

1. Klik op het pictogram IIS-verificatie in het linkermenu binnen de Server Azure meervoudige verificatie.
2. Klik op het tabblad op basis van een formulier.
3. Klik op de toevoegen... knop.
4. Om op te sporen gebruikersnaam, wachtwoord en domein variabelen automatisch de aanmeldings-URL (bijvoorbeeld https://localhost/contoso/auth/login.aspx) in het dialoogvenster Auto-Configure Form-Based Website en klik op OK.
5. Schakel het selectievakje meervoudige verificatie vereisen gebruiker overeenkomst in als u alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor meervoudige verificatie. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van meervoudige verificatie, laat u het vak uitgeschakeld.
6. Als de variabelen pagina kunnen niet automatisch worden gedetecteerd, klikt u op de opgeven handmatig... knop in het dialoogvenster Auto-Configure Form-Based-Website.
7. In het dialoogvenster Add Form-Based Website, voert u de URL naar de aanmeldingspagina in het veld URL verzenden en voer (optioneel) op de naam van een toepassing. De naam van de toepassing wordt weergegeven in Azure meervoudige verificatie-rapporten en kan worden weergegeven in SMS of Mobile-App verificatie-berichten. Zie het help-bestand voor meer informatie op de URL indienen.
8. Selecteer de juiste indeling. Dit is ingesteld op 'Bericht of krijgt' voor de meeste webtoepassingen.
9. Voer de gebruikersnaam, wachtwoord variabele en domein variabele (als deze wordt weergegeven op de aanmeldingspagina). Mogelijk moet u navigeren naar de aanmeldingspagina in een webbrowser, met de rechtermuisknop op de pagina en selecteer "bron weergeven' de namen van de invoervakken binnen de pagina zoeken.
10. Schakel het selectievakje van Azure meervoudige verificatie vereisen gebruiker vergelijken als alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor meervoudige verificatie. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van meervoudige verificatie, laat u het vak uitgeschakeld. Zie het help-bestand voor meer informatie over deze functie.
11.  Klik op de geavanceerde... knop moet worden gereviseerd geavanceerde instellingen, waaronder de mogelijkheid om te selecteren van een bestand van de pagina aangepaste weigering succesvolle authenticatie naar de website voor een periode met behulp van cookies in de cache en selecteren of u wilt verifiëren van de primaire referenties ten opzichte van het Windows-domein, een LDAP-diagram of een RADIUS-server. Klik op de knop OK om terug te keren naar het dialoogvenster Add Form-Based Website wanneer voltooid. Zie het help-bestand voor meer informatie over de geavanceerde instellingen.
12. Klik op de knop OK.
13. Zodra de URL's en pagina-variabelen zijn gevonden of hebt ingevoerd, worden de websitegegevens wordt weergegeven in het deelvenster op basis van een formulier.
14. Zie de IIS inschakelen Plug-ins voor Azure meervoudige verificatieserver sectie rechtstreeks hieronder om de configuratie van IIS-verificatie te voltooien.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Geïntegreerde Windows-verificatie gebruikt met de Server Azure meervoudige verificatie

Als u wilt beveiligen een IIS-webtoepassing die geïntegreerde Windows HTTP-verificatie gebruikt, de Server Azure meervoudige verificatie installeren op de webserver IIS en configureren van de Server per de volgende procedure.

1. Klik op het pictogram IIS-verificatie in het linkermenu binnen de Server Azure meervoudige verificatie.
2. Klik op het tabblad HTTP. Klik op het tabblad op basis van een formulier.
3. Klik op de toevoegen... knop.
4. In het dialoogvenster basis-URL toevoegen, voert u de URL voor de website waar HTTP-verificatie is uitgevoerd (bijvoorbeeld http://localhost/owa) naar het veld basis-URL en voer (optioneel) op de naam van een toepassing. De naam van de toepassing wordt weergegeven in Azure meervoudige verificatie-rapporten en kan worden weergegeven in SMS of Mobile-App verificatie-berichten.
5. Pas de inactiviteit en Maximum sessie tijden als de standaardinstelling niet voldoende is.
6. Schakel het selectievakje meervoudige verificatie vereisen gebruiker overeenkomst in als u alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor meervoudige verificatie. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van meervoudige verificatie, laat u het vak uitgeschakeld. Zie het help-bestand voor meer informatie over deze functie.
7. Schakel het selectievakje van de cache cookie desgewenst.
8. Klik op de knop OK.
9. Zie het gedeelte [inschakelen IIS-invoegtoepassingen voor Azure meervoudige verificatieserver](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) direct onder de IIS-verificatie-configuratie voltooid.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>IIS-Plug-ins inschakelen voor de Server Azure meervoudige verificatie

Nadat u hebt geconfigureerd de op formulieren gebaseerde of HTTP-verificatie-URL's en instellingen, moet u de locaties waar de Azure meervoudige verificatie IIS-plug-ins moet worden geladen en ingeschakeld in IIS. Gebruik de volgende procedure:

1. Als u op IIS 6, klik op het tabblad ISAPI- en selecteert u de website waarop de webtoepassing onder (bijvoorbeeld standaardwebsite) wordt uitgevoerd als het filter Azure meervoudige verificatie ISAPI-invoegtoepassing voor deze site wilt inschakelen.
2. Als u op IIS-7 of hoger, klik op het tabblad systeemeigen Module en selecteert u de server, website (s) of toepassingen om in te schakelen van de invoegtoepassing IIS op het gewenste coderingsniveau.
3. Klik in het vak van de verificatie inschakelen IIS boven aan het scherm. Azure meervoudige verificatie is nu de beveiliging van de geselecteerde IIS-toepassing. Zorg ervoor dat gebruikers in de Server zijn geïmporteerd. Zie het gedeelte vertrouwde IP-adressen als u "witte" lijst die interne IP-adressen wilt zodat deze tweeledige verificatie niet vereist is bij het aanmelden bij de website van deze locaties.


## <a name="trusted-ips"></a>Vertrouwde IP-adressen

Het IP-vertrouwde adressen kunnen gebruikers, worden omzeild Azure meervoudige verificatie voor de website aanvragen die afkomstig zijn van specifieke IP-adressen of subnetten. Bijvoorbeeld, kunt u aan vrijgestelde gebruikers van Azure meervoudige verificatie bij het aanmelden vanuit de office. Hiervoor moet u het office-subnet opgeven als een vermelding vertrouwde IP-adressen. Als u wilt configureren Gebruik vertrouwde IP-adressen in de volgende procedure:

1. Klik op het tabblad vertrouwde IP-adressen in de sectie IIS-verificatie.
2. Klik op de toevoegen... knop.
3. Wanneer het dialoogvenster vertrouwde IP's toevoegen wordt weergegeven, selecteer het één IP-, het IP-bereik of de Subnet keuzerondje.
4. Geef het IP-adres, bereik van IP-adressen of subnet die whitelisted worden moet. Als een subnet invoeren, selecteert u het juiste netmasker en klik op OK. De "witte" lijst is nu toegevoegd.
