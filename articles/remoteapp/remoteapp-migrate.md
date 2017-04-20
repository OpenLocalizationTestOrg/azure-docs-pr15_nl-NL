
<properties
    pageTitle="Gebruikersgegevens migreren van Azure RemoteApp | Microsoft Azure"
    description="Leer hoe u uw gegevens en afmelden bij Azure RemoteApp migreren."
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



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Het migreren van gegevens in- en afmelden bij Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt veel verschillende hulpprogramma's en methoden [Gebruikersgegevens](remoteapp-upd.md) doorverbinden naar en vanuit Azure RemoteApp. Hier volgen enkele manieren:

- Kopiëren en plakken met delen van het Klembord
- Bestanden en gegevens kopiëren naar een bestandsserver
- Bestanden kopiëren naar OneDrive voor bedrijven via een browser
- Bestanden kopiëren omleiding gebruiken

>[AZURE.NOTE] 
> U kunt niet de OneDrive voor bedrijven of consumenten synchronisatie-agenten - inschakelen ze [niet worden ondersteund](remoteapp-onedrive.md) in Azure RemoteApp.

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopiëren en plakken in Verkenner gebruiken

Kopiëren en plakken met het Klembord is ingeschakeld in RemoteApp-implementaties [al dan niet standaard](remoteapp-redirection.md). Hiermee kunnen gebruikers bestanden kopiëren tussen hun lokale PC en RemoteApp-apps. Vaak tot en met de normale cursus van het gebruik van apps in RemoteApp hebt gebruikers opgeslagen bestanden naar hun UPDs - verplaatsen dat gegevens uit RemoteApp gemakkelijk is:

1. [File Explorer publiceren als een app](remoteapp-publish.md) in een RemoteApp-verzameling. (Houd er rekening mee dat dit een administratieve taak is.)
2. Rechtstreekse uw gebruikers, start u de File Explorer-app die u hebt gepubliceerd en dat gebruiken om te kopiëren en plakken van bestanden in hun UPD zowel terugkomt.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Het uploaden van bestanden en gegevens naar een bestandsserver met behulp van standaard netwerk bestand kopiëren

Vaak organisaties gebruik bestandsservers algemene gegevens wilt opslaan. Als u de servernaam of locatie, kunnen uw gebruikers bladeren in het lokale netwerk voor de server en kopieert u de bestanden er, net zoals hierboven. Opnieuw wilt u File Explorer publiceren naar RemoteApp en deze vervolgens delen met uw gebruikers.

>[AZURE.NOTE] 
> De bestandsserver moet in een netwerk met de geschikt RemoteApp is geïmplementeerd in.

## <a name="copy-files-to-onedrive-for-business"></a>Bestanden kopiëren naar OneDrive voor bedrijven
Hoewel u de OneDrive voor bedrijven synchroniseren agent in RemoteApp kan niet inschakelt, kunt u nog steeds bestanden kopiëren van uw UPD naar OneDrive voor bedrijven via een browser. 

1. File Explorer publiceren naar RemoteApp en vraagt u gebruikers toegang hebben tot de bestanden die door deze app. 
2. Het gemakkelijkst bestanden overbrengen als ze worden gecomprimeerd, zodat gebruikers van een ZIP-bestand met alle bestanden maken moeten verplaatsen naar OneDrive voor bedrijven.
3. Gebruikers vragen om gaat u naar de Office 365-portal en gaat u naar OneDrive en het ZIP-bestand uploaden.

## <a name="copy-files-by-using-drive-redirection"></a>Bestanden kopiëren met behulp van stationsomleiding

Als u [stationsomleiding](remoteapp-redirection.md)hebt ingeschakeld, kunt u hebt al een netwerkstation gemaakt voor uw gebruikers. In dit geval kunnen ze hun bestanden op het omgeleide station zip en sla deze op hun lokale PC.