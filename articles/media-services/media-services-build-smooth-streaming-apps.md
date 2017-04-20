<properties 
    pageTitle="Windows Store-App zelfstudie Streaming vloeiend | Microsoft Azure" 
    description="Informatie over het gebruik van Azure Media Services een C# Windows Store-servicetoepassing maken met een besturingselement voor een XML-MediaElement naar afspelen vloeiende stroom inhoud." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Het maken van een vloeiende Streaming Windows Store-toepassing

De vloeiende Streaming Client SDK voor Windows 8 kunt ontwikkelaars op Windows Store-toepassingen waarin op aanvraag en live-vloeiende Streaming inhoud kunnen worden afgespeeld. Naast het eenvoudige afspelen van vloeiende Streaming inhoud, dat de SDK ook biedt uitgebreide functies, zoals Microsoft PlayReady beveiliging, niveau beperking, Live DVR audiokwaliteit streamen veranderen, luisteren naar statusupdates (zoals kwaliteit verandert) en fouten, enzovoort. Zie de [releaseopmerkingen](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes)voor meer informatie van de ondersteunde functies. Zie voor meer informatie [speler Framework voor Windows 8](http://playerframework.codeplex.com/). 

Deze zelfstudie bevat vier lessen:

1. Een eenvoudige vloeiende Streaming Store-servicetoepassing maken
2. Een schuifbalk om te bepalen de voortgang van de Media toevoegen
3. Selecteer vloeiende Streaming-Streams
4. Selecteer vloeiende Streaming nummers

##<a name="prerequisites"></a>Vereisten voor

- Windows 8 32-bits of 64-bits. U kunt [Windows 8 Enterprise evaluatie](http://msdn.microsoft.com/evalcenter/jj554510.aspx) krijgen van MSDN.
- Visual Studio 2012 of Visual Studio Express 2012 (of een latere versie). U kunt de evaluatieversie krijgen van [hier](http://www.microsoft.com/visualstudio/11/downloads).
- [Microsoft vloeiend Streaming Client SDK voor Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


De voltooide oplossing voor elke les kan worden gedownload van MSDN Developer Code Samples (Code galerie): 

- [Les 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - een eenvoudige Windows 8-vloeiend Streaming MediaPlayer 
- [Les 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - een eenvoudige Windows 8 vloeiende Streaming MediaPlayer met een schuifbalk bepalen, 
- [Les 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - een Windows 8-vloeiend Streaming MediaPlayer met Stream selectie,  
- [Les 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - een Windows 8-vloeiend Streaming MediaPlayer met bijhouden selectie.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Les 1: Een eenvoudige vloeiende Streaming Store-servicetoepassing maken

In deze les maakt u een Windows Store-servicetoepassing aan een besturingselement MediaElement om af te spelen vloeiende Stream inhoud.  De actieve toepassing ziet er zo:

![Voorbeeld van vloeiend Streaming Windows Store-toepassing][PlayerApplication]
 
Zie voor meer informatie over het ontwikkelen van de Windows Store-toepassing, [ontwikkelen geweldige Apps voor Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). In deze les bevat de volgende procedures uit:

1.  Een Windows Store-project maken
2.  Ontwerpen van de gebruikersinterface (XAML)
3.  Wijzig de code achter bestand
4.  Compileren en de toepassing te testen

**Een Windows Store-project maken**

1.  Voer Visual Studio 2012 of hoger.
2.  Klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.
3.  In het dialoogvenster Nieuw Project typt of selecteert u de volgende waarden:

Naam|Waarde
---|---
Sjabloongroep|Geïnstalleerd/sjablonen/Visual C# / Windows Store
Sjabloon|Lege App (XAML)
Naam|SSPlayer
Locatie|C:\SSTutorials
De oplossingsnaam van de|SSPlayer
Map voor oplossing maken|(geselecteerd)

4.  Klik op **OK**.

**Een verwijzing naar de vloeiende Streaming Client SDK toevoegen**

1.  Met de rechtermuisknop op **SSPlayer**uit Solution Explorer en klik op **Add Reference**.
2.  Typ of Selecteer de volgende waarden:

Naam|Waarde
---|---
Verwijzingsgroep|/ Uitbreidingen voor Windows
Verwijzing|Selecteer Microsoft vloeiend Streaming Client SDK voor Windows 8 en pakket van Microsoft Visual C++ Runtime
    
3.  Klik op **OK**. 

Na het toevoegen van de verwijzingen, moet u het gerichte platform (x64 of x86), toe te voegen verwijzingen werkt niet voor elk CPU platformconfiguratie.  U ziet in solution explorer gele waarschuwing markeren om deze referenties toegevoegd.

**Bij het ontwerpen van de speler-gebruikersinterface**

1.  In Solution Explorer, dubbelklikt u op **MainPage.xaml** om deze te openen in de ontwerpweergave.
2.  Zoek de ** &lt;raster&gt; ** en ** &lt;/Grid&gt; ** het bestand XAML-labels en plak de volgende code tussen de twee codes:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Het besturingselement MediaElement wordt gebruikt voor het afspelen van media. De schuifregelaar met de naam sliderProgress wordt gebruikt in de volgende les om te bepalen de voortgang van de media.

3.  Druk op **CTRL + S** het bestand wilt opslaan.

Het besturingselement MediaElement biedt geen ondersteuning voor vloeiende Streaming inhoud kant-en-klare. Als u wilt de vloeiende Streaming-ondersteuning inschakelt, moet u de vloeiende Streaming-byte-stream handler door bestandsextensie en MIME-type registreren.  Als u wilt registreren, moet u de methode MediaExtensionManager.RegisterByteStremHandler van de naamruimte Windows.Media gebruiken.

In dit bestand XAML zijn sommige gebeurtenisafhandelingen gekoppeld aan de besturingselementen.  U moet deze gebeurtenishandlers definiëren.

**Voor het wijzigen van de code achter bestand**

1.  Met de rechtermuisknop op **MainPage.xaml**uit Solution Explorer en klik op **Code weergeven**.
2.  Voeg het volgende toe aan de bovenkant van het bestand, met instructie:

        using Windows.Media;

3.  Toevoegen aan het begin van de klas **MainPage** , de leden van de volgende gegevens:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Toevoegen aan het einde van de constructor **MainPage** , de volgende twee regels:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Aan het einde van de klasse **MainPage** , plak de volgende code:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Hier definieert u de gebeurtenis-handler sliderProgress_PointerPressed.  Er zijn meer works kunt doen om te werken werkt, waarvoor u in de volgende les van deze zelfstudie.
6.  Druk op **CTRL + S** het bestand wilt opslaan.

Het voltooide de code achter bestand wordt er zo uit:

![CodeView in Visual Studio van vloeiende Streaming Windows Store-toepassing][CodeViewPic]

**Compileren en test u de toepassing**

1.  Klik op het menu **BUILD** en op **Configuration Manager**.
2.  **Actieve oplossing platform** zodat deze overeenkomt met uw platform development wijzigen.
3.  Druk op **F6** om samen te stellen van het project. 
4.  Druk op **F5** om de toepassing te starten.
5.  Aan de bovenkant van de toepassing, kunt u gebruik van de standaard-URL voor vloeiende Streaming of voer een andere naam. 
6.  Klik op **bron instellen**. Omdat **Automatisch afspelen** is standaard ingeschakeld, wordt automatisch de media afspelen.  De media met het **afspelen**, de knoppen **onderbreken** en **stoppen** , kunt u bepalen.  U kunt het Mediavolume van het met de verticale schuifbalk bepalen.  Echter de horizontale schuifbalk voor het beheren van de voortgang van de media is niet volledig nog geïmplementeerd. 

U kunt lesson1 hebt voltooid.  In deze les gebruikt u een besturingselement MediaElement kunt afspelen vloeiende Streaming inhoud.  In de volgende les voegt u een schuifregelaar om de voortgang van de vloeiende Streaming inhoud te bepalen.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Les 2: Een schuifbalk om te bepalen de voortgang van de Media toevoegen
In les 1, kunt u een Windows Store-servicetoepassing gemaakt met een besturingselement MediaElement XAML kunt afspelen vloeiende Streaming media-inhoud.  Sommige functies eenvoudige media zoals starten, stoppen en onderbreken wordt geleverd.  In deze les wordt u een besturingselement schuifbalk werkbalk toevoegen aan de toepassing.

In deze zelfstudie gebruiken we een timer de schuifregelaar positie op basis van de huidige positie van het besturingselement MediaElement bijwerken.  De schuifregelaar begin- en afstemmen ook moeten worden bijgewerkt voor het geval live-inhoud.  Dit kan beter worden verwerkt in de geavanceerde bron update gebeurtenis.

Media-bronnen zijn objecten die mediagegevens genereren.  De bron-resolvercache een URL-byte stroom neemt en maakt de juiste mediabron voor die inhoud.  De bronoplossing is de standaardmethode voor de toepassingen mediabronnen maken. 

In deze les bevat de volgende procedures uit:

1.  De vloeiende Streaming-handler registreren 
2.  De gebeurtenishandlers geavanceerde bron manager niveau toevoegen
3.  Het niveau gebeurtenishandlers van geavanceerde bron toevoegen
4.  MediaElement gebeurtenishandlers toevoegen
5.  Schuifregelaar gerelateerde streepjescode toevoegen
6.  Compileren en de toepassing te testen

**De byte-stream vloeiende Streaming handler registreren en doorgeven van de propertyset**

1.  Uit Solution Explorer **MainPage.xaml**met de rechtermuisknop op en klik vervolgens op **Programmacode weergeven**.
2.  Voeg het volgende toe aan het begin van het bestand, met instructie:

        using Microsoft.Media.AdaptiveStreaming;

3.  Toevoegen aan het begin van de klas MainPage, de gegevensleden van de volgende:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Binnen de constructor **MainPage** toevoegen de volgende code na de **. Geïnitialiseerd Components();** regel- en de registratie-code geschreven in de vorige les lijnen:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Wijzig de twee RegisterByteStreamHandler methoden om toe te voegen binnen de constructor **MainPage** het vierde parameters:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Druk op **CTRL + S** het bestand wilt opslaan.

**De geavanceerde bron manager niveau gebeurtenis-handler toevoegen**

1.  Uit Solution Explorer **MainPage.xaml**met de rechtermuisknop op en klik vervolgens op **Programmacode weergeven**.
2.  In de klas **MainPage** toevoegen de gegevenslid van de volgende:

        private AdaptiveSource adaptiveSource = null;

3.  Toevoegen aan het einde van de klas **MainPage** , de volgende gebeurtenis-handler:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Toevoegen aan het einde van de constructor **MainPage** , de volgende regel zich kunnen abonneren op de gebeurtenis open van geavanceerde bron:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += nieuwe AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Druk op **CTRL + S** het bestand wilt opslaan.

**Het niveau gebeurtenishandlers geavanceerde bron toevoegen**

1.  Uit Solution Explorer **MainPage.xaml**met de rechtermuisknop op en klik vervolgens op **Programmacode weergeven**.
2.  In de klas **MainPage** toevoegen de gegevenslid van de volgende:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Toevoegen aan het einde van de klas **MainPage** , de volgende gebeurtenishandlers:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Toevoegen aan het einde van de methode **mediaElement AdaptiveSourceOpened** , de volgende code voor een abonnement op de gebeurtenissen:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Druk op **CTRL + S** het bestand wilt opslaan.

De dezelfde gebeurtenissen zijn beschikbaar op geavanceerde Manager niveau gegevensbron, ook die kan worden gebruikt voor het verwerken van de functionaliteit voor alle media-elementen in de app. Elke AdaptiveSource bevat eigen gebeurtenissen en alle AdaptiveSource gebeurtenissen worden trapsgewijs onder AdaptiveSourceManager.

**Media-Element gebeurtenishandlers toevoegen**

1.  Uit Solution Explorer **MainPage.xaml**met de rechtermuisknop op en klik vervolgens op **Programmacode weergeven**.
2.  Toevoegen aan het einde van de klas **MainPage** , de volgende gebeurtenishandlers:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Voeg de volgende code op subscript op de gebeurtenissen die aan het einde van de constructor **MainPage** :
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Druk op **CTRL + S** het bestand wilt opslaan.

**Om toe te voegen schuifbalk gerelateerd code**

1.  Uit Solution Explorer **MainPage.xaml**met de rechtermuisknop op en klik vervolgens op **Programmacode weergeven**.
2.  Voeg het volgende toe aan het begin van het bestand, met instructie:
    
        using Windows.UI.Core;

3.  In de klas **MainPage** toevoegen de gegevensleden van de volgende:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Toevoegen aan het einde van de constructor **MainPage** , de volgende code:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Toevoegen aan het einde van de klas **MainPage** , de volgende code:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Notitie:** CoreDispatcher wordt gebruikt om wijzigingen aanbrengen in de UI-thread uit niet UI-Thread. Voor het geval knelpunt op verzender thread kunt ontwikkelaars gebruiken verzender verstrekt door UI-element hij wil bijwerken.  Bijvoorbeeld:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Toevoegen aan het einde van de methode **mediaElement_AdaptiveSourceStatusUpdated** , de volgende code:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Toevoegen aan het einde van de methode **MediaOpened** , de volgende code:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Druk op **CTRL + S** het bestand wilt opslaan.

**Compileren en test u de toepassing**

1. Druk op **F6** om samen te stellen van het project. 
2.  Druk op **F5** om de toepassing te starten.
3.  Aan de bovenkant van de toepassing, kunt u gebruik van de standaard-URL voor vloeiende Streaming of voer een andere naam. 
4.  Klik op **bron instellen**. 
5.  Test de schuifbalk.

U hebt voltooid les 2.  In deze les kunt u een schuifregelaar toegevoegd aan de toepassing. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Les 3: Selecteer vloeiende Streaming-Streams
Vloeiende Streaming is in staat om de inhoud van de stream met meerdere taal audio nummers die kunnen geselecteerd door de viewers worden.  In deze les wordt u toeschouwers streams selecteren. In deze les bevat de volgende procedures uit:

1. De XAML-bestand wijzigen
2. De code behand-bestand wijzigen
3. Compileren en de toepassing te testen


**De XAML-bestand wijzigen**

1. Met de rechtermuisknop op **MainPage.xaml**uit Solution Explorer en klik op **Ontwerp weergeven**.
2. Zoek &lt;Grid.RowDefinitions&gt;, en de RowDefinitions wijzigen zodat ze eruit:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Binnen de &lt;raster&gt;&lt;/Grid&gt; tags toevoegen de volgende code definiëren van een gewone keuzelijst, zodat gebruikers kunnen zien van de lijst met beschikbare streams en selecteer streams:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Druk op **CTRL + S** de wijzigingen wilt opslaan.


**Voor het wijzigen van de code achter bestand**

1. Met de rechtermuisknop op **MainPage.xaml**uit Solution Explorer en klik op **Code weergeven**.
2. Een nieuwe klasse toevoegen in de naamruimte SSPlayer: #region class Stream
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Toevoegen aan het begin van de klas MainPage, de volgende variabele definities:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. In de klas MainPage toevoegen de volgende regio:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Zoek de methode mediaElement_ManifestReady, de volgende code aan het einde van de functie toevoegen:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Zodat wanneer MediaElement manifest klaar is, wordt de code een lijst met beschikbare stromen ontvangt en vult de keuzelijst UI met de lijst.

6. Zoek de gebruikersinterface in de klas MainPage knoppen op gebeurtenissen regio en vervolgens de volgende functiedefinitie toevoegen:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Compileren en test u de toepassing**

1. Druk op **F6** om samen te stellen van het project. 
2.  Druk op **F5** om de toepassing te starten.
3.  Aan de bovenkant van de toepassing, kunt u gebruik van de standaard-URL voor vloeiende Streaming of voer een andere naam. 
4.  Klik op **bron instellen**. 
5.  De standaardtaal is audio_eng. Probeer te schakelen tussen audio_eng en audio_es. Telkens wanneer, selecteert u een nieuwe stream, moet u deze op de knop verzenden.

U kunt les 3 hebt voltooid.  In deze les voegt u de functionaliteit om streams te selecteren.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Les 4: Selecteer vloeiende Streaming nummers
Een presentatie vloeiende Streaming kan meerdere videobestanden gecodeerd met verschillende kwaliteitsniveaus (tarieven bits) en oplossingen bevatten. In deze les kunt u gebruikers nummers selecteren. In deze les bevat de volgende procedures uit:

1. De XAML-bestand wijzigen
2. De code behand-bestand wijzigen
3. Compileren en de toepassing te testen

**De XAML-bestand wijzigen**

1. Met de rechtermuisknop op **MainPage.xaml**uit Solution Explorer en klik op **Ontwerp weergeven**.
2. Zoek de &lt;raster&gt; markeren met de naam **gridStreamAndBitrateSelection**en de volgende code aan het einde van de tag toevoegen:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Druk op **CTRL + S** he wijzigingen op te slaan


**Voor het wijzigen van de code achter bestand**

1. Met de rechtermuisknop op **MainPage.xaml**uit Solution Explorer en klik op **Code weergeven**.
2. In de naamruimte SSPlayer, moet u een nieuwe klasse toevoegen:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Toevoegen aan het begin van de klas MainPage, de volgende variabele definities:
    
        private List<Track> availableTracks;

4. In de klas MainPage toevoegen de volgende regio:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Zoek de methode mediaElement_ManifestReady, de volgende code aan het einde van de functie toevoegen:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Zoek de gebruikersinterface in de klas MainPage knoppen op gebeurtenissen regio en vervolgens de volgende functiedefinitie toevoegen:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Compileren en test u de toepassing**

1. Druk op **F6** om samen te stellen van het project. 
2.  Druk op **F5** om de toepassing te starten.
3.  Aan de bovenkant van de toepassing, kunt u gebruik van de standaard-URL voor vloeiende Streaming of voer een andere naam. 
4.  Klik op **bron instellen**. 
5.  Standaard zijn alle de nummers van de video stream geselecteerd. Als u wilt experimenteren het bits tarief wordt gewijzigd, kunt u het laagste bits tarief beschikbaar selecteren en selecteer de snelheid van de beschikbare hoogste bits. U moet deze verzenden op na elke wijziging.  Hier ziet u de wijzigingen videokwaliteit.

U kunt les 4 hebt voltooid.  In deze les kunt u de functionaliteit als u nummers wilt toevoegen.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Overige informatiebronnen:
- [Het maken van een toepassing voor de vloeiende Streaming Windows 8 JavaScript met geavanceerde functies](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Vloeiende Streaming technisch overzicht](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
