<properties 
    pageTitle="Android webweergave brug met systeemeigen Mobile betrokkenheid Android SDK" 
    description="Wordt beschreven hoe u een brug tussen webweergave met Javascript en de systeemeigen Mobile betrokkenheid Android SDK maken"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Android webweergave brug met systeemeigen Mobile betrokkenheid Android SDK

> [AZURE.SELECTOR]
- [Brug voor android](mobile-engagement-bridge-webview-native-android.md)
- [iOS-brug](mobile-engagement-bridge-webview-native-ios.md)

Sommige mobiele apps zijn ontworpen als een app hybride waar de app zelf is ontwikkeld met behulp van systeemeigen Android ontwikkeling, maar sommige of zelfs alle schermen binnen een Android webweergave worden weergegeven. U kunt nog steeds Mobile betrokkenheid Android SDK gebruiken in deze apps en deze zelfstudie wordt beschreven hoe u gaat hierover. Het onderstaande voorbeeldcode is gebaseerd op de Android documentatie [hier](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Dit wordt beschreven hoe deze beschreven aanpak willen implementeren voor Mobile betrokkenheid Android SDK van veelgebruikte methoden hetzelfde zodat een webweergave via een hybride-app aanvragen voor het bijhouden gebeurtenissen, taken, fouten, app-info ook starten kunt terwijl ze via onze Android SDK pijpleidingplan kan worden gebruikt. 

1. Ten eerste moet u ervoor zorgen dat u onze [aan de slag zelfstudie](mobile-engagement-android-get-started.md) kunt integreren de betrokkenheid Mobile SDK Android in uw app hybride hebt doorlopen. Nadat u dit doen, uw `OnCreate` methode er als volgt te werk.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Nu zorgen dat uw app hybride een scherm met een webweergave op is geïnstalleerd. De code voor deze is vergelijkbaar met het volgende waar we een lokale HTML worden geladen **Sample.html** bestand in de webweergave in de `onCreate` methode van het scherm. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Maak nu een brug bestand met de naam van **WebAppInterface** die Hiermee maakt u een Wikkel voor enkele veelgebruikte Mobile betrokkenheid Android SDK methoden met de `@JavascriptInterface` methode beschreven in de [documentatie voor Android](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Als we het bovenstaande brug-bestand hebt gemaakt, moeten we ervoor zorgen dat gekoppeld aan onze webweergave is. Voor hiervoor moet u bewerken uw `SetWebview` methode zodanig dat deze als volgt te werk eruitziet:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. In het codefragment van de hierboven, we genoemd `addJavascriptInterface` onze klasse brug koppelen aan onze webweergave en ook gemaakt grip **EngagementJs** om te bellen van de methoden uit het bestand brug genoemd. 

6. Nu maken de volgende bestand met de naam van **Sample.html** in uw project in een map met de naam van de **activa** die in de webweergave is geladen en waar we de methoden uit het bestand brug bellen.

        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
        
                <script type="text/javascript">
        
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Houd rekening met de volgende punten over de bovenstaande HTML-bestand:

    -   Bevat een reeks invoervakken waarin u de gegevens die moeten worden gebruikt als namen voor de gebeurtenis, taak, fout, AppInfo kunt geven. Als u op de knop ernaast klikt, wordt een oproep wordt uitgevoerd naar de Javascript die uiteindelijk de methoden uit het bestand brug aanroept voor het doorgeven van deze aanroep van de mobiele betrokkenheid Android SDK. 
    -   We zijn labelen op sommige statische extra info gebeurtenissen, taken en zelfs fouten om te laten zien hoe dit kan gebeuren. Deze extra info als een JSON tekenreeksexpressie die wordt verzonden als u zoeken in de `WebAppInterface` bestand, is geparseerde en plaatst u op een android-apparaat `Bundle` en wordt doorgegeven samen met het verzenden van gebeurtenissen, taken, fouten. 
    -   Een taak van de betrokkenheid Mobile wordt gestart met de naam die u opgeeft in het vak Invoerbereik uitvoeren voor 10 seconden en afgesloten. 
    -   Een mobiele betrokkenheid appinfo of tag wordt doorgegeven met 'customer_name' als de statische sleutel en de waarde die u hebt opgegeven in het vak invoerbereik als de waarde voor de tag. 
 
9. De app uitgevoerd en u ziet het volgende. Nu een naam voor een gebeurtenis test als volgt uit en klik op **verzenden** eronder. 

    ![][1]

10. Als u naar het tabblad **Beeldscherm** van uw app en kijkt u onder **gebeurtenissen-Details >**gaat, ziet u nu deze gebeurtenis samen met de statische app-informatie die we verzendt worden weergegeven. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
