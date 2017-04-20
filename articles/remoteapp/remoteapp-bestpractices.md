<properties
    pageTitle="Azure RemoteApp aanbevolen procedures | Microsoft Azure"
    description="Aanbevolen procedures voor het configureren en gebruiken van Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Aanbevolen procedures voor het configureren en gebruiken van Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

De volgende informatie kunt u configureren en gebruiken van Azure RemoteApp productief.

## <a name="connectivity"></a>Connectiviteit


- Gebruik altijd de meest recente clientversie. Oudere clients kan resulteren in verbindingsproblemen en andere anders werken ervaringen. Inschakelen van automatische toepassingsupdates voor uw apparaat zorgt ervoor dat de meest recente client wordt altijd geïnstalleerd.
- Gebruik altijd de meest stabiele en betrouwbare internetverbinding beschikbaar zijn voor u.  
- Proxyverbindingen ondersteund gebruik alleen voor de voor optimale verbindingsprestaties.  De SOCKS-proxy wordt niet ondersteund.

## <a name="applications"></a>Toepassingen


- Opslaan en sluit RemoteApp-toepassingen wanneer u klaar bent met de toepassing. De toepassing niet sluiten, kan dit leiden tot verlies van gegevens.
- Aangepaste toepassingen voordat u deze gebruikt in Azure RemoteApp valideren. Dit geldt ook voor zorgen dat ze werken aan een sessie voor multi-platform en onnodige resources zoals geheugen en CPU die mogelijk beroven van een andere gebruiker in dezelfde collectie processortijd niet gebruiken. Voor informatie downloaden en controleer de [Toepassing compatibiliteit aanbevolen procedures voor het Remote Desktop Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Configuratie en beheer


- Uw afbeeldingen van sitesjablonen up-to-date te houden, installatie van software-updates en andere kritieke correcties naar wens. Dit zorgt ervoor dat zoals Azure RemoteApp auto-schalen om te voldoen aan de capaciteit van de elk exemplaar is hersteld.  
- Controleer of dat uw Active Directory Federation Services (AD FS)-implementatie veilig en betrouwbaar is. Anders mislukken client authenticatie, voorkomen dat gebruikers toegang krijgen tot het Azure RemoteApp.
- Afbeeldingen van sitesjablonen met geïnstalleerde toepassingen, rollen of functies configureren zodat ze stateless zijn. Ze mag niet volledig vertrouwen op alle exemplaren van de virtuele machines in een RemoteApp-service wordt in een permanente staat.
    - Alle gegevens van de gebruiker worden opgeslagen in gebruikersprofielen of andere opslaglocaties buiten de service, zoals on-premises implementatie bestand waarden voor aandelen of OneDrive.
    - Gedeelde gegevens opslaan in opslaglocaties buiten de service, zoals on-premises implementatie bestand waarden voor aandelen of OneDrive.
    - Een hele systeem-instellingen configureren in de afbeelding van de sjabloon in plaats van op afzonderlijke virtuele machines in een service.
    - Automatische software-updates voor gepubliceerde toepassingen uitschakelen - in plaats daarvan deze handmatig wilt toepassen op de afbeelding van de sjabloon en deze te testen voordat u van de sjabloon implementeren.
