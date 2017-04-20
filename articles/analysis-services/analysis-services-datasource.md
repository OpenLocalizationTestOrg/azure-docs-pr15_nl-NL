<properties
   pageTitle="Gegevensbron verbindingen | Microsoft Azure"
   description="Gegevensbronverbindingen voor gegevensmodellen in Azure Analysis Services beschreven."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Gegevensbron verbindingen

Gegevensmodellen in Azure Analysis Services mogelijk verschillende gegevensproviders nodig wanneer verbinding maken met bepaalde gegevensbronnen. In sommige gevallen mogelijk tabelmodellen verbinding maken met gegevensbronnen met behulp van systeemeigen providers zoals SQL Server Native Client (SQLNCLI11) een fout geretourneerd.

Bijvoorbeeld; Als er een in het geheugen of directe Query gegevens modelleren die verbinding met een cloud-gegevensbron zoals Azure SQL-Database, maakt als u andere aanbieders van systeemeigen dan SQLOLEDB gebruikt, ziet u mogelijk foutbericht: **"De provider 'SQLNCLI11.1' is niet geregistreerd"**.

Of, als u een verbinding maken met gegevensbronnen on-premises implementatie, DirectQuery-model hebt als u systeemeigen providers ziet u mogelijk foutbericht: **"Fout bij maken van OLE DB rij instellen. Onjuiste syntaxis bij 'LIMIET' "**.

## <a name="data-source-providers"></a>Leveranciers van gegevensbronnen

De volgende voorzieningen van de gegevensbron worden ondersteund voor in het geheugen of Query gegevensmodellen rechtstreekse wanneer u verbinding maakt met on-premises implementatie of cloud gegevensbronnen:

|               | **Gegevensbron**                     | **In het geheugen**                            |  **Directe Query**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Cloud**                     | Azure SQL datawarehouse      | .NET framework-gegevensprovider voor SQL Server | .NET framework-gegevensprovider voor SQL Server |
|                           | Azure SQL-Database            | .NET framework-gegevensprovider voor SQL Server | .NET framework-gegevensprovider voor SQL Server |
| **On-premises** (via Gateway) | SQL Server                    | SQL Server Native Client 11.0               | .NET framework-gegevensprovider voor SQL Server |
|                           |  SQL Server                             | Microsoft OLE DB-Provider voor SQL Server    |   .NET framework-gegevensprovider voor SQL Server                                          |
|                           |  SQL Server                             | .NET framework-gegevensprovider voor SQL Server |  .NET framework-gegevensprovider voor SQL Server                                           |
|                           | Oracle                        | Microsoft OLE DB-Provider voor Oracle        | Oracle-gegevensprovider voor .NET               |
|                           |  Oracle                             | Oracle-gegevensprovider voor .NET               | Oracle-gegevensprovider voor .NET                                            |
|                           | Teradata                      | OLE DB-Provider voor Teradata                | Teradata-gegevensprovider voor .NET             |
|                           |  Teradata                             | Teradata-gegevensprovider voor .NET             |  Teradata-gegevensprovider voor .NET                                            |
|                           | Analysesysteem Platform | .NET framework-gegevensprovider voor SQL Server | .NET framework-gegevensprovider voor SQL Server |


> [AZURE.NOTE] Controleer of de 64-bits providers zijn geïnstalleerd wanneer u een On-Premises Gateway gebruikt.

Wanneer u een lokale SQL Server Analysis Services-tabelmodel migreert met Azure Analysis Services, is het mogelijk dat deze moet de provider wijzigen.

**Om op te geven van een gegevensbron-provider**

1. In SSDT > **Tabelvorm Modelverkenner** > **Gegevensbronnen**, met de rechtermuisknop op een gegevensbronverbinding en klik vervolgens op **Gegevensbron bewerken**.

2. Klik bij **Verbinding bewerken**, klikt u op **Geavanceerd** om de volgende eigenschappen-venster te openen.

3. **Geavanceerde eigenschappen instellen**van > **Providers**en selecteer vervolgens de gewenste provider.

## <a name="impersonation"></a>Imitatie
In sommige gevallen, is het mogelijk dat deze nodig zijn om op te geven van een andere imitatie-account. Imitatie account kan worden opgegeven in SSDT of SSMS.

Voor on-premises gegevensbronnen:

- Als SQL-verificatie gebruikt, moet imitatie serviceaccount.
- Als Windows-verificatie gebruikt, kunt u Windows-gebruiker/wachtwoord instellen. Voor SQL Server, wordt Windows-verificatie met een specifieke imitatie-account alleen ondersteund voor in het geheugen gegevensmodellen.

Voor cloud gegevensbronnen:

- Als SQL-verificatie gebruikt, moet imitatie serviceaccount.


## <a name="next-steps"></a>Volgende stappen

Als u een on-premises gegevensbronnen hebt, moet u de [On-premises gateway](analysis-services-gateway.md)installeren. Zie meer informatie over het beheren van uw server in SSDT of SSMS [beheren uw server](analysis-services-manage.md).
