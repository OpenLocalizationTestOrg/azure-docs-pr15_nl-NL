<properties
    pageTitle="Maken van de eindpunten van een webservice in Machine Learning | Microsoft Azure"
    description="Maken van de eindpunten van een webservice in Azure Machine Learning"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Eindpunten maken

>[AZURE.NOTE] In dit onderwerp wordt beschreven technieken voor een klassiek webservice.

Wanneer u webservices die u doorsturen naar uw klanten verkoopt maakt, moet u ervaren modellen bieden aan elke klant die nog steeds zijn gekoppeld aan het experiment waaruit de webservice hebt gemaakt. Daarnaast moeten updates van het experiment worden toegepast selectief op een eindpunt zonder dat de aanpassingen worden overschreven.

Hiervoor kunt Azure Machine Learning u meerdere eindpunten invoeren voor een webservice geïmplementeerd. Elk eindpunt in de webservice is onafhankelijk gericht, vertraagd en beheerd. Elk eindpunt is een unieke URL en autorisatie opgeven die u naar uw klanten distribueren kunt.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Eindpunten toevoegen aan een webservice

Er zijn drie manieren een eindpunt toevoegen aan een webservice.

* Via programmacode
* Via de portal Azure Machine Learning-webservices
* Hoewel de Azure klassieke portal

Nadat het eindpunt is gemaakt, kunt u deze via synchroon API's, batch API's gebruiken en excel-werkbladen. Naast het toevoegen van eindpunten tot en met deze gebruikersinterface, kunt u ook de eindpunt Management API's gebruiken om toe te voegen via programmacode eindpunten.

 >[AZURE.NOTE] Als u extra eindpunten aan de webservice hebt toegevoegd, kunt u de standaardeindpunt niet verwijderen.

## <a name="adding-an-endpoint-programmatically"></a>Een eindpunt toevoegen via programmacode

U kunt een eindpunt toevoegen aan uw webservice programmacode met de code van de steekproef [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) .

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Een eindpunt met behulp van de portal Azure Machine Learning-webservices toevoegen

1. Machine Learning Studio, klik op de kolom linker navigatiegedeelte op webservices.
2. Klik op **beheren eindpunten**onderaan in het dashboard van de Web-service. De portal Azure Machine Learning-webservices wordt geopend naar de pagina eindpunten voor de webservice.
3. Klik op **Nieuw**.
4. Typ een naam en beschrijving voor de nieuwe eindpunt. Eindpunt namen moeten 24 teken of minder in lengte en moeten bestaan uit een kleine letters alfabetten of getallen. Selecteer het niveau voor logboekregistratie en of voorbeeldgegevens is ingeschakeld. Voor meer informatie over logboekregistratie, Zie [inschakelen van logboekregistratie voor Machine Learning-webservices](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Een eindpunt met behulp van de Azure klassieke portal toevoegen


1. Meld u aan bij de [portal van Azure klassieke](http://manage.windowsazure.com), klikt u op **Machine Learning** in de linkerkolom. Klik op de werkruimte waarin de webservice waarin u geïnteresseerd bent.

    ![Navigeer naar de werkruimte](./media/machine-learning-create-endpoint/figure-1.png)

2. Klik op **webservices**.

    ![Navigeer naar webservices](./media/machine-learning-create-endpoint/figure-2.png)

3. Klik op de webservice waarmee die u bent geïnteresseerd om de lijst met beschikbare eindpunten weer te geven.

    ![Ga naar het eindpunt](./media/machine-learning-create-endpoint/figure-3.png)

4. Klik onder aan de pagina, op **Eindpunt toevoegen**. Typ een naam en beschrijving, ervoor zorgen dat er geen andere eindpunten met dezelfde naam in deze webservice. Laat het niveau van de vertraging met de standaardwaarde, tenzij er geen speciale vereisten. Zie voor meer informatie over het beperken, [Schaalbaarheid API-eindpunten](machine-learning-scaling-webservice.md).

    ![Eindpunt maken](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Volgende stappen

[Hoe u een gepubliceerde Azure Machine Learning-webservice in beslag nemen](machine-learning-consume-web-services.md).
