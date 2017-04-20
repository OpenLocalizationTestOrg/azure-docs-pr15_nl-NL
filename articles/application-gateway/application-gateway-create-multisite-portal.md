<properties
   pageTitle="Een bestaande application gateway voor het hosten van meerdere sites in de portal van Azure configureren | Microsoft Azure"
   description="Deze pagina bevat instructies voor het configureren van een bestaande Azure application gateway voor het hosten van meerdere webtoepassingen op dezelfde gateway met Azure portal."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Een bestaande application gateway voor het hosten van meerdere webtoepassingen configureren

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-multisite-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Meerdere site-host, kunt u meer dan één webtoepassing op dezelfde toepassingsgateway implementeren. Dit is afhankelijk van de aanwezigheid van host koptekst in de binnenkomende HTTP-aanvraag, om te bepalen welke luisteraar ervan af verkeer ontvangt. De luisteraar ervan af vervolgens wordt u omgeleid zodat verkeer naar de juiste backend-toepassingen zoals geconfigureerd in de definitie van de gateway. In webtoepassingen SSL is ingeschakeld, is de toepassingsgateway afhankelijk van de Server naam aanduiding (SNI)-extensie voor het kiezen van de juiste luisteraar ervan af voor de webverkeer. Er wordt vaak gebruikt voor het hosten van meerdere site balance '-verzoeken om verschillende webdomeinen naar andere back-enddatabase server pools laden. Meerdere subdomeinen van het dezelfde hoofddomein kunnen op dezelfde manier ook worden gehost op dezelfde toepassingsgateway.

## <a name="scenario"></a>Scenario

In het volgende voorbeeld, toepassingsgateway wordt verkeer naar contoso.com en fabrikam.com fungeert met twee groepen van de back-end-server: contoso server-groep en fabrikam server-groep. Vergelijkbare setup kan worden gebruikt voor host subdomeinen, zoals app.contoso.com en blog.contoso.com.

![scenario voor meerdere locaties][multisite]

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ondersteuning voor meerdere site toegevoegd aan een bestaande application gateway. Om te voltooien van dit scenario moet scenario voor een bestaande application gateway beschikbaar om te configureren. Ga naar [de toepassingsgateway van een met behulp van de portal maken](./application-gateway-create-gateway-portal.md) om informatie over het maken van een toepassingsgateway eenvoudige in de portal.

## <a name="requirements"></a>Vereisten

- **Back-enddatabase server-groep:** De lijst met IP-adressen van de back-end-servers. De IP-adressen vermeld ofwel moeten behoren tot de virtuele netwerk subnet of een openbare IP/VIP moeten worden. FQDN kan ook worden gebruikt.
- **Back-enddatabase groep serverinstellingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front poort:** Deze poort is de openbare poort dat is geopend in de toepassingsgateway. Verkeer raakt deze poort en vervolgens wordt omgeleid naar een van de back-end-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een front poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload). Voor meerdere locaties Toepassingsgateways, worden hostnaam en SNI indicatoren ook toegevoegd.
- **Regel:** De regel wordt gebonden de luisteraar ervan af, de back-enddatabase server-groep, en wordt gedefinieerd welke back-end-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af.
- **Certificaten:** Elke luisteraar ervan af vereist een unieke certificaat, in dit voorbeeld 2 listeners worden gemaakt voor multi-site. Twee .pfx certificaten en de wachtwoorden opnieuw instellen voor deze moeten worden gemaakt.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Hier volgen de vereiste stappen voor het bijwerken van de toepassingsgateway:

1. Het maken van back-enddatabase pools voor elke site wilt gebruiken.
2. Maak een nieuwe luisteraar ervan af voor elke site toepassingsgateway wordt ondersteund.
3. Maak regels voor het toewijzen van elke luisteraar ervan af met de juiste back-enddatabase.

## <a name="create-back-end-pools-for-each-site"></a>Maken van back-enddatabase pools voor elke site

Een back-enddatabase resourcegroep die voor elke site die toepassingsgateway wordt ondersteuning nodig is, in dit geval 2 wordt gemaakt, één voor contoso11.com en één voor fabrikam11.com.

### <a name="step-1"></a>Stap 1

Navigeer naar een bestaande application gateway in het Azure-portal (https://portal.azure.com). Selecteer **Backend-toepassingen** en klikt u op **toevoegen**

![back-end-groepen toevoegen][7]

### <a name="step-2"></a>Stap 2

Vul de gegevens voor de back-enddatabase groep **pool1**, het IP-adressen of FQDN's toe te voegen voor de back-end-servers en klik op **OK**

![instellingen voor pool1 backend-toepassingen][8]

### <a name="step-3"></a>Stap 3

Klik op **toevoegen** om een extra back-enddatabase groep **pool2**, het IP-adressen of FQDN's toe te voegen voor de back-end-servers op het blad backend-toepassingen en klik op **OK**

![instellingen voor pool2 backend-toepassingen][9]

### <a name="create-listeners-for-each-back-end"></a>Listeners voor elke back-enddatabase maken

Toepassingsgateway is afhankelijk van HTTP 1.1 host-headers voor het hosten van meer dan één website op hetzelfde openbare IP-adres en dezelfde poort. De eenvoudige luisteraar ervan af die is gemaakt in de portal bevat deze eigenschap niet.

### <a name="step-1"></a>Stap 1

Klik op **Listeners** op de bestaande toepassingsgateway en klikt u op **meerdere locaties** om toe te voegen van de eerste luisteraar ervan af.

![listeners overzicht blade][1]

### <a name="step-2"></a>Stap 2

Vul de gegevens voor de luisteraar ervan af, In dit voorbeeld SSL-beëindiging is geconfigureerd, maakt u een nieuwe frontend poort. Upload het .pfx-certificaat moet worden gebruikt voor SSL-beëindiging. De enige verschil op deze vergeleken met het blad standaard eenvoudige luisteraar ervan af blade is de hostnaam.

![luisteraar ervan af eigenschappen blade][2]

### <a name="step-3"></a>Stap 3

Klik op **meerdere locaties** en maken van een andere luisteraar ervan af, zoals wordt beschreven in de vorige stap voor het tweede site. Zorg ervoor dat u een ander certificaat gebruiken voor de tweede luisteraar ervan af. De enige verschil op deze vergeleken met het blad standaard eenvoudige luisteraar ervan af blade is de hostnaam. Vul de gegevens voor de luisteraar ervan af en klik op **OK**.

![luisteraar ervan af eigenschappen blade][3]

> [AZURE.NOTE] Het maken van listeners in Azure portal voor toepassingsgateway is een langdurige taak, het duurt enige tijd om te maken van de twee listeners in dit scenario. Wanneer voert u de voorstelling listeners in de portal zoals gezien in de volgende afbeelding.

![overzicht van de luisteraar ervan af][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Maak regels listeners om aan te wijzen backend-toepassingen

### <a name="step-1"></a>Stap 1

Navigeer naar een bestaande application gateway in het Azure-portal (https://portal.azure.com). Selecteer **regels** en kiest u de bestaande standaard regel **rule1** en klik op **bewerken**.

### <a name="step-2"></a>Stap 2

Vul het blad regels zoals gezien in de volgende afbeelding. De eerste luisteraar ervan af en de eerste groep te kiezen en te klikken op **Opslaan** als alles compleet is.

![bestaande regel bewerken][6]

### <a name="step-3"></a>Stap 3

Klik op **eenvoudige regel** als de 2e regel wilt maken. Vul het formulier met de tweede luisteraar ervan af en de tweede backend-toepassingen en klik op **OK** om op te slaan.

![eenvoudige regel blade toevoegen][10]

Dit is voltooid met het configureren van een bestaande application gateway met ondersteuning voor meerdere site via de portal van Azure.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beveiligen van uw websites met [Toepassingsgateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png