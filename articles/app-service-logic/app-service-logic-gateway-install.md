<properties
   pageTitle="Logica Apps on-premises implementatie gegevensgateway installeren | Microsoft Azure"
   description="Informatie over het installeren van de on-premises implementatie data gateway voor gebruik in een app logica."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>De on-premises implementatie data gateway installeren voor logica-Apps

## <a name="installation-and-configuration"></a>Installatie en configuratie

### <a name="prerequisites"></a>Vereisten voor

Minimum:

* 4.5 .NET framework
* 64-bits versie van Windows 7 of Windows Server 2008 R2 (of hoger)

Aanbevolen:

* 8 core processor
* 8 GB geheugen vereist
* 64-bits versie van Windows 2012 R2 (of hoger)

Gerelateerde overwegingen:

* U kunt een gateway niet installeren op een domeincontroller.
* U kunt een gateway niet mag installeren op een computer, zoals een laptop, die mogelijk zijn uitgeschakeld, asleep, of niet verbonden met Internet, omdat de gateway deze situaties niet worden uitgevoerd. Daarnaast kan gateway prestaties afnemen via een draadloos netwerk.

### <a name="install-a-gateway"></a>Een gateway installeren

U kunt het [installatieprogramma voor de on-premises implementatie data gateway hier](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409)krijgen.

**On-premises gegevensgateway** opgeven als de modus, meld u aan met uw werk of schoolaccount, en vervolgens een nieuwe gateway configureren of migreren, herstellen, of een bestaande gateway overnemen.

* Als u wilt een gateway configureert, typ een **naam** voor deze en een **herstel-toets**, en klik of tik op **configureren**.

    Geef een herstelsleutel die ten minste acht tekens bevat, en een veilige opbergen. U moet deze toets als u wilt migreren, herstellen of overnemen van de gateway.

* Als u wilt migreren, herstellen of een bestaande gateway overnemen, vindt u de toets herstel die is opgegeven toen de gateway is gemaakt.

### <a name="restart-the-gateway"></a>Opnieuw starten van de gateway

De gateway wordt uitgevoerd als een Windows-service in en net als met een andere Windows-service, u kunt starten en stoppen op verschillende manieren. U kunt bijvoorbeeld open een opdrachtprompt met verhoogde machtigingen op de computer waarop de gateway wordt uitgevoerd, en voer een van deze opdrachten:

* Als u wilt stoppen met de service, moet u deze opdracht uitvoert:

    `net stop PBIEgwService`

* De service wilt starten, moet u deze opdracht uitvoert:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Een firewall of proxyserver configureren

Zie voor informatie over het proxy-informatie voor de gateway te verstrekken, [proxy-instellingen configureren](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

U kunt controleren of uw firewall of proxyserver, verbindingen blokkeert mogelijk door de volgende opdracht uit te voeren vanuit een PowerShell-prompt. Hierdoor wordt getest connectiviteit met de Bus Azure-Service. Dit alleen getest netwerkconnectiviteit en heeft niets te doen met de cloud server-service of de gateway. Het handig om te bepalen of uw computer toegang af met internet krijgen kan.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

De resultaten moeten er ongeveer in dit voorbeeld zijn. Als **TcpTestSucceeded** niet waar is, wordt u mogelijk geblokkeerd door een firewall.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Desgewenst kunt u volledig vult u de **computernaam** en **poort** waarden met de lijst onder [configureren poorten](#configure-ports) verderop in dit onderwerp.

De firewall kan ook worden geblokkeerd door de verbindingen die de Azure Service Bus aanbrengt in de Azure datacenters. Als dit het geval is, wilt u "witte" lijst (deblokkeren) alle IP-adressen voor uw regio voor deze datacenters. U kunt een lijst met [hier Azure IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Poorten configureren

De gateway wordt gemaakt van een uitgaande verbinding met Azure Service Bus. Deze op uitgaande poorten communiceert: TCP 443 (standaard), 5671, 5672, 9350 t/m 9354. De gateway vereist geen binnenkomende poorten.

Meer informatie over [hybride oplossingen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMEINNAMEN | UITGAANDE POORTEN | BESCHRIJVING |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671-5672 | Geavanceerde berichtenwachtrij-Protocol (AMQP) |
| *. servicebus.windows.net | 443, 9350-9354 | Listeners op Service Bus Relay via TCP (hiervoor 443 voor toegangsbeheer token acquisition) |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. core.windows.net | 443 | HTTPS |
| Login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Wordt gebruikt voor het testen van internet connectivity of de gateway niet bereikbaar door de Power BI-service is. |

Als u wit lijst IP-adressen in plaats van de domeinen wilt, kunt u downloaden en gebruiken van de [lijst met Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653). In sommige gevallen wordt het Azure Service Bus-verbindingen worden gemaakt met IP-adres in plaats van de volledig gekwalificeerde domeinnamen.

### <a name="sign-in-account"></a>Aanmeldingsaccount

Gebruikers wordt Meld u aan met een een werk- of schoolaccount. Dit is uw organisatieaccount. Als u zich voor een aanbod voor Office 365 aanmeldde en uw e-mail van de werkelijke hoeveelheid werk niet opgeeft, kan het lijken zoals jeff@contoso.onmicrosoft.com. Uw account, klikt u in een cloudservice, is opgeslagen in een tenant in Azure Active Directory (AAD). In de meeste gevallen van uw account AAD UPN komen overeen met het e-mailadres.

### <a name="windows-service-account"></a>Windows-serviceaccount

De on-premises implementatie data gateway is geconfigureerd voor gebruik van NT SERVICE\PBIEgwService voor de aanmeldingsreferenties van de Windows-service. Standaard heeft deze rechts van het logboek op als een service. Dit is in de context van de computer waarop u de gateway installeert.

Dit is het account waarmee verbinding maken met on-premises gegevensbronnen of het werk of school-account waarmee u zich aanmeldt bij cloud services niet.

##<a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="general"></a>Algemene

**Vraag**: wat gegevensbronnen de gateway ondersteunt?<br/>
**Answer**: vanaf deze schrijven, SQL Server.

**Vraag**: heb ik een gateway voor gegevensbronnen in de cloud, zoals SQL Azure nodig? <br/>
**Answer**: Nee. Een gateway verbindt met on-premises gegevensbronnen alleen.

**Vraag**: Wat is de werkelijke Windows-service genoemd?<br/>
**Answer**: In Services, de gateway heet Power BI Enterprise Gateway-Service.

**Vraag**: zijn er geen binnenkomende verbindingen met de gateway vanuit de cloud? <br/>
**Answer**: Nee. De gateway wordt uitgaande verbindingen met Azure Service Bus gebruikt.

**Vraag**: Wat gebeurt er als ik uitgaande verbindingen blokkeren? Wat heb ik nodig om te openen? <br/>
**Answer**: raadpleegt u de poorten en hosts die de gateway wordt gebruikt.


**Vraag**: de gateway hoeft te worden geïnstalleerd op dezelfde computer als de gegevensbron? <br/>
**Answer**: Nee. De gateway maakt verbinding met de gegevensbron met behulp van de verbindingsgegevens die u hebt ontvangen. Van de gateway is als het ware een clienttoepassing in deze zin. Deze moet alleen kunnen verbinding maken met de naam van de server die u hebt ontvangen.


**Vraag**: Wat is de latentie voor het uitvoeren van query's met een gegevensbron van de gateway? Wat is de beste architectuur? <br/>
**Answer**: als u wilt verkleinen netwerklatentie, installeert u de gateway zo dicht bij de gegevensbron mogelijk. Als u de gateway op de werkelijke gegevensbron installeren kunt, wordt deze de latentie kennis minimaliseren. U kunt ook de datacenters. Als uw service het datacenter West Amerikaans gebruikt wordt en u gehost in een VM Azure SQL-Server hebt, wilt u wordt ook Azure VM in West VS is. Hiermee wordt Latentie minimaliseren en egress kosten op de VM Azure voorkomen.


**Vraag**: zijn er een vereisten voor netwerkbandbreedte? <br/>
**Answer**: het wordt aanbevolen om te laten goede doorvoer voor uw netwerkverbinding. Elke omgeving verschilt en de hoeveelheid gegevens die zijn verzonden is van invloed op de resultaten. Het gebruik van ExpressRoute kan helpen om te waarborgen van een niveau doorvoer tussen on-premises implementatie en de Azure datacenters.

U kunt de derde partij hulpprogramma Azure snelheid Test-app gebruiken om te bepalen wat uw doorvoer is.


**Vraag**: kan de Windows-service van de gateway worden uitgevoerd met een Azure Active Directory-account? <br/>
**Answer**: Nee. De Windows-service moet een geldige Windows-account hebben. Standaard wordt deze uitgevoerd met de Service beveiligings-id, NT SERVICE\PBIEgwService.


**Vraag**: hoe worden resultaten verzonden terug naar de cloud? <br/>
**Answer**: hiervoor in de Bus Azure-Service. Zie hoe dit werkt voor meer informatie.


**Vraag**: waar worden mijn aanmeldingsreferenties opgeslagen? <br/>
**Answer**: de referenties die u voor een gegevensbron invoert zijn opgeslagen in de gateway-cloudservice versleuteld. De referenties worden ontsleuteld op de gateway on-premises implementatie.

### <a name="high-availabilitydisaster-recovery"></a>Hoge beschikbaarheid/herstel

**Vraag**: zijn er een abonnement voor het inschakelen van beschikbaarheid scenario's met de gateway? <br/>
**Answer**: dit is op de routekaart, maar we een tijdlijn nog geen hebt.


**Vraag**: welke opties zijn beschikbaar voor herstel? <br/>
**Answer**: U kunt de herstel-toets gebruiken om te zetten of verplaats een gateway. Wanneer u de gateway hebt geïnstalleerd, geeft u de toets herstel.


**Vraag**: Wat is het voordeel van de sleutel voor bestandsherstel? <br/>
**Answer**: biedt een manier om te migreren of herstellen van de gateway-instellingen na een noodgevallen.

### <a name="troubleshooting"></a>Problemen oplossen

**Vraag**: waar zijn de gateway-Logboeken? <br/>
**Answer**: Zie's verderop in dit onderwerp.


**Vraag**: hoe kan ik zien wat query's worden verzonden naar de on-premises gegevensbron? <br/>
**Answer**: kunt u query-tracering, waaronder de query's dat wordt verzonden. U deze terug naar de oorspronkelijke waarde als klaar met problemen oplossen. Query tracering ingeschakeld laat, wordt de logboeken groter wordt.

U kunt ook hulpprogramma's waarmee u uw gegevensbron voor tracering query's heeft bekijken. U kunt bijvoorbeeld uitgebreide gebeurtenissen of SQL Profiler gebruiken voor SQL Server en Analysis Services.

## <a name="how-the-gateway-works"></a>De werking van de gateway

Wanneer een gebruiker communiceert met een element dat verbonden met een lokale gegevensbron:

1. De cloudservice maakt u een query, samen met de versleutelde referenties voor de gegevensbron, en de query te verzenden naar de wachtrij voor de gateway te verwerken.
1. De service analyseert de query en de aanvraag verplaatst naar de Bus Azure-Service.
1. De on-premises implementatie data gateway controleert de Bus van de Azure-Service voor aanvragen in behandeling.
1. De gateway wordt de query, ontsleutelt de referenties en maakt verbinding met de gegevensbron(nen) met de referenties.
1. De gateway naar de gegevensbron voor de uitvoering van de query te verzenden.
1. De resultaten zijn verzonden vanuit de gegevensbron terug naar de gateway en vervolgens naar de cloudservice. De resultaten worden vervolgens gebruikt door de service.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="update-to-the-latest-version"></a>Bijwerken naar de meest recente versie

Een groot aantal problemen kunt implementeren wanneer de gatewayversie verlopen is.  Het is een goede algemene gewoonte om ervoor te zorgen dat u zich in de meest recente versie.  Als u kunt de gateway nog niet hebt bijgewerkt, een maand of langer, wilt u mogelijk kunt u de nieuwste versie van de gateway installeert en Zie als kunt u het probleem reproduceren.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Fout: Gebruiker toevoegen aan groep is mislukt. (-2147463168 PBIEgwService prestaties Log gebruikers)

U kunt deze fout kan optreden als u probeert de gateway installeren op een domeincontroller, die wordt niet ondersteund. U moet de gateway op een computer die niet een domeincontroller implementeren.

## <a name="tools"></a>Hulpmiddelen voor

### <a name="collecting-logs-from-the-gateway-configurator"></a>Logboeken verzamelen van de gateway-configurator

U kunt verschillende logboeken voor de gateway verzamelen. Altijd beginnen waaraan de logboekbestanden!

#### <a name="installer-logs"></a>Installer Logboeken

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Configuratielogboeken

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway-service Logboeken

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Gebeurtenislogboeken

De logboeken aan de Data Management Gateway en PowerBIGateway staan onder **toepassing Logboeken en Services**.

### <a name="fiddler-trace"></a>Fiddler traceren

[Fiddler](http://www.telerik.com/fiddler) is een gratis hulpprogramma uit Telerik die bewaakt de HTTP-verkeer is toegestaan.  U kunt zien van de achtergrond en weer met de Power BI-service van de clientcomputer. Dit kan fouten en andere gerelateerde gegevens worden weergegeven.

## <a name="next-steps"></a>Volgende stappen
- [Een lokale verbinding maken met logica Apps](app-service-logic-gateway-connection.md)
- [Onderdelen van de Enterprise-integratie](app-service-logic-enterprise-integration-overview.md)
- [Logica Apps verbindingslijnen](../connectors/apis-list.md)