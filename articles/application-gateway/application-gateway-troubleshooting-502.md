<properties
   pageTitle="Fouten corrigeren toepassing Gateway ongeldige Gateway (502) | Microsoft Azure"
   description="Meer informatie over het oplossen van de toepassing Gateway 502"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Ongeldige gateway fouten in de toepassingsgateway oplossen

## <a name="overview"></a>Overzicht

Na het configureren van een Gateway Azure-toepassing, een van de fouten die gebruikers kunnen ondervinden is ' Serverfout: 502 - webserver heeft een ongeldig antwoord bij die fungeert als een gateway of proxy-server ". Dit kan gebeuren vanwege de volgende belangrijkste oorzaken hebben:

- Azure-toepassingsgateway van back-enddatabase groep is niet ingesteld of leeg.
- Geen van de VMs of exemplaren in VM schaal instellen in orde zijn.
- Back-enddatabase VMs of instanties van VM schaal instellen reageert niet op de standaard-systeemstatus-test.
- Controleert of ongeldige of onjuiste configuratie van aangepaste status.
- Vraag time-out of verbindingsproblemen met aanvragen van gebruikers.

## <a name="empty-backendaddresspool"></a>Lege BackendAddressPool

### <a name="cause"></a>Oorzaak

Als de Gateway-toepassing heeft geen VMs of VM schaal instellen geconfigureerd in de groep back-enddatabase adressen, elk klantverzoek niet doorsturen en genereert een ongeldige gateway-fout.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de back-enddatabase adres van toepassingen niet leeg is. Dit is mogelijk hetzij via PowerShell, CLI of -portal.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

De uitvoer van de voorgaande cmdlet mogen groep niet-lege back-enddatabase adressen bevatten. Hieronder volgt een voorbeeld waar twee toepassingen die worden geretourneerd met FQDN of IP-adressen voor backend VMs zijn geconfigureerd. De inrichten status van de BackendAddressPool moet worden 'geslaagd'.
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Beschadigde exemplaren in BackendAddressPool

### <a name="cause"></a>Oorzaak

Als alle exemplaren van BackendAddressPool beschadigd, klikt u vervolgens toepassingsgateway geen een back-enddatabase route gebruiker aanvraag om te. Dit kan ook de hoofdletters/kleine letters worden wanneer back-enddatabase exemplaren in orde zijn, maar ik heb de vereiste toepassing geïmplementeerd.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de exemplaren in orde zijn en de toepassing correct is geconfigureerd. Controleren of de back-enddatabase exemplaren kunnen reageren op een ping uit een andere VM in de dezelfde VNet zijn. Zorg ervoor dat de aanvraag van een browser naar de webtoepassing onderhouden is als geconfigureerd met een openbare eindpunt.

## <a name="problems-with-default-health-probe"></a>Problemen met de standaard-systeemstatus-test

### <a name="cause"></a>Oorzaak

502 fouten kunnen ook worden voor veelgebruikte indicatoren de standaard-systeemstatus-test is niet kunnen bereiken back-enddatabase VMs. Wanneer een exemplaar van de toepassingsgateway wordt ingericht, wordt deze automatisch een standaard-systeemstatus-test aan elke BackendAddressPool met eigenschappen van de BackendHttpSetting geconfigureerd. Geen invoer van de gebruiker is vereist voor het instellen van deze test. Wanneer u een regel van taakverdeling is geconfigureerd, wordt specifiek, een koppeling tussen een BackendHttpSetting en BackendAddressPool gemaakt. Een standaard-test is geconfigureerd voor elk van deze koppelingen en toepassingsgateway verbinding maakt periodiek systeemstatus controleren op elk exemplaar in de BackendAddressPool bij de opgegeven in het element BackendHttpSetting poort. Volgende tabel bevat de waarden die zijn gekoppeld aan de standaard-systeemstatus-test.


|Eigenschap zoeken | Waarde | Beschrijving|
|---|---|---|
| URL zoeken| http://127.0.0.1/ | URL-pad |
| Interval | 30 | Interval in seconden zoeken |
| Time-out  | 30 | Time-out voor zoeken in seconden |
| Drempelwaarde voor beschadigd | 3 | Aantal nieuwe pogingen onderzoeken. De back-end-server is gemarkeerd omlaag nadat de opeenvolgende test mislukt telling de beschadigde drempel bereikt. |

### <a name="solution"></a>Oplossing

- Zorg ervoor dat een standaard-site is geconfigureerd en op 127.0.0.1 controleert.
- Als BackendHttpSetting een poort dan 80 geeft, moet de standaard-site te beluisteren aan die poort worden geconfigureerd.
- De oproep door naar http://127.0.0.1:port moet een code voor het resultaat van HTTP van 200 retourneren. Dit moet worden geretourneerd binnen de periode van de time-30 seconden.
- Zorg ervoor dat poort geconfigureerd geopend is en dat er geen firewallregels of Azure netwerk beveiligingsgroepen, waarin binnenkomende of uitgaande verkeer op de poort die is geconfigureerd blokkeren.
- Als Azure klassieke VMs of Cloud-Service wordt gebruikt met FQDN of openbare IP-, zorg ervoor dat de bijbehorende wordt [eindpunt](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) geopend.
- Als de VM via Azure resourcemanager is geconfigureerd en buiten de VNet waar toepassingsgateway wordt geïmplementeerd, moet [De beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) worden geconfigureerd voor toegang op de gewenste poort.


## <a name="problems-with-custom-health-probe"></a>Problemen met aangepaste systeemstatus-test

### <a name="cause"></a>Oorzaak

Aangepaste status sondes toestaan extra flexibiliteit aan de standaard gedrag scannen. Wanneer u een aangepaste sondes gebruikt, kunnen gebruikers het interval test, de URL, en pad om te testen en het aantal mislukte antwoorden accepteren voordat u het exemplaar van de back-enddatabase toepassingen als beschadigd markeert configureren. De volgende aanvullende eigenschappen worden toegevoegd.

|Eigenschap zoeken| Beschrijving|
|---|---|
| Naam | Naam van de test. Deze naam wordt gebruikt om te verwijzen naar de test in back-enddatabase HTTP-instellingen. |
| Protocol | Het protocol dat wordt gebruikt om te verzenden de test. De test wordt het protocol dat is gedefinieerd in de instellingen van de HTTP-back-enddatabase gebruiken |
| Host |  Hostnaam verzenden de test. Alleen van toepassing wanneer meerdere site is geconfigureerd op de Gateway-toepassing. Dit is anders dan VM hostnaam.  |
| Pad | Relatieve pad naar de test. Het geldige pad begint van '/'. De test wordt verzonden naar \<protocol\>://\<host\>:\<poort\>\<pad\> |
| Interval | Onderzoeken interval in seconden. Dit is het interval tussen twee opeenvolgende sondes.|
| Time-out | Time-out voor zoeken in seconden. De test is gemarkeerd als mislukt als een geldig antwoord niet binnen deze periode time-out wordt ontvangen. |
| Drempelwaarde voor beschadigd | Aantal nieuwe pogingen onderzoeken. De back-end-server is gemarkeerd omlaag nadat de opeenvolgende test mislukt telling de beschadigde drempel bereikt. |


### <a name="solution"></a>Oplossing

Valideren dat het zoeken van aangepaste status is geconfigureerd als de vorige tabel. Naast de voorgaande stappen voor probleemoplossing bovendien het volgende:

- Zorg ervoor dat de test juist is opgegeven aan de hand van de [handleiding](application-gateway-create-probe-ps.md).
- Als de toepassingsgateway is geconfigureerd voor één site, al dan niet standaard de Host moet naam worden opgegeven als '127.0.0.1', tenzij anderszins geconfigureerd in aangepaste test.
- Zorg ervoor dat een oproep naar http://\<host\>:\<poort\>\<pad\> geeft als resultaat een code voor het resultaat van HTTP van 200.
- Zorg ervoor dat Interval, Time-out en UnhealtyThreshold binnen de acceptabel bereiken.

## <a name="request-time-out"></a>Time-out aanvragen

### <a name="cause"></a>Oorzaak

Wanneer een gebruikersaanvraag wordt ontvangen, wordt toepassingsgateway geldt de geconfigureerde regels voor het verzoek en doorgestuurd naar een exemplaar van de back-endpool. Wacht voor een configureerbare tijdsinterval op een reactie van het exemplaar van de back-enddatabase. Standaard is dit interval **30 seconden**. Als de toepassingsgateway niet van toepassing van de back-enddatabase in dit interval een antwoord ontvangt, ziet gebruikersaanvraag een 502 fout.

### <a name="solution"></a>Oplossing

Toepassingsgateway kan gebruikers deze instelling via BackendHttpSetting, die vervolgens kan worden toegepast op verschillende groepen wilt configureren. Verschillende back-enddatabase van toepassingen kunnen hebben verschillende BackendHttpSetting en dus verschillende verzoek time-out geconfigureerd.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet is opgelost, opent u een [tickets ondersteunen](https://azure.microsoft.com/support/options/).
