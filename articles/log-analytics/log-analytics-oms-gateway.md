<properties
    pageTitle="Computers en apparaten verbinden met gebruik van de Gateway OMS OMS | Microsoft Azure"
    description="Verbind uw OMS beheerde apparaten en computers Operations Manager gecontroleerde met de Gateway OMS gegevens naar de OMS-service verzenden wanneer ze geen toegang tot Internet hebt."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Computers en apparaten verbinden met gebruik van de Gateway OMS OMS

In dit document wordt beschreven hoe uw OMS beheerde apparaten en computers systeem Center Operations Manager SCOM gecontroleerde gegevens naar de OMS-service verzenden kunnen wanneer ze geen toegang tot Internet hebt. De Gateway OMS kan de gegevens verzamelen en stuur dit naar de OMS-service namens hen.

De gateway is een doorsturen HTTP-proxy die ondersteuning biedt voor HTTP-tunnelmogelijkheden met de opdracht HTTP verbinding maken. De gateway kunt afhandelen, maximaal 2000 OMS gelijktijdig verbonden apparaten wanneer uitvoert op een 4-core CPU, 16-GB server waarop Windows wordt uitgevoerd.

Als u bijvoorbeeld uw enterprise of grote organisatie mogelijk servers met netwerkconnectiviteit, maar mogelijk geen internetverbinding hebt. In een ander voorbeeld hebt u mogelijk veel punt van verkoop (POS) apparaten met geen middelen om ze rechtstreeks controle uit. En in een ander voorbeeld Operations Manager de Gateway OMS kunt gebruiken als een proxyserver. In deze voorbeelden wordt kunt de Gateway OMS gegevens van de agenten die zijn geïnstalleerd op deze servers of POS apparaten om over te OMS overbrengen.

In plaats van elke afzonderlijke agent verzendende gegevens rechtstreeks naar OMS en waarvoor u een directe internetverbinding vereist, wordt in plaats daarvan agent zorgen dat alle gegevens verzonden via één computer met een internetverbinding. Deze computer is waar u installeren en gebruiken van de gateway. In dit scenario kunt u agenten installeren op computers waarop om gegevens te verzamelen. De gateway vervolgens gegevens van overzet de agenten naar OMS rechtstreeks — de gateway niet een van de gegevens die worden overgebracht analyseren.

Als u wilt controleren van de Gateway OMS en te analyseren prestaties of gebeurtenisgegevens voor de server waarop deze is geïnstalleerd, moet u de OMS-agent installeren op de computer waarop ook de gateway is geïnstalleerd.

De gateway moet hebben toegang tot Internet gegevens uploaden naar OMS. Elke agent moet ook netwerkconnectiviteit naar de bijbehorende gateway zodat gebruikersagenten kunnen automatisch gegevens van en naar de gateway overbrengen. Voor de beste resultaten kan u niet de gateway installeren op een computer die ook een domeincontroller.

Hier ziet u een diagram met gegevensstroom van directe agenten naar OMS.

![directe agent-diagram](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Hier ziet u een diagram met gegevensstroom van Operations Manager naar OMS.

![Operations Manager-diagram](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>De Gateway OMS installeren

Hiermee vervangt u deze Gateway installeren eerdere versies van de Gateway dat u (Log Analytics doorstuurservers) hebt geïnstalleerd.

Vereisten: .net Framework 4.5, Windows Server 2012 R2 SP1 en hoger

1. Download de meest recente versie van de Gateway OMS vanuit het [Microsoft Downloadcentrum](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. U start de installatie, dubbelklikt u op **OMS Gateway.msi**.
3. Klik op de pagina Welcome **volgende**.  
    ![Wizard gateway installeren](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Klik op de pagina licentieovereenkomst Selecteer **ik ga akkoord met de voorwaarden in de gebruiksrechtovereenkomst** akkoord gaan met de overeenkomst en klik op **volgende**.
5. Klik op de poorten en -proxy pagina:
    1. Typ het poortnummer TCP moet worden gebruikt voor de gateway. Instelling wordt dit poortnummer geopend vanuit Windows firewall. De standaardwaarde is 8080.
    Het geldige bereik van het poortnummer is 1-65535. Als de invoer niet in dit bereik valt, wordt een foutbericht weergegeven.
    2. Als de server waarop de gateway is geïnstalleerd, moet een proxyserver gebruiken, typ desgewenst de proxyadres waar de gateway nodig heeft om verbinding te maken. Bijvoorbeeld `http://myorgname.corp.contoso.com:80` als leeg is, de gateway probeert te rechtstreeks verbinding hebt met Internet. De gateway klikt anders de proxy. Als uw proxyserver is verificatie vereist, typt u uw gebruikersnaam en wachtwoord.
        ![Configuratie van proxy Wizard gateway](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Klik op **volgende**
6. Als u geen Microsoft-Updates is ingeschakeld, wordt in de Microsoft Update-pagina weergegeven waarin u kunt kiezen om in te schakelen van Updates van Microsoft. Een optie te kiezen en klik op **volgende**. Ga anders verder met de volgende stap.
7. Klik op de pagina doelmap laat u de standaard map **%ProgramFiles%\OMS Gateway** of typ de locatie waar u gateway installeren en klik op **volgende**.
8. Klik op de pagina gereed voor installatie, klikt u op **installeren**. Een Gebruikersaccountbeheer kan worden weergegeven vraagt machtiging te installeren. Als dat het geval is, klikt u op **Ja**.
9. Nadat de Setup is voltooid, klikt u op **Voltooien**. U kunt controleren of de service wordt uitgevoerd via de module services.msc en controleer of **OMS Gateway** wordt weergegeven in de lijst met services.  
    ![Services – OMS Gateway](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Een agent installeren op apparaten

Zie [verbinding maken met Windows-computers Log Analytics](log-analytics-windows-agents.md) voor informatie over het installeren van rechtstreeks verbonden agenten als nodig. Dit artikel wordt beschreven hoe u de agent met een installatiewizard kunt installeren of met behulp van de opdrachtregel.

## <a name="configure-oms-agents"></a>OMS gebruikersagenten configureren

Zie [de proxy en firewall instellingen configureren met Microsoft Monitoring Agent](log-analytics-proxy-firewall.md) voor informatie over het configureren van een agent een proxyserver, wat in dit geval gebruikt de gateway.

Operations Manager agenten verzenden sommige gegevens zoals Operations Manager waarschuwingen, configuratie assessment exemplaar ruimte en capaciteitsgegevens, met de Server Management. Andere grote hoeveelheden gegevens, zoals IIS-logboeken, prestaties en beveiliging worden rechtstreeks naar de Gateway OMS verzonden. Zie [oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md) voor een volledige lijst met gegevens die worden verzonden via elke kanaal.

>[AZURE.NOTE]
Als u van plan bent voor het gebruik van de Gateway met het netwerk van taakverdeling, raadpleegt u [desgewenst configureren van netwerktaakverdeling](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Een proxyserver SCOM configureren

U configureren Operations Manager als u wilt toevoegen van de gateway te treden als een proxyserver. Wanneer u de proxyconfiguratie bijwerkt, wordt de proxyconfiguratie wordt automatisch toegepast op alle agenten die rapporteren aan Operations Manager.

Om de Gateway te ondersteunen Operations Manager gebruiken, moet u beschikken over:

- Microsoft Monitoring Agent (agent versie – **8.0.10900.0** en hoger) op de Gateway-server hebt geïnstalleerd en geconfigureerd voor de OMS werkruimten die u wilt communiceren.
- De gateway moet internetconnectiviteit of niet worden verbonden met een proxyserver die doet.

### <a name="to-configure-scom-for-the-gateway"></a>SCOM configureren voor de gateway

1. Open de Operations Manager-console en klik op **verbinding** onder- **Bewerkingen Management Suite**en klik op **Proxyserver configureren**:  
    ![Operations Manager-proxyserver configureren](./media/log-analytics-oms-gateway/scom01.png)
2. Selecteer **een proxyserver voor toegang tot de bewerkingen Management-Suite** en typt u het IP-adres van de OMS Gateway-server. Zorg ervoor dat u met begint het `http://` voorvoegsel:  
    ![Operations Manager-proxyadres van de server](./media/log-analytics-oms-gateway/scom02.png)
3. Klik op **Voltooien**. Uw server Operations Manager is verbonden met uw werkruimte OMS.

## <a name="configure-network-load-balancing"></a>Netwerk van taakverdeling configureren

U kunt de gateway beschikbaarheid van netwerktaakverdeling door te maken van een cluster met configureren. Het cluster beheert verkeer van uw agenten door om te leiden van de gevraagde verbindingen vanuit het Microsoft-cmdlets voor controle kunt vinden op de knooppunten. Als u één Gateway-server uitvalt, wordt het verkeer omgeleid naar andere knooppunten.

1. Beheer openen en een cluster te maken.
2. Met de rechtermuisknop op het cluster voordat u gateways toevoegt en selecteer **Eigenschappen van Cluster.** Configureer het cluster eigen IP-adres hebt:  
    ![Beheer – IP-adressen](./media/log-analytics-oms-gateway/nlb01.png)
3. Als u wilt verbinden met een server OMS Gateway met het Microsoft Monitoring-Agent is geïnstalleerd, met de rechtermuisknop op van het cluster IP-adres en klik op **Host aan Cluster toevoegen**.  
    ![Host aan Cluster toevoegen netwerk laden taakverdeling Manager –](./media/log-analytics-oms-gateway/nlb02.png)
4. Voer in het IP-adres van de Gateway-server waarmee u verbinding wilt maken:  
    ![Beheer – netwerk Host aan Cluster toevoegen: verbinding maken](./media/log-analytics-oms-gateway/nlb03.png)
5. Klik op computers waarop geen internetverbinding hebt, zorg ervoor dat u het IP-adres van het cluster gebruiken wanneer u de **Eigenschappen van Microsoft Agent Monitoring**configureren:  
    ![Microsoft Agenteigenschappen – Proxy-instellingen controleren](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Configureren voor automatisering hybride werknemers

Hebt u automatisering hybride werknemers in uw omgeving, u volgt de handmatige, tijdelijke oplossingen om te configureren van de Gateway ter ondersteuning van deze.

In de volgende stappen die u moet weten van de Azure regio waarin het account dat automatisering zich bevindt. De locatie zoeken:

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Selecteer de automatisering Azure-service.
3. Selecteer het juiste automatisering Azure-account.
4. Klik onder **locatie**de bijbehorende regio weergeven.  
    ![Azure-portal – automatisering account locatie](./media/log-analytics-oms-gateway/location.png)

Gebruik de volgende tabellen om aan te geven van de URL voor elke locatie:

**Taak runtime gegevens URL 's**

| **locatie** | **URL** |
| --- | --- |
| Centraal Noord-Amerikaanse | ncus-jobruntimedata-productcatalogus-su1.azure-automation.net |
| West Europa | We-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Zuid-Central US | scus-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Oost-VS | eus-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Centraal Canada | CC-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Noord-Europa | nieuwe-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Zuid-Oost-Azië | zee-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Centraal India | CID-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Japan | jpe-jobruntimedata-productcatalogus-su1.azure-automation.net |
| Australië | ASE-jobruntimedata-productcatalogus-su1.azure-automation.net |

**Agent service-URL 's**

| **locatie** | **URL** |
| --- | --- |
| Centraal Noord-Amerikaanse | ncus-agentservice-productcatalogus-1.azure-automation.net |
| West Europa | We-agentservice-productcatalogus-1.azure-automation.net |
| Zuid-Central US | scus-agentservice-productcatalogus-1.azure-automation.net |
| Oost-VS | eus2-agentservice-productcatalogus-1.azure-automation.net |
| Centraal Canada | CC-agentservice-productcatalogus-1.azure-automation.net |
| Noord-Europa | nieuwe-agentservice-productcatalogus-1.azure-automation.net |
| Zuid-Oost-Azië | zee-agentservice-productcatalogus-1.azure-automation.net |
| Centraal India | CID-agentservice-productcatalogus-1.azure-automation.net |
| Japan | jpe-agentservice-productcatalogus-1.azure-automation.net |
| Australië | ASE-agentservice-productcatalogus-1.azure-automation.net |

Als uw computer is geregistreerd als een werknemer hybride automatisch voor het gebruik van de Update beheeroplossing patches, gebruikt u deze stappen:

1. De taak Runtime gegevens service-URL's toevoegen aan de lijst toegestane Host op de Gateway OMS. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Start de OMS Gateway-Service opnieuw met behulp van de volgende PowerShell-cmdlet:`Restart-Service OMSGatewayService`

Als uw computer is op-aangehouden Azure automatisering met behulp van de hybride werknemer registratie cmdlet, gebruikt u deze stappen:

1. De URL van de registratie agent toevoegen aan de lijst toegestane Host op de Gateway OMS. Bijvoorbeeld:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. De taak Runtime gegevens service-URL's toevoegen aan de lijst toegestane Host op de Gateway OMS. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Start de OMS Gateway-Service.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Handige PowerShell-cmdlets

Cmdlets voor kunt u taken die nodig zijn voor het bijwerken van de Gateway OMS configuratie-instellingen te voltooien. Voordat u deze gebruiken, moet u naar:

1. Installeer de Gateway OMS (MSI).
2. Open het venster PowerShell.
3. Als u wilt importeren de module, typt u deze opdracht:`Import-Module OMSGateway`
4. Als er geen fout opgetreden in de vorige stap, de module is geïmporteerd en de cmdlets kunnen worden gebruikt. Type`Get-Module OMSGateway`
5. Nadat u wijzigingen aanbrengt met behulp van de cmdlets, moet u ervoor zorgen dat u de Gateway-service opnieuw starten.

Als er een fout in stap 3, wordt de module is niet geïmporteerd. De fout kan optreden als PowerShell is niet vinden van de module. U vindt deze in installatiepad van de Gateway: C:\Program File\Microsoft OMS Gateway\PowerShell.

| **Cmdlet** | **Parameters** | **Beschrijving** | **Voorbeelden** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Sleutel (vereist) <br> Waarde | De configuratie van de service wordt gewijzigd | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Toets | De configuratie van de service wordt | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Adres <br> Gebruikersnaam <br> Wachtwoord | Hiermee stelt u het adres (en referentie) van relay (boven)-proxy | 1. voor het instellen van een antwoord-proxy en de referenties:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. een antwoord-proxy die geen verificatie hoeft instellen:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Schakel het selectievakje het antwoord proxyinstelling, dat wil zeggen, hoeft niet een proxy beantwoorden:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Wordt het adres van relay (boven)-proxy | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (vereist) | Telt de host van de lijst met toegestane | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (vereist) | Hiermee verwijdert u de host uit de lijst met toegestane | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | De momenteel toegestane host krijgt (alleen de lokaal geconfigureerde host mogen, neem niet automatisch gedownload toegestane hosts) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Onderwerp (vereist) | Telt het clientcertificaat onderhevig aan de lijst met toegestane | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Onderwerp (vereist) | Hiermee verwijdert u het onderwerp van client certificaat uit de lijst met toegestane | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Wordt de momenteel toegestane client certificaat onderwerpen (alleen de lokaal geconfigureerde toegestane onderwerpen, neem niet automatisch gedownload toegestane onderwerpen) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Problemen met

Het is raadzaam dat u de OMS-agent installeren op computers waarop de gateway is geïnstalleerd. U kunt de agent vervolgens gebruiken voor het verzamelen van de gebeurtenissen die door de gateway worden geregistreerd.

![Logboeken – OMS Gateway Log](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS Gateway gebeurtenis-id's en beschrijvingen**

De volgende tabel ziet u de gebeurtenis-id's en beschrijvingen voor OMS Gateway gebeurtenissen.

| **ID** | **Beschrijving** |
| --- | --- |
| 400 | Fout bij een toepassing die geen een specifieke ID |
| 401 | Verkeerde configuratie. Bijvoorbeeld: listenPort = "tekst" in plaats van een geheel getal |
| 402 | Uitzondering in parseren van TLS handshake berichten |
| 403 | Fout bij het netwerk. Bijvoorbeeld: geen verbinding maken met doelserver |
| 100 | Algemene informatie |
| 101 | Service is gestart |
| 102 | Service is gestopt |
| 103 | Een HTTP verbinding maken met de opdracht ontvangen van client |
| 104 | Niet een opdracht HTTP verbinding maken |
| 105 | Doelserver niet in de lijst met toegestane of de doelpoort is niet beveiligde poort (443) <br> <br> Zorg ervoor dat de MMA-agent op de Gateway-server en de communicatie met de Gateway zijn verbonden met de dezelfde Log Analytics-werkruimte.|
| 105 | Fout TcpConnection – ongeldige clientcertificaat: CN = Gateway <br><br> Zorg ervoor dat: <br> <br> & #149; U gebruikt een Gateway met versienummer 1.0.395.0 of hoger. <br> & #149; De MMA-agent op de Gateway-server en de communicatie met de Gateway hebt verbinding met de dezelfde Log Analytics-werkruimte. |
| 106 | Welke reden dan ook dat de TLS-sessie verdachte en afgekeurd |
| 107 | De TLS-sessie is geverifieerd. |

**Items verzamelen**

De volgende tabel ziet u de van prestatiemeteritems die beschikbaar zijn voor de Gateway OMS. U kunt de items met prestaties Monitor toevoegen.

| **Naam** | **Beschrijving** |
| --- | --- |
| OMS Gateway/actief-clientverbinding | Aantal actieve client Netwerkverbindingen (TCP) |
| OMS Gateway/fout tellen | Aantal fouten |
| OMS Gateway/verbonden Client | Aantal verbonden clients |
| OMS Gateway/weigering tellen | Aantal afwijzingen vanwege een fout in de TLS |

![OMS Gateway prestatie-items](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>U hulp nodig hebt

Wanneer u bent aangemeld bij de portal van Azure, kunt u een verzoek om hulp kunt maken met de Gateway OMS of een ander Azure-service of functie van een service.
Hulp, klik op het vraagteken symbool in de rechterbovenhoek van de portal en klik vervolgens op **nieuwe aanvraag ondersteuning**. Klik, voert u de nieuwe aanvraagformulier voor ondersteuning.

![Nieuwe verzoek voor ondersteuning](./media/log-analytics-oms-gateway/support.png)

U kunt ook feedback over OMS of Log Analytics op het [forum van Microsoft Azure feedback](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Volgende stappen

- [Gegevensbronnen toevoegen](log-analytics-data-sources.md) voor het verzamelen van gegevens uit de bronnen verbonden in uw werkruimte OMS en opslaan in de bibliotheek OMS.
