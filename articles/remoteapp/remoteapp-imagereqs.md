
<properties
    pageTitle="Azure RemoteApp afbeelding vereisten | Microsoft Azure"
    description="Meer informatie over de vereisten voor het maken van afbeeldingen voor gebruik met Azure RemoteApp"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Vereisten voor Azure RemoteApp-afbeeldingen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Afbeelding van een Windows Server 2012 R2 Azure RemoteApp gebruikt voor het hosten van alle programma's die u wilt delen met uw gebruikers. Om een aangepaste afbeelding maken, kunt u beginnen met een bestaande afbeelding of [een nieuw account te maken](remoteapp-create-custom-image.md).

> [AZURE.TIP] Wist u dat uw abonnement Azure RemoteApp beschikt u over naar de afbeelding van een Windows Server 2012 R2 in de galerie met Azure VM die u kunt de afbeelding van uw eigen sjabloon maken? [Controleer of de map af](remoteapp-image-on-azurevm.md).  


De vereisten voor de afbeelding die u voor gebruik met Azure RemoteApp uploaden kunt zijn:


- Aangepaste toepassingen opslaan niet gegevens lokaal op de afbeelding. Deze afbeeldingen stateless zijn en mogen alleen toepassingen.
- De afbeelding bevat geen gegevens die verloren gaan.
- Grootte van de afbeelding moet een veelvoud is van MB. Als u probeert te uploaden van een afbeelding die een veelvoud is, wordt het uploaden mislukt.
- Grootte van de afbeelding moet 127 GB of kleiner.
- Moet zich op een VHD-bestand (VHDX bestanden worden momenteel niet ondersteund).
- De VHD mag geen generatie 2 virtuele machines.
- De VHD kan vaste grootte of dynamisch groeiende zijn. Een dynamisch groeiende VHD wordt aanbevolen, omdat er minder tijd om te uploaden naar Azure dan een vaste grootte VHD-bestand nodig.
- De schijf moet worden geïnitialiseerd met het model opstarten MBR (Record) partitioneren stijl. De GUID partition (GUID) partition tabelstijl wordt niet ondersteund.
- De VHD moet een enkele installatie van Windows Server 2012 R2 bevatten. Meerdere volumes, maar slechts één met een installatie van Windows kan bevatten.
- De rol van het externe bureaublad sessie Host (RDSH) en de functie Bureaubladbelevenis moeten zijn geïnstalleerd.
- De rol van het externe bureaublad verbinding makelaar mag *niet* worden geïnstalleerd.
- Het coderen File System (EFS) moet zijn uitgeschakeld.
- De afbeelding mag SYSPREPed met de parameters **/oobe / generalize/shutdown** (niet gebruiken als de parameter **/mode:vm** ).
- Het uploaden van uw VHD van een ketting momentopname wordt niet ondersteund.

Zie [een Azure RemoteApp-afbeelding maken](remoteapp-imageoptions.md) voor meer informatie over het maken van afbeeldingen voor Azure RemoteApp.
