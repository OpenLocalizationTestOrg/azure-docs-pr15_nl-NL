<properties 
    pageTitle="Gebruik van de hybride Connection Manager | Microsoft Azure" 
    description="Installeren en configureren van de hybride Connection Manager en verbinding maken met on-premises implementatie verbindingslijnen in logica-Apps" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Verbinding maken met on-premises implementatie verbindingslijnen met de hybride Connection Manager

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie. Een gateway logica Apps algemene beschikbaarheid (GA) gebruikt voor on-premises implementatie connectivity. Meer informatie over de nieuwe [gateway](app-service-logic-gateway-connection.md) en [Logica Apps GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Als u wilt gebruiken in een on-premises-systeem, wordt de logica Apps gebruikt de hybride Connection Manager. Sommige verbindingslijnen kunnen verbinden met een on-premises-systeem, zoals SQL Server, SAP, SharePoint en dergelijke. 

De hybride verbinding Manager (HCM) is een klik-eenmaal installer dat op een IIS-server binnen uw netwerk op, achter de firewall is geïnstalleerd. Met een relay Azure Service Bus, verifieert HCM de on-premises-systeem met de verbindingslijn in Azure wordt aangegeven. 

> [AZURE.NOTE] Hybride Connection Manager is vereist alleen als u verbinding met een on-premises implementatie resource achter de firewall maakt. Als u geen verbinding met een on-premises-systeem maakt, moet u niet de hybride Connection Manager.

Als u wilt beginnen, hebt u het volgende nodig:

- Azure-Service Bus relay naamruimte SA's verbindingsreeks. Zie [Service Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/) om te bepalen welke laag relais bevat.
- On-premises implementatie aanmeldingsproblemen systeeminformatie, met inbegrip van de gebruikersnaam en wachtwoord. Bijvoorbeeld als u verbinding met een lokale SQL Server maakt, moet u de SQL Server-aanmeldingsaccount en het wachtwoord.
- On-premises implementatie-servergegevens, inclusief poort nummer en de servernaam. Bijvoorbeeld als u verbinding met een lokale SQL Server maakt, moet u de naam van de SQL Server en TCP-poortnummer.

## <a name="get-the-service-bus-connection-string"></a>De verbindingsreeks van de Service-Bus ophalen

Klik in de portal Azure kopiëren naar de hoofdsite Service Bus verbindingsreeks SA's. Deze verbindingsreeks maakt de Azure verbindingslijn verbinding met uw on-premises implementatie-mailsysteem. 

1. Selecteer de naamruimte van uw Service Bus in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)en selecteer **Verbindingsgegevens**:

    ![][SB_ConnectInfo]

2. Kopieer de verbindingsreeks SA's:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Installeren van de hybride Connection Manager

1. In de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u de verbindingslijn die u hebt gemaakt. Om deze te openen, kunt u Selecteer **Bladeren**, selecteer **API-Apps**en selecteer vervolgens de verbindingslijn of API-App. 
<br/><br/>
Klik bij **Hybride verbinding**is de instelling **onvolledig**:
<br/>
![][2] 

2. Selecteer **hybride verbinding**. De Service Bus-verbindingsreeks die u eerder hebt ingevoerd, wordt vermeld.
3. Kopieer de **primaire configuratietekenreeks**.
<br/>
![][PrimaryConfigString]

4. U kunt onder **On-Premises hybride Connection Manager**downloaden van de hybride verbinding Manager of rechtstreeks vanaf de portal. 
<br/><br/>
Als u wilt installeren rechtstreeks vanuit de portal, gaat u naar uw on-premises implementatie IIS-server, blader naar de portal en selecteer **downloaden en configureren**.
<br/><br/>
De hybride Connection Manager downloaden, gaat u naar uw on-premises implementatie IIS-server en gaat u naar de **ClickOnce-toepassing** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). De installatie wordt automatisch gestart, zodat u deze kunt uitvoeren.

5. In het venster **Instellingen van de luisteraar ervan af** , geeft u de **Primaire configuratietekenreeks** die u eerder hebt geplakt (in stap 3) en selecteer **installeren**.

Wanneer de setup is voltooid, het volgende wordt weergegeven:
<br/>
![][3] 

Wanneer u opnieuw aan de verbindingslijn zoeken, is de verbindingsstatus hybride nu **verbonden**. Mogelijk moet u de verbindingslijn sluiten en opnieuw opent:
<br/>
![][4] 

> [AZURE.NOTE] Als u wilt overschakelen naar de secundaire verbindingsreeks, opnieuw bij te werken hybride verbinding en voer de **Secundaire configuratietekenreeks**.


## <a name="tcp-ports-and-security"></a>TCP-poorten en beveiliging

Wanneer u een hybride verbinding maakt, wordt een website gemaakt op uw lokale on-premises implementatie IIS-server. De IIS-server kan zich in een DMZ. De TCP-Poortvereisten op de server IIS opnemen:

TCP-poorten | Waarom
--- | ---
 | Geen binnenkomende TCP-poorten zijn vereist.
9350 - 9354 | Deze poorten worden gebruikt voor de gegevensoverdracht. De Service Bus relay manager controleert poort 9350 om te bepalen of TCP-verbinding beschikbaar is. Als deze beschikbaar is, klikt u vervolgens wordt ervan uitgegaan dat poort 9352 ook beschikbaar is. Gegevensverkeer is gewijd aan poort 9352. <br/><br/>Sta uitgaande verbindingen tot deze poorten toe.
5671 | Wanneer poort 9352 wordt gebruikt voor gegevens-verkeer is toegestaan, poort 5671 wordt gebruikt als de het kanaal besturingselement. <br/><br/>Sta uitgaande verbindingen naar deze poort toe. 
80, 443 | Als poort 9352 en 5671 niet kan worden gebruikt, worden *vervolgens* poort 80 en 443 de fallback poorten gebruikt voor de gegevensoverdracht en het besturingselement kanaal.<br/><br/>Sta uitgaande verbindingen tot deze poorten toe.
On-premises systeempoort | Klik op het systeem on-premises implementatie, door de poort die wordt gebruikt door het systeem te openen. SQL Server gebruikt, bijvoorbeeld meestal poort 1433. Open deze TCP-poort.

[Hostingprovider achter een Firewall met Service Bus](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Problemen oplossen

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>On-premises implementatie problemen oplossen

1. Bevestig de rol van de web IIS is geïnstalleerd en alle IIS-services worden gestart op de IIS-server.
2. Bevestig de hybride Connection Manager is geïnstalleerd en wordt uitgevoerd op de IIS-server:
 - In de IIS-beheer (inetmgr) de website ***MicrosoftAzureBizTalkHybridListener*** moet worden weergegeven en worden uitgevoerd. 
 - Deze website gebruikt de ***HybridListenerAppPool*** die wordt uitgevoerd als de *Netwerkservice* lokale ingebouwde gebruikersaccount. Deze AppPool moet ook worden gestart.
3. Bevestig dat de verbindingslijn is geïnstalleerd en wordt uitgevoerd op de IIS-server: 
 - Een website wordt gemaakt voor de verbindingslijn. Bijvoorbeeld als u een SQL-connector hebt gemaakt, is er een ***MicrosoftSqlConnector_nnn*** -website. In de IIS-beheer (inetmgr) Controleer deze website wordt weergegeven en is gestart. 
 - Deze website gebruikt een eigen IIS-toepassingen met de naam ***HybridAppPoolnnn***. Deze AppPool wordt uitgevoerd als de *Netwerkservice* lokale ingebouwde gebruikersaccount. Deze website- en AppPool moet beide worden gestart. 
 - Blader op de lokale verbindingslijn. Bijvoorbeeld: als uw website verbindingslijn poort 6569 gebruikt, gaat u naar http://localhost:6569. Een standaarddocument niet is ingesteld zodat een `HTTP Error 403.14 - Forbidden error` wordt verwacht.
4. Bevestig de genoemde in dit onderwerp TCP-poorten zijn geopend in uw firewall.
5. Bekijk het bron- of bestemming systeem:
 - Sommige systemen on-premises implementatie vereisen extra afhankelijkheidsbestanden. Bijvoorbeeld als u verbinding met on-premises implementatie SAP maakt, kan sommige extra SAP-bestanden moeten worden geïnstalleerd op de IIS-server.
 - Controleer de verbinding met het systeem met de aanmeldingsaccount. Bijvoorbeeld, moet de TCP-poorten die worden gebruikt door het systeem zijn geopend, zoals poort 1433 voor SQL Server. De aanmeldingsaccount die u hebt ingevoerd in de portal van Azure moet hebben toegang tot het systeem.
6. Schakel op de IIS-server, de gebeurtenislogboeken voor eventuele fouten. 
7. Opruimen en opnieuw installeren van de hybride Connection Manager: 
 - In IIS, moet u handmatig de website van de verbindingslijn en de bijbehorende toepassing-toepassingen verwijderen. 
 - De hybride Connection Manager opnieuw op en Bevestig dat u de juiste **Primaire configuratietekenreeks** invoert voor de verbindingslijn.



### <a name="in-the-azure-classic-portal"></a>In de klassieke Azure-portal

1. Bevestig dat de naamruimte Service Bus heeft **geactiveerd** .
2. Wanneer u de verbindingslijn maakt, voert u de verbindingsreeks van de Service Bus SA's. Voer de verbindingsreeks ACS geen.


## <a name="faq"></a>FAQ

**Vraag**: Er zijn twee hybride verbinding Managers. Wat is het verschil? 

**Answer**: Er is de [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) technologie die verbinding maken met on-premises implementatie hoofdzakelijk door Web-Apps (voorheen websites) en Mobile-Apps (voorheen mobiele services) wordt gebruikt. Deze hybride verbindingen Manager is van een eigen [setup](../biztalk-services/integration-hybrid-connection-create-manage.md) en gebruikt een Azure BizTalk-Service (achter de schermen). TCP en HTTP-protocollen ondersteunt.

Met Azure App Service verbindingslijnen hebben we ook een hybride Connection Manager.  Deze hybride Connection Manager heeft meer dan de protocollen TCP en HTTP *niet* gebruiken een Azure BizTalk Service (achter de schermen) en ondersteunt. Zie de [verbindingslijnen en de lijst met de API-Apps](app-service-logic-connectors-list.md).

Beide met Azure Service Bus verbinding maken met de on-premises-systeem.

**Vraag**: wanneer ik een aangepaste API-App maakt, kan ik met de App Service hybride Connection Manager verbinding maken met on-premises implementatie? 

**Answer**: niet in de traditionele komen. U kunt een ingebouwde verbindingslijn gebruiken, configureren van de App Service hybride Connection Manager verbinding maken met de on-premises-systeem. Gebruik deze connector vervolgens met uw aangepaste API-App, mogelijk met een logica-App. Op dit moment kan u ontwikkelen of maken van uw eigen hybride API-App (zoals de SQL-connector of het bestand connector).

Als uw aangepaste API een poort TCP of HTTP gebruikt, kunt u de [Hybride verbindingen](../biztalk-services/integration-hybrid-connection-overview.md) en de hybride Connection Manager gebruiken. In dit scenario wordt een Azure BizTalk-Service gebruikt. [Verbinding maken met een lokale SQL Server via een WebApp](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) kunt.  


## <a name="read-more"></a>Meer weten?

[Uw Apps logica controleren](app-service-logic-monitor-your-logic-apps.md)<br/>
[Service-Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
