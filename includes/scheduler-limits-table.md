De volgende tabel worden alle van de belangrijkste quota, beperkingen, standaardwaarden en throttles in Azure Scheduler beschreven.

|Resource|Beschrijving van de limiet|
|---|---|
|**Grootte**|De maximale grootte is 16 kB. Als een opslag of een PATCH in een taak die groter zijn dan deze limieten resulteert, wordt een 400 Ongeldige aanvraag statuscode geretourneerd.|
|**Grootte van de URL aanvragen**|Maximale grootte van de aanvraag-URL is de limiet van 2048 tekens.|
|**Statistische koptekst grootte**|De grootte van de maximale statistische kop is 4096 tekens.|
|**Koptekst tellen**|Maximale koptekst tellen is 50 kopteksten.|
|**Hoofdtekst**|Maximale hoofdtekst is 8192 tekens.|
|**Bereik van terugkeerpatroon**|Maximale terugkeerpatroon reeks is 18 maanden.|
|**Tijd in de begintijd**|Maximum "time to begintijd" is 18 maanden.|
|**Overzicht van functies**|Maximale antwoord hoofdtekst die zijn opgeslagen in de taakgeschiedenis van de is 2048 bytes.|
|**Frequentie**|Het quotum voor de frequentie van max standaard is 1 uur in een verzameling gratis taak en 1 minuut in een verzameling standaard taak. De maximale frequentie kan worden geconfigureerd voor een taak collectie lager dan het maximale aantal. Alle taken in de siteverzameling van de taak zijn beperkt de waarde instellen voor het verzamelen van de taak. Als u probeert te maken van een taak met een hogere frequentie dan de maximale frequentie voor de collectie taak, de aanvraag met een 409 Conflict statuscode mislukt.|
|**Taken**|Het quotum voor standaard max taken is 5 taken in een verzameling gratis taak en 50 taken in een verzameling standaard taak. Het maximum aantal taken kan worden geconfigureerd voor de collectie van een taak. Alle taken in de siteverzameling van de taak zijn beperkt de waarde instellen voor het verzamelen van de taak. Als u probeert te maken van meer taken dan het quotum voor maximale taken, mislukt de aanvraag met een 409 Conflict statuscode.|
|**Opdracht Geschiedenis vasthouden**|Werkervaring blijft behouden voor maximaal 2 maanden of tot de laatste 1000 executions.|
|**Voltooide en mislukte vasthouden**|Voltooide en mislukte taken worden voor 60 dagen bewaard.|
|**Time-out**|Er is een statische (niet te configureren) verzoek time-out van 60 seconden voor HTTP-acties. Voor langere actieve bewerkingen, volgt asynchrone HTTP-protocollen; bijvoorbeeld een 202 terugkeren, maar blijven werken op de achtergrond.|
