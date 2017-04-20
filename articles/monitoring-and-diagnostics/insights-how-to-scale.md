<properties
    pageTitle="Exemplaar tellen schaal handmatig of automatisch | Microsoft Azure"
    description="Leer hoe u de schaal van uw services Azure aanpassen."
    authors="rboucher"
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Exemplaar tellen schaal handmatig of automatisch

Klik in de [Portal van Azure](https://portal.azure.com/)kunt u het aantal exemplaar van uw service handmatig instellen of, kunt u parameters instellen zodat deze automatisch schaal op basis van de aanvraag. Dit is meestal genoemd of *schaal af* *in*.

Voordat schaalbaarheid op basis van exemplaar tellen, kunt u overwegen dat schaalbaarheid wordt beïnvloed door **prijzen laag** naast het exemplaar tellen. Andere prijzen niveaus kunnen hebben verschillende getallen gietkernen en geheugen en dus, hebben ze betere prestaties voor hetzelfde aantal exemplaren (dit is of *schaal omhoog* *Omlaag*). In dit artikel behandelt specifiek *schaal in* en *uit*.

U kunt de schaal in de portal en u kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) gebruiken om aan te passen schaal handmatig of automatisch.

> [AZURE.NOTE] In dit artikel wordt beschreven hoe de instelling voor een automatisch schalen maken in de portal op [http://portal.azure.com](http://portal.azure.com). Automatisch schalen instellingen hebt gemaakt in deze portal kunnen niet worden bewerkt deze de klassieke portal ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Handmatig schaalbaarheid

1. In de [Portal van Azure](https://portal.azure.com/), klikt u op **Bladeren**en navigeer naar de bron die u verkleinen wilt, zoals een **App-abonnement**.

2. De tegel **schaal** in **bewerkingen** kunt u nagaan de status van schaalbaarheid: **uitschakelen** voor wanneer u handmatig **op** voor wanneer u zijn schaling met een of meer prestatiegegevens zijn schaalbaarheid.
    ![Schaal-tegel](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Klikken op de tegel, gaat u naar het blad **schaal** . Aan de bovenkant van het blad schalen ziet u een geschiedenis van automatisch schalen acties de service.  
    ![Schaal blade](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Alleen de acties die worden uitgevoerd door automatisch schalen wordt in deze grafiek weergegeven. Als u het aantal exemplaar van de werkstroom voor het handmatig aanpassen, wordt de wijziging niet in deze grafiek worden doorgevoerd.

4. U kunt het nummer **vindplaatsen** aflopende handmatig aanpassen.
5. Klik op de opdracht **Opslaan** en u hebt aangepast dat aantal exemplaren bijna onmiddellijk.

## <a name="scaling-based-on-a-pre-set-metric"></a>Schaalbaarheid op basis van een vooraf ingestelde meting

Als u het aantal exemplaren automatisch wordt aangepast op basis van een meting, schakelt u de gewenste meetwaarde in de vervolgkeuzelijst **schaal door** . Bijvoorbeeld voor een **App-abonnement** kunt u schalen met **CPU Percentage**.

1. Wanneer u een meting selecteert krijgt u een schuifregelaar en/of, tekstvakken, het aantal exemplaren dat u schalen wilt tussen invoeren:

    ![Schaal blade met CPU Percentage](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Automatisch schalen kost nooit uw service onder of boven de grenzen dat u hebt ingesteld, ongeacht de laden.

2. Tweede, kunt u het doelbereik voor de meetwaarde kiezen. Bijvoorbeeld als u ervoor **CPU percentage kiest**, kunt u instellen een doel voor de gemiddelde CPU voor alle van de exemplaren in uw service. Een schaal out gebeurt er wanneer de gemiddelde CPU groter is dan het maximale aantal dat u definieert, op dezelfde manier een schaal in gebeurt er wanneer u het gemiddelde CPU lager is dan het minimum.

3. Klik op de opdracht **Opslaan** . Automatisch schalen moeten worden gecontroleerd om de paar minuten om ervoor te zorgen dat u in het bereik van exemplaar en doel voor uw metrisch bent. Wanneer uw service extra verkeer ontvangt, krijgt u meer exemplaren zonder te moeten doen.

## <a name="scale-based-on-other-metrics"></a>Schaal op basis van andere gegevens

U kunt op basis van de doelstellingen dan de vooraf ingestelde die worden weergegeven in de vervolgkeuzelijst **met wilt verkleinen** , en kunnen zelfs een complexe reeks schaal af hebt en de schaal in regels schalen.

### <a name="adding-or-changing-a-rule"></a>Toevoegen of wijzigen van een regel

1. Kies de **planning en prestaties regels** in de vervolgkeuzelijst **schaal door** : ![prestaties regels](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Als u automatisch schalen eerder had, ziet op u een weergave van de exacte regels die u had.

3. Als u wilt schalen op basis van een andere metrische Klik op de rij van de **Regel toevoegen** . U kunt ook klikken op een van de bestaande rijen wijzigen van de meetwaarde die u eerder had naar de meetwaarde die u schalen wilt door.
![Regel toevoegen](./media/insights-how-to-scale/Insights_AddRule.png)

4. Nu u nodig hebt om te selecteren waarvoor het metrische die u wilt schalen door. Bij het kiezen van een meting er een paar zaken waar zijn u rekening moet houden:
    * De *resource* de meetwaarde is afkomstig uit. Meestal is dit hetzelfde als de bron die u hebben. Als u schalen dat door de diepte van een wachtrij opslag wilt, is de resource echter de wachtrij die u schalen wilt door.
    * De *naam van de metrische* zelf.
    * De *Tijdaggregatie* van de meetwaarde. Dit is hoe de gegevens combineren is verstreken de *duur*.

5. Nadat uw metrisch kiezen kiest u de drempelwaarde voor de meetwaarde en de operator. U kunt bijvoorbeeld **groter is dan** **80%**zeggen.

6. Kies de actie die u wilt maken. Er zijn een paar verschillende soorten acties:
    * Verhogen of verlagen met: Hiermee kunt toevoegen of verwijderen van het nummer van de **waarde** van de exemplaren die u definieert
    * Vergroot of verkleint percentages: Hiermee wijzigt u het exemplaar tellen met een percentage. Zoals u 25 in het veld **waarde** veelvoudvisualisaties kunt plaatsen en als u momenteel 8 exemplaren had, 2 worden toegevoegd.
    * Vergroten of verkleinen naar: Hiermee wordt het aantal exemplaar ingesteld op de **waarde** die u definieert.

7. Tot slot kunt u fraaie omlaag - hoe lang deze regel na de vorige actie schaal wachten moet aan de nieuwe schaal opnieuw.

8. Klik op **OK**na het configureren van de regel.

9. Wanneer u alle gewenste regels hebt geconfigureerd, moet u de opdracht **Opslaan** raken.

### <a name="scaling-with-multiple-steps"></a>Schaling met meerdere stappen

De bovenstaande voorbeelden zijn heel eenvoudig. Als u wilt meer agressieve over schaalbaarheid omhoog (of omlaag), kunt u echter zelfs meerdere schaal regels voor de dezelfde meetwaarde toevoegen. U kunt bijvoorbeeld twee schaal regels definiëren van processor percentage:

1. Met wilt verkleinen 1 exemplaar als percentage van de processor dan 60 is %
2. Met wilt verkleinen 3 exemplaren als percentage van de processor dan 85 is %

![Meerdere regels voor kleurenschaal](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Met deze aanvullende regel, als uw laden groter is dan 85% voordat u een actie schaal, u hebt twee extra exemplaren in plaats van één.

## <a name="scale-based-on-a-schedule"></a>Schaal op basis van een planning


De standaardinstelling bij het maken van een regel schaal wordt altijd toegepast. U kunt zien dat wanneer u op de kop van het profiel klikt:

![Profiel](./media/insights-how-to-scale/Insights_Profile.png)

U kunt echter hebben meer agressieve schaalbaarheid tijdens de dag of de week, dan in het weekend. U kan zelfs uw service volledig uitschakelen werkuren afgesloten.

1. Klik hiertoe op het profiel dat u hebt Selecteer van **Terugkeerpatroon** in plaats van **altijd,** en kies de gewenste tijden het profiel wilt toepassen.

2. Bijvoorbeeld, zodat u hebt geen profiel die van toepassing in de week is, in de vervolgkeuzelijst **dagen** Schakel **zaterdag** en **zondag**.

3. Als u wilt dat een profiel die van toepassing de overdag is, Stel in de **Begintijd** op de tijd waarop u beginnen wilt bij.
    ![Standaardterugkeerpatroon](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Klik op **OK**.

5. Vervolgens moet u het profiel dat u wilt toepassen op andere momenten toevoegen. Klik op de rij **Profiel toevoegen** .
    ![Niet op het werk](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Geef uw nieuwe, tweede, profiel een naam, zoals u deze **niet op het werk**kunt oproepen.

7. Selecteer **Terugkeerpatroon** opnieuw en kies het gewenste tijdens deze periode exemplaar telling-bereik.

8. Met het standaardprofiel, kiest u de **dagen** wilt u dit profiel toe te passen op en de **Begintijd** gedurende de dag.

>[AZURE.NOTE] Automatisch schalen wordt de zomertijd spaargeld regels gebruiken voor ongeacht **tijdzone** die u hebt geselecteerd. Echter tijdens zomertijd de UTC-tijd in het grondtal tijdzone weer, niet de zomertijd spaargeld UTC-tijd wordt weergegeven.

9. Klik op **OK**.

10. U moet nu de regels die u wilt toepassen tijdens uw tweede profiel toevoegen. Klik op **Regel toevoegen**en klikt u vervolgens kunt u dezelfde regel er tijdens het standaardprofiel.
    ![Regel uit werk toevoegen](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Zorg ervoor dat u beide een regel maken voor schaal af en schaal in, hetzij tijdens het profiel van het aantal exemplaar wordt alleen vergroten (of verkleinen).

12. Klik tot slot op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

* [De doelstellingen van de monitor-service](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd.
* [Schakel controle en diagnostische hulpprogramma's](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde hoge frequentie afmetingen van uw service.
* Cross een drempelwaarde voor [meldingen ontvangen](insights-receive-alert-notifications.md) wanneer operationele gebeurtenissen plaatsvinden of aan de doelstellingen.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt begrijpen precies hoe uw code wordt uitgevoerd in de cloud.
* [Gebeurtenissen weergeven en controlelogboeken](insights-debugging-with-events.md) voor meer informatie over alles die wijzigingen in uw service heeft plaatsgevonden.
* [Beschikbaarheid van de monitor en serverreactie van een webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met de toepassing inzichten zodat u als de pagina vinden kan is niet beschikbaar.
