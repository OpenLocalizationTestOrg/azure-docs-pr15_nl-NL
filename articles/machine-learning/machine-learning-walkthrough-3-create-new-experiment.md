<properties
    pageTitle="Stap 3: Maak een nieuwe Machine Learning-experiment | Microsoft Azure"
    description="Stap 3 van de prognose blog oplossing Stapsgewijze instructies: Maak een nieuwe training experiment in Azure Machine Learning Studio."
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
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Stapsgewijze instructies stap 3: Maak een nieuwe Azure Machine Learning-experiment

Dit is de derde stap van de procedure, [een blog analytics-oplossing in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  **Maak een nieuwe experiment**
4.  [Trainen en de modellen voor evalueren](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice](machine-learning-walkthrough-6-access-web-service.md)

----------

De volgende stap in dit scenario is het opzetten van een experiment in Machine Learning Studio die gebruikmaakt van de gegevensset die wordt geüpload.  

1.  Studio, klikt u op **+ Nieuw** onderaan in het venster.
2.  Selecteer **EXPERIMENT**en selecteer vervolgens "Lege Experiment". Selecteer de naam van de standaard experiment boven aan het tekenpapier en wijzig de naam herkenbare

    > [AZURE.TIP] Het is een goede gewoonte om in te vullen **samenvattings** - en **Beschrijving** voor het experiment in het deelvenster **Eigenschappen** . Deze eigenschappen kunnen u de mogelijkheid om het document het experiment zodat iedereen die naar deze later kijkt duidelijke uw doelstellingen en methodologie.

3.  In het palet module aan de linkerkant van het experiment canvas, **Opgeslagen gegevenssets**uitvouwen.
4.  Zoek de gegevensset die u hebt gemaakt onder **Mijn gegevenssets** en sleep deze naar het canvas. U kunt ook de gegevensset vinden door de naam in het vak **Zoeken** boven het palet invoeren.  

##<a name="prepare-the-data"></a>De gegevens voorbereiden
U kunt de eerste 100 rijen van de gegevens en sommige statistische gegevens voor de hele gegevensset weergeven door te klikken op de uitvoerpoort van de gegevensset (de kleine cirkel onderaan) en **visualiseren**te selecteren.  

Omdat het gegevensbestand niet meegeleverd kolomkoppen, biedt Studio algemene koppen (Kol1, Kol2, *enzovoort*). Goede koppen zijn niet essentiële voor het maken van een model, maar ze eenvoudiger kunt werken met de gegevens in het experiment. Ook als we uiteindelijk dit model in een webservice publiceert, kunt de koppen identificeren van de kolommen aan de gebruiker van de service.  

We kunt toevoegen met de [Metagegevens bewerken] kolomkoppen[ edit-metadata] module.
U gebruikt de [Metagegevens bewerken] [ edit-metadata] module metagegevens die is gekoppeld aan een gegevensset wijzigen. In dit geval kan bieden meer beschrijvende naam voor de kolomkoppen. 

Gebruik van [Metagegevens bewerken][edit-metadata], u eerst opgeven welke kolommen u wilt wijzigen (in dit geval alle labels.) Geef vervolgens de actie moet worden uitgevoerd in de kolommen (in dit geval kolomkoppen wijzigen.)

1.  Typ in het palet module 'metagegevens' in het vak **Zoeken** . Ziet u [Metagegevens bewerken] [ edit-metadata] weergegeven in de modulelijst.
2.  Klik en sleep de [Metagegevens bewerken] [ edit-metadata] module op het tekenpapier en zet deze neer onder de gegevensset we eerder hebt toegevoegd.
3.  Verbinding maken met de gegevensset in de [Metagegevens bewerken][edit-metadata]: klik op de uitvoerpoort van de gegevensset (de kleine cirkel onderaan in de gegevensset.) Sleept u vervolgens naar de invoer poort van [Metagegevens bewerken] [ edit-metadata] (de kleine cirkel boven aan de module), laat de muisknop los. De gegevensset en module blijven verbonden, zelfs als u een op het canvas navigeren.

    Het experiment ziet er nu ongeveer zo uitziet:  

    ![Het toevoegen van metagegevens bewerken][2]
    
    De rood uitroepteken geeft aan dat we de eigenschappen voor deze module nog niet hebt ingesteld. We doen om het dialoogvenster.
    
    > [AZURE.TIP] U kunt een opmerking toevoegen aan een module door te dubbelklikken op de module en tekst in te voeren. Hiermee kunt u één oogopslag zien wat de module in uw experiment doet. In dit geval Dubbelklik op de [Metagegevens bewerken] [ edit-metadata] module en typ de opmerking "toevoegen kolomkoppen". Klik ergens anders op het tekenpapier te sluiten van het tekstvak. Als u wilt weergeven op de opmerking, klikt u op de pijl-omlaag op de module.

4.  Selecteer [Bewerken metagegevens][edit-metadata], klikt u in het deelvenster **Eigenschappen** aan de rechterkant van het tekenpapier **kolom Gegevenskiezer starten**.
5.  Klik in het dialoogvenster **Selecteer kolommen** selecteert u alle rijen in de **Beschikbare kolommen** en klik op > om deze te verplaatsen naar de **Geselecteerde kolommen**.
Het dialoogvenster ziet er als volgt: ![kolom Gegevenskiezer met alle kolommen is geselecteerd][4]
7.  Klik op het vinkje **OK** .
8.  Terug in het deelvenster **Eigenschappen** , zoekt u de parameter **nieuwe kolomnamen** . In dit veld, voert u een lijst met namen van de 21 kolommen in de gegevensset, gescheiden door komma's en in de kolomvolgorde. Kunt u de namen van kolommen uit de gegevensset documentatie op de website van UCI of uitkomt kunt u kopiëren en plakken in de volgende lijst:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Het deelvenster eigenschappen er als volgt uit:

    ![Eigenschappen voor metagegevens bewerken][1]

> [AZURE.TIP] Als u controleren of de kolomkoppen wilt, voert u het experiment (Klik op **uitvoeren** onder het tekenpapier experiment). Wanneer is voltooid (een groen vinkje wordt weergegeven op [Metagegevens bewerken][edit-metadata]), klikt u op de uitvoerpoort van de [Metagegevens bewerken] [ edit-metadata] module en selecteer **visualiseren**. U kunt de uitvoer van elke module weergeven op dezelfde manier om weer te geven van de voortgang van de gegevens via het experiment.

##<a name="create-training-and-test-datasets"></a>Maken van training en gegevenssets testen

De volgende stap in het experiment is om te genereren afzonderlijk gegevenssets die we gebruiken voor zowel training en testen van onze model.

Hiervoor gebruik van de [Gesplitste gegevens] [ split] module.  

1.  De [Gesplitste gegevens] zoeken[ split] module, sleep deze naar het tekenpapier en verbindt u deze naar de laatste [Metagegevens bewerken] [ edit-metadata] module.
2.  Standaard de verhouding tussen de gesplitste 0,5 is en de parameter **Randomized splitsen** is ingesteld. Dit betekent dat er een willekeurig helft van de gegevens uitgevoerd via één poort van de [Gesplitste gegevens wordt] [ split] module en halverwege door de andere. U kunt deze, evenals de **willekeurig** basisparameter, om te wijzigen van de splitsing tussen training en testen van gegevens aanpassen. In dit voorbeeld we laten ze als-is.
    
    > [AZURE.TIP] De eigenschap **deel van de rijen in de eerste uitvoer gegevensset** bepaalt u hoeveel van de gegevens uitvoer via de uitvoerpoort links. Als u de verhouding tussen de instelt op 0,7, klikt u vervolgens is 70% van de gegevens bijvoorbeeld uitvoer via de links poort en 30% via de juiste poort.  
    
3. Dubbelklik op de [Gesplitste gegevens] [ split] module en voert u de opmerking, "Training/testen gegevens splitsen 50%". 

We de beschikking over de uitvoer van de [Gesplitste gegevens] [ split] module echter we lijken, maar we kiest u de links uitvoer gebruiken, zoals trainingsgegevens en rechts als testen gegevens uitvoeren.  

Zoals vermeld op de website van UCI, de kosten van een hoge kredietrisico zo klein onjuiste vijf keer groter is dan de kosten van een lage kredietrisico zo hoog onjuiste. Als u wilt daarom genereren we een nieuwe gegevensset die overeenkomt met deze functie kosten. In de nieuwe gegevensset, wordt elke voorbeeld hoog risico vijf keer gerepliceerd terwijl het voorbeeld van elke laag risico wordt niet gerepliceerd.   

We deze replicatie kunt doen met R-code:  

1.  Zoek en sleep het [R-Script uitvoeren] [ execute-r-script] module naar het experiment canvas en verbindt u de uitvoerpoort links van de [Gesplitste gegevens] [ split] module naar de eerste invoer poort ("Dataset1") van het [R-Script uitvoeren] [ execute-r-script] module.
2. Dubbelklik op het [R-Script uitvoeren] [ execute-r-script] module en voert u de opmerking, 'Set kosten correctie'.
2.  Klik in het deelvenster **Eigenschappen** de standaardtekst in de parameter **R Script** verwijderen en voer dit script:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


We nodig voor deze dezelfde herhaling bewerking voor elke uitvoer van de [Gesplitste gegevens] [ split] module zodat de gegevens van training en testen de dezelfde kostencorrectie hebt.

1.  Met de rechtermuisknop op het [R-Script uitvoeren] [ execute-r-script] module en selecteer **kopiëren**.
2.  Met de rechtermuisknop op het canvas experiment en selecteer **Plakken**.
3.  Verbinding maken met de eerste invoer poort van dit [R-Script uitvoeren] [ execute-r-script] module rechts uitvoer poort van de [Gesplitste gegevens] [ split] module. 
4.  Aan de onderkant van het papier, klikt u op **uitvoeren**. 

> [AZURE.TIP] De kopie van de module R-Script uitvoeren bevat hetzelfde script als de oorspronkelijke module. Wanneer u kopiëren en plakken van een module op het canvas, behoudt de kopie alle eigenschappen van de oorspronkelijke.  

Ons experiment er nu ongeveer zo uitziet:

![Gesplitste module en R scripts toe te voegen][3]

Zie voor meer informatie over het gebruik van R-scripts in uw experimenten [uitbreiden uw experiment met R](machine-learning-extend-your-experiment-with-r.md).

**Volgende: [trein evalueert in de modellen voor](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
