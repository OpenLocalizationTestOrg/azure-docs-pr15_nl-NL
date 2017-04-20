<properties
    pageTitle="Verkeer Manager - verkeer routeren methoden | Microsoft Azure"
    description="In dit artikel kunt u meer informatie over de verschillende verkeer routeren methoden die worden gebruikt door verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Verkeer Manager verkeer-routing methoden

Azure verkeer Manager ondersteunt drie verkeer-routing methoden om te bepalen hoe u het netwerkverkeer doorsturen naar de verschillende service-eindpunten. De methode verkeer-routing geldt verkeer Manager voor elke DNS-query die deze ontvangt. De methode verkeer-routing bepaalt welke eindpunt geretourneerd door de DNS-antwoord.

De resourcemanager Azure-ondersteuning voor verkeer Manager andere terminologie dan het implementatiemodel klassieke gebruikt. De volgende tabel ziet u de verschillen tussen de resourcemanager en klassieke voorwaarden:

| Resourcemanager term | Klassieke term |
|-----------------------|--------------|
| Omleiding van verkeer methode | Methode van taakverdeling |
| Prioriteit methode | Failover-methode |
| Gewogen methode | Round robin methode |
| Prestaties-methode | Prestaties-methode |

Op basis van feedback van klanten, we de terminologie om te verbeteren, helderheid en algemene vaak iets verkeerd te verkleinen gewijzigd. Er is geen verschil in functionaliteit.

Er zijn drie methoden voor het doorsturen van verkeer beschikbaar is in verkeer Manager:

- **Prioriteit:** Selecteer 'Prioriteit' als u wilt een primaire service-eindpunt gebruiken voor alle verkeer en back-ups bieden voor het geval de primaire of de back-eindpunten niet beschikbaar zijn.
- **Gewogen:** Selecteer 'gewogen' wanneer u het verkeer verdelen over een reeks eindpunten wilt wijzigen, hetzij gelijkmatig of op basis van de gewichten en welke u definiëren.
- **Prestaties:** Selecteer 'Prestaties' wanneer u de eindpunten in verschillende geografische locaties hebt en u wilt dat eindgebruikers gebruik van het eindpunt "eerstvolgende" wat betreft de laagste netwerklatentie.

Alle verkeer Manager profielen opnemen van eindpunt gezondheid en automatische eindpunt overname bij storing cmdlets voor controle. Zie voor meer informatie [Verkeer Manager eindpunt controle](traffic-manager-monitoring.md). Één verkeer Manager profiel kunt slechts één verkeer routeren methode gebruiken. U kunt een ander verkeer routeren methode selecteren voor uw profiel op elk gewenst moment. Wijzigingen worden toegepast binnen één minuut en geen downtime wordt weergegeven. Omleiding van verkeer methoden kunnen worden gecombineerd met behulp van geneste verkeer Manager profielen. Geneste kunt geavanceerde en flexibele verkeer-routing configuraties die voldoen aan de behoeften van grotere, complexe toepassingen. Zie [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md)voor meer informatie.

## <a name="priority-traffic-routing-method"></a>Prioriteit verkeer-routing methode

Vaak wil een organisatie betrouwbaarheid voor de services door een of meer van de back-services implementeert geval hun primaire service uitvalt. De methode 'Prioriteit' verkeer-routing kan Azure klanten dit patroon failover op eenvoudige wijze.

![Azure verkeer Manager 'Prioriteit' verkeer-routing methode][1]

Het verkeer Manager-profiel bevat een gesorteerde lijst met service-eindpunten. Standaard stuurt verkeer Manager al het verkeer naar het primaire (hoogste prioriteit)-eindpunt. Als het primaire eindpunt niet beschikbaar is, stuurt verkeer Manager het verkeer naar het tweede eindpunt. Als de primaire en secundaire eindpunten niet beschikbaar zijn, wordt het verkeer gaat naar de derde, enzovoort. Beschikbaarheid van het eindpunt is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en de lopende eindpunt bewaken.

### <a name="configuring-endpoints"></a>Eindpunten configureren

Met Azure resourcemanager u configureren de eindpunt prioriteit die expliciet met behulp van de prioriteitseigenschap '' voor elk eindpunt. Deze eigenschap is een waarde tussen 1 en 1000. Lagere waarden vertegenwoordigen een hogere prioriteit. Eindpunten delen niet prioriteitswaarden. De eigenschap is optioneel. Wanneer wordt weggelaten, wordt een standaardprioriteit op basis van de volgorde eindpunt gebruikt.

Met de klassieke interface, is de prioriteit eindpunt impliciet geconfigureerd. De prioriteit is gebaseerd op de volgorde waarin de eindpunten worden weergegeven in de definitie profiel.

## <a name="weighted-traffic-routing-method"></a>Gewogen verkeer-routing methode

De 'Gewogen' methode voor het verkeer routering kunt u verkeer gelijkmatig verdelen of een vooraf gedefinieerde weging gebruiken.

![Azure verkeer Manager op 'Gewogen' methode verkeer-routing][2]

In de gewogen verkeer-routing methode, kunt u een gewicht toewijzen aan elk eindpunt in de profielconfiguratie van de verkeer beheer. Het gewicht is een geheel getal tussen 1 en 1000. Deze parameter is optioneel. Indien weggelaten, wordt verkeer Managers gebruikt een standaarddikte van de '1'.

Voor elke DNS-query hebt ontvangen, kiest verkeer Manager willekeurig een beschikbaar eindpunt. De kans van het kiezen van een eindpunt is gebaseerd op de gewichten die zijn toegewezen aan alle beschikbare eindpunten. Het gebruik van hetzelfde gewicht op alle eindpunten resultaten in een even verkeer-verdeling. Gebruik van hogere of lagere gewichten op specifieke eindpunten zorgt ervoor dat deze eindpunten naar meer of minder vaak in de DNS-antwoorden worden geretourneerd.

De gewogen methode kunt enkele nuttige scenario's:

- Geleidelijk upgrade: een percentage van verkeer om te leiden naar een nieuw eindpunt toewijzen en geleidelijk verhogen het verkeer na verloop van tijd 100%.
- Toepassingsmigratie naar Azure: een profiel maken met externe zowel Azure eindpunten. De dikte van de eindpunten naar het nieuwe eindpunten liever aanpassen.
- Cloud uiteenspatten voor extra capaciteit: snel een on-premises implementatie in de cloud uitvouwen door de gegevens achter een profiel verkeer Manager. Wanneer u extra capaciteit in de cloud moet, kunt u toevoegen of meer eindpunten inschakelen en geven welk deel van verkeer naar elke eindpunt thuishoren.

De nieuwe portal van Azure ondersteunt de configuratie van gewogen verkeer-mailroutering. Lijndikten kunnen niet worden geconfigureerd in de klassieke-portal. U kunt ook met de resourcemanager en klassieke versies van Azure PowerShell, CLI en de REST API's lijndikten configureren.

Het is belangrijk om te begrijpen dat DNS-antwoorden in cache opslaan door clients en de recursieve DNS-servers die de clients gebruiken voor het oplossen van DNS-namen. Deze caching kunt meteen invloed hebben op gewogen verkeer onderzoeken. Wanneer het aantal clients en recursieve DNS-servers groot is, wordt verkeer verdeling werkt zoals verwacht. Echter wanneer het aantal clients of recursieve DNS-servers kleine is, kunt caching aanzienlijk laten de verkeer-verdeling.

Algemene gebruik zaken opnemen:

- Ontwikkeling en testen omgevingen
- Application-to-application communicatie
- Toepassingen die zijn gericht op een smalle gebruiker-basis die delen een gemeenschappelijke recursieve DNS-infrastructuur (bijvoorbeeld werknemers van bedrijf verbinding via een proxy)

Deze DNS-cache effecten gelden voor alle DNS-verkeer routeren systemen, niet alleen de Manager van Azure-verkeer is toegestaan. In sommige gevallen desgewenst expliciet de DNS-cache wissen een tijdelijke oplossing. In andere gevallen misschien een alternatief verkeer-routing meer geschikt.

## <a name="performance-traffic-routing-method"></a>Prestaties verkeer-routing methode

Eindpunten in twee of meer locaties implementeert overal ter wereld kan de reactiesnelheid van de vele toepassingen door te routeren van verkeer naar de locatie die zich het dichtst voor u. De methode 'Prestaties' verkeer-routing biedt deze functionaliteit.

![Azure verkeer Manager 'Prestaties' verkeer-routing methode][3]

Het 'eerstvolgende' eindpunt is niet per se eerstvolgende gemeten door geografische afstand. De methode voor het verkeer routering van 'Prestaties' bepaalt in plaats daarvan het dichtstbijzijnde eindpunt door te meten netwerklatentie. Verkeer Manager onderhoudt een Internet latentie tabel als u wilt bijhouden heen hoeveel tijd tussen IP-adresbereiken en elke Azure datacenter.

Verkeer Manager gezocht naar het bron-IP-adres van de binnenkomende DNS-aanvraag in de tabel met Internet latentie. Een eindpunt beschikbaar kiest verkeer Manager in het Azure datacenter dat de laagste latentie voor die IP-adresbereiken heeft en geeft vervolgens dat eindpunt in het DNS-antwoord.

Zoals wordt uitgelegd in [Hoe verkeer Manager werkt](traffic-manager-how-traffic-manager-works.md), ontvangt wordt verkeer Manager niet DNS-query's rechtstreeks vanuit clients. In plaats daarvan DNS-query's afkomstig zijn uit de recursieve DNS-service dat de clients zijn geconfigureerd voor gebruik. Daarom het IP-adres wordt gebruikt bij het bepalen van de 'meest' eindpunt is niet van de client IP-adres, maar het is het IP-adres van de recursieve DNS-service. In Word Web App is dit IP-adres een goede proxy voor de client.

Verkeer Manager wordt regelmatig bijgewerkt in de tabel Internet latentie vanwege wijzigingen in de globale Internet- en nieuwe Azure regio's. Prestaties van toepassingen varieert echter op basis van realtime met variaties in laden op Internet. Prestaties verkeer-mailroutering wordt niet gecontroleerd voor belasting op een bepaald service-eindpunt. Echter als een eindpunt niet beschikbaar is, moet wordt verkeer Manager niet in DNS-query antwoorden.

Houd er rekening mee wordt verwezen:

- Als uw profiel meerdere eindpunten in dezelfde Azure regio bevat, klikt u vervolgens verkeer Manager Hiermee stelt verkeer gelijkmatig over de beschikbare eindpunten in die regio. Als u liever een ander verkeer verdeling binnen een regio, kunt u [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md).

- Als alle ingeschakeld eindpunten in een bepaald gebied van de Azure zijn niet beschikbaar is weergegeven, wordt verkeer Manager verkeer verdeeld over alle andere beschikbare eindpunten in plaats van de volgende eerstvolgende eindpunt. Deze logica voorkomt u dat een trapsgewijze fout plaatsvindt door overbelasting niet het volgende eerstvolgende eindpunt. Als u een reeks voorkeur failover definiëren wilt, gebruikt u [geneste verkeer Manager profielen](traffic-manager-nested-profiles.md).

- Wanneer u de prestaties verkeer routeren methode met externe eindpunten of geneste eindpunten, moet u de locatie van de eindpunten opgeven. Kies het dichtst bevindt bij de implementatie Azure regio. Deze locaties zijn de waarden die worden ondersteund door de Internet latentie tabel.

- Het algoritme die door het eindpunt gekozen is deterministic. Herhaalde DNS-query's vanuit dezelfde client, worden doorgestuurd naar hetzelfde eindpunt. Meestal clients verschillende recursieve DNS-servers gebruiken als u onderweg bent. De client kan worden gerouteerd naar een ander eindpunt. Routering kan ook worden beïnvloed door updates naar de tabel met Internet latentie. De prestaties verkeer-routing methode kan daarom niet garanderen dat een client altijd doorgestuurd naar hetzelfde eindpunt.

- Als de tabel met Internet latentie verandert, kunt u mogelijk dat bij sommige clients worden doorgestuurd naar een ander eindpunt. Deze routeren wijziging is nauwkeuriger op basis van huidige latentie gegevens. Deze updates zijn essentieel voor het behoud van de nauwkeurigheid van prestaties verkeer-routing zoals Internet continu ontwikkeld.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het hoge beschikbaarheid ontwikkelen met behulp van [verkeer Manager eindpunt bewaken](traffic-manager-monitoring.md)

Informatie over het [maken van een profiel verkeer Manager](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
