<properties
    pageTitle="Verbinding maken met een Machine Learning-webservice | Microsoft Azure"
    description="Met C# of Python, verbinding maken met een Azure Machine Learning webservice met behulp van een sleutel autorisatie."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Verbinding maken met een Azure Machine Learning-webservice

De Azure Machine Learning-ervaring voor ontwikkelaars is een Web service API om voorspellingen van invoergegevens in realtime of in de batchmodus. U kunt Azure Machine Learning Studio voorspellingen maken en implementeren van een Azure Machine Learning-webservice.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Voor meer informatie over het maken en implementeren van een Machine Learning-webservice Machine Learning Studio gebruiken:

- Zie [uw eerste experiment maken](machine-learning-create-experiment.md)voor een zelfstudie over het maken van een experiment in Machine Learning Studio.
- Zie voor meer informatie over het implementeren van een webservice [Deploy een Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).
- In het algemeen, gaat u naar het [Trainingscentrum voor documentatie Machine](https://azure.microsoft.com/documentation/services/machine-learning/)voor meer informatie over Machine Learning.

## <a name="azure-machine-learning-web-service"></a>Azure Machine Learning-webservice ##

Met de webservice Azure Machine Learning, wordt een externe toepassing communiceert met een werkstroom Machine Learning model score in realtime. Aanroep van een Machine Learning-webservice retourneert tekstvoorspelling resultaten naar een externe toepassing. Als u een Machine Learning-webservice bellen, moet u een API-sleutel dat wordt gemaakt wanneer u een voorspelling implementeert doorgeven. De Machine Learning-webservice is gebaseerd op de REST, een keuze populaire architectuur voor web programming projecten.

Azure Machine Learning heeft twee soorten services:

- Aanvraag en respons Service (records) – een lage latentie, ten zeerste scalable service die biedt een interface voor de stateless modellen die zijn gemaakt en geïmplementeerd vanuit de Machine Learning-Studio.
- Batch Execution Service (BES) – een asynchrone service die scores per batch voor gegevensrecords.

Zie voor meer informatie over Machine Learning-webservices, [Deploy een Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Een Azure Machine Learning autorisatie sleutel ophalen ##

Wanneer u uw experiment implementeert, API sleutels gegenereerd voor de webservice. U kunt de toetsen ophalen uit verschillende locaties indienen.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>In de portal van Microsoft Azure Machine Learning-webservices

Meld u aan bij de portal van [Microsoft Azure Machine Learning-webservices](https://services.azureml.net) .

Naar de API-sleutel voor een webservice van nieuwe Machine Learning ophalen:

1. Klik op **Web Services** het bovenste menu in de portal Azure Machine Learning-webservices.
2. Klik op de webservice waarvoor u wilt de sleutel ophalen.
3. Klik op het bovenste menu op **verbruiken**.
4. Kopieer en sla de **Primaire sleutel**.


Naar de API-sleutel voor een webservice van klassieke Machine Learning ophalen:

1. Klik op **Klassieke webservices** het bovenste menu in de portal Azure Machine Learning-webservices.
2. Klik op de webservice waarmee u werkt.
3. Klik op het eindpunt waarvoor u wilt de sleutel ophalen.
3. Klik op het bovenste menu op **verbruiken**.
4. Kopieer en sla de **Primaire sleutel**.

## <a name="classic-web-service"></a>Klassieke webservice ##

 U kunt ook een sleutel voor een klassiek webservice ophalen uit Machine Learning Studio of de Azure-portal.

### <a name="machine-learning-studio"></a>Machine Learning Studio ###

1. Machine Learning Studio, klikt u op **WEB SERVICES** aan de linkerkant.
2. Klik op een webservice. De **API-sleutel** is op het tabblad **DASHBOARD** .

### <a name="azure-portal"></a>Azure-portal ###

1. Klik op **MACHINE LEARNING** aan de linkerkant.
2. Klik op de werkruimte waarin uw webservice zich bevindt.
3. Klik op **WEBSERVICES**.
4. Klik op een webservice.
5. Klik op een eindpunt. De "API-sleutel" is niet beschikbaar in de rechterbenedenhoek.

## <a id="connect"></a>Verbinding maken met een Machine Learning-webservice

U kunt verbinding maken met een Machine Learning-webservice met elke programmeertaal die ondersteuning biedt voor HTTP vergaderverzoeken en antwoorden. U kunt de voorbeelden in C#, Python en R weergeven van een help-pagina Machine Learning-webservice.

**Help-machine Learning-API** Machine Learning-API help wordt gemaakt wanneer u een webservice implementeren. Zie [Azure Machine Learning-scenario - webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md).
De help Machine Learning-API bevat informatie over een voorspelling webservice.

2. Klik op de webservice waarmee u werkt.
3. Klik op het eindpunt waarvoor u wilt weergeven van de pagina API Help.
3. Klik op het bovenste menu op **verbruiken**.
3. Klik op **help-API pagina** onder de aanvraag en respons of de Batch Execution eindpunten.

**Naar de weergave Machine Learning-API help voor een nieuwe webservice**

In de Azure Machine Learning-Web Services-Portal:

1. Klik op **WEB SERVICES** in het bovenste menu.
2. Klik op de webservice waarvoor u wilt de sleutel ophalen.

Klik op **verbruiken** voor de URI's voor de aanvraag-Reposonse en de Batch Execution Services en de voorbeeld-code in C#, R en Python.

Klik op **Swagger API** als u de documentatie Swagger op basis van de API's naam uit de opgegeven URI's.

### <a name="c-sample"></a>C#-voorbeeld ###

Als u wilt verbinden met een Machine Learning-webservice, gebruikt u een **HttpClient** ScoreData doorgeven. ScoreData bevat een FeatureVector, een n-dimensionaal vector van numerieke functies die de ScoreData vertegenwoordigt. U kunt verifiëren bij de Machine Learning-service met een API-sleutel.

Als u wilt verbinden met een Machine Learning-webservice, moet het **Microsoft.AspNet.WebApi.Client** NuGet-pakket zijn geïnstalleerd.

**Een installatiefout met Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**

1. Publiceren van de gegevensset downloaden uit UCI: volwassen 2 class gegevensset webservice.
2. Klik op **Extra** > **NuGet Package Manager** > **Package Manager-Console**.
2. Kies **- installatiepakket Microsoft.AspNet.WebApi.Client**.

**Om uit te voeren in het voorbeeld**

1. Publiceren "voorbeeld 1: Download dataset van UCI: volwassene 2 class gegevensset" experiment, deel van de siteverzameling van de steekproef Machine Learning.
2. ApiKey met de toets toewijzen uit een webservice. Zie **een Azure Machine Learning autorisatie sleutel ophalen** hierboven.
3. ServiceUri met de URI aanvragen toewijzen.


### <a name="python-sample"></a>Voorbeeld van Python ###

Als u wilt verbinden met een Machine Learning-webservice, gebruikt u het doorgeven van ScoreData **urllib2** -bibliotheek. ScoreData bevat een FeatureVector, een n-dimensionaal vector van numerieke functies die de ScoreData vertegenwoordigt. U kunt verifiëren bij de Machine Learning-service met een API-sleutel.


**Om uit te voeren in het voorbeeld**

1. Implementeren "voorbeeld 1: Download dataset van UCI: volwassene 2 class gegevensset" experiment, deel van de siteverzameling van de steekproef Machine Learning.
2. ApiKey met de toets toewijzen uit een webservice. Zie het gedeelte **krijgen een Azure Machine Learning autorisatie sleutel** aan het begin van dit artikel.
3. ServiceUri met de URI aanvragen toewijzen.
