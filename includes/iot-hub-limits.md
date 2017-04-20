De volgende tabel bevat de limieten die is gekoppeld aan de andere Servicelagen (S1, S2, S3, F1). Zie [IoT Hub prijzen](https://azure.microsoft.com/pricing/details/iot-hub/)voor informatie over de kosten per *eenheid* in elke laag.

| Resource | S1 standaard | S2 standaard | S3 standaard | Gratis F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Berichten per dag | 400.000 | 6,000,000   | 300,000,000 | 8000 man   |
| Maximum aantal eenheden | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Als u het gebruik van meer dan 200 eenheden met een laag S1 of S2 of S3 hub verwacht, neem contact op met Microsoft ondersteuning.

De volgende tabel bevat de limieten die van toepassing op IoT Hub resources:

| Resource | Limiet |
| -------- | ----- |
| Maximum IoT hubs per Azure abonnement dat is betaald | 10 |
| Maximale gratis IoT hubs per Azure abonnement | 1 |
| Maximum aantal apparaat identiteiten<br/>  geretourneerd door één aanroep | 1000 |
| IoT Hub bericht maximale bewaarbeleid voor apparaat naar cloud-berichten | 7 dagen |
| Maximale grootte van het apparaat naar cloud-bericht | 256 KB |
| Maximale grootte van het apparaat naar cloud batch | 256 KB |
| Maximaal aantal berichten in de batch apparaat naar cloud | 500 |
| Maximale grootte van bericht cloud-naar-apparaat | 64 KB |
| Maximale TTL voor berichten cloud-naar-apparaat | twee dagen |
| De bezorging van de maximale aantal voor cloud-naar-apparaat <br/> berichten | 100 |
| De bezorging van de maximale aantal voor Feedbackberichten <br/> in antwoord op een bericht cloud-naar-apparaat | 100 |
| Maximale TTL voor Feedbackberichten in <br/> antwoord op een bericht cloud-naar-apparaat | twee dagen |

> [AZURE.NOTE] Als u meer dan 10 betaalde IoT hubs in een Azure-abonnement nodig hebt, neem contact op met Microsoft ondersteuning.

De service IoT Hub bandbreedte aanvragen wanneer de volgende quota worden overschreden:

| Beperken | Per hub waarde |
| -------- | ------------- |
| Identiteit registerbewerkingen <br/> (maken, ophalen, lijst, bijwerken en verwijderen), <br/> afzonderlijke of bulksgewijs importeren/exporteren | 5000/min/eenheid (voor S3) <br/> 100/min/eenheid (voor S1 en S2). |
| Apparaatverbindingen | 6000/sec/eenheid (voor S3), 120/sec/eenheid (voor S2), 12/sec/eenheid (voor S1). <br/>Minimum van 100/sec. |
| Apparaat-naar-cloud wordt verzonden | 6000/sec/eenheid (voor S3), 120/sec/eenheid (voor S2), 12/sec/eenheid (voor S1). <br/>Minimum van 100/sec. |
| Cloud-naar-apparaat verzenden | 5000/min/eenheid (voor S3), 100/min/eenheid (voor S1 en S2). |
| Cloud-to-device ontvangt | 50000/min/eenheid (voor S3), 1000/min/eenheid (voor S1 en S2). |
| Bestand uploadbewerkingen | 5000 bestand uploaden meldingen/min/eenheid (voor S3), 100 bestand uploaden meldingen/min/eenheid (voor S1 en S2). <br/> 10000 SA's-URI's kan zijn uit voor een account Azure-opslag in één keer.<br/> 10 SA's URI's / apparaat kan zijn af in één keer. |
