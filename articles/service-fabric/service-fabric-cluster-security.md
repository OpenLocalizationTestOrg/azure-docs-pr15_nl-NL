<properties
   pageTitle="Een Service stof cluster Secure | Microsoft Azure"
   description="De beveiliging scenario's voor een cluster Service stof en andere technologieën voor de uitvoering van deze scenario's beschreven."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Scenario's de beveiliging van de service stof cluster

Een Service stof cluster is een resource dat u eigenaar bent. Clusters moeten altijd worden beveiligd als u wilt voorkomen dat ongeautoriseerde gebruikers verbinding maken met uw cluster, met name wanneer er productie werkbelasting u erop. Hoewel het is mogelijk te maken van een onbeveiligde cluster, doen dus, kan elke anonieme gebruiker te brengen als management eindpunten naar openbare Internet worden getoond. 

Dit artikel bevat een overzicht van de beveiliging scenario's voor clusters waarop Azure of zelfstandige en verschillende technologieën voor de uitvoering van deze scenario's. Dit zijn de cluster beveiliging scenario's:

- Naar knooppunten beveiliging
- Client-naar-knooppunt beveiliging
- Rolgebaseerd toegangsbeheer RBAC)

## <a name="node-to-node-security"></a>Naar knooppunten beveiliging
Communicatie tussen de VMs of machines in het cluster beveiligt. Dit zorgt ervoor dat alleen computers die zijn geautoriseerd voor deelname aan het cluster deelnemen kunnen aan het hosten van toepassingen en services in het cluster.

![Diagram van knooppunten naar communicatie][Node-to-Node]

Clusters Azure of zelfstandige clusters waarop Windows waarop kunnen [Certificaatbeveiliging](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows-beveiliging](https://msdn.microsoft.com/library/ff649396.aspx) voor Windows Server machines gebruiken.
### <a name="node-to-node-certificate-security"></a>Naar knooppunten Certificaatbeveiliging
Service stof gebruikt x.509-servercertificaten die u als onderdeel van het type knooppunt configuraties opgeeft wanneer u een cluster maakt. Een beknopt overzicht van wat deze certificaten zijn en hoe u kunt ophalen of maken van deze wordt aangeboden aan het einde van dit artikel.

Certificaatbeveiliging is geconfigureerd tijdens het maken van het cluster via de portal van Azure, Azure resourcemanager sjablonen of een zelfstandige JSON-sjabloon. U kunt een primaire certificaat en een optionele secundaire-certificaat dat wordt gebruikt voor certificaat rollovers opgeven. De primaire en secundaire certificaten die u opgeeft moet anders dan de admin-client en alleen-lezen-clientcertificaten die u voor de [Client tot knooppunt beveiliging opgeeft](#client-to-node-security).

Lees [een cluster met behulp van een sjabloon Azure resourcemanager instellen](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het configureren van Certificaatbeveiliging in een cluster voor Azure.

Voor zelfstandige versie van lezen Windows Server [Secure een zelfstandige cluster op Windows met behulp van x.509-certificaten](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Knooppunten naar windows-beveiliging
Voor zelfstandige versie van lezen Windows Server [Secure een zelfstandige cluster op Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Client-naar-knooppunt beveiliging
Verifieert clients en communicatie tussen een client en afzonderlijke knooppunten in het cluster beveiligt. Dit type beveiliging verifieert en waarbij clientcommunicatie, waarin zorgt ervoor dat alleen bevoegde gebruikers toegang tot hebben het cluster en de toepassingen die zijn geïmplementeerd op het cluster. Clients zijn identificatiecode via de Windows-beveiligingsreferenties of hun beveiligingsreferenties certificaat.

![Diagram van client tot knooppunt communicatie][Client-to-Node]

Clusters waarop Azure of zelfstandige clusters waarop Windows kunnen [Certificaatbeveiliging](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows-beveiliging](https://msdn.microsoft.com/library/ff649396.aspx)gebruiken.

### <a name="client-to-node-certificate-security"></a>Client-naar-knooppunt Certificaatbeveiliging
 Certificaatbeveiliging van de client tot knooppunt is geconfigureerd tijdens het maken van het cluster via de Azure-portal resourcemanager sjablonen of een zelfstandige JSON-sjabloon door het opgeven van een beheerder clientcertificaat en/of een clientcertificaat gebruiker.  De beheerder client en gebruiker clientcertificaten-u opgeeft moeten afwijken van de primaire en secundaire certificaten die u voor [knooppunt naar waardepapier opgeeft](#node-to-node-security).

Clients verbinding maken met het cluster met behulp van de beheerder-certificaat hebben volledige toegang tot beheermogelijkheden.  Clients verbinding maken met het gebruik van de gebruiker alleen-lezen-clientcertificaat cluster hebben alleen lezen toegang tot beheermogelijkheden. Met andere woorden worden deze certificaten gebruikt voor de rol grondtal toegangsbeheer (RBAC) verderop in dit artikel wordt beschreven.

Lees [een cluster met behulp van een sjabloon Azure resourcemanager instellen](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het configureren van Certificaatbeveiliging in een cluster voor Azure.

Voor zelfstandige versie van lezen Windows Server [Secure een zelfstandige cluster op Windows met behulp van x.509-certificaten](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Client-naar-knooppunt Azure Active Directory (AAD) beveiliging van Azure
Clusters waarop Azure kunnen ook toegang tot de eindpunten van de management met Azure Active Directory (AAD) beveiligen. Zie [een cluster door instellen met een sjabloon van Azure resourcemanager](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het maken van de benodigde AAD-onderdelen, hoe u deze tijdens het maken van cluster te vullen en hoe u verbinding maken met deze clusters achteraf.

## <a name="security-recommendations"></a>Aanbevelingen voor beveiliging
Het wordt aanbevolen dat u AAD beveiliging gebruiken om te verifiëren clients en certificaten voor knooppunt naar beveiliging voor Azure kolomgroepen.

Clusters het wordt aanbevolen dat u Windows-beveiliging met de groep gebruikt beheerde accounts (GMA) voor de zelfstandige versie van Windows Server als u Windows Server 2012 R2 en Active Directory. Gebruik Windows-beveiliging anders nog steeds met Windows-accounts.

## <a name="role-based-access-control-rbac"></a>Rolgebaseerd toegangsbeheer RBAC)
Toegangsbeheer kan de beheerder cluster beperken van toegang tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers, het cluster veiliger maken. Twee typen voor verschillende access-besturingselementen worden ondersteund voor clients verbinding maken met een cluster: beheerdersrol en gebruikersrol.

Beheerders hebben volledige toegang tot beheermogelijkheden (inclusief mogelijkheden voor alleen-lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen toepassingen en -services.

U opgeven de functies van de client beheerder en de gebruiker op het moment van maken van het cluster doordat afzonderlijk identiteiten (certificaten, AAD enz.) voor elk label. Zie [Rolgebaseerd toegangsbeheer voor Service stof clients](service-fabric-cluster-security-roles.md)voor meer informatie over de standaardinstellingen voor het beheer van access en hoe u de standaardinstellingen wijzigt.


## <a name="x509-certificates-and-service-fabric"></a>X.509-certificaten en Service stof
Digitale certificaten x.509-worden gebruikt om te verifiëren clients en servers en bij het berichten versleutelen en digitaal ondertekenen. Voor meer informatie over deze certificaten, gaat u naar het [werken met certificaten](http://msdn.microsoft.com/library/ms731899.aspx).

Enkele belangrijke aandachtspunten:

- Certificaten die worden gebruikt in clusters productie werkbelasting uitgevoerd moeten worden gemaakt met behulp van een correct geconfigureerde Windows Server certificaatservice of uit een goedgekeurde [Certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority)opgehaald.
- Gebruik nooit een tijdelijk en testen van certificaten in productie die zijn gemaakt met hulpprogramma's zoals MakeCert.exe.
- U kunt een zelfondertekend certificaat gebruiken, maar u moet alleen doen voor test clusters en niet in productie.

### <a name="server-x509-certificates"></a>Server x.509-certificaten

Certificaten zijn de belangrijkste taak van een server (knooppunt) aan clients verifiëren of verifiëren van een server (knooppunt) op een server (knooppunt). Een van de eerste controles wanneer een client of knooppunt knooppunt verifieert is om te controleren van de waarde van de algemene naam in het veld onderwerp. Deze algemene naam of een van de de certificaten onderwerp alternatieve namen moet aanwezig zijn in de lijst met toegestane algemene namen.

Het volgende artikel wordt beschreven hoe u het genereren van certificaten met alternatieve objectnamen (SAN): [het toevoegen van een alternatieve naam voor onderwerp aan een beveiligde LDAP-certificaat](http://support.microsoft.com/kb/931351).

Het veld onderwerp kan meerdere waarden bevatten, elk voorafgegaan door een initialisatie om aan te geven van het waardetype. Meestal is de initialisatie "CN" voor gangbare naam. bijvoorbeeld: "CN = www.contoso.com". Het is ook mogelijk dat het veld onderwerp leeg is. Als het optionele alternatieve onderwerpnaam-veld is ingevuld, moet deze zowel de algemene naam van het certificaat en één item per alternatieve onderwerpnaam bevatten. Deze zijn als de naam van de DNS-waarden ingevoerd.

De waarde van het veld beoogde doeleinden van het certificaat moet een geschikte waarde, zoals "Server" of 'Client-verificatie' bevatten.

### <a name="client-x509-certificates"></a>Client x.509-certificaten

Certificaten zijn meestal niet uitgegeven door een certificeringsinstantie van derden. In plaats daarvan bevat het persoonlijke archief van de huidige locatie meestal clientcertificaten daar geplaatst door een basisinstantie, met een beoogde doel "Client verificatie". De client kan dit certificaat gebruiken als onderlinge verificatie vereist is.

>[AZURE.NOTE] Alle beheertaken uit te voeren op een Service stof cluster vereisen servercertificaten. Clientcertificaten kunnen niet worden gebruikt om te beheren.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Volgende stappen

In dit artikel bevat algemene informatie over cluster beveiliging. Volgende, [een cluster in Azure wordt aangegeven met een sjabloon resourcemanager maken](service-fabric-cluster-creation-via-arm.md) of via de [portal van Azure](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
