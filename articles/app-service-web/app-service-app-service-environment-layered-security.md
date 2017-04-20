<properties 
    pageTitle="Gelaagde beveiligingsarchitectuur met App Service omgevingen" 
    description="Implementatie van een gelaagde beveiligingsarchitectuur met App-serviceomgevingen." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>De beveiligingsarchitectuur van een gelaagde met App Service omgevingen implementeren

## <a name="overview"></a>Overzicht ##
 
Omdat App serviceomgevingen een geïsoleerd runtimeomgeving die is geïmplementeerd in een virtueel netwerk, kunnen ontwikkelaars een gelaagde beveiligingsarchitectuur waarin u verschillende niveaus voor netwerktoegang voor elke toepassingslaag fysiek-maken.

Een algemene wens is API back-uiteinden van algemene internettoegang verbergen en alleen toestaan API's moet worden gebeld door boven Webapps.  [Beveiligingsgroepen (NSGs) netwerk] [ NetworkSecurityGroups] kunnen worden gebruikt op subnetten App serviceomgevingen met openbare toegang tot de API-toepassingen te beperken.

De onderstaande afbeelding ziet een voorbeeld-architectuur met een WebAPI op basis van app geïmplementeerd op een App-Service-omgeving.  Drie afzonderlijke web app-exemplaren, geïmplementeerd in drie aparte App serviceomgevingen, voeren back-enddatabase met de dezelfde WebAPI-app.

![Conceptuele architectuur][ConceptualArchitecture] 

Het groene plusteken geven aan dat de beveiligingsgroep netwerk op het subnet "apiase" met binnenkomende oproepen vanuit de boven WebApps, kunt u als CRL oproepen van zichzelf.  De hetzelfde netwerk-beveiligingsgroep weigert echter expliciet toegang tot algemene binnenkomende verkeer van Internet. 

De rest van dit onderwerp worden doorlopen de benodigde stappen voor het configureren van de beveiligingsgroep van netwerk op het subnet met 'apiase'.

## <a name="determining-the-network-behavior"></a>Het gedrag van netwerk vaststellen ##
Als u wilt weten welke beveiligingsregels netwerk u nodig hebt, moet u bepalen welke netwerkclients kunnen worden bereikt van de App-Service-omgeving met de API-app en welke clients worden geblokkeerd.

Sinds [netwerk beveiligingsgroepen (NSGs)] [ NetworkSecurityGroups] zijn toegepast op subnetten, en App serviceomgevingen zijn geïmplementeerd in subnetten, de regels van een NSG toepassen op **alle** apps uitgevoerd op een App-Service-omgeving.  Met behulp van de steekproef-architectuur voor in dit artikel, als een netwerk-beveiligingsgroep wordt toegepast op het subnet met 'apiase', worden alle apps uitgevoerd op de App-omgeving "apiase" worden beveiligd met dezelfde reeks beveiligingsregels. 

- **Bepalen van de uitgaande IP-adres van boven bellers:**  Wat is het IP-adres of de adressen van de boven bellers?  Deze adressen moet access expliciet toegestaan in de NSG.  Aangezien oproepen tussen App serviceomgevingen worden beschouwd als "Internet"-gesprekken, betekent dit dat het uitgaande IP-adres toegewezen aan elk van de omgevingen drie boven App Service nodig heeft om te worden toegang is toegestaan in de NSG voor het subnet "apiase".   Voor meer informatie over het vaststellen van het uitgaande IP-adres voor apps uitgevoerd in een omgeving met App-Service Zie [Netwerkarchitectuur] [ NetworkArchitecture] artikel overzicht.
- **De back-enddatabase API-app moet zelf bellen?**  Een punt soms hoofd wordt gezien en subtiele is het scenario waarbij de back-enddatabase-toepassing moet bellen zelf.  Als een back-enddatabase API-toepassing op een App-Service-omgeving zelf bellen moet, wordt dit ook behandeld als een 'Internet'-gesprek.  In de steekproef architectuur moet hiervoor toegang vanaf het uitgaande IP-adres van de 'apiase' App-omgeving als u ook.

## <a name="setting-up-the-network-security-group"></a>Bij het instellen van de beveiligingsgroep van netwerk ##
Zodra de set uitgaande IP-adressen zijn bekend, wordt de volgende stap is om een beveiligingsgroep netwerk te maken.  Beveiligingsgroepen netwerk kunnen worden gemaakt voor beide resourcemanager virtuele netwerken, evenals klassieke virtuele netwerken die zijn gebaseerd.  De onderstaande voorbeelden wordt maken en configureren van een NSG op een klassieke virtueel netwerk via Powershell.

De omgevingen bevinden zich in Zuid centraal ons, voor de steekproef architectuur, zodat een lege NSG wordt gemaakt in die regio:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Eerst een expliciete toestaan regel wordt toegevoegd voor de infrastructuur Azure management zoals beschreven in het artikel over [binnenkomende verkeer] [ InboundTraffic] voor App-Service-omgevingen.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Vervolgens twee regels worden toegevoegd aan het toestaan van HTTP en HTTPS oproepen uit de eerste boven App Service-omgeving ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Spoelen en herhaalt u voor de tweede en derde boven App serviceomgevingen ("fe2ase" en "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ten slotte toegang verlenen tot de uitgaande IP-adres van de back-enddatabase API's App-omgeving zodat deze weer in zichzelf kunt bellen.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Geen andere beveiligingsregels voor netwerken moeten worden geconfigureerd omdat elke NSG heeft een set standaardregels die binnenkomende toegang van Internet ervoor te zorgen dat al dan niet standaard.

De volledige lijst met regels in de beveiligingsgroep van netwerk worden hieronder weergegeven.  Houd er rekening mee hoe binnenkomende toegang van alle bellers dan die expliciet toegang is verleend wordt geblokkeerd door de laatste regel, die is gemarkeerd.

![NSG configuratie][NSGConfiguration] 

De laatste stap is het NSG toepassen op het subnet met de App-omgeving "apiase".  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Alleen de drie boven App serviceomgevingen en de App-Service-omgeving met de API back-enddatabase, met de NSG toegepast op het subnet, zijn toegestaan wilt inbellen bij de omgeving "apiase".


## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie ##
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Informatie over het [netwerk beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md). 

Lidmaatschap [uitgaande IP-adressen] [ NetworkArchitecture] en App serviceomgevingen.

[Netwerk poorten] [ InboundTraffic] die worden gebruikt door de App-Service-omgevingen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
