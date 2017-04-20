<properties 
    pageTitle="Azure RemoteApp netwerkbandbreedte - algemene richtlijnen | Microsoft Azure"
    description="Sommige Basisnetwerk bandbreedte richtlijnen voor uw Azure RemoteApp verzamelingen en apps te begrijpen."
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
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp netwerkbandbreedte - algemene richtlijnen (als u niet kunt uw eigen testen)

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u de tijd of de functie voor het uitvoeren van de [netwerk bandbreedte tests](remoteapp-bandwidthtests.md) voor Azure RemoteApp niet hebt, moet u hier enkele vrij algemene richtlijnen waarmee u schatten van de netwerkbandbreedte per gebruiker kunt volgen.

Als u een combinatie van deze scenario's hebt, wordt niet aanbevolen iets kleiner dan (of gelijk aan) 10 MB/s als de MINIMUM-netwerkbandbreedte voor moderne internetverbinding apps in een externe-omgeving. (Hoewel, zoals besproken, wordt dit niet een betere garanderen dan gemiddelde gebruikerservaring.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Complexe PowerPoint, eenvoudige PowerPoint

Azure RemoteApp biedt het beste op LAN 100 MB. Bij het 10 MB/s netwerkprofiel wanneer jitter boven 120 ms meer dan 5%, wordt ziet de gebruiker een gemiddelde ervaring. Bij 1 MB/s de verschillende is ongeautomatiseerde verwerking - bijvoorbeeld in een diavoorstelling, de gebruiker ziet mogelijk niet bewegende overgangen helemaal omdat frames worden overgeslagen.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer gemengde PDF-bestand, PDF-bestand, tekst

10 MB/s netwerkprofiel lijkt op LAN in de meeste aspecten. 1 MB/s, vindt u een OK ervaring, hoewel er mogelijk nog enkele jitter wanneer een gebruiker terwijl er afbeeldingen op het scherm.

## <a name="flash-video-youtube"></a>Flash video (YouTube)

100 MB/s LAN biedt de beste ervaring, terwijl 10 MB/s is aanvaardbaar (dat wil zeggen we het tarief weer dat frame bijhouden maar jitter toeneemt). Bij 1 MB/s is jitter zeer hoog en opvallend.

## <a name="word-typing-word-remote-input"></a>Word-typen (Word externe invoer)
Dit is een gebruiksscenario voor met lage bandbreedte. Om 256 KB/s bieden wij zo goed van een ervaring als LAN.

## <a name="learn-more"></a>Meer informatie
- [Schatten van de Azure RemoteApp netwerkbandbreedte gebruikt](remoteapp-bandwidth.md)

- [Azure RemoteApp - hoe doet netwerkbandbreedte en de kwaliteit van zich werk samen?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting uw netwerkbandbreedte gebruikt met enkele veelvoorkomende scenario's](remoteapp-bandwidthtests.md)