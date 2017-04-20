<properties 
    pageTitle="Filters maken met Azure mediaservices .NET SDK" 
    description="In dit onderwerp wordt beschreven hoe filters maken zodat de klant ze stream bepaalde secties van een stream kan gebruiken. Media Services Hiermee maakt u dynamische manifesten als u wilt bereiken deze selectief streaming." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Filters maken met Azure mediaservices .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)

Media Services kunt vanaf 2.11 versie, u filters definiëren voor uw activa. Deze filters zijn serverregels, zodat uw klanten doen items, zoals kiezen: afspelen alleen een gedeelte van een video (in plaats van de hele video afspelen), of alleen een subset van audio- en -weergaven van uw klant-apparaat kan verwerken (in plaats van alle weergaven die gekoppeld aan de activa zijn) opgeven. Deze filteren van uw activa is tot en met **Dynamische bestandenlijst**s die zijn gemaakt op verzoek van de klant om te streamen van een video op basis van de opgegeven filters bereikt.

Zie voor meer gedetailleerde informatie over filters en dynamische bestandenlijst, [dynamische manifesten overzicht](media-services-dynamic-manifest-overview.md).

Dit onderwerp wordt uitgelegd hoe u met Media Services .NET SDK maken, bijwerken en verwijderen van filters. 


Opmerking dat als u een filter bijwerkt, het kan maximaal 2 minuten duren voordat streaming eindpunt als u wilt vernieuwen van de regels. Als de inhoud is aangeboden door middel van dit filter (en met cache in proxy's en CDN cache), kan het bijwerken van dit filter resulteren in speler fouten. Is het aanbevolen om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter. 

##<a name="types-used-to-create-filters"></a>Typen gebruikt om te maken van filters

De volgende typen worden gebruikt bij het maken van filters: 

- **IStreamingFilter**.  Dit type is gebaseerd op de volgende REST API- [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Dit type is gebaseerd op de volgende REST API [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Dit type is gebaseerd op de volgende REST API [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** en **IFilterTrackPropertyCondition**. Dit soort zijn gebaseerd op de volgende REST API's [FilterTrackSelect en FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Algemene filters maken/bijwerken/gelezen/verwijderen

De volgende code ziet hoe u met .NET maken, bijwerken, lezen en verwijderen van activa filters.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Filters maken/bijwerken/gelezen/verwijderen van activa

De volgende code ziet hoe u met .NET maken, bijwerken, lezen en verwijderen van activa filters.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Bouwen streaming van URL's die filters gebruiken

Voor informatie over het publiceren en geven van de activa, Zie [Inhoud leveren aan klanten overzicht](media-services-deliver-content-overview.md).


De volgende voorbeelden wordt het filters toevoegen aan de URL van uw streaming.

**MPEG-STREEPJE** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Vloeiende Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**SCHIJVEN**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zie ook 

[Overzicht van de dynamische manifesten](media-services-dynamic-manifest-overview.md)
 

