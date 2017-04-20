<properties
    pageTitle="Logica apps migreren naar schema versie 2015-08-01-preview | Microsoft Azure-App-Service"
    description="U kunt eenvoudig uw apps logica migreren naar de nieuwste schemaversie. Volg deze stappen."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Logica apps migreren naar schema versie 2015-08-01-voorbeeld

Ga als volgt te werk als u wilt uw bestaande logica apps verplaatsen naar het nieuwe schema:  
1. Open de logica-app in de portal van Azure  
2. Klik op Update Schema:

 ![API-pictogram][step1]   
De pagina Update Schema worden weergegeven en bevat een koppeling naar een document met meer informatie over de verbeteringen in het nieuwe schema: ![API-pictogram][step2]

>[AZURE.NOTE] Wanneer u **Update Schema**selecteert, wordt automatisch migratiestappen uitvoeren en geef de code-uitvoer voor u. U kunt dit echter de definitie van uw, bijwerken, zorgen dat u volgt goede coding procedures zoals die uit die in de sectie **Aanbevolen procedures** hieronder beschreven.

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Aanbevolen procedures wanneer uw apps logica migreren naar de meest recente schemaversie:  

- Kopieer het gemigreerde script naar een nieuwe logica-App - niet overschrijven de oude totdat u klaar bent met testen en de gemigreerde app bevestigd werkt zoals verwacht.
- Test uw logica app **voordat u** plaatsen in productie
- Nadat de migratie is voltooid, start u het bijwerken van uw apps logica voor het gebruik van de [beheerde API's](./apis-list.md) indien mogelijk. U kunt bijvoorbeeld gaan gebruiken Dropbox v2, ziekte u DropBox v1 gebruikt.


## <a name="whats-next"></a>Volgende stappen
-  [Informatie over het handmatig migreren van uw apps logica](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






