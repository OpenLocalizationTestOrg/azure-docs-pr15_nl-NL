<properties 
    pageTitle="Geografische Distributed schalen met App Service omgevingen" 
    description="Leer hoe u horizontaal schalen apps met behulp van geografische-verdeling met verkeer managervorm en App serviceomgevingen." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Geografische Distributed schalen met App Service omgevingen

## <a name="overview"></a>Overzicht ##
Toepassing-scenario's waarvoor zeer hoog schaal kunnen groter is dan de capaciteit van berekeningscluster resources beschikbaar zijn voor een enkele distributie van een app.  Stemmen toepassingen, sportevenementen en naar Ontspanning gebeurtenissen en zijn alle voorbeelden van scenario's waarvoor zeer hoog schaal. Vereisten voor hoge schaal kunnen worden voldaan door apps, horizontaal schalen met meerdere app implementaties binnen een bepaalde regio, alsmede tussen regio's, die wordt uitgevoerd voor het verwerken van extreme laden vereisten.

App Service omgevingen zijn een ideaal platform voor horizontale schaal af.  Één keer een App-Service-omgeving configuratie is geselecteerd die ondersteuning voor een tarief bekende aanvraag bieden, ontwikkelaars kunnen implementeren extra App serviceomgevingen op "cookie snijden" manier als u wilt bereiken van de capaciteit van een gewenste piek laden.

Stel een app uitvoeren op een App-omgeving-configuratie is getest verwerken van 20K aanvragen per seconde (RPS).  Als de gewenste piek laden capaciteit 100K RPS is, kunnen klikt u vervolgens vijf (5) App serviceomgevingen worden gemaakt en hebt geconfigureerd om ervoor te zorgen dat de toepassing kan de verwachte maximumbelasting verwerken.

Aangezien klanten toegang meestal apps met behulp van een domein aangepaste (of aangepaste), moeten ontwikkelaars een manier om de app aanvragen verdelen over alle de exemplaren van de App-omgeving.  Een uitstekende manier om dit te doen is voor het oplossen van het aangepaste domein met een [Azure verkeer Manager profiel][AzureTrafficManagerProfile].  Het profiel verkeer Manager kan worden geconfigureerd om te laten verwijzen helemaal van de afzonderlijke App Service-omgevingen.  Verkeer Manager verwerkt distributie klanten automatisch voor alle van de App-Service-omgevingen op basis van de instellingen in het profiel verkeer Manager van taakverdeling.  Deze methode werkt ongeacht of alle van de App-Service-omgevingen zijn geïmplementeerd in één Azure regio of wereldwijd tussen meerdere Azure regio's worden geïmplementeerd.

Bovendien zijn klanten niet op de hoogte van het aantal App serviceomgevingen met een app sinds klanten access-apps via het aangepaste domein.  Hierdoor kunnen ontwikkelaars snel en gemakkelijk toevoegen en verwijderen, App serviceomgevingen op basis van waargenomen verkeer laden.

De onderstaande conceptueel diagram ziet u een app horizontaal schaal af in drie App serviceomgevingen binnen één regio.

![Conceptuele architectuur][ConceptualArchitecture] 

De rest van dit onderwerp worden doorlopen de stappen die nodig zijn voor het instellen van een gedistribueerde topologie voor de steekproef-app met meerdere App Service-omgevingen.

## <a name="planning-the-topology"></a>Planning van de topologie ##
Voordat u het bouwen van een gedistribueerde app op het milieu, is het handig als u een paar onderdelen informatie tijd vooraf hebt.

- **Aangepaste domein voor de app:**  Wat is de aangepaste domeinnaam die klanten gebruiken voor toegang tot de app?  Voor de steekproef-app is de naam van het aangepaste domein *www.scalableasedemo.com*
- **Verkeer Manager-domein:**  Een domeinnaam moet worden gekozen bij het maken van een [profiel Azure verkeer Manager][AzureTrafficManagerProfile].  Deze naam wordt gecombineerd met het achtervoegsel *trafficmanager.net* de invoer in een domein dat wordt beheerd door verkeer Manager registreren.  Voor de steekproef-app is de naam van de gekozen *scalable-ase-demo*.  Hierdoor wordt de volledige domeinnaam die wordt beheerd door verkeer Manager *scalable-ase-demo.trafficmanager.net*.
- **Strategie voor schaalbaarheid van de app op het milieu:**  De toepassing op het milieu worden verdeeld over meerdere App serviceomgevingen in één regio?  Meerdere regio's?  Een combinatie-en-overeenkomst van beide methoden?  Het besluit moet worden gebaseerd op de verwachtingen van waar de klantverkeer wordt vandaan komen, evenals hoe goed de schaal van de rest van het van een app ondersteunende back-enddatabase infrastructuur kunt aanpassen.  Bijvoorbeeld, met een 100% stateless toepassing, kan een app worden massively vergroot met een combinatie van meerdere App serviceomgevingen per Azure regio, vermenigvuldigd met App-serviceomgevingen geïmplementeerd tussen meerdere Azure regio's.  Klanten met 15 + openbare Azure regio's beschikbaar waaruit u kunt kiezen, kunnen behoorlijk een milieu wereldwijd hyper-schaal toepassing maken.  Voor de steekproef-app wordt gebruikt voor dit artikel, zijn in één Azure regio (Zuid centraal ons) drie App serviceomgevingen gemaakt.
- **Naamgevingsconventie voor de App-Service-omgevingen:**  Elke App Service-omgeving moet een unieke naam.  Buiten een of twee App-omgevingen is het handig om een naamgevingsconventie om vast te stellen van elke App Service-omgeving.  Een eenvoudige naamgevingsconventie is voor de steekproef-app wordt gebruikt.  De namen van de drie App serviceomgevingen zijn *fe1ase*, *fe2ase*en *fe3ase*.
- **Naamgevingsconventie voor de apps:**  Aangezien meerdere exemplaren van de app wordt geïmplementeerd, is een naam voor elk exemplaar van de geïmplementeerd app nodig.  Eén weinig bekende maar zeer handige functie van App-serviceomgevingen is dat de naam van de dezelfde app kan worden gebruikt in meerdere App serviceomgevingen.  Aangezien elke App Service-omgeving een unieke domeinachtervoegsel heeft, kunt ontwikkelaars exacte dezelfde naam van de app opnieuw gebruiken in elke omgeving.  Een ontwikkelaar kan dus apps met de naam als volgt: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, enzovoort.  Voor de steekproef-app door elk exemplaar van de app, heeft ook een unieke naam.  De namen van de app-exemplaar gebruikt zijn *webfrontend1*, *webfrontend2*en *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Bij het instellen van het profiel verkeer Manager ##
Als u meerdere exemplaren van een app worden geïmplementeerd op meerdere App serviceomgevingen, kunnen de afzonderlijke app-exemplaren met verkeer Manager worden geregistreerd.  Voor de steekproef-app een Manager verkeer profiel nodig voor *scalable-ase-demo.trafficmanager.net* die klanten naar een van de volgende geïmplementeerd app-exemplaren doorsturen kunt:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Een exemplaar van de steekproef-app geïmplementeerd op de eerste App Service-omgeving.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Een exemplaar van de steekproef-app geïmplementeerd op de tweede App Service-omgeving.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Een exemplaar van de steekproef-app geïmplementeerd op de derde App Service-omgeving.

De eenvoudigste manier om u te registreren van meerdere Azure App Service eindpunten, alle uitgevoerd in de **dezelfde** Azure regio, wordt met de Powershell [Manager Azure-verkeer resourcemanager ondersteunen][ARMTrafficManager].  

De eerste stap is het opzetten van een profiel Azure verkeer Manager.  De onderstaande code wordt aangegeven hoe het profiel is gemaakt voor de steekproef-app:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Zoals u ziet hoe de parameter *RelativeDnsName* is ingesteld op *scalable-ase-demo*.  Dit is hoe het domein naam *scalable-ase-demo.trafficmanager.net* is gemaakt en dat is gekoppeld aan een profiel verkeer Manager.

Het beleid verkeer Manager wordt gebruikt om te bepalen hoe u het laden van de klant verspreid over alle van de beschikbare eindpunten van taakverdeling Hiermee definieert u de parameter *TrafficRoutingMethod* .  In dit voorbeeld de *gewogen* is methode gekozen.  Hierdoor wordt de klantaanvragen wordt verdeeld over alle de eindpunten van de geregistreerde toepassingen op basis van het relatieve gewicht dat is gekoppeld aan elk eindpunt. 

Met het profiel dat is gemaakt, wordt elk exemplaar van de app aan het profiel toegevoegd als een systeemeigen Azure-eindpunt.  De onderstaande code ophaalt van een verwijzing naar elke front-end van web-app, en telt vervolgens elke app als een eindpunt verkeer Manager in de parameter *TargetResourceId* .


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
U ziet hoe er één aanroep *Toevoegen-AzureTrafficManagerEndpointConfig* voor elk exemplaar van de afzonderlijke app.  De parameter *TargetResourceId* in elke Powershell-opdracht wordt verwezen naar een van de drie exemplaren die zijn geïmplementeerd app.  Het profiel verkeer Manager wordt verdeeld over laden alle drie eindpunten geregistreerd in het profiel.

Alle van de drie eindpunten dezelfde waarde (10) gebruiken voor de parameter *dikte* .  Hierdoor verkeer Manager verspreiding klantaanvragen in alle exemplaren van de drie app relatief gelijkmatig. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Aangepaste domein van de App in het domein verkeer Manager aan te wijzen ##
De laatste stap die nodig is zodat deze verwijzen van het aangepaste domein van de app in het verkeer Manager-domein.  Dit betekent wijzen *www.scalableasedemo.com* *scalable-ase-demo.trafficmanager.net*voor de steekproef-app.  Deze stap moet worden voltooid bij de domeinregistrar die worden beheerd in het aangepaste domein.  

Hulpmiddelen voor projectbeheer van uw domeinregistrar-domein gebruikt, een CNAME-records moet worden gemaakt waarin het aangepaste domein in het verkeer Manager-domein verwijst.  De onderstaande afbeelding ziet u een voorbeeld van hoe deze CNAME-configuratie eruitziet:

![CNAME voor aangepaste domein][CNAMEforCustomDomain] 

Hoewel het niet worden besproken in dit onderwerp, houd er rekening mee dat elk exemplaar van de afzonderlijke app, moet het aangepaste domein is geregistreerd bij deze ook.  Het verzoek, anders mislukt als een aanvraag kunt u met een exemplaar van de app en de toepassing geen het aangepaste domein is geregistreerd bij de app heeft.  

In dit voorbeeld is het aangepaste domein *www.scalableasedemo.com*en elk toepassingsexemplaar heeft het aangepaste domein is gekoppeld aan.

![Aangepaste domein][CustomDomain] 

Zie het volgende artikel voor een recap van een aangepast domein registreren met Azure-Service voor App-apps, voor het [registreren van aangepaste domeinen][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>De topologie Distributed uitprobeert ##
Het eindresultaat van de Manager van verkeer en DNS-configuratie is dat verzoeken om *www.scalableasedemo.com* tot en met de volgende volgorde doorloopt:

1. Een browser of het apparaat, waarmee u een zoekactie DNS voor *www.scalableasedemo.com*
2. De CNAME-vermelding bij de domeinregistrar zorgt ervoor dat de DNS-zoekopdracht wordt omgeleid naar Azure verkeer Manager.
3. Een DNS-zoekopdracht wordt uitgevoerd voor *scalable-ase-demo.trafficmanager.net* ten opzichte van een van de Azure verkeer Manager DNS-servers.
4. Op basis van het beleid (de parameter *TrafficRoutingMethod* eerder wordt gebruikt bij het maken van het profiel verkeer Manager) van taakverdeling, wordt verkeer Manager selecteert u een van de geconfigureerde eindpunten en de FQDN-naam van dat eindpunt terug naar de browser of het apparaat.
5.  Aangezien de FQDN-naam van het eindpunt de Url van een app-exemplaar uitgevoerd op een App-Service-omgeving is, vraagt de browser of het apparaat een Microsoft Azure-DNS-server de FQDN-naam omzetten in een IP-adres. 
6. De browser of het apparaat wordt verzend het verzoek HTTP/S omgezet in het IP-adres.  
7. Het verzoek aankomt in een van de app-exemplaren actief zijn voor een van de App-Service-omgevingen.

De onderstaande console-afbeelding ziet een DNS-zoekopdracht voor de steekproef-app aangepaste domein het oplossen van op een app-exemplaar uitgevoerd op een van de drie serviceomgevingen van een steekproef-App (in dit geval de tweede van de drie App Service-omgevingen):

![DNS-zoekopdracht][DNSLookup] 

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie ##
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Documentatie op de Powershell [Manager Azure-verkeer resourcemanager ondersteunen][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
