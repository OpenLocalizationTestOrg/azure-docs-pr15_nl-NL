<properties
    pageTitle="Verticaal schalen Azure virtuele machines met Azure automatisering | Microsoft Azure"
    description="Hoe u verticaal de schaal van een Linux virtuele Machine in antwoord op waarschuwingen met Azure automatisering bewaken"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machine-with-azure-automation"></a>Verticaal schalen Azure virtuele machines met Azure automatisering

Verticaal schalen, is het proces van het vergroten of verkleinen van de bronnen van een machine in antwoord op de werklast. In Azure kunt dit doen door de grootte van de virtuele Machine. Dit kan helpen in de volgende scenario 's

- Als de virtuele Machine niet vaak wordt gebruikt, kunt u het formaat naar beneden af op het kleinere naar uw maandelijkse verlagen
- Als de virtuele Machine een laden piek ziet is, het formaat kunt wijzigen in een groter formaat om uit te breiden van de capaciteit

Het overzicht voor de stappen hiervoor is als onder

1. Setup Azure automatisering voor toegang tot uw virtuele Machines
2. De verticale schaal van Azure automatisering runbooks importeren in uw abonnement
3. Een webhook toevoegen aan uw runbook
4. Een waarschuwing toevoegen aan uw virtuele machines

> [AZURE.NOTE] Vanwege de grootte van de eerste virtuele Machine, de grootte die deze kan worden aangepast, kan worden beperkt vanwege de beschikbaarheid van de andere grootte in het cluster die huidige virtuele Machine wordt geïmplementeerd in. In de gepubliceerde automatisering runbooks gebruikt in dit artikel we dit geval kunt verrichten en alleen schaal binnen de onder VM grootte paren. Dit betekent dat een Standard_D1v2 virtuele Machine wordt niet ineens naar Standard_G5 vergroot of verkleind tot Basic_A0.

>| VM die groter zijn schaal koppelen |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Setup Azure automatisering voor toegang tot uw virtuele Machines

Het eerste wat dat u moet doen is een Azure automatisering account maken die wordt gehost in het runbooks kon u de schaal van de exemplaren VM schaal instellen. De service automatisering geïntroduceerd onlangs de "Als account uitvoeren"-functie waardoor u de instelling van de Service hoofdsom voor het automatisch uitvoeren van de runbooks van de gebruiker namens heel eenvoudig. U vindt meer informatie over dit in het artikel hieronder:

* [Runbooks verifiëren met Azure uitvoeren als account](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>De verticale schaal van Azure automatisering runbooks importeren in uw abonnement

De runbooks die nodig zijn voor uw VM verticaal schaalbaarheid zijn al gepubliceerd in de galerie met Azure automatisering Runbook. U moet ze importeren in uw abonnement. U kunt informatie over het importeren van runbooks het volgende artikel lezen.

* [Runbook en module galerieën voor het automatiseren van Azure](../automation/automation-runbook-gallery.md)

De runbooks die moeten worden geïmporteerd worden in de onderstaande afbeelding weergegeven

![Runbooks importeren](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook

Nadat u hebt geïmporteerd toevoegen de runbooks u moet een webhook aan het runbook, zodat deze kan worden veroorzaakt door een melding van een virtuele Machine. De details van het maken van een webhook voor uw Runbook kunnen hier worden gelezen

* [Azure automatisering webhooks](../automation/automation-webhooks.md)

Zorg ervoor dat u de webhook kopiëren voordat u het dialoogvenster webhook afsluit, zoals u dit in het volgende gedeelte moet.

## <a name="add-an-alert-to-your-virtual-machine"></a>Een waarschuwing toevoegen aan uw virtuele machines

1. Selecteer VM-instellingen
2. Selecteer "Waarschuwingsregels"
3. Selecteer "Toevoegen melding"
4. Selecteer een meting de melding gestart op
5. Selecteer een voorwaarde, die tijdens voldaan kan ertoe leiden dat de melding naar fire
6. Selecteer een drempel voor de voorwaarde in stap 5. moet worden voldaan
7. Selecteer een periode aanduidt waarop de controle-service moeten worden gecontroleerd voor de voorwaarde en een drempelwaarde in stap 5 en 6
8. Plak in de webhook die u hebt gekopieerd uit de vorige sectie.

![Melding toevoegen aan VM 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Melding toevoegen aan VM 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)