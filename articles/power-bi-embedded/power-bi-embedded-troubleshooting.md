<properties
   pageTitle="Microsoft Power BI ingesloten Preview problemen oplossen"
   description="Microsoft Power BI ingesloten Preview problemen oplossen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Microsoft Power BI ingesloten Preview problemen oplossen
In dit artikel vindt u antwoorden voor het oplossen van **Power BI ingesloten**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Tekenreeksen van SQL Server-verbinding instellen
Als u wilt instellen op een SQL Server verbinding tekenreeks, die u moet volgen van een specifieke notatie. Hieronder vindt u een voorbeeld-verbindingsreeks voor SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Meer informatie over SQL Server verbindingstekenreeksen, raadpleegt u de volgende artikelen:

-   [SQL Server verbindingstekenreeksen](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Referenties instellen
In het geval waar zich de referenties voor een ontwikkeling of testomgeving, zoals de gebruikersnaam en wachtwoord, moet u mogelijk referenties die overeenkomen met een oplossing productie bijwerken.

## <a name="see-also"></a>Zie ook
- [Aan de slag met steekproef](power-bi-embedded-get-started-sample.md)
- [Wat is Power BI ingesloten](power-bi-embedded-what-is-power-bi-embedded.md)
