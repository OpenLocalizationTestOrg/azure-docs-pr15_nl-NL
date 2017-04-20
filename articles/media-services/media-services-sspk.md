<properties 
    pageTitle="Volumelicenties Microsoft® vloeiende Streaming Client Kit overdragen" 
    description="Informatie over hoe u naar de Microsoft® vloeiende Streaming Client overbrengen Kit-licenties." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Volumelicenties Microsoft® vloeiende Streaming Client Kit overdragen

##<a name="overview"></a>Overzicht

Microsoft vloeiende Streaming Client overbrengen Kit (**SSPK** voor korte) is een goede Streaming client implementatie die is geoptimaliseerd voor de help ingesloten apparaatfabrikanten, kabel en mobiele providers inhoud serviceproviders, handset fabrikanten, onafhankelijke leveranciers (ISV's) en oplossing providers producten en services voor geavanceerde streaming inhoud in vloeiende Streaming format streaming maken. SSPK is een apparaat en de platform onafhankelijke implementatie van vloeiende Streaming client die kan worden geïmplementeerd door de licentiehouder aan elk apparaat en de platform. 

Opgenomen onder is een hoog niveau architectuur en IIS vloeiende Streaming overbrengen Kit vak is de vloeiende Streaming Client-implementatie geleverd door Microsoft en alle corelogica voor het afspelen van vloeiende Streaming inhoud bevat. Dit is vervolgens overgezet door partners voor een specifieke apparaat of platform door het juiste interfaces implementeren. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Beschrijving

SSPK wordt op termen die bronnen met uitstekende bedrijven waarde bieden licentie. SSPK licentie bevat de bedrijfstak met:

- Vloeiende overbrengen Kit Streaming bron in C++ 
  - implementeert vloeiende Streaming Client functionaliteit
  - Hiermee kunt u opmaak parseren, heuristiek, buffer logica, enzovoort toevoegen.
- Speler toepassing API 's 
  - Programming Interface voor interactie met een toepassing voor media player.
- Platform Abstraction laag (PAL) Interface 
  - Programming Interface voor interactie met het besturingssysteem (threads, sockets)
- Hardware Abstraction HAL (Layer)-Interface 
  - Programming Interface voor interactie met hardware A / V decoders (decoderen, weergeven)
- Digital Rights Management (DRM) Interface 
  - API's voor het afhandelen van DRM tot en met de DRM Abstraction laag (DAL)
  - Microsoft PlayReady overbrengen Kit wordt afzonderlijk worden geleverd, maar werkt naadloos samen via deze interface. Voor meer informatie over Microsoft PlayReady Device licenties, klikt u op [hier](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Voorbeelden van de implementatie 
  - voorbeeld PAL implementatie voor Linux
  - voorbeeld HAL implementatie voor GStreamer

##<a name="licensing-options"></a>Opties voor volumelicenties

Microsoft vloeiende Streaming Client overbrengen Kit is ter beschikking gesteld houders onder twee afzonderlijke gebruiksrechtovereenkomst: een voor het ontwikkelen van vloeiende Streaming Client tussentijdse producten en een andere voor vloeiende Streaming Client definitief producten aan eindgebruikers distribueren.
 
- Voor chipsetfabrikanten, systeemintegrators of onafhankelijke softwareleveranciers (ISV's) die nodig hebben een broncode kit tussentijdse producten ontwikkelen overdragen, moet een Microsoft vloeiende Streaming Client overbrengen Kit **Tussentijdse productlicentie** worden uitgevoerd.
- Voor fabrikanten of ISV's die nodig distributiegroep rights voor vloeiende Streaming Client definitief producten aan eindgebruikers, moet de Microsoft vloeiende Streaming Client overbrengen Kit **Definitief productlicentie** worden uitgevoerd.

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft-vloeiend Streaming Client Kit tussentijdse productlicentie overdragen

Onder deze licentie biedt Microsoft een vloeiende Streaming Client overbrengen Kit en de benodigde intellectuele eigendomsrechten ontwikkelen en vloeiende Streaming Client tussentijdse producten distribueren naar andere vloeiende Streaming Client overbrengen Kit apparaat houders die vloeiend Streaming Client definitief producten distribueert.

####<a name="fee-structure"></a>Kostenstructuur

Amerikaans 50.000 eenmalige licentiekosten biedt toegang tot de vloeiende Streaming Client overbrengen Kit. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft-vloeiend Streaming Client Kit definitief productlicentie overdragen

Microsoft biedt onder deze licentie alle benodigde intellectuele eigendomsrechten vloeiende Streaming Client tussentijdse producten ontvangen van andere houders vloeiende Streaming Client overbrengen Kit en bedrijf huisstijl vloeiende Streaming Client definitief om producten te distribueren aan eindgebruikers.

####<a name="fee-structure"></a>Kostenstructuur

Het vloeiende Streaming Client definitieve Product wordt aangeboden onder een model royalty als onder:

- $0,10 per apparaat-implementatie is verzonden
- De royalty is beperkt tot 50.000 elk jaar
- Geen royalty voor eerste 10.000 apparaat implementaties elk jaar 

##<a name="licensing-procedure-and-sspk-access"></a>Volumelicenties Procedure en SSPK toegang

Stuur een e-mail [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) licenties voor alle query's.

De [portal SSPK verdeling](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is toegankelijk voor geregistreerde tussentijdse houders.

Tussentijdse en definitieve SSPK houders technische vragen om te kunnen indienen [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft-vloeiend Streaming Client tussentijdse Product overeenkomst houders

- Adroit Business Solutions, Inc.
- Geavanceerde digitale uitzending SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret W.S.
- Albis Technologies Ltd.
- Alticast Corporation
- Amazon Digital Services, Inc.
- AVC Multimedia Software Co., Ltd.
- Cavium, Inc.
- EchoStar kopen Corporation
- Enseo, Inc.
- Fluendo SA
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto VS Inc.
- Liberty globale Services BV
- MediaTek Inc.
- MStar Co Ltd
- NINTENDO Co., Ltd.
- OpenTV, Inc.
- Saffraan digitale beperkt
- Sichuan Changhong elektrische Co. Ltd
- SoftAtHome
- Sony Corporation
- Tatung Technology Inc.
- TCL Technoly elektronische (Huizhou) Co., Ltd.
- Vestel Elektronik Sanayi ve Ticaret W.S.
- VisualOn, Inc.
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft-vloeiend Streaming Client eindproduct overeenkomst houders

- Geavanceerde digitale uitzending SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret W.S.
- Albis Technologies Ltd.
- Amazon Digital Services, Inc.
- AmTRAN technologie Co., Ltd.
- Arcadyan Technology Corporation
- ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
- Brits Sky uitzenden beperkt
- CastPal Technology Inc., Shenzhen
- Compal elektronische, Inc.
- Dongguan digitale AV Technology Corp., Ltd.
- EchoStar kopen Corporation
- Enseo, Inc.
- Filmflex films beperkt
- Fluendo SA
- Gibson innovaties beperkt
- Haier informatie Applicantion S.R.L
- HANDAN BroadInfoCom Co., Ltd.
- Homecast Co. Ltd
- Hon Hai precisie Industry Co., Ltd.
- Infomir GMBH
- Kaonmedia Co., Ltd.
- KDDI Corporation
- NINTENDO Co., Ltd.
- Oranje SA
- Saffraan digitale beperkt
- Sagemcom breedband SA 's
- Shenzhen Coship elektronische CO. LTD
- Shenzhen Jiuzhou elektrische Co. Ltd
- Shenzhen Skyworth digitale technologie Co. Ltd
- Sichuan Changhong elektrische Co., Ltd.
- Skardin industrieel Corp.
- Sky Deutschland Fernsehen GmbH & Co. KG
- SmarDTV SA
- SoftAtHome
- Sony Corporation
- Beperkt TCL overzeese Marketing (Offshore Macau commercieel)
- Technicolor bezorging technologieën SA 's
- Tongfang globale Ltd.
- Toshiba leven-producten en Services Corporation
- Universele Media Corporation /Slovakia/ s.r.o.
- VIZIO, Inc.
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
