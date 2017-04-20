<properties   
    pageTitle="BizTalk Server bewerken oplossingen migreren naar BizTalk Services technische handleiding | Microsoft Azure"
    description="Bewerken migreren naar MAB; Microsoft Azure BizTalk-Services"
    services="biztalk-services"
    documentationCenter="na"
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


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>BizTalk Server bewerken oplossingen migreren naar BizTalk Services: technische handleiding

Auteur: Tim Wieman en Mirchandani Mehrotra

Revisoren: Karthik Bharthy

Geschreven met: Microsoft Azure BizTalk Services – februari 2014 loslaat.

## <a name="introduction"></a>Inleiding

Elektronische Data Interchange (bewerken) is een van de meest gangbare gemiddelden door welke bedrijven exchange-gegevens elektronisch, ook als Business-to-Business of B2B transacties wordt genoemd. BizTalk Server heeft bewerken ondersteuning voor via een tien jaar, sinds de eerste keer BizTalk-Server. Met BizTalk-Services blijft Microsoft de ondersteuning voor bewerken oplossingen op het Microsoft Azure-platform. B2B transacties zijn voornamelijk buiten een organisatie en dus is het gemakkelijker te implementeren als deze wordt geïmplementeerd op een cloud-platform. Microsoft Azure biedt deze functionaliteit via BizTalk-Services.

Sommige klanten kijken naar BizTalk-Services als een 'greenfield'-platform voor nieuwe bewerken oplossingen, hebben veel klanten huidige BizTalk Server bewerken-oplossingen die ze mogelijk wilt migreren naar Azure. Omdat BizTalk Services bewerken is ontworpen is op basis van dezelfde belangrijke diensten als BizTalk Server bewerken architectuur (handelsdag partners, entiteiten, overeenkomsten), het mogelijk om te migreren BizTalk Server bewerken-onderdelen naar BizTalk-Services.

In dit document worden enkele van de verschillen die nodig zijn voor het migreren BizTalk Server bewerken-onderdelen naar BizTalk Services beschreven. In dit document wordt ervan uitgegaan gewerkt met BizTalk Server bewerken verwerking en Partner overeenkomsten handelsdag. Zie voor meer informatie over BizTalk Server bewerken, [Partner Management gebruiken BizTalk-Server handelsdag](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welke versie van BizTalk Server bewerken onderdelen kan worden gemigreerd naar BizTalk-Services?

De module BizTalk Server bewerken is aanzienlijk enhanced voor BizTalk Server 2010, wanneer deze is opnieuw gemodelleerd om op te nemen partners, profielen en overeenkomsten. Hetzelfde model BizTalk-Services gebruikt om de werkbladen partners en de afdelingen bedrijven binnen de werkbladen partners te organiseren. Hierdoor is migreren bewerken onderdelen van BizTalk Server 2010 en hoger naar BizTalk-Services, een veel meer rechtstreeks doorsturen proces. Als u wilt migreren bewerken onderdelen die is gekoppeld aan versies vóór BizTalk Server 2010, moet u eerst een upgrade naar BizTalk Server 2010 en vervolgens uw onderdelen bewerken te migreren naar BizTalk-Services.

## <a name="scenariosmessage-flow"></a>Scenario's / bericht stroom

Als met BizTalk Server, is bewerken verwerken in BizTalk Services gebaseerd op een oplossing handelsdag Partner (Trusted). De oplossing TPM heeft de volgende belangrijke onderdelen:

- Handelsdag partners, waarin de organisatie in een transactie B2B vertegenwoordigen.
- Profielen die afdelingen binnen een handelen partner vertegenwoordigen.
- Handelsdag partner overeenkomsten (of overeenkomsten), waarin de business-overeenkomst tussen twee partners/profielen vertegenwoordigen.

De volgende afbeelding ziet u de overeenkomsten, evenals de verschillen tussen een oplossing BizTalk Server bewerken en een oplossing BizTalk Services bewerken:

![][EDImessageflow]

De belangrijke verschillen en overeenkomsten tussen een stroom van de oplossing bewerken in BizTalk Server en BizTalk Services zijn:

- Net als een verkooppijplijn EDIReceive BizTalk Server gebruikt voor het ontvangen van een bericht bewerken en een verkooppijplijn EDISend als een bericht bewerken wilt verzenden, geeft BizTalk Services maakt gebruik van een bewerken ontvangen brug voor het ontvangen van en brug bewerken verzenden bewerken berichten verzenden. De pijpleidingen zijn gekoppeld aan een overeenkomst met behulp van verzenden of ontvangen van poorten in BizTalk-Server. In BizTalk-Services, de overeenkomst zelf geeft het verzenden of ontvangen van brug.
- In BizTalk Server, nadat de pijplijn EDIReceive het bericht bewerken verwerkt, het bericht in een teksteditor weergeven met een SQL Server-database. De pijplijn EdiSend vervolgens het bericht van de SQL Server-database wordt opgehaald, verwerkt en verstuurt het vervolgens naar de werkbladen partner.

    Klik in BizTalk-Services, nadat het bewerken hebt ontvangen brug verwerkt het bericht bewerken, deze het bericht stuurt naar een extern proces. De externe-proces kan worden uitgevoerd op Microsoft Azure of on-premises implementatie. Het externe proces moet het bericht doorsturen naar de bewerken verzenden brug; het bericht wordt niet per definitie halen door de brug verzenden. Nadat het bericht is verwerkt, stuurt de bewerken verzenden brug het bericht naar de werkbladen partner.

BizTalk Services biedt een eenvoudig-en-klare configuratie-ervaring om snel te maken en implementeren van een B2B-overeenkomst tussen de partners handelsdag zonder configureren van een Microsoft Azure berekenen exemplaren (Web- of werknemer rollen), een Microsoft Azure SQL-Databases of een Microsoft Azure opslag-accounts. Meer complexe scenario's moeten worden zo koppelen in werkstromen of andere service verwerking "rond de randen' van een Partner handelsdag-overeenkomst, dat wil zeggen, voor of na verwerking van brug handelsdag Partner overeenkomst bewerken. Uitgebreid beschreven, wordt de volgende reeks gebeurtenissen optreden tijdens een bewerken-berichten in BizTalk-Services.

1. Een bericht bewerken is ontvangen van de partner, Fabrikam handel.  Voor het bewerken berichten ontvangen van werkbladen partners, ondersteunt BizTalk Services transportprotocollen zoals FTP, SFTP AS2 en HTTP/S.

2. De werkbladen partner overeenkomst ontvangen aan de clientzijde verwerking ontleed het bericht bewerken om XML-indeling.  U kunt het bewerken bericht (in XML-indeling) doorsturen naar de eindpunten Service Bus zoals een Service Bus Relay-eindpunt, Service Bus onderwerp, Service Bus wachtrij of een brug BizTalk-Services.

3. XML-berichten kunnen vervolgens worden ontvangen van het eindpunt voor verdere aangepaste verwerking.  Deze eindpunten kunnen worden verwerkt door een on-premises implementatie-onderdeel of een exemplaar van Microsoft Azure berekenen naar verdere proces het bericht in een Windows-werkstroom (WF) of Windows Communication Foundation (WCF)-service, bijvoorbeeld.

4. De "verzenden aan de clientzijde verwerking" van de werkbladen partner overeenkomst vervolgens samengevoegd naast het XML-bericht naar de indeling bewerken en verzonden naar werkbladen partner, Contoso.  Voor het verzenden van berichten bewerken voor werkbladen partners, ondersteunt BizTalk Services dezelfde protocollen als die worden gebruikt voor het ontvangen van berichten bewerken.

Verder in dit document bevat conceptuele richtlijnen enkele van de verschillende onderdelen van de BizTalk Server bewerken op migreren naar BizTalk-Services.

## <a name="sendreceive-ports-to-trading-partners"></a>Verzenden/ontvangen poorten handel Partners

In BizTalk Server stelt u locaties ontvangen en ontvangen poorten om te bewerken/XML-berichten ontvangt van werkbladen partners en u verzenden poorten bewerken/XML-berichten verzenden naar werkbladen partner instellen. U verbinden, klikt u vervolgens omhoog deze poorten naar een handelen partnerovereenkomst met behulp van de beheerconsole van BizTalk-Server. In BizTalk-Services, de locaties waarop u berichten ontvangt van de handel partners en waar u de berichten handel partners zijn geconfigureerd als onderdeel van de werkbladen partnerovereenkomst zelf (als onderdeel van instellingen voor Transport) in de Portal van de Services BizTalk verzenden.  Zodat heb u niet echt het concept van "verzenden poorten" en 'ontvangt locaties', zodanig in BizTalk-Services. Zie [Overeenkomsten maken](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx)voor meer informatie.

## <a name="pipelines-bridges"></a>Pijpleidingen (bruggen)

In BizTalk Server bewerken zijn pijpleidingen bericht verwerking entiteiten die ook aangepaste logica voor specifieke verwerking-functies, zoals vereist door de toepassing kunnen opnemen. Voor BizTalk-Services zou het equivalent een brug bewerken. Echter in BizTalk-Services, voorlopig de bruggen bewerken zijn 'gesloten'.  U kunt uw eigen aangepaste activiteiten dat wil zeggen niet toevoegen aan een brug bewerken. Uw eigen bewerkingen moet worden uitgevoerd buiten de brug bewerken in de toepassing, voor of na het bericht de brug geconfigureerd als onderdeel van de Partner handelsdag-overeenkomst binnenkomt. EAI bruggen hebt de optie aangepaste verwerking. Als u aangepaste verwerking wilt, kunt u EAI bruggen voor of na het bericht is verwerkt door de brug bewerken is ingesteld. Lees [hoe u aangepaste Code opnemen in bruggen](https://msdn.microsoft.com/library/azure/dn232389.aspx)voor meer informatie.

U kunt een stroom publicaties/abonnementen met aangepaste code en/of Service Bus messaging wachtrijen en onderwerpen voordat de werkbladen partnerovereenkomst ontvangt het bericht of na de overeenkomst verwerkt het bericht en stuurt dit naar een Service Bus-eindpunt gebruikt invoegen.

Zie **Scenario's / bericht-mailstroom** in dit onderwerp voor het bericht stroom-patroon.

## <a name="agreements"></a>Overeenkomsten

Als u bekend met het BizTalk-Server 2010 handel Partner overeenkomsten gebruikt voor de verwerking van bewerken bent, klikt u vervolgens vindt BizTalk Services partner overeenkomsten handelsdag u erg bekend voorkomen. De meeste instellingen overeenkomst zijn hetzelfde en de dezelfde terminologie gebruiken. In sommige gevallen, de overeenkomst-instellingen zijn veel eenvoudiger vergeleken met dezelfde instellingen in BizTalk Server. Microsoft Azure BizTalk Services ondersteunt X12, EDIFACT en AS2 transport.

Microsoft Azure BizTalk Services bevat ook een migratieprogramma van **TPM gegevens** voor het migreren van werkbladen partners en overeenkomsten van BizTalk-Server handelsdag Partner module bij BizTalk Services-Portal. Het migratieprogramma van TPM gegevens is beschikbaar als onderdeel van een pakket's, die kan worden gedownload van de [MAB SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Het pakket bevat ook het Leesmij-bestand met instructies voor het gebruik van het hulpmiddel en basisinformatie over probleemoplossing voor het hulpmiddel.

## <a name="schemas"></a>Schema's maken

BizTalk Services biedt bewerken schema's maken die kunnen worden gebruikt in BizTalk Services-oplossingen.  BizTalk Server bewerken schema's kunnen bovendien ook worden gebruikt met BizTalk Services omdat het knooppunt van de hoofdmap van het schema bewerken dezelfde meerdere BizTalk Server, evenals BizTalk-Services is. U bent dus mogen rechtstreeks uw BizTalk Server bewerken schema's maken en gebruiken in de bewerken-oplossingen die u ontwikkelt met behulp van BizTalk-Services. U kunt ook de schema's downloaden uit de [MAB SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Kaarten (transformaties)

Kaarten in BizTalk Server heten transformaties in BizTalk-Services. Toewijzingen van de BizTalk Server migreren naar BizTalk Services mogelijk een van de complexere taken om te bereiken (afhankelijk van de complexiteit van de map). Het hulpmiddel toewijzing is gebruikt voor BizTalk Services verschilt van de toewijzing BizTalk. Hoewel de toewijzing voornamelijk er hetzelfde uitziet, wordt de opmaak van de onderliggende map verschilt. De functoids (genoemd **Kaart bewerkingen** in BizTalk Services) beschikbaar voor de gebruikers zijn ook verschillende.  U niet een BizTalk-kaart in feite rechtstreeks in BizTalk Services gebruiken. Niet alle functoids die beschikbaar zijn in BizTalk Server zijn ook beschikbaar als de map bewerkingen in BizTalk-Services.

### <a name="new-transform-operations"></a>Nieuwe transformatiebewerkingen

Hoewel de lijst met transformeren kaart bewerkingen beschikbaar heel anders dan de BizTalk Server-toewijzing lijken kan, BizTalk Services transformaties beschikken over de nieuwe manieren dezelfde taken uitvoeren. BizTalk Services transformaties hebben bijvoorbeeld **Lijst bewerkingen** beschikbaar. Dit is niet beschikbaar in de BizTalk-toewijzing.  De **Lijst met bewerkingen** kunt u maken en gebruiken van een 'lijst", waarbij een lijst met een reeks items (ook wel ' rijen") en waarbij elk item kan meerdere leden (ook wel ' kolommen') hebben.  U kunt de lijst sorteren, selecteert u items op basis van een voorwaarde, enzovoort.

Een ander voorbeeld van de nieuwe functies in BizTalk Services transformaties zijn de **Lus bewerkingen**.  Het is moeilijk te maken van geneste lussen in de BizTalk Server-toewijzing.  Zo worden de bewerkingen van de kaart lus toegevoegd voor de BizTalk Services getransformeerd.

Nog is een ander voorbeeld de **If-Then-Else** -expressie kaart bewerking.  Werk om een if-then-else bewerking is mogelijk in de BizTalk-toewijzing, maar deze meerdere functoids voor het uitvoeren van een schijnbaar eenvoudige taak vereist.

### <a name="migrating-biztalk-server-maps"></a>Migreren BizTalk Server kaarten

Microsoft Azure BizTalk Services biedt een venster voor het migreren van BizTalk Server wordt toegewezen aan BizTalk Services transformaties. De **BTMMigrationTool** is beschikbaar als onderdeel van het pakket's **** is voorzien van de [BizTalk Services SDK downloaden](http://go.microsoft.com/fwlink/p/?LinkId=235057). Zie [een kaart BizTalk naar een BizTalk Services transformeren converteren](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx)voor meer informatie over het hulpmiddel.

U kunt ook een steekproef door Sandro Pereira, BizTalk MVP, over het migreren van [BizTalk Server toewijzingen naar BizTalk Services transformaties](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx)bekijken.

## <a name="orchestrations"></a>Integraties

Als u migreren van BizTalk Server-integratie verwerken van Microsoft Azure wilt, moet de integraties worden herschreven omdat Microsoft Azure biedt geen ondersteuning voor actieve BizTalk Server-integraties.  U kunt de functionaliteit voor integratie in de Windows Workflow Foundation 4.0 (WF4)-service kan herschrijven.  Dit is een volledig herschreven omdat er momenteel geen migratie van BizTalk Server integraties naar WF4. Hier volgen enkele bronnen voor Windows-werkstroom:

- [*Het integreren van een werkstroom WCF-Service met Service Bus wachtrijen en onderwerpen*](https://msdn.microsoft.com/library/azure/hh709041.aspx) door Paolo Salvatori. 

- [ *Building-apps gebruiken met Windows Workflow Foundation en Azure* -sessie](http://go.microsoft.com/fwlink/p/?LinkId=237314) vanuit de vergadering opbouwen 2011.

- [*Windows Workflow Foundation Developer Center*](http://go.microsoft.com/fwlink/p/?LinkId=237315) op MSDN.

- [*Windows Workflow Foundation 4 (WF4) documentatie*](https://msdn.microsoft.com/library/dd489441.aspx) op MSDN.

## <a name="other-considerations"></a>Andere relevante informatie

Hier volgen een paar aandachtspunten die u moet volwaardig BizTalk-Services.

### <a name="fallback-agreements"></a>Fallback overeenkomsten

Verwerking van BizTalk Server bewerken heeft het concept van het "Gebruik overeenkomsten".  BizTalk-Services wordt **niet** een concept gebruik overeenkomst tot nu toe hebt.  Zie BizTalk documentatie onderwerpen [De rol van de overeenkomsten in het verwerken van bewerken](http://go.microsoft.com/fwlink/p/?LinkId=237317) en [configureren van globale of gebruik overeenkomst eigenschappen](https://msdn.microsoft.com/library/bb245981.aspx) voor informatie over hoe gebruik overeenkomsten worden gebruikt in BizTalk Server.

### <a name="routing-to-multiple-destinations"></a>Zoekresultaten omleiden naar meerdere bestemmingen

BizTalk Services bruggen, in de huidige status biedt geen ondersteuning voor routeren van berichten naar meerdere bestemmingen publiceren met-model abonneren. U kunt berichten van een brug BizTalk Services naar een onderwerp Service Bus, die vervolgens meerdere abonnementen op het bericht op meer dan één eindpunt kan bevatten in plaats daarvan kan routeren.

## <a name="conclusion"></a>Sluiten

Microsoft Azure BizTalk Services wordt bijgewerkt in de normale mijlpalen meer functies en mogelijkheden toevoegen. Met elke update hopen we naar ondersteunende verbeterde functionaliteit om u te helpen gemakkelijker om end-to-end oplossingen BizTalk-Services en andere Azure technologieën te maken.

## <a name="see-also"></a>Zie ook

[Bedrijfstoepassingen met Azure ontwikkelen](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
