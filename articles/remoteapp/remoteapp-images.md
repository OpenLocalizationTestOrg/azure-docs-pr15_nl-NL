<properties
    pageTitle="Wat staat er in de sjabloonafbeeldingen Azure RemoteApp? | Microsoft Azure"
    description="Meer informatie over de afbeeldingen van sitesjablonen wordt geleverd bij Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Wat staat er in de sjabloonafbeeldingen Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Uw abonnement Azure RemoteApp omvat drie afbeeldingen van sitesjablonen:


- Windows Server 2012
- Microsoft Office 365 ProPlus (Office 365-abonnement vereist)
- Microsoft Office 2013 Professional Plus (alleen voor proefabonnement)

> [AZURE.IMPORTANT]Uw abonnement Azure RemoteApp verleent dat u toegang tot de software in de afbeeldingen, met uitzondering van Office 365 ProPlus, die vereist is voor een afzonderlijk abonnement, en Office 2013, die niet kan worden gebruikt in productie. Dit betekent dat u de programma's of apps op de afbeeldingen van sitesjablonen met uw gebruikers delen kunt. Als u een verzameling die gebruikmaakt van de afbeelding van Windows Server 2012 R2 maakt, kunt u bijvoorbeeld systeem Center Endpoint Protection voor gebruikers toegang tot via RemoteApp publiceren.
>
> Bekijk de [RemoteApp volumelicenties details](remoteapp-licensing.md) voor meer informatie. En [Gebruik van Office met Azure RemoteApp](remoteapp-o365.md) voor de Office-licenties info.

Lees verder voor meer informatie over wat elke afbeelding bevat.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("de vanille afbeelding")
In deze afbeelding is gebaseerd op Microsoft Windows Server 2012 R2 Datacenter-besturingssysteem en heeft de volgende functies en functies die zijn geïnstalleerd om te voldoen aan de vereisten voor de afbeeldingen van sitesjablonen Azure RemoteApp:


- .NET framework 4.5 3.5.1, 3.5
- Bureaubladbelevenis
- Inkt en handschriftservices
- Media Foundation
- Externe bureaubladsessie Host
- Windows PowerShell 4.0
- Windows filter wissen
- WoW64 ondersteuning

In deze afbeelding, heeft ook de volgende toepassingen zijn geïnstalleerd:

- Adobe Flash Player
- Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows MediaPlayer


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (abonnement vereist)
Office 365 is het meest gevraagde-toepassing, zodat we de afbeelding van een "aangepaste" voor het werken met gemaakt.

In deze afbeelding is een uitbreiding van de vanille afbeelding en heeft de volgende onderdelen van Microsoft Office 365 ProPlus naast de onderdelen die worden beschreven in de Windows Server 2012 R2-afbeelding hebt geïnstalleerd:


- Access
- Excel
- Lync
- OneNote
- OneDrive voor bedrijven (Let op de synchronisatie-agent wordt niet ondersteund voor gebruik met Azure RemoteApp)
- Outlook
- PowerPoint
- Word
- Microsoft Office taalprogramma 's

De afbeelding bevat ook Visio Pro en Project Pro.

En de volgende toepassingen, ook:

- Systeemeigen SQL-client
- ODBC-stuurprogramma
- SQL Server Data Mining-client
- MasterDataServices-client
- Microsoft Publisher
- PowerQuery
- Vormbestand


Volledige functionaliteit van Office 365 ProPlus apps is alleen beschikbaar voor gebruikers die een abonnement voor Office 365 ProPlus hebt. Voor meer informatie over de Office 365 raadpleegt-abonnementen u [Office 365-service-abonnementen](http://technet.microsoft.com/library/office-365-plan-options.aspx). Hebt u nog vragen? Bekijk de [Office 365 + RemoteApp](remoteapp-o365.md) -informatie. Ook het nieuwe [het gebruik van uw Office 365-abonnement met Azure RemoteApp](remoteapp-officesubscription.md)-artikel raadplegen.

Opmerking die u moet afzonderlijk - licentie van Office 365 ProPlus, Visio Pro en Project Pro elke ze hun eigen licentie hebben.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (alleen voor proefabonnement)
Tijdens de gratis proefabonnement periode, kunt u de service met de Office 2013-afbeelding testen.

In deze afbeelding is een uitbreiding van de vanille afbeelding en heeft de volgende onderdelen van Microsoft Office 2013 Professional Plus naast de onderdelen die worden beschreven in de Windows Server 2012 R2-afbeelding hebt geïnstalleerd:


- Access
- Excel
- Lync
- OneNote
- OneDrive voor bedrijven (Let op de synchronisatie-agent wordt niet ondersteund voor gebruik met Azure RemoteApp)
- Outlook
- PowerPoint
- Project
- Visio
- Word
- Microsoft Office taalprogramma 's

> [AZURE.IMPORTANT]**Juridische informatie:** In deze afbeelding is niet inbegrepen een Microsoft Office-licentie en *kan niet worden gebruikt voor productie*. De Office 2013 Professional Plus afbeelding is bedoeld voor proefabonnement gebruik alleen. Als u Office-apps gebruiken in Azure RemoteApp voor productie wilt, moet u de Office 365 ProPlus afbeelding gebruiken. Zie voor meer informatie over licenties Office, [Office 365 gebruiken met Azure RemoteApp](remoteapp-o365.md)
