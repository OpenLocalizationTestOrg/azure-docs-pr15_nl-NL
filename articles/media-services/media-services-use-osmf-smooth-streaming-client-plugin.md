<properties 
    pageTitle="Vloeiende Streaming invoegtoepassing voor het kader van de Media bron openen" 
    description="Informatie over het gebruik van de Azure Media Services vloeiende Streaming-invoegtoepassing voor Adobe openen bron Media Framework." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Het gebruik van de Microsoft-vloeiend Plug Streaming voor Adobe bron openen Media Framework

##<a name="overview"></a>Overzicht


De invoegtoepassing voor Microsoft vloeiende Streaming voor geopende bron Media Framework 2.0 (SS voor OSMF) breidt de mogelijkheden van de standaard van OSMF en Microsoft vloeiende Streaming inhoud af te spelen toegevoegd aan nieuwe en bestaande OSMF spelers. Mogelijkheden voor afspelen vloeiende Streaming de invoegtoepassing ook toegevoegd aan stroboscoop Media afspelen (SMP).

SS voor OSMF bevat twee versies van de invoegtoepassing:

- Statische vloeiende Streaming-invoegtoepassing voor OSMF (SWC)

- Dynamische vloeiende Streaming-invoegtoepassing voor OSMF (.swf)

In dit document wordt ervan uitgegaan dat de lezer een algemene werkervaring met OSMF en OSMF heeft plug-ins. Voor meer informatie over OSMF, raadpleegt u de documentatie op de [officiële OSMF-site](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Vloeiende Streaming-invoegtoepassing voor OSMF 2.0

De invoegtoepassing laden en afspelen van vloeiende Streaming inhoud op aanvraag met de volgende functies worden ondersteund:

- Afspelen op aanvraag vloeiende Streaming (afspelen, pauze, Seek, stoppen)
- Live vloeiende Streaming afspelen (afspelen)
- Live DVR-functies (onderbreken, Seek, DVR afspelen, Ga to Live)
- Ondersteuning voor videocodecs - H.264
- Ondersteuning voor audiocodecs - AAC
- Meerdere audio talen schakelen met ingebouwde OSMF-API 's
- Max afspelen kwaliteit selectie met ingebouwde OSMF-API 's
- Ter gesloten bijschriften met de invoegtoepassing voor OSMF bijschriften
- Adobe&reg; Flash&reg; speler 11,4 of hoger.
- Deze versie ondersteunt alleen OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Ondersteunde functies en bekende problemen

Voor een volledige lijst met ondersteunde functies, niet-ondersteunde functies en bekende problemen, raadpleegt u [Dit document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>De invoegtoepassing laden
OSMF-Plug-ins kan worden geladen statisch (bij compilatie) of dynamisch (tijdens runtime). De invoegtoepassing voor vloeiende Streaming OSMF downloaden bevat dynamische en statische versies.

- Statische laden: als u wilt statisch hebt geladen, een statische bibliotheekbestand (SWC) is vereist. Statische Plug-ins worden toegevoegd als een verwijzing naar de projecten en samenvoegen in het definitieve uitvoerbestand bij het compileren.

- Dynamisch laden: als u wilt dynamisch hebt geladen, een vooraf samengestelde (SWF)-bestand is vereist. Dynamische Plug-ins zijn geladen in de runtime en niet opgenomen in de projectuitvoer. (Gecompileerd uitvoer) Dynamische Plug-ins kan worden geladen met HTTP- en bestand protocollen.

Zie voor meer informatie over statische en dynamische laden, de officiële [OSMF Plug-pagina](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS voor OSMF statische laden
Het codefragment hieronder ziet hoe u de invoegtoepassing voor SS statisch voor OSMF laden en afspelen van een eenvoudige video OSMF MediaFactory class. Voordat u de SS voor OSMF code waaronder, zorg ervoor dat de projectverwijzing de statische "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc"-invoegtoepassing bevat.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS voor OSMF dynamisch laden

Het codefragment hieronder ziet hoe u de invoegtoepassing voor SS dynamisch voor OSMF laden en afspelen van een basic video met de klasse OSMF MediaFactory. Voordat u waaronder de SS voor OSMF-code, de "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf"-invoegtoepassing voor dynamische kopiëren naar de projectmap als u wilt laden met bestandsprotocol of kopieer onder een webserver voor HTTP laden. Er is niet nodig om 'MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc' in de projectverwijzingen.

 
{pakket
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Stroboscoop Media afspelen met de dynamische SS ODMF-invoegtoepassing

De vloeiende Streaming voor dynamische OSMF-invoegtoepassing is compatibel met [Stroboscoop Media afspelen (SMP)](http://osmf.org/strobe_mediaplayback.html). U kunt de SS voor OSMF Plug-in vloeiende Streaming inhoud af te spelen toevoegen aan SMP. Kopieer hiervoor "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" onder een webserver voor HTTP laden met de volgende stappen:

1.  Blader op de [pagina voor het opzetten van stroboscoop Media afspelen](http://osmf.org/dev/2.0gm/setup.html). 
2.  Stel de src naar een bron vloeiende Streaming, (bijvoorbeeld http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Breng de gewenste configuratiewijzigingen aan en klik op voorbeeld en bijwerken.
 
    **Opmerking** Uw inhoud webserver moet een geldige crossdomain.xml. 
4.  Kopieer en plak de code aan een eenvoudige HTML-pagina met uw favoriete teksteditor, zoals in het volgende voorbeeld:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Vloeiende Streaming OSMF Plug-in toevoegen aan de ingesloten code en opslaan.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Uw HTML-pagina opslaan en publiceren naar een webserver. Blader naar de gepubliceerde pagina met webonderdelen met uw favoriete Flash&reg; speler ingeschakeld internetbrowser (Internet Explorer, Chrome, Firefox, enzovoort).
7.  Te rekenen op vloeiende Streaming inhoud binnen Adobe&reg; Flash&reg; Player.

Voor meer informatie over algemene OSMF ontwikkeling, raadpleegt u de officiële [OSMF ontwikkeling pagina](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Zie ook

[Microsoft geavanceerde Plug Streaming voor OSMF Update](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
