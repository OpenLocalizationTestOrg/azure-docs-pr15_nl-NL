<properties
    pageTitle="Aanbevolen procedures voor het beeldscherm Azure autoscaling. | Microsoft Azure"
    description="Informatie over beginselen effectief autoscaling in Azure beeldscherm gebruiken."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Aanbevolen procedures voor het beeldscherm Azure autoscaling

De volgende secties in dit document kunnen u informatie over de aanbevolen procedures voor automatisch schalen in Azure. Zodra u deze informatie, hebt zult u beter kunnen automatisch schalen effectief gebruiken in de infrastructuur van uw Azure.

## <a name="autoscale-concepts"></a>Automatisch schalen concepten

- Een resource kan slechts *één* automatisch schalen instelling hebben
- De instelling voor een automatisch schalen kan een of meer profielen en elk profiel kunnen een of meer automatisch schalen regels hebben.
- De instelling voor een automatisch schalen aangepast exemplaren horizontaal, namelijk *af* door te verhogen van de exemplaren en *in* met het aantal exemplaren verlagen.
 De instelling voor een automatisch schalen heeft een maximum, minimum en standaardwaarde van exemplaren.
- Een taak automatisch schalen leest altijd de bijbehorende meetwaarde aan de nieuwe schaal door, als dit is de geconfigureerde drempel voor schalen of schaal in Gekruiste controleren. U kunt een lijst weergeven van de doelstellingen die automatisch schalen met bij [Azure Monitor autoscaling algemene doelstellingen](insights-autoscale-common-metrics.md)kunt verkleinen.
- Alle drempelwaarden worden berekend op het niveau van een exemplaar. Bijvoorbeeld "schaal door 1 exemplaar CPU > 80% wanneer gemiddelde als exemplaar tellen 2 ', betekent dat er schalen wanneer de gemiddelde CPU in alle exemplaren van meer dan 80% is.
- Altijd ontvangt u meldingen over fouten bij via e-mail. De eigenaar, Inzender en lezers van de resource doel specifiek, ontvangen e-mail. U ontvangt een e-mail *herstel* ook altijd wanneer automatisch schalen uit een fout is hersteld en weer verdergaat normaal werkt.
- U kunt aanmelden als u wilt ontvangen een melding van de actie succesvolle schaal via e-mail en webhooks.

## <a name="autoscale-best-practices"></a>Automatisch schalen aanbevolen procedures

Gebruik de volgende aanbevolen procedures terwijl u automatisch schalen gebruikt.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Zorgen de maximum- en minimumbereik waarden zijn verschillende en hebben een voldoende marge ertussen
Als er een instelling met minimum = 2, maximum = 2 en de huidige exemplaar aantal 2 is geen actie schaal kan zich voordoen. Houd een voldoende marge tussen de maximum- en minimumbereik exemplaar tellingen komen, welke inclusief. Deze methode altijd automatisch schalen past tussen deze limieten.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Handmatig schalen opnieuw wordt ingesteld door automatisch schalen min en max
Als u handmatig de exemplaar telling naar een waarde boven of onder het maximale aantal bijwerkt, wordt de engine automatisch schalen automatisch aangepast terug naar het minimum (if hieronder) of het maximale aantal (als hierboven). U zo instellen dat het bereik tussen 3 en 6. Als u één exemplaar van de actieve hebt, wordt de engine automatisch schalen aangepast aan 3 exemplaren op de volgende keer wordt uitgevoerd. Ook deze zou schaal in 8 exemplaren weer tot en met 6 op de volgende keer wordt uitgevoerd.  Handmatig schalen is zeer tijdelijk, tenzij u ook de regels voor automatisch schalen opnieuw instellen.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Gebruik altijd een combinatie van schalen en schaal in regel waarmee een vergroten en verkleinen
Als u alleen een deel van de combinatie gebruikt, is automatisch schalen schaal in die af of in, één tot het maximum of minimum, bereikt.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Niet overschakelen tussen de Azure-portal en de Azure klassieke portal bij het beheren van automatisch schalen
Gebruik de Azure-portal (portal.azure.com) maken en beheren van instellingen voor automatisch schalen voor Cloudservices en App-Services (Web-Apps). Gebruik PoSH, CLI of REST API maken en beheren van de instelling automatisch schalen voor VM schaal Sets. Niet schakelen tussen de Azure klassieke-portal (manage.windowsazure.com) en de Azure-portal (portal.azure.com) bij het beheren van automatisch schalen configuraties. De Azure klassieke portal en de onderliggende backend heeft beperkingen. Verplaatsen naar de Azure-portal voor het beheren van automatisch schalen met een grafische gebruikersinterface. Er zijn de opties voor het gebruik van de automatisch schalen PowerShell, CLI of REST API (via Azure Resource Explorer).

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Kies de juiste statistische voor uw metrisch diagnostische gegevens
Diagnostische gegevens aan de doelstellingen, kunt u kiezen *Average*, *Minimum*, *Maximum* - en *totale* als een meting met wilt verkleinen. De meest voorkomende statistische is *gemiddelde*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Kies de drempels zorgvuldig door voor alle metrische typen
We raden zorgvuldig door verschillende drempelwaarden voor schalen en schaal in op basis van praktische situaties.

We *raden u niet* automatisch schalen instellingen tevreden bent met het dezelfde of een vergelijkbaar drempelwaarden voor uit- en voorwaarden in de voorbeelden hieronder:

- Exemplaren verhogen met 1 tellen wanneer aantal threads < = 600
- Exemplaren verlagen met 1 tellen wanneer aantal threads > = 600

Bekijk een voorbeeld van wat kan leiden tot een gedrag dat duidelijker is lijkt. De volgende volgorde cosider.

1. Wordt ervan uitgegaan dat er 2 exemplaren beginnen en klikt u vervolgens het gemiddelde aantal threads per exemplaar groeit tot 625.
2. Deze methode automatisch schalen past een 3e exemplaar toe.
3. Vervolgens wordt ervan uitgegaan dat het gemiddelde aantal threads over exemplaar naar 575 valt.
4. Voordat u het verkleinen, worden automatisch schalen pogingen doen om een schatting van de uiteindelijke status te als deze schaal. Voorbeeld: 575 x 3 (huidige exemplaar aantal) = 1,725 / 2 (definitieve aantal exemplaren wanneer verkleind) = 862.5 threads. Dit betekent dat automatisch schalen zou moeten direct schalen opnieuw zelfs nadat u deze in, aangepast als het gemiddelde aantal threads dezelfde blijft of zelfs slechts weinig valt. Echter als deze opnieuw, het hele proces hebt aangepast wilt herhalen, waardoor een lus.
5. Als u wilt voorkomen dat deze situatie ('flapping' genoemd), wordt automatisch schalen niet schaal omlaag helemaal. In plaats daarvan wordt overgeslagen en de voorwaarde reevaluates opnieuw de volgende keer dat de taak van de service wordt uitgevoerd. Dit kan veel mensen verwarrend omdat automatisch schalen wouldn't worden weergegeven om te werken bij het gemiddelde aantal threads 575 is.

Schatting tijdens een schaal-invoegtoepassing is bedoeld om "flappy" situaties te vermijden. Houd rekening met dit probleem in gedachten wanneer u dezelfde drempels voor schalen kiest en in.

We raden aan om een voldoende marge tussen de schalen en in drempelwaarden. Een voorbeeld, kunt u de volgende betere regel combinatie.

- Exemplaren verhogen met 1 tellen wanneer CPU % > = 80
- Exemplaren verlagen met 1 tellen wanneer Processorsnelheid % < = 60

In dit geval  

1. Wordt ervan uitgegaan dat er 2 exemplaren beginnen.
2. Als de gemiddelde CPU % alle werkstroomexemplaren Hiermee gaat u naar 80, verkleind, automatisch schalen uit het toevoegen van een derde exemplaar.
3. Nu is ervan uitgegaan dat na verloop van tijd de CPU-percentage 60 valt.
4. Automatisch schalen van schaal in regel maakt een schatting van de laatste staat te schaal in zijn. Voorbeeld: 60 x 3 (huidige exemplaar telling) = 180 / 2 (definitieve aantal exemplaren wanneer verkleind) = 90. Zodat automatisch schalen wordt niet schaal in omdat deze schalen direct opnieuw zou moeten. In plaats daarvan overgeslagen schaalbaarheid omlaag.
5. De volgende keer automatisch schalen Hiermee wordt gecontroleerd, blijft de CPU tot 50 vallen. Deze maakt een schatting van opnieuw - exemplaar van 50 x 3 = 150 / 2 exemplaren = 75, dat zich onder de drempelwaarde voor schalen 80, zodat het schalen succes in 2-exemplaren.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Overwegingen voor schaalbaarheid drempelwaarden voor speciale aan de doelstellingen
 Speciale aan de doelstellingen zoals opslag of Service Bus wachtrij lengte metrisch is de drempelwaarde voor het gemiddelde aantal berichten per huidige aantal exemplaren beschikbaar. Kies de kiezen zorgvuldig de drempelwaarde voor deze metrisch.

Laten we illustreren dit met een voorbeeld om te zorgen dat u het gedrag van betere kennen.

- Exemplaren verhogen met 1 tellen als opslag wachtrij mailbericht tellen > = 50
- Exemplaren verlagen met 1 tellen als opslag wachtrij mailbericht tellen < = 10

Houd rekening met de volgende volgorde:

1. Er zijn 2 opslag wachtrij exemplaren.
2. Berichten komen steeds en wanneer u de wachtrij opslag, het totale aantal 50 leest. U mogelijk wordt ervan uitgegaan dat deze automatisch schalen een actie schalen moet beginnen. Bedenk wel dat deze nog steeds 50/2 = 25 berichten per exemplaar. Ja, schalen treedt niet. Voor de eerste schalen veroorzaken, moet het totale aantal berichten in de wachtrij opslag 100.
3. Vervolgens wordt ervan uitgegaan dat het totale aantal berichten 100 bereikt.
4. Een 3e opslag wachtrij exemplaar wordt vanwege een actie schalen toegevoegd.  De volgende schalen actie gebeurt niet totdat het totale aantal berichten in de wachtrij 150 bereikt omdat 150/3 = 50.
5. Nu wordt het aantal berichten in de wachtrij verkleind. Met 3 exemplaren, de eerste actie schaal in gebeurt er wanneer het totale aantal berichten in alle wachtrijen maximaal 30 toevoegen omdat 30/3 = 10 berichten per exemplaar, dat wil zeggen de drempelwaarde voor schaal in.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Overwegingen voor schaalbaarheid wanneer meerdere profielen zijn geconfigureerd in een instelling automatisch schalen

In een instelling automatisch schalen, kunt u een standaardprofiel, die altijd zonder alle afhankelijkheden van planning of tijd wordt toegepast, of u een terugkerende profiel of een profiel kunt kiezen voor een bepaalde termijn met een datum- en tijdbereik.

Wanneer automatisch schalen service ze verwerkt, er altijd op wordt gecontroleerd in de volgende volgorde:

1. Vaste datum profiel
2. Terugkerende profiel
3. Standaard ("altijd")-profiel

Als een profiel-voorwaarde is voldaan, is automatisch schalen controleert niet de volgende voorwaarde profiel eronder. Automatisch schalen verwerkt slechts één profiel tegelijk. Dit betekent dat als u opnemen ook een verwerkingsvoorwaarde van een profiel lagere niveaus wilt, moet u opnemen deze regels ook in het huidige profiel.

We gaan volgt een voorbeeld gebruiken:

De onderstaande afbeelding ziet u de instelling voor een automatisch schalen met een standaardprofiel van de minimale exemplaren = 2- en maximumwaarden exemplaren = 10. In dit voorbeeld regels schalen wanneer het aantal berichten in de wachtrij groter dan 10 is zijn geconfigureerd en schaal in wanneer het aantal berichten in de wachtrij kleiner dan 3 is. De resource kan nu schalen tussen 2 en 10 exemplaren.

Daarnaast is er een terugkerende profiel instellen voor maandag. Dit is ingesteld voor minimale exemplaren = 2- en maximumwaarden exemplaren = 12. Dit betekent op maandag, de eerste keer automatisch schalen Hiermee wordt gecontroleerd op deze voorwaarde, als het aantal exemplaar 2 is, deze wordt aangepast aan de nieuwe minimum van 3. Zo lang maken als automatisch schalen blijft zoeken met deze voorwaarde profiel overeenkomend (maandag), alleen worden verwerkt de CPU gebaseerde schaal-out/in regels geconfigureerd voor dit profiel. Op dit moment, worden deze niet gecontroleerd voor de lengte van de wachtrij. Echter desgewenst kunt u ook de wachtrij lengte voorwaarde moet worden gecontroleerd, moet u opnemen die regels uit het standaardprofiel ook in uw profiel maandag.

Op dezelfde manier wanneer automatisch schalen schakelt u terug naar het standaardprofiel, deze eerst gecontroleerd of als de minimum- en maximumwaarden voorwaarden is voldaan. Als het aantal exemplaren op het moment 12 is, wordt het naar 10, het toegestane maximum van het standaardprofiel schalen.

![instellingen voor automatisch schalen](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Overwegingen voor schaalbaarheid wanneer meerdere regels zijn geconfigureerd in een profiel
Er zijn situaties waarin u hebt mogelijk voor het instellen van meerdere regels in een profiel. De volgende set automatisch schalen regels worden gebruikt door services gebruiken wanneer u meerdere regels worden ingesteld.

*Schaal af*, automatisch schalen wordt uitgevoerd als elke regel is voldaan.
Automatisch schalen vereisen op *schaal in*alle regels moet worden voldaan.

Om te illustreren, wordt ervan uitgegaan dat u hebt de volgende 4 automatisch schalen regels:

- Als CPU < 30%, schaal in met een 1
- Als het geheugen < 50%, schaal in met een 1
- Als CPU > 75%, schalen met een 1
- Als het geheugen > 75%, schalen met een 1

Gebeurt het volgende:

- Als CPU 76% en geheugen 50%, we schalen.
- Als CPU 50% en geheugen 76% we schalen.

Aan de andere kant als CPU 25% en geheugen 51% automatisch schalen wordt **niet** schaal in. Om schaal hebt aangemeld CPU moet 29% en geheugen 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Altijd een aantal van de exemplaar veilige standaard selecteren
De standaard exemplaar telling is belangrijk automatisch schalen uw service aan die het aantal wordt aangepast wanneer aan de doelstellingen niet beschikbaar zijn. Daarom selecteren een standaard-exemplaar telling die voor uw werkbelasting veilig is.

### <a name="configure-autoscale-notifications"></a>Automatisch schalen meldingen configureren
Automatisch schalen krijgt de beheerders en medewerkers van de resource per e-mail als een van de volgende voorwaarden optreedt:

- automatisch schalen service mislukt een actie te ondernemen.
- Aan de doelstellingen zijn niet beschikbaar voor automatisch schalen service moet schaal zelf.
- Aan de doelstellingen zijn beschikbaar (herstellen) opnieuw moet schaal zelf.
Naast de bovenstaande voorwaarden, kunt u e-mailbericht of webhook meldingen aan een melding ontvangen voor een succesvolle schaal acties configureren.
