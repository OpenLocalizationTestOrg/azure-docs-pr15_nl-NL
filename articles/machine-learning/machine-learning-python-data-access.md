<properties 
    pageTitle="Toegang tot gegevenssets met Machine Learning Python clientbibliotheek | Microsoft Azure" 
    description="Installeren en gebruiken van de bibliotheek van de client Python kunnen openen en beheren van Azure Machine Learning-gegevens veilig uit een lokale Python-omgeving." 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Access gegevenssets met Python met behulp van de Azure Machine Learning Python client-bibliotheek 

De Preview-versie van Microsoft Azure Machine Learning Python clientbibliotheek kunt beveiligde toegang inschakelen naar uw Azure Machine Learning-gegevenssets uit een lokale Python-omgeving en schakelt het maken en beheren van gegevenssets in een werkruimte.

In dit onderwerp bevat instructies voor het:

* de bibliotheek van de client Machine Learning Python installeren 
* openen en upload gegevenssets, met inbegrip van de instructies voor het ophalen van autorisatie voor toegang tot Azure Machine Learning gegevenssets vanaf uw lokale Python-omgeving
*  toegang tot tussenliggende gegevenssets vanaf experimenten
*  de bibliotheek van de client Python gegevenssets opsommen, toegang tot metagegevens, de inhoud van een gegevensset lezen, nieuwe gegevenssets maken en bijwerken van bestaande gegevenssets opnieuw gebruiken

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Vereisten voor

De bibliotheek van de client Python is getest onder de volgende omgevingen:

 - Windows, Mac en Linux
 - Python 2.7, 3.3 en 3.4

Deze is afhankelijk van de volgende pakketten:

 - aanvragen
 - Python-dateutil
 - pandas

Het is raadzaam om met die een verdeling Python zoals [Anaconda](http://continuum.io/downloads#all) of [bladerdak](https://store.enthought.com/downloads/), die komen met Python, IPython en de drie pakketten hierboven wordt geïnstalleerd. Hoewel IPython niet strikt vereist is, is een geweldige omgeving voor het werken en interactief gegevens visualiseren.


###<a name="installation"></a>Het installeren van de Azure Machine Learning Python client-bibliotheek

De bibliotheek van de client Azure Machine Learning Python moet ook zijn geïnstalleerd als de taken die worden beschreven in dit onderwerp wilt uitvoeren. Deze is beschikbaar via de [Python pakket Index](https://pypi.python.org/pypi/azureml). Als u wilt installeren in uw omgeving Python, voer de volgende opdracht uit uw lokale Python-omgeving:

    pip install azureml

U kunt ook downloaden en installeren uit de bronnen op [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Hebt u een cijfer op uw computer zijn geïnstalleerd, kunt u pip rechtstreeks vanuit de bibliotheek cijfer installeren:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Studio codefragmenten gebruiken voor toegang tot gegevenssets

De bibliotheek van de client Python biedt u via programmacode toegang tot uw bestaande gegevenssets uit experimenten die zijn uitgevoerd.

U kunt codefragmenten die alle benodigde informatie om te downloaden en gegevenssets terugconverteren als Pandas DataFrame objecten op uw computer locatie bevatten genereren van de Studio web-interface.

### <a name="security"></a>Beveiliging voor toegang tot gegevens

De codefragmenten die is verstrekt door Studio voor gebruik met de bibliotheek van de client Python, inclusief uw werkruimte-id en autorisatie token. Deze volledige toegang tot uw werkruimte te bieden en moeten worden beveiligd, zoals een wachtwoord.

De functionaliteit van de codefragment van de code is alleen beschikbaar voor gebruikers die de instellen als **eigenaar** voor de werkruimte van hun rol vanwege de beveiliging. Uw rol wordt weergegeven in Azure Machine Learning Studio op de pagina **gebruikers** onder **Instellingen**.

![Beveiliging][security]

Als uw rol niet is ingesteld als **eigenaar**, kunt u een aanvraag om deel te opnieuw worden uitgenodigd als eigenaar of de eigenaar van de werkruimte zodat u het codefragment vragen.

Als u het Autorisatietoken, kunt u het volgende doen:



- Vragen om een token van een eigenaar. Eigenaren hebben toegang tot hun tokens autorisatie van de pagina instellingen van de werkruimte in Studio. Selecteer **Instellingen** in het linkerdeelvenster en klik op **AUTORISATIE TOKENS** als u wilt zien van de primaire en secundaire tokens.  Hoewel de primaire of de secundaire autorisatie tokens kunnen worden gebruikt in het codefragment, wordt het wordt aanbevolen dat eigenaren alleen de secundaire autorisatie tokens delen.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Vraag om te worden gepromoveerd tot de rol van eigenaar.  Hiervoor moet een huidige eigenaar van de werkruimte eerst verwijdert u van de werkruimte en u opnieuw uitnodigen voor dit als een eigenaar.

Nadat u ontwikkelaars hebt ontvangen de werkruimte-id en autorisatie toegangstoken, ze hebben toegang tot de werkruimte met behulp van het codefragment ongeacht hun rol.

Autorisatie tokens worden beheerd op de pagina **AUTORISATIE TOKENS** onder **Instellingen**. U kunt ze genereren, maar deze procedure trekt u toegang tot het vorige token.

### <a name="accessingDatasets"></a>Access gegevenssets uit een lokale Python-toepassing

1. Machine Learning Studio, klikt u op de navigatiebalk aan de linkerkant op **GEGEVENSSETS** .

2. Selecteer de gegevensset die u wilt openen. U kunt een van de gegevenssets selecteren in de lijst **Mijn GEGEVENSSETS** of in de lijst met **voorbeelden** .

3. Klik op de onderste werkbalk, op **Data Access-Code genereren**. Als de gegevens in een indeling die niet compatibel met de bibliotheek van de client Python, wordt deze knop niet beschikbaar.

    ![Gegevenssets][datasets]

4. Selecteer het codefragment in het venster dat wordt weergegeven en deze naar het Klembord kopiëren.

    ![Toegangscode][dataset-access-code]

5. Plak de code in het notitieblok van uw lokale Python-toepassing.

    ![Notitieblok][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Access tussenliggende gegevenssets van Machine Learning-experimenten

Nadat een experiment in de Machine Learning-Studio is uitgevoerd, is het mogelijk voor toegang tot de tussenliggende gegevenssets vanaf de knooppunten uitvoer van modules. Tussenliggende gegevenssets zijn gegevens die is gemaakt en gebruikt voor tussenliggende stap, wanneer een hulpmiddel model is uitgevoerd.

Tussenliggende gegevenssets kunnen worden geopend, zo lang maken als de gegevensindeling compatibel met de bibliotheek van de client Python is.

De volgende indelingen worden ondersteund (constanten voor deze zijn de `azureml.DataTypeIds` class):

 - Tekst zonder opmaak
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

U kunt de opmaak bepalen door een module uitvoer knooppunt aanwijst. Samen met de naam van het knooppunt, klikt u in de knopinfo wordt weergegeven.

Enkele van de modules, zoals de [gesplitste] [ split] module uitvoer naar een indeling met de naam `Dataset`, die niet wordt ondersteund door de Python client-bibliotheek.

![Opmaak van de gegevensset][dataset-format]

U moet een module conversie, zoals [converteren naar CSV]gebruiken[convert-to-csv], een uitvoer in een ondersteunde indeling opvragen.

![GenericCSV opmaken][csv-format]

De volgende stappen uit weergeven een voorbeeld van een experiment maakt, wordt deze uitgevoerd en toegang heeft tot de tussenliggende gegevensset.

1. Maak een nieuwe experiment.

2. Voeg een module **volwassen telling inkomsten binaire indeling gegevensset** .

3. Invoegen van een [gesplitste] [ split] module, en verbindt u de invoer voor de uitvoer van de module gegevensset.

4. Invoegen een [converteren naar CSV] [ convert-to-csv] module en verbindt u de invoer voor een van de [gesplitste] [ split] module uitvoer.

5. Sla het experiment, worden uitgevoerd en wacht totdat deze voltooien.

6. Klik op het knooppunt uitvoer op de [converteren naar CSV] [ convert-to-csv] module.

7. Wanneer het contextmenu wordt weergegeven, selecteert u **Data Access-Code genereren**.

    ![Het contextmenu][experiment]

8. Selecteer het codefragment en naar het Klembord kopiëren vanuit het venster dat wordt weergegeven.

    ![Toegangscode][intermediate-dataset-access-code]

9. Plak de code in uw notitieblok.

    ![Notitieblok][ipython-intermediate-dataset]

10. U kunt de gegevens met matplotlib kunt visualiseren. Hiermee worden weergegeven in een histogram voor de kolom Leeftijd:

    ![Histogram][ipython-histogram]


##<a name="clientApis"></a>De bibliotheek van de client Machine Learning Python kunt openen, lezen, maken en beheren van gegevenssets opnieuw gebruiken

### <a name="workspace"></a>Werkruimte

De werkruimte is het ingangspunt voor de bibliotheek van de client Python. Geef de `Workspace` klasse met uw werkruimte-id en autorisatie token een exemplaar te maken:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Gegevenssets opsommen

Opsommen alle gegevenssets in een bepaalde werkruimte:

    for ds in ws.datasets:
        print(ds.name)

Alleen de gebruiker gedefinieerde gegevenssets opsommen:

    for ds in ws.user_datasets:
        print(ds.name)

Alleen de voorbeeld-gegevenssets opsommen:

    for ds in ws.example_datasets:
        print(ds.name)

U kunt een gegevensset openen met hun naam weergeven (dit is hoofdlettergevoelig):

    ds = ws.datasets['my dataset name']

Of u kunt deze openen met de index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metagegevens

Gegevenssets hebben metagegevens, naast de inhoud. (Tussenliggende gegevenssets vormen een uitzondering op deze regel en hoeft niet alle metagegevens.)

Sommige metagegevenswaarden die zijn toegewezen door de gebruiker bij het maken:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Anderen zijn waarden door Azure ML toegewezen:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Zie de `SourceDataset` class voor meer informatie over de beschikbare metagegevens.


### <a name="read-contents"></a>De inhoud lezen

De codefragmenten die is verstrekt door Machine Learning Studio automatisch downloaden en terugconverteren van de gegevensset aan een object Pandas DataFrame. Dit is gedaan met de `to_dataframe` methode:

    frame = ds.to_dataframe()

Als u liever de onbewerkte gegevens downloaden en uitvoeren van de deserialisatie uzelf, is dat een optie. Dit is de enige optie om notaties, zoals 'ARFF', terugconverteren van de bibliotheek van de client Python kan niet op het moment wordt.

De inhoud lezen als tekst:

    text_data = ds.read_as_text()

De inhoud lezen als binair bestand:

    binary_data = ds.read_as_binary()

U kunt ook gewoon een stroom aan de inhoud openen:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Een nieuwe gegevensset maken

De bibliotheek van de client Python kunt u gegevenssets vanuit uw programma Python uploaden. Deze gegevenssets zijn vervolgens beschikbaar voor gebruik in uw werkruimte.

Als u uw gegevens in een DataFrame Pandas hebt, gebruikt u de volgende code:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Als uw gegevens al serienummer heeft, kunt u het volgende gebruiken:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

De bibliotheek van de client Python kan een DataFrame Pandas naar de volgende indelingen serialiseren (constanten voor deze zijn de `azureml.DataTypeIds` class):

 - Tekst zonder opmaak
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Een bestaande gegevensset bijwerken

Als u probeert te uploaden van een nieuwe gegevensset met een naam die overeenkomt met een bestaande dataset, krijgt u moet een conflict is opgetreden.

Als u een bestaande gegevensset bijwerken, moet u eerst een verwijzing naar de bestaande gegevensset ophalen:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Gebruik vervolgens `update_from_dataframe` serialiseren en de inhoud van de gegevensset op Azure vervangen:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Als u de gegevens naar een andere indeling wilt, geeft u een waarde voor de optionele `data_type_id` parameter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Desgewenst kunt u een nieuwe beschrijving instellen door op te geven van een waarde voor de `description` parameter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

U kunt desgewenst een nieuwe naam instellen door op te geven van een waarde voor de `name` parameter. Vanaf nu op, moet u de gegevensset met alleen de nieuwe naam ophalen. De volgende code bijgewerkt de gegevens, de naam en beschrijving.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

De `data_type_id`, `name` en `description` parameters zijn optioneel en standaard naar hun vorige waarde. De `dataframe` parameter is altijd vereist.

Als uw gegevens al serienummer heeft, gebruikt u `update_from_raw_data` in plaats van `update_from_dataframe`. Als u alleen doorgeeft in `raw_data` in plaats van `dataframe`, deze werkt op een soortgelijke manier.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
