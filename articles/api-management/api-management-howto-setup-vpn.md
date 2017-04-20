<properties
    pageTitle="VPN-verbindingen in Azure API Management instellen"
    description="Informatie over het instellen van een VPN-verbinding in Azure API Management en access webservices door de werkmap."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>VPN-verbindingen in Azure API Management instellen

API Management van VPN-ondersteuning kunt u verbinding maken met uw API Management gateway bij naar een Azure Virtual Network (klassieke). Hierdoor API Management klanten veilig verbinding maken met hun backend-webservices die zijn on-premises implementatie of anderszins niet toegankelijk zijn voor de openbare internet.

>[AZURE.NOTE] Azure API Management werkt met klassieke VNETs. Zie voor informatie over het maken van een klassieke VNET [maken een virtueel netwerk (klassieke) met behulp van de Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Zie voor informatie over het klassieke VNETs verbinden met ARM VNETS, [verbinding maken klassieke VNets naar nieuwe VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Inschakelen VPN-verbindingen

>VPN-verbinding is alleen beschikbaar in de lagen **Premium** en **ontwikkelaars** . Als u wilt deze, open uw API Management-service in de [Klassieke Azure-Portal][] en open vervolgens het tabblad **schaal** . Selecteer onder de sectie **Algemeen** de Premium-laag en klik op opslaan.

Om in te schakelen VPN connectivity, open uw API Management-service in de [Klassieke Azure-Portal][] en Ga naar het tabblad **configureren** . 

Klik onder de sectie VPN door **VPN-verbinding** naar **op**te gaan

![Tabblad API Management exemplaar van configureren][api-management-setup-vpn-configure]

Nu ziet u een lijst met alle regio's waar uw API Management-service is ingericht.

Selecteer een VPN- en subnet voor elke regio. De lijst met VPN's is ingevuld op basis van de virtuele beschikbaar in uw Azure abonnement netwerken die zijn ingesteld in het gebied dat u configureert.

![Selecteer VPN][api-management-setup-vpn-select]

Klik op **Opslaan** onder aan het scherm. Niet is mogelijk aan andere bewerkingen uitvoeren op de API Management-service in de klassieke Azure-Portal terwijl deze wordt bijgewerkt. De gateway service blijven beschikbaar en runtime oproepen niet moeten worden beïnvloed.

Houd er rekening mee dat het VIP-adres van de gateway wordt gewijzigd wanneer VPN is ingeschakeld of uitgeschakeld.

## <a name="connect-vpn"> </a>Verbinding maken met een webservice achter VPN

Nadat uw API Management-service is verbonden met het VPN, is toegang krijgen tot webservices binnen het virtuele netwerk niet anders dan de toegang tot openbare services. Typ in het lokale adres of de hostnaam van uw web-service (als een DNS-server is geconfigureerd voor het Azure Virtual Network) naar het veld **URL webservice** bij het maken van een nieuwe API of een bestaande eigenschap bewerken.

![API van VPN toevoegen][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Vereiste poorten voor API Management VPN-ondersteuning

Wanneer een exemplaar van de service API Management wordt gehost in een VNET, wordt de poorten in de volgende tabel worden gebruikt. Als deze poorten zijn geblokkeerd, is het mogelijk dat de service niet goed werkt. Met een of meer van deze poorten geblokkeerd is een probleem met de meest voorkomende onjuiste configuratie als u API Management met een VNET.

| Poorten                      | Richting        | Transportprotocol | Doel                                                          | Bron / bestemming              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Binnenkomende verbindingen          | TCP                | Clientcommunicatie naar API beheren                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Uitgaande         | TCP                | API Management afhankelijkheid van Azure opslagruimte en Azure Service Bus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Uitgaande         | TCP                | API Management afhankelijkheden voor SQL                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Uitgaande         | TCP                | API Management afhankelijkheden op Service bevinden                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Uitgaande         | AMQP               | API Management afhankelijkheid voor logboek van gebeurtenis Hub beleid            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Inkomende/uitgaande poort | UDP                | API Management afhankelijkheden op Cache bestand Vgx.                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Uitgaande         | TCP                | API Management afhankelijkheid van Azure bestandsshare voor cijfer            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Installatie van aangepaste DNS-server

API Management, is afhankelijk van een getal van Azure services. Wanneer een exemplaar van de service API Management wordt gehost in een VNET waarin een aangepaste DNS-server wordt gebruikt, moet deze kunnen worden omgezet hostnamen die gebruikmaken van deze Azure services. Volg [deze](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) richtlijnen op aangepaste DNS-instellingen.  

## <a name="related-content"> </a>Verwante inhoud


* [Een virtueel netwerk maken met een VPN-verbinding van het site-naar-site met behulp van de Portal van Azure-klassiek][]
* [Het gebruik van de controle API traceren roept in Azure API Management][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure klassieke Portal]: https://manage.windowsazure.com/

[Een virtueel netwerk maken met een VPN-verbinding van het site-naar-site met behulp van de Portal van Azure-klassiek]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Het gebruik van de controle API traceren roept in Azure API Management]: api-management-howto-api-inspector.md
