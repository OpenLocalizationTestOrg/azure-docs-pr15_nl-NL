<properties
   pageTitle="Gateway-WebSocket programmaondersteuning | Microsoft Azure"
   description="Deze pagina bevat een overzicht van de toepassing Gateway WebSocket-ondersteuning."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Gateway-WebSocket programmaondersteuning

Toepassingsgateway biedt ondersteuning voor WebSocket over kleine gateway. Er is geen gebruiker worden geconfigureerd instelling selectief-of uitschakelen WebSocket ondersteuning. U kunt doorgaan met het gebruik van een standaard HTTPListener op poort 80/443 WebSocket verkeer ontvangen. WebSocket-verkeer is vervolgens doorgestuurd naar WebSocket ingeschakeld endserver met de juiste backend-toepassingen zoals opgegeven in de toepassing gateway regels. Een duplex communicatie tussen server en client kunt WebSocket protocol in [RFC6455](https://tools.ietf.org/html/rfc6455) gestandaardiseerd via een langdurige TCP-verbinding. Deze functie kunt voor een meer interactieve communicatie tussen webserver en mailclient gebruikt, mag bidirectionele zonder dat u polling als vereiste in HTTP gebaseerde implementaties.  WebSocket hebt laag belasting in tegenstelling tot HTTP en de dezelfde TCP-verbinding voor meerdere aanvraag/antwoorden met een efficiënter gebruik van bronnen resultaat kunt hergebruiken. WebSocket protocollen zijn ontworpen voor gebruik via traditionele HTTP-poorten van 80 en 443.

De backend-server moet reageren op toepassing gateway sondes, die worden beschreven in de sectie [systeemstatus test overzicht](application-gateway-probe-overview.md) . Toepassing gateway systeemstatus sondes zijn HTTP-/ HTTPS alleen, dit houdt in dat elke endserver moet reageren op HTTP-sondes voor toepassingsgateway voor het routeren van WebSocket verkeer naar de server.

## <a name="listener-configuration-element"></a>Luisteraar ervan af configuratie-element

Bestaande HTTPListener kan worden gebruikt voor de ondersteuning van WebSocket. Hier volgt een fragment van HttpListeners element van een steekproef-sjabloonbestand. Moet u zowel HTTP en HTTPS listeners ter ondersteuning van WebSocket en WebSocket verkeer beveiligen. Op dezelfde manier kunt u de [portal](application-gateway-create-gateway-portal.md) of [PowerShell](application-gateway-create-gateway-arm.md) voor het maken van een toepassingsgateway waarbij listeners op poort 80/443 ter ondersteuning van WebSocket-verkeer is toegestaan.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Configuratie van de regel BackendAddressPool, BackendHttpSetting en omleiden

BackendAddressPool moet worden gebruikt voor het definiëren van een resourcegroep die backend met WebSocket ingeschakeld-servers. BackendHttpSetting moeten worden gedefinieerd met backend poort 80/443 alleen. Eigenschappen van affiniteit cookie gebaseerde en requestTimeouts zijn niet relevant voor WebSocket-verkeer is toegestaan. Er is geen wijziging vereist in doorstuurregel. Doorstuurregel 'Eenvoudige' moet nog worden gebruikt om te koppelen van de juiste luisteraar ervan af met de bijbehorende backend adresgroep. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket ingeschakeld backend

Uw backend moet een HTTP-/ HTTPS-webserver waarop de geconfigureerde (meestal 80/443) poort voor WebSocket om te werken. Deze vereiste geldt omdat WebSocket protocol is vereist voor de eerste handshake moeten HTTP met Upgrade naar WebSocket-protocol als koptekstveld.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Een andere reden hiervoor is dat die toepassing gateway-test voor status van back-end ondersteunt alleen HTTP-/ HTTPS-protocollen. Als de backend-server niet reageert op HTTP-/ HTTPS sondes, zouden moeten worden genomen afmelden bij de backend-toepassingen en er geen aanvragen die met inbegrip van WebSocket aanvragen, zou dit backend hebt bereikt.

## <a name="next-steps"></a>Volgende stappen

Na het leren werken met WebSocket ondersteuning, gaat u naar het [maken van een toepassingsgateway](application-gateway-create-gateway.md) aan de slag met een webtoepassing WebSocket ingeschakeld.
