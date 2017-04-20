<properties 
    pageTitle="Geavanceerde codering werkstromen maken met werkstroomontwerper | Microsoft Azure" 
    description="Meer informatie over het maken van geavanceerde codering werkstromen met werkstroomontwerper." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Geavanceerde codering werkstromen maken met werkstroomontwerper

##<a name="overview"></a>Overzicht

De **Workflow Designer** is een Windows-desktopprogramma die wordt gebruikt om te ontwerpen en bouwen van aangepaste werkstromen voor het coderen met **Media Encoder Premium werkstroom**.
U kunt met behulp van de kracht van de werkstroom ontwerpfunctie hulpmiddel, ontwerpen en maken van complexe werkstromen die wordt uitgevoerd in de **Media Encoder Premium**.  

Werkstromen kunnen klant beslissing logica bevatten en vertakkingen op basis van eigenschappen van het bestand invoerbron. U kunt werkstromen maken met overridable eigenschappen en dynamische waarden u kunt zelfs het meest complexe codering taken eenvoudig te herhalen en aanpassen in de cloud.

Voorbeeld van de werkstromen die u kunt maken onder andere:

- Beslissing op basis van werkstromen die bij het controleren van de broninhoud voor resolutie en coderen van alleen de gewenste uitvoer nummers.  Dit is helfpul doordat de verspild nummers die moeten worden gegenereerd door de bron inhoud ongeluk upscaling.
- Meerdere invoer bestanden kunnen worden gebruikt om bijschriften, overlays en hechten samen inhoud te ondersteunen. 

Dit hulpmiddel kan ook worden gebruikt voor het wijzigen van een van onze [werkstromen die zijn gepubliceerd](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Als u uw exemplaar van het hulpmiddel werkstroomontwerper, neemt u contact op mepd@microsoft.com.


Nadat een werkstroom voor het bestand is gemaakt, kunnen worden geüpload als activa, en vervolgens worden gebruikt voor het coderen van media-bestanden. Zie [Geavanceerde coderen met Media Encoder Premium werkstroom](media-services-encode-with-premium-workflow.md)voor informatie over het coderen met **Media Encoder Premium werkstroom** met **.NET**.

##<a id="existing_workflows"></a>Bestaande werkstromen wijzigen

De standaard [werkstromen die zijn gepubliceerd](media-services-workflow-designer.md#existing_workflows) kan worden gewijzigd met de ontwerpfunctie functie. U krijgt standaard Werkstroombestanden [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). De map bevat ook de beschrijving van deze bestanden.

De volgende video's demonstreren het gebruik van de ontwerpfunctie.

###<a name="day-1--getting-started"></a>Dag 1 – aan de slag

Dag 1 video worden de volgende onderwerpen besproken:

- Ontwerpfunctie overzicht
- Eenvoudige werkstromen – "Hallo allemaal"
- Maken van meerdere MP4-bestanden voor gebruik met Azure Media Services streaming uitvoer

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Dag 2

Dag 2 video worden de volgende onderwerpen besproken:

- Variërende bron bestand scenario's: audio verwerken
- Werkstromen met geavanceerde logica
- Graph fasen

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>Dag 3

Dag 3 video worden de volgende onderwerpen besproken:

- Uitvoeren van scripts binnen werkstromen/blauwdrukken
- Beperkingen met de huidige Encoder
- Q & A
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Stuur een e-mail aan als u nodig hebt ondersteuning of vragen hebt over het maken van aangepaste werkstromen in de ontwerpfunctie hulpmiddel van de werkstroom, mepd@microsoft.com.

##<a name="see-also"></a>Zie ook

[Azure Premium Encoder Workflow Designer trainingsvideo 's](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
