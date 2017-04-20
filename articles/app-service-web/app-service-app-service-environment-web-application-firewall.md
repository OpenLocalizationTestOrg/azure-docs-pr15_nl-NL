<properties 
    pageTitle="Een Web-toepassing Firewall (WAF) configureren voor App-Service-omgeving" 
    description="Informatie over het configureren van een firewall van de toepassing web vóór uw App Service-omgeving." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Een Web-toepassing Firewall (WAF) configureren voor App-Service-omgeving

## <a name="overview"></a>Overzicht ##
Toepassing firewalls zoals de [Barracuda WAF voor Azure](https://www.barracuda.com/programs/azure) die beschikbaar is op de beveiligde dat uw webtoepassingen door te controleren inkomende webverkeer als u wilt blokkeren SQL-injecties, op meerdere sites uitvoeren van scripts, malware uploads & DDoS-toepassing en andere aanvallen [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) -helpt met webonderdelen. Zij ook keurt de antwoorden van de back-enddatabase-endwebservers voor gegevens verlies preventie (DLP). Gecombineerd met de moeten worden geïsoleerd en aanvullende schaalbaarheid is verstrekt door App serviceomgevingen, biedt dit een ideale omgeving voor host bedrijven kritieke webtoepassingen die moeten zijn bestand tegen schadelijke aanvragen en hoog volume-verkeer is toegestaan.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Setup ##
De omgeving van onze App Service achter meerdere laden gelijkmatig exemplaren van Barracuda WAF voor dit document die we configureert, zodat alleen verkeer van de WAF de App-Service-omgeving bereiken kunt en deze niet toegankelijk zijn vanuit de DMZ worden. We hebben Azure verkeer Manager ook vóór de exemplaren van onze Barracuda WAF voor het verdelen over Azure datacenters en regio's. Een hoog niveau diagram van de configuratie eruitziet wat wordt weergegeven onder.

![Architectuur][Architecture] 

> Opmerking: Door de introductie van het [ILB ondersteuning voor App-omgeving](app-service-environment-with-internal-load-balancer.md), kunt u de ASE worden geopend vanuit het DMZ en alleen zichtbaar zijn voor het particuliere netwerk. 

## <a name="configuring-your-app-service-environment"></a>Uw App Service-omgeving configureren ##
Als u wilt configureren raadpleegt u een App-Service-omgeving [onze documentatie](app-service-web-how-to-create-an-app-service-environment.md) over het onderwerp. Nadat u een App-Service-omgeving die is gemaakt hebt, kunt u [Web Apps](app-service-web-overview.md), [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) en [Mobile-Apps](../app-service-mobile/app-service-mobile-value-prop.md) kunt maken in deze omgeving waarin alle beschermd achter de WAF we in het volgende gedeelte configureren.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Uw Barracuda WAF Cloud-Service configureren ##
Barracuda heeft een [gedetailleerde artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) over het implementeren van de WAF op een virtuele machine in Azure wordt aangegeven. Maar omdat redundantie wilt en niet een potentieel risico voorstellen, moet ten minste 2 WAF exemplaar VMs implementeren in dezelfde Cloud Service wanneer deze instructies te volgen.

### <a name="adding-endpoints-to-cloud-service"></a>Eindpunten naar Cloud Service toevoegen ###
Wanneer u 2 of meer exemplaren van WAF VM in uw Cloudservice hebt kunt u de [Azure-portal](https://portal.azure.com/) om toe te voegen HTTP en HTTPS eindpunten die worden gebruikt door de toepassing, zoals wordt weergegeven in de onderstaande afbeelding.

![Eindpunt configureren][ConfigureEndpoint]

Als uw toepassingen andere eindpunten gebruiken, moet u deze toevoegen aan deze lijst ook. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF via de Portal Management configureren ###
Barracuda WAF TCP-poorten 8000 voor configuratie via de portal management wordt gebruikt. Aangezien er meerdere exemplaren van de VMs WAF moet u de volgende stappen proberen herhalen voor elk exemplaar VM. 


> Opmerking: Als u klaar bent met WAF configuratie, verwijder het eindpunt TCP/8000 uit uw WAF VMs u uw WAF beveiligen.

Het eindpunt management toevoegen zoals weergegeven in de onderstaande afbeelding voor het configureren van uw WAF Barracuda.

![Management eindpunt toevoegen][AddManagementEndpoint]
 
Een browser gebruiken om te bladeren naar het eindpunt beheer van uw Service Cloud. Als uw Cloudservice test.cloudapp.net heet, zou u dit eindpunt openen door te bladeren naar http://test.cloudapp.net:8000. Ziet u een aanmeldingspagina zoals hieronder dat u zich kunt aanmelden met referenties die u hebt opgegeven in de WAF VM setup-fase.

![Aanmeldingspagina voor projectmanagement][ManagementLoginPage]

Wanneer u login ziet u een dashboard als de relatie in de onderstaande afbeelding die wordt eenvoudige statistieken over de beveiliging WAF presenteren.

![Beheerdashboard][ManagementDashboard]

Klikken op het tabblad Services kunt u uw WAF voor deze beveiligt-services configureren. U kunt [hun documentatie](https://techlib.barracuda.com/waf/getstarted1)raadplegen voor meer informatie over het configureren van uw WAF Barracuda. In het voorbeeld hieronder een Azure-Web-App is voor verkeer op http- en HTTPS geconfigureerd.

![Management Services toevoegen][ManagementAddServices]

> Opmerking: Afhankelijk van hoe uw toepassingen zijn geconfigureerd en welke functies worden gebruikt in uw omgeving van de Service-App, moet u stuurt verkeer voor TCP poorten dan 80 en 443, bijvoorbeeld als u IP SSL-instelling voor een Web-App. Raadpleeg [besturingselement binnenkomend verkeer documentatie van](app-service-app-service-environment-control-inbound-traffic.md) netwerk-poorten sectie voor een lijst van netwerkpoorten gebruikt in App serviceomgevingen.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Configuratie van Microsoft Azure verkeer Manager (optioneel) ##
Als uw toepassing beschikbaar in meerdere gebieden, is wordt u zou willen laden saldo vanaf deze achter [Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md). U kunt een eindpunt toevoegen in de [klassieke Azure-portal](https://manage.azure.com) met de naam van de Cloudservice voor uw WAF in het profiel verkeer Manager zoals wordt weergegeven in de onderstaande afbeelding kunt doen. 

![Verkeer Manager eindpunt][TrafficManagerEndpoint]

Als uw toepassing verificatie is vereist, zorgt u ervoor dat u enkele resource waarvoor de verificatie voor verkeer Manager pingen van de beschikbaarheid van de toepassing niet hebben. U kunt de URL onder de sectie configureren op de [portal van Azure klassieke](https://manage.azure.com) configureren zoals hieronder wordt weergegeven.

![Verkeer Manager configureren][ConfigureTrafficManager]

Als u wilt de ping-verkeer Manager opdrachten van uw WAF doorsturen naar uw toepassing, moet u setup Website vertalingen op uw WAF Barracuda voor het doorsturen van verkeer naar uw toepassing, zoals wordt weergegeven in het onderstaande voorbeeld.

![Vertalingen van de website][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Beveiligen van verkeer naar App Service-omgeving met beveiligingsgroepen netwerk (NSG)##
Volg de [besturingselement binnenkomend verkeer documentatie](app-service-app-service-environment-control-inbound-traffic.md) voor meer informatie op het verkeer naar uw App Service-omgeving van de WAF beperken alleen met behulp van het VIP-adres van uw Service Cloud. Hier volgt een voorbeeld Powershell-opdracht voor de uitvoering van deze taak voor TCP-poort 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

De SourceAddressPrefix vervangen door een virtueel IP-adres (VIP) van uw WAF-Cloudservice.

> Opmerking: De VIP van uw Cloudservice wordt gewijzigd wanneer u verwijderen en opnieuw de Cloudservice maken. Zorg ervoor dat het IP-adres in het netwerk resourcegroep bijwerken nadat u dit doet. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
