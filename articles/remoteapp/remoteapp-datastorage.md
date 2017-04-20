
<properties
    pageTitle="Nooit gevoelige gegevens opslaan op aangepaste afbeeldingen voor Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over de richtlijnen voor het opslaan van gegevens in de aangepaste afbeeldingen in Azure RemoteApp"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Vertrouwelijke gegevens nooit store op aangepaste afbeeldingen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Wanneer u uw eigen toepassing in Azure RemoteApp host, wordt de eerste stap is een aangepaste afbeelding maken. We die aangepaste afbeelding gebruiken om VM-exemplaren die uw apps aan uw gebruikers dienen te maken. De aangepaste afbeelding mag alleen toepassingen en nooit gevoelige gegevens die mogelijk verloren, zoals SQL-databases, personeelsbestanden of speciale gegevensbestanden zoals QuickBooks bedrijf-bestanden bevatten. Alle gevoelige gegevens moet zich bevinden buiten Azure RemoteApp op een bestandsserver, een andere Azure VM, of in SQL Azure wordt aangegeven. De afbeelding moet alleen de toepassing die is verbonden met de gegevensbron en worden de gegevens hosten. Lees [vereisten voor Azure RemoteApp-afbeeldingen](remoteapp-imagereqs.md) voor meer informatie. 

Als u wilt weten over waarom u geen gevoelige gegevens moet opslaan, moet u meer informatie over de werking van Azure RemoteApp. Wanneer een siteverzameling wordt gemaakt of bijgewerkt, achter de schermen meerdere klonen of kopieën van de afbeelding worden gemaakt. Alle exemplaren van deze VM zijn exacte kopieën van de aangepaste afbeelding. Wanneer gebruikers toepassingen starten zijn ze verbonden met een van deze exemplaren VM. Maar hetzelfde exemplaar is niet gegarandeerd en moet niet van belang omdat deze niet-permanente. De VM exemplaren hostingprovider de toepassingen worden niet-permanente en kan worden verwijderd of verwijderde gebaseerd, bijvoorbeeld tijdens het bijwerken van de siteverzameling. 

Zodra de verzameling is ingericht en gebruikers starten verbinding maakt met de VMs, gebruikersgegevens permanente is en beveiligde omdat het is opgeslagen op afzonderlijke opslag binnen een VHD dat verwijst naar een [schijf van de gebruiker-profiel (UPD)](remoteapp-upd.md), dat wil het gebruikersprofiel in c:\users zeggen\<userprofile >. Wanneer een toepassing wordt gestart, wordt de UPD gekoppeld en net als een lokaal gebruikersprofiel verwerkt door het besturingssysteem. Meer informatie over hoe u [Azure RemoteApp bespaart gebruikersgegevens en instellingen](remoteapp-upd.md).

Voorbeeldgegevens die niet in de afbeelding moet bevinden:

- Gegevens voor gebruikers toegang hebben tot gedeeld
- SQL-DB- of QuickBooks DB
- Alle gegevens in D:\

Voorbeeldgegevens die kunnen worden ondergebracht in het standaardprofiel moeten worden gekopieerd naar elke gebruikers UPD:

- Configuratiebestanden per gebruiker
- Scripts die gebruikers nodig zou hebben behouden in hun UPD

Belangrijkste punten:

- Nooit store gevoelige gegevens die gaan op de afbeelding verloren kunnen bij het maken van een aangepaste afbeelding.
- Gevoelige gegevens moet altijd bevinden zich op een afzonderlijk bestand tijdstempelserver afzonderlijk Azure VM, klik op de cloud, en altijd buiten de VM exemplaren hostingprovider van uw Azure RemoteApp-toepassingen. 
- Gebruikersgegevens is opgeslagen en zich blijft voordoen in het profiel schijf van de gebruiker (UPD)


