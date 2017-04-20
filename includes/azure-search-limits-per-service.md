Opslag wordt beperkt door schijfruimte of door een vaste limiet op het *maximum aantal* van indexen of documenten, afhankelijk van wat zich het eerste voordoet. 

Resource|Vrij te geven|Eenvoudige|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Serviceovereenkomst (SLA)|Geen <sup>1</sup> |Ja |Ja  |Ja |Ja |Ja
Opslagruimte per partition|50 MB |2 GB|25 GB|100 GB|200 GB|200 GB
Partities per service|N/B|1|12|12|12|3
Partitiegrootte|N/B|2 GB|25 GB|100 GB|200 GB |200 GB
Replica|N/B|1|12|12|12|12
Maximale indexen|3|5|50|200|200|1000 per partition of 3000 per service
Maximum aantal documenten|10.000|1 miljoen|15 miljoen per partition of 180 miljoen per service |60 miljoen per partition of 720 miljoen per service |120 miljoen per partition of 1.4 miljard per service|1 miljoen per index of 200 miljoen per partition |
Geschatte query's per seconde (QPS)|N/B|~ 3 per replica|~ 15 per replica|~ 60 per replica|~ 60 per replica|> 60 per replica

<sup>1</sup> gratis en Preview SKU's niet afkomstig zijn met serviceovereenkomsten (Sla's). Serviceovereenkomsten worden afgedwongen nadat een SKU algemeen beschikbaar.