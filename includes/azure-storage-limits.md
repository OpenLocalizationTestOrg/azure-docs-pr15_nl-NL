Resource|Standaardlimiet
---|---
Aantal opslag accounts per abonnement|200<sup>1</sup>
TB per opslag-account|500 TB
Maximumaantal blob containers, BLOB's, gedeelde bestanden, tabellen, wachtrijen, entiteiten of berichten per opslag-account|Alleen limiet is de capaciteit van 500 TB opslagruimte account
Maximale grootte van één blob container, tabel of wachtrij|500 TB
Maximaal aantal blokken in een blob blokkeren of blob toe te voegen|50.000
Maximale grootte van een blok in een blob blokkeren of blob toe te voegen|4 MB
Maximale grootte van een blob blokkeren of blob toe te voegen|50.000 x 4 MB (ongeveer 195 GB) 
Maximale grootte van een pagina-blob |1 TB
Maximumgrootte van een Tabelentiteit|1 MB
Maximumaantal eigenschappen in een Tabelentiteit|252
Maximale grootte van een bericht in een wachtrij|64 KB
Maximale grootte van een bestandsshare|5 TB
Maximale grootte van een bestand in een bestand delen|1 TB
Maximumaantal bestanden in een bestand delen|Alleen limiet is de capaciteit van 5 TB van het bestand delen
Max 8 KB IO's / s per delen|1000
Maximumaantal bestanden in een bestand delen|Alleen limiet is de capaciteit van 5 TB van het bestand delen
Maximumaantal blob containers, BLOB's, gedeelde bestanden, tabellen, wachtrijen, entiteiten of berichten per opslag-account|Alleen limiet is de capaciteit van 500 TB opslagruimte account
Maximumaantal opgeslagen clienttoegangsbeleid per container, bestandsshare, tabel of wachtrij|5
Totale aanvragen tarieven (ervan uitgaande dat de grootte van objecten 1KB) per opslag-account|Maximaal 20.000 IO's / s, entiteiten per seconde of berichten per seconde
Doel doorvoer voor één blob|Maximaal 60 MB per seconde of maximaal 500 aanvragen per seconde
TARGET doorvoer voor één wachtrij (1 KB berichten)|Maximaal 2000 berichten per seconde
Doel doorvoer voor één tabel partition (1 KB entiteiten)|Maximaal 2000 entiteiten per seconde
TARGET doorvoer voor één bestand delen|Maximaal 60 MB per seconde
Max ingress<sup>2</sup> per opslag-account (US regio's)|10 GB/s als GRS/ZRS<sup>3</sup> ingeschakeld, 20 GB/s voor LRS
Max egress<sup>2</sup> per opslag-account (ons regio's)|20 GB/s als AB-GRS/GRS/ZRS<sup>3</sup> ingeschakeld, 30 GB/s voor LRS
Max ingress<sup>2</sup> per opslag-account (Europese en Aziatische gebieden)|5 GB/s als GRS/ZRS<sup>3</sup> ingeschakeld, 10 GB/s voor LRS
Max egress<sup>2</sup> per opslag-account (Europese en Aziatische gebieden)|10 GB/s als AB-GRS/GRS/ZRS<sup>3</sup> ingeschakeld, 15 GB/s voor LRS

<sup>1</sup> Dit geldt ook voor zowel Standard en Premium opslag-accounts. Als u meer dan 200 opslag accounts vereist, moet u een aanvraag via [Azure-ondersteuning](https://azure.microsoft.com/support/faq/). Het team van Azure Storage de zaak bedrijven wordt Controleer en maximaal 250 opslag-accounts kan goedkeuren. 

<sup>2</sup> *Ingress* verwijst naar alle gegevens (aanvragen) dat wordt verzonden naar een opslag-account. *Egress* verwijst naar alle gegevens (antwoorden), wordt ontvangen van een opslag-account.  

<sup>3</sup> Azure opslag replicatie-opties zijn:

- **AB-GRS**: leestoegang geografische-redundante opslag. Als AB-GRS is ingeschakeld, zijn egress doelen voor de secundaire locatie gelijk aan die voor de primaire locatie.
- **GRS**: geografische-redundante opslag. 
- **ZRS**: Zone-redundante opslag. Alleen beschikbaar voor BLOB's blokkeren. 
- **LRS**: lokaal redundante opslag. 

