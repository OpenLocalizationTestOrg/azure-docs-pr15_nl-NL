<properties
   pageTitle="ADFS implementeren in Azure | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met Active Directory Federation Services autorisatie in Azure wordt aangegeven."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Implementatie van Azure Active Directory Federation Services (ADFS)

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel worden aanbevolen procedures voor de uitvoering van een beveiligde hybride netwerk die doorloopt in uw on-premises netwerk naar Azure en die gebruikmaakt van [Active Directory Federation Services (ADFS)] [ active-directory-federation-services] om uit te voeren federatieve verificatie en machtiging voor onderdelen die worden uitgevoerd in de cloud. Deze architectuur breidt de structuur door [Active Directory uitbreiden naar Azure]beschreven[implementing-active-directory].

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze verwijzing architectuur gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

ADFS on-premises implementatie kan worden uitgevoerd kan, maar in een hybride scenario waar toepassingen in Azure bevinden zich efficiënter deze functionaliteit implementeren in de cloud. Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Hybride toepassingen waar werkbelasting uitvoeren gedeeltelijk on-premises implementatie en deels in Azure.

- Oplossingen die gebruikmaken van federatieve autorisatie om weer te geven van webtoepassingen naar partnerorganisaties.

- Systemen die ondersteuning bieden voor toegang via webbrowsers uitgevoerd buiten de organisatie-firewall.

- Systemen waarmee gebruikers kunnen de toegang tot uw webtoepassingen door verbinding te maken van geautoriseerde externe apparaten, zoals een externe computers, notitieblokken en andere mobiele apparaten. 

Zie [Overzicht van Active Directory Federation Services]voor meer informatie over de werking van ADFS,[active-directory-federation-services-overview]. Bovendien het artikel [ADFS-implementatie in Azure] [ adfs-intro] bevat een gedetailleerde stapsgewijze inleiding tot het implementeren van ADFS in Azure wordt aangegeven.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur (*Klik op om in te zoomen*) gemarkeerd. Lees voor meer informatie over elk element die niet zijn gerelateerd aan ADFS, [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven][implementing-a-secure-hybrid-network-architecture], [uitvoering van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], en [implementeren van een beveiligde hybride netwerkarchitectuur met Active Directory-identiteiten in Azure][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] In dit diagram ziet u de volgende gevallen van gebruiken:
>
>- Toepassingscode die wordt uitgevoerd in een andere organisatie toegang heeft tot een webtoepassing gehost binnen uw VNet Azure.
>
>- Een externe, de geregistreerde gebruiker (met de referenties die zijn opgeslagen in Hiermee) toegang krijgen tot een webtoepassing gehost binnen uw VNet Azure.
>
>- Een gebruiker verbinding maakt met uw VNet met een geautoriseerde apparaat en uitvoeren van een webtoepassing gehost binnen uw VNet Azure.
>
>Daarnaast kunnen deze architectuur bevat informatie over passieve Federatie, waar de federatieservers hoe en wanneer bepalen een gebruiker te verifiëren. De gebruiker wordt naar verwachting aanmelden informatie opgeven wanneer een toepassing wordt gestart. Dit is het meest worden gebruikt door webbrowsers om en heeft betrekking op een protocol waarmee de browser omgeleid naar een site waarin de gebruiker hun referenties kunt geven. ADFS ondersteunt ook actieve Federatie waarbij een toepassing krijgt verantwoordelijk voor het opgeven van referenties zonder verdere interactie van de gebruiker, maar deze zaak is buiten het bereik van deze architectuur.

- **Hiermee subnet.** De Hiermee-servers bevinden zich in hun eigen subnet. Regels voor NSG de servers Hiermee beveiligt en een firewall tegen verkeer uit onverwachte bronnen kunnen bevatten.

- **Servers toegevoegd.** Hierna ziet u domeincontrollers als VMs in de cloud. Deze servers kunnen verificatie voor lokale identiteiten binnen het domein bieden.

- **ADFS subnet.** De ADFS-servers kunnen bevinden zich in hun eigen subnet, met NSG regels die fungeert als een firewall.

- **ADFS-servers.** De ADFS-servers bieden federatieve autorisatie en verificatie. In deze architectuur uitvoeren ze de volgende taken:

    - Vorderingen die zijn aangebracht door een federatieserver partner namens een partner met beveiligingstokens, kunnen ze ontvangen. ADFS kunt controleren of deze tokens zijn geldig voor het doorgeven van de claims naar webtoepassing uitgevoerd in Azure wordt aangegeven. De bedrijfs-webtoepassing (in Azure) kunt u deze claims gebruiken om het Autoriseer aanvragen. In dit scenario de bedrijfs-webtoepassing is de *te vertrouwen van derden*, en de verantwoordelijkheid van de partner federatieserver uitgeven vorderingen die worden begrepen door de bedrijfs-webtoepassing. De partner-federatieservers worden genoemd *accountpartners* omdat ze toegangsaanvragen namens geverifieerde accounts niet in de andere organisatie indienen. De ADFS-servers worden *resource partners* genoemd omdat ze toegang tot resources (webtoepassingen, in dit geval bieden).

    - Ze kunnen verifiëren (via Hiermee en de [Active Directory apparaat registratieservice][ADDRS]) en Autoriseer verzoeken voor oproepen van externe gebruikers met een webbrowser of het apparaat waarop toegang tot uw zakelijke webtoepassingen nodig heeft. 

    De ADFS-servers zijn geconfigureerd als een farm, via een Azure taakverdeling. Deze structuur kunt beschikbaarheid en schaalbaarheid te verbeteren. Bedenk ook, dat de ADFS-servers worden niet rechtstreeks blootgesteld aan Internet, liever alle internetverkeer tot en met ADFS web application-proxyservers en een DMZ wordt gefilterd.

- **AD FS-proxy subnet.** De AD FS-proxyservers kunnen worden opgenomen in hun eigen subnet, met NSG regels die bescherming bieden. De servers in dit subnet worden blootgesteld aan Internet via een set netwerk virtuele toestellen waarmee een firewall tussen uw Azure virtuele netwerk en Internet.

- **ADFS web application-proxyservers (WAP).** Deze computers fungeren als ADFS-servers voor binnenkomende aanmeldingsaanvragen van de partnerorganisaties en externe apparaten. De servers WAP fungeren als een filter, de ADFS-servers tegen directe toegang vanuit de openbare Internet afscherming. Als u met de ADFS-servers implementeren van het draadloze Toegangspunt servers in een farm met taakverdeling biedt groter beschikbaarheid en schaalbaarheid dan het implementeren van een verzameling zelfstandige servers.

    >[AZURE.NOTE] Zie voor gedetailleerde informatie over het installeren van WAP servers [installeren en configureren van de proxyserver van de Web-toepassing][install_and_configure_the_web_application_proxy_server]

- **Partnerorganisatie.** Dit is een voorbeeld partner-organisatie, die wordt uitgevoerd van een webtoepassing die toegang aanvraagt tot de webtoepassing uitgevoerd in Azure wordt aangegeven. De federatieserver van de andere organisatie wordt geverifieerd door lokaal aanvragen en beveiligingstokens claims naar ADFS uitgevoerd in Azure wordt aangegeven met verstuurt. De beveiligingstokens is gevalideerd met ADFS in Azure wordt aangegeven en dat ze geldig zijn doorgegeven de claims naar de webtoepassing uitgevoerd in Azure toe te staan.

    >[AZURE.NOTE] U kunt ook een VPN-tunnel Azure Gateway met directe toegang geven tot ADFS voor vertrouwde partners configureren. Aanvragen ontvangen van deze partners doorgeeft geen tot en met de servers WAP.

## <a name="recommendations"></a>Aanbevelingen

In deze sectie bevat een overzicht van de aanbevelingen voor de uitvoering van ADFS in Azure wordt aangegeven, die betrekking hebben op:

- VM aanbevelingen.

- Aanbevelingen voor netwerken.

- Beschikbaarheid aanbevelingen.

- Aanbevelingen voor beveiliging.

- Aanbevelingen voor ADFS-installatie.

- Aanbevelingen ADFS vertrouwen.

### <a name="vm-recommendations"></a>VM aanbevelingen

Maak VMs met voldoende resources worden afgehandeld van het verwachte volume verkeer. De grootte van de bestaande machines hostingprovider ADFS on-premises als uitgangspunt gebruiken. Het Resourcegebruik bewaken. U kunt het formaat van de VMs en verkleinen als ze te groot is zijn.

Volg de aanbevelingen weergegeven in [een Windows-VM op Azure uitgevoerd][vm-recommendations].

### <a name="networking-recommendations"></a>Netwerken aanbevelingen

Configureer de netwerkinterface voor elk van de VMs hostingprovider ADFS en WAP servers met statische privé IP-adressen.

Geef de ADFS-VMs openbare IP-adressen niet. Zie voor meer informatie [beveiligingsoverwegingen][security-considerations].

Stel het IP-adres van de voorkeur en secundaire DNS-servers voor de netwerkinterfaces voor elke ADFS en WAP VM om te verwijzen naar de telt VMs (dat moet worden DNS wordt uitgevoerd). Deze stap is nodig om in te schakelen elke VM deel te nemen aan het domein.

### <a name="availability-recommendations"></a>Beschikbaarheid van aanbevelingen

Maak een ADFS-farm met ten minste twee servers om beschikbaarheid van de ADFS-service te vergroten.

Gebruik verschillende opslag accounts voor elke VM ADFS in de farm. Deze methode helpt om ervoor te zorgen dat een fout in een enkel opslag-account geen de gehele farm niet toegankelijk maakt.

Beschikbaarheid van afzonderlijke Azure sets maken voor het ADFS en WAP VMs. Zorg ervoor dat er ten minste twee VMs in elke set. Elke set beschikbaarheid moet ten minste twee update-domeinen en twee foutenstructuuranalyse domeinen hebben.

Configureer de netwerktaakverdelers voor de ADFS-VMs en WAP VMs als volgt:

- Gebruik een Azure taakverdeling voor externe toegang tot het draadloze Toegangspunt VMs en een interne taakverdeling distributie van het selectievakje laden voor de ADFS-servers in de ADFS-farm.

- Alleen doorgeven verkeer op poort 443 (HTTPS) met de servers ADFS/WAP wordt weergegeven.

- Geeft de taakverdeling een statische IP-adres.

- Maak een systeemstatus-test met de TCP-protocol in plaats van HTTPS. U kunt pingen poort 443 om te controleren of er een ADFS-server functioneert.

    >[AZURE.NOTE] ADFS-servers gebruikmaken van het protocol Server naam aanduiding (SNI), zodat u probeert te zoeken met een HTTPS-eindpunt van de verdeling van mislukt van het laden.

- Een DNS- *A* -record toevoegen op het domein voor de verdeling van de ADFS-belasting. 

    Geef het IP-adres van de taakverdeling en geef deze een naam in het domein (zoals adfs.contoso.com). Dit is de naam waarmee clients en de servers WAP toegang de ADFS-server-farm tot.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

Voorkomen dat de ADFS-servers directe blootgesteld aan Internet. ADFS-servers zijn domein behoren computers waarop volledige autorisatie beveiligingstokens verlenen. Als een ADFS-server betrouwbaar is, kan een kwaadwillende gebruiker tokens volledige toegang verlenen voor alle webtoepassingen en voor federatieservers die zijn beveiligd met ADFS. Als aanmeldingsaanvragen van externe gebruikers die niet per se verbinding te maken van partnersites vertrouwde moet worden verwerkt door het systeem, gebruikt u WAP servers om deze aanvragen te verwerken. Zie voor meer informatie, [een Federatieserverproxy locatie][where-to-place-an-fs-proxy].

Plaats ADFS-servers en WAP servers in afzonderlijke subnetten met hun eigen firewalls. U kunt regels voor NSG firewallregels definiëren. Als u meer uitgebreide bescherming vereist kunt u de omtrek van een extra beveiliging rond servers implementeren met behulp van een paar subnetten en NVAs, zoals wordt beschreven door het [implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure]document[implementing-a-secure-hybrid-network-architecture-with-internet-access]. Houd er rekening mee dat alle firewalls mag worden verkeer op poort 443 (HTTPS).

Rechtstreekse aanmeldingstoegang tot de servers ADFS en WAP beperken. Alleen DevOps personeel moet mogelijk om verbinding te maken.

U niet deelnemen aan de servers WAP op het domein.

### <a name="adfs-installation-recommendations"></a>Aanbevelingen voor ADFS-installatie

Het artikel [een serverfarm Federatie implementeert] [ Deploying_a_federation_server_farm] vindt u gedetailleerde instructies voor het installeren en configureren van ADFS. De volgende taken uitvoeren voordat u de eerste ADFS-server in de farm configureren:

1. Een openbaar vertrouwde certificaat voor de uitvoering van serververificatie aanvragen. De *onderwerpnaam* moet de naam waarmee klanten toegang de federation-service tot bevatten. Dit is de naam van de DNS-geregistreerd voor de verdeling van de belasting, bijvoorbeeld *adfs.contoso.com* (vermijden namen van jokertekens zoals **. contoso.com*, vanwege de beveiliging). Gebruik hetzelfde certificaat op alle VMs voor ADFS-server. U kunt een certificaat van een vertrouwde certificeringsinstantie aanschaffen, maar als uw organisatie gebruikmaakt van Active Directory Certificate Services kunt u uw eigen. 

    De *alternatieve onderwerpnaam* wordt gebruikt door de DRS toegang van externe apparaten in te schakelen. Dit moet van het formulier *enterpriseregistration.contoso.com*.

    Zie voor meer informatie [verkrijgen en een SSL-certificaat voor ADFS configureren][adfs_certificates].

2. Op de domeincontroller, een nieuwe sleutel in de hoofdmap voor de verdeling Key Service te genereren. Stel de effectieve tijd wordt de huidige tijd min 10 uur (deze configuratie Hiermee reduceert u de vertraging die zich voordoen kan bij distribueren en synchronisatie van toetsen in het domein). Deze stap is voor het maken van de groep serviceaccount die worden gebruikt voor het uitvoeren van de ADFS-service nodig. De volgende Powershell-opdracht toont een voorbeeld van hoe u dit wilt doen:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Elke ADFS-server VM toevoegen aan het domein.

>[AZURE.NOTE] Als u wilt ADFS hebt geïnstalleerd, moet de domeincontroller met de primaire domeincontroller emulator FSMO-functie voor het domein actief zijn en toegankelijk zijn vanuit de ADFS-VMs.

### <a name="adfs-trust-recommendations"></a>Aanbevelingen ADFS vertrouwen

Tot stand brengen federation-vertrouwensrelatie tussen uw ADFS-installatie en de federatieservers van een partnerorganisaties. Een op claims filteren en de toewijzing vereist configureren. Dit proces is vereist:

- DevOps medewerkers **van elke andere organisatie** als u wilt toevoegen van een gebruikmakende partij vertrouwen voor de webtoepassingen die toegankelijk is via uw ADFS-servers.

- DevOps medewerkers **in uw organisatie** om te configureren op claims-provider vertrouwen zodat uw ADFS-servers om te vertrouwen de claims die partnerorganisaties bieden.

- DevOps medewerkers **in uw organisatie** om te configureren ADFS doorgeven van claims bij webtoepassingen van uw organisatie.

    Zie voor meer informatie, [Tot stand te brengen Federatie vertrouwen][establishing-federation-trust].

Publiceren van webtoepassingen van uw organisatie en beschikbaar maken voor externe partners met behulp van verificatie via de WAP-servers. Zie voor meer informatie, [toepassingen publiceren Gebruik vooraf van ADFS-verificatie.][publish_applications_using_AD_FS_preauthentication]

Houd er rekening mee dat ADFS token-transformatie en vergroting ondersteunt. Azure Active Directory biedt geen deze functie. Met ADFS, bij het instellen van de vertrouwensrelaties, kunt u:

- Configureer claimen transformaties voor autorisatieregels. U kunt bijvoorbeeld, Groepsbeveiliging van een weergave die wordt gebruikt door de partnerorganisatie van een niet-Microsoft-aan een item dat die hiermee in uw organisatie kunt Autoriseer toewijzen.

- Op claims van de ene indeling naar een andere transformeren. Bijvoorbeeld: u kunt toewijzen van SAML 2.0 aan SAML 1.1 als de toepassing alleen SAML 1.1 claims ondersteunt. 

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

U kunt SQL Server- of Windows interne Database (WID) hebben ADFS-configuratiegegevens voor. WID biedt eenvoudige redundantie. Wijzigingen worden geschreven rechtstreeks naar slechts één van de ADFS-databases in de ADFS-cluster, terwijl de andere servers replicatie gebruiken naar hun databases up-to-date te houden. Met SQL Server kan worden verstrekt volledige database redundantie en betere beschikbaarheid met failover cluster of spiegelen.

## <a name="security-considerations"></a>Veiligheidsoverwegingen

ADFS maakt gebruik van de HTTPS-protocol, dus zorg ervoor dat de NSG regels voor het subnet met het web VMs vergunning aan te vragen HTTPS-verzoeken trapsgewijs. Deze aanvragen kunnen afkomstig zijn van de on-premises netwerk, de subnetten die met de weblaag, bedrijven laag, van gegevens, persoonlijke DMZ, openbare DMZ en het subnet met de ADFS-servers.

Kunt u overwegen een reeks virtuele netwerkapparaten die gedetailleerde informatie over verkeer doorlopen van de rand van het virtuele netwerk voor controle doeleinden Logboeken.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

De volgende overwegingen, samengevat uit het document [de ADFS-implementatie plannen][plan-your-adfs-deployment], geven een beginpunt voor het aanpassen van ADFS farms:

- Als u minder dan 1000 gebruikers hebt, maak geen speciale ADFS-servers, maar in plaats daarvan ADFS installeren op alle servers Hiermee in de cloud (Zorg ervoor dat er ten minste twee Hiermee-servers beschikbaar). Maak een enkel WAP-server.

- Als u tussen 1000 en 15000 gebruikers hebt, maakt u twee speciale ADFS-servers en twee speciale WAP-servers.

- Als u tussen 15000 en 60000 gebruikers hebt, maakt u tussen de drie en vijf speciale ADFS-servers en ten minste twee speciale WAP-servers.

Deze cijfers wordt ervan uitgegaan dat u gebruikt twee vier core VMs (standaard D4_v2 of beter) voor het hosten van de servers in Azure wordt aangegeven.

Houd er rekening mee dat als u de interne Windows-Database voor de opslag ADFS configuratiegegevens gebruikt, u beperkt tot acht ADFS-servers in de farm zijn. Als u dat u meer nodig hebt verwacht, gebruikt u SQL Server. Voor meer informatie raadpleegt u [De rol van de Database ADFS-configuratie][adfs-configuration-database].

## <a name="management-considerations"></a>Overwegingen bij

DevOps personeel moet worden bereid de volgende taken uitvoeren:

- Het beheren van de federatieservers (de ADFS-farm beheren, vertrouwensbeleid op de federatieservers beheren en beheren van de certificaten gebruikt door de federation services).

- Het beheren van de WAP-servers (beheren van de WAP-farm, het beheren van de certificaten WAP).

- Het beheren van webtoepassingen (gebruikmakende partijen, verificatiemethoden en claims toewijzingen configureren).

- Een back-up ADFS-onderdelen.

## <a name="monitoring-considerations"></a>Overwegingen voor controle

Het [Microsoft System Center Management Pack voor Active Directory Federation Services 2012 R2] [ oms-adfs-pack] biedt zowel proactief te volgen en uw implementatie ADFS van de federatieserver. Dit management pack bewaakt:

- Gebeurtenissen die de ADFS service-records in de ADFS-gebeurtenislogboeken.

- De prestatiegegevens die de items ADFS verzamelen. 

- De algemene status van de ADFS-systeem en web-toepassingen (gebruikmakende partijen), en biedt u meldingen voor kritieke problemen en waarschuwingen.

## <a name="solution-components"></a>Onderdelen van oplossingen

Een oplossing voorbeeldscript, [Deploy-ReferenceArchitecture.ps1][solution-script], is beschikbaar die u gebruiken kunt voor de uitvoering van de architectuur die volgt op de aanbevelingen in dit artikel beschreven. Dit script maakt gebruik van Azure resourcemanager sjablonen. De sjablonen zijn beschikbaar als een set fundamentele bouwstenen, die elk een specifieke actie zoals een VNet maken of te configureren van een NSG uitvoeren. Het doel van het script is goedkeuringen sjabloonimplementatie.

De sjablonen zijn van parameters voorzien, met de parameters gehouden in afzonderlijke JSON-bestanden. De parameters in deze bestanden configureren voor de implementatie aan uw eigen vereisten voldoet, kunt u wijzigen. U hoeft niet te wijzigen van de sjablonen zelf. Houd er rekening mee dat u moet de schema's van de objecten in de parameterbestanden niet wijzigen.

Wanneer u de sjablonen hebt bewerkt, maakt u objecten die de naamconventies die worden beschreven in [Naamgevingsconventies voor aanbevolen voor Azure Resources][naming-conventions].

De voorbeeld-oplossing wordt gemaakt en configureert de omgeving in de cloud bestaande uit de Hiermee subnet en de ADFS-subnet en servers, ADFS-proxy subnet en -servers-servers DMZ, weblaag, zakelijke laag en laag voor gegevenstoegang, VPN gateway en management laag. De oplossing voorbeeld bevat ook een optionele configuratie voor het maken van een gesimuleerd on-premises omgeving.

De volgende secties worden de elementen van de on-premises implementatie en configuraties cloud.

### <a name="on-premises-components"></a>Onderdelen van de on-premises implementatie

>[AZURE.NOTE] Deze onderdelen zijn niet de nadruk van de architectuur die worden beschreven in dit document en overzichtelijk wilt geeft u de gelegenheid om te testen van de cloud-omgeving veilig, in plaats van met een reële productieomgeving worden gegeven. Daarom in deze sectie alleen bevat een overzicht van de belangrijkste parameterbestanden. U kunt instellingen zoals de IP-adressen of de grootte van de VMs wijzigen, maar het is raadzaam te laat veel van de andere parameters ongewijzigd.

Deze omgeving bestaat uit een AD-bos voor een domein naam contoso.com. Het domein bevat twee Hiermee servers met IP-adressen 192.168.0.4 en 192.168.0.5. Deze twee servers worden ook de DNS-service uitvoeren. Het lokale beheerdersaccount op beide VMs heet `testuser` met wachtwoord `AweS0me@PW`. Daarnaast kunnen bepaalt de configuratie een VPN-gateway voor de verbinding met de VNet in de cloud. U kunt de configuratie wijzigen door het bewerken van de volgende JSON-bestanden die zich bevindt in de [**parameters/lokale**] [ on-premises-folder] map:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Dit bestand bepaalt de ruimte voor het adres van netwerk voor de on-premises omgeving.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Dit bestand bevat de configuratie voor de on-premises implementatie VMs Hiermee hostingservices. Standaard worden twee *standaard-DS3-v2* VMs gemaakt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** en ** [connection.parameters.json][on-premises-connection-parameters]**. Deze bestanden houdt u de instellingen voor de VPN-verbinding met de gateway Azure VPN in de cloud, met inbegrip van de gedeelde sleutel moet worden gebruikt om te beveiligen verkeer doorlopen van de gateway.

De resterende bestanden in de map bevatten de configuratiegegevens gebruikt om te maken van de on-premises domein met deze infrastructuur. U kunt ze Hiermee installeert, setup-DNS, een bos maken en configureren van de replicatie-sites voor de bos.

### <a name="cloud-components"></a>Onderdelen van de cloud

Deze onderdelen vormen een belangrijk onderdeel van deze architectuur. De [**parameters/azure**] [ azure-folder] map bevat de volgende parameterbestanden voor het configureren van de volgende onderdelen:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Structuur van de VNet door dit bestand wordt gedefinieerd voor de VMs en andere elementen in de cloud. Het bevat instellingen, zoals de naam, -adresruimte, subnetten en de adressen van de DNS-servers vereist. Houd er rekening mee dat de DNS-adressen uit dit voorbeeld verwijzen naar de IP-adressen van de lokale DNS-servers en ook de standaard Azure DNS-server. Deze adressen om te verwijzen naar uw eigen DNS-instellingen als u geen gebruik van de steekproef on-premises omgeving wijzigen:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Dit bestand configureert het VMs Hiermee uitgevoerd in de cloud. De configuratie bestaat uit twee VMs. Moet u de beheerder-gebruikersnaam en wachtwoord in het `virtualMachineSettings` sectie, en u kunt desgewenst het formaat VM zodat deze overeenkomen met de vereisten van het domein wijzigen:

    Zie voor meer informatie, [Active Directory uitbreiden naar Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [toevoegen-telt-domein-controller.parameters.json][add-adds-domain-controller-parameters]**. Dit bestand bevat de instellingen voor het maken van de CONTOSO-domein die de servers Hiermee in beslag nemen. Aangepaste uitbreidingen die stand brengen van het domein en de servers Hiermee toevoegen aan deze wordt gebruikt. Tenzij u aanvullende Hiermee-servers maken (in dat geval moet u deze toevoegen aan de `vms` matrix), hun namen wijzigen van de standaard of wilt maken van een domein met een andere naam die u niet nodig hebt om het bestand.

- ** [loadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Het bestand bevat twee sets met configuratieparameters die. De `virtualMachineSettings` sectie definieert de VMs die als de ADFS-service in de cloud host. Standaard maakt het script twee van de volgende VMs in bepaalde beschikbaarheid:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    De `loadBalancerSettings` gedeelte bevat de beschrijving van de verdeling van de belasting voor deze VMs. De taakverdeling doorgegeven verkeer dat wordt weergegeven op poort 443 (HTTPS) aan een of andere van de VMs:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [adfs-farm-domein-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Dit bestand bevat de instellingen kunt u de ADFS-servers toevoegen aan de CONTOSO-domein. U hoeft slechts eenmaal om het bestand als u extra ADFS-servers hebt gemaakt (bijwerken de `vms` in dit geval een matrix), of u de domeinnaam hebt gewijzigd.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs-farm-first.parameters.json][adfs-farm-first-parameters]**, en ** [adfs-farm-rest.parameters.json][adfs-farm-rest-parameters]**. Het script gebruikt de instellingen in deze bestanden te maken van de ADFS-server-farm. 

    Het bestand *gmsa.parameters.json* bevat de instellingen voor de groep beheerde serviceaccount gebruikt door de ADFS-service. Als u wilt wijzigen van de naam van het account of het domein, kunt u dit bestand wijzigen.

    Het bestand *adfs-farm-first.parameters.json* bevat de informatie die nodig zijn om te maken van de ADFS-server-farm en voeg de eerste server. Als u het domein of de naam van de groep beheerde service-account hebt gewijzigd, moet u dit bestand bijwerken.

    De *adfs-farm-rest.parameters.json* -bestand wordt gebruikt voor de overige ADFS-servers toevoegen aan de farm. Klik nogmaals als u het domein of de naam van de groep beheerde service-account hebt gewijzigd, moet u dit bestand bijwerken. Update van de `vms` matrix als u extra ADFS-servers hebt gemaakt.

- ** [loadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Dit bestand lijkt in de structuur en inhoud op het bestand *loadBalancer-adfs.parameters.json* . De gegevens voor het samenstellen van de AD FS-proxyservers en taakverdeling bevat.

- ** [adfsproxy-farm-first.parameters.json][adfsproxy-farm-first-parameters]**, en ** [adfsproxy-farm-rest.parameters.json][adfsproxy-farm-rest-parameters]**. Het script gebruikt de instellingen in deze bestanden om de ADFS-proxy server-farm te maken. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Dit bestand bevat de instellingen voor het maken van de gateway Azure VPN in de cloud gebruikt om te verbinden met de on-premises netwerk. U mag wijzigen de `sharedKey` waarde in de `connectionsSettings` sectie zodat deze overeenkomt met die van het apparaat van de VPN on-premises implementatie. Zie [een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren]voor meer informatie,[hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** en ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Deze bestanden configureren de binnenkomende (openbaar) en uitgaande (privé) zijden van de VMs waaruit de DMZ, beveiligen van de servers in de cloud. Zie voor meer informatie over deze elementen en hun configuratie, [uitvoering van een DMZ tussen Azure en Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters-json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters-json][loadBalancer-biz-parameters]**, en ** [loadBalancer-data.parameters-json][loadBalancer-data-parameters]**. Deze bestanden parameters bevatten de VM specificaties voor het web en business data access-lagen en configureren van netwerktaakverdelers voor elke laag. Hierna ziet u de VMs die host de WebApps en -databases en de werkbelasting bedrijven uitvoeren voor de organisatie. U kunt de grootte en het aantal VMs in elke laag wijzigen op basis van uw vereisten.

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Dit bestand bevat de configuratie voor de lijnsprong vak/management VMs. Alleen is het mogelijk om te krijgen van aanmeldings- en beheerderstoegang tot de VMs in het web, business en niveaus van de gegevens in het vak sprong. Standaard het script Hiermee maakt u een enkel *Standard_DS1_v2* VM, maar u kunt dit bestand als u wilt maken van groter te maken of aanvullende VMs als de werklast management is waarschijnlijk aanzienlijk aanpassen.

## <a name="solution-deployment"></a>Implementatie van oplossingen

De oplossing wordt ervan uitgegaan van de volgende vereisten:

- U hebt een bestaand Azure-abonnement waarin u resourcegroepen kunt maken.

- U hebt gedownload en geïnstalleerd van de meest recente versie van Azure Powershell. Zie [hier] [ azure-powershell-download] voor instructies.

Het script uitvoeren dat de oplossing implementeert:

1. Verplaatsen naar een handige map op uw lokale computer en de volgende submappen maken:

    - Scripts

    - Scripts/Parameters

    - Scripts/Parameters/lokale

    - Scripts/Parameters/Azure

2. Download de [Deploy-ReferenceArchitecture.ps1] [ solution-script] bestand naar de map

3. De inhoud van de [parameters/lokale] downloaden[ on-premises-folder] map naar de map Scripts/Parameters/lokale:

4. De inhoud van de [parameters/azure] downloaden[ azure-folder] map naar de map Scripts/Parameters/Azure.

5. Bewerk het bestand Deploy-ReferenceArchitecture.ps1 in de map Scripts, waarna de volgende regels als u wilt aangeven welke resourcegroepen dat moeten worden gemaakt of voor het opslaan van de resources die zijn gemaakt door het script wijzigen:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Bewerk de parameterbestanden in de Scripts/Parameters/lokale en Scripts/Parameters/Azure-mappen. De resource groep verwijzingen in deze bestanden zodat deze overeenkomt met de namen van de resourcegroepen die zijn toegewezen aan de variabelen in het bestand Deploy-ReferenceArchitecture.ps1 bijwerken. De volgende tabel ziet u welke parameterbestanden verwijzen naar welke resourcegroep. Houd er rekening mee dat de groepen *AB-adfs-werkbelasting-rg*, *AB-adfs-beveiliging-rg* *AB-adfs-telt-rg*, *AB-adfs-adfs-rg*en *AB-AD FS-proxy-rg* alleen in de PowerShell-script gebruikt worden en niet worden weergegeven in de parameterbestanden.

  	|Resourcegroep|Parameter bestanden|
  	|--------------|--------------|
  	|AB-adfs-lokale-rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|AB-adfs-netwerk-rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-Public.parameters.JSON<br />parameters\azure\loadBalancer-ADFS.parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-with-onpremise-and-Azure-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*twee exemplaren*)

    Daarnaast instellen van de configuratie van de on-premises en cloud onderdelen, zoals wordt beschreven in de sectie [Components oplossing] [oplossing-components].

7. Open een Azure PowerShell-venster, verplaatsen naar de map Scripts en voer de volgende opdracht:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Vervang `<subscription id>` met uw Azure abonnement-ID.

    Voor `<location>`, typt u een Azure gebied, bijvoorbeeld `eastus` of `westus`.

    De `<mode>` parameter kunt beschikken over een van de volgende waarden:

    - `Onpremise`, de gesimuleerd on-premises omgeving te maken.

    - `Infrastructure`, om te maken van de VNet-infrastructuur en springen van vak in de cloud.

    - `CreateVpn`, voor het bouwen van Azure virtuele netwerkgateway en verbindt u deze naar de on-premises netwerk.

    - `AzureADDS`, als u wilt samenstellen de VMs die fungeert als Hiermee servers, Active Directory implementeren naar deze VMs en maken van het domein in de cloud.

    - `AdfsVm`de ADFS-VMs maken en deze toevoegen aan het domein in de cloud.

    - `ProxyVm`voor de AD FS-proxy VMs bouwen en deze toevoegen aan het domein in de cloud.

    - `Prepare`, waarin alle bovenstaande taken kunt uitvoeren. **Dit is de aanbevolen optie als u een volledig nieuwe implementatie samenstelt en u een bestaande on-premises implementatie-infrastructuur geen hebt.**

    >[AZURE.NOTE] U kunt ook het script uitvoeren een `<mode>` -parameter van `Workload` het web, en grote ondernemingen, en gegevenslaag VMs en netwerk maken. Deze instelling is geen deel uit van de `Prepare` modus.

    Als u de `Prepare` de optie het script duurt enkele uren en is voltooid met het bericht *voorbereiding is voltooid. Installeer certificaat tot alle ADFS en proxy VMs.*

8.  Start opnieuw op het vak sprong (*AB-adfs-mgmt-vm1* in de groep *AB-adfs-beveiliging-rg* ) als u wilt dat de DNS-instellingen zijn doorgevoerd.

9.  [Een SSL-certificaat aanvragen voor ADFS] [ adfs_certificates] en dit certificaat installeren op de ADFS-VMs. Houd er rekening mee dat u verbinding met de ADFS-VMs via het vak sprong maken kunt. De IP-adressen zijn *10.0.5.4* en *10.0.5.5*. De standaard-gebruikersnaam is *contoso\testuser* met wachtwoord *AweSome@PW*.

    >[AZURE.NOTE] De opmerkingen in het script Deploy-ReferenceArchitecture.ps1 bieden nu gedetailleerde instructies voor het maken van een zelfondertekend testcertificaat en het gebruik van de instantie de `makecert` opdracht. Gebruik echter niet de certificaten die zijn gegenereerd door makecert in een productieomgeving.

10. Voer de volgende Powershell-opdracht op de ADFS-server-farm configureren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Blader in het vak gaat u naar *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* de ADFS-installatie (u mogelijk een certificaat waarschuwing weergegeven, waarin u voor deze test negeren kunt) te testen. Controleer of de aanmeldingspagina van Contoso Corporation wordt weergegeven. Meld u aan als *contoso\testuser* met wachtwoord *AweS0me@PW*.

12. De SSL-certificaat installeren op de ADFS-proxy VMs. De IP-adressen zijn *10.0.6.4* en *10.0.6.5*.

13. Voer de volgende Powershell-opdracht om de eerste ADFS-proxyserver te configureren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Volg de instructies weergegeven door het script om te testen van de installatie van de eerste AD FS-proxyserver.

15. Voer de volgende Powershell-opdracht voor het configureren van de tweede eerste AD FS-proxyserver:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Volg de instructies weergegeven door het script test de configuratie van de volledige AD FS-proxy.

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Secure hybride netwerkarchitectuur met Active Directory"
