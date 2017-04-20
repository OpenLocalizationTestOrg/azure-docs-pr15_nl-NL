<properties 
    pageTitle="Het gebruik van de controle API traceren roept in Azure API Management" 
    description="Informatie over het gebruik van de controle API in Azure API Management traceren." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Het gebruik van de controle API traceren roept in Azure API Management

API-beheer biedt een API controle hulpmiddel waarmee u kunt met foutopsporing en probleemoplossing bij uw API's. De controle API via programmacode kan worden gebruikt en u kunt ook rechtstreeks vanuit de developer portal gebruiken. 

Naast het traceren bewerkingen uitvoeren, wordt API controle ook [beleidsexpressie](https://msdn.microsoft.com/library/azure/dn910913.aspx) evaluaties getraceerd. Zie voor een demonstratie, [Cloud begeleidende aflevering 177: meer API beheerfuncties](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruitspoelen naar 21:00.

Deze handleiding bevat een overzicht van het gebruik van API controle.

>[AZURE.NOTE] API controle sporen zijn alleen gegenereerd en beschikbaar gemaakt om te verzoeken met abonnement toetsen die deel uitmaken van [het beheerdersaccount](api-management-howto-create-groups.md) .

## <a name="trace-call"></a> Gebruik API controle moeten worden traceren van een gesprek

Toevoegen als u wilt gebruiken API controle, een **ocp-apim-doelcellen: waar** koptekst aan uw gesprek bewerking aanvragen en vervolgens downloaden en controleren van de tracering via de URL die wordt aangegeven door tekenarcering **ocp-apim-trace-locatie** . Dit via programmacode kan worden uitgevoerd en is ook mogelijk rechtstreeks vanuit de developer portal.

Deze zelfstudie wordt getoond hoe u de controle API moeten worden doelcellen bewerkingen de eenvoudige rekenmachine-API die is geconfigureerd in deze zelfstudie [beheren uw eerste API](api-management-get-started.md) ophalen slag te gaan gebruiken. Als u deze zelfstudie nog niet voltooid duurt alleen even de eenvoudige rekenmachine-API importeren of kunt u een andere API van uw keuze zoals de API Echo. Elk exemplaar van de service API Management wordt geleverd vooraf geconfigureerde met een Echo-API die kunnen worden gebruikt om te experimenteren met en meer informatie over API-Management. De API Echo geeft weer welke invoer wordt verzonden naar deze. Als u wilt gebruiken, kunt u een werkwoord HTTP roepen, en wordt de retourwaarde gewoon wat u hebt verzonden. 



Als u wilt beginnen, klikt u op **developer portal** in de klassieke Azure-Portal voor uw service API Management. Bewerkingen kunnen worden aangeroepen rechtstreeks vanuit de developer portal die is een handige manier om te bekijken en de bewerkingen van een API testen.

>Als u nog een exemplaar van de service API Management hebt gemaakt, ziet u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

![API Management developer portal][api-management-developer-portal-menu]

Klik op **API's** van het bovenste menu en klik vervolgens op **Eenvoudige rekenmachine**.

![Echo-API][api-management-api]

Klik op **het uitproberen** om te proberen de bewerking **toevoegen twee gehele getallen** .

![Het uitproberen][api-management-open-console]

Zorg dat de standaard parameterwaarden en selecteer de sleutel abonnement voor het product dat u wilt gebruiken vanaf de **abonnement-toets** pijl-omlaag.

Koptekst is al dan niet standaard in de developer portal de **Ocp-Apim-Trace** al ingesteld op **waar**. Deze koptekst configureert al dan niet een spoor wordt gegenereerd.

![Verzenden][api-management-http-get]

Klik op **verzenden** om aan te roepen met de bewerking.

![Verzenden][api-management-send-results]

Kopteksten wordt een **ocp-apim-trace-locatie** met een waarde die vergelijkbaar is met het volgende voorbeeld worden in de reactie.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

De tracering kan worden gedownload van de opgegeven locatie en beoordeeld zoals u in de volgende stap.

## <a name="inspect-trace"> </a>De tracering controleren

Als u wilt bekijken van de waarden in de trace, door de traceringsbestand te downloaden via de **ocp-apim-trace-locatie** -URL. Er is een tekstbestand in de indeling van JSON en bevat items die vergelijkbaar is met het volgende voorbeeld.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Vervolgstappen

-   Bekijk een video van het beleid expressies in aanwijzen [Cloud begeleidende aflevering 177: meer API beheerfuncties](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Vooruitspoelen op 21:00 om te zien van de video.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 