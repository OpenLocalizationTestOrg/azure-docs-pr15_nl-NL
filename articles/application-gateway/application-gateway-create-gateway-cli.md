<properties
   pageTitle="Maken van een toepassingsgateway met de CLI Azure in resourcemanager | Microsoft Azure"
   description="Meer informatie over het maken van een Gateway-toepassing met behulp van de Azure CLI in resourcemanager"
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

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Een toepassingsgateway maken met behulp van de Azure CLI

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Azure resourcemanager-sjabloon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure-toepassingsgateway is een laag tot en met 7 taakverdeling. Deze biedt failover, performance-routing HTTP-aanvragen tussen verschillende servers, ongeacht of deze op de cloud of on-premises implementatie zijn. Toepassingsgateway heeft de volgende functies van de toepassing-bezorging: HTTP verdeeld en cookie gebaseerde sessie affiniteit en Secure Sockets Layer (SSL) offload, aangepaste status sondes laden en ondersteuning voor multi-site.

## <a name="prerequisite-install-the-azure-cli"></a>Let op: De Azure CLI installeren

Als u wilt de stappen in dit artikel uitvoert, moet u [de Interface van de opdrachtregel Azure voor Mac, Linux, en Windows (Azure CLI) installeren](../xplat-cli-install.md) en moet u [zich aanmeldt bij Azure](../xplat-cli-connect.md). 

> [AZURE.NOTE] Als u geen een Azure-account hebt, moet u een. Aanmelden voor een [gratis proefversie hier](../active-directory/sign-up-organization.md)omhoog gaan.

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway met behulp van de Azure portal maken.

Dit scenario wordt:

- Een toepassingsgateway medium-maken met twee instanties.
- Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerde CIDR tekstblok 10.0.0.0/16.
- Maak een subnet Appgatewaysubnet die wordt gebruikt 10.0.0.0/28 als de blok CIDR genoemd.
- Een certificaat voor SSL offload configureren.

![Scenario-voorbeeld][scenario]

>[AZURE.NOTE] Controleert of extra configuratie van toepassingsgateway, inclusief aangepaste status, backend groep adressen en aanvullende regels zijn geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure-toepassingsgateway is een eigen subnet vereist. Zorg ervoor dat u onvoldoende adresruimte als u wilt dat meerdere subnetten laat bij het maken van een virtueel netwerk. Nadat u een toepassingsgateway naar een subnet implementeert, zijn alleen extra toepassingsgateways kunnen worden toegevoegd aan het subnet.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure

Open de **opdrachtprompt van Microsoft Azure**en meld u aan. 

    azure login

Nadat u het voorgaande voorbeeld typt, wordt een code is opgegeven. Navigeer naar https://aka.ms/devicelogin in een browser kunt doorgaan met de aanmelding.

![cmd waarin apparaat login][1]

Voer de code die u hebt ontvangen in de browser. U wordt omgeleid naar een aanmeldingspagina.

![browser code te geven][2]

Zodra de code is ingevoerd u bent aangemeld, sluit de browser om door te gaan op met het scenario.

![aangemeld][3]

## <a name="switch-to-resource-manager-mode"></a>Resourcemanager-modus

    azure config mode arm

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de toepassingsgateway maakt, is een resourcegroep gemaakt voor de toepassingsgateway. Hieronder ziet u de opdracht.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Nadat de resourcegroep is gemaakt, wordt een virtueel netwerk gemaakt voor de toepassingsgateway.  In het volgende voorbeeld is de adresruimte als 10.0.0.0/16 zoals gedefinieerd in de voorgaande scenario-notities.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Een subnet maken

Nadat het virtuele netwerk is gemaakt, wordt een subnet voor de toepassingsgateway toegevoegd.  Als u van plan bent toepassingsgateway gebruiken met een web-app in hetzelfde virtuele netwerk als de toepassingsgateway wordt gehost, moet u onvoldoende ruimte is voor een ander subnet.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Zodra het virtuele netwerk en subnet zijn gemaakt, zijn de minimumvereisten voor de toepassingsgateway zijn voltooid. Daarnaast een certificaat eerder geëxporteerde .pfx-bestand en het wachtwoord voor het certificaat zijn vereist voor de volgende stap: het IP-adressen die wordt gebruikt voor de backend zijn de IP-adressen voor uw back endserver. Deze waarden kunnen zijn privé IP-adressen in het virtuele netwerk, openbare IP-adressen of volledig gekwalificeerde domeinnamen voor uw back-end-servers.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Voor een lijst met parameters die kan worden verstrekt tijdens het maken van de volgende opdracht: **azure netwerk toepassing-gateway maken--help**.

In dit voorbeeld wordt een toepassingsgateway eenvoudige gemaakt met de standaardinstellingen voor de luisteraar ervan af, backend-toepassingen, backend http-instellingen en regels. Dit configureert SSL offload ook. U kunt deze instellingen aanpassen aan uw implementatie zodra de inrichting van gelukt is wijzigen.
Als u al uw webtoepassing gedefinieerd met behulp van de de backend-toepassingen in de voorgaande stappen, nadat gemaakt, taakverdeling begint.

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van aangepaste status sondes via [een aangepaste status-test maken](application-gateway-create-probe-portal.md)

Leer hoe u SSL Offloading configureren en uitvoeren van de dure SSL decoderen uit uw endwebservers via [SSL Offload configureren](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png