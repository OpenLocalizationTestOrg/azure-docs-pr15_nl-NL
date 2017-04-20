<properties 
    pageTitle="Fouten opsporen in uw Model in Azure Machine Learning | Microsoft Azure" 
    description="Dit artikel wordt uitgelegd hoe u hoe u fouten opsporen in uw Model in Azure Machine Learning." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Fouten opsporen in uw Model in Azure Machine Learning

In dit artikel wordt uitgelegd hoe u kunt uw modellen in Microsoft Azure Machine Learning foutopsporing. Deze bedekt specifiek, de mogelijke redenen waarom een van de volgende twee mislukt scenario's zich voordoen kan bij het uitvoeren van een model:

* het [Model trein] [ train-model] module genereert een fout 
* het [Model Score] [ score-model] module onjuiste resultaten oplevert 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Trein Model Module genereert een fout

![image1](./media/machine-learning-debug-models/train_model-1.png)

Het [Model trein] [ train-model] Module verwacht de volgende 2 invoer:

1. Het type indeling/regressie Model uit de verzameling modellen verstrekt door Azure Machine Learning
2. De trainingsgegevens met een opgegeven kolom met labels. De kolom Label geeft de variabele te voorspellen. De rest van de kolommen die zijn opgenomen zijn uitgegaan functies.

Deze module genereert een fout in de volgende gevallen:

1. De kolom Label is onjuist opgegeven omdat meer dan één kolom is geselecteerd als het etiket of een onjuiste kolomindex is geselecteerd. Het tweede geval zou bijvoorbeeld toepassen als een kolomindex van 30 is gebruikt met een invoer dataset die slechts 25 kolommen heeft.

2. De gegevensset bevat geen functie kolommen. Bijvoorbeeld als de invoer gegevensset slechts 1 kolom bevat, die is gemarkeerd als de kolom Label, zou er geen functies waarmee u het model kunt maken. In dit geval de [Trein Model] [ train-model] module in de weergave van een fout.

3. De invoer gegevensset (functies of Label) bevatten oneindigheid als een waarde.


## <a name="score-model-module-does-not-produce-correct-results"></a>Score Model Module levert geen correcte resultaten

![image2](./media/machine-learning-debug-models/train_test-2.png)

In een normale grafiek training/testen voor gecontroleerde leren, de [Gesplitste gegevens] [ split] module deelt de oorspronkelijke gegevensset in twee delen: het deel dat wordt gebruikt voor het model trainen en het deel dat wordt gereserveerd voor hoe u ook het ervaren model wordt uitgevoerd op gegevens deze score heeft geen training op. Het ervaren model wordt vervolgens gebruikt voor het verkrijgen van de testgegevens waarna de resultaten worden geëvalueerd om te bepalen de nauwkeurigheid van het model.

Het [Model Score] [ score-model] module vereist twee invoeren:

1. Een ervaren model-uitvoer van [Trein Model] [ train-model] module
2. Een score gegevensset niet die het model is niet op training

Het kan voorkomen dat hoewel het experiment is geslaagd, het [Model Score] [ score-model] module onjuiste resultaten oplevert. Verschillende scenario's kunnen ertoe leiden dat dit gebeurt:

1. Als de opgegeven naam bestaan is en een regressiemodel is training voor gegevens, een onjuiste uitvoer zou worden geproduceerd door het [Model Score] [ score-model] module. Dit komt doordat regressie een doorlopend antwoord-variabele is vereist. In dit geval moet meer geschikt is voor het gebruik van een classificatie-model. 
2. Als een classificatie-model is trainen op een gegevensset met zwevende kommagetallen in de kolom Label, kan deze op dezelfde manier ongewenste resultaten opleveren. Dit komt doordat classificatie vereist een aparte antwoord variabele waarmee alleen waarden dat bereik een eindige en meestal iets kleine set klassen.
3. Als de score gegevensset bevat niet alle functies die worden gebruikt om te trainen van het model, het [Model Score] [ score-model] een fout zullen produceren.
4. Het [Model Score] [ score-model] zou uitvoer overeenkomt met een rij in de score gegevensset die een ontbrekende waarde of een oneindig waarde voor een van de functies bevat geen produceren.
5. Het [Model Score] [ score-model] identieke uitvoer voor alle rijen in de score gegevensset kunnen produceren. Dit kan gebeuren, bijvoorbeeld in de wanneer u probeert classificatie met behulp van beslissing Forests als het minimum aantal voorbeelden per knooppuntniveau knooppunt wordt gekozen moeten meer dan het aantal training voorbeelden beschikbaar.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
