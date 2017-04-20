<properties
   pageTitle="Een toepassingsgateway voor SSL offload geconfigureerd met behulp van de portal | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken van een toepassingsgateway met SSL offload met behulp van de portal"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Een toepassingsgateway voor SSL offload geconfigureerd met behulp van de portal

> [AZURE.SELECTOR]
-[Azure-portal](application-gateway-ssl-portal.md)
-[Azure resourcemanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassieke PowerShell](application-gateway-ssl.md)

Azure-toepassingsgateway kan worden geconfigureerd om de sessie Secure Sockets Layer (SSL) op de gateway om te voorkomen dure SSL decoderen taken om optreden bij de webfarm te beëindigen. SSL-offload eenvoudiger ook de installatie van front-server en het beheer van de webtoepassing.

## <a name="scenario"></a>Scenario

De volgende scenario doorloopt configureren van SSL offload op een bestaande application gateway. Het scenario wordt ervan uitgegaan dat u de stappen om te [maken van een Gateway toepassing](application-gateway-create-gateway-portal.md)al hebt gevolgd.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt configureren SSL offload met een toepassingsgateway, is een certificaat vereist. Dit certificaat is geladen op de toepassingsgateway en gebruikt om te versleutelen en het verkeer verzonden via SSL ontsleutelen. Het certificaat moet in de indeling Personal Information Exchange (pfx). Deze bestandsindeling kunt voor de persoonlijke sleutel wilt exporteren die door de toepassingsgateway is vereist voor het uitvoeren van de versleuteling en decoderen van verkeer.

## <a name="add-an-https-listener"></a>Een HTTPS luisteraar ervan af toevoegen

De HTTPS luisteraar ervan af Hiermee wordt gezocht naar verkeer op basis van de configuratie en helpt het verkeer doorsturen naar de backend-toepassingen.

### <a name="step-1"></a>Stap 1

Navigeer naar de Azure-portal en selecteer een bestaande application gateway

### <a name="step-2"></a>Stap 2

Listeners op en klik op de knop toevoegen om toe te voegen een luisteraar ervan af.

![App gateway overzicht blade][1]

### <a name="step-3"></a>Stap 3

Vul de vereiste gegevens voor de luisteraar ervan af en upload het .pfx-certificaat, als alles compleet Klik op OK.

**Naam** - met deze waarde is een beschrijvende naam van de luisteraar ervan af.

**Frontend IP-configuratie** - met deze waarde is de frontend IP-configuratie die wordt gebruikt voor de luisteraar ervan af.

**Frontend poort (naam/poort)** - een beschrijvende naam voor de poort die worden gebruikt op de front-end van de toepassingsgateway en de werkelijke poort gebruikt.

**Protocol** - een schakeloptie om te bepalen of https of http is gebruikt voor de front-end.

**Certificaat (naam en wachtwoord)** - als SSL offload wordt gebruikt, een .pfx-certificaat is vereist voor deze instelling en een beschrijvende naam en wachtwoord zijn vereist.

![luisteraar ervan af blade toevoegen][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Een regel maken en deze te koppelen aan de luisteraar ervan af

De luisteraar ervan af is gemaakt. Het is tijd om een regel voor het verwerken van het verkeer van de luisteraar ervan af te maken.

### <a name="step-1"></a>Stap 1

Klik op de **regels** van de toepassingsgateway en klik vervolgens op toevoegen.

![App gateway regels blade][3]

### <a name="step-2"></a>Stap 2

Klik op het blad **eenvoudige regel toevoegen** , typt u in de beschrijvende naam voor de regel en kies de luisteraar ervan af in de vorige stap hebt gemaakt. Kies de juiste backend-toepassingen en http-instelling en klik op **OK**

![venster HTTPS-instellingen][4]

De instellingen worden nu opgeslagen in de toepassingsgateway. Het opslaan van proces voor deze instellingen kunnen het enige tijd duren voordat ze zijn beschikbaar via de portal of via PowerShell weergeven. Nadat u de toepassingsgateway opgeslagen omgaat met de versleuteling en decoderen van verkeer. Alle verkeer tussen de toepassingsgateway en de backend-endwebservers wordt verwerkt via http. Alle communicatie terug naar de client als gestart via https keert terug naar de client die is versleuteld.

## <a name="next-steps"></a>Volgende stappen

Zie informatie over het configureren van een aangepaste status-test met Azure-toepassingsgateway, [maken een aangepaste status-test](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png