<properties
   pageTitle="Gegevens uit Lake gegevensopslag registreren in de gegevenscatalogus Azure | Microsoft Azure"
   description="Gegevens uit Lake gegevensopslag registreren in Azure-gegevenscatalogus"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Gegevens uit Lake gegevensopslag registreren in Azure-gegevenscatalogus

In dit artikel leert u hoe u Azure Lake gegevensopslag integreren met Azure-gegevenscatalogus dat uw gegevens makkelijker binnen een organisatie kan worden gevonden door integratie met de gegevenscatalogus. Zie voor meer informatie over catalogiseren gegevens, [Azure-gegevenscatalogus](../data-catalog/data-catalog-what-is-data-catalog.md). Als u wilt weten over scenario's waarin u de gegevenscatalogus kunt gebruiken, raadpleegt u [veelvoorkomende scenario's voor een Azure-gegevenscatalogus](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Uw abonnement op Azure inschakelen** voor openbare voorbeeld van gegevens Lake Store. Zie de [instructies](data-lake-store-get-started-portal.md#signup).

- **De gegevensopslag Lake azure-account**. Volg de instructies bij [aan de slag met Azure Lake gegevensopslag met behulp van de Azure-Portal](data-lake-store-get-started-portal.md). Voor deze zelfstudie laat ons maken een Lake gegevensopslag account **datacatalogstore**genoemd. 

    Nadat u het account hebt gemaakt, een verzameling voorbeeldgegevens naar hebt geüpload. Voor deze zelfstudie laat ons de CSV-bestanden onder de map **AmbulanceData** in de [Azure gegevens Lake cijfer bibliotheek](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/)uploaden. U kunt verschillende clients, zoals [Azure opslag Explorer](http://storageexplorer.com/)gebruiken om gegevens te uploaden naar een container blob.

- **Azure gegevens catalogus**. Uw organisatie moet al een Azure-gegevenscatalogus gemaakt voor uw organisatie. Catalogus met slechts één is voor elke organisatie toegestaan.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register voor gegevensopslag Lake als een bron voor de catalogus met gegevens

>[AZURE.VIDEO adcwithadl] 

1. Ga naar `https://azure.microsoft.com/services/data-catalog`, en klik op **aan de slag**.

2. Meld u aan bij de portal catalogus van Azure-gegevens en klik op **publiceren gegevens**.

    ![Een gegevensbron registreert] (./media/data-lake-store-with-data-catalog/register-data-source.png "Een gegevensbron registreert")

3. Klik op de volgende pagina op **Toepassing starten**. Hiermee wordt de manifest bestand van de toepassing op uw computer downloaden. Dubbelklik op het manifest bestand om de toepassing te starten.

4. Klik op de pagina Welcome op **aanmelden**en voert u uw referenties.

    ![Welkomstscherm] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Welkomstscherm")

5. Klik op de pagina Selecteer een gegevensbron, selecteert u **Lake van Azure-gegevens**en klik op **volgende**.

    ![Selecteer gegevensbron] (./media/data-lake-store-with-data-catalog/select-source.png "Selecteer gegevensbron")

6. Geef de naam van het Lake gegevensopslag-account dat u wilt registreren in gegevenscatalogus op de volgende pagina. Laat de andere opties als standaard en klik op **verbinding maken**.

    ![Verbinding maken met gegevensbron] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Verbinding maken met gegevensbron")

7. De volgende pagina kan worden onderverdeeld in de volgende segmenten.

    een. Het vak **Server hiërarchie** staat de mapstructuur voor gegevensopslag Lake-account. **$Root** staat voor de hoofdsite van de account Lake gegevensopslag en **AmbulanceData** voor de map die is gemaakt in de hoofdmap van het account voor gegevensopslag Lake.

    b. Het vak **beschikbare objecten** bevat de bestanden en mappen onder de map **AmbulanceData** .

    c. **Objecten geregistreerde vak** bevat de bestanden en mappen die u wilt registreren in Azure-gegevenscatalogus.

    ![Structuur van de gegevens weergeven] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Structuur van de gegevens weergeven")

8. Voor deze zelfstudie moet u alle bestanden in de adreslijst registreren. Voor die, klikt u op de knop (![objecten verplaatsen](./media/data-lake-store-with-data-catalog/move-objects.png "objecten verplaatsen")) om alle bestanden verplaatsen naar **objecten worden geregistreerd** in. 

    Omdat de gegevens worden geregistreerd in een organisatiebrede gegevenscatalogus, is een aanpak aanbevolen om toe te voegen sommige metagegevens die u later gebruiken kunt om de gegevens snel te vinden. U kunt bijvoorbeeld een e-mailadres toevoegen voor de eigenaar van de gegevens (bijvoorbeeld een die de gegevens wordt geüpload) of een markering toevoegen om te bepalen de gegevens. De schermopname hieronder ziet u een tag die we aan de gegevens toevoegen.

    ![Structuur van de gegevens weergeven] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Structuur van de gegevens weergeven")

    Klik op **registreren**.

8. De volgende schermafbeelding wordt aangegeven dat de gegevens in de gegevenscatalogus is geregistreerd.

    ![Registratie voltooid] (./media/data-lake-store-with-data-catalog/registration-complete.png "Structuur van de gegevens weergeven")

9. Klik op **Weergave-Portal** als u wilt teruggaan naar de gegevenscatalogus-portal en controleer of u kunt nu de geregistreerde gegevens openen vanuit de portal. Als u wilt zoeken de gegevens, kunt u de tag die u tijdens het registreren van de gegevens hebt gebruikt.

    ![Gegevens zoeken in catalogus] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Gegevens zoeken in catalogus")

10. U kunt nu bewerkingen zoals het toevoegen van aantekeningen en documentatie aan de gegevens uitvoeren. Zie de volgende koppelingen voor meer informatie.
    * [Gegevensbronnen in de gegevenscatalogus van aantekeningen voorzien](../data-catalog/data-catalog-how-to-annotate.md)
    * [Document-gegevensbronnen in de gegevenscatalogus](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Zie ook

* [Gegevensbronnen in de gegevenscatalogus van aantekeningen voorzien](../data-catalog/data-catalog-how-to-annotate.md)
* [Document-gegevensbronnen in de gegevenscatalogus](../data-catalog/data-catalog-how-to-documentation.md)
* [Gegevensopslag Lake integreren met andere services van Azure](data-lake-store-integrate-with-other-services.md)
