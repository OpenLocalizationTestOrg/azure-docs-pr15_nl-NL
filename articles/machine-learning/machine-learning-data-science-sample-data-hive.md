<properties
    pageTitle="Voorbeeld van de gegevens in tabellen Azure HDInsight component | Microsoft Azure"
    description="Omlaag gegevens in tabellen van Azure HDInsight (Hadopop) component steekproef"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Voorbeeldgegevens in tabellen Azure HDInsight-component

In dit artikel wordt beschreven hoe omlaag gepaarde steekproeven gegevens die zijn opgeslagen in Azure HDInsight component tabellen met query's in component. Behandeld popularly gebruikte steekproeven op drie manieren:

* Uniform steekproeven
* Steekproeven door groepen
* Toepassing stratificatie steekproeven

**Waarom voorbeeld van uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze in een kleinere maar vertegenwoordiger en beter beheerbare grootte omlaag gepaarde steekproeven. Dit vergemakkelijkt gegevens lidmaatschap, te verkennen en functie engineering. De rol in het Team gegevens wetenschap proces is het inschakelen van snel prototypen van de functies voor gegevensverwerking en machine learning-modellen.

Het **menu** onder koppelingen naar onderwerpen waarin wordt uitgelegd hoe u een voorbeeld van gegevens uit verschillende opslagomgevingen.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Deze taak steekproeven is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Het indienen van component query 's
Component query's kunnen worden verzonden vanaf de opdrachtregel Hadoop-console op het hoofd knooppunt van het cluster Hadoop. Klik hiertoe Meld u aan bij het hoofd knooppunt van de cluster Hadoop, open de opdrachtregel Hadoop-console en de component query's vanaf hier indienen. Zie voor instructies voor het indienen van component query's in de opdrachtregel Hadoop-console, [hoe u query's voor component indienen](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Uniform steekproeven
Uniform steekproeven betekent dat elke rij in de gegevensset heeft een gelijk kans monster wordt genomen. Dit kan worden geïmplementeerd door toe te voegen een extra veld ASELECT() naar de gegevensset in de binnenste "select" query en in de buitenste "select" query die voorwaarde op dat willekeurig veld.

Hier volgt een voorbeeldquery:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Hier `<sample rate, 0-1>` geeft de verhouding van de records die de gebruikers monster wilt maken.

## <a name="group"></a>Steekproeven door groepen

Wanneer steekproeven bestaan gegevens, u mogelijk wilt opnemen of uitsluiten van alle exemplaren van een bepaalde waarde van een variabele bestaan. Dit is wat wordt bedoeld met "steekproeven per groep".
Bijvoorbeeld als er een bestaan variabele 'Staat', die bevat een waarde za, MA, CA, NJ, PA, enzovoort, worden de gewenste records van dezelfde staat worden altijd samen en of ze al dan niet worden vastgelegd.

Hier volgt een van de voorbeeldquery die voorbeelden per groep:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Toepassing stratificatie steekproeven

Steekproeven is toepassing stratificatie met betrekking tot een bestaan variabele wanneer de voorbeelden verkregen waarden van hebt dat bestaan die zijn in de verhouding zoals in de bovenliggende populatie waaruit de voorbeelden zijn verkregen. Het voorbeeld als hierboven, Stel dat uw gegevens onderliggend bevolking door Staten, NJ heeft 100 observaties, za heeft 60 observaties en WA heeft 300 observaties zegt. Als u het tarief weer dat van toepassing stratificatie steekproeven moeten 0,5 opgeeft, moet klikt u vervolgens de steekproef verkregen hebben ongeveer 50, 30 en 150 observaties van NJ, za en WA respectievelijk.

Hier volgt een voorbeeldquery:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Zie voor informatie over geavanceerdere steekproeven methoden die beschikbaar in component zijn [LanguageManual steekproeven](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
