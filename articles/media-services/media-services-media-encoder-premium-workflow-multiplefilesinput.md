<properties
    pageTitle="Gebruik van meerdere invoer bestanden en onderdeeleigenschappen met Premium Encoder | Microsoft Azure"
    description="In dit onderwerp wordt uitgelegd hoe u setRuntimeProperties werken met meerdere invoer bestanden en aangepaste gegevens aan de Media Encoder Premium media werkstroomverwerking doorgeven."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Gebruik van meerdere invoer bestanden en onderdeeleigenschappen met Premium Encoder

## <a name="overview"></a>Overzicht

Er zijn scenario's waarin u mogelijk nodig hebt om aan te passen onderdeeleigenschappen opgeven Clip lijst XML-inhoud of meerdere invoer bestanden verzenden wanneer u een taak met de **Media Encoder Premium** media werkstroomverwerking indient. Er zijn enkele voorbeelden:

- Tekst op video en de tekenreeks (bijvoorbeeld de huidige datum) instellen tijdens runtime voor elke invoer video bedekken.
- Het aanpassen van de Clip lijst XML (als u wilt opgeven een of meer bronbestanden, met of zonder het inkorten van het, enz.).
- Een logoafbeelding bedekken op de video invoer, terwijl de video gecodeerd.

Als u wilt dat de **Media Encoder Premium werkstroom** weten dat u bepaalde eigenschappen in de werkstroom wilt wijzigen wanneer u de taak maakt of meerdere invoer bestanden verzenden, moet u het gebruik van een configuratietekenreeks bevat die **setRuntimeProperties** en/of **transcodeSource**. In dit onderwerp wordt uitgelegd hoe u ze kunt gebruiken.


## <a name="configuration-string-syntax"></a>De syntaxis van de configuratie-tekenreeks

De configuratietekenreeks om in te stellen in de codering taak gebruikt een XML-document dat er zo uitziet:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Hieronder ziet u de C#-code die leest de XML-configuratie uit een bestand en wordt doorgegeven aan de taak in een taak.

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Componenteigenschappen van de aanpassen  

### <a name="property-with-a-simple-value"></a>Eigenschap met een eenvoudige waarde
In sommige gevallen is het handig om aan te passen van de eigenschap van een component samen met het bestand werkstroom die u wilt worden uitgevoerd door de Media Encoder Premium werkstroom.

Stel dat u ontwerpt een werkstroom die overlays tekst op uw video's en de tekst (bijvoorbeeld de huidige datum) moet tijdens runtime worden ingesteld. U kunt dit doen door te sturen van de tekst van de taak codering als de nieuwe waarde voor de eigenschap text van de overlay-component worden ingesteld. U kunt deze methode gebruiken om andere eigenschappen van een onderdeel in de werkstroom (zoals de positie of kleur van de overlay, de bitsnelheid AVC encoder, enz.) te wijzigen.

**setRuntimeProperties** wordt gebruikt om een eigenschap in de onderdelen van de werkstroom te overschrijven.

Voorbeeld:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Eigenschap met een XML-waarde

Een eigenschap die een XML-waarde verwacht wilt instellen, onderbrengen met behulp van `<![CDATA[ and ]]>`.

Voorbeeld:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Zorg ervoor dat niet moeten worden geplaatst vlak na de afzender van een regeleinden `<![CDATA[`.


### <a name="propertypath-value"></a>propertyPath waarde

In de voorgaande voorbeelden, de propertyPath is "/ mediabestanden invoer/filename" of "/ inactiveTimeout" of "clipListXml".
Dit kan in het algemeen, is de naam van het onderdeel en vervolgens de naam van de eigenschap. Het pad kan hebben meer of minder niveaus, zoals "/ primarySourceFile" (omdat de eigenschap in de hoofdmap van de werkstroom) of "/ Video verwerking/grafische Overlay/dekking" (omdat de Overlay in een groep).    

Als u wilt controleren of de naam van het pad en de eigenschap, gebruikt u de actieknop die direct naast elke eigenschap. U kunt Klik op deze actieknop en selecteer **bewerken**. Hier ziet u de feitelijke naam van de eigenschap en direct erboven, de naamruimte.

![Actie/Edit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Eigenschap](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Meerdere bestanden van de invoer

Elke taak die u bij de **Media Encoder Premium werkstroom** indienen vereist twee activa:

- De eerste fase is een *Werkstroom activa* met een werkstroom voor het bestand. U kunt Werkstroombestanden ontwerpen met behulp van de [Werkstroomontwerper](media-services-workflow-designer.md).
- Het tweede is een *Media-activa* waarin de media-bestanden die u wilt coderen.

Wanneer u meerdere media-bestanden naar de encoder **Media Encoder Premium werkstroom** verzendt, wordt de volgende beperkingen gelden:

- De mediabestanden moeten zich in dezelfde *Media-activa*. Gebruik van meerdere Media-activa wordt niet ondersteund.
- U moet het primaire bestand instellen in deze Media-activa (in het ideale geval dit is de belangrijkste videobestand die de encoder wordt gevraagd om te verwerken).
- Het is nodig om de configuratiegegevens met de **setRuntimeProperties** en/of **transcodeSource** element dat moet worden de processor te geven.
  - **setRuntimeProperties** wordt gebruikt voor het opheffen van de eigenschap filename of nog een eigenschap in de onderdelen van de werkstroom.
  - **transcodeSource** wordt gebruikt om op te geven van de Clip lijst XML-inhoud.

Verbindingen in de werkstroom:

 - Als u een of meerdere onderdelen van de invoer van Media-bestand en plan **setRuntimeProperties** gebruiken om op te geven van de bestandsnaam gebruikt, klikt u vervolgens sluit niet de primaire bestand onderdeel pincode hierop toe. Zorg ervoor dat er geen verbinding tussen het object primaire bestand en het Media-bestand Input(s).
 - Als u liever de Clip lijst XML- en één Media Source component, kunt klikt u vervolgens u beide elkaar verbinden.

![Geen verbinding tussen primaire bronbestand en Media bestandsinvoer](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Er is geen verbinding uit het primaire bestand met de onderdelen van de invoer van Media-bestand als u setRuntimeProperties gebruikt voor het instellen van de eigenschap filename.*

![Verbinding vanuit Knipsellijst XML in de lijst bron illustraties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*U kunt XML-Clip lijst verbinden met Media-bron en transcodeSource gebruiken.*


### <a name="clip-list-xml-customization"></a>Illustraties van XML-lijst aanpassen
In de werkstroom gedurende runtime kunt u de Clip lijst XML opgeven met behulp van **transcodeSource** in de configuratietekenreeks XML. U moet hiervoor de Clip lijst XML-vastmaken aan het niet worden verbonden met het onderdeel bron van de Media in de werkstroom.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Als u wilt opgeven /primarySourceFile als u wilt deze eigenschap gebruiken om de uitvoerbestanden met behulp van 'Expressies', klikt u vervolgens raadzaam de Clip lijst XML als een eigenschap *nadat* de eigenschap /primarySourceFile, om te voorkomen dat de Knipsellijst worden overschreven door de instelling /primarySourceFile doorgeven.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Met extra frame nauwkeurig opmaak:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Voorbeeld

Bekijk een voorbeeld waarin u een logoafbeelding op de video invoer overlay wilt terwijl de video gecodeerd. In dit voorbeeld de invoer video heet 'MyInputVideo.mp4' en het logo heet 'MyLogo.png'. U moet de volgende stappen uitvoeren:

- Maak een werkstroom activa met de werkstroom-bestand (Zie het volgende voorbeeld).
- Maak een Media-activa, waarin twee bestanden: MyInputVideo.mp4 als de primaire bestands- en MyLogo.png.
- Een taak verzenden naar de Media Encoder Premium media werkstroomverwerking met de bovenstaande invoer activa en geef de volgende configuratietekenreeks.

Configuratie:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


In het bovenstaande voorbeeld wordt de naam van het videobestand verzonden naar het onderdeel Media bestandsinvoer en de eigenschap primarySourceFile. De naam van het logobestand wordt verzonden naar een andere invoer van de Media bestand die is gekoppeld aan de afbeelding overlay-component.

>[AZURE.NOTE]De naam van het videobestand wordt verzonden naar de eigenschap primarySourceFile. De reden hiervoor is gebruikt u deze eigenschap in de werkstroom voor het samenstellen van de naam van het juiste uitvoerbestand expressies, bijvoorbeeld gebruiken.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Stapsgewijze werkstroom maken die als overlay voor een logo boven aan de video     

Hier volgen de stappen voor het maken van een werkstroom die twee bestanden als invoer duurt: een video en een afbeelding. Dit wordt de afbeelding boven aan de video overlay.

Open **Werkstroomontwerper** en selecteer **bestand** > **Nieuwe werkruimte** > **Transcoderen blauwdruk**.

De nieuwe werkstroom ziet u drie elementen:

- Primaire bronbestand
- Knipsellijst XML
- Uitvoer bestand/activa  

![Nieuwe werkstroom voor het coderen](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nieuwe werkstroom voor het coderen*


Om te kunnen het mediabestand invoer accepteert, begint u met Media-bestand invoer onderdelen toevoegen. Als u wilt een onderdeel toevoegen aan de werkstroom, zoekt u hiernaar in het zoekvak van de bibliotheek en sleep de gewenste vermelding in het deelvenster ontwerpfunctie.

Voeg nu het videobestand moet worden gebruikt voor het ontwerpen van de werkstroom. Klik in het deelvenster achtergrond in werkstroomontwerper hiertoe en zoek de eigenschap primaire bronbestand in het deelvenster rechts eigenschap. Klik op het mappictogram en selecteer het juiste videobestand.

![Primaire bestandsbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primaire bestandsbron*


Geef het videobestand vervolgens in het onderdeel Media bestandsinvoer.   

![Mediabestanden invoerbron](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Mediabestanden invoerbron*


Als dit klaar is, wordt het onderdeel van de invoer van Media-bestand controleren van het bestand en vullen de uitvoer pincodes zodat het bestand dat deze gecontroleerd.

De volgende stap is een 'Video gegevens Type bijwerken"om op te geven van de kleurruimte Rec.709 toevoegen. Een "Video conversieprogramma van' die is ingesteld op gegevensindeling indeling type toevoegen = configureerbare vlakke. Hiermee wordt de video-stream converteren naar een indeling die kan worden uitgevoerd als een bron van de overlay-component.

![video-gegevens Type bijwerken en opmaak conversieprogramma](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videogegevens Type bijwerken en opmaak conversieprogramma*

![Indelingstype = configureerbare vlakke](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Indelingstype is configureerbare vlakke*

Vervolgens een Video Overlay-onderdeel toevoegen en de (niet-gecomprimeerde) video pincode verbinden met de (niet-gecomprimeerde) video pincode van de invoer van de media-bestand.

Toevoegen van een andere invoer door Media-bestand (als u wilt laden het logobestand), klikt u op dit onderdeel en wijzig de naam 'Media bestand invoer logo' en selecteer een afbeelding (bijvoorbeeld een .png-bestand) in de bestandseigenschap. De pincode niet-gecomprimeerde afbeelding verbinden met de pincode niet-gecomprimeerde afbeelding van de overlay.

![Bron overlay-onderdeel en afbeelding](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Bron overlay-onderdeel en afbeelding*


Als u wilt wijzigen van de positie van het logo op de video (bijvoorbeeld u mogelijk wilt plaatst u deze aan 10 procent mensen uit de linkerbovenhoek van de video), schakel het selectievakje "Handmatige invoer". Omdat u de invoer van een Media-bestand op te geven van het logobestand met de overlay-component gebruikt, kunt u dit doen.

![Overlay positie](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Overlay positie*


Toevoegen om te coderen van de video-stream naar H.264, de onderdelen van de encoder AVC Video Encoder en AAC aan de ontwerpfunctie oppervlakte. Verbinding met het maken van de pennen.
De encoder AAC instellen en selecteer Audio opmaken conversie/definitie: 2.0 (L, R).

![Audio- en videogesprekken Encoders](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio- en videogesprekken Encoders*


Nu de **ISO Mpeg-4 Multiplexer** en **Bestandsuitvoer** onderdelen toevoegen en verbindt u de pennen zoals wordt weergegeven.

![MP4 multiplexer en bestandsuitvoer](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexer en bestandsuitvoer*


U moet de naam voor het uitvoerbestand instellen. Klik op het **Bestandsuitvoer** -onderdeel en de expressie voor het bestand bewerken:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Uitvoer-bestandsnaam](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Uitvoer-bestandsnaam*

U kunt de werkstroom lokaal om te controleren of deze juist wordt uitgevoerd kunt uitvoeren.

Nadat deze is voltooid, kunt u deze kunt uitvoeren in Azure Media Services.

Activa in Azure Media Services met twee bestanden in deze eerst voorbereiden: het videobestand en het logo. U kunt dit doen met behulp van de .NET of REST API. U kunt dit ook doen met behulp van de Azure beheerportal of [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Deze zelfstudie ziet u hoe u activa met AMSE beheren. Er zijn twee manieren bestanden toevoegen aan een actief:

- Een lokale map maken, de twee bestanden in deze, kopiëren en slepen en neerzetten van de map naar het tabblad **activa** .
- Upload het videobestand als een actief, de informatie voor activa weergeven, gaat u naar het tabblad bestanden en een extra (logo)-bestand uploaden.

>[AZURE.NOTE]Controleer of het instellen van een primaire-bestand in de activa (het belangrijkste videobestand).

![Activa-bestanden in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Activa-bestanden in AMSE*


Selecteer de activa en kies naar deze coderen met Premium Encoder. De werkstroom uploaden en selecteer deze.

Klik op de knop om gegevens aan de processor doorgeven en de volgende XML om in te stellen van de runtime-eigenschappen toevoegen:

![Premium Encoder in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium Encoder in AMSE*


Plak de volgende XML-gegevens. U moet de naam van het videobestand opgeven voor de invoer van Media-bestand en de primarySourceFile. Geef de naam van de bestandsnaam voor het logo te.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Als u de .NET SDK maken en uitvoeren van de taak hebt gebruikt, heeft dit XML-gegevens moeten worden doorgegeven als configuratietekenreeks.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Wanneer de taak voltooid is, wordt het MP4-bestand in de uitvoer van activa de overlay!

![Klik op de video overlay](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Klik op de video overlay*


U kunt de voorbeeldwerkstroom downloaden van [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Zie ook

- [Inleiding tot Premium codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Het gebruik van de Premium-codering in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [De inhoud op aanvraag met Azure Media Services-codering](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media Encoder Premium werkstroom notaties en codecs](media-services-premium-workflow-encoder-formats.md)

- [Voorbeeldbestanden werkstroom](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Hulpprogramma Azure Media Services Explorer](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
