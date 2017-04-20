<properties
    pageTitle="Gegevens importeren in Machine Learning Studio uit een lokaal bestand | Microsoft Azure"
    description="Hoe u de trainingsgegevens Azure Machine Learning Studio importeren uit een lokaal bestand."
    keywords="gegevensindeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren"
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
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Uw trainingsgegevens in Azure Machine Learning Studio uit een lokaal bestand importeren

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Als u uw eigen gegevens gebruikt in Machine Learning Studio, kunt u een gegevensbestand tijd vooraf uploaden van uw lokale harde schijf maken van een module gegevensset in uw werkruimte. 


## <a name="import-data-from-a-local-file"></a>Gegevens importeren uit een lokaal bestand

U kunt gegevens importeren uit een lokale harde schijf door het volgende te doen:

1. Klik op **+ Nieuw** onderaan in het venster Machine Learning Studio.
2. Selecteer **GEGEVENSSET** en **FROM lokaal bestand**.
3. Blader in het dialoogvenster **een nieuwe gegevensset uploaden** naar het bestand dat u wilt uploaden
4. Voer een naam, het gegevenstype identificeren en voer desgewenst een beschrijving. Een beschrijving wordt aanbevolen: Hiermee kunt u eventuele kenmerken over de gegevens die u onthouden wilt wanneer u met de gegevens in de toekomst opnemen.
5. Het selectievakje **Dit is de nieuwe versie van een bestaande gegevensset** kunt u een bestaande gegevensset bijwerken met nieuwe gegevens. Dit selectievakje klikt u op en voer vervolgens de naam van een bestaande gegevensset.

Tijdens het uploaden ziet u een bericht dat het bestand wordt geüpload. Uploaden tijd afhankelijk van de grootte van uw gegevens en de snelheid van uw verbinding met de service.
Als u weet dat het bestand wordt erg lang duren, kunt u andere dingen die u binnen Machine Learning Studio kunt doen, terwijl u wacht. De browser te sluiten treedt echter het uploaden van gegevens mislukt.

Als uw gegevens is geüpload, wordt opgeslagen in een module gegevensset en is beschikbaar voor een experiment in uw werkruimte.
Als u een experiment aan het bewerken bent, kunt u de gegevenssets die u hebt gemaakt in de lijst **Mijn gegevenssets** onder de lijst **Opgeslagen gegevenssets** in het palet module kunt vinden. U kunt slepen en neerzetten van de gegevensset op het tekenpapier experiment wanneer u wilt gebruiken van de gegevensverzameling voor de verdere analyses en machine learning.



