<properties
   pageTitle="Een regel op basis van een pad voor een toepassingsgateway maken met behulp van de portal | Microsoft Azure"
   description="Leer hoe u een regel op basis van een pad voor een toepassingsgateway maken met behulp van de portal"
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

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Een regel op basis van een pad voor een toepassingsgateway maken met behulp van de portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-url-route-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-url-route-arm-ps.md)

URL-pad gebaseerde routering, kunt u routes op basis van het URL-pad van HTTP-aanvraag koppelen. Er wordt gecontroleerd of er is een doorsturen naar een groep met back-enddatabase geconfigureerd voor het URL-lijsten in Gateway-toepassing en het netwerkverkeer verzenden naar de gedefinieerde back-endpool. Er wordt een vaak gebruikt voor URL-e-mailroutering balance '-verzoeken om verschillende inhoudstypen aan groepen van verschillende back-end-server te laden.

URL-e-mailroutering maakt u kennis met een nieuwe regeltype tot toepassingsgateway. Toepassingsgateway heeft twee regeltypen: basisfilters en pad gebaseerde regels. Eenvoudige regeltype biedt round robin-service voor het back-enddatabase van toepassingen tijdens het pad gebaseerde regels naast round robin distributie, ook rekening pad patroon van de aanvraag-URL tijdens het kiezen van de backend-toepassingen.

## <a name="scenario"></a>Scenario

De volgende scenario doorloopt maken van een regel op basis van het pad in een bestaande application gateway.
Het scenario wordt ervan uitgegaan dat u de stappen om te [maken van een Gateway toepassing](application-gateway-create-gateway-portal.md)al hebt gevolgd.

![URL doorsturen][scenario]

## <a name="createrule"></a>De regel op basis van een pad maken

Een regel op basis van een pad vereist een eigen luisteraar ervan af, voordat het maken van de regel moet u controleren of er een beschikbaar luisteraar ervan af gebruiken.

### <a name="step-1"></a>Stap 1

Ga naar http://portal.azure.com en selecteer een bestaande application gateway. Klik op **regels**

![Overzicht van de Gateway toepassing][1]

### <a name="step-2"></a>Stap 2

Klik op de knop **pad gebaseerde** als u wilt toevoegen van een nieuwe regel op basis van het pad.

### <a name="step-3"></a>Stap 3

Het **pad gebaseerde regel toevoegen** blad bevat twee secties. De eerste sectie is waar u de luisteraar ervan af, de naam van de standaardinstellingen voor het pad en de regel hebt gedefinieerd. De standaardinstellingen voor het pad zijn voor routes die niet onder het aangepaste pad gebaseerde route vallen. De tweede sectie van het blad **pad gebaseerde regel toevoegen** is waar u het pad gebaseerde regels zelf definieert.

**Basisinstellingen**

- **Naam** - dit is een beschrijvende naam op de regel die in de portal toegankelijk is.
- **Luisteraar ervan af** - dit is de luisteraar ervan af die wordt gebruikt voor de regel.
- **Standaardgroep backend** - met deze instelling is de instelling die u de back-enddatabase definieert moet worden gebruikt voor de standaardregel
- **Instellingen van de standaard HTTP** - met deze instelling is de instelling die de HTTP-instellingen moet worden gebruikt voor de standaardregel definieert.

**Pad gebaseerde regels**

- **Naam** - dit is een beschrijvende naam op de regel op basis van het pad.
- **Paden** - met deze instelling wordt gedefinieerd voor het pad dat de regel zoekt bij het doorsturen van verkeer
- **Groep Backend** - met deze instelling is de instelling die u de back-enddatabase definieert moet worden gebruikt voor de regel
- **Instelling voor HTTP** - met deze instelling is de instelling die u definieert de HTTP-instellingen voor de regel moet worden gebruikt.

>[AZURE.IMPORTANT] Paden: De lijst met pad patronen om aan te passen. Elk moet beginnen met / en alleen een '\*"is toegestaan is aan het einde. Geldige voorbeelden zijn/xyz, / XYZ* of /xyz/*.  

![Regel pad gebaseerde blade toevoegen met informatie is ingevuld][2]

Een regel op basis van het pad toevoegen aan een bestaande application gateway is een eenvoudig proces via de portal. Nadat een regel op basis van een pad is gemaakt, kan deze om toe te voegen aanvullende regels eenvoudig worden bewerkt. 

![aanvullende pad gebaseerde regels toevoegen][3]

## <a name="next-steps"></a>Volgende stappen

Zie informatie over het configureren van SSL Offloading met Azure-toepassingsgateway [SSL Offload configureren](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png