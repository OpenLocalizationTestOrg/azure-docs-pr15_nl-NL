<properties 
    pageTitle="Een runbook testen in Azure automatisering | Microsoft Azure"
    description="Voordat u een runbook in Azure automatisering publiceert, kunt u testen om ervoor te zorgen dat werkt zoals verwacht.  In dit artikel wordt beschreven hoe een runbook testen en de uitvoer te bekijken."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Een runbook testen in Azure automatisering
Wanneer u een runbook test, de [conceptversie](automation-creating-importing-runbook.md#publishing-a-runbook) wordt uitgevoerd en acties die worden uitgevoerd worden voltooid. Geen werkervaring wordt gemaakt, maar de [uitvoer](automation-runbook-output-and-messages.md#output-stream) en [waarschuwingen en fouten](automation-runbook-output-and-messages.md#message-streams) -streams worden weergegeven in de Test deelvenster Uitvoer. Berichten naar de [uitgebreide Stream](automation-runbook-output-and-messages.md#message-streams) worden weergegeven in het deelvenster Uitvoer alleen als de [$VerbosePreference-variabele](automation-runbook-output-and-messages.md#preference-variables) is ingesteld op Doorgaan.

Hoewel de conceptversie wordt uitgevoerd, wordt het runbook nog steeds de werkstroom normaal wordt uitgevoerd en bewerkingen ten opzichte van resources in de omgeving worden uitgevoerd. Daarom moet u alleen runbooks in niet-productieve bronnen testen.

De procedure voor het testen van elk [type runbook](automation-runbook-types.md) is dezelfde en er is geen verschil tussen de tekstuele editor en de grafische editor in de portal van Azure testen.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Een runbook testen in de portal van Azure

U kunt werken met een [runbook type](automation-runbook-types.md) in de portal van Azure.

1. Open de conceptversie van het runbook in de [tekstuele editor](automation-editing-a-runbook.md#Portal) of de [grafische editor](automation-graphical-authoring-intro.md).
2. Klik op de knop **testen** om te openen van het blad Test.
3. Als het runbook parameters heeft, wordt ze worden vermeld in het linkerdeelvenster waarin u waarden moet worden gebruikt voor de test kunt geven.
4. Desgewenst kunt u de test uitvoeren op een [Hybride Runbook werknemer](automation-hybrid-runbook-worker.md) **Uitvoeren instellingen** wijzigen voor **Hybride werknemer** en selecteer de naam van de doelgroep.  Anders, houd de standaard **Azure** de test uitvoeren in de cloud.
5. Klik op de knop **Start** de test wilt starten.
6. Als het runbook [PowerShell werkstroom](automation-runbook-types.md#powershell-workflow-runbooks) of [grafische is](automation-runbook-types.md#graphical-runbooks), kunt u stoppen of onderbreken deze terwijl deze wordt getest met de knoppen onder het deelvenster Uitvoer. Wanneer u het runbook onderbreekt, dit is de huidige activiteit voltooid voordat het wordt onderbroken. Zodra het runbook is geschorst, kunt u deze stoppen of start u het opnieuw.
7. De uitvoer van het runbook in het deelvenster Uitvoer controleren.


## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over het maken of importeren van een runbook, [maken of importeren van een runbook in Azure automatisering](automation-creating-importing-runbook.md)
- Zie meer informatie over grafische Authoring [grafische beschikbaar is in Azure automatisering](automation-graphical-authoring-intro.md)
- Als u wilt beginnen met PowerShell werkstroom runbooks, raadpleegt u [Mijn eerste runbook voor PowerShell-werkstroom](automation-first-runbook-textual.md)
- Zie meer informatie over het configureren van runboks om terug te keren statusberichten en fouten, aanbevolen procedures, inclusief [Runbook uitvoer en berichten in Azure automatisering](automation-runbook-output-and-messages.md)