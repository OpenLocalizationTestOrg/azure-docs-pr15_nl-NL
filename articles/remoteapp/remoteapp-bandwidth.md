
<properties 
    pageTitle="Schatten van de Azure RemoteApp netwerkbandbreedte gebruikt | Microsoft Azure"
    description="Meer informatie over de vereisten voor de bandbreedte van netwerk voor uw Azure RemoteApp verzamelingen en apps."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Schatten van de Azure RemoteApp netwerkbandbreedte gebruikt 

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp gebruikt de Remote Desktop Protocol (RDP) voor de communicatie tussen toepassingen die wordt uitgevoerd in de cloud Azure en uw gebruikers. In dit artikel bevat enkele eenvoudige richtlijnen die kunt u die netwerkgebruik schatten en potentieel evalueren netwerkbandbreedte gebruikt per Azure RemoteApp-gebruiker.

Schatting van bandbreedtegebruik per gebruiker is zeer complexe en meerdere toepassingen tegelijk worden uitgevoerd in multitasking scenario's waarin toepassingen mogelijk van invloed zijn elkaars prestaties op basis van hun behoefte netwerkbandbreedte vereist. Zelfs het type extern-bureaubladclient (zoals Mac client versus HTML5 client) kan leiden tot verschillende bandbreedteresultaten. Als u deze problemen behandelen, wordt we de gebruik scenario's Splits in verscheidene van de algemene categorieën repliceren echte scenario's. (Indien de echte scenario natuurlijk, een combinatie van categorieën is en per gebruiker verschilt.)

Voordat we verder - gaat, houd er rekening mee dat we wordt ervan uitgegaan dat RDP biedt een goed naar bronnen met uitstekende ervaring voor de meeste scenario's voor gebruik in netwerken met een latentie onder 120 ms en bandbreedte meer dan 5 MB: dit is gebaseerd op de mogelijkheid van RDP dynamisch aanpassen met behulp van de beschikbare netwerkbandbreedte en de bandbreedte geschatte toepassing behoeften. In dit artikel gaat dan de 'federatieve gebruik' om te zoeken op de rand, waar scenario's beginnen te laten verdwijnen en gebruikerservaring beginnen af te nemen.

Bekijk nu de volgende artikelen voor meer informatie, met inbegrip van factoren u rekening moet houden, aanbevelingen van de basislijn en wat we bevat geen in onze maakt een schatting.

- [Hoe doet netwerkbandbreedte en de kwaliteit van zich werk samen?](remoteapp-bandwidthexperience.md)
- [Uw netwerk bandbreedtegebruik met enkele veelvoorkomende scenario's testen](remoteapp-bandwidthtests.md)
- [Snelle richtlijnen als u niet beschikt over de tijd of de mogelijkheid om te testen](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Wat zijn we niet inclusief?

Wanneer u de voorgestelde tests en onze aanbevelingen algehele (en weliswaar algemene), let er zijn verschillende factoren die we niet hebt beschouwd. De gebruikerservaring problemen verstrekt door de asymmetrische aard van upload versus download bijvoorbeeld bandbreedte. De asymmetrische aard van de meeste Wi-Fi-netwerken wordt ook invloed op de prestaties en de gebruikerservaring Beeld. Voor interactieve scenario's het volgende verkeer mogelijk prioriteit krijgen lager dan het boven, die mogelijk het aantal verloren video of audio frames vergroten en kunnen daarom van invloed zijn op de gebruiker beeld van de streaming-ervaring. U kunt uw eigen experimenten om te zien wat is een goed idee voor uw specifieke use-case en netwerk kunt uitvoeren.

Hoewel we apparaatomleiding bespreken, heeft we geen rekening de bandbreedte gevolgen van het netwerkverkeer die worden veroorzaakt door bijgevoegde apparaten, zoals opslag, printers, scanners, web camera's en andere USB-apparaten. Het effect van deze apparaten is meestal tijdelijk de behoeften van de bandbreedte bereikt en meer verdwijnt wanneer de taak voltooid is. Maar als u vaak klaar bandbreedte aanvraag mogelijk helemaal opvallend.

We ook niet aan de orde hoe een gebruiker kan van invloed zijn op andere gebruikers in hetzelfde netwerk. Bijvoorbeeld één gebruiker door andere 4K video op een 100 MB/s-netwerk mogelijk aanzienlijk van invloed zijn op andere gebruikers op die hetzelfde netwerk, probeert te dezelfde taak. Helaas krijgt het geleidelijk moeilijker om te bepalen wat de invloed van gelijktijdige gebruik geven een algemene of die omvat van alle aanbeveling over de manier waarop het systeem op aggregatie uitvoert. Alle we kunt uitspreken is dat de onderliggende protocol technologie brengt het beste gebruik van de beschikbare netwerkbandbreedte, maar beschikt over de beperkingen.