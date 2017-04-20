<properties
    pageTitle="Prestaties verbeteren door het comprimeren van bestanden in Azure CDN | Microsoft Azure"
    description="Leer hoe u het bestand doorverbinden sneller en prestaties van de pagina laden verhogen door uw bestanden in Azure CDN te comprimeren."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="improve-performance-by-compressing-files"></a>De prestaties verbeteren door bestanden te comprimeren

Compressie is een eenvoudige en effectieve methode om te verbeteren bestand doorverbinden snelheid en pagina laden prestaties verbeteren door te verkleinen van de bestandsgrootte voordat deze is verzonden vanuit de server. Er Hiermee reduceert u bandbreedtekosten en beschikt u over een meer heeft gereageerd ervaring voor uw gebruikers.

Er zijn twee manieren compressie inschakelen:

- U kunt compressie inschakelen op de server origin, waarin de hoofdletters/kleine letters de CDN wordt via de gecomprimeerde bestanden en gecomprimeerde bestanden overbrengen op clients die deze aanvragen.
- Kunt u de compressie rechtstreeks op CDN edge-servers, waarin de hoofdletters/kleine letters de CDN wordt de bestanden comprimeren en dienen voor eindgebruikers, zelfs als ze worden niet gecomprimeerd door de oorspronkelijke server inschakelen.

> [AZURE.IMPORTANT] CDN-configuratiewijzigingen enige tijd om te doorvoeren via het netwerk.  Voor <b>Azure CDN van Akamai</b> profielen voltooit het doorgeven van meestal onder een minuut.  Voor <b>Azure CDN uit Verizon</b> profielen ziet u meestal uw wijzigingen zijn van toepassing binnen 90 minuten.  Als dit de eerste keer dat u de compressie voor uw eindpunt CDN hebt ingesteld, kunt u overwegen 1-2 uur wachten om er zeker van te zijn dat de compressie instellingen hebt doorgegeven aan de POP's voor probleemoplossing

## <a name="enabling-compression"></a>Compressie inschakelen

> [AZURE.NOTE] De standaardweergave en de Premium-CDN lagen bieden dezelfde compressie functionaliteit, maar verschilt van de gebruikersinterface.  Zie voor meer informatie over de verschillen tussen de standaard- en Premium CDN lagen, [Azure CDN overzicht](cdn-overview.md).

### <a name="standard-tier"></a>Standaard laag

> [AZURE.NOTE] Deze sectie wordt toegepast op **Azure CDN standaard uit Verizon** - en **Azure CDN van Akamai** profielen.

1. Klik op het blad CDN-profiel op het CDN-eindpunt dat u wilt beheren.

    ![CDN profiel blade eindpunten](./media/cdn-file-compression/cdn-endpoints.png)

    Hiermee opent u het blad CDN-eindpunt.

2. Klik op de knop **configureren** .

    ![CDN profiel blade knop beheren](./media/cdn-file-compression/cdn-config-btn.png)

    Hiermee opent u het blad CDN configuratie.

3. **Compressie**inschakelen.

    ![CDN-compressieopties](./media/cdn-file-compression/cdn-compress-standard.png)

4. Gebruik van de standaardtypen of wijzig de lijst door verwijderen of bestandstypen toe te voegen.
    
    > [AZURE.TIP] Tijd mogelijk niet verdient compressie toepassen op gecomprimeerde indelingen, zoals ZIP-, MP3-, MP4-, JPG-, enzovoort.
    
5. Wanneer u klaar bent, klikt u op de knop **Opslaan** .

### <a name="premium-tier"></a>Premium laag

> [AZURE.NOTE] Deze sectie wordt toegepast op **Azure CDN Premium uit Verizon** profielen.

1. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-file-compression/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

2. Plaats de muisaanwijzer boven de **HTTP grote** tab en vervolgens de muis boven de **Cache-instellingen** : flyout.  Klik op **compressie**.

    Compressieopties worden weergegeven.

    ![Bestandscompressie](./media/cdn-file-compression/cdn-compress-files.png)

3. Compressie inschakelen door te klikken op het keuzerondje **Compressie ingeschakeld** .  Voer de MIME-typen die u wilt comprimeren als een lijst door komma's gescheiden (geen spaties) in het tekstvak **Bestandstypen** .
        
    > [AZURE.TIP] Tijd mogelijk niet verdient compressie toepassen op gecomprimeerde indelingen, zoals ZIP-, MP3-, MP4-, JPG-, enzovoort. 

4. Wanneer u klaar bent, klikt u op de knop **bijwerken** .


## <a name="compression-rules"></a>Regels voor compressie

Deze tabel wordt beschreven Azure CDN compressie gedrag voor elke scenario.

> [AZURE.IMPORTANT] Voor **Azure CDN uit Verizon** (standaard en Premium), worden alleen in aanmerking komend bestanden gecomprimeerd.  Als u in aanmerking komen voor compressie, moet een bestand:
>
> - Groter zijn dan 128 bytes.
> - Niet groter zijn dan 1 MB.
> 
> Voor **Azure CDN van Akamai**zijn alle bestanden in aanmerking komen voor compressie.
>
> Voor alle Azure CDN-producten moet een bestand een MIME-type dat [geconfigureerd voor compressie is](#enabling-compression).
>
> **Azure CDN uit Verizon** profielen (standaard en Premium) **gzip**, **verkleinen**of **bzip2** codering te ondersteunen.  **Azure CDN van Akamai** profielen wordt alleen ondersteund **gzip** -codering.
>
> **Azure CDN van Akamai** eindpunten aanvragen altijd **gzip** gecodeerde bestanden vanaf de oorsprong, ongeacht de aanvraag van de client.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Compressie uitgeschakeld of bestand wordt niet in aanmerking voor compressie

|De gewenste indeling client (via accepteren-codering koptekst)|In de cache-bestandsindeling|CDN-antwoord op de client|Notities|
|----------------|-----------|------------|-----|
|Gecomprimeerd|Gecomprimeerd|Gecomprimeerd|   |
|Gecomprimeerd|Niet-gecomprimeerd|Niet-gecomprimeerd|    | 
|Gecomprimeerd|Niet in de cache|Wel of niet gecomprimeerd|Afhankelijk van de oorsprong antwoord|
|Niet-gecomprimeerd|Gecomprimeerd|Niet-gecomprimeerd|    |
|Niet-gecomprimeerd|Niet-gecomprimeerd|Niet-gecomprimeerd|    |   
|Niet-gecomprimeerd|Niet in de cache|Niet-gecomprimeerd|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Compressie en de bestandsnaam komt in aanmerking voor compressie

|De gewenste indeling client (via accepteren-codering koptekst)|In de cache-bestandsindeling|CDN-antwoord op de client|Notities|
|----------------|-----------|------------|-----|
|Gecomprimeerd|Gecomprimeerd|Gecomprimeerd|CDN transcodes tussen ondersteunde indelingen|
|Gecomprimeerd|Niet-gecomprimeerd|Gecomprimeerd|CDN uitvoert compressie|
|Gecomprimeerd|Niet in de cache|Gecomprimeerd|CDN uitvoert compressie als origin niet gecomprimeerd geeft.  **Azure CDN uit Verizon** wordt het niet-gecomprimeerde bestand op de eerste aanvraag doorgeven en vervolgens comprimeren en het bestand voor volgende aanvragen in cache.  Bestanden met `Cache-Control: no-cache` koptekst nooit worden gecomprimeerd. 
|Niet-gecomprimeerd|Gecomprimeerd|Niet-gecomprimeerd|CDN decompressie kunt uitvoeren|
|Niet-gecomprimeerd|Niet-gecomprimeerd|Niet-gecomprimeerd|     |  
|Niet-gecomprimeerd|Niet in de cache|Niet-gecomprimeerd|     |

## <a name="media-services-cdn-compression"></a>Media Services CDN compressie

Voor Media Services CDN ingeschakeld streaming eindpunten, compressie is standaard ingeschakeld voor de volgende inhoudstypen: application/vnd.ms-sstr + XML-, application/dash+xml,application/vnd.apple.mpegurl, toepassing/f4m + XML. U kunt geen-/ uitschakelen compressie voor de desbetreffende typen met behulp van de Azure portal.  

## <a name="see-also"></a>Zie ook
- [Probleemoplossing CDN bestandscompressie](cdn-troubleshoot-compression.md)    
