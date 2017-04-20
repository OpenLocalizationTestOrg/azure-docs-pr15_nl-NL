<properties
   pageTitle="Werken met rapporten met behulp van de JavaScript-API | Microsoft Azure"
   description="Power BI ingesloten, werken met rapporten met behulp van de JavaScript-API"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Werken met Power BI-rapporten met de JavaScript-API

De JavaScript-API van Power BI kunt u eenvoudig insluiten van Power BI-rapporten in uw toepassingen. Met de API, kunnen uw toepassingen via een programma werken met verschillende rapport grafiekelementen, zoals pagina's en filters. Deze interactiviteit wordt Power BI-rapporten beter geïntegreerde onderdeel van uw toepassing.

U kunt een Power BI-rapport insluiten in uw toepassing met behulp van een iframe die wordt gehost als onderdeel van de toepassing. De ingevoegde iframe fungeert als een begrenzing tussen uw toepassing en het rapport, zoals u in de volgende afbeelding ziet. 

![Power BI ingesloten iframe zonder Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

De ingevoegde iframe vergemakkelijkt het insluiten proces veel, maar zonder de JavaScript-API het rapport en uw toepassing niet kunnen communiceren met elkaar. Het gebrek aan interactie kunt u vindt dat het rapport niet echt deel uit van de toepassing. Het rapport en -toepassing, echt nodig hebt om te communiceren heen en weer, zoals in de volgende afbeelding.

![Power BI ingesloten iframe met Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

De JavaScript-API van Power BI kunt u programmacode die veilig door de iframe-grens schrijven. Hiermee worden uw toepassing via een programma een actie wordt uitgevoerd in een rapport en om te luisteren naar gebeurtenissen van acties die gebruikers in het rapport aanbrengen.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Wat kunt u doen met de Power BI JavaScript-API?
Met de JavaScript-API kunt u rapporten beheren, navigeer naar de pagina's in een rapport, een rapport filteren en verwerken gebeurtenissen te sluiten. In het volgende diagram ziet u de structuur van de API.

![Power BI JavaScript-API-diagram](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Rapporten beheren
De Javascript-API kunt u voor het beheren van gedrag op het niveau van de pagina en rapport:

- Een specifiek rapport van Power BI veilig insluiten in uw toepassing - probeer het [insluiten van demo-toepassing](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Toegangstoken instellen
- Het rapport configureren
  - Inschakelen en het deelvenster voor filter en het deelvenster Paginanavigatie uitschakelen: probeer de [Instellingen demo toepassing bijwerken](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Standaardwaarden instellen voor pagina's en filters - probeer de [standaardwaarden demo instellen](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Voer en modus voor volledig scherm afsluiten

[Meer informatie over het insluiten van een rapport](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Navigeer naar de pagina's in een rapport
De JavaScript-API-enbales u kunnen vinden van alle pagina's in een rapport en om in te stellen van de huidige pagina. Probeer de [Navigatie demo-toepassing](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Meer informatie over Paginanavigatie](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Een rapport filteren
De JavaScript-API biedt basisfilters en geavanceerde filtermogelijkheden voor ingesloten rapporten en rapportpagina's. Probeer het [filteren van demo-toepassing](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)en Controleer hier inleidende code.  


#### <a name="basic-filters"></a>Basisfilters
Een eenvoudig filter op een kolom of een hiërarchie niveau is geplaatst en bevat een lijst met waarden wilt opnemen of uitsluiten.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Geavanceerde filters
Geavanceerde filters gebruiken de logische operator en of of, en een of twee voorwaarden, elk voorzien van hun eigen operator en waarde accepteren. Ondersteunde voorwaarden zijn:

- Geen
- LessThan
- LessThanOrEqual
- Groter dan
- GreaterThanOrEqual
- Bevat
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Is
- IsNot
- ISLEEG
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Meer informatie over filteren](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Gebeurtenissen afhandelen
Naast de gegevens in de ingevoegde iframe verzendt, kan uw toepassing ook informatie over de volgende gebeurtenissen die afkomstig zijn uit de iframe ontvangen:

- Insluiten
  - geladen
  - Fout
- Rapporten
  - pageChanged
  - dataSelected (binnenkort)

[Meer informatie over het afhandelen van gebeurtenissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over de Power BI JavaScript-API, raadpleegt u de volgende koppelingen:

- [JavaScript-API Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Naslaginformatie](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Voorbeelden
  - [Hoek](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Live-demo](https://microsoft.github.io/PowerBI-JavaScript/demo/)
