<properties
    pageTitle="Geavanceerde aanvraag beperken met Azure API Management."
    description="Informatie over het maken en toepassen flexibele quota en tarief van beleid met Azure API Management beperken."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Geavanceerde aanvraag beperken met Azure API Management.

Verzoeken voor oproepen beperken het is een belangrijke rol van Azure API Management. Hetzij API Management toegestaan door het tarief weer dat van serviceaanvragen of de totale aanvragen/gegevens worden overgedragen bepalen, API-providers om hun API's beschermen tegen abuse en waarde voor verschillende API product lagen te maken.

## <a name="product-based-throttling"></a>Product gebaseerd beperken
Datum, de rente beperken tot zijn mogelijkheden beperkt tot wordt beperkt tot een bepaald Product-abonnement (in feite een sleutel), gedefinieerd in de publisher-API-beheerportal. Dit is handig voor de API-provider beperkingen toepassen op de ontwikkelaars die hebben aangemeld bij hun API gebruiken, maar deze niet helpt, bijvoorbeeld in afzonderlijke eindgebruikers van de API beperken. Het is mogelijk dat voor één gebruiker van de ontwikkelaar van de toepassing te gebruiken van het hele quotum en vervolgens voorkomen dat andere klanten van de ontwikkelaar gebruikmaken van de toepassing. Meerdere klanten die mogelijk een groot aantal aanvragen genereren mogelijk ook toegang tot incidentele gebruikers beperken.

## <a name="custom-key-based-throttling"></a>Aangepaste sleutel gebaseerd beperken
Het nieuwe [tarieven-limiet-door-sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quotum door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) beleid bieden een aanzienlijk flexibeler oplossing aan een besturingselement voor verkeer. Deze nieuwe beleidsregels kunnen u expressies voor het aanduiden van de toetsen die wordt gebruikt voor het bijhouden van verkeer gebruik definiëren. De die dit werkt wordt geïllustreerd hoe u easiest met een voorbeeld. 

## <a name="ip-address-throttling"></a>IP-adres beperken
De volgende beleidsitems beperken tot tot een enkele client IP-adres slechts 10 oproepen elke minuut, met een totaal van 1.000.000 oproepen en 10.000 kilobytes bandbreedte per maand. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Als alle clients op Internet een unieke IP-adres hebt gebruikt, is dit mogelijk een efficiënte manier gebruik per gebruiker te beperken. Het is echter helemaal waarschijnlijk dat meerdere gebruikers wordt één openbare IP-adres vanwege ze toegang tot Internet via een NAT-apparaat delen. Nadat u dit doet voor API's die niet-geverifieerde toegang toestaan de `IpAddress` mogelijk de beste optie.

## <a name="user-identity-throttling"></a>Gebruiker identiteit beperken
Als een eindgebruiker is geverifieerd, en een bandbreedteregeling sleutel kan worden gegenereerd op basis van de informatie die deze identificeert op een die gebruiker.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

In dit voorbeeld we extraheren van de koptekst autorisatie, converteren naar `JWT` object en gebruikt u het onderwerp van het token herkennen van de gebruiker en gebruiken als het tarief weer dat toets beperken. Als de identiteit van de gebruiker is opgeslagen in de `JWT` als een van de andere vervolgens vorderingen dat de waarde in plaats daarvan kan worden gebruikt.

## <a name="combined-policies"></a>Beleid voor het gecombineerde
Hoewel het nieuwe bandbreedteregeling beleid meer controle dan de bestaande bandbreedteregeling beleidsregels bieden, is er nog steeds waarde beide mogelijkheden combineren. Beperken door de productcode van abonnement ([limiet gesprek tarieven per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en [instellen van de quota Resourcegebruik door abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is een uitstekende manier om het inschakelen van een API monetizing door opladen op basis van gebruik niveaus. Het meer korrelig besturingselement niet in staat beperken door gebruiker is een aanvulling en voorkomen dat één gebruiker gedrag daardoor verslechteren de ervaring van een ander. 

## <a name="client-driven-throttling"></a>Client basis van hoeveelheid werk beperken
Wanneer de bandbreedteregeling sleutel is gedefinieerd met behulp van een [beleidsexpressie](https://msdn.microsoft.com/library/azure/dn910913.aspx), is de API-provider die is kiezen hoe de beperking is beperkt. Echter een ontwikkelaar handig om te bepalen hoe ze classificeren limiet hun eigen klanten. Dit kan worden ingeschakeld door de provider API door de introductie van een aangepaste kop toe te staan dat de clienttoepassing van de ontwikkelaar kunt u de toets tot de API communiceren.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Hiermee worden de clienttoepassing van de ontwikkelaar om te kiezen hoe ze willen maken van het tarief weer dat toets beperken. Met een beetje draaien kan een ontwikkelaar client hun eigen niveaus tarief maken door sets met toetsen toewijzen aan gebruikers en het gebruik van de sleutel draaien.

## <a name="summary"></a>Overzicht
Azure API beheer biedt de snelheid en offerte beperken als u wilt beveiligen zowel waarde toevoegen aan uw service API. Het nieuwe bandbreedteregeling beleid met aangepaste bereikregels kunnen u meer korrelig controle over beleid zodat uw klanten nog beter toepassingen maken. Het gebruik van deze nieuwe beleid de voorbeelden in dit artikel worden door manufacturing tarief beperken toetsen met client IP-adressen, gebruikers-id en waarden van de client die zijn gegenereerd. Er zijn echter vele andere delen van het bericht dat kan worden gebruikt zoals gebruikersagent, de URL-pad fragmenten, de grootte van het bericht.

## <a name="next-steps"></a>Volgende stappen
Geef ons uw feedback in de thread Disqus voor in dit onderwerp. Deze zou fantastische voor informatie over andere mogelijke sleutelwaarden die een logische keuze in uw scenario's zijn.

## <a name="watch-a-video-overview-of-these-policies"></a>Bekijk een video-overzicht van dit beleid
Voor meer informatie over het [rente-limiet-door-sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quotum door sleutel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) beleid beschreven in dit artikel, neemt u de volgende video bekijken.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
