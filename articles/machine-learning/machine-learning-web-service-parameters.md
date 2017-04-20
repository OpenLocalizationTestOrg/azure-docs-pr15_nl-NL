<properties 
    pageTitle="Gebruik van Azure Machine Learning-Web-serviceparameters | Microsoft Azure" 
    description="Het gebruik van Azure Machine Learning Web Service Parameters voor het wijzigen van het gedrag van het model als de webservice wordt geopend." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Azure Machine Learning-Web-serviceparameters gebruiken

Een webservice Azure Machine Learning is gemaakt door te publiceren van een experiment met modules met configureerbare parameters. In sommige gevallen wilt u mogelijk het gedrag van de module wijzigen terwijl de webservice wordt uitgevoerd. *Web Service Parameters* kunt u deze taak. 

Een algemeen voorbeeld is instellen van de [Gegevens importeren] [ reader] module zodat de gebruiker van de gepubliceerde webservice een andere gegevensbron opgeven kunt wanneer de webservice wordt geopend. Of te configureren de [Gegevens exporteren] [ writer] module zodat een andere bestemming kan worden opgegeven. Andere voorbeelden zijn onder andere het aantal bits wijzigen voor de [Functie Hashing] [ feature-hashing] module of het aantal gewenst functies voor de [Functieselectie Filter gebaseerde] [ filter-based-feature-selection] module. 

U kunt Web Service Parameters instellen en koppelen aan een of meer moduleparameters in uw experiment en kunt u opgeven of ze verplicht of optioneel zijn. De gebruiker van de webservice kan vervolgens waarden voor deze parameters opgeven wanneer ze de webservice belt. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Het instellen en gebruiken van Web Service Parameters

U kunt een Parameter van de Service Web definiëren door te klikken op het pictogram naast de parameter voor een module en 'Zet als web Serviceparameter' te selecteren. Hiermee maakt een nieuwe Parameter van de Web-Service en met die parameter module verbonden. Als de webservice wordt geopend, vervolgens de gebruiker een waarde kunt opgeven voor de Parameter en deze wordt toegepast op de module-parameter.

Nadat u een Parameter van de Service Web definieert, is het beschikbaar voor alle andere module parameters op het experiment. Als u de Parameter van een Web-webservice die is gekoppeld aan een parameter voor één module definieert, kunt u die dezelfde Web Service-Parameter voor elke andere module, zo lang maken als de parameter hetzelfde soort waarde verwacht. Bijvoorbeeld als de Parameter een numerieke waarde is, kan klikt u vervolgens deze alleen worden gebruikt voor moduleparameters die wel een numerieke waarde wordt verwacht. Wanneer de gebruiker wordt een waarde voor de Parameter ingesteld, wordt deze worden toegepast op alle bijbehorende moduleparameters.

U kunt beslissen of u kunt een standaardwaarde opgeven voor de Parameter. Als u doet, klikt u vervolgens is de parameter optioneel voor de gebruiker van de webservice. Als u niet een standaardwaarde bevatten, klikt u vervolgens de gebruiker moet een waarde invoeren als de webservice wordt geopend.

De API-documentatie voor de webservice bevat informatie voor de gebruiker van de web-service voor het opgeven van de Parameter via een programma bij het openen van de webservice.

>[AZURE.NOTE] De API-documentatie voor een klassiek webservice is beschikbaar via de koppeling **API help-pagina** in de webservice **DASHBOARD** in Machine Learning Studio. De API-documentatie voor een nieuwe webservice is beschikbaar via de portal [Azure Machine Learning-webservices](https://services.azureml.net/Quickstart) op de pagina **verbruiken** en **Swagger-API's** voor uw webservice.


##<a name="example"></a>Voorbeeld

Als u bijvoorbeeld Stel dat we hebben een experiment met een [Gegevens exporteren] [ writer] module die gegevens naar Azure-blobopslag worden. Definiëren we een Web-Service-Parameter met de naam 'Blob pad' waarmee de gebruiker van de web-service het pad naar de blobopslag wijzigen wanneer de service wordt geopend.

1.  Klik op de [Gegevens exporteren] in Machine Learning Studio[ writer] module om deze te selecteren. De eigenschappen worden weergegeven in het deelvenster Eigenschappen aan de rechterkant van het tekenpapier experiment.

2.  Het opslagtype opgeven:

    - Selecteer onder **Ga gegevensbestemming opgeeft**, "Azure-blobopslag".
    - Selecteer onder **Geef verificatietype**, 'Account'.
    - Voer de gegevens van het account voor de Azure-blobopslag. 
    <p />

3.  Klik op het pictogram rechts van het **pad naar het begin met container parameter blob**. Deze ziet er zo uit:

    ![Pictogram van de Service queryparameter Web][icon]

    Selecteer 'Zet als web Serviceparameter'.

    Een vermelding wordt onder **Web Service Parameters** onderaan in het eigenschappenvenster met de naam "Pad naar het begin met container blob" toegevoegd. Dit is de Parameter die is nu gekoppeld aan deze [Gegevens exporteren] [ writer] module parameter.

4.  De naam van de Parameter, klik op de naam, voert u 'Blob pad' en druk op **ENTER** . 
 
5.  Een standaardwaarde opgeven voor de Parameter van de Service Web en klikt u op het pictogram rechts van de naam, selecteert u 'Zodat de waarde', voer een waarde (bijvoorbeeld "container1/output1.csv"), druk op **ENTER** .

    ![Parameter voor web-webservice][parameter]

6.  Klik op **uitvoeren**. 

7.  Klik op **Webservice implementeren** en **Implementeren van een webservice [klassieke]** of **Implementeren webservice [Nieuw]** naar het implementeren van de webservice.

De gebruiker van de webservice kan nu nieuwe bestemming opgeeft voor de [Gegevens exporteren] [ writer] module bij het openen van de webservice.

##<a name="more-information"></a>Meer informatie

Zie voor meer gedetailleerde voorbeeld van een de vermelding van de [Parameters van Web-Service](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) in de [Machine Learning-Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Zie voor meer informatie over het openen van een webservice Machine Learning, [hoe u een gepubliceerde machine learning-webservice in beslag nemen](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
