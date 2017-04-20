
<properties
    pageTitle="Secure apps en bronnen in Azure RemoteApp | Microsoft Azure"
    description="Leer hoe u apps en bronnen in Azure RemoteApp vergrendelen"
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



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Secure apps en bronnen in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp biedt gebruikers toegang tot centraal beheerde Windows apps, waarin u kunt bepalen wat uw gebruikers wel en niet kunnen doen.  Dit is vooral handig wanneer de gebruiker verbinding in een onbeheerde-apparaat (zoals hun persoonlijke Macbook maakt) en u wilt de toegang van gebruikers of -ervaring.

Bijvoorbeeld als u van Active Directory voor gebruikersverificatie gebruikmaakt en u wilt voorkomen dat uw gebruikers gegevens uit een app kopiëren, kunt u een extern bureaublad-Groepsbeleid voorkomen dat gebruikers gegevens kopiëren.

Een ander voorbeeld is als u wilt blokkeren internettoegang voor een bepaalde app in uw siteverzameling. U kunt een regel voor Windows Firewall die de access blokkeert wanneer u de afbeelding voor uw siteverzameling maken maken.

## <a name="implementation-options"></a>Opties voor regelimplementatie

  Hier volgen de belangrijke implementatieopties, die kunnen worden gebruikt afzonderlijk of samen zo nodig:

1.  Als de RemoteApp-verzameling is een domein toegevoegd, kunt u een [Groepsbeleid](https://technet.microsoft.com/library/cc725828.aspx) (met uitzondering van de niet-actief en verbinding verbreken time-out beleidsregels beschreven [hier](../azure-subscription-service-limits.md)) afdwingen.
2.  Als alternatief voor Groepsbeleid (als uw siteverzameling niet domein toegevoegd is of u niet de juiste bevoegdheden over in AD), kunt u [Lokaal beleid](https://technet.microsoft.com/library/cc775702.aspx) configureren in de afbeelding van uw sjabloon.  Houd er rekening mee dat Groepsbeleid troef lokaal beleid wanneer er een conflict.
3.  Sommige OS/app-instellingen worden niet geconfigureerd via beleid, maar kunnen zijn via registersleutel de [RegEdit hulpmiddel](./remoteapp-hybridtrouble.md) gebruiken tijdens het configureren van de afbeelding van uw sjabloon.
4.  U kunt [Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) voor toegangsbeheer netwerk en naar de computer waarop de app wordt uitgevoerd. Zorg ervoor dat u de URL's en poorten definitie hier niet blokkeren.
5.  U kunt [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) gebruiken om te bepalen welke toepassingen en bestanden gebruikers kunnen worden uitgevoerd. Ervaren gebruikers kunnen bijvoorbeeld het uitvoeren van toepassingen die u niet heeft gepubliceerd maar die beschikbaar zijn in de afbeelding die u gebruikt om te maken van de verzameling - AppLocker kan dit blokkeren bepalen.

## <a name="detailed-information"></a>Gedetailleerde informatie

- De volgende RDS beleidsitems zijn waarschijnlijk het meest nuttig zijn:
    - [Apparaat en bronnen](https://technet.microsoft.com/library/ee791794.aspx)
    - [Printeromleiding van de](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profielen](https://technet.microsoft.com/library/ee791865.aspx).
- Houd er rekening mee dat, omleidingen via de RemoteApp PowerShell configureren module (zoals gezien [hier](./remoteapp-redirection.md)) is afhankelijk van de clientcomputer het beleid wilt afdwingen, dus als beveiliging de hoofddoelstelling is u wilt afdwingen van het beleid via het beleid voor een lokale van de sjabloon afbeeldingen of via Groepsbeleid.
- [Windows Server 2012 R2 beleid](https://technet.microsoft.com/library/hh831791.aspx).
- [Beleid van Office 2013](https://technet.microsoft.com/library/cc178969.aspx) (met inbegrip van [het aanpassen van de Office-werkbalk](https://technet.microsoft.com/library/cc179143.aspx)).
