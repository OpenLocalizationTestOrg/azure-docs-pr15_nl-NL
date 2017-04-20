<properties
    pageTitle="Gegevens verkennen in component tabellen met query's component | Microsoft Azure"
    description="Gegevens verkennen in component tabellen met component query's."
    services="machine-learning"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Gegevens verkennen in component tabellen met component query 's

Dit document bevat voorbeeldscripts component die worden gebruikt voor het verkennen van gegevens in tabellen component in een cluster HDInsight Hadoop.

Het volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u met hulpmiddelen voor gegevens verkennen van verschillende opslagomgevingen.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, raadpleegt u [een opslag van Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)
* Deze is ingericht een aangepaste Hadoop-cluster met de service HDInsight. Als u instructies nodig hebt, raadpleegt u [Azure HDInsight Hadoop Clusters aanpassen voor geavanceerde analyses](machine-learning-data-science-customize-hadoop-cluster.md).
* De gegevens is geüpload naar component tabellen in Azure HDInsight Hadoop clusters. Als dit niet heeft, volgt u de instructies in [maken en laden gegevens aan component tabellen](machine-learning-data-science-move-hive-tables.md) gegevens aan component tabellen om eerst te uploaden.
* Externe toegang tot het cluster ingeschakeld. Als u instructies nodig hebt, raadpleegt u [toegang tot het hoofd knooppunt van Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Als u nodig hebt voor instructies voor het indienen van component query's, raadpleegt u [hoe u query's voor component indienen](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Component query voorbeeldscripts voor het verkennen

1. Het aantal waarnemingen per partition tellen `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Het aantal waarnemingen per dag tellen `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Niveaus in een kolom bestaan ophalen  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Het aantal niveaus in combinatie van twee bestaan kolommen ophalen `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Ophalen van de verdeling voor numerieke kolommen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Records ophalen uit het koppelen van twee tabellen

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Aanvullende query-scripts voor taxi reis gegevens scenario 's

Voorbeelden van query's die specifiek voor [Tevens Taxi reis gegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn zijn ook beschikbaar in [Github opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al gegevensschema opgegeven hebt en klaar bent om te worden verzonden om uit te voeren.
