<properties 
    pageTitle="Overzicht Service Bus messaging voorbeelden | Microsoft Azure"
    description="Categoriseert en Service Bus voorbeelden met koppelingen naar elke messaging beschreven."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>SMS-service Bus-voorbeelden

De Service Bus SMS voorbeelden wordt getoond belangrijke functies in [Service Bus messaging](https://azure.microsoft.com/services/service-bus/) (cloudservice) en [Service Bus voor Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). In dit artikel wordt gecategoriseerd en wordt beschreven in de voorbeelden beschikbaar, met koppelingen naar elk.

>[AZURE.NOTE] Voorbeelden van de service Bus worden niet geïnstalleerd met de SDK. U kunt de voorbeelden downloaden van de [pagina met Azure SDK voorbeelden](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Daarnaast kunnen er is een bijgewerkte reeks Service Bus messaging voorbeelden [hier](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (schrijven van dit, ze zijn niet in dit artikel beschreven).  

Raadpleeg voor relay voorbeelden [Service Bus relay voorbeelden](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Service Bus messaging

In de volgende voorbeelden illustreren hoe u toepassingen die gebruikmaken van de Service Bus messaging schrijft.

Houd er rekening mee dat de SMS-berichten voorbeelden een verbindingsreeks voor toegang tot de naamruimte van uw Service Bus vereist.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Een verbindingsreeks ophalen voor Azure Service Bus

1. Meld u aan bij de [portal van Azure](http://portal.azure.com).

1. Klik in de linkerkolom op **Service-Bus**.

1. Klik op de naam van de naamruimte in de lijst.

3. Klik in het blad naamruimte op **clienttoegangsbeleid gedeeld**.

4. Klik in het blad **clienttoegangsbeleid gedeeld** op **RootManageSharedAccessKey**.

6. De verbindingsreeks naar het Klembord kopiëren.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Een verbindingsreeks ophalen voor Service Bus voor Windows Server

1. De volgende PowerShell-cmdlet uitvoeren:

    ```
    get-sbClientConfiguration
    ```

2. Plak de verbindingsreeks in het bestand App.config voor de steekproef.

### <a name="getting-started-samples"></a>Aan de slag te gaan voorbeelden

Deze voorbeelden wordt SMS basisfunctionaliteit beschreven.

|De naam van de steekproef|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Aan de slag: Messaging met wachtrijen](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Laat zien hoe u met Microsoft Azure-Service Bus berichten verzenden en ontvangen uit een wachtrij.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Aan de slag: Messaging met onderwerpen](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Laat zien hoe u met Microsoft Azure-Service Bus berichten verzenden en ontvangen van een onderwerp met meerdere abonnementen.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|

### <a name="exploring-features"></a>Functies verkennen

De volgende voorbeelden laten zien diverse functies van Service Bus.

|De naam van de steekproef|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[HTTP-Token-Providers](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Toont de verschillende manieren een HTTP/REST-client met Service Bus te verifiëren.|2.1|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Service Bus HTTP-Client](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Laat zien hoe berichten te verzenden en ontvangen van berichten van Service Bus via HTTP/REST.|2.3|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Service Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Laat zien hoe berichten uit een wachtrij, abonnement of wachtrij automatisch doorsturen naar een andere wachtrij of onderwerp. U ziet ook hoe u een bericht verzenden naar een wachtrij of onderwerp via een wachtrij doorverbinden.|2.3|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Voorbeeld van WCF-kanaal sessie](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Laat zien hoe het gebruik van Microsoft Azure-Service Bus Windows Communication Foundation (WCF) kanalen gebruiken. Het voorbeeld ziet het gebruik van WCF-kanalen verzenden en ontvangen van berichten via een Service Bus wachtrij. Het voorbeeld ziet u zowel sessie en niet-sessie communicatie via de Service-Bus.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: transacties](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Het gebruik van de Microsoft Azure-Service Bus messaging functies binnen een transactiebereik om ervoor te zorgen batches van messaging bewerkingen zijn vastgelegde ziet atomically.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Management bewerkingen REST gebruiken](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Laat zien hoe management bewerkingen op Service-bevinden met REST uit te voeren.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Resource-Provider REST API 's](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Laat zien hoe de nieuwe Service Bus RDFE REST API's gebruiken om naamruimten en SMS entiteiten te beheren.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: WCF-Service sessie steekproef](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Laat zien hoe Microsoft Azure-Service Bus met het model WCF-service gebruiken. Het voorbeeld ziet het gebruik van het model WCF-service voor het uitvoeren van sessie-communicatie via een Service Bus wachtrij.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Aanvraag antwoord](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Laat zien hoe de Bus Microsoft Azure-Service en de functionaliteit van de aanvraag en respons gebruiken. Het voorbeeld ziet eenvoudige clients en servers communiceren via een Service Bus wachtrij.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: onbestelbare wachtrij](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Laat zien hoe Microsoft Azure-Service Bus en de SMS-berichten "onbestelbare"-wachtrijfunctionaliteit gebruiken. Het voorbeeld ziet een eenvoudige afzender en de ontvanger communiceren via een Service Bus wachtrij.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Berichten uitgesteld](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Laat zien hoe de functie van de uitgestelde bericht van Microsoft Azure-Service Bus gebruiken. Het voorbeeld ziet een eenvoudige afzender en de ontvanger communiceren via een Service Bus wachtrij.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Berichten van sessie](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Laat zien hoe Microsoft Azure-Service Bus en de sessie Messaging-functionaliteit gebruiken. Het voorbeeld ziet u eenvoudige afzenders en ontvangers communiceren via een Service Bus wachtrij.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Verzoek antwoord onderwerp](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Laat zien hoe de aanvraag en respons patroon met Microsoft Azure-Service Bus onderwerpen en abonnementen implementeren. Het voorbeeld ziet eenvoudige clients en servers communiceren via een Service Bus-onderwerp.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Wachtrij voor aanvragen antwoord](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Laat zien hoe Microsoft Azure-Service Bus en de functionaliteit van de aanvraag en respons gebruiken. Het voorbeeld ziet eenvoudige clients en servers communiceren via twee wachtrijen voor Service Bus.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Dubbele detectie](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Laat zien hoe u Microsoft Azure-Service Bus dubbele bericht detectie met wachtrijen. Twee wachtrijen, een met dubbele detectie wilt inschakelen en andere een zonder dubbele detectie wordt gemaakt.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Asynchrone Messaging](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Laat zien hoe u met Microsoft Azure-Service Bus berichten verzenden en ontvangen asynchroon uit een wachtrij. De wachtrij biedt losgekoppelde, asynchrone communicatie tussen een afzender en een willekeurig aantal ontvangers (u kunt een enkele ontvanger).|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Geavanceerde Filters](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Laat zien hoe Microsoft Azure-Service Bus publiceren/abonneren geavanceerde filters gebruiken. Hiermee maakt u een onderwerp en 3 abonnementen met verschillende filterdefinities, verzonden berichten naar het onderwerp, en ontvangen alle berichten van een abonnement.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Brokered Messaging: Berichten Prefetch](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Laat zien hoe de functie prefetch van Microsoft Azure-Service Bus berichten wilt gebruiken. Laat het gebruik van de functie berichten prefetch na ontvangen.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|

## <a name="service-bus-reference-tools"></a>Service Bus verwijzingshulpmiddelen

De volgende voorbeelden laten zien diverse andere functies van de service.

|De naam van de steekproef|Beschrijving|Minimale SDK versie|Beschikbaarheid|
|---|---|---|---|
|[Service Bus Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|De Service Bus Explorer kunnen gebruikers verbinding maken met een naamruimte van de service-Service Bus en SMS entiteiten op een eenvoudige manier beheren. Het hulpmiddel bevat geavanceerde functies zoals importeren/exporteren functionaliteit en de mogelijkheid om te testen SMS entiteiten en relay services.|1,8|Microsoft Azure-Service Bus; Service Bus voor Windows Server|
|[Autorisatie: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|In dit voorbeeld ziet u hoe maken en beheren van service-identiteiten in Microsoft Azure Active Directory-toegangsbeheer (ook wel bekend als Access Control-Service of ACS) voor gebruik met Service Bus.|N/B|Microsoft Azure-Service Bus|

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor overzichten van concepten van Service Bus.

- [SMS-service Bus-overzicht](service-bus-messaging-overview.md)
- [Service-busarchitectuur](service-bus-architecture.md)
- [Service Bus over grondbeginselen](service-bus-fundamentals-hybrid-solutions.md)
