<properties 
    pageTitle="Releaseopmerkingen voor stream Analytics | Microsoft Azure" 
    description="Releaseopmerkingen voor stream Analytics" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Releaseopmerkingen voor stream Analytics

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Notities op 04/15/2016-versie van de Stream Analytics ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
Algemene beschikbaarheid voor Power BI-uitvoer  | [Hiermee kunt u Power BI](stream-analytics-power-bi-dashboard.md) zijn nu algemeen beschikbaar. De verlooptijd 90 dagen autorisatie voor Power BI is verwijderd. Zie het gedeelte [autorisatie voor verlengen](stream-analytics-power-bi-dashboard.md#Renew-authorization) van een Power BI-dashboard maken voor meer informatie over scenario's waarin autorisatie moet worden vernieuwd.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Notities op 03/03/2016-versie van de Stream Analytics ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
Nieuwe Stream Analytics-querytaal items  | SAQL heeft nu [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-pagina"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN-pagina") en [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-pagina").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Notities op 10-12-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
REST API versie bijwerken | De REST API-versie is bijgewerkt naar 10-01-2015. Meer informatie vindt u op MSDN via [Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx) en [Machine Learning-integratie in de Stream Analytics](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Azure Machine Learning-integratie | Met deze release wordt geleverd ondersteuning voor Azure Machine Learning-gebruiker gedefinieerde functies. Zie de [zelfstudie](stream-analytics-machine-learning-integration-tutorial.md) voor meer informatie, evenals de [aankondiging van de algemene blog](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Notities op 12-11-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende update.

Titel | Beschrijving
---|---
Nieuwe gedrag van selecteren | Selecteer in de Stream Analytics is uitgebreid om toe te staan * als een eigenschapsaccessor van een geneste record. Raadpleeg [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Complexe gegevenstypen")voor meer informatie.

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Notities op 22-10-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.

Titel | Beschrijving
---|---
Aanvullende query taalfuncties | Stream Analytics heeft de querytaal uitgebreid met behulp van de volgende functies: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx) [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [aanmelden](https://msdn.microsoft.com/library/azure/mt605290.aspx), [vierkante](https://msdn.microsoft.com/library/azure/mt605288.aspx)en [WORTEL](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Statistische beperkingen verwijderd  | Deze release Hiermee verwijdert u de beperking van 15 aggregaties in een query. Er is nu geen beperking voor het aantal aggregaties per query.
Toegevoegde groep door System.Timestamp-functie | De functie [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) kan nu window_type of [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Toegevoegde verschuiving voor Tumbling en Hopping van windows | Standaard [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) en [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) windows zijn uitgelijnd tegen de nul tijd (1/1/0001 12:00:00 AM UTC). De nieuwe (optioneel) parameter 'offsetsize' kunt een aangepaste verschuiving (of uitlijning).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Notities op 29-09-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.

Titel | Beschrijving
---|---
Azure IoT Suite Public Preview | Stream Analytics is opgenomen in de openbare preview-versie van de Azure IoT-Suite.
Azure Portal-integratie | Naast voortdurende aanwezigheid in de beheerportal van Azure, is de Stream Analytics nu geïntegreerd in de [Portal van Azure](https://azure.microsoft.com/overview/preview-portal/). Opmerking Stream Analytics functionaliteit in de portal Preview momenteel is een subset van de functionaliteit voor aangeboden in de beheerportal van Azure, zonder ondersteuning voor het testen van query's in de browser Power BI uitvoer configuratie en bladeren naar of het maken van nieuwe invoer en uitvoer resources in abonnementen die u toegang hebt.
Ondersteuning voor DocumentDB uitvoer | Stream Analytics taken kunnen nu uitvoeren naar [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Ondersteuning voor IoT Hub invoer | Stream Analytics taken kunnen nu gegevens uit IoT Hubs nemen.
TIJDSTEMPEL door voor heterogene gebeurtenissen | Als een eenmalige gegevensstroom meerdere gebeurtenistypen met tijdstempels in verschillende velden bevat, kunt u nu gebruiken [Tijdstempel door](http://msdn.microsoft.com/library/mt573293.aspx) met expressies kunt u verschillende tijdstempelvelden voor elk geval opgeven.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Notities op 10-09-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.

Titel|Beschrijving
---|---
Ondersteuning voor PowerBI-groepen|Als u wilt delen van gegevens met andere gebruikers Power BI inschakelt, kunnen Stream Analytics taken nu schrijven aan [PowerBI groepen](stream-analytics-define-outputs.md#power-bi) in uw Power BI-account.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Notities op 20-08-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.

Titel|Beschrijving
---|---
LAATSTE functie toegevoegd |De [laatste](http://msdn.microsoft.com/library/mt421186.aspx) functie is nu beschikbaar in Stream Analytics-taken, zodat u kunt het ophalen van de meest recente gebeurtenis in de stroom van een gebeurtenis binnen een bepaalde tijd.
Nieuwe functies van de matrix|Matrixfuncties [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) en [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) zijn nu beschikbaar.
Nieuwe Record-functies|Record functies [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) en [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) zijn nu beschikbaar.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Notities op 30-07-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.

Titel|Beschrijving
---|---
Organisatie-Id voor Power BI is ontkoppelde van Azure-Id|Deze functie kunt [Power BI-uitvoer](stream-analytics-power-bi-dashboard.md) voor ASA taken onder elk type Azure-account (Live Id of organisatie-Id). U kunt ook een organisatie-Id hebt voor uw Azure-account en gebruiken van een andere naam om Power BI-uitvoer te machtigen.
Ondersteuning voor Service Bus wachtrijen uitvoer|Uitvoer van de [Service Bus wachtrijen](stream-analytics-define-outputs.md#service-bus-queues) zijn nu beschikbaar in Stream Analytics-taken.
Ondersteuning voor Service Bus onderwerpen uitvoer|Uitvoer van de [Service Bus onderwerpen](stream-analytics-define-outputs.md#service-bus-topics) zijn nu beschikbaar in Stream Analytics-taken.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Notities op 09-07-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.


Titel|Beschrijving
---|---
Aangepaste Blob-uitvoer partitioneren|Uitvoer van BLOB storage kunnen nu een optie voor de frequentie die uitvoer BLOB's worden geschreven en de structuur en opmaak van de gegevens voor uitvoer pad mapstructuur. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Notities op 03-05-2015-versie van de Stream Analytics ##

Deze versie bevat de volgende updates.


Titel|Beschrijving
---|---
Verbeterde maximumwaarde voor Out aantal volgorde tolerantie venster|De maximale grootte voor het venster van de tolerantie bij aantal volgorde is nu 59:59 (mm: SS)
Indeling van JSON-uitvoer: Regel gescheiden of een matrix|Er is nu een optie bij het uitvoeren naar Blob Storage of gebeurtenis Hub om uit te voeren als een matrix van JSON objecten of door te scheiden van JSON objecten met een nieuwe regel. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Notities op 16-04-2015-versie van de Stream Analytics ##


Titel|Beschrijving
---|---
Vertraging in de configuratie van de opslag van Azure-account|Wanneer u een Stream Analytics-taak in een gebied voor de eerste keer, wordt u gevraagd om te maken van een nieuw account voor de opslag of een bestaand account voor het controleren van de Stream Analytics-taken in dat gebied opgeven. Vanwege latentie in het configureren van monitoring, wordt een andere Stream Analytics-taak maken in hetzelfde gebied binnen 30 minuten gevraagd om aan te geven van een tweede opslag-account in plaats van onlangs geconfigureerde dat in de vervolgkeuzelijst Monitoring opslag Account weergegeven. Als u wilt voorkomen dat een onnodige opslag-account maken, wacht 30 minuten na het maken van een taak in een gebied voor het eerst voordat u gaat inrichten van extra taken in die regio.
Taak-Upgrade|Op dit moment kan ondersteunt Stream Analytics geen live wijzigingen in de definitie of de configuratie van een lopend project. Om te kunnen invoer, uitvoer, query, schaal of de configuratie van een actieve taak wijzigen, moet u eerst de taak beëindigen.
Gegevenstypen die zijn afgeleid van invoerbron|Als een instructie CREATE TABLE niet wordt gebruikt, het invoertype wordt afgeleid van de invoer-indeling, bijvoorbeeld alle velden uit CSV tekenreeks zijn. Velden moeten expliciet worden geconverteerd naar het juiste type de CAST-functie gebruiken om te voorkomen type verkeerde combinaties fouten.
Ontbrekende velden zijn output als null-waarden|Null-waarden in de gebeurtenis uitvoer treden verwijst naar een veld dat niet aanwezig is in de invoerbron.
MET instructies voorafgaan SELECT-instructies|SELECT-instructies moeten subquery's die zijn gedefinieerd met instructies volgen in uw query.
Probleem bij geheugen|Streaming Analytics-taken met een grote tolerantie voor out volgorde gebeurtenissen en/of complexe query's voor het onderhouden van dat een grote hoeveelheid staat kan ertoe leiden dat de taak wilt uitvoeren onvoldoende geheugen, wat resulteert in een taak opnieuw starten. De start en beëindiging bewerkingen zijn zichtbaar in Logboeken aan de bewerking van de taak. U lost dit op verschillende partities de schaal van de query uit. In een toekomstige release, wordt deze beperking opgelost door daardoor verslechteren prestaties op risico taken in plaats van deze opnieuw te starten.
Grote blob invoeritems zonder nettolading tijdstempel kunnen Out geheugen probleem veroorzaken|Grote bestanden uit Blob storage verbruikt mogelijk Stream Analytics taken loopt vast als een tijdstempelveld niet is opgegeven via tijdstempel door. Houd om te voorkomen dat dit probleem, elke blob kleiner dan 10MB grootte.
SQL-Database gebeurtenis volume beperking|Wanneer u SQL-Database als een uitvoerdoel, kunnen de taak Stream Analytics time-out veroorzaken in zeer grote hoeveelheden uitvoergegevens. U lost dit probleem, Verminder het volume uitvoer met aggregaties of filter operatoren of kies Azure-blobopslag of gebeurtenis Hubs als een uitvoerdoel in plaats daarvan.
PowerBI gegevenssets kan slechts één tabel bevatten.|PowerBI biedt geen ondersteuning voor meerdere tabellen in een bepaalde gegevensset.

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
