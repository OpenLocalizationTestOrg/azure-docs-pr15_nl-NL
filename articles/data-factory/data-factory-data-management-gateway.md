<properties 
    pageTitle="Data Management Gateway voor gegevens Factory | Microsoft Azure"
    description="Een data gateway instellen om gegevens te verplaatsen tussen de on-premises en de cloud. Data Management Gateway in fabriek van Azure-gegevens gebruiken om uw gegevens te verplaatsen." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="jingwang"/>

# <a name="data-management-gateway"></a>Data Management Gateway
De Data Management Gateway is een client-agent die u moet installeren in uw on-premises omgeving om te kopiëren gegevens tussen cloud en on-premises gegevens winkels. De on-premises implementatie gegevens winkels worden ondersteund door gegevens Factory worden vermeld in de sectie [ondersteunde gegevensbronnen](data-factory-data-movement-activities.md##supported-data-stores) . 

> [AZURE.NOTE] Gateway ondersteunt momenteel alleen de kopie en opgeslagen procedure activiteit in Data Factory. Het is niet mogelijk is om te gebruiken van de gateway van een aangepaste activiteit voor toegang tot on-premises gegevensbronnen. 

In dit artikel is een aanvulling op de stapsgewijze instructies in de [gegevens verplaatsen tussen de on-premises en cloud gegevensopslag](data-factory-move-data-between-onprem-and-cloud.md) artikel. In de stapsgewijze instructies maakt u een pijplijn waarin de gateway voor gegevens uit een lokale SQL Server-database verplaatsen naar een Azure blob wordt gebruikt. In dit artikel vindt u gedetailleerde uitgebreide informatie over de Data Management Gateway.   

## <a name="overview"></a>Overzicht

### <a name="capabilities-of-data-management-gateway"></a>Mogelijkheden van Data Management gateway
Data Management Gateway biedt de volgende mogelijkheden:

- Model van on-premises gegevensbronnen en gegevensbronnen in dezelfde gegevens fabriek cloud en gegevens verplaatsen.
- Een door één venster voor controle en beheer met inzicht in de status van gateway van het blad gegevens Factory hebben.
- Toegang tot on-premises gegevensbronnen veilig beheren.
    - Geen wijzigingen die zijn vereist voor bedrijfsfirewall. Gateway wordt alleen uitgaande HTTP gebaseerde verbindingen met het openen van internet.
    - Referenties voor uw on-premises implementatie gegevens winkels met uw certificaat versleutelen.
- Gegevens efficiënt verplaatsen: gegevens is overgedragen parallel robuuste door onregelmatige netwerkproblemen met auto logica opnieuw.

### <a name="command-flow-and-data-flow"></a>Opdracht-mailstroom en gegevensstroom
Wanneer u een activiteit in een kopie gegevens te kopiëren tussen on-premises en cloud gebruikt, wordt de activiteit een gateway gegevens van de on-premises gegevensbron overbrengen naar de cloud en omgekeerd.

Hier op hoog niveau gegevens stroom voor en overzicht van stappen voor het kopiëren met gegevensgateway: ![gegevensstroom met gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1.  Gegevens ontwikkelaars Hiermee maakt u een gateway voor een Azure-gegevens Factory via de [portal van Azure](https://portal.azure.com) of de [PowerShell-Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx). 
2.  Gegevens ontwikkelaars Hiermee maakt u een gekoppelde service voor een on-premises implementatie gegevensopslag door het opgeven van de gateway. Als onderdeel van het instellen van de gekoppelde service, worden gegevens ontwikkelaars de toepassing instelling referenties gebruikt voor verificatietypen en referenties opgeven.  Het dialoogvenster van de toepassing instelling referenties communiceert met de gegevensopslag verbinding en de gateway bij naar het opslaan van referenties te testen.
3. Gateway versleutelt de referenties met het certificaat dat is gekoppeld aan de gateway (gegeven door de ontwikkelaar gegevens), voordat u de referenties opslaat in de cloud.
4. Factory-gegevensservice communiceert met de gateway voor plannen en beheren van taken via een besturingselement-kanaal waarin een gedeelde Azure service bus wachtrij. Wanneer een project kopiëren activiteit worden gestart moet, wachtrijen Data Factory het verzoek samen met referentiegegevens. Gateway weer uit de taak na de wachtrij polling.
5.  De gateway ontsleutelt de referenties met hetzelfde certificaat en maakt vervolgens verbinding met de on-premises implementatie gegevensopslag met BEGINLETTERS verificatietype en referenties.
6.  In een cloudopslag, of vice versa afhankelijk van hoe de activiteit kopiëren is geconfigureerd in de pijplijn gegevens, de gateway gegevens opgehaald uit een on-premises implementatie-winkel. Voor deze stap communiceert de gateway rechtstreeks met cloudgebaseerde opslag-services zoals Azure-blobopslag via een beveiligde (HTTPS)-kanaal.

### <a name="considerations-for-using-gateway"></a>Overwegingen bij het gebruik van de gateway
- Een enkel exemplaar van Data Management Gateway kan worden gebruikt voor meerdere on-premises implementatie-gegevensbronnen. Echter **een exemplaar van één gateway is gekoppeld aan slechts één Azure gegevens factory** en met een andere gegevens fabriek kan worden gedeeld.
- U kunt **slechts één exemplaar van Data Management Gateway** is geïnstalleerd op één computer hebben. Stel dat u hebt twee gegevens-factory's die moeten toegang hebben tot de on-premises gegevensbronnen, moet u gateways installeren op twee on-premises implementatie-computers. Met andere woorden, is een gateway gekoppeld aan een specifieke gegevens factory
- De die wordt **gateway niet hoeft te worden op dezelfde computer als de gegevensbron**. Echter minder gateway dichter naar de gegevensbron met tijd voor de gateway verbinding maken met de gegevensbron. Het is raadzaam dat u de gateway installeren op een computer die verschilt van degene die als host on-premises gegevensbron fungeert. Wanneer de gateway en de gegevensbron op verschillende computers, zijn de gateway niet geconfigureerd voor resources met de gegevensbron.
- U kunt **meerdere gateways op verschillende computers verbinding maken met dezelfde lokale gegevensbron**hebben. Bijvoorbeeld er twee gateways voor twee bedrijven van de gegevens maar dezelfde lokale gegevensbron is geregistreerd bij beide de gegevens factory's.
- Als u al een gateway is geïnstalleerd op uw computer voor een **Power BI** -scenario, installeert u een **aparte gateway voor Factory van Azure-gegevens** op een andere computer.
- Gateway moet worden gebruikt, zelfs wanneer u **ExpressRoute**gebruiken.
- Uw gegevensbron behandelen als een lokale gegevensbron (die zich achter een firewall) zelfs wanneer u **ExpressRoute**gebruiken. Gebruik de gateway tot stand brengen van de connectiviteit tussen de service en de gegevensbron.
- U moet **de gateway gebruiken** , zelfs als het gegevensarchief zich in de cloud op een **Azure IaaS VM**. 

## <a name="installation"></a>Installatie

### <a name="prerequisites"></a>Vereisten voor
- **De ondersteunde besturingssystemen** zijn Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Installatie van de Data Management Gateway op een domeincontroller is momenteel niet ondersteund.
- .NET framework 4.5.1 of hoger is vereist. Als u gateway op een computer met Windows 7 installeert, installeert u .NET Framework 4.5 of hoger. Zie [Systeemvereisten voor .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) voor meer informatie. 
- De aanbevolen **configuratie** voor de gatewaycomputer is ten minste 2 GHz, 4 cores 8 GB RAM en schijf 80 GB.
- Als de host in slaapstand, reageert de gateway niet op gegevensaanvragen. Een juiste **energiebeheerschema** op de computer daarom configureren voordat u de gateway installeert. Als de computer is geconfigureerd voor slaapstand, vraagt de gateway-installatie een bericht.
- U moet een beheerder op de computer installeren en configureren van de Data Management Gateway is. U kunt extra gebruikers toevoegen aan de **Data Management Gateway gebruikers** lokale Windows-groep. De leden van deze groep kunnen de Data Management Gateway Configuration Manager gebruiken voor het configureren van de gateway. 

Als kopie activiteit wordt uitgevoerd op een specifieke frequentie plaatsvinden, volgt het Resourcegebruik (CPU, geheugen) op de computer ook hetzelfde patroon met piek en niet-actieve tijden. Resourcegebruik is ook intensief afhankelijk van de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie taken uitgevoerd worden, ziet u Resourcegebruik omhoog gaan momenten piek. 

### <a name="installation-options"></a>Opties voor installatie
Data Management Gateway kan worden geïnstalleerd op de volgende manieren: 

- Als u installatiepakket door te downloaden van een MSI-bestand vanuit het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717).  Het MSI-bestand kan ook worden gebruikt bestaande Data Management Gateway naar de nieuwste versie, met alle instellingen behouden upgrade uit te voeren.
- Door te klikken op de koppeling voor **downloaden en installeren van de gegevensgateway** onder handmatige instelling of **rechtstreeks op deze computer installeren** onder EXPRESS-instelling. Zie [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het gebruik van setup express. De handmatige stap gaat u naar het Downloadcentrum.  Er zijn de instructies voor het downloaden en installeren van de gateway van download center in het volgende gedeelte. 

### <a name="installation-best-practices"></a>Aanbevolen procedures voor installatie:
1.  Energiebeheerschema op de hostcomputer voor de gateway zodanig configureren dat de computer niet slaapstand. Als de host in slaapstand, reageert de gateway niet op gegevensaanvragen.
2.  Back-up van het certificaat dat is gekoppeld aan de gateway.

### <a name="install-gateway-from-download-center"></a>Gateway installeren vanuit het Downloadcentrum
1. Navigeer naar [de downloadpagina van Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717). 
2. Klik op **downloaden**, selecteer de juiste versie (**32-bits** en **64-bits**) en klik op **volgende**. 
3. De **MSI** rechtstreeks uitvoeren of opslaan op de harde schijf en uitvoeren.
4. Op **de welkomstpagina,** Klik op **volgende**een **taal** selecteren.
5. **Accepteren** de gebruiksrechtovereenkomst en klik op **volgende**. 
6. Selecteer de **map** voor het installeren van de gateway en klik op **volgende**. 
7. Klik op de pagina **Gereed om te installeren** op **installeren**. 
8. Klik op **Voltooien** om installatie te voltooien.
9. De toets krijgen van de Azure-portal. Zie de volgende sectie voor stapsgewijze instructies. 
10. Voer de volgende stappen uit op de pagina **gateway registreren** van **Data Management Gateway Configuration Manager** op uw computer: 
    1. De sleutel in de tekst te plakken.
    2. Klik desgewenst op **gateway-sleutel wordt weergegeven** als u wilt zien van de belangrijkste tekst.
    3. Klik op **registreren**. 

### <a name="register-gateway-using-key"></a>Register-gateway met toets

#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Als u een logische gateway al hebt gemaakt in de portal
Als u wilt maken van een gateway in de portal en de toets krijgen van het blad **configureren** , stappen uit de stapsgewijze instructies in het artikel [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) .    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Als u de logische gateway al hebt gemaakt in de portal
1. Ga naar het blad **Gegevens Factory** in Azure-portal en klik op de tegel **Gekoppelde Services** .

    ![Gegevens Factory blade](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Selecteer in het blad **Gekoppelde Services** , de logische **gateway** die u hebt gemaakt in de portal. 

    ![logische gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
2. Klik in het blad **Data Gateway** op **downloadt en installeert u gegevensgateway**.

    ![Downloadkoppeling in de portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
3. Klik in het blad **configureren** op de **toets opnieuw**. Klik op Ja in het waarschuwingsbericht voor lezen en deze zorgvuldig.

    ![Toets opnieuw te maken](media/data-factory-data-management-gateway/recreate-key-button.png)
4. Klik op de knop kopiëren naast de toets. De toets wordt gekopieerd naar het Klembord.
    
    ![Toets kopiëren](media/data-factory-data-management-gateway/copy-gateway-key.png) 

### <a name="system-tray-icons-notifications"></a>Pictogrammen in het systeemvak / meldingen
De volgende afbeelding ziet u enkele van de pictogrammen in systeemvak die u ziet. 

![pictogrammen in het systeemvak](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Als u de cursor op het systeem/Meldingsbericht voor een pictogram systeemvak plaatst, ziet u meer informatie over de status van de gateway/update-bewerking in een pop-upvenster.

### <a name="ports-and-firewall"></a>Poorten en firewall
Er zijn twee firewalls, moet u rekening houden: **bedrijfsfirewall** waarop de centrale router van de organisatie, en **Windows firewall** geconfigureerd als een daemon op de lokale computer waarop de gateway is geïnstalleerd.  

![firewalls](./media/data-factory-data-management-gateway/firewalls.png)

Op het niveau van de bedrijfsfirewall, moet u de volgende domeinen en uitgaande poorten configureren:

| Domeinnamen | Poorten | Beschrijving |
| ------ | --------- | ------------ |
| *. servicebus.windows.net | 443, 80 | Listeners op Service Bus Relay via TCP (hiervoor 443 voor toegangsbeheer token acquisition) | 
| *. servicebus.windows.net | 9350-9354, 5671 | Optionele service bus relay via TCP | 
| *. core.windows.net | 443 | HTTPS | 
| *. clouddatahub.net | 443 | HTTPS | 
| Graph.Windows.NET | 443 | HTTPS |
| Login.Windows.NET | 443 | HTTPS | 

Op windows firewall niveau, zijn deze uitgaande poorten normaal gesproken ingeschakeld. Als u niet het geval is, kunt u de domeinen en poorten hiervan op de gatewaycomputer.

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Gegevens uit een bron-gegevensopslag naar een gegevensopslag sink kopiëren

Zorg ervoor dat de firewallregels correct zijn ingeschakeld op de bedrijfsfirewall, Windows firewall op de gatewaycomputer en de gegevens opslaan zelf. Inschakelen van deze regels, kunt de gateway bij naar de verbinding maken met beide bron en succes vangen. Regels voor elke gegevensopslag die deel uitmaakt van de kopieeropdracht inschakelen.

Bijvoorbeeld als u wilt kopiëren vanuit **een on-premises implementatie gegevensopslag naar een sink Azure SQL-Database of een Azure SQL Data Warehouse vangen**, volgt u de volgende stappen uit: 

- Uitgaande **TCP** -communicatie toestaan op poort **1433** voor zowel Windows firewall uit en bedrijfsfirewall
- De firewallinstellingen van Azure SQL server om het IP-adres van de gatewaycomputer toevoegen aan de lijst met toegestane IP-adressen configureren. 


### <a name="proxy-server-considerations"></a>Overwegingen bij proxy-server
Als uw zakelijke netwerkomgeving een proxyserver gebruikt voor toegang tot internet, configureert u Data Management Gateway als u wilt gebruiken, passende proxy-instellingen. U kunt de proxy instellen tijdens de registratie van de eerste fase. 

![Proxy instellen tijdens de registratie](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Gateway maakt gebruik van de proxyserver verbinding maken met de cloudservice. Klik op koppeling **wijzigen** tijdens de eerste configuratie. U ziet het dialoogvenster **proxy-instellingen** .

![Set-proxy met config manager](media/data-factory-data-management-gateway/SetProxySettings.png)

Er zijn drie configuratieopties: 

- **Gebruik geen proxy**: Gateway eventuele proxy niet expliciet gebruiken om te verbinden met cloudservices.
- **Gebruik systeem proxy**: Gateway wordt gebruikt voor de proxy-instelling die is geconfigureerd in diahost.exe.config.  Als geen proxy is geconfigureerd in diahost.exe.config, maakt gateway verbinding met cloudservice rechtstreeks zonder tussenkomst proxy.
- **Aangepaste proxy gebruiken**: de HTTP-proxy-instelling wilt gebruiken voor de gateway, in plaats van het gebruik van configuraties in diahost.exe.config configureren.  Adres en poort zijn vereist.  Gebruikersnaam en wachtwoord zijn optioneel afhankelijk van uw proxy verificatie-instelling.  Alle instellingen zijn versleuteld met het referentiecertificaat van de gateway en lokaal opgeslagen op de gatewaycomputer host.

De Data Management Gateway Host-Service automatisch opnieuw gestart nadat u de bijgewerkte proxy-instellingen hebt opgeslagen. 

Nadat de gateway is geregistreerd, als u wilt weergeven of proxy-instellingen bijwerken, gebruikt u Data Management Gateway Configuration Manager. 

1. Start de Data Management Gateway Configuration Manager.
2. Overschakelen naar het tabblad **Instellingen** .
3. Klik op de koppeling **wijzigen** in de sectie **HTTP-Proxy** aan het dialoogvenster **HTTP-Proxy instellen** starten.  
4. Nadat u op de knop **volgende** hebt geklikt, ziet u een waarschuwing waarin wordt gevraagd uw toestemming voor de proxy-instellingen opslaan en de Gateway Host-Service opnieuw starten.

U kunt bekijken en bijwerken van HTTP-proxy via Configuration Manager-hulpmiddel. 

![Set-proxy met config manager](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [AZURE.NOTE] Als u een proxyserver met NTLM-verificatie hebt ingesteld, wordt de Gateway Host Service uitgevoerd onder de domeinaccount. Als u het wachtwoord voor het domeinaccount later wijzigt, moet bijwerken configuratie-instellingen voor de service en hiervan het programma opnieuw. Vanwege deze vereiste, wordt u aangeraden dat u een domeinaccount speciale gebruiken voor toegang tot de proxyserver waarvoor u het wachtwoord regelmatig bijwerken.

### <a name="configure-proxy-server-settings-in-diahostexeconfig"></a>Proxyserverinstellingen configureren in diahost.exe.config
Als u **Gebruik systeem proxy** -instellingen voor de HTTP-proxy selecteert, wordt in gateway maakt gebruik van de proxy-instelling in diahost.exe.config.  Als geen proxy is opgegeven in diahost.exe.config, maakt gateway verbinding met cloudservice rechtstreeks zonder tussenkomst proxy. De volgende procedure bevat instructies voor het bijwerken van het configuratiebestand. 

1.  Controleer in File Explorer, een veilige kopie van C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config back-up van het oorspronkelijke bestand.
2.  Starten Notepad.exe uitvoeren als beheerder en open tekstbestand "C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. U vindt het standaardlabel voor system.net, zoals wordt weergegeven in de volgende code:

            <system.net>
                <defaultProxy useDefaultCredentials="true" />
            </system.net>   

    Vervolgens kunt u proxy-server-gegevens toevoegen, zoals wordt weergegeven in het volgende voorbeeld:

            <system.net>
                  <defaultProxy enabled="true">
                        <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
                  </defaultProxy>
            </system.net>

    Aanvullende eigenschappen zijn toegestaan in de tag proxy om op te geven van de vereiste instellingen zoals scriptLocation. Raadpleeg [proxy Element (netwerkinstellingen)](https://msdn.microsoft.com/library/sa91de1e.aspx) op de syntaxis van de.

            <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Het configuratiebestand opslaan in de oorspronkelijke locatie en vervolgens de Host van Data Management Gateway-service, die u de wijzigingen neemt opnieuw. De service opnieuw starten: services van Configuratiescherm vanuit het Configuratiescherm of de **Data Management Gateway Configuration Manager** gebruiken > Klik op de knop **Service stoppen** en klik op de **Service starten**. Als de service niet wordt gestart, is deze waarschijnlijk dat een onjuiste syntaxis van de XML-code is toegevoegd in het configuratiebestand die is bewerkt.     

Naast deze punten moet u ook om ervoor te zorgen dat Microsoft Azure is in "witte" lijst van uw bedrijf. De lijst met geldige Microsoft Azure IP-adressen kan worden gedownload vanuit het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mogelijke symptomen voor firewall en proxy server kwesties in verband met
Als er fouten overeen met de volgende procedures optreden, is deze waarschijnlijk vanwege onjuiste configuratie van de server firewall of proxyserver, die worden geblokkeerd gateway geen verbinding maken met gegevens Factory om zichzelf te verifiëren. Verwijzen naar de vorige sectie om te zorgen dat uw firewall en proxyserver correct zijn geconfigureerd.

1.  Wanneer u probeert om de gateway te registreren, het volgende foutbericht: "kan niet registreren van de gateway-sleutel. Voordat u probeert om te registreren om de gateway-sleutel opnieuw bevestigen dat de Data Management Gateway verbonden is en de Data Management Gateway Host-Service wordt gestart."
2.  Wanneer u Configuration Manager opent, ziet u de status als "Verbroken" of "Verbinding maken." Bij het weergeven van gebeurtenislogboeken van Windows, onder "Logboeken" > "Toepassingen en servicelogboeken" > 'Data Management Gateway', ziet u foutberichten zoals het volgende foutbericht weergegeven:`Unable to connect to the remote server` 
    `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Poort 8050 voor referentieversleuteling openen 
De binnenkomende poort **8050** relay referenties met de gateway bij het instellen van een on-premises gebruikt voor de toepassing van de **Instelling referenties** gekoppeld-service in de portal van Azure. Tijdens de installatie van de gateway wordt standaard de Data Management Gateway-installatie geopend op de gatewaycomputer.
 
Als u een firewall van derden gebruikt, kunt u handmatig de poort 8050 openen. Als u tijdens de installatie van de gateway firewall probleem ondervindt, kunt u proberen met de volgende opdracht voor het installeren van de gateway zonder het configureren van de firewall.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Als u niet voor de poort 8050 op de gatewaycomputer openen kiest, gebruikt u regelingen dan gegevens store-referenties configureren met behulp van de **Instelling referenties** -toepassing. U kunt bijvoorbeeld [Nieuw AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet. Zie [instelling referenties en beveiliging](#set-credentials-and-securityy) sectie op hoe de referenties voor het opslaan van gegevens kan worden ingesteld.

## <a name="update"></a>Update 
Standaard worden Data Management Gateway wordt automatisch bijgewerkt wanneer er een nieuwere versie van de gateway is beschikbaar. De gateway is niet bijgewerkt totdat de geplande taken kunt uitvoeren. Geen verdere taken worden verwerkt door de gateway totdat de updatebewerking is voltooid. Als de update is mislukt, is gateway hersteld de oude versie. 

Ziet u de geplande update-tijd in de volgende locaties:

- Het gateway-eigenschappen-blad in de portal van Azure.
- Startpagina van de Data Management Gateway Configuration Manager
- Systeem systeemvak Meldingsbericht. 

Het tabblad Start van de Data Management Gateway Configuration Manager het schema update weergegeven en de laatste keer dat de gateway is geïnstalleerd/bijgewerkt. 

![Updates plannen](media/data-factory-data-management-gateway/UpdateSection.png)

U kunt de update direct af of wachten om door de gateway bij naar automatisch geplande tegelijkertijd worden bijgewerkt. Bijvoorbeeld de volgende afbeelding ziet u de melding die wordt weergegeven in de Gateway Configuration Manager samen met de knop bijwerken die u klikken kunt om te onmiddellijk wilt installeren. 

![Update in DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Het meldingsbericht in het systeemvak eruit zoals in de volgende afbeelding: 

![Systeem systeemvak bericht](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

U ziet dat de status van updatebewerking (handmatig of automatisch) in het systeemvak. Wanneer u Gateway Configuration Manager zodra starten er een bericht wordt weergegeven op de balk van de melding dat de gateway is bijgewerkt samen met een koppeling naar [Wat is nieuw onderwerp](data-factory-gateway-release-notes.md).

### <a name="to-disableenable-auto-update-feature"></a>Aan de functie voor automatisch bijwerken in-/ uitschakelen
U kunt uitschakelen/inschakelen de functie voor automatisch bijwerken door de volgende stappen uit te voeren: 

1. Start Windows PowerShell op de gatewaycomputer. 
2. Overschakelen naar de map C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.
3. De volgende opdracht uit te schakelen van de auto-update uit te schakelen aanbevelen uitvoeren.   

        .\GatewayAutoUpdateToggle.ps1  -off

4. Naar het weer inschakelen: 
    
        .\GatewayAutoUpdateToggle.ps1  -on  

## <a name="configuration-manager"></a>Configuration Manager 
Nadat u de gateway hebt geïnstalleerd, kunt u de Data Management Gateway Configuration Manager starten op een van de volgende manieren: 

- Typ in het venster **Zoeken** **Data Management Gateway** voor toegang tot dit hulpprogramma. 
- Het uitvoerbare **ConfigManager.exe** worden uitgevoerd in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 
 
### <a name="home-page"></a>Startpagina
De startpagina van Lotus kunt u de volgende acties uitvoeren: 

- De status van de weergave van de gateway (verbonden met de cloudservice enz.). 
- **Registreren** met behulp van een sleutel in de portal.
- **Stoppen** en de **Data Management Gateway Host-service** starten op de gatewaycomputer.
- **Updates plannen** op een bepaald tijdstip van de dagen.
- De datum waarop de gateway **Laatst bijgewerkt is**weergeven. 

### <a name="settings-page"></a>Pagina-instellingen
De pagina instellingen kunt u de volgende acties uitvoeren:

- Bekijken, wijzigen en exporteren **certificaat** dat wordt gebruikt door de gateway. Dit certificaat is gebruikt voor het coderen van gegevensbronreferenties.
- **HTTPS-poort** voor het eindpunt wijzigen. De gateway wordt geopend een poort voor het instellen van de referenties voor gegevensbronnen. 
- **Status** van het eindpunt
- Weergave **SSL-certificaat** wordt gebruikt voor SSL-communicatie tussen-portal en de gateway naar referenties voor gegevensbronnen.  

### <a name="diagnostics-page"></a>Pagina Diagnostische gegevens
De pagina Diagnostische gegevens kunt u de volgende acties uitvoeren:

- Uitgebreide **logboekregistratie**inschakelen, bekijkt u Logboeken in Logboeken en aanmeldingslogboeken naar Microsoft verzenden als er een fout opgetreden is.
- **Verbinding testen** met een gegevensbron.  

### <a name="help-page"></a>Help-pagina
De Help-pagina bevat de volgende gegevens:  

- Korte beschrijving van de gateway
- Versienummer
- Koppelingen naar online-help en privacyverklaring licentieovereenkomst.  

## <a name="troubleshooting"></a>Problemen oplossen

- U vindt meer informatie in het vak gateway Logboeken in gebeurtenislogboeken van Windows. U kunt ze vinden met behulp van Windows **-Logboeken** onder **toepassingen en servicelogboeken** > **Data Management Gateway**. Bij het oplossen van problemen met de gateway, zoekt u fout niveau gebeurtenissen in het logboek viewer.
- Als de gateway niet meer werkt nadat u **het certificaat worden gewijzigd**, start u de **Data Management Gateway-Service** via de Microsoft Data Management Gateway Configuration Manager hulpmiddel of Services in het Configuratiescherm. Als u nog steeds een foutbericht ziet, is het wellicht expliciete machtigingen voor de gebruiker van de Data Management Gateway-service voor toegang tot het certificaat in certificaten Manager (certmgr.msc) te geven.  De standaard-gebruikersaccount voor de service is: **NT Service\DIAHostService**. 
- Als de toepassing **Referentiebeheer** niet **versleutelen** referenties wanneer u op versleutelen klikt in gegevens Factory-Editor, controleert u of u deze toepassing worden uitgevoerd op de **gatewaycomputer**. Als dat niet zo is, wordt het uitvoeren van de toepassing op de gatewaycomputer en probeer het referenties coderen.  
- Als u gegevens opslaan verbinding of stuurprogramma gerelateerde fouten ziet, volgt u de volgende stappen uit: 
    - **Data Management Gateway Configuration Manager** starten op de gatewaycomputer.
    - Ga naar het tabblad **Diagnostische gegevens**
    - Selecteer/waarden invoeren voor velden in de groep **testverbinding met een lokale gegevensbron met deze gateway**
    - Klik op **verbinding testen** om te zien of u kunt verbinding maken met de gegevensbron on-premises implementatie van de gatewaycomputer gebruik van de informatie over de databaseverbinding en de referenties. Als de testverbinding nog steeds niet nadat u een stuurprogramma hebt geïnstalleerd, start u de gateway voor deze volgt te werk om de meest recente wijziging.  

    ![Testverbinding](./media/data-factory-data-management-gateway/TestConnection.png)

### <a name="send-gateway-logs-to-microsoft"></a>Gateway-aanmeldingslogboeken naar Microsoft verzenden
Wanneer u contact opnemen met Microsoft Support voor hulp bij het oplossen van problemen van de gateway, wordt u wordt gevraagd om te delen van de gateway-Logboeken. De versie van de gateway kunt u eenvoudig delen vereiste gateway logboeken tot en met twee muisklikken in Gateway Configuration Manager.   

1. Overschakelen naar het tabblad **Diagnostische gegevens** van de gateway configuratiemanager.
 
    ![Data Management Gateway - tabblad Diagnostische gegevens](media/data-factory-data-management-gateway/data-management-gateway-diagnostics-tab.png)
2. Klik op koppeling **Logboeken verzenden** als u wilt zien van het volgende dialoogvenster: 

    ![Data Management Gateway - logboeken verzenden](media/data-factory-data-management-gateway/data-management-gateway-send-logs-dialog.png)
3. (optioneel) Klik op **weergave logboeken** als u wilt bekijken van de logboeken.
4. (optioneel) Klik op **privacy** als u wilt bekijken van de privacyverklaring voor Microsoft online services. 
3. Zodra u tevreden bent met wat u wilt uploaden, klikt u op **Logboeken verzenden** als u wilt dat is wel logboeken verzenden vanuit de afgelopen zeven dagen naar Microsoft voor probleemoplossing zijn. Hier ziet u de status van de bewerking van de logboeken verzenden zoals wordt weergegeven in de volgende afbeelding:

    ![Data Management Gateway - verzenden logboeken status](media/data-factory-data-management-gateway/data-management-gateway-send-logs-status.png)
4. Zodra de bewerking voltooid is, ziet u een dialoogvenster, zoals wordt weergegeven in de volgende afbeelding:
    
    ![Data Management Gateway - verzenden logboeken status](media/data-factory-data-management-gateway/data-management-gateway-send-logs-result.png)
5. Het **rapport-ID** noteren en delen met Microsoft Support. Het rapport-ID wordt gebruikt om te zoeken van uw gateway-logboeken die u hebt geüpload voor probleemoplossing.  De lijst-ID wordt ook opgeslagen in de logboeken voor uw verwijzing.  U kunt vinden door te kijken naar de gebeurtenis-ID "25" en controleer de datum en tijd.
    
    ![Data Management Gateway - verzenden logboeken rapport-ID](media/data-factory-data-management-gateway/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archief gateway aanmeldt gatewaycomputer host
Er zijn enkele scenario's waarin u gateway-problemen hebt en u kunt Logboeken aan de gateway niet rechtstreeks delen: 

- U handmatig de gateway installeren en registreren van de gateway;
- U probeert te registreren van de gateway met een geregenereerde toets op configuratiemanager. 
- U probeert te logboeken verzenden en de gateway host-service kan niet worden verbonden;

U kunt in dat geval gateway Logboeken opslaan als een zipbestand en deel deze wanneer later contact opneemt met Microsoft ondersteuning. Bijvoorbeeld als u een foutbericht ontvangt tijdens het registreren van de gateway als weergegeven in de volgende afbeelding:   

![Data Management Gateway - registratie-fout](media/data-factory-data-management-gateway/data-management-gateway-registration-error.png)

Klik op **archief gateway** logboeken koppeling als u wilt archiveren en Logboeken opslaan en vervolgens het zip-bestand delen met Microsoft ondersteuning. 

![Data Management Gateway - archief Logboeken](media/data-factory-data-management-gateway/data-management-gateway-archive-logs.png)

### <a name="gateway-is-online-with-limited-functionality"></a>Gateway is online met beperkte functionaliteit 
Ziet u de status van de gateway als **online met beperkte functionaliteit** voor een van de volgende oorzaken hebben.

- Gateway geen verbinding maken met de cloudservice via service-bus.
- Cloudservice kan geen verbinding maken met gateway via service-bus.

Wanneer gateway online met beperkte functionaliteit is, kunt u mogelijk geen gebruikmaken van de Wizard Factory kopie maken van gegevens pijpleidingen voor het kopiëren van gegevens uit lokale gegevens winkels.

Resolutie/tijdelijke oplossing voor dit probleem (online met beperkte functionaliteit) is gebaseerd op of de gateway geen verbinding met cloudservice of de andere manier maken. De volgende secties vindt deze oplossingen. 

#### <a name="gateway-cannot-connect-to-cloud-service-through-service-bus"></a>Gateway geen verbinding maken met cloudservice tot en met service bus
Volg deze stappen om de gateway weer online ophalen: 

1. Uitgaande poorten 9350-9354 op beide Windows Firewall op gatewaycomputer en bedrijfsfirewall inschakelen. Zie [poorten en firewall](#ports-and-firewall) het gedeelte voor details.
2. Proxy-instellingen configureren voor de gateway. Zie de sectie [Proxy server overwegingen](#proxy-server-considerations) voor details. 

Als een tijdelijke oplossing gebruiken om gegevens Factory-Editor in Azure-portal (of) Visual Studio (of) Azure PowerShell.

#### <a name="error-cloud-service-cannot-connect-to-gateway-through-service-bus"></a>Fout: Cloudservice kan geen verbinding maken met gateway via service-bus.
Volg deze stappen om de gateway weer online ophalen:
 
1. Uitgaande poort 5671 en 9350-9354 op beide Windows Firewall op gatewaycomputer en bedrijfsfirewall inschakelen. Zie [poorten en firewall](#ports-and-firewall) het gedeelte voor details.
2. Proxy-instellingen configureren voor de gateway. Zie de sectie [Proxy server overwegingen](#proxy-server-considerations) voor details.
3. Verwijder statische IP-beperking op de proxyserver. 

Als een tijdelijke oplossing, kunt u gegevens Factory-Editor in Azure-portal (of) Visual Studio (of) Azure PowerShell.
 
## <a name="move-gateway-from-a-machine-to-another"></a>Gateway vanaf een computer naar de andere verplaatsen
Deze sectie bevat stappen voor het zwevend gateway-client vanaf één apparaat naar een andere computer. 

2. Klik in de portal Navigeer naar de **Data Factory-startpagina**en klik op de tegel **Gekoppelde Services** . 

    ![Gegevens Gateways koppeling](./media/data-factory-data-management-gateway/DataGatewaysLink.png) 
3. Selecteer de gateway in het gedeelte **GEGEVENSGATEWAYS** van het blad **Gekoppelde Services** .
    
    ![Gekoppelde Services blade met gateway geselecteerd](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
4. Klik in het blad **Data gateway** op **downloadt en installeert u gegevensgateway**.
    
    ![Gateway downloadkoppeling](./media/data-factory-data-management-gateway/DownloadGatewayLink.png) 
5. Klik op **downloaden en installeren van de gegevensgateway**in het blad **configureren** en volg de instructies voor het installeren van de gegevensgateway op de computer. 

    ![Blade configureren](./media/data-factory-data-management-gateway/ConfigureBlade.png)
6. De **Microsoft Data Management Gateway Configuration Manager** geopend houden. 
 
    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png) 
7. In het blad **configureren** in de portal, klik op **toets opnieuw** op de balk met opdrachten en klikt u op **Ja** voor het waarschuwingsbericht. Klik op **de knop kopiëren** naast belangrijke tekst die de sleutel naar het Klembord kopiëren. De gateway op de oude computer uitvalt zoals binnenkort u opnieuw de sleutel maken.  
    
    ![Toets opnieuw te maken](./media/data-factory-data-management-gateway/RecreateKey.png)
     
8. Plak de **code** in tekstvak op de pagina **Gateway registreren** van **Data Management Gateway Configuration Manager** op uw computer. (optioneel) Klik op selectievakje **gateway-sleutel wordt weergegeven** als u wilt zien van de belangrijkste tekst. 
 
    ![Kopie-toets en Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
9. Klik op **registreren** om u te registreren van de gateway bij de cloudservice.
10. Klik op het tabblad **Instellingen** op **wijzigen** , selecteert u het certificaat dat is gebruikt met de oude gateway, voert u het **wachtwoord**en klik op **Voltooien**. 
 
    ![Certificaat opgeven](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

    U kunt een certificaat exporteren uit de oude gateway door de volgende stappen uit te voeren: Data Management Gateway Configuration Manager starten op de oude computer, Ga naar het tabblad **certificaat** , klik op de knop **exporteren** en volg de instructies. 
10. Na de registratie van de gateway is voltooid ziet u de **registratie** ingesteld op **geregistreerd** en **Status** is ingesteld op **gestart** op de startpagina van de Gateway Configuration Manager. 

## <a name="encrypting-credentials"></a>Coderen van referenties 
Als u wilt versleutelen referenties in de gegevens Factory Editor, volgt u de volgende stappen uit:

1. Webbrowser op de **gatewaycomputer**starten, gaat u naar [Azure-portal](http://portal.azure.com). Zoeken naar uw gegevens factory indien nodig, gegevensbestand factory in het blad **Gegevens FACTORY** en klik vervolgens op **auteur en implementeren** om de gegevens Factory-Editor te starten.   
1. Klik op een bestaande **gekoppelde service** in de structuurweergave zien van de JSON-definitie of een gekoppelde service waarvoor een Data Management Gateway maken (bijvoorbeeld: SQL Server of Oracle). 
2. Klik in de editor JSON voor de eigenschap **gatewayName** , voer de naam van de gateway. 
3. Voer de naam van de server voor de eigenschap **Gegevensbron** in de **connectionString**.
4. Voer de naam van de database voor de eigenschap **Initiële catalogus** in de **connectionString**.    
5. Klik op de knop **versleutelen** in de balk met opdrachten waarmee wordt geopend de Klik-eenmaal **Referentiebeheer** -toepassing. Hier ziet u het dialoogvenster **Instelling Credentials** . 
    ![Dialoogvenster instelling-referenties](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
6. Voer de volgende stappen uit in het dialoogvenster **Instelling referenties** :  
    1.  **Verificatie** dat u wilt dat de gegevens Factory-service gebruiken om verbinding met de database te selecteren. 
    2.  Voer de naam van de gebruiker die toegang tot de database voor de **gebruikersnaam** -instelling heeft. 
    3.  Wachtwoord voor de gebruiker opgeven voor de instelling **wachtwoord** .  
    4.  Klik op **OK** als u wilt versleutelen referenties en sluit het dialoogvenster. 
5.  U ziet nu een eigenschap **encryptedCredential** in de **connectionString** .      
        
            {
                "name": "SqlServerLinkedService",
                "properties": {
                    "type": "OnPremisesSqlServer",
                    "description": "",
                    "typeProperties": {
                        "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                        "gatewayName": "adftutorialgateway"
                    }
                }
            }

Als u toegang hebt tot de portal van een computer die verschilt van de gatewaycomputer, moet u ervoor zorgen dat de toepassing Referentiebeheer verbinding met de gatewaycomputer maken kunt. Als de toepassing niet kan van de gatewaycomputer bereiken, kunt niet u de referenties voor de gegevensbron instellen en testen van verbinding met de gegevensbron.  

Wanneer u de toepassing **Instelling referenties** gebruikt, worden de referenties in de portal versleuteld met het certificaat dat is opgegeven in het tabblad van het **certificaat** van de **Gateway Configuration Manager** op de gatewaycomputer. 

Als u een API gebaseerde methode voor het coderen van de referenties zoekt, kunt u de [Nieuwe AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet voor het coderen van referenties. De cmdlet wordt gebruikt voor het certificaat dat gateway is geconfigureerd voor de referenties versleutelen. U toevoegen gecodeerde referenties aan het element **EncryptedCredential** van de **connectionString** in de JSON. U kunt de JSON gebruiken met de cmdlet [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) of in de gegevens Factory Editor. 

    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Er is een meer methode voor het instellen van referenties met gegevens Factory-Editor. Als u een SQL Server gekoppeld-service maken met behulp van de editor en referenties in tekst zonder opmaak ingevoerde, kan de referenties zijn versleuteld met een certificaat dat u eigenaar van de gegevens Factory-service is. Deze geen gebruikmaakt van het certificaat dat gateway is geconfigureerd voor gebruik. Deze methode is mogelijk iets sneller in sommige gevallen, is maar het minder veilig. Daarom, is het raadzaam deze methode alleen voor de ontwikkeling/testdoeleinden te volgen. 


## <a name="powershell-cmdlets"></a>PowerShell-cmdlets 
In deze sectie wordt beschreven hoe maken en registreren van een gateway met Azure PowerShell-cmdlets. 

1. Start **Azure PowerShell** beheerdersaccount. 
2. Meld u aan bij uw Azure-account door de volgende opdracht en invoeren van uw Azure referenties. 

    Login-AzureRmAccount
2. Gebruik de cmdlet **New-AzureRmDataFactoryGateway** om het maken van een logische gateway als volgt:

        $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

    **Voorbeeld van de opdrachten en parameters uitvoer**:


        PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

        Name              : MyGateway
        Description       : gateway for walkthrough
        Version           :
        Status            : NeedRegistration
        VersionStatus     : None
        CreateTime        : 9/28/2014 10:58:22
        RegisterTime      :
        LastConnectTime   :
        ExpiryTime        :
        ProvisioningState : Succeeded
        Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI

    
4. In Azure PowerShell, Ga naar de map: * *C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Uitvoeren * *RegisterGateway.ps1* * die is gekoppeld aan de lokale variabele * *$Key** zoals wordt weergegeven in de volgende opdracht uit. Dit script registreert de client-agent is geïnstalleerd op uw computer met de logische gateway die u eerder hebt gemaakt.

        PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
        
        Agent registration is successful!

    U kunt de gateway op een externe computer met behulp van de parameter IsRegisterOnRemoteMachine registreren. Voorbeeld:
        
        .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true

5. U kunt de cmdlet **Get-AzureRmDataFactoryGateway** gebruiken om de lijst met Gateways in uw fabriek gegevens. Wanneer de **Status** **online**ziet, betekent dit dat de gateway is klaar voor gebruik.

        Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

U kunt verwijderen van een gateway met de cmdlet **Verwijderen-AzureRmDataFactoryGateway** en werk de beschrijving van een gateway met de cmdlets **Set-AzureRmDataFactoryGateway** . Zie naslag voor Data Factory-Cmdlet voor syntaxis van de en andere informatie over deze cmdlets.  

### <a name="list-gateways-using-powershell"></a>Lijst met gateways via PowerShell

    Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup

### <a name="remove-gateway-using-powershell"></a>Gateway via PowerShell verwijderen
    
    Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force 


## <a name="next-steps"></a>Volgende stappen
- Zie [gegevens verplaatsen tussen de on-premises en cloud gegevensopslag](data-factory-move-data-between-onprem-and-cloud.md) artikel. In de stapsgewijze instructies maakt u een pijplijn waarin de gateway voor gegevens uit een lokale SQL Server-database verplaatsen naar een Azure blob wordt gebruikt.  
