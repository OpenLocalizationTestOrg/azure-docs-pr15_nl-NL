<properties
    pageTitle="Overzicht van de hybride gegevensverbindingen | Microsoft Azure"
    description="Meer informatie over hybride verbindingen, beveiliging, TCP-poorten en ondersteunde configuraties. MAB, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Overzicht van de hybride-gegevensverbindingen
Inleiding tot hybride verbindingen, worden de ondersteunde configuraties en bevat de vereiste TCP-poorten.


## <a name="what-is-a-hybrid-connection"></a>Wat is de verbinding van een hybride

Hybride verbindingen zijn een functie van Azure BizTalk-Services. Hybride verbindingen bieden een eenvoudige en handige manier om verbinding te maken van de Web Apps-functies in Azure App-Service (voorheen Websites) en de Mobile-Apps in Azure App-Service (voorheen Mobile Services) met lokale bronnen achter de firewall.

![Hybride verbindingen][HCImage]

Hybride verbindingen-voordelen:

- WebApps en Mobile-Apps hebben toegang tot bestaande on-premises gegevens en -services veilig.
- Meerdere Web Apps of Mobile-Apps kunt delen van een hybride-verbinding voor toegang tot een on-premises implementatie resource.
- Minimale TCP-poorten zijn vereist voor toegang tot uw netwerk.
- Toepassingen hybride verbindingen met toegang tot de specifieke on-premises implementatie-resource die is gepubliceerd via de hybride verbinding.
- Kunt verbinden met een on-premises implementatie-resource die gebruikmaakt van een statische TCP-poorten, zoals SQL Server, MySQL HTTP-Web-API's en de meeste aangepaste Web-Services.

    > [AZURE.NOTE] TCP gebaseerde services met dynamische poorten (zoals FTP passieve modus of passieve modus Uitgebreide) worden momenteel niet ondersteund. LDAP wordt ook niet ondersteund. LDAP werkt met een statische TCP-poorten, maar dit kan ook UDP gebruiken. Hierdoor wordt niet ondersteund.

- Kan worden gebruikt met alle kaders worden ondersteund door de Web-Apps (.NET, PHP, Java, Python, Node.js) en Mobile-Apps (Node.js, .NET).
- WebApps en Mobile-Apps toegang tot lokale bronnen op precies dezelfde manier als wanneer het Web of de Mobile-App bevindt zich op het lokale netwerk. Bijvoorbeeld kunnen de dezelfde verbinding tekenreeks die wordt gebruikt on-premises implementatie ook worden gebruikt op Azure.


Hybride verbindingen bieden ook beheerders van het bedrijf met control en inzicht in de bedrijfsresources toegankelijk voor hybride toepassingen, waaronder:

- Werken met groepsbeleidsinstellingen, beheerders kunnen hybride verbindingen toestaan op het netwerk en ook aanwijzen resources die toegankelijk is voor hybride-toepassingen.
- Logboeken aan de gebeurtenis en controle op het bedrijfsnetwerk bevinden bieden inzicht in de resources die zijn toegankelijk voor hybride verbindingen.


## <a name="example-scenarios"></a>Voorbeelden van scenario 's

Hybride verbindingen ondersteunen de volgende framework en -toepassing:

- .NET framework toegang tot SQL Server
- .NET framework toegang tot HTTP-/ HTTPS-services met WebClient
- PHP toegang tot SQL Server, MySQL
- Java-toegang tot SQL Server, MySQL en Oracle
- Java-toegang tot HTTP-/ HTTPS-services

Wanneer u een hybride verbindingen gebruikt voor toegang tot lokale SQL Server, het volgende overwegen:

- SQL Express benoemde exemplaren moet worden geconfigureerd voor gebruik van statische poorten. Standaard naam SQL Express exemplaren dynamische poorten gebruiken.
- SQL Express standaard exemplaren een statische poort gebruiken, maar TCP moet zijn ingeschakeld. TCP is standaard niet ingeschakeld.
- Wanneer u cluster of beschikbaarheidsgroepen, gebruikt de `MultiSubnetFailover=true` modus wordt momenteel niet ondersteund.
- De `ApplicationIntent=ReadOnly` wordt momenteel niet ondersteund.
- Het is mogelijk dat SQL-verificatie vereist als de autorisatie voor end-to-end-methode ondersteund door de Azure-toepassing en de lokale SQL server.


## <a name="security-and-ports"></a>Beveiligings- en -poorten

Hybride verbindingen gebruik autorisatie voor gedeelde Access handtekening (SA's) u de verbindingen van de Azure-toepassingen en de on-premises implementatie hybride Connection Manager met de verbinding hybride beveiligt. Toetsen van afzonderlijke verbinding worden gemaakt voor de toepassing en de on-premises implementatie hybride Connection Manager. Deze toetsen verbinding kunnen worden overgebracht en onafhankelijk ingetrokken.

Hybride verbindingen bieden voor naadloze en veilige verdeling van de sleutels tot de toepassingen en de on-premises implementatie hybride Connection Manager.

Zie [hybride verbindingen maken en beheren](integration-hybrid-connection-create-manage.md).

*Toepassing autorisatie staat los van de hybride verbinding*. Elke methode juiste machtigingen kan worden gebruikt. De methode autorisatie, is afhankelijk van de autorisatie voor end-to-end-methoden ondersteund tussen de Azure cloud en de onderdelen van de on-premises implementatie. Uw Azure-toepassing wordt bijvoorbeeld een lokale SQL Server. In dit scenario mogelijk SQL autorisatie de autorisatie-methode die wordt ondersteund end-to-end.

#### <a name="tcp-ports"></a>TCP-poorten
Hybride verbindingen nodig alleen uitgaande verbinding met TCP of HTTP uit uw persoonlijke netwerk. U hoeft niet alle firewallpoorten openen of wijzigen van de omtrek en configureren van uw netwerk zodat alle binnenkomende verbindingen in uw netwerk.

De volgende TCP-poorten worden gebruikt door de hybride verbindingen:

Poort | Waarom u deze nodig hebt
--- | ---
9350 - 9354 | Deze poorten worden gebruikt voor de gegevensoverdracht. De Service Bus relay manager controleert poort 9350 om te bepalen of TCP-verbinding beschikbaar is. Als deze beschikbaar is, klikt u vervolgens wordt ervan uitgegaan dat poort 9352 ook beschikbaar is. Gegevensverkeer is gewijd aan poort 9352. <br/><br/>Sta uitgaande verbindingen tot deze poorten toe.
5671 | Wanneer poort 9352 wordt gebruikt voor gegevens-verkeer is toegestaan, wordt poort 5671 wordt gebruikt als het besturingselement kanaal. <br/><br/>Sta uitgaande verbindingen naar deze poort toe.
80, 443 | Deze poorten worden gebruikt voor sommige gegevensaanvragen voor Azure. Ook poort 9352 en 5671 zijn niet kan worden gebruikt, zijn *klikt u vervolgens* de poort 80 en 443 wanneer de fallback poorten gebruikt voor de gegevensoverdracht en het besturingselement kanaal.<br/><br/>Sta uitgaande verbindingen tot deze poorten toe. <br/><br/>**Opmerking** Het verdient aanbeveling geen gebruik van deze als de fallback poorten in plaats van de andere TCP-poorten. De HTTP/WebSocket wordt gebruikt als het protocol in plaats van systeemeigen TCP voor gegevenskanalen. Dit kan resulteren in lagere prestaties.



## <a name="next-steps"></a>Volgende stappen

[Hybride verbindingen maken en beheren](integration-hybrid-connection-create-manage.md)<br/>
[Azure-WebApps verbinden met een On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Verbinding maken met een lokale SQL Server vanuit een Azure web-app](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Zie ook

[REST API voor het beheren van BizTalk-Services op Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk Services: edities grafiek](biztalk-editions-feature-chart.md)<br/>
[Een BizTalk-Service met behulp van Azure portal maken](biztalk-provision-services.md)<br/>
[BizTalk Services: De tabbladen Dashboard, Monitor en schaal](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
