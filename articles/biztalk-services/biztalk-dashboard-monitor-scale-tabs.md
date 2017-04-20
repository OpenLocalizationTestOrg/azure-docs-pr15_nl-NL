<properties 
    pageTitle="Dashboard, beeldscherm, schaal, configureert, en hybride verbindingen in BizTalk Services | Microsoft Azure" 
    description="Meer informatie over de besturingselementen en prestaties op de tabbladen van de portal voor BizTalk Services controleren: Dashboard, beeldscherm, schaal, configureren en hybride verbindingen. MAB, WABS" 
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
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>De tabbladen Dashboard, Monitor schaal, configureren en hybride verbinding controleren

Nadat u uw BizTalk Service maken en implementeren van uw toepassing, kunt u bepaalde de BizTalk Service-instellingen wijzigen en de toepassingsprestaties controleren. 

Wanneer u de Azure klassieke portal opent, wordt u automatisch geplaatst op het tabblad **Alle ITEMS** . Weergeven van uw BizTalk Service, selecteer uw BizTalk-Service op het tabblad **Alle ITEMS** of Selecteer het tabblad **BIZTALK SERVICES** ; en selecteer vervolgens de naam van uw BizTalk-Service.

Hiermee wordt een nieuw venster geopend met de volgende tabbladen. In dit onderwerp worden de volgende tabbladen.

## <a name="quick-start-quick-startquickstart"></a>Snel aan de slag (![Snel aan de slag][QuickStart])
Afhankelijk van de BizTalk Services Edition, alle vermelde opties mogelijk niet beschikbaar. 
<table border="1">
    <tr>
        <td><strong>De hulpmiddelen voor krijgen</strong></td>
        <td>Download de SDK BizTalk-Services als u wilt de Visual Studio project-sjablonen op de lokale computer installeren. Deze sjablonen voor maken de <strong>BizTalk-Services</strong> (brug) en de <strong>Onderdelen van BizTalk-Service</strong> (Transformeren) Visual Studio-projecten die zijn geïmplementeerd in uw BizTalk Service.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hoe kan ik gebruiken de SDK van Azure BizTalk-Services</a> en de <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installatie van de SDK van Azure BizTalk-Services</a> vindt u de stappen aan de slag.
        </td>
    </tr>
    <tr>
        <td><strong>Partner overeenkomsten maken</strong></td>
        <td>Hiermee opent u de Portal Azure BizTalk diensten die worden gehost op Azure waar u partners toevoegen en X12, AS2, maken en EDIFACT EDI overeenkomsten.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Onderdelen configureren voor bewerken Messaging op BizTalk Services-Portal</a> vindt u de stappen aan de slag.
        </td>
    </tr>

<tr>
        <td><strong>Meer informatie over BizTalk-Services</strong></td>
        <td>Ga naar het <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">beheercentrum van leermateriaal</a> voor meer informatie over Azure BizTalk-Services.</td>
</tr>
</table>


In de taakbalk onder, kunt u het volgende doen:

<table border="1">

<tr>
<td><strong>Beheren</strong> uw toepassingsimplementatie van</td>
<td>Hiermee opent u de portal Azure BizTalk-Services. De BizTalk Services-Portal is de toegang tot bewerken configuratie, zoals het toevoegen van partners en het maken van X12, AS2, en EDIFACT overeenkomsten.
<br/><br/>
Het resultaat is hetzelfde als <strong>maken partner overeenkomsten</strong> op het tabblad <strong>Snel starten</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Onderdelen configureren voor bewerken Messaging op BizTalk Services-Portal</a> vindt u meer informatie op de BizTalk Services-Portal.</td>
</tr>

<tr>
<td><strong>Informatie over de databaseverbinding</strong> van de Access-besturingselement Namespace</td>
<td>Wanneer u informatie over de databaseverbinding selecteert, worden klikt u vervolgens de Access besturingselement Namespace, standaard uitgever en standaard-toets weergegeven. U kunt deze waarden kopiëren.
<br/><br/>
U kunt ook de Access-besturingselement-Portal openen. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Maak een Access-besturingselement Namespace</a> vindt u meer informatie op de Control-Portal van Access.</td>
</tr>

<tr>
<td><strong>Synchroniseren van toetsen</strong> in de opslagruimte-Account</td>
<td>Wanneer u een account opslag maakt, worden automatisch een primaire sleutel en de tweede sleutel gemaakt. Deze toetsen versleuteling beheren toegang tot uw Account opslag. De primaire sleutel automatisch wordt gebruikt door uw BizTalk-Service. <strong>Synchronisatie toetsen</strong> kunnen gebruikers om te schakelen tussen de primaire sleutel en de tweede sleutel zonder storend voor de BizTalk-Service.
<br/><br/>
U wilt bijvoorbeeld de BizTalk-Service voor het gebruik van een nieuwe primaire sleutel voor de opslag-Account. Dit wilt doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk-Service en selecteer <strong>Synchronisatie toetsen</strong>. Selecteer de tweede sleutel. Wanneer u dit doet, wordt de BizTalk-Service wordt gestart met de secundaire toets.</li>
<li>Selecteer uw account voor de opslag en genereren van de primaire sleutel in de klassieke Azure portal. Onthoud dat uw BizTalk Service gebruikt de secundaire sleutel.</li>
<li>Selecteer uw BizTalk-Service en selecteer <strong>Synchronisatie toetsen</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>Selecteer uw account opslag in de Azure klassieke portal, en de tweede sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'rollover toetsen' genoemd. Het doel is om gebruikers om te schakelen tussen de primaire sleutel en de tweede sleutel zonder storend voor de BizTalk-Service.</td>
</tr>

<tr>
<td><strong>Verwijderen</strong> uw toepassing</td>
<td>Wanneer u uw BizTalk-Service, verwijderen en alle items die zijn geïmplementeerd in deze worden verwijderd.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Afhankelijk van de BizTalk Services Edition, alle vermelde opties mogelijk niet beschikbaar. 

Wanneer u de naam van uw BizTalk Service selecteert, wordt het tabblad Dashboard wordt weergegeven. In Dashboard, kunt u het volgende doen:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Gebruik overzicht: Toont het aantal gebruikte hybride verbindingen
Het dataverbruik wordt ook weergegeven in GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrische grafiek: Een vaste lijst met prestatiegegevens worden weergegeven
Deze gegevens bieden realtime waarden voor de status van de BizTalk-Service. U kunt ook de **relatieve** of **Absolute** waarden en het tijdsbereik **Interval** van de criteria die worden weergegeven in de grafiek. 

Voor een beschrijving van deze prestatiegegevens, gaat u naar [De doelstellingen van de beschikbare](#Metrics) in dit onderwerp.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Snel aan te duiden: Lijsten de eigenschappen van uw BizTalk-Service

<table border="1">

<tr>
<td><strong>Referenties voor bijhouden van Database bijwerken</strong></td>
<td>De gebruikersnaam en wachtwoord voor het Meld u aan bij de Database voor het bijhouden van wijzigingen.</td>
</tr>
<tr>
<td><strong>SSL-certificaat bijwerken</strong></td>
<td>De BizTalk-Service voor het gebruik van een ander SSL-certificaat kan worden bijgewerkt. Een zelfondertekend SSL-certificaat wordt automatisch gemaakt wanneer u <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">de BizTalk Service maken</a>.</td>
</tr>
<tr>
<td><strong>Download-certificaat</strong></td>
<td>U kunt het SSL-certificaat dat is gebruikt door uw BizTalk Service naar een lokale computer downloaden.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Geeft de huidige status van uw BizTalk-Service. Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service staat grafiek</a>. </td>
</tr>
<tr>
<td><strong>Service-URL</strong></td>
<td>De URL van uw Service BizTalk. Het resultaat is hetzelfde als de <strong>URL voor het domein</strong> ingevoerd wanneer uw BizTalk-Service wordt gemaakt.</td>
</tr>
<tr>
<td><strong>Openbare (VIP) in de virtuele IP-adres</strong></td>
<td>Het IP-adres is toegewezen aan uw BizTalk-Service. Er wordt gebruikt voor alle invoer eindpunten en is het bronadres voor uitgaand verkeer. Dit IP-adres hoort bij uw BizTalk Service zo lang maken als deze is gemaakt. Als u de BizTalk Service verwijdert, wordt het IP-adres is toegewezen aan een andere BizTalk-Service.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>Verifieert met de BizTalk-Service.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Lijsten de editie ingevoerd wanneer de BizTalk-Service wordt gemaakt.</td>
</tr>
<tr>
<td><strong>Locatie</strong></td>
<td>Geeft de geografische regio waarop uw BizTalk-Service.</td>
</tr>
<tr>
<td><strong>Gemaakt</strong></td>
<td>Geeft de datum en tijd die de BizTalk-Service is gemaakt.</td>
</tr>
<tr>
<td><strong>Het bijhouden van de Database</strong></td>
<td>De naam van het type Azure SQL-Database waarin de tabellen van de bijhouden die worden gebruikt door uw BizTalk-Service. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten uitleg over</a> bevat informatie voor de Database bijhouden.</td>
</tr>
<tr>
<td><strong>Cmdlets voor controle/archivering opslag</strong></td>
<td>De naam van de opslag van Azure-account waarin de controle uitvoer van uw BizTalk-Service.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vereisten uitleg over</a> bevat informatie voor de opslag-account.</td>
</tr>
<tr>
<td><strong>De naam van abonnement</strong></td>
<td>Een lijst met het abonnement waarop uw BizTalk-Service. Het abonnement bestuurt de toegang tot de portal van Azure klassieke.</td>
</tr>
<tr>
<td><strong>Abonnements-ID</strong></td>
<td>Wanneer u een abonnement is gemaakt, wordt er automatisch een abonnements-ID gegenereerd. Wanneer u een REST API's gebruikt, moet u mogelijk opgeven de abonnement-ID.</td>
</tr>
</table>

[BizTalk Services: inrichten gebruik Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) vindt u de stappen om te maken van een BizTalk Service.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Beheren, informatie over de databaseverbinding, synchronisatie toetsaanslagen en verwijderen op de taakbalk weergegeven:

<table border="1">

<tr>
<td><strong>Beheren</strong> uw toepassingsimplementatie van</td>
<td>Hiermee opent u de Portal van Azure BizTalk-Services. De BizTalk Services-Portal is de toegang tot bewerken configuratie, zoals het toevoegen van partners en het maken van X12, AS2, en EDIFACT overeenkomsten.
<br/><br/>
Het resultaat is hetzelfde als <strong>maken partner overeenkomsten</strong> op het tabblad <strong>Snel starten</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Onderdelen configureren voor bewerken Messaging op BizTalk Services-Portal</a> vindt u meer informatie op de BizTalk Services-Portal.</td>
</tr>
<tr>
<td><strong>Informatie over de databaseverbinding</strong> van de Access-besturingselement Namespace</td>
<td>Geeft de Access besturingselement Namespace, standaard uitgever en standaard sleutelwaarden; die kunnen worden gekopieerd.
<br/><br/>
U kunt ook de Access-besturingselement-Portal openen. Deze Access Control-Portal is hetzelfde als met de optie Active Directory in het linkernavigatiedeelvenster.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Beheer van uw ACS Namespace</a> vindt u meer informatie op de Control-Portal van Access.</td>
</tr>
<tr>
<td><strong>Synchroniseren van toetsen</strong> in de opslagruimte-Account</td>
<td>Wanneer u een account opslag maakt, worden automatisch een primaire sleutel en de tweede sleutel gemaakt. Deze toetsen versleuteling beheren toegang tot uw Account opslag. De primaire sleutel automatisch wordt gebruikt door uw BizTalk-Service. <strong>Synchronisatie toetsen</strong> kunnen gebruikers om te schakelen tussen de primaire sleutel en de tweede sleutel zonder storend voor de BizTalk-Service.
<br/><br/>
U wilt bijvoorbeeld de BizTalk-Service voor het gebruik van een nieuwe primaire sleutel voor de opslag-Account. Dit wilt doen:
<br/><br/>
<ol>
<li>Selecteer uw BizTalk-Service en selecteer <strong>Synchronisatie toetsen</strong>. Selecteer de tweede sleutel. Wanneer u dit doet, wordt de BizTalk-Service wordt gestart met de secundaire toets.</li>
<li>Selecteer uw account voor de opslag en genereren van de primaire sleutel in de klassieke Azure portal. Onthoud dat uw BizTalk Service gebruikt de secundaire sleutel.</li>
<li>Selecteer uw BizTalk-Service en selecteer <strong>Synchronisatie toetsen</strong>. Selecteer nu de primaire sleutel. Dit is de nieuwe primaire sleutel die u opnieuw gegenereerd.</li>
<li>Selecteer uw account opslag in de Azure klassieke portal, en de tweede sleutel opnieuw genereren.</li>
</ol>
<br/>
Dit proces wordt 'rollover toetsen' genoemd. Het doel is om gebruikers om te schakelen tussen de primaire sleutel en de tweede sleutel zonder storend voor de BizTalk-Service.</td>
</tr>

<tr>
<td><strong>Verwijderen</strong> uw toepassing</td>
<td>Uw BizTalk Service en alle items die zijn geïmplementeerd in deze worden verwijderd.</td>
</tr>
</table>


## <a name="monitor"></a>Monitor
Geldt niet voor de gratis versie.

Wanneer u de naam van uw BizTalk Service selecteert, wordt het tabblad beeldscherm is beschikbaar en wordt het volgende weergegeven:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrische grafiek: De geselecteerde prestatiegegevens weergegeven
Deze gegevens bieden realtime waarden voor de status van de BizTalk-Service. U kiezen welke prestatiemetingen worden weergegeven. Maximaal zes prestatiegegevens kan tegelijk worden weergegeven. 

U kunt ook de **relatieve** of **Absolute** waarden en het tijdsbereik **Interval** van de criteria die worden weergegeven. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Om te verwijderen of de doelstellingen worden weergegeven in de grafiek:
1. Selecteer het tabblad **Beeldscherm** .
2. Selecteer **Toevoegen aan de doelstellingen** op de taakbalk weergegeven:  
![Selecteer toevoegen aan de doelstellingen][AddMetrics]
3. Controleer de prestatiegegevens die u wilt weergeven.
4. Selecteer het vinkje om terug te keren naar het tabblad **Beeldscherm** .
5. Selecteer de cirkel naast de meetwaarde van die metrisch waarde in de grafiek weergegeven.  

    Bijvoorbeeld wordt de meetwaarde **CPU-gebruik** grijs weergegeven; de uitvoer niet wordt weergegeven in de grafiek:  
![CPU-gebruik metrisch grijs wordt weergegeven][GrayedMetric]  

    Selecteer de grijs out cirkel om in te schakelen van de meetwaarde **CPU-gebruik** om de uitvoer in de grafiek weer te geven:  
![CPU-gebruik metrisch is ingeschakeld][EnabledMetric]

6. U verwijdert een meting uit in de grafiek weergeven en de lijst, selecteert u **Metrisch verwijderen** in de taakbalk. Als u wilt de metrische weer toe te voegen aan de lijst, selecteert u **Toevoegen aan de doelstellingen** in de taakbalk, de meetwaarde controleren en selecteer het vinkje om terug te keren naar het tabblad **Beeldscherm** . Selecteer de grijs out cirkel om in te schakelen van de meetwaarde.

## <a name="Metrics"></a>Aan de doelstellingen beschikbaar
De volgende items/prestatiegegevens zijn beschikbaar:

<table border="1">

<tr>
<td><strong>RountdTrip latentie</strong></td>
<td>Geeft de gemiddelde tijd in milliseconden ([ms) aan een bericht van de tijd die het bericht is ontvangen totdat het bericht volledig is verwerkt door de BizTalk Service verwerken in alle bruggen. Alleen de berichten die zijn verwerkt, worden geteld.<br/><br/>
Wanneer de volgende gebeurtenissen plaatsvinden, wordt er een tijdstempel gemaakt:
<ul>
<li>Bericht binnenkomt de gateway</li>
<li>Bericht is omgeleid naar de bestemming</li>
<li>Bestemming antwoord wordt ontvangen</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>
Deze meetwaarde geeft het resultaat van de volgende berekening:
<br/><br/>
[Bestemming bevestiging antwoord verzonden naar de gateway] - [bericht binnenkomt de gateway]</td>
</tr>
<tr>
<td><strong>Fouten bij de bron</strong></td>
<td>Wordt het totale aantal berichten die niet door de BizTalk Service wanneer berichten van de eindpunten van de bron kunnen ophalen.</td>
</tr>
<tr>
<td><strong>CPU-gebruik</strong></td>
<td>Een lijst met de gemiddelde % processortijd van de overal rol.</td>
</tr>
<tr>
<td><strong>Verwerking van latentie</strong></td>
<td>Geeft de gemiddelde tijd In milliseconden ([ms) verwerkingstijd van een bericht door de BizTalk Service over alle bruggen, met uitzondering van de tijd in bestemmingen. Alleen de berichten die zijn verwerkt, worden geteld.<br/><br/>
Wanneer elk van de volgende gebeurtenissen plaatsvinden, wordt er een tijdstempel gemaakt:

<ul>
<li>Bericht binnenkomt de gateway</li>
<li>Bericht is omgeleid naar de bestemming</li>
<li>Bestemming antwoord wordt ontvangen</li>
<li>Bestemming bevestiging antwoord verzonden naar de gateway</li>
</ul>
<br/>Deze meetwaarde geeft het resultaat van de volgende berekening:<br/><br/>
[Bestemming bevestiging antwoord verzonden naar de gateway] - [bericht binnenkomt de gateway] - [bestemming antwoord is ontvangen] + [bericht is omgeleid naar de bestemming]</td>
</tr>
<tr>
<td><strong>Fouten In het proces</strong></td>
<td>Het totale aantal berichten die is mislukt tijdens het verwerken van door de BizTalk Service over alle bruggen binnen een tijdsinterval weergegeven.</td>
</tr>
<tr>
<td><strong>Berichten die zijn verzonden</strong></td>
<td>Het totale aantal berichten die door de BizTalk Service over alle bruggen binnen een tijdsinterval weergegeven. Deze metrisch wordt verhoogd wanneer een bericht van een pijplijn de bestemming van de route bereikt. Deze metrisch betekent niet dat een bericht wordt verwerkt.<br/><br/>
In een scenario voor verzoek beantwoorden, wordt de meetwaarde wanneer de bestemming van de route een bevestiging ontvangstbevestiging terug naar de pijplijn stuurt verhoogd.</td>
</tr>
<tr>
<td><strong>Berichten ontvangen</strong></td>
<td>Het totale aantal berichten ontvangen door de BizTalk Service over alle bruggen binnen een tijdsinterval weergegeven. Deze metrisch wordt verhoogd wanneer een nieuw bericht is ontvangen door de pijplijn.</td>
</tr>
<tr>
<td><strong>Berichten In het proces</strong></td>
<td>Het totale aantal berichten momenteel door de BizTalk Service verwerkt binnen een tijdsinterval weergegeven.</td>
</tr>
<tr>
<td><strong>Berichten die worden verwerkt</strong></td>
<td>Het totale aantal berichten verwerkt door de BizTalk Service over alle bruggen binnen een tijdsinterval weergegeven. Deze metrisch wordt verhoogd wanneer een bericht is ontvangen door de pijplijn en is doorgestuurd naar de bestemming.</td>
</tr>
</table>


## <a name="scale"></a>Schaal
Het tabblad Schaal kunt u optellen of aftrekken van het aantal eenheden dat is gebruikt door uw BizTalk-Service. Standaard is er een eenheid geconfigureerd. Aanvullende eenheden kunnen worden toegevoegd aan de nieuwe schaal uw BizTalk-Service. Wanneer u de schaal vergroot, bent u doorvoer vergroten. De hoeveelheid resources ook Hiermee verhoogt u, inclusief geïmplementeerd bruggen, overeenkomsten, LOB-verbindingen en het verwerken van power. Bijvoorbeeld, vergroot u de schaal van 1 eenheid tot 2 eenheden. In dit geval kunt u dubbele het aantal bruggen implementeren, dubbelklik de overeenkomsten, dubbelklik de LOB-verbindingen en dubbelklik de van verwerkingskracht.

Sommige edities BizTalk bieden geen een optie voor schaal. In dit geval is als u één eenheid toegestaan. Om te bepalen hoeveel eenheden uw edition kan worden vergroot, raadpleegt u [BizTalk Services: edities grafiek](biztalk-editions-feature-chart.md).

Het aantal eenheden met groter wordende mogelijk van invloed prijzen. Als u de eenheden die zijn vergroot, wordt een bericht dat u als de facturering wordt beïnvloed selecteren **Opslaan** wordt weergegeven. U kiest om door te gaan. Wanneer u het aantal eenheden, de wijzigingen van de BizTalk Service-status van actief in bijwerken vergroten. Uw BizTalk Service blijft in de stand bijwerken om uit te voeren.

[BizTalk Services: edities grafiek](biztalk-editions-feature-chart.md) een 'eenheid"definieert.


## <a name="configure"></a>Configureren
Geldt niet voor hybride verbindingen.

De back-Status wordt ingesteld op geen of automatische. Als de waarde op geen, worden geen back-ups automatisch gemaakt. Als de waarde op automatisch, configureren u de back-locatie, de frequentie van de back-up, en hoe lang wilt behouden de back-upbestanden. 

[BizTalk Services: back-up maken en terugzetten](biztalk-backup-restore.md) bevat de informatie. 


## <a name="HybridConnections"></a>Hybride verbindingen
Hybride verbindingen verbinden een Azure-toepassing, zoals Web Apps of Mobile-Apps in Azure App-Service, met een on-premises implementatie resource die een statische TCP-poorten, zoals SQL Server, MySQL HTTP-Web-API's en de meeste aangepaste Web-Services gebruikt. Hybride verbindingen worden beheerd in BizTalk Services in de portal van Azure klassieke.

Zie [Access on-premises implementatie van hybride-verbindingen in Azure App Service resources](../app-service-web/web-sites-hybrid-connection-get-started.md)hybride verbindingen maken in Azure App-Service.

Als u wilt maken of beheren hybride verbindingen in Azure BizTalk-Services, raadpleegt u [Hybride verbindingen](integration-hybrid-connection-overview.md).



## <a name="next"></a>Volgende
Nu dat u bekend met de andere tabbladen bent, kunt u meer informatie over de onderdelen van de Azure BizTalk Services:

- [BizTalk-Services: beperken](biztalk-throttling-thresholds.md)  
- [BizTalk Services: De naam van de uitgever en de uitgever sleutel](biztalk-issuer-name-issuer-key.md)  
- [BizTalk Services: Back-up maken en deze herstellen](biztalk-backup-restore.md)

## <a name="see-also"></a>Zie ook
- [Hybride verbindingen](integration-hybrid-connection-overview.md)  
- [BizTalk Services: Ontwikkelaars, Basic, Standard en Premium edities grafiek](biztalk-editions-feature-chart.md)  
- [BizTalk Services: Inrichting klassieke portal Azure gebruiken](biztalk-provision-services.md)  
- [BizTalk Services: BizTalk servicestatus grafiek](biztalk-service-state-chart.md)  
- [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
