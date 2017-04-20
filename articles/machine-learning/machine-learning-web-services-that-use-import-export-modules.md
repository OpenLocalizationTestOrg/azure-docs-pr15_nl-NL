<properties
    pageTitle="Implementatie van Azure ML-webservices die gebruikmaken van gegevens importeren en exporteren van gegevens modules | Microsoft Azure"
    description="Informatie over het gebruik van de gegevens importeren en exporteren van gegevens modules verzenden en ontvangen van gegevens uit een webservice."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Azure ML-webservices die gebruikmaken van gegevens importeren en exporteren van gegevens modules implementeren 

Wanneer u een blog experiment maakt, toevoegen u meestal een service web en uitvoer. Wanneer u het experiment implementeert, kunnen de consumenten verzenden en ontvangen van gegevens uit de webservice tot en met de invoer en uitvoer. Voor sommige-toepassingen, van een consument gegevens uit een gegevensfeed beschikbaar of al bevinden zich in een externe gegevensbron zoals Azure-blobopslag. In dat geval hoeven ze niet lezen en schrijven van gegevens met behulp van web service invoer en uitvoer. Ze kunnen, in plaats daarvan de Batch Execution Service (BES) gebruiken om te lezen van gegevens uit de gegevensbron met behulp van een module gegevens importeren en de score resultaten schrijven naar een andere locatie met een module gegevens exporteren.

De gegevens importeren en exporteren gegevens modules, kan lezen en schrijven voor een aantal gegevens locaties zoals een Web-URL via HTTP, een Query component, een Azure SQL-database, Azure-tabelopslag, Azure-blobopslag, een Gegevensfeed opgeven of een on-premises SQL-database.

In dit onderwerp wordt gebruikt de "voorbeeld 5: trein, Test, evalueren voor binaire indeling: volwassen gegevensset" voorbeeld en wordt ervan uitgegaan de gegevensset al is geladen in een Azure SQL-tabel met de naam censusdata.

## <a name="create-the-training-experiment"></a>Het experiment training maken 
 
Als u opent de "voorbeeld 5: trein, Test, evalueren voor binaire indeling: volwassen gegevensset" steekproef site gebruikmaakt van de steekproef volwassen telling inkomsten binaire indeling gegevensset. En het experiment in het tekenpapier wordt lijken op de volgende afbeelding.

![Eerste configuratie van het experiment.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

De gegevens uit de tabel Azure SQL lezen:

1.  Verwijder de module gegevensset.
2.  Typ in het zoekvak onderdelen importeren.
3.  Toevoegen een module *Gegevens importeren* naar het tekenpapier experiment uit de lijst met resultaten.
4.  Verbinding met het maken van de uitvoer van de module *Gegevens importeren* de invoer van de module *Wissen.Control ontbrekende gegevens* .
5.  Selecteer in het deelvenster Eigenschappen, **Azure SQL-Database** in de vervolgkeuzelijst van de **Gegevensbron** .
6.  Voer de vereiste gegevens voor de database in de **naam van de Database-server**, **de naam van de Database**, **gebruikersnaam**en **wachtwoord** velden.
7.  Voer de volgende query in het veld Database query.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  Aan de onderkant van het experiment canvas, klikt u op **uitvoeren**.

## <a name="create-the-predictive-experiment"></a>Het blog experiment maken

De volgende die het instellen van het blog experiment waaruit u uw webservice gaat implementeren.

1.  Klik onderaan het tekenpapier experiment op **Webservice instellen** en selecteer **Blog webservice (aanbevolen)**.
2.  De *Invoer van de Web-Service* en *Web Service uitvoer modules* verwijderen uit het blog experiment. 
3.  Typ in het zoekvak onderdelen exporteren.
4.  Toevoegen een module *Gegevens exporteren* naar het tekenpapier experiment uit de lijst met resultaten.
5.  Verbinding met het maken van de uitvoer van de module *Score Model* de invoer van de module *Gegevens exporteren* . 
6.  Selecteer in het deelvenster Eigenschappen, **Azure SQL-Database** in de vervolgkeuzelijst van de bestemming gegevens.
7.  Voer de vereiste gegevens voor de database in de **naam van de Database-server**, **databasenaam**, **Server-account gebruikersnaam**en **wachtwoord voor een gebruikersaccount Server** velden.
8.  Typ in het veld **door komma's gescheiden lijst met kolommen worden opgeslagen** Labels van een score voorzien.
9.  Typ in het **naamveld van een tabel gegevens**dbo. ScoredLabels. Als de tabel niet bestaat, wordt deze gemaakt wanneer het experiment wordt uitgevoerd of de webservice wordt genoemd.
10. Typ in het veld **door komma's gescheiden lijst met kolommen gegevenstabel** ScoredLabels.

Wanneer u een toepassing die de uiteindelijke webservice u belt schrijven, is het raadzaam om op te geven van een andere invoer query of de doeltabel tijdens runtime. Als u wilt deze invoer en uitvoer configureren, kunt u de functie Web Service Parameters voor het instellen van de eigenschap *Gegevens importeren* module *gegevensbron* en de eigenschap *Gegevens exporteren* modus data bestemming.  Voor meer informatie over Web Service Parameters, raadpleegt u de [vermelding van de AzureML Web Service Parameters](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) op de Cortana Intelligence en Machine Learning-Blog.

Parameters van de webservice voor de query importeren en de doeltabel configureren:

1.  Klik op het pictogram boven in het deelvenster Eigenschappen voor de module *Gegevens importeren* , rechtsboven in het veld van de **databasequery's** en selecteer **instellen als de parameter webservice web**.
2.  Klik op het pictogram boven in het deelvenster Eigenschappen voor de module *Gegevens exporteren* , rechtsboven in het veld **gegevens van de naam van een tabel** en selecteer **instellen als de parameter webservice web**.
3.  Klik op databasequery's en wijzig de naam Query onderaan in het deelvenster *Gegevens exporteren* module eigenschappen, in het gedeelte **Web Service Parameters** .
4.  Klik op **de naam van een tabel gegevens** en wijzig de naam in **tabel**.

Wanneer u klaar bent, uw experiment moet er ongeveer uit de volgende afbeelding.
 
![Definitief uiterlijk van een experiment.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

U kunt nu het experiment implementeren als een webservice.

## <a name="deploy-the-web-service"></a>De webservice implementeren 
U kunt implementeren naar een klassieke of nieuwe webservice.

### <a name="deploy-a-classic-web-service"></a>Een klassieke webservice implementeren

Te implementeren als een klassieke webservice en maken van een toepassing om te kunnen maken:

1.  Aan de onderkant van het papier experiment, klikt u op uitvoeren.
2.  Wanneer de klaar is voltooid, klikt u op **Webservice implementeren** en selecteer **Webservice implementeren [klassieke]**.
3.  Zoek op het dashboard van de service web uw API-sleutel. Kopiëren en opslaan om later te gebruiken.
4.  Klik op de koppeling **Batch Execution** om de pagina API Help te openen in de tabel **Standaard eindpunt** .
5.  Visual Studio, een C#-console-toepassing te maken.
6.  Zoek op de pagina API Help voor de sectie **Steekproef Code** onder aan de pagina.
7.  Kopieer en plak de code C# voorbeeld in het bestand Program.cs en verwijder alle verwijzingen naar de blobopslag.
8.  De waarde van de variabele *apiKey* bijwerken met de API-sleutel eerder hebt opgeslagen.
9.  Zoek de declaratie van de aanvraag en bijwerken van de waarden van Web Service Parameters die worden doorgegeven aan de *Gegevens importeren* en *Exporteren van gegevens* modules. U wordt in dit geval gebruikt de oorspronkelijke query, maar de naam van een nieuwe tabel definiëren.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Voer de toepassing. 

Na afloop van de uitvoeren, wordt een nieuwe tabel toegevoegd aan de database met de score resultaten.

### <a name="deploy-a-new-web-service"></a>Een nieuwe webservice implementeren

Te implementeren als een nieuwe webservice en maken van een toepassing om te kunnen maken:

1.  Aan de onderkant van het experiment canvas, klikt u op **uitvoeren**.
2.  Wanneer de klaar is voltooid, klikt u op **Webservice implementeren** en selecteer **Implementeren webservice [Nieuw]**.
3.  Klik op de pagina Experiment implementeren Voer een naam voor uw webservice en selecteert u een abonnement prijzen en klik op **Deploy**.
4.  Klik op de pagina **Quickstart** op **verbruiken**.
5.  Klik in de sectie **Voorbeeldcode** op **Batch**.
6.  Visual Studio, een C#-console-toepassing te maken.
7.  Kopieer en plak de code C# voorbeeld in het bestand Program.cs.
8.  De waarde van de variabele *apiKey* bijwerken met de **Primaire sleutel** in de sectie **eenvoudige verbruik van gegevens** .
9.  Zoek de declaratie *scoreRequest* en bijwerken van de waarden van Web Service Parameters die worden doorgegeven aan de *Gegevens importeren* en *Exporteren van gegevens* modules. U wordt in dit geval gebruikt de oorspronkelijke query, maar de naam van een nieuwe tabel definiëren.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Voer de toepassing. 
 

