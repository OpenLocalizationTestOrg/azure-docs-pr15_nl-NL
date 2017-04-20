<properties
    pageTitle="Een app publiceren in Azure RemoteApp | Microsoft Azure"
    description="Informatie over het publiceren van toepassingen en bronnen in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Het publiceren van een app in RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Nadat u uw RemoteApp-siteverzameling maakt, moet u publiceert de apps of resources die u beschikbaar wilt maken voor uw gebruikers. De sjabloonafbeeldingen voorzien van uw abonnement heb slechts een paar apps al dan niet standaard - aan de andere apps delen die zijn gepubliceerd, moet u ze te publiceren.

> [AZURE.NOTE] Moet u een app bijwerken? U moet [bijwerken van de afbeelding](remoteapp-update.md) eerst.

Klik op **publiceren**op het tabblad **publiceren** in de portal. U kunt een app toevoegen in menu van de Sjabloonafbeelding van uw **Start** of het pad naar het waarop de app is geïnstalleerd op de afbeelding van de sjabloon. Als u toevoegen vanuit het menu **Start wilt** , kiest u de app te publiceren in de lijst. Als u besluit om het pad naar de app, voert u een naam voor de app en het pad naar de app. Variabelen in het pad - bijvoorbeeld "% systeemstation %" gebruiken in plaats van ' c:\".

> [AZURE.NOTE] Als u toevoegen van uw app vanuit het menu **Start wilt** , moet u beschikken over *toegevoegd die app naar de * *starten* * menu op de afbeelding van uw sjabloon.* Anders RemoteApp worden alleen weergegeven welke *is* in het menu **Start** en u wordt verwarren. 

>Om ervoor te zorgen dat uw app wordt uitgevoerd in het menu **Start** , een snelkoppelingsbestand - **lnk** - tussen de Menu\Programs %systemdrive%\ProgramData\Microsoft\Windows\Start te plaatsen.

> Als u bent vergeten de app toevoegen aan het menu **Start** wanneer u de sjabloon hebt gemaakt, kiest u het pad toevoegen aan de app. (Of de afbeelding van uw sjabloon opnieuw, maar dat is heel iets meer tijdelijke.)


 