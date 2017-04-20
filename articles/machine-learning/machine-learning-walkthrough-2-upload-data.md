<properties
    pageTitle="Stap 2: Gegevens uploaden in een experiment Machine Learning | Microsoft Azure"
    description="Stap 2 van de prognose blog oplossing Stapsgewijze instructies: Upload in openbare gegevens opgeslagen in Azure Machine Learning Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Stapsgewijze instructies stap 2: Bestaande gegevens in een experiment Azure Machine Learning uploaden

Dit is de tweede stap van de procedure, [een blog analytics-oplossing in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Bestaande gegevens uploaden**
3.  [Maak een nieuwe experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Trainen en de modellen voor evalueren](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice](machine-learning-walkthrough-6-access-web-service.md)

----------

Als u wilt een blog model voor kredietrisico ontwikkelt, moeten we gegevens die u gebruiken kunt om te trainen en test vervolgens het model. Voor deze procedure gebruiken we de "UCI Statlog (Duitse krediet gegevens) gegevensverzameling" uit de UCI Machine Learning-bibliotheek. U kunt deze hier vinden:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+credit+Data)</a>

We het bestand met de naam **german.data**gebruiken. Hiermee kunt u dit bestand downloaden naar uw lokale harde schijf.  

Deze dataset bevat rijen met 20 variabelen voor 1000 afgelopen asielzoekers creditcard. Deze 20 variabelen vertegenwoordigen van de gegevensset functie vector, waarmee identificerende kenmerken voor elke sollicitant creditcard. Een extra kolom in elke rij vertegenwoordigt van de sollicitant berekende kredietrisico, met 700 sollicitanten die zijn geïdentificeerd als een laag krediet- en 300 als een hoog risico.

De website UCI biedt een beschrijving van de kenmerken van de functie vector, waaronder financiële gegevens, krediet geschiedenis indiensttreding status en persoonlijke gegevens. Voor elke sollicitant een binaire beoordeling is opgegeven die aangeeft of ze een laag zijn of hoog risico creditering.  

We gebruiken deze gegevens om te trainen een blog analytics-model. Als we klaar bent, is onze model moet kunnen informatie voor andere personen te accepteren en voorspellen of ze een hoog of creditcard risico zijn.  

Hier ziet u een interessante twist. De beschrijving van de gegevensset wordt uitgelegd dat onjuiste van een persoon, zoals een lage kredietrisico wanneer ze daadwerkelijk een hoge kredietrisico zijn 5 keer meer dure aan de financiële instelling dan een laag kredietrisico zo hoog onjuiste is. Een eenvoudige manier om te rekening in ons experiment is door te dupliceren (5 keer) de items die iemand met een hoge kredietrisico vertegenwoordigen. Vervolgens gaat u als het model ten onrechte een hoge kredietrisico zo klein classificeert, het doet die misclassification 5 keer eenmaal voor elke dupliceren. Hiermee worden de kosten van deze fout in de zoekresultaten training vergroten.  

##<a name="convert-the-dataset-format"></a>De opmaak van de gegevensset converteren
De oorspronkelijke gegevensset gebruikt een lege waarde gescheiden. Machine Learning Studio werkt beter met een bestand met door komma's gescheiden waarden (.csv), zodat we de gegevensset converteren wordt met spaties vervangen door komma's.  

Er zijn tal van manieren om deze gegevens te converteren. Er is een manier met behulp van de volgende Windows PowerShell-opdracht:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Een andere manier is met de opdracht Unix-sed:  

    sed 's/ /,/g' german.data > german.csv  

We hebben een versie door komma's gescheiden van de gegevens in een bestand met de naam **german.csv** die we in ons experiment gebruiken gemaakt in beide gevallen.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>De gegevensset uploaden naar Machine Learning Studio

Nadat de gegevens naar een CSV-indeling is geconverteerd, moeten we uploadt dit naar Machine Learning Studio. Zie voor meer informatie over het aan de slag met Machine Learning Studio, [Microsoft Azure Machine Learning Studio start](https://studio.azureml.net/).

1.  Open Machine Learning-Studio ([https://studio.azureml.net](https://studio.azureml.net)). Wanneer u wordt gevraagd aan te melden, gebruikt u het account dat u hebt opgegeven als de eigenaar van de werkruimte.
1.  Klik op het tabblad **Studio** boven aan het venster.
1.  Klik op **+ Nieuw** onderaan in het venster.
1.  Selecteer **GEGEVENSSET**.
1.  Selecteer **FROM lokaal bestand**.
1.  Klik in het dialoogvenster **een nieuwe gegevensset uploaden** op **Bladeren** en zoek het **german.csv** -bestand dat u hebt gemaakt.
1.  Voer een naam voor de gegevensset. Dit scenario wordt als we bellen van 'UCI Duits creditcard-gegevens'.
1.  Selecteer voor gegevenstype, **algemene CSV-bestand met geen koptekst (. nh.csv)**.
1.  Een beschrijving toevoegen als u wilt.
1.  Klik op **OK**.  

![De gegevensset uploaden][1]  


Dit uploadt de gegevens in een gegevensset-module die we in een experiment kunnen gebruiken.

> [AZURE.TIP] Klik op het tabblad **GEGEVENSSETS** aan de linkerkant van het venster Studio als u wilt beheren gegevenssets die u hebt geüpload naar Studio.

Zie voor meer informatie over het importeren van diverse soorten gegevens in een experiment [trainingsgegevens naar Azure Machine Learning Studio importeren](machine-learning-data-science-import-data.md).

**Volgende: [maken een nieuwe experiment](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
