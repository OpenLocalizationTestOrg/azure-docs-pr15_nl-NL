<properties 
    pageTitle="Azure Relay hybride verbindingen Protocol | Microsoft Azure"
    description="zure Relay hybride verbindingen Protocol handleiding."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay hybride verbindingen-Protocol

Azure Relay is een van de de mogelijkheid van de belangrijkste pijlers van de Azure Service Bus-platform. De nieuwe "Hybride verbindingen" mogelijkheid van de Relay is een beveiligde, openen-protocol ontwikkeling op basis van HTTP en WebSockets.

Het eerste gelijkmatig met de naam "BizTalk Services"-functie die is gebaseerd op een eigen protocol foundation vervangt. De integratie van hybride verbindingen in Azure App Services blijven gebruiken als-is.

"Hybride verbindingen" kunnen bidirectionele, binaire stream communicatie tussen twee netwerk-toepassingen, waarbij een of beide van de partijen zich achter NAT's of Firewalls bevinden kunnen tot stand te brengen.

In dit document wordt beschreven aan de clientzijde interacties met de hybride verbindingen relay om verbinding te maken-clients in luisteraar ervan af en afzender rollen en hoe listeners nieuwe verbindingen geaccepteerd.

## <a name="interaction-model"></a>Interactie model

De hybride verbindingen relay verbindt twee partijen door een punt rendezvous in de Azure cloud dat beide partijen kunnen ontdekken en verbinding maken met vanuit het oogpunt van hun eigen netwerk te bieden. Dat punt rendezvous heet 'Hybride verbinding' in dit en andere documentatie, in de API's, en ook in de Portal Azure. Het eindpunt van de service hybride verbindingen zal worden verwezen als "service" voor de rest van dit document.

Het model interactie leans de nomenclatuur tot stand gebracht door vele andere netwerken API:

Er is een luisteraar ervan af die eerst wordt aangegeven gereedheid u omgaat met binnenkomende verbindingen en vervolgens accepteert wanneer deze binnenkomen. Aan de andere kant, moet u er een verbindende client die verbinding maakt de luisteraar ervan af, verwacht verbinding geaccepteerd voor het instellen van een pad bidirectionele communicatie is. "Verbinden", "Luisteren", "Accepteren" zijn dezelfde voorwaarden u op de meeste socket API's vindt.

Een communicatiemodel indirecte heeft beide partijen uitgaande verbindingen richting van een service-eindpunt, waarmee de "luisteraar ervan af" ook een 'client' gewone gebruikt en mogelijk ook andere overbelastingen terminologie; maken de exacte terminologie die we dus voor hybride verbindingen gebruiken is als volgt:

De programma's op beide zijden van een verbinding heten 'client', omdat ze klanten met de service. De client die wacht verbindingen accepteert is de "luisteraar ervan af' of worden in de"luisteraar ervan af rol' genoemd. De client waarmee een nieuwe verbinding richting van een luisteraar ervan af via de service wordt de 'afzender' genoemd of in de "afzender rol".

## <a name="listener-interactions"></a>Luisteraar ervan af interacties

De luisteraar ervan af heeft vier interacties met de service; alle kabel details worden verderop in dit document in het gedeelte naslaginformatie beschreven.

### <a name="listen"></a>Luisteren

Om aan te geven van de gereedheid van de service die wordt gereed om te accepteren verbindingen, dat wordt gemaakt een uitgaande Webverbinding socket. De verbinding handshake wordt de naam van de verbinding van een hybride geconfigureerd in de naamruimte Relay en een beveiligingstoken dat de "luisteren" dan verleent die een naam geven. Wanneer de web-socket is geaccepteerd door de service, de registratie is voltooid en de socket waarop deze is opgezet web leven als het 'besturingselement kanaal' voor het inschakelen van alle opeenvolgende interacties wordt bewaard. De service kunnen maximaal 25 gelijktijdige listeners een hybride verbinding. Als er 2 of meer active listeners, worden binnenkomende verbindingen worden verdeeld over deze in een willekeurige volgorde; reële verdeling is niet gegarandeerd.

### <a name="accept"></a>Accepteren

Wanneer u een afzender wordt geopend een nieuwe verbinding in de service, wordt de service kiezen en een van de actieve listeners op de verbinding hybride een melding. De melding wordt verzonden naar de luisteraar via het kanaal openen besturingselement als een JSON-bericht met de URL van het Web socket-eindpunt dat de luisteraar ervan af koppelen moet aan voor het accepteren van de verbinding.

De URL kunt en rechtstreeks door de luisteraar ervan af zonder extra werk; moet worden gebruikt de gecodeerde informatie geldt alleen voor een korte periode, in principe gedurende de periode waarin de afzender zich bereid te wachten totdat de verbinding waarop deze is opgezet end-to-end worden, maar maximaal 30 seconden. De URL kan alleen worden gebruikt voor een succesvolle verbinding. Zodra de Web socketverbinding met de URL rendezvous is gemaakt, worden alle verdere activiteit voor deze socket Web wordt doorgegeven van en tot de afzender, zonder tussenkomst of interpretatie door de service.

### <a name="renew"></a>Verlengen 

Terwijl de luisteraar ervan af actief is het mogelijk dat het beveiligingstoken die moet worden gebruikt om te registreren van de luisteraar ervan af en voor het behoud van het besturingselement kanaal verloopt. Het token verstrijken is niet van invloed op de lopende verbindingen, maar wordt het kanaal besturingselement verloren gaan door de service bij of direct nadat het chatgesprek vervaldatum. De beweging 'verlengen' is een JSON-bericht dat de luisteraar ervan af voor het vervangen van het token dat is gekoppeld aan het kanaal besturingselement, zodat het kanaal besturingselement kan worden bijgehouden voor langere kunt verzenden.

### <a name="ping"></a>Ping 

Als het kanaal besturingselement niet actief geweest lange tijd, tussenpersonen voor de manier waarop blijft, zoals laden balancers of NAT's mogelijk weghalen de TCP-verbinding. De beweging "ping" voorkomt dat door te sturen van een kleine hoeveelheid gegevens op het kanaal die eraan iedereen op de netwerkroute die de verbinding is bedoeld als levend zijn herinnert, en deze ook vormt een liveness-toets voor de luisteraar ervan af. Als de ping mislukt, het besturingselement kanaal onbruikbaar moet worden beschouwd en de luisteraar ervan af moet opnieuw verbinding maakt.

## <a name="sender-interaction"></a>Afzender interactie

De afzender heeft slechts één interactie met de service, verbinding wordt gemaakt.

### <a name="connect"></a>Verbinding maken

De beweging 'koppelen' een socket Web op de service, met de naam van de hybride verbinding wordt geopend en een (optioneel, maar vereist al dan niet standaard) beveiligingstoken dat machtiging 'Verzenden' in de query. De service wordt vervolgens interactie met de luisteraar ervan af in zoals hierboven is beschreven en de luisteraar ervan af het opzetten van een verbinding rendezvous die met deze socket Web koppelt hebt. Nadat de Web-socket is geaccepteerd, dus alle verdere interacties op de Web-socket wordt met een verbonden luisteraar ervan af.

## <a name="interaction-summary"></a>Interactie samenvatting

Het resultaat van dit model interactie is dat de client van de afzender afkomstig zijn uit de handshake met een "schone" Web socket die is verbonden met een luisteraar ervan af en die geen verdere preambles of voorbereiding moet. Hiermee kunt vrijwel elk bestaande Web socket client-implementatie om gemakkelijk te profiteren van de service hybride verbindingen opnemen door het opgeven van de URL van een correct opgemaakte in hun Web socket clientlaag.

De verbinding rendezvous Web Socket waarmee de luisteraar ervan af wordt opgehaald door de interactie accepteren ook schoon is en kan worden doorgestuurd naar een bestaande Web socket server-implementatie met enkele minimale extra abstractie die wordt onderscheid tussen 'accepteren' bewerkingen op van hun framework lokale netwerk listeners en hybride-verbindingen externe "accepteren" bewerkingen gemaakt.

## <a name="protocol-reference"></a>Protocol verwijzing

Dit onderwerp vindt de details van het protocol interacties hierboven is beschreven.

Alle Web socketverbindingen worden uitgevoerd op poort 443 als een upgrade van HTTPS 1.1, waarvan de meest beschouwing door sommige Web socket framework of API. De beschrijving van de hier wordt bewaard implementatie neutrale, zonder suggesties voor een specifieke kader.

## <a name="listener-protocol"></a>Luisteraar ervan af protocol

Het protocol luisteraar ervan af bestaat uit twee verbinding aanraakbewegingen en drie bericht bewerkingen.

### <a name="listener-control-channel-connection"></a>Luisteraar ervan af besturingselement kanaalverbinding

Het besturingselement kanaal wordt geopend met het maken van een Web socketverbinding met:

*wss: / / {naamruimte-adres} /* *$servicebus* */* *hybridconnection /**{path}? sb-CH-actie =... & sb-CH-id =... & sb-CH-token =... '*

De *naamruimte-adres* is de FQDN-naam van de Azure Relay naamruimte waarop de hybride verbinding, meestal aan het formulier {*mijnnaam}. servicebus.windows.net.*

De query tekenreeks parameteropties zijn als volgt

| Parameter        | Vereist? | Beschrijving                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-CH-actie | Ja       | Voor de rol van de luisteraar ervan af de parameter moet **sb-CH-actie luisteren =**                                                                                                                                |
| {path}       | Ja       | Het URL-codering naamruimtepad van de vooraf geconfigureerde hybride-verbinding met deze luisteraar ervan af op registreren. Deze expressie wordt toegevoegd aan de vaste * **$servicebus**/**hybridconnection /*** padgedeelte. |
| SB-CH-token  | Ja\*     | De luisteraar ervan af moet een geldige, URL-codering Service Bus gedeeld Access Token opgeven voor de naamruimte of hybride verbinding die het recht **beluisteren geeft** .                                           |
| SB-CH-id     | Nee        | Deze client geleverde optioneel-ID kunt end-to-end diagnostische tracing.                                                                                                                             |

Als de Web Socket-verbinding vanwege de hybride verbinding pad niet wordt geregistreerd mislukt, of een ongeldige of ontbrekende token of een andere fout, de feedback fout krijgt met het normale HTTP 1.1 status feedback-model. De statusbeschrijving van de bevat een fout bijhouden-id die kan worden doorgegeven aan Azure ondersteuning:

| Code | Fout          | Beschrijving                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Niet gevonden      | De verbinding van de hybride **pad** ongeldig is of de basis-URL is onjuist |
| 401  | Onbevoegde   | Het beveiligingstoken is ontbrekende of onjuiste of ongeldige                  |
| 403  | Verboden      | Het beveiligingstoken geldt niet voor dit pad voor deze actie          |
| 500  | Interne fout | Is een fout opgetreden in de service                                    |

Als de Web socket-verbinding is met opzet hebt afgesloten door de service nadat deze is in eerste instantie ingesteld, de reden hiervoor zodat wordt worden verstrekt met behulp van een juiste Web socket protocol foutcode samen met een beschrijvende foutbericht wordt weergegeven die ook een bijhouden-id bevat. De service wordt niet op het besturingselement kanaal afgesloten zonder dat er een fout. Een afgesloten is client beheerd.

| WS Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Het pad hybride verbinding is verwijderd of uitgeschakeld.                           |
| 1008      | Het beveiligingstoken is verlopen en het beleid wordt daarom geschonden. |
| 1011      | Er is iets fout gegaan binnen de service.                                           |

### <a name="accept-handshake"></a>Handshake accepteren 

De melding accepteren is verzonden door de service naar de luisteraar ervan af via het kanaal eerder vastgestelde besturingselement als een bericht JSON in een frame Web socket tekst. Er is geen antwoord op dit bericht.

Het bericht bevat een JSON object met de naam 'accepteren', waarin de volgende eigenschappen op dit moment:

-   **adres** : de URL-tekenreeks moet worden gebruikt voor het instellen van de Web-Socket de service nodig om een binnenkomende verbinding te accepteren.

-   **id** – de unieke id voor deze verbinding. Als de-id die is verstrekt door de afzender-client, is de waarde van de afzender die zijn opgegeven, anders is de waarde van een systeem gegenereerd.

-   **connectHeaders** – alle HTTP-headers die zijn opgegeven naar het eindpunt Relay door de afzender, waaronder ook het Sec-WebSocket-Protocol en de koppen Sec-WebSocket-extensies.

| Bericht accepteren                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

De adres-URL die u in het bericht JSON wordt gebruikt door de luisteraar ervan af tot stand brengen van de Web-Socket voor goedkeuren of weigeren van de afzender socket.

#### <a name="accepting-the-socket"></a>Acceptatie van de socket

Als u wilt accepteren, maakt de luisteraar ervan af WebSocket verbinding met het adres van het opgegeven.

Er rekening mee dat voor de preview-periode, het adres URI een absoluut IP-adres gebruikt mogelijk en het TLS-certificaat dat is opgegeven door de server dat adres worden gevalideerd mislukt. Dit zullen worden verholpen in de voorbeeldweergave.

Als het bericht 'accepteren' aanwezig zijn op de kop van een 'Sec-WebSocket-Protocol', wordt verwacht dat de luisteraar ervan af alleen de WebSocket worden geaccepteerd als het protocol ondersteunt en dat deze de koptekst instelt, zoals de WebSocket tot stand is gebracht.

Dit geldt ook voor de koptekst "Sec-WebSocket-extensies". Als het kader extensie ondersteunt, moet deze de koptekst ingesteld op het antwoord van de *server* -kant van de vereiste "Sec-WebSocket-extensies" handshake voor de extensie.

De URL moet worden gebruikt als-is voor het instellen van de socket accepteren, maar bevat de volgende parameters:

| Parameter        | Vereist? | Beschrijving                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-CH-actie | Ja       | Voor het accepteren van een socket de parameter moet **sb-CH-actie = accepteren**                                                                                                                                                                                                                          |
| {path}       | Ja       | Het URL-codering naamruimtepad van de vooraf geconfigureerde hybride-verbinding met deze luisteraar ervan af op registreren. Deze expressie wordt toegevoegd aan de vaste * **$servicebus**/**hybridconnection /*** padgedeelte.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB-CH-id | No        | Zie de beschrijving van **id** .                                                                                                                                                                                                                                                              |

Als er een fout treedt, de service als volgt mogelijk beantwoorden:

| Code | Fout          | Beschrijving                         |
|------|----------------|-------------------------------------|
| 403  | Verboden      | De URL is niet geldig.               |
| 500  | Interne fout | Is een fout opgetreden in de service |

Nadat de verbinding tot stand is gebracht, wordt de server de Web-socket afgesloten wanneer de afzender Web socket wordt afgesloten omlaag of met de volgende status

| WS Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | De client van de afzender is afgesloten van de verbinding                                        |
| 1001      | Het pad hybride verbinding is verwijderd of uitgeschakeld.                           |
| 1008      | Het beveiligingstoken is verlopen en het beleid wordt daarom geschonden. |
| 1011      | Er is iets fout gegaan binnen de service.                                           |

#### <a name="rejecting-the-socket"></a>De socket weigeren

De socket weigeren na het controleren van het bericht 'accepteren' is een soortgelijke handshake vereist, zodat de statuscode en de beschrijving van de status de reden voor de geweigerde communiceren kunnen flow terug naar de afzender.

Het protocol ontwerpkeuze hier is via een WebSocket handshake (dat geschikt is voor het laatste teken een gedefinieerde foutstatus) zodat luisteraar ervan af client implementaties kunt gaat u verder met de aanmelding van een client WebSocket en hoeft te gebruiken van een extra, b HTTP-client.

Als u wilt negeren de socket, de client duurt het adres URI uit het bericht 'accepteren' en voegt twee queryreeks-parameters toe aan deze:

| Parameter             | Vereist? | Beschrijving                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Ja       | Numerieke HTTP-statuscode                |
| statusDescription | Ja       | Menselijke leesbare reden voor de weigering |

De resulterende URI wordt vervolgens gebruikt tot stand brengen van een verbinding WebSocket; Klik nogmaals mee dat het certificaat TLS mogelijk niet overeenkomen met het adres in de voorbeeldweergave, zodat validatie misschien moet zijn uitgeschakeld.

Als het goed is voltooid, mislukt dit handshake met opzet hebt met een foutcode HTTP 410, aangezien er geen WebSocket tot stand is gebracht. Als u een fout optreedt, zijn de opties:

| Code | Fout          | Beschrijving                         |
|------|----------------|-------------------------------------|
| 403  | Verboden      | De URL is niet geldig.               |
| 500  | Interne fout | Is een fout opgetreden in de service |

### <a name="listener-token-renewal"></a>Luisteraar ervan af token verlenging

Wanneer de luisteraar ervan af token bijna verlopen is, kunt deze dit vervangen door een frame tekstbericht verzenden naar de service via het kanaal waarop deze is opgezet besturingselement. Het bericht bevat een JSON-object met de naam 'renewToken', waarin de volgende eigenschap op dit moment:

-   **token** – een geldige, URL-codering Service Bus gedeeld Access Token voor de naamruimte of hybride verbinding die het recht **beluisteren geeft** .

| renewToken bericht                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Als de token validatie mislukt, toegang en cloudservice het besturingselement kanaal websocket met een fout wordt gesloten, anders kan is er geen antwoord.

| WS Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Het beveiligingstoken is verlopen en het beleid wordt daarom geschonden. |

<a name="sender-protocol"></a>Afzender protocol
---------------

Het protocol afzender is in feite gelijk is aan hoe een luisteraar ervan af tot stand is gebracht. Het doel is de maximale doorzichtigheid voor de end-to-end Web socket. Het adres verbinding maken met is hetzelfde als voor de luisteraar ervan af, maar de "" varieert en het token heeft een andere toestemming nodig:

*wss: / / {naamruimte-adres} /* *$servicebus* */* *hybridconnection /**{path}? sb-CH-actie =... & sb-CH-id =... \[& sbc-CH-token =... \]*

De *naamruimte-adres* is de FQDN-naam van de Azure Relay naamruimte waarop de hybride verbinding, meestal aan het formulier {*mijnnaam}. servicebus.windows.net.*

Het verzoek kan willekeurige extra HTTP-headers, waaronder bestanden toepassingsspecifieke bevatten. Alle opgegeven kopteksten flow naar de luisteraar en kunnen worden gevonden op het object "connectHeader" van het besturingselement 'accepteren' bericht.

De query tekenreeks parameteropties zijn als volgt

| Parameter        | Vereist? | Beschrijving                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-CH-actie | Ja       | Voor de rol van de luisteraar ervan af de parameter moet **actie = verbinding**                                                                                                                                                    |
| {path}       | Ja       | Het URL-codering naamruimtepad van de vooraf geconfigureerde hybride-verbinding met deze luisteraar ervan af op registreren.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB-CH-token | Yes\*     | De luisteraar ervan af moet een geldige, URL-codering Service Bus gedeeld Access Token opgeven voor de naamruimte of hybride verbinding die het recht **verzenden geeft** .                                                            | | SB-CH-id | No        | Een optioneel-ID die kunt end-to-end diagnostische tracering en beschikbaar wordt gesteld naar de luisteraar tijdens de handshake accepteren.                                                                                       |

Als de Web Socket-verbinding vanwege de hybride verbinding pad niet wordt geregistreerd mislukt, of een ongeldige of ontbrekende token of een andere fout, de feedback fout krijgt met het normale HTTP 1.1 status feedback-model. De statusbeschrijving van de bevat een fout bijhouden-id die kan worden doorgegeven aan Azure ondersteuning:

| Code | Fout          | Beschrijving                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Niet gevonden      | De verbinding van de hybride **pad** ongeldig is of de basis-URL is onjuist |
| 401  | Onbevoegde   | Het beveiligingstoken is ontbrekende of onjuiste of ongeldige                  |
| 403  | Verboden      | Het beveiligingstoken geldt niet voor dit pad voor deze actie          |
| 500  | Interne fout | Is een fout opgetreden in de service                                    |

Als de Web socket-verbinding is met opzet hebt afgesloten door de service nadat deze is in eerste instantie ingesteld, de reden hiervoor zodat wordt worden verstrekt met behulp van een juiste Web socket protocol foutcode samen met een beschrijvende foutbericht wordt weergegeven die ook een bijhouden-id bevat.

| WS Status | Beschrijving                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | De luisteraar ervan af is afgesloten van de socket.                                                 |
| 1001      | Het pad hybride verbinding is verwijderd of uitgeschakeld.                           |
| 1008      | Het beveiligingstoken is verlopen en het beleid wordt daarom geschonden. |
| 1011      | Er is iets fout gegaan binnen de service.                                           |

## <a name="next-steps"></a>Volgende stappen:

- [Relay Veelgestelde vragen](relay-faq.md)
- [Een naamruimte maken](relay-create-namespace-portal.md)
- [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Aan de slag met knooppunt](relay-hybrid-connections-node-get-started.md)