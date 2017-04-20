<properties
    pageTitle="Wat is er nieuw in Azure stapel | Microsoft Azure"
    description="Wat is er nieuw in Azure stapel"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Wat is er nieuw in Azure stapel Technical Preview-2
Biedt nieuwe functies in deze release voor zowel tenants en beheerders.

## <a name="network"></a>Netwerk   
   - [IDN](azure-stack-understanding-dns-in-tp2.md) biedt registratie interne netwerk en Domain Name System (DNS) resolutie zonder extra DNS-infrastructuur.
   - [Virtuele netwerkgateways](azure-stack-create-vpn-connection-one-node-tp2.md) bieden VPN connectiviteitsopties naar Azure of on-premises bronnen.
   - Gebruiker gedefinieerd Routes kunt routeert netwerkverkeer via firewalls, beveiliging, andere apparaten en andere services.
   - U kunt resources netwerk maken van Marketplace.   

## <a name="storage"></a>Opslag
 - [Azure wachtrijen](https://msdn.microsoft.com/library/dd179353.aspx) inschakelen betrouwbaar en permanente service messaging.
 - Prestatiegegevens voor [opslag-analyses](https://msdn.microsoft.com/library/azure/hh343270.aspx) vastleggen opslag. U kunt deze gegevens aanvragen aanwijzen, gebruik trends analyseren en diagnose stellen bij problemen met uw account opslag.
 - Blobopslag ondersteunt [toevoegbewerking blokkeren](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Ondersteuning voor Premium opslag API-account.
 - Opslag serviceondersteuning voor algemene hulpprogramma's en SDK's, zoals Azure CLI, PowerShell, .NET, Python en Java SDK tenant. 
 - Gedetailleerde delegeren van toegang tot uw opslagservices inschakelen [Opslag Account gedeeld Access handtekening](https://msdn.microsoft.com/library/azure/mt584140.aspx) zonder dat u moet uw volledige accountsleutel delen.  
 - [Groep beheerde serviceaccounts](https://technet.microsoft.com/library/hh831477.aspx) opslagservices nu gebruiken voor sterke beveiliging met lage management realiseren.
 - U kunt niet-gebruikte tenant resources op de aanvraag vrijgemaakt.
 - Verwijderde opslagruimte account bewaarbeleid lengte kan worden geconfigureerd.
 - U kunt verwijderde tenant opslag accounts herstellen.

## <a name="compute"></a>Berekenen
- U kunt virtuele machines toewijzing.
- U kunt Implementeer deze opnieuw VM uitbreidingen voor probleemoplossing- en configuratieservices beheer.
- U kunt het formaat van VM schijven.
- Virtuele machines kan meerdere netwerkinterfaces hebben.

### <a name="portal-experience"></a>Portal-ervaring
 - Azure stapel gebieden zijn een logische eenheid schaal en beheer in Azure stapel. In dit voorbeeld, kunt u informatie weergeven op services zoals berekeningscluster, het netwerk en opslag per regio.
 - Nu kunt u de interface van de [azure-stack-updates.md] (updates) bekijken.

## <a name="key-vault"></a>Belangrijke kluis
- [Toets kluis Azure gestapelde](azure-stack-kv-intro.md) biedt secure beheer van uw sleutels en wachtwoorden voor cloud-apps.
- U kunt controleren en het gebruik van de sleutel door apps en VMs controleren.

## <a name="billing-and-usage"></a>Facturering en het gebruik
 - [Facturering en verbruik API's](azure-stack-billing-and-chargeback.md) tonen gegevens op hoe uw services worden verbruikt.  
 - Gedelegeerd Providers inschakelen wederverkopers voor het aanbieden van uw services Azure stapel naar hun klanten.

## <a name="monitoring-and-health"></a>Cmdlets voor controle en systeemstatus
 - Nieuwe functies beschikbaar in de portal en API's kunt u proactief te bekijken en beheren van meldingen van uw omgeving.  

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over Azure-Stack Haalbaarheidstest architectuur](azure-stack-architecture.md)      
- [Meer informatie over vereisten voor implementatie](azure-stack-deploy.md)
- [Azure stapel implementeren](azure-stack-run-powershell-script.md)

  
