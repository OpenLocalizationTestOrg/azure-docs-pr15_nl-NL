## <a name="load-balancer-differences"></a>Laden verschillen van de verdeling

Er zijn verschillende opties voor het distribueren van netwerkverkeer met Microsoft Azure. Deze opties werken anders van elkaar verschillen, met een andere functie instellen en ondersteuning voor verschillende scenario's. Ze elk kunnen worden gebruikt in moeten worden geïsoleerd of deze worden gecombineerd.

- **Azure-taakverdeling** werkt op de transportlaag (laag 4 in de stapel OSI netwerk verwijzing). Dit biedt netwerk niveau verdeling van verkeer verschillende exemplaren van een toepassing wordt uitgevoerd in de dezelfde Azure Datacenter.

- **Toepassingsgateway** werkt achter de toepassingslaag (7 in de stapel OSI netwerk verwijzing laag). Deze fungeert als een reverse-proxy-service, de clientverbinding wordt verbroken en doorsturen van aanvragen voor back-enddatabase eindpunten.

- **Verkeer Manager** werkt op het niveau van DNS.  Via DNS-antwoorden eindgebruikers-verkeer naar globaal verdeelde eindpunten. Clients vervolgens verbinding maken met deze eindpunten rechtstreeks.

De volgende tabel worden de functies van elke service:

| Service | Azure taakverdeling | Toepassingsgateway | Verkeer Manager |
|---|---|---|---|
|Technologie| Transportniveau (laag 4) | Niveau van de webtoepassing (laag 7) | DNS-niveau |
| Toepassingsprotocollen die worden ondersteund | Een | HTTP- en HTTPS |  Een (een HTTP-eindpunt is vereist voor het eindpunt monitoring) |
| Eindpunten | Azure VMs en Cloud Services rol exemplaren | Een Azure interne IP-adres of openbare internet IP-adres | Azure VMs, Cloudservices Azure Web Apps en externe eindpunten |
| Ondersteuning voor Vnet | Kan worden gebruikt voor beide Internet interne en externe omlaag (Vnet)-toepassingen | Kan worden gebruikt voor beide Internet interne en externe omlaag (Vnet)-toepassingen |    Biedt alleen ondersteuning voor internetgerichte toepassingen |
Eindpunt bewaken | Ondersteund via sondes | Ondersteund via sondes | Ondersteund via HTTP-/ HTTPS-ophalen | 

Azure taakverdeling en toepassingsgateway Routenetwerk verkeer naar eindpunten maar hebben verschillende gebruik de volgende voorbeeldscenario's welke verkeer worden afgehandeld. De volgende tabel kunt het verschil tussen de twee netwerktaakverdelers:

| Type | Azure taakverdeling | Toepassingsgateway |
|---|---|---|
| Protocollen | UDP/TCP | HTTP / HTTPS |
| IP-reservering | Ondersteund | Niet ondersteund | 
| Laden van taakverdeling modus | 5-tuple(source IP, source port, destination IP, destination port, protocol type) | Round Robin<br>Routering op basis van URL | 
| Modus van taakverdeling (bron-IP / Plaktoetsen sessies) |  2-tupel (bron-IP- en doel-IP-), 3-tupel (bron-IP, doel-IP- en poort). Kan schalen omhoog of omlaag op basis van het aantal virtuele machines | Cookie gebaseerde affiniteit<br>Routering op basis van URL |
| Servicestatus sondes | Standaard: Test interval - 15 seconden. Afmelden bij de draaiing die u hebt gemaakt: 2 continue fouten. Controleert of de gebruiker gedefinieerde ondersteunt | Niet-actieve test interval 30 seconden. Die na 5 opeenvolgende live verkeer fouten of een enkel-test is mislukt in niet-actieve modus. Controleert of de gebruiker gedefinieerde ondersteunt | 
| SSL-offloading | Niet ondersteund | Ondersteund | 
  