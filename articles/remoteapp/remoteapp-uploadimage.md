
<properties
    pageTitle="Een aangepaste afbeelding uploaden voor Azure RemoteApp | Microsoft Azure"
    description="Informatie over het uploaden van een aangepaste afbeelding voor Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Een aangepaste afbeelding uploaden voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Nu u de afbeelding van uw aangepaste sjabloon hebt gemaakt of met wijzigingen bijgewerkt, moet u die afbeelding uploaden naar uw Azure RemoteApp-afbeeldingsbibliotheek is geopend. Gebruik deze stappen.


## <a name="before-you-start"></a>Voordat u begint

1.      Controleer of uw aangepaste afbeelding voldoet aan de [vereisten van de afbeelding](remoteapp-imagereqs.md) en de [toepassingsvereisten](remoteapp-appreqs.md).
2.      Installeer de [Azure PowerShell-module](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Stap voor stap over hoe u aangepaste afbeelding uploaden

1.      Open Azure Management Portal en Ga naar de pagina RemoteApp.
2.      Klik op **uploaden** onder aan de pagina op het tabblad **afbeeldingen van sitesjablonen** .
4.      Voer een beschrijvende naam voor uw afbeelding en geef de opslaglocatie van het account. Controleer of de locatie dezelfde locatie als uw RemoteApp-siteverzameling of naar een locatie waar u een account maakt.
5.      Wanneer u wordt gevraagd, kunt u het script downloaden naar uw lokale PC.
6.      De opdrachtparameters in het tekstvak naar het Klembord kopiëren.
7.      Open een verhoogde Windows PowerShell-venster.
8.      Navigeer naar dezelfde map waarin u het script hebt gedownload vanuit het venster verhoogde Windows PowerShell.
9.      Plak de gekopieerde opdracht en druk op **Enter**.

    Het uploadproces begint en duur afhankelijk zijn van veel factoren zoals uw netwerksnelheid en de grootte van de afbeelding

11.    Als het uploaden vanwege netwerk onderbroken of dingen zoals dat mislukt, kunt u altijd de uploadproces dat u van start is gegaan hervatten. Als u wilt doorgaan met het uploaden, het script opnieuw met de opdrachtregel worden uitgevoerd.

> [AZURE.WARNING] Wijzig nooit het script uploaden. Afgestemde controles zijn uitgevoerd om ervoor te zorgen dat de afbeelding voldoet aan de vereisten van de afbeelding en de toepassingsvereisten.

## <a name="common-problems"></a>Algemene problemen

- Controleer of dat u Windows PowerShell, niet Azure PowerShell gebruikt. U moet de Azure PowerShell-module worden geïnstalleerd omdat bepaalde modules u nodig tijdens het uploaden hebt.
- Het script nooit wijzigen, validatie zijn er voor uw gemak.
- Als het vhd-bestand is vergrendeld tijdens het uploaden, kopieert u het bestand of deze opnieuw verplaatsen naar een nieuwe locatie en een poging upload. Het is mogelijk dat er een Windows-proces waardoor uploaden.  
