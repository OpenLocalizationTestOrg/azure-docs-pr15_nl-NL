<properties
    pageTitle="Verkeer Manager eindpunt typen | Microsoft Azure"
    description="In dit artikel wordt uitgelegd verschillende soorten eindpunten die kunnen worden gebruikt met Azure verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Verkeer Manager eindpunten

Microsoft Azure verkeer Manager kunt u aangeven hoe netwerkverkeer toepassing implementaties uitgevoerd in verschillende datacenters wordt verdeeld. U configureren elke toepassingsimplementatie als een 'eindpunt' in verkeer Manager. Als verkeer Manager een DNS-aanvraag ontvangt, worden een beschikbaar eindpunt voor retournering in de reactie van de DNS-gekozen. In het verkeer manager wordt de keuze gebaseerd op de huidige Eindpuntstatus en de methode verkeer-mailroutering. Zie voor meer informatie [Hoe verkeer Manager werkt](traffic-manager-how-traffic-manager-works.md).

Er zijn drie soorten eindpunt worden ondersteund door verkeer Manager:

- **Azure eindpunten** worden gebruikt voor de services van Azure.
- **Externe eindpunten** worden gebruikt voor services gehost buiten Azure, een on-premises implementatie of bij een andere hostingprovider.
- **Geneste eindpunten** worden gebruikt voor het combineren van verkeer Manager profielen flexibeler verkeer-routing's ter ondersteuning van de behoeften van grotere, complexe implementaties maken.

Er is geen beperking op hoe de eindpunten van verschillende typen worden gecombineerd in één verkeer Manager profiel. Elk profiel kan elke combinatie van eindpunt typen bevatten.

De volgende secties worden elk type eindpunt groter alles.

## <a name="azure-endpoints"></a>Azure eindpunten

Azure eindpunten worden gebruikt voor services op basis van Azure in verkeer Manager. De volgende typen Azure resources worden ondersteund:

- 'Klassieke' IaaS VMs en PaaS cloud services.
- WebApps
- PublicIPAddress resources (die kunnen worden verbonden met VMs rechtstreeks of via een taakverdeling Azure). De publicIpAddress moet een DNS-naam die moet worden gebruikt in een profiel verkeer Manager is toegewezen.

PublicIPAddress resources zijn Azure resourcemanager resources. Ze bestaan niet in het implementatiemodel klassieke. Ze zijn dus alleen ondersteund in verkeer van Manager Azure resourcemanager ervaringen. Andere typen eindpunt worden via resourcemanager zowel het implementatiemodel klassieke ondersteund.

Wanneer u met Azure eindpunten, wordt verkeer Manager detecteert wanneer een 'Klassieke' IaaS VM, cloudservice of een Web-App wordt gestopt en gestart. Deze status wordt weergegeven in de Eindpuntstatus. Zie [controleren van verkeer Manager-eindpunt](traffic-manager-monitoring.md#endpoint-and-profile-status) voor meer informatie. Wanneer de onderliggende service is gestopt, geen verkeer Manager eindpunt systeemstatus controles of directe verkeer naar het eindpunt uitgevoerd. Geen verkeer Manager billing voor het exemplaar gestopt gebeurtenissen. Wanneer de service opnieuw is opgestart, komt factureringsbeheerder cv's en het eindpunt in aanmerking voor verkeer. Deze detectie geldt niet voor PublicIpAddress eindpunten.

## <a name="external-endpoints"></a>Externe eindpunten

Externe eindpunten worden gebruikt voor services buiten Azure. Bijvoorbeeld een service on-premises implementatie gehost of met een andere provider. Externe eindpunten kunnen worden gebruikt afzonderlijk of gecombineerd met Azure eindpunten in hetzelfde profiel zijn dat verkeer Manager. Azure eindpunten met externe eindpunten combineren, kunt verschillende scenario's:

- In beide een actieve of actieve-passieve failover-model, gebruikt u Azure te leveren vergrote redundantie voor een bestaande on-premises implementatie toepassing.
- Als u wilt verkleinen toepassing latentie voor gebruikers overal ter wereld, uitbreiden een bestaande on-premises implementatie-toepassing naar aanvullende geografische locaties in Azure wordt aangegeven. Zie voor meer informatie [verkeer Manager 'Prestaties' verkeersroutering](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Gebruik Azure op te geven extra capaciteit voor een bestaande on-premises implementatie-toepassing, continu of als een oplossing 'burst cloud' om te voldoen aan een Prikker van de vraag.

In bepaalde gevallen, is het handig zijn gebruik van externe eindpunten om te verwijzen naar Azure services (Zie de [Veelgestelde vragen](#faq)voor voorbeelden). In dit geval zijn systeemstatus controles gefactureerd snelheid Azure eindpunten, niet het tarief weer dat externe eindpunten. Echter, in tegenstelling tot Azure eindpunten, als u stopt of verwijderen van de onderliggende service, systeemstatus controleren facturering totdat u uitschakelen of verwijderen van het eindpunt in verkeer Manager.

## <a name="nested-endpoints"></a>Geneste eindpunten

Geneste eindpunten Combineer meerdere verkeer Manager profielen flexibele verkeer-routing schema's maken en ondersteuning voor de behoeften van grotere, complexe implementaties. Een profiel 'onderliggende' wordt als een eindpunt met geneste eindpunten toegevoegd aan een profiel 'parent'. De onderliggende en bovenliggende profielen, kunnen andere eindpunten van elk type, met inbegrip van andere geneste profielen bevatten. Zie [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md)voor meer informatie.

## <a name="web-apps-as-endpoints"></a>Web Apps als eindpunten

Aantal aanvullende overwegingen toepassen tijdens het configureren van Web Apps als eindpunten in verkeer Manager:

1. Alleen Web-Apps op de 'Standaard' SKU of hoger komen in aanmerking voor gebruik met verkeer Manager. Pogingen om toe te voegen een Web-App van een lagere SKU is mislukt. De SKU van een bestaande Web-App Downgrade zoekresultaten in verkeer Manager, niet meer verzenden verkeer naar die Web-App.

2. Als een eindpunt ontvangt een HTTP-aanvraag, gebruikt de koptekst 'host' in het verzoek om te bepalen welke Web-App moet service het verzoek. De kop host bevat de DNS-naam waarmee de aanvraag, bijvoorbeeld contosoapp.azurewebsites.net initiëren. Als u wilt gebruiken een andere naam voor de DNS-Web-App, moet de DNS-naam worden geregistreerd als een aangepaste domeinnaam voor de App. Bij het toevoegen van een eindpunt Web App als een Azure eindpunt, wordt automatisch de naam van het verkeer Manager profiel DNS geregistreerd voor de App. Deze registratie wordt automatisch verwijderd wanneer het eindpunt wordt verwijderd.

3. Elk profiel verkeer Manager kan maximaal één Web App-eindpunt hebben uit elke Azure regio. U omzeilt voor deze beperking, kunt u een Web-App als een externe eindpunt. Raadpleeg de [Veelgestelde vragen](#faq)voor meer informatie.

## <a name="enabling-and-disabling-endpoints"></a>In- en eindpunten uitschakelen

Een eindpunt in verkeer Manager uitschakelen is soms handig om tijdelijk verkeer verwijderen uit een eindpunt dat in de onderhoudsmodus of opnieuw worden geïmplementeerd. Zodra het eindpunt opnieuw wordt uitgevoerd, mogelijk opnieuw worden ingeschakeld.

Eindpunten kunnen worden ingeschakeld en uitgeschakeld via de portal verkeer Manager PowerShell, CLI of REST API, die worden ondersteund in zowel resourcemanager en het implementatiemodel klassieke.

>[AZURE.NOTE] Een eindpunt Azure uitschakelen heeft niets te maken met de implementatie staat in Azure wordt aangegeven. Een Azure-service (zoals een VM of Web-App blijft actief zijn en mogen verkeer zelfs wanneer uitgeschakeld in verkeer Manager te ontvangen. Verkeer kan worden opgelost rechtstreeks naar het exemplaar van de plaats van via de DNS-naam van het verkeer Manager-profiel. Zie [de werking van verkeer Manager](traffic-manager-how-traffic-manager-works.md)voor meer informatie.

De huidige geschiktheid van elk eindpunt verkeer ontvangen, is afhankelijk van de volgende factoren:

- De status van het profiel (ingeschakeld/uitgeschakeld)
- De Eindpuntstatus (ingeschakeld/uitgeschakeld)
- De resultaten van de status Hiermee wordt gecontroleerd op dat eindpunt

Zie voor meer informatie [verkeer Manager eindpunt bewaken](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Aangezien verkeer Manager bij het niveau van de DNS-werkt, kan het niet van invloed zijn op bestaande verbindingen naar een eindpunt. Wanneer een eindpunt niet beschikbaar is, wordt verkeer Manager u omgeleid zodat nieuwe verbindingen naar een andere beschikbare eindpunt. De host achter het eindpunt uitgeschakeld of beschadigd kan echter blijven ontvangen van verkeer via bestaande verbindingen, totdat deze sessies worden beëindigd. Toepassingen moeten de sessieduur als u wilt dat verkeer is toegestaan uit bestaande verbindingen beperken.

Als alle eindpunten in een profiel zijn uitgeschakeld of als het profiel zelf is uitgeschakeld, klikt u vervolgens verkeer Manager een reactie 'NXDOMAIN' naar een nieuwe DNS-query stuurt.

## <a name="faq"></a>FAQ

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kan ik verkeer Manager gebruiken met de eindpunten van meerdere abonnementen?

Gebruik van de eindpunten van meerdere abonnementen is niet mogelijk is met Azure Web Apps. Azure-WebApps vereist een aangepaste domeinnaam die wordt gebruikt met Web Apps wordt alleen gebruikt in een één-abonnement. Dit kan niet worden met het Web Apps gebruiken uit meerdere abonnementen met dezelfde domeinnaam.

Andere typen eindpunt is het mogelijk te verkeer Manager gebruiken met eindpunten uit meer dan één abonnement. Hoe u verkeer Manager configureren, is afhankelijk van of u het implementatiemodel klassieke of de Resource Manager-ervaring worden gebruikt.

- In resourcemanager eindpunten van een abonnement kunnen worden toegevoegd als naar verkeer Manager, zolang de persoon die de Manager verkeer-profiel alleen toegang tot het eindpunt heeft. Deze machtigingen kunnen worden toegekend met [Azure resourcemanager Rolgebaseerd toegangsbeheer op (RBAC)](../active-directory/role-based-access-control-configure.md).
- In de klassieke implementatie model-interface verkeer Manager is vereist dat Cloudservices of Web-Apps die zijn geconfigureerd als Azure eindpunten zich in hetzelfde abonnement als het verkeer Manager-profiel bevinden. Cloud Service eindpunten in andere abonnementen kunnen naar verkeer Manager worden toegevoegd als 'externe' eindpunten. Deze externe eindpunten worden gefactureerd als Azure eindpunten, in plaats van het tarief weer dat externe.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kan ik verkeer Manager gebruiken met Cloudservice 'Tijdelijke' sleuven?

Ja. Cloudservice tijdelijke sleuven kan in verkeer Manager worden geconfigureerd als externe eindpunten. Servicestatus controles zijn nog steeds btw geheven Azure eindpunten snelheid. Omdat het type externe eindpunt gebruikt wordt, zijn wijzigingen in de onderliggende service niet opgenomen automatisch. Verkeer Manager kan geen met externe eindpunten vinden wanneer de Cloud-Service is gestopt of verwijderd. De Manager verkeer blijft daarom facturering voor status controles tot het eindpunt is uitgeschakeld of verwijderd.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Ondersteunt verkeer Manager IPv6 eindpunten?

Verkeer Manager biedt momenteel geen IPv6-addressible naamservers. Verkeer Manager kunnen echter nog steeds worden gebruikt door IPv6-clients verbinding maken met IPv6-eindpunten. Een client maakt DNS-aanvragen geen rechtstreeks naar verkeer Manager. De client gebruikt in plaats daarvan een recursieve DNS-service. Een alleen IPv6-client verzendt aanvragen naar de recursieve DNS-service via IPv6. De service recursieve moet vervolgens kunnen de naamservers van het verkeer Manager met IPv4 contact op te nemen.

Verkeer Manager reageert met de DNS-naam van het eindpunt. Een eindpunt IPv6-ondersteuning, moet een AAAA DNS-record die de naam van de DNS-eindpunt wijst naar de IPv6-adres bestaan. IPv4-adressen wordt alleen ondersteund door verkeer Manager systeemstatus controles. De service moet een IPv4-eindpunt op dezelfde DNS-naam.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kan ik verkeer Manager gebruiken met meer dan één Web-App in dezelfde regio?

Verkeer Manager wordt meestal gebruikt voor het verkeer door naar toepassingen die zijn geïmplementeerd in verschillende regio's. Echter kan dit ook worden gebruikt waar een toepassing heeft meer dan één implementatie in dezelfde regio. De eindpunten verkeer Manager Azure staan niet meer dan één Web App-eindpunt uit het dezelfde Azure regio moet worden toegevoegd aan hetzelfde profiel zijn dat verkeer Manager.

De volgende stappen bieden een tijdelijke oplossing u kunt deze beperking:

1. Controleer of uw eindpunten in verschillende web app 'schaaleenheden zijn'. Een domeinnaam moet toewijzen aan een site in een eenheid voor tijdschaal opgegeven. Twee Web-Apps in dezelfde schaaleenheid delen niet daarom een profiel verkeer Manager.
2. De naam van uw aangepaste domein toevoegen als een aangepaste hostname aan elke Web-App. Elke Web-App moeten zich in een andere schaal eenheid groeperen. Alle Web-Apps moet behoren tot hetzelfde abonnement.
3. Eén (en slechts één) Web App-eindpunt als een Azure eindpunt toevoegen aan uw profiel verkeer Manager.
4. Elk extra Web App-eindpunt toevoegen aan uw profiel verkeer Manager als een externe eindpunt. Externe eindpunten kunnen alleen worden toegevoegd met het implementatiemodel resourcemanager.
5. Een DNS CNAME-record maken in uw aangepaste domein die naar uw Manager verkeer profiel DNS-naam verwijst (<> …. trafficmanager.net).
6. Toegang tot uw site via de aangepaste domeinnaam, niet de verkeer Manager DNS-profielnaam.

## <a name="next-steps"></a>Volgende stappen

- Informatie over [de werking van verkeer Manager](traffic-manager-how-traffic-manager-works.md).
- Meer informatie over het verkeer Manager [eindpunt cmdlets voor controle en automatische failover](traffic-manager-monitoring.md).
- Meer informatie over verkeer Manager [methoden voor het doorsturen van verkeer](traffic-manager-routing-methods.md).
