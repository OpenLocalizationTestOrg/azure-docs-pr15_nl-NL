<properties
    pageTitle="Azure AD verbinding systeemstatus Agent installatie | Microsoft Azure"
    description="Dit is de status verbinding maken met Azure AD-pagina die goed de installatie agent voor AD FS en synchroniseren beschrijft."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect systeemstatus Agent installatie

In dit document begeleidt u bij het installeren en configureren van de Azure AD verbinding systeemstatus kunt vinden. U kunt de agenten downloaden van [hier](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  <a name="requirements"></a>Vereisten
In de volgende tabel wordt een lijst met vereisten voor het gebruik van Azure AD verbinding status.

| Vereiste | Beschrijving|
| ----------- | ---------- |
|Azure AD Premium| Azure AD verbinding servicestatus is een functie Azure AD Premium en Azure AD Premium vereist. </br></br>Zie voor meer informatie [aan de slag met Azure AD Premium](active-directory-get-started-premium.md) </br>Zie een gratis proefabonnement van 30 dagen starten [starten van een proefversie.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|U moet een globale beheerder van uw Azure AD aan de slag met Azure AD verbinding systeemstatus|Standaard kunnen alleen globale beheerders installeren en configureren van de systeemstatus kunt vinden om aan de slag, toegang tot de portal en andere bewerkingen binnen Azure AD verbinding systeemstatus uitvoeren. Zie [het telefoonboek van uw Azure AD beheren](active-directory-administer.md)voor meer informatie. <br><br> Gebruik van de rol gebaseerd toegangsbeheer kunt u toegang tot Azure AD verbinding systeemstatus aan andere gebruikers in uw organisatie toestaat. Zie voor meer informatie [rol gebaseerd toegangsbeheer voor Azure AD verbinding systeemstatus.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Belangrijke:** Het account wordt gebruikt tijdens de installatie van de agenten moet een account voor werk- of schoolaccount. Een Microsoft-account kan niet. Voor meer informatie raadpleegt u [zich aanmeldt voor Azure als organisatie](sign-up-organization.md)
|De Azure AD verbinding systeemstatus-Agent is geïnstalleerd op elke gerichte server| Azure AD verbinding systeemstatus vereist een agent installatie op gerichte-servers op te geven van de gegevens die wordt weergegeven in de portal. </br></br>Als u gegevens vanuit de infrastructuur van uw AD FS-on-premises implementatie, moet de agent bijvoorbeeld worden geïnstalleerd op de AD FS, AD FS-Proxy en Web toepassingsproxy-servers. Ook toegang tot gegevens in uw on-premises AD DS-infrastructuur de agent moet zijn geïnstalleerd op de domeincontrollers. </br></br>**Belangrijke:** Het account wordt gebruikt tijdens de installatie van de agenten moet een account voor werk- of schoolaccount. Een Microsoft-account kan niet. Voor meer informatie raadpleegt u [zich aanmeldt voor Azure als organisatie](sign-up-organization.md)|
|Uitgaande verbinding met de eindpunten Azure-service|Tijdens de installatie en runtime, de-agent is connectiviteit vereist met Azure AD verbinding systeemstatus service eindpunten. Als uitgaande verbinding is geblokkeerd, moet u ervoor zorgen dat de volgende eindpunten worden toegevoegd aan de lijst met toegestane: </br></br><li>& #42;. BLOB.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - poort: 5671 </li><li>https://Management.Azure.com </li><li>https://S1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.dc.AD.MSFT.NET/</li><li>https://login.Windows.NET</li><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Firewallpoorten op de server met de agent.| De-agent is vereist voor de volgende firewallpoorten worden geopend in de volgorde voor de agent om te communiceren met de eindpunten van de service Azure AD status.</br></br><li>TCP/UDP-poort 443</li><li>TCP/UDP-poorten 5671</li>
|De volgende websites toestaan als IE verbeterde beveiliging is ingeschakeld| Verbeterde beveiliging van IE is ingeschakeld, moet de volgende websites zijn toegestaan op de server die u wilt de agent is geïnstalleerd.</br></br><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://login.Windows.NET</li><li>De federatieserver voor uw organisatie vertrouwd door Azure Active Directory. Bijvoorbeeld: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Verbinding maken met de installatie van de Azure AD met systeemstatus Agent voor AD FS
U start de agent-installatie, dubbelklikt u op het .exe-bestand dat u hebt gedownload. Klik op het eerste scherm, klikt u op installeren.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health-requirements/install1.png)

Nadat de installatie is voltooid, klikt u op nu configureren.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health-requirements/install2.png)

Een opdrachtprompt wordt gestart, gevolgd door sommige PowerShell die wordt uitgevoerd Register-AzureADConnectHealthADFSAgent. Wanneer u hierom wordt gevraagd om aan te melden bij Azure, verdergaan en meld u aan.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health-requirements/install3.png)


Na het aanmelden, blijft de PowerShell. Zodra deze is voltooid, kunt u PowerShell sluiten en de configuratie is voltooid.

Nu moeten de services worden gestart automatisch toestaan de agent om te controleren en gegevens te verzamelen. Als u niet alle minimumvereisten die worden beschreven in de vorige gedeelten voldaan, weergegeven waarschuwingen in het venster PowerShell. Zorg ervoor dat de [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) uitvoert voordat u de agent installeert. De volgende schermafbeelding is een voorbeeld van deze fouten.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health-requirements/install4.png)

Als u wilt controleren of dat de-agent is geïnstalleerd, zoekt u de volgende services op de server. Als u de configuratie voltooid, moeten ze al worden uitgevoerd. Anders worden ze gestopt totdat de configuratie voltooid is.

- Azure AD Connect systeemstatus AD FS diagnostische Service
- Azure AD Connect systeemstatus AD FS inzichten Service
- Azure AD Connect systeemstatus AD FS-Service controleren

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Agent installatie op Windows Server 2008 R2 Servers

Stappen voor het Windows Server 2008 R2-servers:

1. Zorg ervoor dat de server wordt uitgevoerd op de Service Pack 1 of hoger.
1. Verbeterde beveiliging uitschakelen voor agent installatie:
1. Installeer Windows PowerShell 4.0 op alle servers ligt het installeren van de servicestatus van AD-agent. Windows PowerShell 4.0 installeren:
 - Installeer [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) met de volgende koppeling om te downloaden van het installatieprogramma van offline.
 - Filter wissen (vanuit Windows-functies) installeren
 - Installeer de [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Installeren van Internet Explorer versie 10 of hoger op de server. (Vereist door de Service systeemstatus om te verifiëren, met uw beheerdersreferenties van Azure.)
1. Zie het wiki-artikel voor meer informatie over het installeren van Windows PowerShell 4.0 op Windows Server 2008 R2 [hier](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Controle voor AD FS inschakelen

> [AZURE.NOTE] In deze sectie geldt alleen voor AD FS-federatieservers.

De status verbinding maken met Azure AD-agent moet in de volgorde voor de functie gebruiksanalyses verzamelen en analyseren van gegevens, de informatie in de AD FS controlelogboeken bijhouden. Deze logboeken worden niet al dan niet standaard ingeschakeld. Gebruik de volgende procedures uit AD FS controle inschakelen en zoek de AD FS controlelogboeken bijhouden, op de AD FS-servers.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Controle voor AD FS 2.0 inschakelen

1. Klik op **Start**, wijs achtereenvolgens **programma's**, **Systeembeheer**en klik vervolgens op **Lokaal beveiligingsbeleid**.
2. Navigeer naar de map **Beveiliging instellingen\Beveiligingsinstellingen\Lokaal beleid\Toewijzing Rights Management** en dubbelklik vervolgens op genereren kunnen worden opgeslagen.
3. Controleer of de AD FS 2.0-serviceaccount wordt weergegeven op het tabblad **Lokale beveiligingsinstellingen** . Als niet aanwezig is, klikt u op **gebruiker of groep toevoegen** en toe te voegen aan de lijst en klik vervolgens op **OK**.
4. Als u wilt controleren, open een opdrachtprompt met verhoogde bevoegdheden en voer de volgende opdracht:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Sluit Lokaal beveiligingsbeleid en open vervolgens de module Beheer. Als u wilt openen de module Beheer, klik op **Start**, wijs achtereenvolgens **programma's**, **Systeembeheer**en klik vervolgens op AD FS 2.0 Management.
6. Klik in het deelvenster Acties op Federatie Service-eigenschappen bewerken.
7. Klik op het tabblad **gebeurtenissen** in het dialoogvenster **Eigenschappen van de Service Federatie** .
8. Schakel de selectievakjes **geslaagde** en **mislukte controles** .
9. Klik op **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Controle voor AD FS op Windows Server 2012 R2 inschakelen

1. Open **Lokaal beveiligingsbeleid** door **Serverbeheer** in het startscherm of Serverbeheer openen in de taakbalk op het bureaublad en klik op **Hulpmiddelen voor/lokaal beveiligingsbeleid**.
2. Navigeer naar de map **Beveiliging instellingen\Beveiligingsinstellingen\Lokaal Beleid\toewijzing** en dubbelklik vervolgens op **genereren beveiliging kunnen worden opgeslagen**.
3. Controleer of de AD FS-serviceaccount wordt weergegeven op het tabblad **Lokale beveiligingsinstellingen** . Als niet aanwezig is, klikt u op **gebruiker of groep toevoegen** en toe te voegen aan de lijst en klik vervolgens op **OK**.
4. Als u wilt controleren, open een opdrachtprompt met verhoogde bevoegdheden en voer de volgende opdracht:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. **Lokaal beveiligingsbeleid**sluiten en open vervolgens het **Beheer van AD FS** -module (in Serverbeheer op Extra en selecteer vervolgens AD FS Management).
6. Klik in het deelvenster Acties op **Federatie Service-eigenschappen bewerken**.
7. Klik op het tabblad **gebeurtenissen** in het dialoogvenster Eigenschappen van de Service Federatie.
8. Schakel de selectievakjes **geslaagde en mislukte controles** en klik vervolgens op **OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Om te zoeken als u de AD FS-controlelogboeken Logboeken


1. Open **Logboeken**.
2. Ga naar de Windows-Logboeken en selecteer **beveiliging**.
3. Klik op **Filter huidige logboeken**aan de rechterkant.
4. Klik onder bron van gebeurtenis, selecteer **AD FS controle**.

![AD FS controlelogboeken bijhouden](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Als een groepsbeleid dat het uitschakelen van AD FS controle is bestaat, klikt u vervolgens is de Azure AD verbinding systeemstatus-Agent niet mogelijk om informatie te verzamelen. Zorg ervoor dat er geen een Groepsbeleid die mogelijk worden uitschakelen controle.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>De status verbinding maken met Azure AD-agent voor synchronisatie installeren
De status verbinding maken met Azure AD-agent voor synchronisatie wordt automatisch geïnstalleerd in de meest recente versie van Azure AD Connect. Als u wilt gebruiken Azure AD Connect voor synchronisatie, moet u de nieuwste versie van Azure AD Connect downloaden en installeren. Kunt u de meest recente versie downloaden [hier](http://www.microsoft.com/download/details.aspx?id=47594).

Als u wilt controleren of dat de-agent is geïnstalleerd, zoekt u de volgende services op de server. Als u de configuratie voltooid, moeten ze al worden uitgevoerd. Anders worden ze gestopt totdat de configuratie voltooid is.

- Azure AD Connect status synchroniseren inzichten Service
- Azure AD Connect status synchroniseren Service controleren

![Controleer of Azure AD Connect servicestatus voor synchronisatie](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Houd er rekening mee dat het gebruik van Azure AD verbinding systeemstatus Azure AD Premium vereist. Als u nog geen Azure AD Premium, kan u niet met de configuratie in de portal van Azure voltooien. Zie de [pagina vereisten](active-directory-aadconnect-health-agent-install.md#requirements)voor meer informatie.


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Handmatige Azure AD verbinding servicestatus voor registratie van synchronisatie
Als de Azure op AD verbinding maken met status voor synchronisatie agent registratie mislukt na de installatie van Azure AD Connect is, kunt u het volgende PowerShell-opdracht handmatig registreren de agent.

>[AZURE.IMPORTANT] Met deze PowerShell-opdracht is alleen als de registratie agent mislukt na de installatie van Azure AD Connect vereist.

De volgende PowerShell opdracht is vereist alleen wanneer de status agent registratie mislukt zelfs na een geslaagde installatie en configuratie van Azure AD Connect. De status verbinding maken met Azure AD-services begint nadat de-agent is geregistreerd.

U kunt handmatig de status verbinding maken met Azure AD-agent voor synchronisatie met de volgende PowerShell-opdracht registreren:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

De opdracht gaat volgen parameters:

- AttributeFiltering: $true (standaard) - als Azure AD Connect is niet gesynchroniseerd voor het kenmerk standaard instellen en is aangepast voor het gebruik van een kenmerkset gefilterde. $false anders.
- StagingMode: $false (standaard) - als de server Azure AD Connect niet is in de modus $true tijdelijke als de server is geconfigureerd om te worden in de modus waarin u tijdelijk opslaat.

Wanneer u wordt gevraagd voor verificatie moet u het account globale beheerder (zoals admin@domain.onmicrosoft.com) die is gebruikt voor het configureren van Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Verbinding maken met de installatie van de Azure AD met systeemstatus Agent voor AD DS
U start de agent-installatie, dubbelklikt u op het .exe-bestand dat u hebt gedownload. Klik op het eerste scherm, klikt u op installeren.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Nadat de installatie is voltooid, klikt u op nu configureren.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Een opdrachtprompt wordt gestart, gevolgd door sommige PowerShell die wordt uitgevoerd Register-AzureADConnectHealthADDSAgent. Wanneer u hierom wordt gevraagd om aan te melden bij Azure, verdergaan en meld u aan.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Na het aanmelden, blijft de PowerShell. Zodra deze is voltooid, kunt u PowerShell sluiten en de configuratie is voltooid.

Nu moeten de services worden gestart automatisch toestaan de agent om te controleren en gegevens te verzamelen. Als u niet alle minimumvereisten die worden beschreven in de vorige gedeelten voldaan, weergegeven waarschuwingen in het venster PowerShell. Zorg ervoor dat de [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) uitvoert voordat u de agent installeert. De volgende schermafbeelding is een voorbeeld van deze fouten.

![Controleer of Azure AD Connect servicestatus voor AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Als u wilt controleren of dat de-agent is geïnstalleerd, zoekt u de volgende services op de domeincontroller.

- Azure AD Connect inzichten gezondheidszorg AD DS
- Azure AD Connect systeemstatus AD DS-Service controleren

Als u de configuratie voltooid, moeten al deze services worden uitgevoerd. Anders worden ze gestopt totdat de configuratie voltooid is.

![Controleer of Azure AD Connect systeemstatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Verbinding maken met de installatie van de Azure AD met systeemstatus Agent voor AD DS van de Server Core.
Na de installatie van de .exe-bestand, kunt u het registratieproces uitvoeren met behulp van de volgende PowerShell-opdracht:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Azure AD verbinding systeemstatus agenten als u wilt gebruiken, HTTP-Proxy configureren
U kunt Azure AD verbinding systeemstatus kunt vinden om te werken met een HTTP-Proxy configureren.

>[AZURE.NOTE]
- Gebruik 'Netsh WinHttp zet ProxyServerAddress' wordt niet ondersteund, zoals de agent System.Net gebruikt om te leiden webaanvragen in plaats van Microsoft Windows HTTP Services.
- De geconfigureerde Http-Proxy-adres wordt gebruikt voor Pass Through-versleutelde Https-berichten.
- Geverifieerde proxy's (via HTTPBasic) worden niet ondersteund.

### <a name="change-health-agent-proxy-configuration"></a>Configuratie van systeemstatus-Agent-Proxy wijzigen
U hebt de volgende opties voor het configureren van Azure AD verbinding systeemstatus Agent om een HTTP-Proxy te gebruiken.

>[AZURE.NOTE]Alle Azure AD verbinding systeemstatus Agent services moeten opnieuw worden gestart, in de volgorde voor de proxy-instellingen zijn bijgewerkt. Voer de volgende opdracht:<br>
Restart-Service AdHealth *

#### <a name="import-existing-proxy-settings"></a>Bestaande proxy-instellingen importeren

##### <a name="import-from-internet-explorer"></a>Importeren vanuit Internet Explorer
Internet Explorer HTTP-proxy-instellingen kunnen worden geïmporteerd, om te worden gebruikt door de Azure AD verbinding systeemstatus kunt vinden. Voer de volgende PowerShell-opdracht op elk van de servers met de systeemstatus-agent:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importeren uit WinHTTP
WinHTTP proxy-instellingen kunnen worden geïmporteerd, om te worden gebruikt door de Azure AD verbinding systeemstatus kunt vinden. Voer de volgende PowerShell-opdracht op elk van de servers met de systeemstatus-agent:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Proxyadressen handmatig opgeven
Klik op elk van de servers met de systeemstatus-Agent, door de volgende PowerShell-opdracht uit te voeren, kunt u handmatig een proxyserver opgeven:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Voorbeeld: *Set-AzureAdConnectHealthProxySettings - HttpsProxyAddress myproxyserver: 443*

- '-adres kunt werken met de naam van een DNS-omgezet server of een IPv4-adres
- "poort" kan worden weggelaten. Indien weggelaten en vervolgens als standaardpoort 443 wordt gekozen.

#### <a name="clear-existing-proxy-configuration"></a>Configuratie van bestaande proxy wissen
U kunt de configuratie van de bestaande proxy wissen door de volgende opdracht uit te voeren:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Lees huidige proxy-instellingen
U kunt de geconfigureerde proxy-instellingen lezen door de volgende opdracht uit te voeren:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Test-connectiviteit met Azure AD gezondheidszorg verbinding maken
Het is mogelijk dat problemen zich kunnen voordoen die ertoe leiden dat de status verbinding maken met Azure AD-agent verbroken met de status verbinding maken met Azure AD-service. Het gaat hierbij om netwerkproblemen, machtiging problemen of verschillende redenen.

Als de-agent niet mogelijk om gegevens te sturen naar de status verbinding maken met Azure AD-service voor meer dan twee uur is, deze wordt aangegeven met de volgende melding in de portal: "gegevensverbinding voor webservices status is niet up-to-date." U kunt bevestigen of de desbetreffende Azure AD Connect systeemstatus agent kunnen gegevens uploaden naar de status verbinding maken met Azure AD-service door de volgende PowerShell-opdracht uit te voeren:

    Test-AzureADConnectHealthConnectivity -Role ADFS

De parameter rol kost momenteel de volgende waarden:

- ADFS
- Synchroniseren
- WORDT TOEGEVOEGD

U kunt de vlag - ShowResults in de opdracht weergeven gedetailleerde logboeken. Gebruik het volgende voorbeeld:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Als u wilt het hulpmiddel connectivity gebruikt, moet u eerst de registratie agent uitvoeren. Als u niet kunt voltooien van de registratie agent, zorg ervoor dat u hebt voldoet aan alle [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) Azure AD verbinding status. Deze toets connectivity is uitgevoerd al dan niet standaard tijdens de registratie van agent.



## <a name="related-links"></a>Verwante koppelingen

* [Azure AD Connect systeemstatus](active-directory-aadconnect-health.md)
* [Azure AD Connect systeemstatus bewerkingen](active-directory-aadconnect-health-operations.md)
* [Gebruik van Azure AD status verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Gebruik van Azure AD status verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect systeemstatus Veelgestelde vragen](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect systeemstatus versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
