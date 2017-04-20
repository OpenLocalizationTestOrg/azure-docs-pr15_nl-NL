<properties
    pageTitle="Wizard gegevens Factory-Azure kopiëren | Microsoft Azure"
    description="Meer informatie over het gebruik van de Wizard gegevens Factory Azure kopiëren gegevens kopiëren van ondersteunde gegevensbronnen naar sinks."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="spelluru"/>

# <a name="azure-data-factory---copy-wizard"></a>Azure gegevens Factory - Wizard kopiëren
De Wizard Azure gegevens Factory kopiëren is om te vergemakkelijken het proces van het ingesting gegevens, dat wil meestal een eerste stap in een scenario voor end-to-end gegevens-integratie zeggen. Bij gebruik van de Wizard Azure gegevens Factory kopiëren, hoeft u niet voor meer informatie over eventuele JSON definities voor gekoppelde services, gegevenssets en pijpleidingen. Echter nadat u de stappen in de wizard hebt voltooid, de wizard maakt automatisch een pijplijn gegevens uit de geselecteerde gegevensbron kopiëren naar de geselecteerde bestemming. Bovendien de Wizard kopiëren helpt u bij het valideren van de gegevens op het moment van (cocreatie), wordt bij die veel van uw tijd opslaat, met name wanneer u zijn ingesting gegevens voor de eerste keer uit de gegevensbron. Start de Wizard kopiëren, klikt u op de tegel **gegevens kopiëren** in de startpagina van uw gegevens factory.

![Wizard kopiëren](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Een intuïtieve wizard voor het kopiëren van gegevens
Met deze wizard kunt u eenvoudig Verplaats gegevens van een grote verscheidenheid aan bronnen naar bestemmingen in minuten. Nadat u hebt uitgevoerd de wizard, is een pijplijn met de activiteit in een kopie automatisch voor u gemaakt samen met afhankelijke gegevens Factory-entiteiten (gekoppelde services en gegevenssets). Geen extra stappen vereist voor het maken van de pijplijn.   

![Gegevensbron selecteren](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Zie [Wizard kopiëren zelfstudie](data-factory-copy-data-wizard-tutorial.md) artikel stapsgewijze instructies voor het maken van een steekproef pijplijn om te kopiëren gegevens uit een Azure blob aan een tabel Azure SQL-Database. 

De wizard is ontworpen met grote gegevens mee vanaf het begin. Het is eenvoudig en efficiënt naar gegevens Factory-pijpleidingen die honderden mappen, bestanden of tabellen met de wizard gegevens kopiëren verplaatsen van de auteur. De wizard ondersteunt de volgende drie functies: voorbeeld van automatische gegevens, schema vastleggen en toewijzen en filteren van gegevens. 

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens 
De wizard kopiëren kunt u bekijken van een deel van de gegevens uit de geselecteerde gegevensbron voor u wilt controleren of de gegevens is de juiste gegevens die u wilt kopiëren. Als de brongegevens zich in een tekstbestand, parseert de wizard kopiëren bovendien het tekstbestand meer rij en kolomscheidingstekens en schema automatisch. 

![Opmaakinstellingen van bestand](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schema vastleggen en toewijzen 
Het schema van invoergegevens mogelijk niet overeenkomen met het schema van uitvoergegevens in sommige gevallen. In dit scenario moet u kolommen uit de bron-schema naar kolommen uit het schema bestemming toewijzen. 

De wizard kopiëren worden automatisch kolommen in het schema bron aan kolommen in het schema bestemming toegewezen. U kunt de toewijzingen overschrijven met behulp van de vervolgkeuzelijsten (of) opgeven of een kolom worden overgeslagen moet tijdens het kopiëren van de gegevens.   

![Schematoewijzing](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filteren van gegevens  
De wizard kunt u filteren brongegevens om alleen de gegevens die moeten worden gekopieerd naar de bestemming/sink gegevensopslag te selecteren. Filteren Hiermee reduceert u het volume van de gegevens die moeten worden gekopieerd naar de gegevensopslag sink en kunnen daarom verbetert de doorvoer van de bewerking kopiëren. Deze biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van SQL query taal (of) bestanden in een map Azure blob met behulp van [gegevens Factory-functies en variabelen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filteren van gegevens in een database  
In het voorbeeld wordt de SQL-query wordt gebruikt de `Text.Format` functie en `WindowStart` variabele. 

![Valideer de expressies](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filteren van gegevens in een map met Azure blob
U kunt variabelen in de map gebruiken om gegevens te kopiëren van een map die wordt bepaald gedurende runtime op basis van [variabelen](data-factory-functions-variables.md#data-factory-system-variables). De ondersteunde variabelen zijn: **{jaar}**, **{maand}**, **{dag}**, **{uur}**, **{minuut}**en **{aangepaste}**. Voorbeeld: inputfolder / {jaar} / {maand} / {dag}.

Stel dat u mappen in de volgende indeling hebt ingevoerd:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klik op de knop **Bladeren** voor het **bestand of map**, blader naar een van deze mappen (bijvoorbeeld 2016 -> 03 -> 01-02 >), en klikt u op **kiezen**. U ziet nu `2016/03/01/02` in het tekstvak. Nu vervangen door **2016** **{jaar}**, **03** met **{maand}**, **01** met **{dag}**en **02** met **{uur}**en druk op Tab. Hier ziet u de vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Systeemvariabelen gebruiken](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Zoals u in de volgende schermafbeelding, kunt u ook een **aangepaste** variabele en eventuele [ondersteund tekenreeksen met de opmaak](https://msdn.microsoft.com/library/8kb3ddd4.aspx)gebruiken. Als u wilt een map die structuur selecteert, gebruikt u eerst de knop **Bladeren** . Vervolgens vervangen door een waarde **{aangepaste}**en druk op Tab om te zien van het tekstvak waarin u de notatietekenreeks kunt typen.     

![Met behulp van aangepaste variabele](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Ondersteuning voor verschillende gegevens en objecttypen
U kunt honderden mappen, bestanden of tabellen efficiënt verplaatsen met behulp van de Wizard kopiëren.

![Selecteer de tabellen waaruit u gegevens kopiëren](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren eenmaal of volgens een schema (per uur per dag, enzovoort). Beide opties kunnen worden gebruikt voor de breedte van de verbindingslijnen verkoopeenheden in on-premises implementatie, cloud en lokale bureaublad kopie.

Een eenmalige kopieeropdracht kunt slechts één keer verplaatsing van de gegevens uit een bron naar een bestemming. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u gegevens op een vooraf aangegeven terugkeerpatroon kopiëren. U kunt uitgebreide instellingen (zoals opnieuw, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Eigenschappen van de planning](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Volgende stappen
Zie voor een snelle rondleiding van het gebruik van de Wizard Factory kopie maken van een pijplijn met kopie activiteit, [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md).
