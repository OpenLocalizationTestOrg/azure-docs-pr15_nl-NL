<properties 
    pageTitle="Azure RemoteApp - hoe doet netwerkbandbreedte en de kwaliteit van zich werk samen? | Microsoft Azure"
    description="Leer hoe netwerkbandbreedte in Azure RemoteApp kan invloed hebben op de kwaliteit van de gebruiker van ervaring."
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

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - hoe doet netwerkbandbreedte en de kwaliteit van zich werk samen?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Wanneer u de [algehele netwerkbandbreedte](remoteapp-bandwidth.md) vereist voor Azure RemoteApp bekijkt, houd rekening met de volgende factoren: dit zijn alle onderdelen van een dynamisch systeem die van invloed op de algehele gebruikerservaring. 

- **Beschikbare netwerkbandbreedte en de huidige netwerkcondities** - een reeks parameters (verlies, latentie, jitter) in hetzelfde netwerk op een bepaald moment kan van invloed zijn op de toepassing streaming ervaring, waarbij een verlaagde algemene gebruikerservaring. De bandbreedte die beschikbaar zijn in uw netwerk is een functie overbelasting, willekeurige verlies, latentie omdat alle parameters van invloed zijn op het besturingselement overbelasting om, wat dan weer besturingselementen de overdrachtssnelheid om te voorkomen dat conflicten.  Bijvoorbeeld, met verlies netwerkbeheerder of netwerk met een hoge latentie, kunt u de gebruiker ongeldige zelfs op een netwerk met 1000 MB bandbreedte. De winst- en latentie afhankelijk van het aantal gebruikers die zich op hetzelfde netwerk en wat die gebruikers doet (bijvoorbeeld, video's bekijken, downloaden of uploaden van grote bestanden, af te drukken).
- **Gebruiksscenario** - de-ervaring is afhankelijk van de gebruikers als personen en als een groep in hetzelfde netwerk presteren. Bijvoorbeeld het lezen van één dia is vereist één frame moeten worden bijgewerkt; Als de gebruiker skims en schuift u over de inhoud van een tekstdocument, moet een hoger aantal frames per seconde worden bijgewerkt. De communicatie heen en weer op de server in dit scenario wordt uiteindelijk meer netwerkbandbreedte gebruiken. Ook rekening houden met een extreme voorbeeld: meerdere gebruikers zijn HD-video's (zoals resolutie van 4K) bekijken, vasthouden HD telefonische vergaderingen, afspelen van 3D-video spellen of werken op CAD-systemen. Al deze kunt zelfs een netwerk met een echt hoge bandbreedte vrijwel onbruikbaar aanbrengen.
- **Schermresolutie en het aantal schermen** - meer netwerkbandbreedte is verplicht voor het volledige update grotere schermen dan kleinere schermen. De onderliggende technologie uitstekend heel van codering en het verzenden van alleen de regio's van de schermen die zijn bijgewerkt, maar eens aan staan, het hele scherm moet worden bijgewerkt. Wanneer de gebruiker heeft een hogere Resolutiescherm (bijvoorbeeld 4K resolutie), is deze update vereist meer bandbreedte dan een scherm met lagere resolutie (zoals 1024x768px). Deze dezelfde logica is van toepassing als u meerdere schermen voor omleiding gebruikt. Bandbreedte nodig heeft om uit te breiden met het aantal schermen.
- **Omleiding van het Klembord en apparaat** - dit is een zeer niet duidelijk probleem, maar in veel gevallen als een gebruiker een grote hoeveelheid gegevens naar het Klembord, slaat het duurt iets van tijd voor die gegevens overbrengen van de client extern bureaublad naar de server. De volgende ervaring kan worden beïnvloed door de ervaring van stroom verzenden van de Klembord-inhoud. Geldt ook voor apparaatomleiding - als een scanner of webcam genereert een groot aantal gegevens die moeten worden verzonden boven op de server of een printer moet ontvangen van een grote document, of lokale opslag beschikbaar moet zijn aan een app uitgevoerd in de cloud een grote bestand te kopiëren, gebruikers ervaart mogelijk verloren frames of tijdelijk video "is geblokkeerd" omdat de gegevens die u nodig hebt voor de apparaatomleiding van het groter de netwerkbandbreedte wordende is nodig heeft. 

Wanneer u de behoeften van uw netwerk bandbreedte evalueren, moet u rekening moet houden alle van deze factoren die als systeem.

Ga nu terug te gaan naar de [belangrijkste netwerk bandbreedte artikel](remoteapp-bandwidth.md)of gaat u naar uw [netwerkbandbreedte](remoteapp-bandwidthtests.md)testen.

## <a name="learn-more"></a>Meer informatie
- [Schatten van de Azure RemoteApp netwerkbandbreedte gebruikt](remoteapp-bandwidth.md)

- [Azure RemoteApp - testen van uw netwerk bandbreedtegebruik met enkele veelvoorkomende scenario 's](remoteapp-bandwidthtests.md)

- [Azure RemoteApp netwerkbandbreedte - algemene richtlijnen (als u niet kunt uw eigen testen)](remoteapp-bandwidthguidelines.md)