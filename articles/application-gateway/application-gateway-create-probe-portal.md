<properties
   pageTitle="Een aangepaste test voor een toepassingsgateway maken met behulp van de portal | Microsoft Azure"
   description="Meer informatie over het maken van een aangepaste test voor de Gateway-toepassing met behulp van de portal"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Een aangepaste test voor de Gateway-toepassing maken met behulp van de portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-probe-ps.md)
- [Azure klassieke PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Scenario

De volgende scenario doorloopt maken van een aangepaste status-test in een bestaande application gateway.
Het scenario wordt ervan uitgegaan dat u de stappen om te [maken van een Gateway toepassing](application-gateway-create-gateway-portal.md)al hebt gevolgd.

## <a name="createprobe"></a>De test maken

Sondes worden geconfigureerd in een proces via de portal. De eerste stap is het opzetten van de test, naast u de test toevoegen aan de backend HTTP-instellingen van de toepassingsgateway.

### <a name="step-1"></a>Stap 1

Ga naar http://portal.azure.com en selecteer een bestaande application gateway.

![Overzicht van de Gateway toepassing][1]

### <a name="step-2"></a>Stap 2

Klik op **controleert** en klik op de knop **toevoegen** als u wilt toevoegen van een nieuwe test.

![Test blade toevoegen met informatie is ingevuld][2]

### <a name="step-3"></a>Stap 3

Vul de vereiste gegevens voor de test en klik op **OK**als voltooid.

- **Naam** - dit is een beschrijvende naam voor de test die in de portal toegankelijk is.
- **Host** - dit is de hostnaam die wordt gebruikt voor de test. Van toepassing alleen als er meerdere site is geconfigureerd op de Gateway-toepassing, anders gebruiken '127.0.0.1'. Dit is anders dan VM hostnaam.
- **Pad** - de rest van de volledige url voor de aangepaste-test. Een geldig pad begint met '/'
- **Interval (sec)** - hoe vaak de test wordt uitgevoerd als u wilt controleren op status. Het wordt niet aanbevolen om in te stellen hoe lager dan 30 seconden.
- **Time-out (sec)** - de hoeveelheid tijd die de test voordat er een time-out optreedt wacht. De time-outinterval moet hoog genoeg dat een http-oproep om ervoor te zorgen dat de pagina van de servicestatus backend is beschikbaar kan worden aangebracht.
- **Drempelwaarde voor beschadigd** - aantal mislukte pogingen als beschadigd worden beschouwd. Een drempelwaarde van 0 betekent dat als een systeemstatus controleren mislukt de back-enddatabase wordt bepaald beschadigd onmiddellijk.

> [AZURE.IMPORTANT] de hostnaam is niet de naam van de server. Dit is de naam van de virtuele host uitgevoerd op de toepassingsserver. De test wordt verzonden naar http://(host name):(port from httpsetting)/urlPath

![Test configuratie-instellingen][3]

## <a name="add-probe-to-the-gateway"></a>Test toevoegen aan de gateway

Nu de test is gemaakt, is het tijd toe te voegen aan de gateway. Test-instellingen zijn ingesteld op de backend-http-instellingen van de toepassingsgateway.

### <a name="step-1"></a>Stap 1

Klik op de **HTTP-instellingen** van de toepassingsgateway en klik vervolgens op de huidige back-end HTTP-instellingen in het venster om het blad configuratie weer te geven.

![venster HTTPS-instellingen][4]

### <a name="step-2"></a>Stap 2

Klik op het blad **appGatewayBackEndHttp** instellingen, klikt u op **aangepaste test gebruiken** en kies de test die is gemaakt in de sectie [maken de test](#createprobe) .
Als alles compleet is, klikt u op **OK** en de instellingen zijn toegepast.

![appgatewaybackend instellingen blade][5]

De standaard-test controleert de toegang tot de webtoepassing. Nu dat een aangepaste-test is gemaakt, wordt in de toepassingsgateway maakt gebruik van het aangepaste pad gedefinieerd om de status voor het back-end geselecteerd te houden. De toepassingsgateway op basis van de criteria die is gedefinieerd, wordt het bestand dat is opgegeven in de test gecontroleerd. Als de oproep door naar host: poort / pad resulteert niet in een Http 200 OK status antwoord wordt verzonden, wordt de server overgenomen afmelden bij de draaiing, nadat de beschadigde drempel is bereikt. Klik op het beschadigde exemplaar om te bepalen wanneer deze verandert in orde opnieuw blijft scannen. Zodra het exemplaar terug is toegevoegd aan de orde server-groep verkeer begint die doorloopt tot het opnieuw en scannen naar het item blijft met de opgegeven interval gebruiker als normale.


## <a name="next-steps"></a>Volgende stappen

Zie informatie over het configureren van SSL Offloading met Azure-toepassingsgateway [SSL Offload configureren](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png