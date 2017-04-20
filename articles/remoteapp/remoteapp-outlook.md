<properties
    pageTitle="Outlook gebruiken in Azure RemoteApp | Microsoft Azure" 
    description="Meer informatie over het configureren en gebruiken van Outlook in Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Microsoft Outlook gebruiken in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp ondersteunt Microsoft Outlook O365. Meer informatie over hoe u [Office werkt in Azure RemoteApp](remoteapp-officesubscription.md). Er zijn enkele aanbevolen instellingen voor Outlook wanneer in Azure RemoteApp gebruikt.

## <a name="cached-mode"></a>Modus met cache
Modus met cache is een aanbevolen configuratie als u Outlook gebruikt in Azure RemoteApp. Als u een account met Outlook 2013 voor het gebruik van de Exchange-modus met cache configureert, werkt Outlook 2013 uit een lokale kopie van Microsoft Exchange-postvak van de gebruiker die is opgeslagen in een offline-gegevensbestand (OST-bestand) op de computer van de gebruiker, samen met de Offline offlineadresboek (OAB). Het postvak in de cache en OAB worden regelmatig bijgewerkt van de O365-service. Meer informatie over [de verschillen tussen in de cache en online-modus](https://technet.microsoft.com/library/jj683103.aspx).

De gebruiker kan **De Exchange-modus met cache** of **Onlinemodus** selecteren tijdens de installatie van de account of door de accountinstellingen te wijzigen. U kunt ook een modus of de andere met behulp van de OCT (OCT) of Groepsbeleid implementeren.  

Lees de [Stapsgewijze instructies over het inschakelen van modus met cache](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Zoeken
In Azure RemoteApp heeft met zoeken in Outlook beperkingen. Gegroepeerde VMs Azure RemoteApp gebruikt voor sessies. Search indexeren, is afhankelijk van de computer-ID, dat wil verschillen voor verschillende VMs zeggen. Het is mogelijk dat telkens wanneer een gebruiker zich in Azure RemoteApp, ze worden doorgestuurd naar een nieuwe VM. Dit betekent dat, als we lokale zoekopdracht inschakelt, de indexering wordt uitgevoerd telkens wanneer de computer-ID wordt gewijzigd (wanneer de gebruiker zich op een andere VM). Afhankelijk van de grootte van de. OST-bestand, de indexering duurt erg lang duren en van resources die nodig zijn voor andere apps gebruiken. Zoeken niet alleen zou traag, maar kan geen resultaat oplevert. Met een account-profiel onlinemodus zou dit probleem omzeilen, maar de algehele prestaties zou beïnvloed door het gebrek aan een lokale cache (Zie de bovenstaande link voor meer informatie over het verschil tussen de modus met cache en online). Helaas kunt geïndexeerde/lokale zoekopdracht kan niet worden uitgeschakeld en online zoeken kan niet worden ingeschakeld al dan niet standaard in Outlook 2013.
