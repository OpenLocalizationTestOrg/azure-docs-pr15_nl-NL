
De code voor alle functies in een bepaalde functie app bevindt zich in een hoofdmap met een hostconfiguratiebestand en een of meer submappen, die elk de code voor een afzonderlijke functie, zoals in het volgende voorbeeld bevatten

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Het bestand *host.json* sommige configuratie runtime / regiospecifieke bevat en bevindt zich in de hoofdmap van de functie-app. Zie [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in de WebJobs.Script opslagplaats wiki voor informatie over instellingen die beschikbaar zijn.

Elke functie heeft een map met een of meer codebestanden, de configuratie function.json en andere afhankelijkheden.