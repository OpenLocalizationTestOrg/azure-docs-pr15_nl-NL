<properties
    pageTitle="Een internetgerichte taakverdeling-oplossing implementeert met IPv6 met een sjabloon | Microsoft Azure"
    description="Het implementeren van IPv6-ondersteuning voor Azure taakverdeling en VMs verdeeld."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure taakverdeling, dubbele stapel, openbare IP-, systeemeigen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Een internetgerichte taakverdeling oplossing implementeert met IPv6 met een sjabloon

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sjabloon](./load-balancer-ipv6-internet-template.md)

Een Azure taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt beschikbaarheid door te distribueren binnenkomende verkeer tussen exemplaren van de orde service in de cloudservices of virtuele machines in een set van de verdeling van laden. Azure taakverdeling geeft soms ook deze services op meerdere poorten, meerdere IP-adressen of beide.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

In het volgende diagram ziet u de oplossing van taakverdeling wordt geïmplementeerd met de voorbeeldsjabloon in dit artikel beschreven.

![Scenario voor de verdeling van laden](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende Azure bronnen:

- een virtueel netwerk-interface voor elke VM met IPv4 en IPv6-adressen die zijn toegewezen
- een taakverdeling internetgerichte met een IPv4- en een IPv6 openbare IP-adres
- twee laden taakverdeling regels voor de openbare VIP's toewijzen aan de privé eindpunten
- een beschikbaarheid-instellen met de twee VMs
- twee virtuele machines (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>De sjabloon met behulp van de Azure portal implementeren

In dit artikel wordt verwezen naar een sjabloon die in de galerie met [Sjablonen van Azure Quickstart](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) is gepubliceerd. U kunt de sjabloon downloaden vanuit de galerie of starten van de implementatie in Azure rechtstreeks vanuit de galerie. In dit artikel wordt ervan uitgegaan dat u de sjabloon hebt gedownload naar uw lokale computer.

1. Open het Azure-portal en meld u aan met een account met machtigingen voor het maken van VMs en netwerken resources in een Azure-abonnement. Ook, tenzij u bestaande resources gebruikt, moet het account machtigingen voor het maken van een resourcegroep en een opslag-account.

2. Klik op '+ Nieuw' in het menu Klik op type 'sjabloon' in het zoekvak. Selecteer 'sjabloonimplementatie' in de lijst met zoekresultaten.

    ![kg-ipv6-portal-stap 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. In de alles blade, klikt u op "Sjabloon-implementatie."

    ![kg-ipv6-portal-step3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klik op 'Maken'.

    ![kg-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klik op 'Sjabloon bewerken'. Verwijder de bestaande inhoud en kopiëren en plakken in de volledige inhoud van het sjabloonbestand (voor, zoals het begin en einde {}) en klik vervolgens op 'Opslaan'.

    > [AZURE.NOTE] Als u Microsoft Internet Explorer gebruikt wanneer u u plakt verschijnt een dialoogvenster gevraagd om toegang te krijgen tot het Klembord van Windows. Klik op "Toegang toestaan".

    ![kg-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klik op 'Parameters bewerken'. In het blad Parameters geeft u de waarden per de richtlijnen in het gedeelte sjabloon voor parameters, klik op 'Opslaan' om te sluiten van het blad Parameters. In het blad aangepaste implementatie, selecteert u uw abonnement, een bestaande resourcegroep of een account maakt. Als u een resourcegroep maakt, selecteert u een locatie voor de resourcegroep. Vervolgens **juridische voorwaarden**, klik op **aanschaffen** voor de juridische voorwaarden. Azure begonnen met het implementeren van de resources. Het duurt enkele minuten om te implementeren van alle resources.

    ![kg-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Zie de sectie [Sjabloonparameters en variabelen](#template-parameters-and-variables) verderop in dit artikel voor meer informatie over deze parameters.

7. Als u wilt zien van de resources die door de sjabloon is gemaakt, klikt u op Bladeren, schuif omlaag in de lijst totdat u overzicht van "Resourcegroepen" en vervolgens klikt u erop.

    ![kg-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Klik op het blad Resource groepen, klikt u op de naam van de resourcegroep die u hebt opgegeven in stap 6. U ziet een lijst met alle resources die zijn geïmplementeerd. Als alles goed is, moet er 'Geslaagd' onder "Laatste implementatie." Zo niet, Controleer of het account dat u gebruikt machtigingen zijn de benodigde resources maken.

    ![kg-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Als u uw resourcegroepen zoeken direct na stap 6, wordt 'Laatste implementatie' de status van 'Implementeren' weergegeven terwijl de resources zijn die wordt geïmplementeerd.

9. Klik op 'myIPv6PublicIP' in de lijst met bronnen. U ziet dat er een IPv6-adres onder IP-adres en dat de DNS-naam de waarde die u hebt opgegeven voor de parameter dnsNameforIPv6LbIP in stap 6. Deze resource is de openbare IPv6-adres en host naam die toegankelijk is voor Internet-clients.

    ![kg-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Connectiviteit valideren

Wanneer de sjabloon is geïmplementeerd, kunt u connectivity valideren door de volgende taken uit te voeren:

1. Meld u aan bij de portal van Azure en verbinding maken met elk van de VMs die door de sjabloonimplementatie is gemaakt. Als u een Windows Server VM, ipconfig uitvoeren geïmplementeerd/alles vanaf een opdrachtprompt. U ziet dat de VMs IPv4 en IPv6-adressen hebben. Als u Linux VMs geïmplementeerd, moet u het besturingssysteem Linux om te ontvangen dynamische IPv6-adressen voor de instructies voor uw Linux-verdeling met configureren.
2. Starten vanuit een client IPv6 internetverbinding, een verbinding met de openbare IPv6-adres van de taakverdeling. Om te bevestigen dat de taakverdeling is verdelen tussen de twee VMs, kunt u een webserver zoals Microsoft Internet Information Services (IIS) op elk van de VMs installeren. De standaard-webpagina op elke server kan de tekst 'Server0' of 'Server1' identificeren bevatten. Open een internetbrowser op een client IPv6 internetverbinding vervolgens en blader naar de hostnaam die u voor de parameter dnsNameforIPv6LbIP van de verdeling van de belasting om te bevestigen end-to-end IPv6-connectiviteit voor elke VM opgegeven. Als u alleen de pagina met webonderdelen van slechts één server ziet, moet u mogelijk de browsercache wissen. Open meerdere privé bladeren sessies. Hier ziet u een antwoord van elke server.
3. Starten vanuit een client IPv4 internetverbinding, een verbinding met de openbare IPv4-adres van de taakverdeling. Om te bevestigen dat de taakverdeling de twee VMs van taakverdeling is, kunt u testen met IIS, zoals wordt beschreven in stap 2.
4. Starten vanuit elke VM, een uitgaande verbinding met een apparaat IPv6 of Internet IPv4-verbonden. In beide gevallen wordt het bron-IP zichtbaar voor de Bestemmingsapparaat de openbare IPv4 of IPv6-adres van de taakverdeling.

>[AZURE.NOTE]
ICMP voor IPv4 en IPv6 is geblokkeerd in het Azure netwerk. Hierdoor mislukt ICMP-hulpprogramma's zoals altijd ping. Gebruik een alternatief TCP zoals TCPing of de cmdlet PowerShell Test-NetConnection connectiviteit testen. Houd er rekening mee dat het IP-adressen weergegeven in het diagram ziet u voorbeelden van waarden die kunnen optreden. Aangezien de IPv6-adressen dynamisch zijn toegewezen, wordt de adressen die u ontvangt verschillen en kunnen verschillen per regio. Het is ook algemene voor de openbare IPv6-adres van de verdeling van de belasting om te beginnen met een ander voorvoegsel dan de privé IPv6-adressen in de groep back-enddatabase.

## <a name="template-parameters-and-variables"></a>Sjabloonparameters en variabelen

Een sjabloon Azure resourcemanager bevat meerdere variabelen en parameters die u aan uw wensen voldoet aanpassen kunt. Variabelen worden gebruikt voor vaste waarden die u niet wilt dat een gebruiker om te wijzigen. Parameters worden gebruikt voor waarden die u wilt dat een gebruiker op te geven bij het distribueren van de sjabloon. De voorbeeldsjabloon is geconfigureerd voor het scenario in dit artikel beschreven. U kunt dit aanpassen aan de behoeften van uw omgeving.

De voorbeeldsjabloon gebruikt in dit artikel bevat de volgende variabelen en parameters:

| Parameter / variabele | Notities |
|-----------|-------|
| adminUsername | Geef de naam van het beheerdersaccount gebruikt om aan te melden bij de virtuele machines met. |
| Beheerderswachtwoord | Geef het wachtwoord voor het account aanmelden bij de virtuele machines met gebruikt. |
| dnsNameforIPv4LbIP | Geef de DNS-host-naam die u wilt toewijzen aan de openbare naam van de taakverdeling. Deze naam wordt omgezet naar de openbare IPv4-adres van de verdeling van de belasting. De naam moet een kleine letter zijn en overeenkomen met de regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Geef de DNS-host-naam die u wilt toewijzen aan de openbare naam van de taakverdeling. Deze naam wordt omgezet naar de openbare IPv6-adres van de verdeling van de belasting. De naam moet een kleine letter zijn en overeenkomen met de regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Dit is dezelfde naam als het IPv4-adres. Wanneer een client stuurt een DNS-query voor deze naam Azure retourneert records A zowel AAAA wanneer de naam worden gedeeld. |
| vmNamePrefix | Geef het voorvoegsel VM. De sjabloon voegt u een nummer (0, 1, enzovoort) op de naam wanneer de VMs worden gemaakt. |
| nicNamePrefix | Het netwerk interface naam voorvoegsel opgeven. De sjabloon voegt u een nummer (0, 1, enzovoort) op de naam wanneer de netwerkinterfaces worden gemaakt. |
| storageAccountName | Voer de naam van een bestaand opslag-account of geef de naam van een nieuw moet worden gemaakt door de sjabloon. |
| availabilitySetName | Voer vervolgens de naam van de beschikbaarheid instellen voor gebruik met de VMs |
| addressPrefix | Het adresvoorvoegsel gebruikt om te definiëren het adresbereik van het virtuele netwerk |
| subnetName | De naam van het subnet in voor de VNet gemaakt |
| subnetPrefix | Het adresvoorvoegsel gebruikt voor het adresbereik van het subnet definiëren |
| vnetName | Geef de naam voor de VNet die worden gebruikt door de VMs. |
| ipv4PrivateIPAddressType | De toewijzingsmethode gebruikt voor het persoonlijke IP-adres (statische of dynamische) |
| ipv6PrivateIPAddressType | De methode voor de toewijzing die wordt gebruikt voor het persoonlijke IP-adres (dynamisch). IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| numberOfInstances | Het aantal exemplaren van taakverdeling geïmplementeerd via de sjabloon |
| ipv4PublicIPAddressName | Geef de DNS-naam die u wilt gebruiken om te communiceren met de openbare IPv4-adres van de taakverdeling. |
| ipv4PublicIPAddressType | De toewijzingsmethode gebruikt voor het openbare IP-adres (statische of dynamische) |
| Ipv6PublicIPAddressName | Geef de DNS-naam die u wilt gebruiken om te communiceren met de openbare IPv6-adres van de taakverdeling. |
| ipv6PublicIPAddressType | De methode voor de toewijzing die wordt gebruikt voor het openbare IP-adres (dynamisch). IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| lbName | Geef de naam van de taakverdeling. Deze naam wordt weergegeven in de portal of gebruikt als verwijzing te maken met een CLI of PowerShell-opdracht. |

De resterende variabelen in de sjabloon bevatten afgeleide waarden die zijn toegewezen wanneer Azure de bronnen maakt. Wijzig niet die variabelen.
