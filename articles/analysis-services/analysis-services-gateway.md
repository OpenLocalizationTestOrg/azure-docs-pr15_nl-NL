<properties
   pageTitle="Lokale gegevensgateway | Microsoft Azure"
   description="Een gateway On-premises is nodig als uw server Analysis Services in Azure maakt verbinding met on-premises gegevensbronnen."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Lokale gegevensgateway

De on-premises implementatie data gateway fungeert als een brug, leveren van de overdracht van beveiligde gegevens tussen on-premises gegevensbronnen en uw Azure Analysis Services-server in de cloud.

Een gateway is geïnstalleerd op een computer in uw netwerk. Een gateway moet worden geïnstalleerd voor elke Azure Analysis Services-server die u in uw Azure-abonnement hebt. Als u twee servers in uw Azure abonnement die verbinding met on-premises gegevensbronnen maken hebt, moet er bijvoorbeeld een gateway zijn geïnstalleerd op twee verschillende computers in uw netwerk.

## <a name="requirements"></a>Vereisten

**Minimale vereisten:**

- 4.5 .NET framework
- 64-bits versie van Windows 7 / Windows Server 2008 R2 (of hoger)

**Aanbevolen:**

- 8 core processor
- 8 GB geheugen vereist
- 64-bits versie van Windows 2012 R2 (of hoger)

**Belangrijke overwegingen:**

- De gateway kan niet worden geïnstalleerd op een domeincontroller.

- Slechts één gateway kan worden geïnstalleerd op één computer.

- De gateway installeren op een computer waarop blijft op en gaat niet verder slaapstand. Als de computer niet ingeschakeld is, kunnen uw Azure Analysis Services-server kan geen verbinding maken met uw lokale gegevensbronnen om gegevens te vernieuwen.

- Installeer de gateway niet op een computer draadloos verbonden met uw netwerk. Prestaties kan afnemen.

- Als u wilt wijzigen op de naam van de server voor een gateway die al is geconfigureerd, moet u opnieuw te installeren en configureren van een nieuwe gateway.

- In sommige gevallen mogelijk tabelmodellen verbinding maken met gegevensbronnen met behulp van systeemeigen providers zoals SQL Server Native Client (SQLNCLI11) een fout geretourneerd. Zie voor meer informatie, [Uitvoergegevensbron verbindingen](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Ondersteunde on-premises gegevensbronnen
Voor preview ondersteunt de gateway verbindingen tussen uw Azure Analysis Services-server en de volgende gegevensbronnen van on-premises implementatie:

- SQL Server
- SQL datawarehouse
- APS
- Oracle
- Teradata


## <a name="download"></a>Downloaden
 [Downloaden van de gateway](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Installeren en configureren

1. Voer setup uit.

2. Kies de locatie van een installatie en accepteer de licentievoorwaarden.

3. Meld u aan bij Azure.

4. Geef uw Azure-analyse-servernaam. U kunt slechts één server per gateway opgeven. Klik op **configureren** en u bent klaar.

    ![Meld u aan bij azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Werkwijze
De gateway wordt uitgevoerd als een Windows-service, **On-premises gegevensgateway**, op een computer in het netwerk van uw organisatie. De gateway die u hebt geïnstalleerd voor gebruik met Azure Analysis Services is gebaseerd op dezelfde gateway die wordt gebruikt voor andere services zoals Power BI, maar met enkele verschillen in hoe deze geconfigureerd.

![Werkwijze](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Query's en gegevens stroom werken als volgt:

1.  Een query is gemaakt door de cloudservice met het versleutelde referenties voor de on-premises gegevensbron. Deze wordt vervolgens naar een wachtrij voor de gateway bij naar het proces verzonden.

2.  De gateway-cloudservice analyseert de query en de aanvraag verplaatst naar de [Bus van Azure-Service](https://azure.microsoft.com/documentation/services/service-bus/).

3.  De on-premises implementatie data gateway controleert de Bus van de Azure-Service voor aanvragen in behandeling.

4.  De gateway wordt de query, ontsleutelt de referenties en maakt verbinding met de gegevensbronnen met de referenties.

5.  De gateway naar de gegevensbron voor de uitvoering van de query te verzenden.

6.  De resultaten zijn verzonden vanuit de gegevensbron, terug naar de gateway en vervolgens naar de cloudservice.

## <a name="windows-service-account"></a>Windows-serviceaccount

De on-premises implementatie data gateway is geconfigureerd voor gebruik van *NT SERVICE\PBIEgwService* voor de aanmeldingsreferenties van de Windows-service. Standaard heeft deze rechts van aanmelden als een service; in de context van de computer waarop u de gateway installeert op. Deze referentie is niet hetzelfde account verbinding maakt met on-premises gegevensbronnen gebruikt of uw Azure-account.  

Als u problemen met uw proxyserver vanwege verificatie ondervindt, wilt u mogelijk de Windows-serviceaccount wijzigen aan de domeingebruiker van een of serviceaccount beheerd.

## <a name="ports"></a>Poorten

De gateway wordt gemaakt van een uitgaande verbinding met Azure Service Bus. Deze op uitgaande poorten communiceert: TCP 443 (standaard), 5671, 5672, 9350 tot en met 9354.  De gateway is geen binnenkomende poorten vereist.

Het is raadzaam in u "witte" lijst de IP-voor de regio van uw gegevens in uw firewall adressen. U kunt de [lijst met Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653)downloaden. Deze lijst wordt wekelijkse bijgewerkt.

> [AZURE.NOTE]  De IP-adressen weergegeven in de lijst met Azure Datacenter IP zijn in CIDR-notatie. 10.0.0.0/24 betekent bijvoorbeeld niet 10.0.0.0 tot en met 10.0.0.24. Meer informatie over de [CIDR-notatie](http://whatismyipaddress.com/cidr).

Hier volgen de volledig gekwalificeerde domeinnamen gebruikt door de gateway.

|Domeinnamen|Uitgaande poorten|Beschrijving|
|---|---|---|
|*. powerbi.com|80|HTTP gebruikt om te downloaden van het installatieprogramma.|
|*. powerbi.com|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671-5672|Geavanceerde berichtenwachtrij-Protocol (AMQP)|
|*. servicebus.windows.net|443, 9350-9354|Listeners op Service Bus Relay via TCP (hiervoor 443 voor toegangsbeheer token acquisition)|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. core.windows.net|443|HTTPS|
|Login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Wordt gebruikt voor het testen van internet connectivity of de gateway niet bereikbaar door de Power BI-service is.|
|*.microsoftonline-p.com|443|Wordt gebruikt voor verificatie afhankelijk van de configuratie.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>HTTPS-communicatie met Azure Service Bus afdwingen

U kunt de gateway om te communiceren met Azure Service Bus HTTPS gebruiken in plaats van rechtstreekse TCP; afdwingen Dit kunt prestaties echter enorm verkleinen. Moet u het bestand *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* wijzigen. Wijzig de waarde van `AutoDetect` naar `Https`. Dit bestand zich bevindt, al dan niet standaard in *C:\Program Files\On-premises gegevensgateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Problemen oplossen
Geavanceerde instellingen weergeven is de on-premises implementatie data gateway die wordt gebruikt om verbinding te maken van Azure Analysis Services naar uw on-premises gegevensbronnen dezelfde gateway die wordt gebruikt met Power BI.

Als u problemen bij het installeren en configureren van een gateway hebt, moet u Zie [problemen met de Power BI-Gateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Als u denkt dat u een probleem ondervindt met uw firewall dat, raadpleegt u de secties firewall of proxyserver.

Als u denkt dat zich proxy problemen, klikt u met de gateway, Zie [proxy-instellingen configureren voor de Power BI-Gateways](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Volgende stappen
- [Analyseservices beheren](analysis-services-manage.md)
- [Gegevens ophalen uit Analysis Services van Azure](analysis-services-connect.md)
