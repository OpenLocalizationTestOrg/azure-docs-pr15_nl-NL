<properties
    pageTitle="Meer informatie over functies in BizTalk Services edities | Microsoft Azure"
    description="De mogelijkheden van de edities BizTalk-Services vergelijken: vrij, ontwikkelaars, Basic, Standard en Premium. MAB, WABS."
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
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk Services: Edities grafiek

Azure BizTalk Services biedt verschillende versies. Lees dit artikel om te bepalen welke versie geschikt is voor uw scenario en zakelijke behoeften.


## <a name="compare-the-editions"></a>De edities vergelijken

**Vrij te geven (Preview)**

Kunt maken en beheren van hybride verbindingen. Een hybride-verbinding is een eenvoudige manier om een Azure website verbinden met een on-premises-systeem, zoals SQL Server.

**Ontwikkelaars**

Verwerking van de berichten hybride verbindingen, EAI en bewerken met een eenvoudig-en-klare handelen partner management portal en ondersteuning voor algemene bewerken schema's en uitgebreide bewerken verwerking bevat via X12 en AS2. Gebruikelijke scenario's voor EAI services in de cloud verbinding met een HTTP/S, REST, FTP, WCF en SFTP protocollen te lezen en schrijven van berichten kunt worden maken.  Connectiviteit met on-premises implementatie LOB-systemen met kant-en-klare SAP, Oracle eBusiness, Oracle DB, Siebel en SQL Server-adapter gebruiken. Een ontwikkelaar centraal-omgeving gebruiken met Visual Studio tools voor eenvoudig ontwikkeling en implementatie. Beperkt tot ontwikkeling en test doeleinden alleen met geen Service niveau overeenkomst (SLA).

**Eenvoudig**

Bevat de meeste ontwikkelaars mogelijkheden met toeneemt in hybride verbindingen, EAI bruggen bewerken overeenkomsten en BizTalk Adapter Pack verbindingen. Ook biedt beschikbaarheid en de optie voor het schalen met een Service Level (Agreement).

**Standaard**

Bevat alle essentiële mogelijkheden met toeneemt in hybride verbindingen, EAI bruggen bewerken overeenkomsten en BizTalk Adapter Pack verbindingen. Ook biedt beschikbaarheid en de optie voor het schalen met een Service Level (Agreement).

**Premium**

Bevat alle standaard mogelijkheden met toeneemt in hybride verbindingen, EAI bruggen bewerken overeenkomsten en BizTalk Adapter Pack verbindingen. Bevat ook archiveren, beschikbaarheid en de optie voor het schalen met een Service Level (Agreement).


## <a name="editions-chart"></a>Edities grafiek
De volgende tabel bevat de verschillen.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Vrij te geven (Preview)</th>
        <th>Ontwikkelaars</th>
        <th>Eenvoudige</th>
        <th>Standaard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Prijs starten</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Azure BizTalk Services prijzen</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure Rekenmachine prijzen</a></td>
</tr>
<tr>
<td><strong>Standaard minimale configuratie</strong></td>
<td>1 gratis eenheid</td>
<td>1 ontwikkelaars per eenheid</td>
<td>1 basiseenheid</td>
<td>1 standaardeenheid</td>
<td>1-eenheid</td>
</tr>
<tr>
<td><strong>Schaal</strong></td>
<td>Geen schaal</td>
<td>Geen schaal</td>
<td>Ja, in de stappen 1 basiseenheid</td>
<td>Ja, in de stappen 1 standaardeenheid</td>
<td>Ja, in de stappen 1-eenheid</td>
</tr>
<tr>
<td><strong>Maximaal toegestane schaal af</strong></td>
<td>Geen schaal</td>
<td>Geen schaal</td>
<td>Maximaal 8 eenheden</td>
<td>Maximaal 8 eenheden</td>
<td>Maximaal 8 eenheden</td>
</tr>
<tr>
<td><strong>EAI bruggen per eenheid</strong></td>
<td>Niet opgenomen</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>BEWERKEN, AS2</strong>
<br/><br/>
TPM overeenkomsten bevat</td>
<td>Niet opgenomen</td>
<td>Opgenomen. 10 overeenkomsten per eenheid.</td>
<td>Opgenomen. 50 overeenkomsten per eenheid.</td>
<td>Opgenomen. 250 overeenkomsten per eenheid.</td>
<td>Opgenomen. 1000 overeenkomsten per eenheid.</td>
</tr>
<tr>
<td><strong>Hybride verbindingen per eenheid</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hybride verbinding gegevens doorverbinden (GB) per eenheid</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk Adapter Service-verbindingen met on-premises implementatie LOB-systemen</strong></td>
<td>Niet opgenomen</td>
<td>1 verbinding</td>
<td>2-verbindingen</td>
<td>5-verbindingen</td>
<td>25 verbindingen</td>
</tr>
<tr>
<td align="left"><strong>Ondersteunde protocollen/systemen:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus (SB)</li>
<li>Azure Blob</li>
<li>REST API 's</li>
</ul>
</td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Beschikbaarheid</strong>
<br/><br/>
Voor Sla (Service niveau Agreement), Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk Services prijzen</a>.
</td>
<td>Niet opgenomen</td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Back-up en herstellen</strong></td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Bijhouden</strong></td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Archiveren</strong><br/><br/>
Bevat niet betrouwbaar ontvangst (NRR) en de berichten worden bijgehouden</td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Niet opgenomen</td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Gebruik van aangepaste code</strong></td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
<tr>
<td><strong>Gebruik van transformaties, inclusief aangepaste XSLT</strong></td>
<td>Niet opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
<td>Opgenomen</td>
</tr>
</table>

> [AZURE.NOTE] Voor tolerantie tegen hardwarefouten houdt beschikbaarheid met meerdere VMs binnen één BizTalk eenheid.


## <a name="faqs"></a>Veelgestelde vragen

#### <a name="what-is-a-biztalk-unit"></a>Wat is een eenheid BizTalk?
Een "eenheid' is het atomaire niveau van een implementatie van Azure BizTalk-Services. Elke versie wordt geleverd met een eenheid met verschillende berekeningscluster capaciteit en geheugen. Bijvoorbeeld een basiseenheid heeft meer berekeningscluster dan ontwikkelaars, standaard meer berekeningscluster dan eenvoudige, enzovoort. Wanneer u een BizTalk Service wilt verkleinen, wordt u schalen met eenheden.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Wat is het verschil tussen BizTalk-Services en Azure BizTalk VM?
BizTalk Services biedt een waar Platform-als-een-Service (PaaS)-architectuur voor het ontwikkelen van oplossingen voorstelt integratie in de cloud. Met het model PaaS u volledig richten op de toepassingslogica en laat alle het infrastructuurbeheer van Microsoft, waaronder:

- Niet nodig om te beheren of patch virtuele machines.
- Microsoft zorgt ervoor dat beschikbaarheid.
- U bepalen schaal op verzoek door het aanvragen van een gewoon meer of minder capaciteit via de portal van Azure.

BizTalk Server op Azure virtuele Machines biedt een architectuur infrastructuur-als-een-Service (IaaS). U virtuele machines maken en configureren precies zoals uw on-premises omgeving, om de bestaande toepassingen uitvoeren in de cloud zonder code wijzigingen te vereenvoudigen. Met IaaS bent u nog steeds verantwoordelijk voor het configureren van de virtuele machines, de virtuele machines (bijvoorbeeld installeren van software en OS patches) beheren en aanpassen van de toepassing voor maximale beschikbaarheid.

Als u op zoek bent bij het ontwikkelen van nieuwe integratie van oplossingen voorstelt die uw infrastructuur management hoeveelheid minimaliseren, gebruikt u BizTalk-Services. Als u op zoek bent naar snel migreren van uw bestaande BizTalk-oplossingen of zoekt u een op aanvraag-omgeving ontwikkelen en testen van BizTalk Server-toepassingen, BizTalk Server op een Azure virtuele Machine gebruiken.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Wat is het verschil tussen BizTalk Adapter Service en hybride verbindingen?
De BizTalk Adapter-Service wordt gebruikt door een Azure BizTalk-Service. De BizTalk Adapter-Service gebruikt de BizTalk Adapter Pack verbinding maken met een on-premises lijn van Business (LOB)-systeem. De verbinding van een hybride biedt een eenvoudige en handige manier verbinding maken van Azure-toepassingen, zoals de Web Apps-functie in Azure App-Service en Azure Mobile Services, met een on-premises implementatie-bron.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Wat betekent 'Hybride verbinding overdracht van gegevens (GB) per eenheid'? Is dit per minuut/uur/dag/week/maand? Wat gebeurt er als de limiet is bereikt?

De verbinding van de hybride kosten per eenheid, is afhankelijk van de BizTalk Services edition. Kortom, kosten zijn afhankelijk van hoeveel gegevens u overbrengen. Bijvoorbeeld kosten kleiner dan 100 GB dagelijks overbrengen dagelijks overbrengen van 10 GB-gegevens. De [Prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/?scenario=full) voor BizTalk Services gebruiken om te bepalen specifieke kosten. De limieten zijn meestal dagelijks afgedwongen. Als u de limiet overschrijdt, een overdosering in rekening wordt gebracht met de snelheid van $1 per GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Wanneer ik een overeenkomst in BizTalk Services maakt, waarom het aantal bruggen omhoog gaan door twee in plaats van slechts één?

Elke overeenkomst omvat van twee verschillende bruggen: een brug verzenden aan de communicatie en een ontvangen kant communicatie-brug.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Wat gebeurt er wanneer ik de quotumlimiet van het aantal bruggen of overeenkomsten raken?

U bent niet kan worden eventuele nieuwe bruggen implementeren of maak een nieuwe overeenkomsten. Als u wilt meer implementeren, moet u voor meer eenheden van de service BizTalk vergroten, of een upgrade naar een hogere editie.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Hoe Migreer ik uit één laag van BizTalk-Services naar een andere?

De gratis versie kan niet worden gemigreerd of 'aangepast' aan een andere laag, niet back-up gemaakt en op een andere laag hersteld. Als u een andere laag nodig hebt, maakt u een nieuwe BizTalk Service met de nieuwe laag. Alle onderdelen die zijn gemaakt met behulp van de gratis versie, inclusief hybride verbindingen, moeten opnieuw worden ingesteld in de nieuwe BizTalk-Service. 

Voor de resterende edities, gebruikt u de back-up en herstellen voor het migreren van uw onderdelen vanaf één laag naar de andere. Bijvoorbeeld back-up maken van uw onderdelen in de standaard laag en zet ze naar de Premium-laag. [BizTalk Services: back-up maken en terugzetten](biztalk-backup-restore.md) beschrijving van de ondersteunde migratiepaden en lijsten welke onderdelen zijn back-up gemaakt. Houd er rekening mee dat hybride verbindingen worden niet back-up gemaakt. Nadat u een back-up en herstellen naar een nieuwe laag, hebt opnieuw u de hybride verbindingen.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Is de BizTalk Adapter Service opgenomen in de service? Hoe krijg ik de software?

Ja, de Service BizTalk Adapter met de BizTalk Adapter Pack zijn opgenomen in de SDK van Azure BizTalk Services [downloaden](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Volgende stappen

Als u wilt maken Azure BizTalk Services in de portal van Azure, gaat u naar [BizTalk Services: inrichten met behulp van de Azure portal](biztalk-provision-services.md). Als u wilt beginnen met het maken van toepassingen, gaat u naar [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Aanvullende informatie
- [BizTalk Services: Inrichten met behulp van de Azure portal](biztalk-provision-services.md)<br/>
- [BizTalk Services: Inrichting Status grafiek](biztalk-service-state-chart.md)<br/>
- [BizTalk Services: De tabbladen Dashboard, Monitor en schaal](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk Services: Back-up maken en deze herstellen](biztalk-backup-restore.md)<br/>
- [BizTalk-Services: beperken](biztalk-throttling-thresholds.md)<br/>
- [BizTalk Services: De naam van de uitgever en de uitgever sleutel](biztalk-issuer-name-issuer-key.md)<br/>
- [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
