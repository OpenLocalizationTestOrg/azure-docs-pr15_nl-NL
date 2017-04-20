<properties
   pageTitle="Een toepassingsgateway maken met web application firewall met behulp van de portal | Microsoft Azure"
   description="Meer informatie over het maken van een Gateway-toepassing met web application firewall met behulp van de portal"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Een toepassingsgateway maken met web application firewall met behulp van de portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-web-application-firewall-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-web-application-firewall-powershell.md)

De firewall-toepassing (WAF) in het vak Azure-toepassingsgateway voorkomt webtoepassingen algemene aanvallen zoals SQL webweergave, uitvoeren van scripts op meerdere sites-aanvallen en sessie hijacks. Webtoepassing beschermen tegen veel van de OWASP bovenste 10 algemene web problemen.

Azure-toepassingsgateway is een laag tot en met 7 taakverdeling. Deze biedt failover, performance-routing HTTP-aanvragen tussen verschillende servers, ongeacht of deze op de cloud of on-premises implementatie zijn.
Toepassing bevat veel toepassing bezorging Controller (ADC) functies zoals HTTP van taakverdeling, cookie gebaseerde sessie affiniteit, Secure Sockets Layer (SSL) offload, aangepaste status sondes, ondersteuning voor multi-site en dergelijke.
Ga voor een volledige lijst met ondersteunde functies naar [Toepassing Gateway-overzicht](application-gateway-introduction.md)

## <a name="scenarios"></a>Scenario 's

In dit artikel zijn er twee scenario's:

In het eerste scenario leert u [web application firewall aan een bestaande application gateway toevoegen](#add-web-application-firewall-to-an-existing-application-gateway).

In het tweede scenario leert u [een toepassingsgateway met web application firewall maken](#create-an-application-gateway-with-web-application-firewall)

![Scenario-voorbeeld][scenario]

>[AZURE.NOTE] Controleert of extra configuratie van toepassingsgateway, inclusief aangepaste status, backend groep adressen en aanvullende regels zijn geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure-toepassingsgateway is een eigen subnet vereist. Zorg ervoor dat u onvoldoende adresruimte als u wilt dat meerdere subnetten laat bij het maken van een virtueel netwerk. Nadat u een toepassingsgateway naar een subnet implementeert, zijn alleen extra toepassingsgateways kunnen worden toegevoegd aan het subnet.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Web application firewall toevoegen aan een bestaande application gateway

Dit scenario wordt een bestaande application gateway ter ondersteuning van web application firewall in de modus voor preventie bijgewerkt.

### <a name="step-1"></a>Stap 1

Navigeer naar de Azure-portal, selecteert u een bestaande Application Gateway.

![Toepassingsgateway maken][1]

### <a name="step-2"></a>Stap 2

Klik op **configuratie** en bijwerken van de gateway-instellingen van toepassing. Klik op **Opslaan** als voltooid

De instellingen voor bijwerken van een bestaande toepassingsgateway ter ondersteuning van web application firewall zijn:

- **Laag** - de laag geselecteerd moet **WAF** ter ondersteuning van web application firewall
- **SKU grootte** - met deze instelling wordt de grootte van de toepassingsgateway met web application firewall, beschikbare opties zijn (**Normaal** en **groot**).
- **Status van de firewall** - met deze instelling uitschakelt of kunt web application firewall.
- **Firewallmodus** - met deze instelling is hoe web application firewall omgaat met schadelijk verkeer. Modus voor **detectie** Logboeken alleen de gebeurtenissen, waar de modus voor **preventie** gebeurtenissen logboeken en het schadelijke verkeer stopt.

![blade met eenvoudige instellingen][2]

>[AZURE.NOTE] Als u wilt bekijken weblogs toepassing firewall, diagnostische hulpprogramma's moet zijn ingeschakeld en ApplicationGatewayFirewallLog geselecteerd. Een aantal exemplaar van 1 u kunt kiezen voor testdoeleinden. Het is belangrijk te weten dat een exemplaar tellen onder twee instanties valt buiten de SLA en worden daarom niet aanbevolen. Kleine gateways zijn niet beschikbaar wanneer u een web-toepassing firewall gebruikt.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Een toepassingsgateway maken met web application firewall

Dit scenario wordt:

- Een gateway medium web application firewall-toepassing maken met twee instanties.
- Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerde CIDR tekstblok 10.0.0.0/16.
- Maak een subnet Appgatewaysubnet die wordt gebruikt 10.0.0.0/28 als de blok CIDR genoemd.
- Een certificaat voor SSL offload configureren.

### <a name="step-1"></a>Stap 1

Navigeer naar de Azure-portal, klik op **Nieuw** > **netwerkproblemen** > **Toepassingsgateway**

![Toepassingsgateway maken][1-1]

### <a name="step-2"></a>Stap 2

Vul vervolgens de basisinformatie over de toepassingsgateway. Zorg ervoor dat u kiest u **WAF** als de laag. Klik op **OK** als voltooid

De informatie die u nodig hebt voor de basisinstellingen luidt als volgt:

- **De naam van** -de naam voor de toepassingsgateway.
- **Laag** - de laag van de toepassingsgateway beschikbare opties zijn (**standaard** en **WAF**). Web application firewall is alleen beschikbaar in de laag WAF.
- **SKU grootte** - met deze instelling wordt de grootte van de toepassingsgateway, beschikbare opties zijn (**Normaal** en **groot**).
- **Exemplaar telling** - het aantal exemplaren, deze waarde moet een getal tussen **2** en **10**.
- **Resourcegroep** - resourcegroep voor het opslaan van de toepassingsgateway kunt werken met een bestaande resourcegroep of een nieuwe record.
- **Locatie** - het gebied voor de toepassingsgateway dezelfde locatie op de resourcegroep is. *De locatie is belangrijk als het virtuele netwerk en openbare IP moet op dezelfde locatie als de gateway*.

![blade met eenvoudige instellingen][2-2]

>[AZURE.NOTE] Een aantal exemplaar van 1 u kunt kiezen voor testdoeleinden. Het is belangrijk te weten dat een exemplaar tellen onder twee instanties valt buiten de SLA en worden daarom niet aanbevolen. Kleine gateways worden niet ondersteund voor web application firewall scenario's.

### <a name="step-3"></a>Stap 3

Zodra de basisinstellingen zijn gedefinieerd, wordt de volgende stap is het definiëren van het virtuele netwerk moet worden gebruikt. Het virtuele netwerk bevinden zich de toepassing die de toepassingsgateway taakverdeling voor.

Klik op **kiezen een virtueel netwerk** om te configureren van het virtuele netwerk.

![blade met instellingen voor de toepassingsgateway][3]

### <a name="step-4"></a>Stap 4

Klik op **Nieuw** in het blad **Virtual Network kiezen**

Terwijl u niet in dit scenario wordt uitgelegd, kan nu een bestaand virtuele netwerk selecteren.  Als een bestaand virtuele netwerk wordt gebruikt, is het belangrijk te weten dat het virtuele netwerk moet een lege subnet of een subnet van alleen toepassing gateway resources moet worden gebruikt.

![Kies virtueel netwerk blade][4]

### <a name="step-5"></a>Stap 5

Vul de netwerkinformatie in het blad **Virtual Network maken** zoals is beschreven in de beschrijving van de voorgaande [Scenario](#scenario) .

![Virtuele netwerk blade maken waarin gegevens zijn ingevoerd][5]

### <a name="step-6"></a>Stap 6

Nadat het virtuele netwerk is gemaakt, wordt de volgende stap is het definiëren van de front IP voor de toepassingsgateway. Nu is de keuze tussen een openbaar of een privé IP-adres van de front. De keuze afhankelijk of de toepassing internet is omlaag of interne alleen. Dit scenario wordt ervan uitgegaan met een openbare IP-adres. Als u een persoonlijk IP-adres, kan de knop **privé** worden geklikt. Een automatisch toegewezen IP-adres is gekozen of kunt u het selectievakje **kiezen een specifiek privé IP-adres** als u een handmatig.

Klik op **kiezen een openbare IP-adres**. Als een bestaande openbare IP-adres beschikbaar is op dit moment kunt kiezen, in dit scenario maakt u een nieuwe openbare IP-adres. Klik op **Nieuw**

![Kies openbare IP-adres blade][6]

### <a name="step-7"></a>Stap 7

Vervolgens Geef het openbare IP-adres een beschrijvende naam en klik op **OK**

![Openbare IP-adres blade maken][7]

### <a name="step-8"></a>Stap 8

Vervolgens kunt u de luisteraar ervan af-configuratie instellen.  Als **http** wordt gebruikt, wordt er nog niets configureren en **OK** kan worden geklikt. Als u wilt gebruiken **https** verder is configuratie vereist.

Als u wilt gebruiken **https**, is een certificaat vereist. De persoonlijke sleutel van het certificaat nodig is zodat een .pfx exporteren van het certificaat moet worden opgegeven en het wachtwoord voor het bestand.

**HTTPS**en klik op **het mappictogram naast het tekstvak **uploaden PFX certificaat** ** .
Ga naar het certificaat .pfx-bestand op uw bestandssysteem. Wanneer deze is geselecteerd, Geef een beschrijvende naam op voor het certificaat en typ het wachtwoord voor het .pfx-bestand.

Klik op **OK** om de instellingen voor de Gateway-toepassing te controleren als voltooid.

![Sectie van de configuratie luisteraar ervan af op instellingen blade][8]

### <a name="step-9"></a>Stap 9

De specifieke **WAF** -instellingen configureren.

- **Status van de firewall** - met deze instelling schakelt WAF in of uit.
- **Firewallmodus** - met deze instelling bepaalt dat de acties WAF krijgt schadelijk verkeer. Als u **detectie** kiest, wordt alleen verkeer geregistreerd.  Als u **preventie** kiest, wordt verkeer aangemeld en gestopt met een 403 niet gemachtigd.

![instellingen webtoepassing firewall][9]

### <a name="step-10"></a>Stap 10

Controleer de overzichtspagina en klik op **OK**.  De toepassingsgateway is nu in de wachtrij geplaatst en die zijn gemaakt.

### <a name="step-12"></a>Stap 12

Nadat de toepassingsgateway is gemaakt, gaat u naar deze in de portal om na te gaan met de configuratie van de toepassingsgateway.

![Toepassing Gateway resourceweergave][10]

Deze stappen maken een eenvoudige toepassingsgateway met de standaardinstellingen voor de luisteraar ervan af, backend-toepassingen, backend http-instellingen en regels. U kunt deze instellingen aanpassen aan uw implementatie zodra de inrichting van gelukt is wijzigen

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van diagnostische gegevens vastleggen, als u wilt de gebeurtenissen die zijn gevonden of is vanwege Meld met Web Application Firewall via de [Toepassing Gateway diagnostische gegevens](application-gateway-diagnostics.md)

Informatie over het maken van aangepaste status sondes via [een aangepaste status-test maken](application-gateway-create-probe-portal.md)

Leer hoe u SSL Offloading configureren en uitvoeren van de dure SSL decoderen uit uw endwebservers via [SSL Offload configureren](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png