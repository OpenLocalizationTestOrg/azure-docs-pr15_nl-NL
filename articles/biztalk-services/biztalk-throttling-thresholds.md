<properties 
    pageTitle="Meer informatie over het beperken van BizTalk Services | Microsoft Azure" 
    description="Meer informatie over drempelwaarden beperken en resulteert runtime gedrag voor BizTalk-Services. Beperken is gebaseerd op geheugengebruik en het aantal berichten. MAB, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>BizTalk-Services: beperken

Azure BizTalk Services implementeert service beperken op basis van twee voorwaarden: geheugengebruik en het aantal tegelijk berichten verwerken. In dit onderwerp vindt de bandbreedteregeling drempelwaarden en een beschrijving van het gedrag Runtime wanneer een bandbreedteregeling voorwaarde plaatsvindt.

## <a name="throttling-thresholds"></a>Drempelwaarden beperken

De volgende tabel bevat de bandbreedteregeling bron en de drempels:

||Beschrijving|Lage drempel|Hoge drempelwaarde|
|---|---|---|---|
|Geheugen|% van totaal systeem geheugen beschikbaar/PageFileBytes. <p><p>Totaal beschikbare PageFileBytes is ongeveer 2 maal het RAM van het systeem.|60%|70%|
|Verwerking van berichten|Aantal berichten tegelijk verwerken|40 * aantal cores|100 * aantal cores|

Wanneer u een hoge drempelwaarde wordt bereikt, begint Azure BizTalk Services beperken. Beperken kleurovergangsbeëindigingen wanneer de lage drempelwaarde is bereikt. Uw service wordt bijvoorbeeld 65% systeemgeheugen gebruiken. In dit geval wordt de service niet beperken. Uw service wordt gestart met behulp van 70% systeemgeheugen. In dit geval de service bandbreedte en zich blijft voordoen beperken tot de service wordt gebruikt voor 60% (lage drempel) systeemgeheugen.

Azure BizTalk-Services kunt u de bandbreedteregeling status (normale status versus vertraagd staat) en de bandbreedteregeling duur bijhouden.


## <a name="runtime-behavior"></a>Runtime gedrag

Wanneer u Azure BizTalk Services in een bandbreedteregeling staat, gebeurt het volgende:

- Beperken is per rol exemplaar. Bijvoorbeeld:<br/>
RoleInstanceA is beperken. RoleInstanceB is niet beperken. In dit geval worden berichten in RoleInstanceB worden verwerkt zoals verwacht. Berichten in RoleInstanceA worden verwijderd en mislukken met het volgende foutbericht weergegeven:<br/><br/>
**Server is bezet. Probeer het opnieuw.**<br/><br/>
- Alle bronnen halen niet poll uitvoeren onder of downloaden van een bericht. Bijvoorbeeld:<br/>
Een pijplijn Hiermee worden berichten van een externe FTP-bron. Het exemplaar van de rol wijze de halen krijgt in een bandbreedteregeling staat. In dit geval de pijplijn stopt downloaden van extra berichten totdat het exemplaar van de functie niet meer beperken.
- Een antwoord wordt verzonden naar de klant, zodat de client kan het bericht opnieuw aan te bieden.
- U moet wachten tot de beperking opgelost is. Specifiek, moet u wachten totdat de lage drempelwaarde is bereikt.

## <a name="important-notes"></a>Belangrijke notities
- Beperken kan niet worden uitgeschakeld.
- Drempelwaarden beperken kan niet worden gewijzigd.
- Beperken, wordt de hele systeem geïmplementeerd.
- De Azure SQL Database-Server, heeft ook ingebouwde beperken.

## <a name="additional-azure-biztalk-services-topics"></a>Aanvullende Azure BizTalk Services onderwerpen

-  [Installatie van de Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Zelfstudies: Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zie ook
- [BizTalk Services: Ontwikkelaars, Basic, Standard en Premium edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Inrichting klassieke portal Azure gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Inrichting Status grafiek](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk Services: De tabbladen Dashboard, Monitor en schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Back-up maken en deze herstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk Services: De naam van de uitgever en de uitgever sleutel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
