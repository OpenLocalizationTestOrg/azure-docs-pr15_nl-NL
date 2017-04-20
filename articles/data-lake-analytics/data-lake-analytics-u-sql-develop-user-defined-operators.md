<properties 
   pageTitle="I-SQL door de gebruiker gedefinieerde operatoren voor taken van Azure gegevens Lake analyses ontwikkelen | Azure" 
   description="Informatie over het ontwikkelen van de gebruiker gedefinieerde operators worden gebruikt en opnieuw kan worden gebruikt in gegevens Lake Analytics-taken. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>I-SQL door de gebruiker gedefinieerde operatoren voor taken van Azure gegevens Lake analyses ontwikkelen

Informatie over het ontwikkelen van de gebruiker gedefinieerde operators worden gebruikt en opnieuw kan worden gebruikt in gegevens Lake Analytics-taken. U kunt een aangepaste operator als u wilt converteren landnamen worden ontwikkelen.

##<a name="prerequisites"></a>Vereisten voor

- Visual Studio-2015, Visual Studio 2013 bijwerken 4 of Visual Studio 2012 met visuele C++ geïnstalleerd 
- Microsoft Azure SDK voor .NET versie 2.5 of hoger.  Installeren met behulp van het installatieprogramma van de Web-platform.
- Een gegevens Lake Analytics-account.  Zie [Aan de slag met Azure gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md).
- Ga via de zelfstudie [aan de slag met Azure gegevens Lake Analytics I-SQL Studio](data-lake-analytics-u-sql-get-started.md) .
- Verbinding maken met Azure, raadpleegt u [aan de slag met Azure gegevens Lake Analytics I-SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- De brongegevens uploaden, raadpleegt u [aan de slag met Azure gegevens Lake Analytics I-SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definiëren en gebruiken van de gebruiker gedefinieerde operator in I-SQL

**Maken en indienen van een taak I-SQL** 

1. Klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.
2. Selecteer het type **Project I-SQL** .

    ![nieuwe I-SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klik op **OK**. Een oplossing maakt in Visual studio met een Script.usql-bestand.
4. **Oplossing Explorer**Script.usql uitvouwen, en dubbelklik vervolgens op **Script.usql.cs**.
5. Plak de volgende code in het bestand:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Open Script.usql en plak de volgende I-SQL-script:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. Vanuit **Solution Explorer**, **Script.usql**met de rechtermuisknop op en klik vervolgens op **Script maken**.
6. Vanuit **Solution Explorer**, **Script.usql**met de rechtermuisknop op en klik op **Script indienen**.
7. Als u dit nog niet hebt verbonden met uw Azure-abonnement, kunt u zult prompt om uw Azure-account.
7. Klik op **verzenden**. Resultaten van indienen en taakkoppeling zijn beschikbaar in het venster resultaten wanneer de indiening is voltooid.
8. U moet deze op de knop Vernieuwen om te zien van de meest recente taakstatus en het scherm vernieuwen.

**Om de uitvoer van de taak weer te geven**

1. Van **Server Explorer** **Azure**uitvouwen, **Gegevens Lake Analytics**uitvouwen, uitvouwen van uw gegevens Lake Analytics-account, **Opslag Accounts**uitvouwen, met de rechtermuisknop op de standaard-opslag, en klik op **Explorer**. 
2. Vouw van voorbeelden, uitvoer van uit en dubbelklik vervolgens op **Stuurprogramma's.csv**.


##<a name="see-also"></a>Zie ook

- [Aan de slag met gegevens Lake Analytics via PowerShell](data-lake-analytics-get-started-powershell.md)
- [Aan de slag met gegevens Lake analyses met behulp van de Azure portal](data-lake-analytics-get-started-portal.md)
- [Data Lake Tools voor Visual Studio gebruiken voor het ontwikkelen van I-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
