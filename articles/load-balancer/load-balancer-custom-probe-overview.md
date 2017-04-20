<properties
  pageTitle="Laden van de verdeling van aangepaste sondes en status van de servicestatus bewaken | Microsoft Azure"
  description="Leer hoe u aangepaste sondes voor taakverdeling Azure gebruikt om de exemplaren achter taakverdeling te houden"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Meer informatie over laden verdeling sondes

Azure taakverdeling biedt de mogelijkheid om de status van exemplaren van de server de met behulp van sondes te houden. Wanneer een test niet meer reageert, stopt taakverdeling nieuwe verbindingen naar het beschadigde exemplaar verzendt. De bestaande verbindingen worden niet beïnvloed en nieuwe verbindingen in orde exemplaren worden verzonden.

Cloud service rollen (werknemer rollen en web rollen) gebruik een agent Gast voor het controleren van de test. TCP of HTTP aangepaste sondes moeten worden geconfigureerd wanneer u virtuele machines achter taakverdeling gebruikt.

## <a name="understand-probe-count-and-timeout"></a>Test tellen en time-outinstellingen begrijpen

Afhankelijk van de test gedrag:

- Het aantal succesvolle sondes waarmee een exemplaar wilt markeren als afgerond.
- Het aantal mislukte sondes die ertoe leiden dat een exemplaar wilt markeren als omlaag.

De waarde-out en de frequentie in SuccessFailCount bepalen of een exemplaar is bevestigd moeten worden uitgevoerd of niet uitgevoerd. Klik in de portal Azure is de time-out ingesteld op twee maal de waarde van de frequentie.

De test de configuratie van taakverdeling overal voor een eindpunt (dat wil zeggen een set taakverdeling) moeten hetzelfde. Dit betekent dat u kunt een andere test-configuratie voor elk exemplaar van de rol of VM geen in dezelfde gehoste service voor een combinatie van de desbetreffende eindpunt. Elk exemplaar mag bijvoorbeeld identieke lokale poorten en -outs hebben.

>[AZURE.IMPORTANT] Een test taakverdeling wordt gebruikt voor het IP-adres 168.63.129.16. Dit openbare IP-adres vergemakkelijkt communicatie met interne platform bronnen voor de voren-uw-eigenaar bent van-IP-Azure virtuele netwerk scenario. Het virtuele openbare IP-adres 168.63.129.16 wordt gebruikt in alle regio's en wordt niet gewijzigd. We raden u aan dit IP-adres in een lokale firewall-beleid. Deze niet beschouwd een beveiligingsrisico omdat alleen het interne Azure platform een bericht van dat adres kunt gegevensbron. Als u niet doet dit, wordt er onverwacht gedrag op tal van scenario's zoals het configureren van het bereik met dezelfde IP-adres van 168.63.129.16 en IP-adressen ondervindt gedupliceerd.

## <a name="learn-about-the-types-of-probes"></a>Meer informatie over de soorten sondes

### <a name="guest-agent-probe"></a>Gast agent-test

Deze test is alleen beschikbaar voor Azure-Cloudservices. Taakverdeling maakt gebruik van de Gast-agent in de virtuele computer luistert en reageert met een reactie HTTP 200 OK alleen als het exemplaar in de status gereed is (dat wil zeggen niet in een andere aangeven zoals bezet, hergebruik of stoppen).

Zie [de definitie servicebestand (csdef) configureren voor status sondes](https://msdn.microsoft.com/library/azure/ee758710.aspx) of [aan de slag voor het maken van een verdeling van de belasting internetgerichte voor cloudservices](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services)voor meer informatie.

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Wat is een gast agent test markeren van een exemplaar als beschadigd?

Als de Gast-agent niet meer reageert met HTTP 200 OK, wordt de taakverdeling Hiermee markeert u het exemplaar reageert en stopt verkeer naar die instantie verzendt. De taakverdeling blijft het exemplaar ping. Als de Gast-agent met een HTTP-200 reageert, worden de taakverdeling verkeer opnieuw naar die instantie.

Wanneer u een Webrol gebruikt, de websitecode meestal wordt uitgevoerd in w3wp.exe, waarin wordt niet gecontroleerd door de Azure stof of Gast agent. Dit betekent dat fouten in w3wp.exe (bijvoorbeeld HTTP 500 antwoorden) wordt niet aan de Gast-agent worden gemeld en de taakverdeling niet dat exemplaar afmelden bij de draaiing duurt.

### <a name="http-custom-probe"></a>Aangepaste HTTP-test

De aangepaste taakverdeling HTTP-test overschrijft de standaard Gast agent test, wat betekent dat u uw eigen aangepaste logica om te bepalen de status van het exemplaar van de rol kunt maken. De taakverdeling controleert uw eindpunt elke 15 seconden, al dan niet standaard. Het exemplaar wordt beschouwd als in de draaiing van de verdeling van laden als deze met een HTTP-200 binnen de toegestane tijd (31 seconden al dan niet standaard reageert).

Dit is handig als u uw eigen logica wilt voor het verwijderen van exemplaren van taakverdelingsrotatie implementeren. U kunt bijvoorbeeld besluit voor het verwijderen van een exemplaar als deze zich boven 90% CPU en geeft als de status van een niet-200 resultaat. Als er web rollen die w3wp.exe gebruiken, betekent dit ook dat u een automatische controle van uw website, omdat er fouten in uw websitecode retourneert de status van een niet-200 naar de test van de verdeling van laden.

>[AZURE.NOTE] De aangepaste HTTP-test ondersteunt relatieve paden en alleen van toepassing zijn op HTTP-protocol. HTTPS wordt niet ondersteund.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Wat is een aangepaste HTTP-test markeren van een exemplaar als beschadigd?

- De HTTP-toepassing geeft als resultaat een HTTP antwoordcode dan 200 (bijvoorbeeld 403, 404 of 500). Dit is een positieve bevestiging dat exemplaar van de toepassing moet worden genomen afmelden bij service direct af.
- De HTTP-server reageert niet helemaal achter de punt time-out. Afhankelijk van de time-outwaarde die is ingesteld, kan dit betekenen die meerdere test aanvraagt start voordat de test wordt gemarkeerd als niet uitgevoerd met openstaande (dat wil zeggen voordat SuccessFailCount sondes worden verzonden).
- De server Hiermee sluit u de verbinding via een TCP opnieuw instellen.

### <a name="tcp-custom-probe"></a>Aangepaste TCP-test

Een verbinding starten TCP sondes door een handshake drie manieren met de gedefinieerde poort te voeren.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Wat is een aangepaste TCP-test markeren van een exemplaar als beschadigd?

- De TCP-server reageert niet helemaal achter de punt time-out. Wanneer de test is gemarkeerd als niet uitgevoerd, is afhankelijk van het aantal mislukte test aanvragen die zijn geconfigureerd als u wilt gaan onbeantwoorde voordat u markeert de test niet uitgevoerd.
- De test ontvangt een TCP herstellen via het exemplaar van de rol.

Zie [aan de slag voor het maken van een taakverdeling internetgerichte in resourcemanager via PowerShell](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer)voor meer informatie over het configureren van een HTTP-test voor status of een TCP-test.

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Orde exemplaren weer aan taakverdelingsrotatie toevoegen

TCP en HTTP sondes worden beschouwd als orde en het exemplaar van de rol als orde wanneer markeren:

- De taakverdeling krijgt een positief test de eerste keer de VM wordt gestart.
- Het getal SuccessFailCount (hierboven) Hiermee definieert u de waarde van succesvolle sondes die nodig zijn om het exemplaar van de rol orde markeren. Als een exemplaar van de rol is verwijderd, moet het aantal geslaagd, opeenvolgende sondes gelijk of groter is dan de waarde van SuccessFailCount het exemplaar van de rol als voorlopig markeren.

>[AZURE.NOTE] Als de status van een exemplaar van de rol is veranderen, wordt de taakverdeling meer wacht voordat u het exemplaar van de rol weer aan de orde zijn. Dit gebeurt via beleid ter bescherming van de gebruiker en de infrastructuur.

## <a name="use-log-analytics-for-load-balancer"></a>Log analytics gebruiken voor de verdeling van belasting

U kunt [log analytics voor taakverdeling](load-balancer-monitor-log.md) gebruiken om te controleren of de test systeemstatus status en test tellen. Logboekregistratie kan worden gebruikt met Power BI of Azure operationele inzichten te leveren statistieken over de status van taakverdeling status.
