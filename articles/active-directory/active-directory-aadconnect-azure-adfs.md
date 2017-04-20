<properties
    pageTitle="Active Directory Federation Services in Azure | Microsoft Azure"
    description="In dit document leert u hoe u het implementeren van AD FS in Azure wordt aangegeven voor hoge beschikbaarheid."
    keywords="AD FS in azure implementeren, azure AD FS, azure AD FS, azure ad fs implementeren, adfs implementeren, ad fs implementeren adfs in Azure wordt aangegeven, implementeren adfs in Azure wordt aangegeven, adfs naar azure AD FS in azure, adfs azure, inleiding tot AD FS, Azure, AD FS in Azure wordt aangegeven, iaas, ADFS, implementeren verplaatsen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>AD FS-implementatie in Azure wordt aangegeven 

AD FS biedt vereenvoudigde en beveiligde identiteitsfederatie en mogelijkheden voor eenmalige aanmelding (SSO) Web. Federatie met Azure AD of O365 kunnen gebruikers met on-premises implementatie referenties verifiëren en toegang tot alle bronnen in de cloud. Hierdoor wordt het belang dat u over een zeer beschikbare AD FS-infrastructuur om ervoor te zorgen toegang tot bronnen beide on-premises implementatie en in de cloud. AD FS in Azure implementeert, kunt u helpen de beschikbaarheid vereist met minimale inspanningen bereiken.
Er zijn verschillende voordelen van het inzetten van AD FS in Azure wordt aangegeven, een aantal hierna volgen:

* **Beschikbaarheid** - met de kracht van Azure beschikbaarheid Sets u ervoor zorgen dat een zeer beschikbare infrastructuur.
* **Gemakkelijk schaal** – meer prestaties nodig? Eenvoudig migreren naar krachtiger systemen door een paar muisklikken in Azure wordt aangegeven
* **Cross-geografische redundantie** – met Azure geografische redundantie worden automatisch dat de infrastructuur van uw ten zeerste beschikbaar overal ter wereld is
* **Gemakkelijk te beheren** : met de opties ten zeerste vereenvoudigd beheer in Azure-portal beheren van de infrastructuur van uw is heel eenvoudig en probleemloze 

## <a name="design-principles"></a>Ontwerpprincipes voor het

![Implementatie-ontwerp](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Het bovenstaande diagram ziet u de aanbevolen topologieën om te beginnen met het implementeren van de infrastructuur van uw AD FS in Azure wordt aangegeven. Hieronder ziet u de beginselen achter de verschillende onderdelen van de topologie:

* **Domeincontroller / ADFS-Servers**: als u minder dan 1000 gebruikers hebt u gewoon functie van AD FS op domeincontroller kunt installeren. Als u niet dat elke invloed op de prestaties op de domeincontrollers wilt of als u meer dan 1000 gebruikers hebt, klikt u vervolgens AD FS op afzonderlijke servers te implementeren.
* **WAP Server** – het is om te implementeren Web Application-proxyservers, zodat gebruikers kunnen de AD FS wanneer deze niet op het netwerk van het bedrijf ook zijn hebt bereikt.
* **DMZ**: het Web Application-proxyservers in de DMZ worden geplaatst en alleen TCP/443 toegang tussen de DMZ en het interne subnet is toegestaan.
* **Balancers laden**: om ervoor te zorgen betere beschikbaarheid van AD FS-en Web Application-proxyservers, wordt aangeraden met behulp van een interne taakverdeling voor AD FS-servers en Azure taakverdeling voor Web Application-proxyservers.
* **Beschikbaarheid Sets**: als u wilt redundantie voor de AD FS-implementatie, het wordt aanbevolen dat u twee of meer virtuele machines in een beschikbaarheid instellen voor soortgelijke werkbelasting groeperen. Deze configuratie zorgt ervoor dat tijdens een een onderhoudsgebeurtenis of niet gepland ten minste één VM beschikbaar zal zijn
* **Opslag-Accounts**: het wordt aanbevolen om te laten twee opslag-accounts. Zonder dat een één opslag-account hebt, kan leiden tot het maken van een potentieel risico en kunnen leiden tot de implementatie te worden niet beschikbaar zijn in een niet waarschijnlijk scenario waar het account opslag uitvalt. Twee opslag accounts helpen één opslag-account voor elke regel foutenstructuuranalyse koppelen.
* **Netwerk scheiding**: Web Application-proxyservers moeten worden geïmplementeerd op een aparte DMZ-netwerk. U kunt een virtueel netwerk verdelen over twee subnetten en vervolgens implementeren de Web Application-proxyservers in een geïsoleerd subnet. U kunt het netwerk groep beveiligingsinstellingen configureren voor elk subnet en alleen vereist communicatie tussen de twee subnetten toestaan. Meer informatie krijgen per implementatiescenario onderstaande

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Stappen voor het implementeren van AD FS in Azure wordt aangegeven

De stappen die worden genoemd in deze sectie een overzicht maken van de handleiding voor het implementeren de onder AD FS-infrastructuur in de voorbeelden in Azure wordt aangegeven.

### <a name="1-deploying-the-network"></a>1. implementeren het netwerk

Als hierboven beschreven, kunt u op twee subnetten maken in één virtuele netwerk anders twee volledig verschillende virtuele netwerken (VNet) maken. In dit artikel wordt richten op één virtuele netwerk implementeert en deze verdelen over twee subnetten. Dit is momenteel een gemakkelijker aanpak twee afzonderlijke VNets een VNet tot VNet gateway voor communicatie nodig.

**1.1 virtuele netwerk maken**

![Virtuele netwerk maken](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
Klik in de portal Azure select virtueel netwerk en u kunt het virtuele netwerk en implementeren één subnet direct met één klik. INT subnet ook is gedefinieerd en is nu klaar voor VMs moet worden opgeteld.
De volgende stap is een ander subnet toevoegen aan het netwerk, dat wil zeggen het subnet DMZ. Het subnet DMZ gewoon maken

* Selecteer het nieuwe netwerk
* Selecteer in de eigenschappen subnet
* Klik in het subnet Configuratiescherm op de knop toevoegen
* Geef de subnet naam en het adres ruimte informatie als u wilt het subnet maken

![Subnet](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Subnet DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. maken van het netwerk beveiligingsgroepen**

Een beveiligingsgroep netwerk (NSG) bevat een lijst met regels voor Access (Toegangsbeheerlijst) die toestaan of weigeren netwerkverkeer tot uw VM-sessies in een virtueel netwerk. NSGs kan worden gekoppeld aan subnetten of afzonderlijke VM exemplaren in dat subnet. Wanneer een NSG gekoppeld aan een subnet is, wordt de ACL regels toepassen op alle VM exemplaren in dat subnet.
Voor de toepassing van deze instructies, we twee NSGs gaan maken: een voor een intern netwerk en een DMZ. Ze worden gelabeld NSG_INT en NSG_DMZ respectievelijk.

![NSG maken](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Nadat de NSG is gemaakt, wordt er 0 regels voor binnenkomende en 0 uitgaande. Zodra de rollen op de desbetreffende servers geïnstalleerd en functionele zijn, kunnen de binnenkomende en uitgaande regels worden gemaakt op basis van het gewenste niveau van beveiliging.

![NSG geïnitialiseerd](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Nadat de NSGs zijn gemaakt, NSG_INT koppelen met subnet INT en NSG_DMZ met subnet DMZ. Hieronder volgt een voorbeeld schermafbeelding:

![NSG configureren](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klik op subnetten om het deelvenster voor subnetten te openen
* Selecteer het subnet koppelen aan de NSG 

Na de configuratie ziet het deelvenster voor subnetten zoals hieronder:

![Subnetten na NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. verbinding met on-premises maken**

Er wordt een verbinding met on-premises implementatie nodig hebt bij het implementeren van de domeincontroller (domeincontroller) in Azure wordt aangegeven. Azure laat verschillende connectiviteitsopties zien voor de infrastructuur van uw on-premises implementatie verbinden met de infrastructuur van uw Azure.

* Punt-naar-site
* Virtual Network Site-naar-site
* ExpressRoute

Het wordt aanbevolen ExpressRoute gebruiken. ExpressRoute kunt u persoonlijke verbindingen tussen Azure datacenters en dat op uw locatie of in een omgeving met anderen samenwerkt locatie-infrastructuur maken. ExpressRoute verbindingen gaan niet via de openbare Internet. Ze bieden meer betrouwbaarheid, snellere snelheden, onderste vertragingstijden en hoger beveiliging dan de normale verbindingen via Internet.
Terwijl het wordt aanbevolen ExpressRoute gebruiken, kunt u eventuele verbindingsmethode meest geschikt zijn voor uw organisatie. Lees meer informatie over ExpressRoute en de verschillende connectiviteitsopties met ExpressRoute [ExpressRoute technisch overzicht](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. opslag-accounts maken

Informatie voor het behoud van beschikbaarheid en afhankelijkheid van een account één opslag voorkomen, kunt u twee opslag-accounts maken. De machines in elke set beschikbaarheid verdeeld over twee groepen en wijs elke groep een aparte opslag-account.

![Opslag-accounts maken](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. beschikbaarheid sets maken

Voor elke rol (domeincontroller/AD FS en WAP), beschikbaarheid-sets met 2 machines ten minste te maken. Zo kunt rekenen op grotere beschikbaarheid voor elke rol. Maken van de beschikbaarheid wordt ingesteld, zijn maar er essentiële van het volgende bepalen:
* **Foutenstructuuranalyse domeinen**: virtuele machines in hetzelfde foutenstructuuranalyse domein delen de dezelfde power bron- en fysieke netwerk veranderen. Een minimum van 2 foutenstructuuranalyse domeinen worden aanbevolen. De standaardwaarde is 3 en laat u deze zo is voor de toepassing van deze installatie
* **Domeinen bijwerken**: Machines die deel uitmaken van het domein dat dezelfde update samen opnieuw worden gestart tijdens een update. U wilt hebben minimum van 2 update-domeinen. De standaardwaarde is 5 en laat u deze zo is voor de toepassing van deze installatie

![Beschikbaarheid van sets](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

De volgende beschikbaarheid sets maken

| Beschikbaarheid instellen | Rol | Domeinen voor foutenstructuuranalyse | Domeinen bijwerken |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | DOMEINCONTROLLER/ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. virtuele machines implementeren
De volgende stap wordt geïmplementeerd virtuele machines die wordt gehost in de verschillende rollen in de infrastructuur van uw. Ten minste twee machines worden aanbevolen in elke set beschikbaarheid. Zes virtuele machines voor de eenvoudige implementatie maken.

| Machine | Rol | Subnet | Beschikbaarheid instellen | Opslag-account | IP-adres |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|DOMEINCONTROLLER/ADFS|INT|contosodcset|contososac1|Statische|
|contosodc2|DOMEINCONTROLLER/ADFS|INT|contosodcset|contososac2|Statische|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Statische|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Statische|

Als u opgevallen mogelijk, is geen NSG opgegeven. Dit komt doordat azure kunt u NSG op het subnetniveau van gebruiken. U kunt vervolgens machine netwerkverkeer bepalen met behulp van de afzonderlijke NSG die is gekoppeld aan op het subnet anders het NIC-object. Lees meer over [Wat is een netwerk beveiliging groep (NSG)](https://aka.ms/Azure/NSG).
Statisch IP-adres wordt aanbevolen als u de DNS-records beheert. U kunt gebruiken Azure DNS en in plaats daarvan in de DNS-records voor uw domein, verwijzen naar de nieuwe machines door hun FQDN Azure.
Het deelvenster VM ziet er als onder nadat de installatie is voltooid:

![Virtuele Machines geïmplementeerd](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. configureren van de domeincontroller / AD FS-servers
 Om te verifiëren alle inkomende verzoeken, moet AD FS contact opnemen met de domeincontroller. Het wordt aanbevolen om op te slaan de dure reis van Azure voor on-premises implementatie domeincontroller voor verificatie, om te implementeren van een replica van de domeincontroller in Azure wordt aangegeven. Het wordt aanbevolen om te kunnen bereiken beschikbaarheid, om een set beschikbaarheid minimale 2-domeincontrollers te maken.

|Domeincontroller|Rol|Opslag-account|
|:-----:|:-----:|:-----:|
|contosodc1|Replica|contososac1|
|contosodc2|Replica|contososac2|

* De twee servers verhogen als replicadomeincontrollers met DNS
* Configureer de AD FS-servers met het installeren van de functie van de AD FS met de Serverbeheer.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. implementeert interne taakverdeling (ILB)

**6.1. de ILB maken**

Als u wilt een ILB implementeert, toevoegen Selecteer netwerktaakverdelers in de portal van Azure en klik op (+).
>[AZURE.NOTE] Als u **Netwerktaakverdelers** in het menu niet ziet, klikt u op **Bladeren** in de linkerbenedenhoek van de portal en gaat u totdat u **Netwerktaakverdelers**ziet.  Klik vervolgens op de gele ster toe te voegen aan het menu. Selecteer nu het nieuwe pictogram van de verdeling van laden om het deelvenster om te beginnen met de configuratie van de taakverdeling te openen.

![Blader van taakverdeling](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Naam**: Geef een naam op geschikt verdeling van de belasting
* **Kleurenschema**: aangezien deze taakverdeling vóór de AD FS-servers worden geplaatst en is bedoeld voor interne netwerkverbindingen alleen, selecteer "Interne"
* **Virtual Network**: het virtuele netwerk waar u uw AD FS implementeert kiezen
* **Subnet**: Kies het interne subnet hier
* **IP-adres toewijzing**: dynamische

![Interne taakverdeling](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Nadat u op maken en de ILB wordt geïmplementeerd, moet u dit ziet in de lijst met netwerktaakverdelers:

![Netwerktaakverdelers na ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Volgende stap is het configureren van de backend-toepassingen en de backend-test.

**6.2. ILB backend groep configureren**

Selecteer de zojuist gemaakte ILB in het deelvenster netwerktaakverdelers. Het deelvenster instellingen wordt geopend. 
1.  Selecteer backend-groepen in het deelvenster instellingen voor
2.  In het deelvenster toevoegen backend toepassingen, klikt u op op VM toevoegen
3.  U krijgt een deelvenster waarin u beschikbaarheid instellen kiezen kunt
4.  Kies de AD FS beschikbaarheid instellen

![ILB backend groep configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. test configureren**

Selecteer in het deelvenster instellingen voor ILB sondes.
1.  Klik op toevoegen
2.  Informatie geven voor test een. **Naam**: naam b onderzoeken. **Protocol**: TCP c. **Poort**: 443 (HTTPS) d. **Interval**: 5 (standaardwaarde) – Dit is het interval waarop ILB de machines in de groep backend e wordt onderzoeken. **Beschadigde drempelwaarden**: 2 (standaard val ue) – Dit is de limiet van opeenvolgende test fouten waarna ILB een machine in de backend-toepassingen worden declareren niet meer reageert en stoppen verkeer hiernaar te sturen.

![ILB test configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. taakverdeling regels maken**

Om te kunnen effectief saldo vanaf het verkeer, moet de ILB worden geconfigureerd met regels van taakverdeling. Om te maken van een regel van taakverdeling 
1.  Selecteer de regel in het deelvenster instellingen van de ILB van taakverdeling
2.  Klik op toevoegen in het Configuratiescherm van de regel van taakverdeling
3.  Klik in het Configuratiescherm van de regel voor taakverdeling van toevoegen een. **Naam**: Geef een naam voor de regel b. **Protocol**: Selecteer TCP c. **Poort**: 443 d. **Back-end poort**: 443 e. **Back-end-toepassingen**: Selecteer de toepassingen die u hebt gemaakt voor de AD FS cluster eerdere f. **Test**: Selecteer de test eerder hebt gemaakt voor AD FS-servers

![Het verdelen van regels ILB configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. DNS met ILB bijwerken**

Ga naar uw DNS-server en maak een CNAME voor de ILB. De CNAME moet voor de service Federatie met het IP-adres dat is het IP-adres van de ILB aan te wijzen. Bijvoorbeeld als het adres ILB DIP 10.3.0.8 en de federation-service geïnstalleerd is fs.contoso.com, maakt u een CNAME voor fs.contoso.com 10.3.0.8 aan te wijzen.
Dit zorgt ervoor dat dat alle communicatie met betrekking tot het einde van de fs.contoso.com aan de ILB boven en correct worden gerouteerd.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. configureren de Web Application-proxyserver

**7.1. configureren de Web Application-proxyservers te bereiken van AD FS-servers**

Om ervoor te zorgen dat Web Application-proxyservers kunnen bereiken van de AD FS-servers achter de ILB zijn, moet u een record maken in de %systemroot%\system32\drivers\etc\hosts voor de ILB. Houd er rekening mee dat de DN (Distinguished name) moet de naam van de service Federatie, bijvoorbeeld fs.contoso.com. En het IP-fragment moet van het IP-adres van de ILB (10.3.0.8 zoals in het voorbeeld).

**7.2. installeren de rol van de Web-toepassingsproxy**

Nadat u ervoor zorgen dat Web Application-proxyservers kunnen bereiken van de AD FS-servers achter ILB zijn, kunt u de Web Application-proxyservers vervolgens installeren. Toepassingsproxy endwebservers niet worden toegevoegd aan het domein. Het installeren van de rollen Web toepassingsproxy op de twee Web Application-proxyservers door te selecteren van de rol Remote Access. De server manager begeleidt u om de WAP-installatie te voltooien.
Lees voor meer informatie over het implementeren van WAP [installeren en configureren van de Web Application-proxyserver](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. implementeren Internet die tegenover elkaar liggen (openbaar) taakverdeling

**8.1. Internet die tegenover elkaar liggen (openbaar) taakverdeling maken**
 
Klik in de portal Azure netwerktaakverdelers selecteren en klik op toevoegen. Voer de volgende gegevens in het deelvenster maken laden de verdeling
1. **Naam**: naam voor de verdeling van de belasting
2. **Kleurenschema**: openbare – deze optie zorgt ervoor dat Azure dat deze taakverdeling een openbaar adres moet.
3. **IP-adres**: Maak een nieuwe IP-adres (dynamisch)

![Omlaag taakverdeling Internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Na implementatie verschijnt de taakverdeling in de lijst van de balancers laden.

![Laden de verdeling van lijst](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. een DNS-label toewijzen aan het openbare IP**

Klik op de vermelding van de verdeling van gemaakte laden in het deelvenster laden balancers om het deelvenster voor configuratie weer te geven. Voer de onderstaande stappen te volgen voor het configureren van de DNS-label voor het openbare IP:
1.  Klik op het openbare IP-adres. Hiermee opent u het deelvenster voor het openbare IP- en de instellingen
2.  Klik op configuratie
3.  Geef een DNS-label. Hiermee worden de openbare DNS-label waartoe u toegang vanaf elke locatie, bijvoorbeeld contosofs.westus.cloudapp.azure.com. U kunt een vermelding toevoegen in de externe DNS-records voor de federation-service (zoals fs.contoso.com) die wordt omgezet in het DNS-label van de externe taakverdeling (contosofs.westus.cloudapp.azure.com).

![Internet die tegenover elkaar liggen taakverdeling configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Internet die tegenover elkaar liggen taakverdeling (DNS) configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. backend-toepassingen configureren voor taakverdeling Internet die tegenover elkaar liggen (openbaar)** 

Volg de stappen die in de interne taakverdeling, om te configureren van de backend-toepassingen voor (openbaar) taakverdeling Internet die tegenover elkaar liggen als de beschikbaarheid instellen voor de servers WAP maken. Bijvoorbeeld: contosowapset.

![Back-end-groep van Internet die tegenover elkaar liggen taakverdeling configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8,4. test configureren**

Volg de stappen die in het configureren van de verdeling van de interne belasting om te configureren van de test voor de groep backend met WAP-servers.

![Test van taakverdeling voor Internet-die tegenover elkaar liggen configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. (s) van taakverdeling maken**

Volg de stappen die in ILB voor het configureren van de regel voor taakverdeling voor TCP 443.

![Configureer taakverdeling regels van taakverdeling voor Internet-die tegenover elkaar liggen](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. beveiligen het netwerk

**9.1. beveiligen het interne subnet**

Algemene, moet u de volgende regels voor het efficiënt beveiligen van uw interne subnet (in de volgorde zoals hieronder wordt weergegeven)

|Regel|Beschrijving|Stroom|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| De HTTPS-communicatie van DMZ toestaan | Binnenkomende verbindingen |
|DenyInternetOutbound| Geen toegang tot internet | Uitgaande |

![INT toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[opmerking]: <> (![toegangsregels INT (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [opmerking]: <> (![toegangsregels INT (uitgaand)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. beveiligen het subnet DMZ**

|Regel|Beschrijving|Stroom|
|:----|:----|:------:|
|AllowHTTPSFromInternet| HTTPS vanaf internet naar de DMZ toestaan | Binnenkomende verbindingen|
|DenyInternetOutbound|  Alles behalve HTTPS met internet is geblokkeerd | Uitgaande |

![Externe toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[opmerking]: <> (![uit toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [opmerking]: <> (![toegangsregels uit (uitgaand)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Als een gebruiker van client-verificatie certificaat (client-TLS-verificatie met X509 certificaten) is vereist, en vervolgens de AD FS moeten TCP poort 49443 voor binnenkomende toegang zijn ingeschakeld.

###<a name="10--test-the-ad-fs-sign-in"></a>10. testen de AD FS-aanmelding

De eenvoudigste manier is om te testen van AD FS met behulp van de pagina IdpInitiatedSignon.aspx. Om te kunnen doen dat is vereist voor de IdpInitiatedSignOn van de AD FS-eigenschappen. Volg de onderstaande stappen om te controleren of de AD FS-configuratie
1.  Voer de onderstaande cmdlet op de AD FS-server, via PowerShell, om deze in te stellen ingeschakeld.
    Set-AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  Uit een externe computer toegang https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  U ziet de AD FS-pagina zoals hieronder:

![Test-aanmeldingspagina](./media/active-directory-aadconnect-azure-adfs/test1.png)

Bij succesvolle aanmelden krijgt deze u een bericht zoals hieronder wordt weergegeven:

![Success testen](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Sjabloon voor het implementeren van AD FS in Azure wordt aangegeven

De sjabloon implementeert een 6 machine-instelling 2 voor domeincontrollers, AD FS en WAP.

[AD FS in Azure-implementatie-sjabloon](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

U kunt een bestaand virtuele netwerk gebruiken of een nieuwe VNET maken tijdens het implementeren van deze sjabloon. Hieronder ziet u de verschillende parameters die beschikbaar zijn voor het aanpassen van de implementatie met de beschrijving van het gebruik van de parameter in het implementatieproces. 

| Parameter | Beschrijving |
|:--------|:-----|
|Locatie| Het gebied de resources implementeren naar, bijvoorbeeld Oost ons. |
|StorageAccountType| Het type van het opslag-Account dat is gemaakt|
|VirtualNetworkUsage| Geeft aan of een nieuw virtuele netwerk wordt gemaakt of een bestaande eigenschap te gebruiken|
|VirtualNetworkName| De naam van het virtuele netwerk te maken, verplicht over het gebruik van bestaand of nieuw virtueel netwerk|
|VirtualNetworkResourceGroupName| Geeft de naam van de resourcegroep waarin het bestaande virtuele netwerk zich bevindt. Wanneer u een bestaand virtuele netwerk, wordt dit een verplichte parameter, zodat de implementatie de ID van het bestaande virtuele netwerk kan vinden|
|VirtualNetworkAddressRange| Het adresbereik van de nieuwe VNET, verplicht als een nieuw virtuele netwerk maken|
|InternalSubnetName| De naam van het interne subnet, verplicht op beide virtueel netwerk gebruiksopties (nieuw of bestaand)|
|InternalSubnetAddressRange| Het adresbereik van de interne subnet, waarin de domeincontrollers en ADFS-servers verplicht als een netwerk met een nieuwe virtuele maakt.|
|DMZSubnetAddressRange| Het adresbereik van de dmz-subnet, waarin de Windows-toepassing proxyservers, verplicht als een netwerk met een nieuwe virtuele maakt.|
|DMZSubnetName| De naam van het interne subnet, verplicht op beide virtueel netwerk gebruiksopties (nieuwe of bestaande). |
|ADDC01NICIPAddress| Het interne IP-adres van de eerste domeincontroller, dit IP-adres statisch worden toegewezen aan de domeincontroller en moet een geldige IP-adres binnen het interne subnet|
|ADDC02NICIPAddress| Het interne IP-adres van de tweede domeincontroller, dit IP-adres statisch worden toegewezen aan de domeincontroller en moet een geldige IP-adres binnen het interne subnet|
|ADFS01NICIPAddress| Het interne IP-adres van de eerste ADFS-server, dit IP-adres statisch worden toegewezen aan de ADFS-server en moet een geldige IP-adres binnen het interne subnet|
|ADFS02NICIPAddress| Het interne IP-adres van de tweede ADFS-server, dit IP-adres statisch worden toegewezen aan de ADFS-server en moet een geldige IP-adres binnen het interne subnet|
|WAP01NICIPAddress| Het interne IP-adres van de eerste WAP-server, dit IP-adres statisch worden toegewezen aan de WAP-server en moet een geldige IP-adres binnen het subnet DMZ|
|WAP02NICIPAddress| Het interne IP-adres van de tweede WAP-server, dit IP-adres statisch worden toegewezen aan de WAP-server en moet een geldige IP-adres binnen het subnet DMZ|
|ADFSLoadBalancerPrivateIPAddress| Het interne IP-adres van de ADFS-taakverdeling, dit IP-adres statisch worden toegewezen aan de taakverdeling en moet een geldige IP-adres binnen het interne subnet|
|ADDCVMNamePrefix| VM voorvoegsel voor besturing voor domein|
|ADFSVMNamePrefix| VM voorvoegsel voor ADFS-servers|
|WAPVMNamePrefix| VM voorvoegsel voor WAP servers|
|ADDCVMSize| De vm grootte van het domein|
|ADFSVMSize| De grootte vm van de ADFS-servers|
|WAPVMSize| De grootte vm van de WAP-servers|
|AdminUserName| De naam van de lokale beheerder van de virtuele machines|
|Beheerderswachtwoord| Het wachtwoord voor het lokale beheerdersaccount van de virtuele machines|

## <a name="additional-resources"></a>Aanvullende informatie
* [Beschikbaarheid van Sets](https://aka.ms/Azure/Availability ) 
* [Azure taakverdeling](https://aka.ms/Azure/ILB)
* [Interne taakverdeling](https://aka.ms/Azure/ILB/Internal)
* [Internet omlaag taakverdeling](https://aka.ms/Azure/ILB/Internet)
* [Opslag-Accounts](https://aka.ms/Azure/Storage )
* [Azure virtuele netwerken](https://aka.ms/Azure/VNet)
* [AD FS- en Web Application-Proxy koppelingen](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Volgende stappen

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
* [Configureren en beheren van uw AD FS met Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [Beschikbaarheid cross-geografische AD FS-implementatie in Azure wordt aangegeven met Azure verkeer Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




