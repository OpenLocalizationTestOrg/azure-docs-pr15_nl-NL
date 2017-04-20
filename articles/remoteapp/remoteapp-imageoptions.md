<properties
    pageTitle="Maken van een afbeelding Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over de opties die beschikbaar zijn voor het maken van afbeeldingen voor Azure RemoteApp"
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



# <a name="create-an-azure-remoteapp-image"></a>Een Azure RemoteApp-afbeelding maken

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp gebruikt afbeeldingen om te bewaren van de apps die u met uw gebruikers deelt. (We uw afbeelding maken en Hiermee VMs - die van wat de toegang van gebruikers wanneer ze zich aanmelden bij Azure RemoteApp maken.) Als u wilt maken met uw keuze van toepassingen, een siteverzameling Azure RemoteApp of u nu cloud of hybride, begint u met het maken van een afbeelding met deze toepassingen hebt geïnstalleerd. Maak een siteverzameling die wordt gebruikt die afbeelding, gebruikers toewijzen aan de collectie en apps naar die gebruikers publiceren.

U hebt verschillende opties voor het maken of afbeeldingen gebruiken. De eenvoudige [vereiste](remoteapp-imagereqs.md) voor een afbeelding is dat het uitvoeren van Windows Server 2012 R2 en de rol van het externe bureaublad sessie Host (RDSH) is geïnstalleerd. Hoe u krijgt dat is waar dingen interessante ophalen.

Wanneer u naar afbeeldingen gaat hebt u de volgende opties:

- U kunt importeren en gebruikt u een [afbeelding op basis van een Azure virtuele machines](remoteapp-image-on-azurevm.md). Dit is het handig voor LOB-apps die hebben aangepaste instellingen nodig. U kunt de afbeelding om te werken voor de app aanpassen.
- U kunt [maken en een aangepaste afbeelding uploaden](remoteapp-create-custom-image.md). Dit is het handig als u al een afbeelding die u voor uw on-premises Remote Desktop Services-implementatie gebruikt.
- U kunt een van de [afbeeldingen van sitesjablonen](remoteapp-images.md) deel uitmaakt van uw abonnement RemoteApp. Deze afbeeldingen worden gemaakt en beheerd door het team RemoteApp en sommige standaard-toepassingen (zoals de Office-suite) die u beschikbaar aan uw gebruikers stellen kunt bevatten. Houd er rekening mee dat alleen de afbeelding Office 365 Pro Plus kan worden gebruikt in de productieomgeving invoert.

Ongeacht waar u uw afbeelding ophalen of hoe u deze hebt gemaakt, wilt u Zorg ervoor dat u de [vereisten voor de app](remoteapp-appreqs.md) om ervoor te zorgen dat uw app goed in RemoteApp werkt kennen. De volgende stap is een verzameling [cloud](remoteapp-create-cloud-deployment.md) of [hybride](remoteapp-create-hybrid-deployment.md) maken.
