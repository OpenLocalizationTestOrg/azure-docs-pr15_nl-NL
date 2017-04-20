
<properties
    pageTitle="Lijst met poorten en URL's naar "witte" lijst voor Azure RemoteApp geïmplementeerd in customer network voor virtual | Microsoft Azure"
    description="Informatie over welke poorten en URL's moet u configureren voor communicatie via Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Lijst met poorten en URL's om toegang te verlenen voor Azure RemoteApp geïmplementeerd in klant Virtual Network 

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Het volgende geldt voor Azure RemoteApp een verzameling cloud of hybride als u deze in een virtueel netwerk (VNET implementeert). Lees voor meer informatie over virtuele netwerken, [Virtuele netwerk overzicht](../virtual-network/virtual-networks-overview.md). Als u een netwerk-beveiligingsgroep (NSG) voor het beperken van verkeer naar uw virtueel netwerkbronnen die u hebt gekozen voor Azure RemoteApp hebt gemaakt, zorg ervoor dat de volgende toegankelijk zijn en toegestane tot en met de beveiligingsbeleid op het virtuele netwerk. Voor meer informatie over beveiligingsgroepen netwerk, Lees [Wat is een beveiligingsgroep netwerk? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp subnet moet toegang tot deze eindpunten en URL's: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Uitgaande: TCP: 443, TCP: 10101-10175 
*    Optioneel: UDP: 10201-10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-clients moeten toegang tot deze eindpunten en URL's: 

Door clients ik betekent dat de bureaubladen, enzovoort dat personen met verbinding maken met de apps die zijn geïmplementeerd in de verzameling Azure RemoteApp-apparaten.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (de optioneel UDP-poorten zijn voor dit adres) 
-  https://login.Windows.NET  
-  https://login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Uitgaande: TCP: 443  
-  Optionele - UDP: 3391 