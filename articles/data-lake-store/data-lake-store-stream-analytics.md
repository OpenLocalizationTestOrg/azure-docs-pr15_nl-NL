<properties
   pageTitle="Gegevens uit Stream Analytics streamen in Lake gegevensopslag | Azure"
   description="Azure Stream Analytics gebruikt om gegevens van de stream naar Azure Lake gegevensopslag"
   services="data-lake-store,stream-analytics" 
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
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Stream gegevens van Azure de Blob Storage in Lake gegevensopslag door middel van Azure Stream analyses

In dit artikel leert u hoe u Azure Lake gegevensopslag gebruikt als een uitvoer voor een taak Azure Stream Analytics. In dit artikel ziet u een eenvoudige scenario die leest gegevens van een opslag van Azure blob (input) en schrijft de gegevens naar Lake gegevensopslag (uitvoer).

>[AZURE.NOTE] Op dit moment kan wordt maken en de configuratie van Lake gegevensopslag uitvoer van Stream analysegegevens alleen ondersteund in de [Klassieke Azure-Portal](https://manage.windowsazure.com). Sommige onderdelen van deze zelfstudie wordt daarom gebruikt in de klassieke Azure-Portal.

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Uw abonnement op Azure inschakelen** voor openbare voorbeeld van gegevens Lake Store. Zie de [instructies](data-lake-store-get-started-portal.md#signup).

- **Opslag van azure-account**. U kunt een container blob via dit account wilt gebruiken voor de invoer van gegevens voor een taak Stream Analytics. Voor deze zelfstudie wordt ervan uitgegaan dat u een naam **datalakestoreasa** en een container in het account genoemd **datalakestoreasacontainer**opslag-account maken. Nadat u de container hebt gemaakt, een voorbeeld van gegevensbestand naar hebt geüpload. U kunt een voorbeeld van gegevensbestand krijgen van de [Azure gegevens Lake cijfer opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). U kunt verschillende clients, zoals [Azure opslag Explorer](http://storageexplorer.com/)gebruiken om gegevens te uploaden naar een container blob.

    >[AZURE.NOTE] Als u het account van de Azure-Portal maken, zorg er dan voor dat u deze hebt gemaakt met het **klassieke** implementatie-model. Dit zorgt ervoor dat het account opslag kan worden geopend in de klassieke Portal Azure omdat dit we gebruiken een Stream Analytics-taak wilt maken. Zie voor instructies over het maken van een account voor de opslag van de Azure-Portal met de implementatie klassieke, [een Azure Storage-account maken](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Of u kunt een opslag-account maken in de klassieke Azure-Portal.

- **De gegevensopslag Lake azure-account**. Volg de instructies bij [aan de slag met Azure Lake gegevensopslag met behulp van de Azure-Portal](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

U beginnen met het maken van een taak Stream analyses met een invoerbron en de uitvoerbestemming van een. De bron is een container Azure blob en de bestemming is Lake gegevensopslag voor deze zelfstudie.

1. Aanmelden bij de [Portal van Azure klassieke](https://manage.windowsazure.com).

2. In de linkerbenedenhoek van het scherm, klikt u op **Nieuw**, **Data Services**, **Stream analyses**, **Snelle te maken**. Geef de waarden zoals hieronder wordt weergegeven en klik vervolgens op **Stream Analytics-taak maken**.

    ![Een Stream Analytics-taak maken] (./media/data-lake-store-stream-analytics/create.job.png "Een taak Stream analyses maken")

## <a name="create-a-blob-input-for-the-job"></a>Een Blob invoer voor de taak maken

1. Open de pagina voor de Stream Analytics-taak, klik op het tabblad **invoeritems** en klik vervolgens op **een invoertaal toevoegen** om een wizard te starten.

2. Klik op de pagina **toevoegen een invoer voor uw werk** **gegevensstroom**selecteren en klik vervolgens op de pijl naar rechts.

    ![Een invoer voor uw werk toevoegen] (./media/data-lake-store-stream-analytics/create.input.1.png "Een invoer voor uw werk toevoegen")

3. Klik op de pagina **toevoegen een gegevensstroom aan uw project** **-blobopslag**selecteren en klik vervolgens op de pijl naar rechts.

    ![Een gegevensstroom aan de taak toevoegen] (./media/data-lake-store-stream-analytics/create.input.2.png "Een gegevensstroom aan de taak toevoegen")

4. Klik op de pagina **Instellingen voor Blob storage** door informatie op te geven voor de blobopslag die u als invoergegevens bron gebruiken wilt.

    ![Geef de blob storage-instellingen] (./media/data-lake-store-stream-analytics/create.input.3.png "Geef de blob storage-instellingen")

    * **Enter een invoer alias**. Dit is een unieke naam die u voor de taak invoeren opgeeft.
    * **Selecteer een account opslag**. Controleer of de opslag-account is in hetzelfde gebied, als de taak Stream Analytics of u extra kosten van het verplaatsen van gegevens tussen regio's in rekening worden gebracht.
    * **Een container opslag opgeven**. U kunt een nieuwe container maken of selecteren van een bestaande container.

    Klik op de pijl naar rechts.

5. Klik op de pagina **Instellingen voor serialisatie** de serialiseringsindeling instellen als **CSV**, scheidingsteken als **tabblad**en codering als **UTF8**, en klik op het vinkje.

    ![Geef de serialisatie-instellingen] (./media/data-lake-store-stream-analytics/create.input.4.png "Geef de serialisatie-instellingen")

6. Als u klaar bent met de wizard, de invoer blob, klik op het tabblad **invoer** worden toegevoegd en **OK**moet worden weergegeven in de kolom **diagnose** . U kunt de verbinding met de invoer ook expliciet testen met behulp van de knop **Verbinding testen** onderaan.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Een uitvoer Lake gegevensopslag voor de taak maken

1. Open de pagina voor de Stream Analytics-taak, klik op het tabblad **uitvoer van** en klik vervolgens op **een uitvoer toevoegen** om een wizard te starten.

2. Klik op de pagina **toevoegen een uitvoer aan uw project** **Lake gegevensopslag**selecteren en klik vervolgens op de pijl naar rechts.

    ![Een uitvoer op uw werk toevoegen] (./media/data-lake-store-stream-analytics/create.output.1.png "Een uitvoer op uw werk toevoegen")

3. Klik op de pagina **autoriseren verbinding** als u al een account voor gegevensopslag Lake hebt gemaakt, klikt u op **Nu Autoriseer**. Anders, klikt u op **Nu registreren** om een nieuw account te maken. Voor deze zelfstudie laat ons is ervan uitgegaan dat u al een Lake gegevensopslag-account gemaakt (zoals die worden genoemd in de vereiste). U wordt automatisch gemachtigd met de referenties waarmee u aangemeld bij de Portal van Azure-klassiek.

    ![Autoriseer Lake gegevensopslag] (./media/data-lake-store-stream-analytics/create.output.2.png "Autoriseer Lake gegevensopslag")

4. Klik op de pagina **Instellingen voor gegevens Lake Store** Voer de gegevens zoals wordt weergegeven in de onderstaande schermopname.

    ![Instellingen opgeven Lake gegevensopslag] (./media/data-lake-store-stream-analytics/create.output.3.png "Instellingen opgeven Lake gegevensopslag")

    * **Enter een uitvoeralias**. Dit is een unieke naam die u voor de taak-uitvoer opgeeft.
    * **Geef een Lake gegevensopslag-account**. U moet al hebt gemaakt, zoals in de vereiste is vermeld.
    * **Geef een pad voorvoegselpatroon**. Dit is nodig om aan te geven van de uitvoerbestanden die naar Lake gegevensopslag worden geschreven door de Stream Analytics-taak. Omdat de titels van uitvoer geschreven door de taak in een GUID-indeling, helpen inclusief een voorvoegsel de geschreven uitvoer identificeren. Als u wilt opnemen van een datum- en -tijdstempel als onderdeel van het voorvoegsel controleert u of u opnemen `{date}/{time}` in het voorvoegselpatroon. Als u dit opneemt, de **Datum **en **Tijd-indeling** -velden zijn ingeschakeld en kunt u de opmaak van keuze.

    Klik op de pijl naar rechts.

5. Klik op de pagina **Instellingen voor serialisatie** de serialiseringsindeling instellen als **CSV**, scheidingsteken als **tabblad**en codering als **UTF8**, en klik op het vinkje.

    ![Geef de uitvoerindeling] (./media/data-lake-store-stream-analytics/create.output.4.png "Geef de uitvoerindeling")

6. Als u klaar bent met de wizard, de uitvoer Lake gegevensopslag, klik op het tabblad **uitvoer** worden toegevoegd en **OK**moet worden weergegeven in de kolom **diagnose** . U kunt de verbinding met de uitvoer ook expliciet testen met behulp van de knop **Verbinding testen** onderaan.

## <a name="run-the-stream-analytics-job"></a>De taak Stream analyses uitvoeren

Als u wilt een taak Stream analyses uitvoeren, moet u een query uitvoeren op het tabblad Query. Voor deze zelfstudie kunt u de voorbeeldquery uitvoeren door de tijdelijke aanduidingen vervangen door de taak invoer en uitvoer aliassen, zoals wordt weergegeven in de onderstaande schermopname.

![Query uitvoeren] (./media/data-lake-store-stream-analytics/run.query.png "Query uitvoeren")

Klik op **Opslaan** vanaf de onderkant van het scherm en klik vervolgens op **starten**. Klik in het dialoogvenster Selecteer **Aangepaste tijd**en selecteer vervolgens een datum in het verleden, zoals **1/1/2016**. Klik op het vinkje om te starten van de taak. Het kan enkele minuten duren om de taak te starten.

![Tijd voor taak instellen] (./media/data-lake-store-stream-analytics/run.query.2.png "Tijd voor taak instellen")

Nadat de taak wordt gestart, klikt u op het tabblad **Beeldscherm** om te zien hoe de gegevens is verwerkt.

![Monitor met een taak] (./media/data-lake-store-stream-analytics/run.query.3.png "Monitor met een taak")

Tot slot kunt u de [Azure-Portal](https://portal.azure.com) naar uw account voor gegevensopslag Lake opent en controleer of de gegevens in het account is geschreven.

![Verifiëren uitvoer] (./media/data-lake-store-stream-analytics/run.query.4.png "Verifiëren uitvoer")

Klik in het deelvenster Data Explorer u ziet dat de uitvoer is geschreven naar een map als opgegeven in de gegevensopslag Lake uitvoerinstellingen (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Zie ook

* [Maken van een cluster HDInsight als u wilt gebruiken Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
