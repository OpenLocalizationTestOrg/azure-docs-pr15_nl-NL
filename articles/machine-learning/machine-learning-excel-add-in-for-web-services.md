<properties
    pageTitle="Excel-invoegtoepassing voor Machine Learning-webservices | Microsoft Azure"
    description="Het gebruik van Azure Machine Learning-webservices rechtstreeks in Excel een code te schrijven."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-invoegtoepassing voor Azure Machine Learning-webservices

Excel kunt u gemakkelijk om te bellen webservices rechtstreeks zonder dat u een code te schrijven.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Stappen voor het gebruik van een bestaande webservice in de werkmap

1. Open het [Excel-voorbeeldbestand](http://aka.ms/amlexcel-sample-2), waarin de Excel-invoegtoepassing en gegevens over personen op de Titanic.
2. Kies de webservice door erop te klikken-"Titanic pensioen voorspelde (Excel-invoegtoepassing voorbeeld) [Score]" in dit voorbeeld.

    ![Selecteer webservice][01]

3. Hiermee gaat u naar de sectie **Predict** .  Deze werkmap al voorbeeldgegevens bevat, maar voor een lege werkmap kunt u een cel selecteren in Excel en klikt u op **voorbeeldgegevens gebruiken**.
4. Selecteer de gegevens met kopteksten en klik op het pictogram van invoergegevens bereik.  Zorg ervoor dat het selectievakje "Mijn gegevens hebben koppen" is ingeschakeld.
5. Voer onder **uitvoer**, nummer van de cel waar u de uitvoer wilt plaatsen, bijvoorbeeld "H1" hier.
6. Klik op **voorspellen**.

    ![Sectie voorspellen][02]

## <a name="steps-to-add-a-new-web-service"></a>Stappen voor het toevoegen van een nieuwe webservice

Een webservice implementeren of gebruikt u een bestaande webservice. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: implementeren van de Azure Machine Learning-webservice](machine-learning-walkthrough-5-publish-web-service.md).

De API-sleutel voor uw webservice krijgen. Waar u deze stap is afhankelijk van of u een klassieke Machine Learning-webservice van een webservice van nieuwe Machine Learning gepubliceerd.

**Een klassieke webservice gebruiken** 

1. Machine Learning Studio, klikt u op de sectie **WEB SERVICES** in het linkerdeelvenster en selecteer vervolgens de webservice.

    ![Studio, selecteer een webservice][04]

2. Kopieer de API-sleutel voor de webservice.

    ![Studio API-sleutel][05]

3. Klik op het tabblad **DASHBOARD** voor de webservice, klikt u op de koppeling van de **Aanvraag en respons** .
4. Zoek naar de sectie **URI aanvragen** .  Kopieer en sla de URL.

>[AZURE.NOTE] Nu is het mogelijk u zich aanmeldt bij de portal [Azure Machine Learning-webservices](https://services.azureml.net) de API-sleutel voor een webservice van klassieke Machine Learning verkrijgt.

**Een nieuwe webservice gebruiken**

1. Klik in de portal [Azure Machine Learning-webservices](https://services.azureml.net) klikt u op **Webservices**, selecteert u uw webservice. 
2. Klik op **gebruiken**.
3. Zoek naar de sectie **eenvoudige verbruik info** . Kopieer en sla de **Primaire sleutel** en de URL van de **Aanvraag en respons** .


## <a name="steps-to-add-a-new-web-service"></a>Stappen voor het toevoegen van een nieuwe webservice

1. Een webservice implementeren of gebruikt u een bestaande webservice. Zie voor meer informatie over het implementeren van een webservice [scenario stap 5: implementeren van de Azure Machine Learning-webservice](machine-learning-walkthrough-5-publish-web-service.md).
2. Klik op **gebruiken**.
3. Zoek naar de sectie **eenvoudige verbruik info** . Kopieer en sla de **Primaire sleutel** en de URL van de **Aanvraag en respons** .
2. In Excel, gaat u naar de sectie **Web Services** (als u zich in de sectie **Predict** , klikt u op de pijl-terug te gaan naar de lijst met webservices).

    ![Ga naar de Web-service selectie][03]
    
3. Klik op **webservice toevoegen**.
4. Plak de URL in het Excel-invoegtoepassing tekstvak **URL**het label.
5. Plak de API/primaire sleutel in het tekstvak het label van **API-sleutel**.
6. Klik op **toevoegen**.

    ![URL en API-sleutel voor een klassiek webservice.][06]

10. Als u wilt de webservice gebruikt, volgt u de voorgaande aanwijzingen, "Stappen voor het gebruik van een bestaande-webservice."

## <a name="sharing-your-workbook"></a>Delen van uw werkmap

Als u de werkmap opslaat, wordt ook de API/primaire sleutel voor de Web-services die u hebt toegevoegd opgeslagen. Dat betekent dat u moet de werkmap alleen delen met personen die u vertrouwt.

Vragen te stellen in de volgende sectie van de opmerking of op onze [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
