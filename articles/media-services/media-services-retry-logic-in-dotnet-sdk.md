<properties
    pageTitle="Probeer de logica in de Media Services SDK voor .NET | Microsoft Azure"
    description="Het onderwerp biedt een overzicht van de logica opnieuw op de Media Services SDK voor .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Probeer de logica in de Media Services SDK voor .NET

Als u werkt met Microsoft Azure-services, worden tijdelijke fouten kunnen optreden. Als een tijdelijke fout treedt op, in de meeste gevallen na een paar pogingen die de bewerking is geslaagd. De Media Services SDK voor .NET implementeert opnieuw logica voor de afhandeling van tijdelijke fouten die hoort bij uitzonderingen en fouten die worden veroorzaakt door een website gecompileerd uitvoeren van query's, wijzigingen en opslagbewerkingen op te slaan.  Standaard wordt de Media Services SDK voor .NET vier nieuwe pogingen uitgevoerd voordat u de uitzondering in uw toepassing opnieuw te genereren. De code in uw toepassing moet deze uitzondering vervolgens correct verwerken.  
  
 Hieronder ziet u een kort richtlijn met Web aanvragen, opslag, Query en SaveChanges beleid.  
  
-   Het beleid opslag wordt gebruikt voor blob storage bewerkingen (uploads of download van bestanden van de activa).  
  
-   Het beleid Web aanvragen wordt gebruikt voor algemene webaanvragen (bijvoorbeeld voor het ophalen van een verificatietoken en het oplossen van het eindpunt van de cluster gebruikers).  
  
-   Het beleid van de Query wordt gebruikt voor query's uitvoeren entiteiten van REST (bijvoorbeeld mediaContext.Assets.Where(...)).  
  
-   Het beleid SaveChanges wordt gebruikt voor het uitvoeren van alle items waarin gegevens binnen de service (bijvoorbeeld maken een entity bijwerken van een entiteit, bellen van een servicefunctie voor een bewerking) wordt gewijzigd.  
  
 In dit onderwerp vindt u een uitzondering typen en foutcodes die zijn afgehandeld door de Media Services SDK voor .NET logica opnieuw.  
  
## <a name="exception-types"></a>Uitzondering typen  

De volgende tabel wordt beschreven uitzonderingen die de Media Services SDK voor .NET verwerkt of niet verwerkt voor bepaalde bewerkingen die tijdelijke problemen kunnen veroorzaken.  
  
Uitzondering|Webaanvraag|Opslag|Query|SaveChanges
----|------|----|---|---
WebException<br/>Zie het gedeelte [WebException statuscodes](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) voor meer informatie.|Ja|Ja|Ja|Ja  
DataServiceClientException<br/> Zie voor meer informatie [foutcodes voor status HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nee|Ja|Ja|Ja
DataServiceQueryException<br/> Zie voor meer informatie [foutcodes voor status HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nee|Ja|Ja|Ja  
DataServiceRequestException<br/> Zie voor meer informatie [foutcodes voor status HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nee|Ja|Ja|Ja  
DataServiceTransportException|Nee|Nee|Ja|Ja
TimeoutException|Ja|Ja|Ja|Nee
SocketException|Ja|Ja|Ja|Ja  
StorageException|Nee|Ja|Nee|Nee 
IOException|Nee|Ja|Nee|Nee
  
###  <a name="WebExceptionStatus"></a>WebException statuscodes  

De volgende tabel ziet u welke foutcodes WebException de logica opnieuw wordt geïmplementeerd. De opsomming [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) Hiermee definieert u de statuscodes.  
  
Status|Webaanvraag|Opslag|Query|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Ja|Ja|Ja|Ja
NameResolutionFailure|Ja|Ja|Ja|Ja  
ProxyNameResolutionFailure|Ja|Ja|Ja|Ja  
SendFailure|Ja|Ja|Ja|Ja
PipelineFailure|Ja|Ja|Ja|Nee  
ConnectionClosed|Ja|Ja|Ja|Nee  
KeepAliveFailure|Ja|Ja|Ja|Nee  
UnknownError|Ja|Ja|Ja|Nee 
ReceiveFailure|Ja|Ja|Ja|Nee  
RequestCanceled|Ja|Ja|Ja|Nee  
Time-out|Ja|Ja|Ja|Nee
Een protocolfout <br/>Het opnieuw op een protocolfout wordt bepaald door de afhandeling van HTTP status code. Zie voor meer informatie [foutcodes voor status HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ja|Ja|Ja|Ja|  
  
###  <a name="HTTPStatusCode"></a>HTTP-foutcodes status  

Wanneer de Query en SaveChanges bewerkingen genereren DataServiceClientException, DataServiceQueryException of DataServiceQueryException, wordt de statuscode voor HTTP-fout in de eigenschap StatusCode geretourneerd.  De volgende tabel ziet u welke foutcodes de logica opnieuw wordt geïmplementeerd.  
  
 
Status|Webaanvraag|Opslag|Query|SaveChanges 
---|----|----|----|----
401|Nee|Ja|Nee|Nee
403|Nee|Ja<br/>Nieuwe pogingen met langer wachten op voor de verwerking.|Nee|Nee  
408|Ja|Ja|Ja|Ja
429|Ja|Ja|Ja|Ja  
500|Ja|Ja|Ja|Nee  
502|Ja|Ja|Ja|Nee  
503|Ja|Ja|Ja|Ja  
504|Ja|Ja|Ja|Nee  
  
Als u wilt maken wordt bekijkt u de daadwerkelijke uitvoering van de Media Services SDK voor .NET logica opnieuw, Zie [azure-sdk-voor-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
