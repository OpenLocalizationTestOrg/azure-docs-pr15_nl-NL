<properties 
   pageTitle="Stream Analytics beperkingen voor tabel"
   description="Beschrijving van systeemlimieten en aanbevolen grootte voor Stream Analytics-onderdelen en verbindingen."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Limiet-ID | Limiet       | Opmerkingen |
|----------------- | ------------|--------- |
| Maximum aantal eenheden Streaming per abonnement per regio | 50 | Een verzoek om uit te breiden streaming eenheden voor uw abonnement dan 50 kan worden aangebracht door contact opneemt met [Microsoft ondersteuning](https://support.microsoft.com/en-us). |
| Maximumdoorvoer van een eenheid Streaming | 1MB / s * | Maximumdoorvoer per SU, is afhankelijk van het scenario. Werkelijke doorvoer mogelijk lager en hangt af van de query complexiteit en partitioneren. Meer informatie vindt u in het artikel [schaal Azure Stream Analytics-taken om uit te breiden doorvoer](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maximum aantal ingangen per project | 60 | Er is een vaste limiet 60 ingevoerde per Stream Analytics-project. |
| Maximum aantal uitvoer per project | 60 | Er is een vaste limiet van 60 uitvoer per Stream Analytics-project. |
| Maximum aantal functies per project | 60 | Er is een vaste limiet van 60 functies per Stream Analytics-project. |
| Maximum aantal taken per regio | 1500 | Elk abonnement mogelijk maximaal 1500 projecten per geografisch gebied. |