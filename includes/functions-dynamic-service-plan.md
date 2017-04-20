## <a name="dynamic-service-plan"></a>Dynamische serviceplan

In de dynamische Service van plan bent, worden uw functie-apps toegewezen aan de app-exemplaar van een functie. Als nodig meer exemplaren wordt dynamisch worden toegevoegd.
Deze exemplaren kunnen over meerdere IT-bronnen, waardoor optimaal gebruikmaken van de beschikbare Azure infrastructuur beslaan. Daarnaast wordt uw functies uitgevoerd parallel minimaliseren van de totale tijd die nodig zijn voor het verwerken van aanvragen. Tijd voor elke functie wordt opgeteld, in seconden, en samengevoegd met de met functie-app. Met kosten basis met het aantal exemplaren, hun grootte van geheugen en totale tijd voor uitvoering gemeten in Gigabyte seconden. Dit is een uitstekende keus als uw behoeften berekeningscluster nu door onregelmatige of taaktijden voor uw vaak zeer korte zoals u kunt alleen betalen voor berekeningscluster resources wanneer ze daadwerkelijk gebruikt worden.   

### <a name="memory-tier"></a>Geheugen laag

Afhankelijk van de functie moet kunt u de hoeveelheid geheugen moet worden uitgevoerd ze in de functie-App (container met functies).
De opties van de grootte van geheugen variëren van **128 MB aan 1.536 MB**. De geheugengrootte van de geselecteerde correspondeert met de werkset nodig zijn voor alle functies die deel van de functie-app uitmaken. Als uw code meer geheugen dan de geselecteerde grootte vereist, wordt het exemplaar van de functie app afgesloten vanwege onvoldoende geheugen beschikbaar.

### <a name="scaling"></a>Schaalbaarheid

De functies van Azure platform wordt het verkeer behoeften beoordelen, op basis van de geconfigureerde triggers, om te bepalen wanneer aan de nieuwe schaal omhoog of omlaag. De granulatie van schaalbaarheid is de functie-app. Schaalbaarheid van in dit geval betekent dat er meer exemplaren van een functie-app toevoegen. Omgekeerd zoals verkeer uitvalt, zijn functie app-exemplaren uitgeschakeld-uiteindelijk schaalbaarheid ingedrukt op nul wanneer er geen worden uitgevoerd.  

### <a name="resource-consumption-and-billing"></a>Resourceverbruik en facturering

In de dynamische modus resourcetoewijzing klaar is anders dan het standaard-App-abonnement, dus het model verbruik ook verschillende, waardoor een model "betalen per gebruik". Verbruik worden gerapporteerd per app, functie, alleen voor tijd wanneer code wordt uitgevoerd.  
Deze functie wordt berekend door de geheugengrootte (in GB) te vermenigvuldigen met het totale bedrag voor de uitvoering (in seconden) voor alle functies die worden uitgevoerd binnen de app van deze functie. De maateenheid verbruik is **GB-s (Gigabyte seconden)**.