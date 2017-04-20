Resource|Vrij te geven|Gedeelde (Preview)|Eenvoudige|Standaard|Premium (Preview)</th>
---|---|---|---|---|---
[Web, mobiel of API apps](https://azure.microsoft.com/services/app-service/) per [App-abonnement](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Onbeperkte<sup>2</sup>|Onbeperkte<sup>2</sup>|Onbeperkte<sup>2</sup>
[Logica-apps](https://azure.microsoft.com/services/app-service/logic/) per [App serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 per core|20 per core
[App-abonnement](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 per regio|10 per resourcegroep|100 per resourcegroep|100 per resourcegroep|100 per resourcegroep
Exemplaartype berekenen|Gedeeld|Gedeeld|Speciale<sup>3</sup>|Speciale<sup>3</sup>|Speciale<sup>3</sup></p>
[Schalen](../articles/app-service-web/web-sites-scale.md) (max exemplaren)|1 gedeeld|1 gedeeld|3 speciale<sup>3</sup>|10 speciale<sup>3</sup>|20 specifiek (50 in ASE)<sup>3,4</sup>
Opslag-<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU tijd (korte naam)<sup>6</sup>|3 minuten|3 minuten|Onbeperkt, betalen bij standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a>|Onbeperkt, betalen bij standaardtarieven|Onbeperkt, betalen bij standaardtarieven
CPU tijd (dag)<sup>6</sup>|60 minuten|240 minuten|Onbeperkt, betalen bij standaard [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a>|Onbeperkt, betalen bij standaardtarieven|Onbeperkt, betalen bij standaardtarieven
Geheugen (1 uur)|1024 MB per App-abonnement|1024 MB per app|N/B|N/B|N/B
Bandbreedte|165 MB|Onbeperkt, [gegevens overbrengen tarieven](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen|Onbeperkt, overdracht van gegevens tarieven toepassen|Onbeperkt, overdracht van gegevens tarieven toepassen|Onbeperkt, overdracht van gegevens tarieven toepassen
Toepassingsarchitectuur|32-bits|32-bits|32-bits/64-bits|32-bits/64-bits|32-bits/64-bits
Web Sockets per exemplaar<sup>7</sup>|5|35|350|Onbeperkt|Onbeperkt
Gelijktijdige [Foutopsporing verbindingen](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) per toepassing|1|1|1|5|5
[azurewebsites.NET subdomein met FTP/S en SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
Ondersteuning voor [aangepaste domein](../articles/app-service-web/web-sites-custom-domain-name.md)||X|X|X|X
Aangepaste domein [SSL ondersteunen](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Onbeperkt|Onbeperkt, 5 SNI SSL en 1 IP SSL-verbindingen opgenomen|Onbeperkt, 5 SNI SSL en 1 IP SSL-verbindingen opgenomen
Geïntegreerde taakverdeling||X|X|X|X
[Permanente](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Geplande back-ups](../articles/app-service-web/web-sites-backup.md)||||Één keer per dag|Eenmaal elke 5 minuten-<sup>8</sup>
[Automatisch schalen](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) ondersteuning||X|X|X|X
[Eindpunt bewaken](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Tijdelijke sleuven (Preview)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Aangepaste domeinen per app</a>||500|500|500|500
SLA||<p>|99,9%|99.95%<sup>10</sup>|99.95%<sup>10</sup>

<sup>1</sup> Apps en de opslagquota zijn per App serviceplan tenzij anderszins vermeld.  
<sup>2</sup> Het werkelijke aantal apps die u op deze apparaten hosten kunt, is afhankelijk van de activiteit van de apps, de grootte van de exemplaren van de computer en het bijbehorende Resourcegebruik.  
<sup>3</sup> Speciale exemplaren kunnen van verschillende grootte zijn. Zie de [App-serviceprijzen](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) voor meer informatie.  
<sup>4</sup> Premium laag kan maximaal 50 berekent exemplaren (afhankelijk van beschikbaarheid) en 500 GB schijfruimte bij gebruik van de App serviceomgevingen en 20 exemplaren en 250 GB opslagruimte anders berekenen.  
<sup>5</sup> De opslaglimiet is de totale grootte van de inhoud in alle apps in het dezelfde App Service-abonnement. Meer opslagruimte opties zijn beschikbaar in de [App-omgeving](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Deze resources worden beperkt door fysieke bronnen op het speciale exemplaren (het exemplaar-formaat en het aantal exemplaren).  
<sup>7</sup> Als u een app in de eenvoudige laag in twee exemplaren wilt verkleinen, hebt u 350 verbindingen voor elk van de twee instanties.  
<sup>8</sup> Premium laag kunt back-intervallen omlaag naar elke 5 minuten bij gebruik van de App serviceomgevingen, en 50 keer per dag anderszins.  
<sup>9</sup> Aangepaste uitvoerbare bestanden en/of scripts worden uitgevoerd op aanvraag, volgens een schema of continu als een achtergrondtaak binnen uw exemplaar van de App-Service. Altijd op is vereist voor doorlopende WebJobs uitvoeren. Azure Scheduler gratis of standaard is vereist voor de geplande WebJobs. Er is geen vooraf gedefinieerde limiet van het aantal WebJobs die kan worden uitgevoerd in een exemplaar van de App Service, maar er zijn praktische limieten die afhankelijk zijn van wat de toepassingscode probeert te doen.   
<sup>10</sup> SLA van 99.95% is opgegeven voor die gebruikmaken van meerdere exemplaren met Azure verkeer Manager is geconfigureerd voor failover.  
