<properties
    pageTitle="Problemen met automatisch schalen met VM schaal Sets | Microsoft Azure"
    description="Problemen met automatisch schalen met VM schaal Sets. Meer informatie over veelvoorkomende problemen heeft voorgedaan en hoe u ze kunt oplossen."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Probleemoplossing automatisch schalen met VM schaal Sets

**Probleem** : u hebt een infrastructuur autoscaling gemaakt in Azure resourcemanager via VM schaal Sets – zoals door het implementeren van een sjabloon als volgt: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale: u hebt uw schaal regels die zijn gedefinieerd en deze werkt geweldig, behalve dat ongeacht hoeveel belasting u in de VMs plaatsen, deze automatisch schalen won't.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Er zijn enkele dingen die u rekening moet houden:

- Hoeveel cores elke VM heeft, en laadt u elke core?
 Het bovenstaande voorbeeld Azure Quickstart sjabloon heeft een script do_work.php, dat wordt geladen een enkel core. Als u een groter zijn dan een enkel core VM grootte zoals Standard_A1 of D1 VM gebruikt moet u deze belasting meerdere keren uitvoeren. Hoeveel aantal processorcores uw VMs [grootten voor Windows virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md) bekijken om te controleren

- Hoeveel VMs in de VM schaal stelt u mee bezig zijn werk voor elk?

    Een schaal van de gebeurtenis vindt alleen plaats wanneer het gemiddelde CPU over **alle** de VMs in een set schaal groter is dan de drempelwaarde, via de interne keer gedefinieerd in de regels automatisch schalen.

- U de schaal gebeurtenissen mist?

    Controleer dat de controlelogboeken Logboeken in Azure portal voor schaal gebeurtenissen. Wellicht er is een schaal en een schaal omlaag die is gemist. U kunt filteren op "Schaal"..

    ![Controlelogboeken bijhouden][audit]

- De schaal in- en schalen drempelwaarden voldoende verschillende zijn?

    Stel dat u een regel aan de nieuwe schaal af wanneer gemiddelde CPU groter dan 50% meer dan 5 minuten en op schaal is in wanneer gemiddelde CPU minder dan 50% is ingesteld. Dit zou "flapping" problemen veroorzaken wanneer CPU-gebruik deze drempel, lijkt met schaal acties voortdurend vergroten en verkleinen van de grootte van de set. Reden de automatisch schalen-service wordt geprobeerd om te voorkomen dat 'klapperen', die als niet schaal kunt bestandenlijst. Dus controleer of uw schalen en schaal in drempels verschillen voldoende toe te staan dat sommige ruimte tussen de schaalbaarheid.

- U uw eigen sjabloon JSON schrijven?

    Het is eenvoudig fouten maken door de dus beginnen met een sjabloon zoals een waarboven bewezen is werken en kleine incrementele wijzigingen aanbrengen. 

- Kunt u handmatig schalen in- of uitzoomen?

    Probeer het opnieuw distribueren van de resource VM schaal instellen met een instelling voor verschillende "capaciteit" het aantal VMs handmatig wijzigen. Een voorbeeldsjabloon hiervoor is hier: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – mogelijk moet u de sjabloon om te controleren of er dezelfde grootte machine tijdens het instellen van uw schaal gebruikt bewerken. Als u het aantal VMs succes handmatig wijzigen kunt, weet u dat het probleem is gerelateerd aan automatisch schalen.

- Controleer uw Microsoft.Compute/virtualMachineScaleSet, en Microsoft.Insights resources in [Azure Resource Explorer](https://resources.azure.com/)

    Dit is een onmisbaar hulpprogramma voor probleemoplossing waarin u de status van uw Azure resourcemanager resources. Klik op uw abonnement en kijkt u naar de resourcegroep die u wilt oplossen. Klik onder de provider van de resource berekeningscluster kijkt u naar de VM schaal instellen u hebt gemaakt en controleren van de weergave exemplaar, waarin u de status van een implementatie. Ook controleren of de weergave exemplaar van VMs in de VM schaal instellen. Vervolgens gaat u naar de provider van de resource Microsoft.Insights en schakel dat de regels automatisch schalen mooi uiterlijk te geven.

- Is de diagnostische extensie werken en die prestatiegegevens?

    __Update:__ Azure automatisch schalen is verbeterd als u wilt gebruiken, een host op basis van de doelstellingen pijplijn waarvoor u niet langer extensie diagnostische hulpprogramma's zijn geïnstalleerd. Dit betekent dat de volgende die paragrafen niet langer van toepassing als u een autoscaling toepassing maken met de nieuwe pijplijn. Voorbeelden van Azure sjablonen die zijn geconverteerd naar het gebruik van de pijplijn host zijn: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Met behulp van host op basis van de doelstellingen voor automatisch schalen is het beter om de volgende redenen:

    - Minder bewegende onderdelen als er geen extensies diagnostische gegevens moeten worden geïnstalleerd.
    - Eenvoudiger sjablonen. Inzichten automatisch schalen regels toe te voegen aan een bestaande schaal set-sjabloon.
    - Meer betrouwbare rapporteren en nieuwe VMs sneller gestart.

    De enige redenen dat u mogelijk wilt blijven gebruiken extensie diagnostische zou zijn als u nodig geheugen diagnostisch hulpprogramma rapportage/schaalbaarheid hebt. Host op basis van de doelstellingen rapporteren niet geheugen.

    Daarom alleen Volg de rest van dit artikel als u nog steeds diagnostische uitbreidingen voor uw autoscaling.

    Automatisch schalen in Azure resourcemanager kunt werken (maar niet meer heeft tot) met behulp van een VM extensie de extensie diagnostisch hulpprogramma genoemd. Hiermee plaatst u de prestatiegegevens met een opslag-account dat u in de sjabloon definieert. Deze gegevens wordt vervolgens samengevoegd door de Monitor Azure-service.

    Als de inzichten-service niet kunt van gegevens uit de VMs lezen, moet deze verzendt u een e-mailbericht – bijvoorbeeld alsof het VMs omlaag, dus Controleer uw e-mail (het één u hebt opgegeven bij het maken van de Azure-account).

    U kunt ook Ga en bekijk de gegevens zelf. Bekijk het Azure opslag-account met een cloud-Verkenner. U gebruikt voor voorbeeld met de [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), meld u aan en kies het Azure abonnement en de hulpprogramma's voor diagnose-opslagaccountnaam waarnaar wordt verwezen in de definitie van de extensie diagnostische gegevens in uw implementatiesjabloon...

    ![Cloud Explorer][explorer]

    Hier ziet u een reeks tabellen waar de gegevens van elke VM worden opgeslagen. Het duurt Linux en de CPU-meetwaarde als voorbeeld, kijkt u op de meest recente rijen. De Visual Studio cloud explorer ondersteunt een querytaal zodat u kunt een query zoals uitvoeren "tijdstempel BT datetime'2016-02-02T21:20:00Z'" om ervoor te zorgen dat u de meest recente gebeurtenissen (wordt ervan uitgegaan dat moment is in UTC). Gebruikt de gegevens die u in de zien er komen overeen met de regels schaal u instellen? In het onderstaande voorbeeld wordt de CPU voor machine 20 gestart via de laatste 5 minuten met groter wordende 100%...

    ![Opslag-tabellen][tables]

    Als de gegevens niet er, klikt u vervolgens geeft deze dat het probleem is met de diagnostische extensie uitgevoerd in de VMs. Als de gegevens aanwezig is, betekent dit dat er is een probleem met de regels schaal of met de service inzichten. [Azure Status](https://azure.microsoft.com/status/)controleren.

    Nadat u via deze stappen geworden bent als u nog steeds problemen automatisch schalen u probeer de forums op [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)of [stapelen overloop](http://stackoverflow.com/questions/tagged/azure)of meld u belt. Zorg ervoor dat de sjabloon en een weergave van de prestatiegegevens delen.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
