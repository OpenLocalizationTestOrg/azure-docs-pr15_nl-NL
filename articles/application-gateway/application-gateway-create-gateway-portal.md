<properties
   pageTitle="Maken van een toepassingsgateway met behulp van de portal | Microsoft Azure"
   description="Meer informatie over het maken van een Gateway-toepassing met behulp van de portal"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Een toepassingsgateway maken met behulp van de portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Azure resourcemanager-sjabloon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure-toepassingsgateway is een laag tot en met 7 taakverdeling. Deze biedt failover, performance-routing HTTP-aanvragen tussen verschillende servers, ongeacht of deze op de cloud of on-premises implementatie zijn. Toepassingsgateway bevat veel toepassing bezorging Controller (ADC) functies zoals HTTP van taakverdeling, cookie gebaseerde sessie affiniteit, Secure Sockets Layer (SSL) offload, aangepaste status sondes, ondersteuning voor multi-site en dergelijke. Ga voor een volledige lijst met ondersteunde functies naar [Toepassing Gateway-overzicht](application-gateway-introduction.md)

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway met behulp van de Azure portal maken.

Dit scenario wordt:

- Een toepassingsgateway medium-maken met twee instanties.
- Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerde CIDR tekstblok 10.0.0.0/16.
- Maak een subnet Appgatewaysubnet die wordt gebruikt 10.0.0.0/28 als de blok CIDR genoemd.
- Een certificaat voor SSL offload configureren.

![Scenario-voorbeeld][scenario]

>[AZURE.IMPORTANT] Controleert of extra configuratie van toepassingsgateway, inclusief aangepaste status, backend groep adressen en aanvullende regels zijn geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure-toepassingsgateway is een eigen subnet vereist. Zorg ervoor dat u onvoldoende adresruimte als u wilt dat meerdere subnetten laat bij het maken van een virtueel netwerk. Nadat u een toepassingsgateway naar een subnet implementeert, zijn alleen extra toepassingsgateways kunnen worden toegevoegd aan het subnet.

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

### <a name="step-1"></a>Stap 1

Navigeer naar de Azure-portal, klik op **Nieuw** > **netwerkproblemen** > **Application Gateway**

![Toepassingsgateway maken][1]

### <a name="step-2"></a>Stap 2

Vul vervolgens de basisinformatie over de toepassingsgateway. Klik op **OK** als voltooid

De informatie die u nodig hebt voor de basisinstellingen luidt als volgt:

- **De naam van** -de naam voor de toepassingsgateway.
- **Laag** - dit is de laag van de toepassingsgateway. Twee lagen zijn beschikbaar, **WAF** en de **standaardversie**. WAF kunt de functie web application firewall.
- **SKU grootte** - met deze instelling wordt de grootte van de toepassingsgateway, beschikbare opties zijn (**klein**, **Normaal**en **groot**). *Klein is niet beschikbaar wanneer WAF laag wordt gekozen*
- **Exemplaar tellen** - het aantal exemplaren, deze waarde moet liggen tussen 2 en 10.
- **Resourcegroep** - resourcegroep voor het opslaan van de toepassingsgateway kunt werken met een bestaande resourcegroep of een nieuwe record.
- **Locatie** - het gebied voor de toepassingsgateway dezelfde locatie op de resourcegroep is. *De locatie is belangrijk als het virtuele netwerk en openbare IP moet op dezelfde locatie als de gateway*.

![blade met eenvoudige instellingen][2]

>[AZURE.NOTE] Een exemplaar telling van 1 u kunt kiezen voor testdoeleinden. Het is belangrijk te weten dat een exemplaar count onder twee instanties valt buiten de SLA en worden daarom niet aanbevolen. Kleine gateways moeten worden gebruikt voor ontwikkelaar test en niet voor productiedoeleinden.

### <a name="step-3"></a>Stap 3

Zodra de basisinstellingen zijn gedefinieerd, wordt de volgende stap is het definiëren van het virtuele netwerk moet worden gebruikt. Het virtuele netwerk bevinden zich de toepassing die de toepassingsgateway taakverdeling voor.

Klik op **kiezen een virtueel netwerk** om te configureren van het virtuele netwerk.

![blade met instellingen voor de toepassingsgateway][3]

### <a name="step-4"></a>Stap 4

Klik op **Nieuw** in het blad *Virtual Network kiezen*

Terwijl u niet in dit scenario wordt uitgelegd, kan nu een bestaand virtuele netwerk selecteren.  Als een bestaande virtueel netwerk wordt gebruikt, is het belangrijk te weten dat het virtuele netwerk moet een lege subnet of een subnet van alleen toepassing gateway resources moet worden gebruikt.

![Kies virtueel netwerk blade][4]

### <a name="step-5"></a>Stap 5

Vul de netwerkgegevens in het blad **Virtual Network maken** zoals is beschreven in de beschrijving van de voorgaande [Scenario](#scenario) .

![Virtuele netwerk blade maken waarin gegevens zijn ingevoerd][5]

### <a name="step-6"></a>Stap 6

Nadat het virtuele netwerk is gemaakt, wordt de volgende stap is het definiëren van de front IP voor de toepassingsgateway. Nu is de keuze tussen een openbaar of een privé IP-adres van de front. De keuze afhankelijk of de toepassing internet is omlaag of interne alleen. Dit scenario wordt ervan uitgegaan met een openbare IP-adres. Als u een persoonlijk IP-adres, kan de knop **privé** worden geklikt. Een automatisch toegewezen IP-adres is gekozen of kunt u het selectievakje **kiezen een specifiek privé IP-adres** als u een handmatig.

### <a name="step-7"></a>Stap 7

Klik op **kiezen een openbare IP-adres**. Als een bestaande openbare IP-adres beschikbaar is op dit moment kunt kiezen, in dit scenario maakt u een nieuwe openbare IP-adres. Klik op **Nieuw**

![Kies openbare IP-adres blade][6]

### <a name="step-8"></a>Stap 8

Vervolgens Geef het openbare IP-adres een beschrijvende naam en klik op **OK**

![Openbare IP-adres blade maken][7]

### <a name="step-9"></a>Stap 9

De laatste instelling voor het configureren van bij het maken van een toepassingsgateway is de configuratie luisteraar ervan af.  Als **http** wordt gebruikt, wordt er nog niets configureren en **OK** kan worden geklikt. Als u wilt gebruiken **https** verder is configuratie vereist.

Als u wilt gebruiken **https**, is een certificaat vereist. De persoonlijke sleutel van het certificaat nodig is zodat een .pfx-uitvoer van het certificaat en het wachtwoord moet worden verstrekt.

### <a name="step-10"></a>Stap 10

Klik op **HTTPS**, klikt u op **het mappictogram naast het tekstvak **uploaden PFX certificaat** ** .
Navigeer naar de .pfx-certificaatbestand op uw bestandssysteem. Zodra deze is geselecteerd, Geef een beschrijvende naam op voor het certificaat en typ het wachtwoord voor het .pfx-bestand.

Klik op **OK** om de instellingen voor de Gateway-toepassing te controleren als voltooid.

![Sectie van de configuratie luisteraar ervan af op instellingen blade][9]

### <a name="step-11"></a>Stap 11

Controleer de overzichtspagina en klik op **OK**.  De toepassingsgateway is nu in de wachtrij geplaatst en die zijn gemaakt.

### <a name="step-12"></a>Stap 12

Nadat de toepassingsgateway is gemaakt, gaat u naar deze in de portal om na te gaan met de configuratie van de toepassingsgateway.

![Toepassing Gateway resourceweergave][10]

Deze stappen maken een eenvoudige toepassingsgateway met de standaardinstellingen voor de luisteraar ervan af, backend-toepassingen, backend http-instellingen en regels. U kunt deze instellingen aanpassen aan uw implementatie zodra de inrichting van gelukt is wijzigen.

## <a name="next-steps"></a>Volgende stappen

Dit scenario wordt gemaakt van een standaard-gateway-toepassing. De volgende stappen zijn voor het configureren van de toepassingsgateway door leden van de groep toe te voegen, instellingen wijzigen en aanpassen van de regels in de gateway voor deze werkt alleen naar behoren.

Informatie over het maken van aangepaste status sondes via [een aangepaste status-test maken](application-gateway-create-probe-portal.md)

Leer hoe u SSL Offloading configureren en uitvoeren van de dure SSL decoderen uit uw endwebservers via [SSL Offload configureren](application-gateway-ssl-portal.md)

Informatie over het beveiligen van uw toepassingen met [Web Application Firewall](application-gateway-webapplicationfirewall-overview.md) een functie van toepassingsgateway.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
