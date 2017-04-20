
<properties
    pageTitle="Formaat wijzigen van gegevens voor een VNET in Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over de vereisten van het IP-adres voor Azure RemoteApp met een VNET"
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



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Formaat wijzigen van gegevens voor een VNET in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u Azure RemoteApp gebruikt met een virtueel netwerk (VNET), gebruikt RemoteApp IP-adressen binnen het subnet. Op basis van de schaal van uw RemoteApp-service, moet u ervoor zorgen dat uw subnet voldoende IP-adressen die beschikbaar zijn voor RemoteApp virtuele machines heeft. Terwijl deze richtlijnen hoekformaatgreep is niet perfect gegeven hoe RemoteApp dynamisch draait omhoog en omlaag virtuele machines binnen een verzameling draait, kunt u het subnetbereik van uw schatting. Dit is vooral belangrijk zoals nadat een RemoteApp-service in een VNET wordt geplaatst, u niet kunt groter subnet zonder te RemoteApp verwijderen.

Voor elke RemoteApp-siteverzameling die u wilt uitvoeren op maximale capaciteit, hebt u nodig 100 IP-adressen beschikbaar. Als u een RemoteApp-verzameling in de standaard-abonnement hebt en u wilt de maximale 500 gebruikers hebt, moet u bijvoorbeeld 100 IP-adressen voor die siteverzameling hebt. Daarnaast moet u 100 IP-adressen voor een RemoteApp-verzameling in de eenvoudige-abonnement dat 800 gebruikers heeft. Als u van plan bent om te laten minder gebruikers (kleiner dan het maximale aantal), kunt u de benodigde per siteverzameling IP-adressen verkleinen. De minimale subnet grootte vereiste is 30 IP-adressen (/ 27).

Raadpleeg de volgende informatie om ervoor te zorgen dat uw VNET is geconfigureerd en werkt propertly:

- [Migreren van een persoonlijke VNET naar een Azure-VNET](remoteapp-migratevnet.md)
- [Valideer de VNET Azure voor gebruik met Azure RemoteApp](remoteapp-vnet.md)
