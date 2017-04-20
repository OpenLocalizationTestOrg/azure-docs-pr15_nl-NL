<properties
    pageTitle="Beschikbaarheid cross-geografische AD FS-implementatie in Azure wordt aangegeven met Azure verkeer Manager | Microsoft Azure"
    description="In dit document leert u hoe u het implementeren van AD FS in Azure wordt aangegeven voor hoge beschikbaarheid."
    keywords="AD fs met Azure verkeer manager, adfs met Azure verkeer Manager, geografische, multi-datacenter, geografische datacenters, meerdere geografische datacenters, AD FS in azure implementeren, azure AD FS, azure AD FS, azure ad fs implementeren, adfs implementeren, ad fs implementeren adfs in Azure wordt aangegeven, implementeren adfs in Azure wordt aangegeven, AD FS in azure, adfs azure, inleiding tot AD FS, Azure, AD FS in Azure wordt aangegeven, iaas implementeren , ADFS, adfs naar azure verplaatsen"
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
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Beschikbaarheid cross-geografische AD FS-implementatie in Azure wordt aangegeven met Azure verkeer Manager

[AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md) biedt stapsgewijze om hoe u een eenvoudige AD FS-infrastructuur voor uw organisatie in Azure wordt aangegeven implementeren kunt. Dit artikel bevat de volgende stappen voor het maken van een cross-geografische implementatie van AD FS in Azure wordt aangegeven met [Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md). Azure verkeer Manager helpt een geografisch verspreiding hoge beschikbaarheid en de krachtige AD FS-infrastructuur voor uw organisatie maken door gebruik van het bereik van routeren methoden beschikbaar voor verschillende situaties uit de infrastructuur.

Een zeer beschikbare cross-geografische AD FS-infrastructuur kunt:

* **Verwijdering van potentieel risico:** Met failover mogelijkheden van Azure verkeer Manager, kunt u een zeer beschikbare AD FS-infrastructuur bereiken, zelfs als u een van de datacenters in een deel van de wereldbol uitvalt
* **Verbeterde prestaties:** U kunt de voorgestelde implementatie in dit artikel een high-performance AD FS-infrastructuur waarmee gebruikers verifiëren sneller kunt opgeven. 

##<a name="design-principles"></a>Ontwerpprincipes voor het

![Algemene ontwerpen](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

De eenvoudige principes zijn dezelfde die is vermeld in principes voor het ontwerpen in het artikel AD FS-implementatie in Azure wordt aangegeven. Het bovenstaande diagram ziet u een eenvoudige uitbreiding van de eenvoudige implementatie naar een ander geografisch gebied. Hieronder vindt u enkele punten overwegingen bij het verlengen van uw implementatie naar nieuwe geografische regio

* **Virtuele netwerk:** U kunt een nieuw virtuele netwerk maken in een geografisch gebied dat u wilt implementeren aanvullende AD FS-infrastructuur. In het bovenstaande diagram ziet u Geo1 VNET en Geo2 VNET als de twee virtuele netwerken in elke geografische regio.

* **Domeincontrollers en AD FS-servers in nieuwe geografische VNET:** Het wordt aanbevolen om te implementeren domein controllers in de nieuwe geografische regio zodat de AD FS-servers in de nieuwe regio niet hoeft te maken met een domeincontroller in een andere ver netwerk om te voltooien van een verificatie en daarmee ook de prestaties te verbeteren.

* **Opslag-accounts:** Opslag-accounts zijn gekoppeld aan een gebied. Aangezien u machines in een nieuwe geografische regio distribueren, moet u nieuwe opslagruimte accounts moet worden gebruikt in de regio maken.  

* **Netwerk beveiligingsgroepen:** Als de opslagruimte accounts, netwerk-beveiligingsgroepen die is gemaakt in een gebied kunnen niet worden gebruikt in een ander geografisch gebied. Daarom moet u nieuw netwerk maken beveiligingsgroepen overeenkomen met die in de eerste geografische regio voor INT en DMZ subnet in de nieuwe geografische regio.

* **DNS Labels voor openbare IP-adressen:** Azure verkeer Manager gebruikt om te verwijzen naar de eindpunten alleen via DNS-etiketten. U moet dus DNS-labels maken voor de externe netwerktaakverdelers openbare IP-adressen.

* **Azure verkeer Manager:** Microsoft Azure verkeer Manager kunt u de verdeling van de gebruikersverkeer naar uw service-eindpunten uitgevoerd in verschillende datacenters overal ter wereld aangeven. Azure verkeer Manager werkt op het niveau van DNS. Via DNS-antwoorden eindgebruikers-verkeer naar globaal distributed eindpunten. Clients vervolgens verbinding maken met deze eindpunten rechtstreeks. Met verschillende routeren opties van prestaties, gewogen en prioriteit, kunt u gemakkelijk de optie voor het routeren meest geschikt zijn voor de behoeften van uw organisatie kiezen. 

* **V-net aan V-net connectiviteit tussen twee delen:** U hoeft niet te verbinding tussen de virtuele netwerken zelf hebt. Aangezien elke virtueel netwerk toegang tot domeincontrollers heeft en AD FS en WAP server in zelf heeft, kunnen ze werken zonder eventuele connectiviteit tussen de virtuele netwerken in verschillende regio's. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Stappen voor het integreren van Azure verkeer Manager

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>AD FS in de nieuwe geografische regio implementeren
Volg de stappen en de richtlijnen in [AD FS-implementatie in Azure wordt aangegeven](active-directory-aadconnect-azure-adfs.md) om te implementeren van de dezelfde topologie in de nieuwe geografische regio.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>DNS-labels voor openbare IP-adressen van de netwerktaakverdelers Internet die tegenover elkaar liggen (openbaar)
Bovengenoemde, de Manager van Azure verkeer alleen kan verwijzen naar DNS-etiketten als eindpunten en daarom is het belangrijk om DNS-etiketten voor de externe netwerktaakverdelers openbare IP-adressen te maken. Onder schermafbeelding ziet u hoe u uw DNS-label voor het openbare IP-adres kunt configureren. 

![DNS-Label](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Azure verkeer Manager implementeren

Volg de onderstaande stappen om een verkeer manager-profiel maken. Voor meer informatie kunt u ook verwijzen naar [een profiel Azure verkeer Manager beheren](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Maken van een profiel verkeer Manager:** Geef uw profiel van de manager verkeer een unieke naam. Deze naam van het profiel maakt deel uit van de naam van de DNS- en fungeert als een voorvoegsel voor het label verkeer Manager domein naam. De naam / voorvoegsel wordt toegevoegd aan. trafficmanager.net maken van een label DNS voor uw verkeer manager. De volgende schermafbeelding ziet u de manager verkeer DNS-voorvoegsel wordt ingesteld als mysts en wordt resulterende DNS-label mysts.trafficmanager.net. 

    ![Verkeer Manager profiel maken](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Verkeer routeren methode:** Er zijn drie routeren opties beschikbaar in verkeer manager:

    * Prioriteit 
    * Prestaties
    * Gewogen
    
    **Prestaties** is het aanbevolen optie om te bereiken ten zeerste heeft gereageerd AD FS-infrastructuur. U kunt echter een methode meest geschikt zijn voor uw behoeften implementatie. De AD FS-functionaliteit wordt niet beïnvloed door de optie voor het routeren geselecteerd. Zie [verkeer Manager verkeer routeren methoden](../traffic-manager/traffic-manager-routing-methods.md) voor meer informatie. Schermafbeelding hierboven u ziet in de steekproef de methode **prestaties** is geselecteerd.
   
3.  **Eindpunten configureren:** Klik op eindpunten in de pagina voor het beheren van verkeer en selecteer toevoegen. Hiermee opent u een pagina vergelijkbaar met de onderstaande schermafbeelding eindpunt van toevoegen
 
    ![Eindpunten configureren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Voor de verschillende ingevoerde gegevens, volgt u de onderstaande richtlijn:

    **Type:** Azure eindpunt selecteren terwijl we wordt verwijzen naar een Azure openbare IP-adres.

    **Naam:** Een naam die u wilt koppelen aan het eindpunt te maken. Dit is niet de naam van de DNS- en heeft geen gevolgen voor de DNS-records.

    **Resourcetype doelgroep:** Selecteer het openbare IP-adres als de waarde moet worden deze eigenschap. 

    **Doelgerichte resource:** Hiermee krijgt u een optie en kies uit de andere DNS-etiketten waarover u beschikt onder uw abonnement. Kies de DNS-label voor.

    Eindpunt voor elke geografisch gebied dat u de route-verkeer naar Azure verkeer Manager wilt toevoegen.
    Raadpleeg voor meer informatie en gedetailleerde stappen voor het toevoegen / eindpunten configureren in verkeer manager, toe te [voegen, uitschakelen, inschakelen of verwijderen van eindpunten](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Configureren-test:** Klik in de pagina voor het beheren van verkeer op configuratie. De pagina configuratie moet u de instellingen controleren om te zoeken op HTTP-poort 80 en relatieve pad /adfs/probe wijzigen

    ![Test configureren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Ervoor zorgen dat de status van de eindpunten ONLINE zodra de configuratie voltooid is is**. Als alle eindpunten in 'anders werken' staat, doet Azure verkeer Manager aanbevolen wordt bij een poging om te leiden het verkeer die ervan uitgaande dat de diagnostische hulpprogramma's is onjuist en alle eindpunten bereikbaar zijn.

5. **Wijzigen DNS-records voor Azure verkeer Manager:** Uw service Federatie moet een CNAME-record met de naam van de Azure verkeer Manager DNS. Maak een CNAME in de openbare DNS-records zodat iedereen die is probeert te bereiken van de federation-service de Manager van Azure verkeer daadwerkelijk bereikt.

    Bijvoorbeeld om te laten verwijzen de Federatie service fs.fabidentity.com naar de verkeer Manager, moet u uw DNS resource record bijwerken zodat deze het volgende:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Testen of de Routering en AD FS-aanmelding   

###<a name="routing-test"></a>Test-mailroutering

Een zeer eenvoudige toets voor de routering is om te proberen ping de Federatie service DNS-naam van een computer in elke geografische regio. Afhankelijk van de gekozen routeren methode, worden het eindpunt dat deze daadwerkelijk Hiermee doorgevoerd in de ping-weergave. Bijvoorbeeld als u de prestaties routering hebt geselecteerd, wordt klikt u vervolgens het eindpunt die het dichtst bij de regio van de client bereikt. Hieronder ziet u de momentopname van twee ping-opdrachten uit twee verschillende regio clientcomputers verbinding, een in Oost-Aziatische regio en een in West VS. 

![Test-mailroutering](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS aanmeldingsproblemen testen

De eenvoudigste manier om te testen van AD FS is via de pagina IdpInitiatedSignon.aspx. Om te kunnen doen dat is vereist voor de IdpInitiatedSignOn van de AD FS-eigenschappen. Volg de onderstaande stappen om te controleren of de AD FS-configuratie
 
1. Voer de onderstaande cmdlet op de AD FS-server, via PowerShell, om deze in te stellen ingeschakeld. Set-AdfsProperties - EnableIdPInitiatedSignonPage $true
2. Uit een externe computer toegang https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. U ziet de AD FS-pagina zoals hieronder:

    ![Test in ADFS - verificatie uitdaging](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    bij succesvolle aanmelden, vindt u met een bericht zoals hieronder wordt weergegeven:

    ![Test in ADFS - verificatie success](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Verwante koppelingen
* [Eenvoudige AD FS-implementatie in Azure wordt aangegeven](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure verkeer Manager](../traffic-manager/traffic-manager-overview.md)
* [Verkeer Manager verkeer routeren methoden](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Volgende stappen
* [Een profiel Azure verkeer Manager beheren](../traffic-manager/traffic-manager-manage-profiles.md)
* [Toevoegen, uitschakelen, inschakelen of eindpunten verwijderen](../traffic-manager/traffic-manager-endpoints.md) 

