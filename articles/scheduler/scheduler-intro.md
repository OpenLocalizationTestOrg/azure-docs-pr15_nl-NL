<properties
 pageTitle="Wat is Azure Scheduler? | Microsoft Azure"
 description="Azure Scheduler kunt u declaratieve beschrijven acties worden uitgevoerd in de cloud. Er vervolgens plant en deze acties automatisch wordt uitgevoerd."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Wat is Azure Scheduler?

Azure Scheduler kunt u declaratieve beschrijven acties worden uitgevoerd in de cloud. Er vervolgens plant en deze acties automatisch wordt uitgevoerd.  Scheduler doet dit met behulp van [de Azure-portal](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx)of Azure PowerShell.

Scheduler maakt, onderhoudt en Hiermee wordt de geplande hoeveelheid werk.  Scheduler niet alle werkbelasting hosten of elke willekeurige code uitvoeren. IT alleen _roept_ code die ergens anders worden gehost, in Azure wordt aangegeven, on-premises of met een andere provider. Deze aanroept via HTTP, HTTPS, een wachtrij opslag, een service bus wachtrij of een service bus onderwerp.

Planner [taken](scheduler-concepts-terms.md)plant, blijft een geschiedenis van een taak kan worden uitgevoerd resultaten dat een bekijken kunt en jokertekenelement en betrouwbaar werkbelasting plant moet worden uitgevoerd. Azure WebJobs (onderdeel van de functie Web Apps in Azure App Service) en andere mogelijkheden plannen Azure Scheduler op de achtergrond gebruikt. De [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) helpt bij de communicatie voor deze acties. Als zodanig ondersteunt Scheduler [complexe planningen en geavanceerde terugkeerpatroon](scheduler-advanced-complexity.md) eenvoudig.

Er zijn verschillende scenario's waarin zich aan het gebruik van Scheduler lenen. Bijvoorbeeld:

+ _Acties in bedrijfstoepassingen terugkerend:_ Regelmatig gegevens verzamelen uit Twitter in een feed.
+ _Dagelijkse onderhoud:_ Dagelijkse weghalen van Logboeken, back-ups en andere onderhoudstaken uitvoeren. Bijvoorbeeld een beheerder bepaalt of back-up van de database om 1:00 elke dag van de volgende negen maanden.

Scheduler kunt maken, bijwerken, verwijderen, weergeven en taken en [taak verzamelingen](scheduler-concepts-terms.md) via programmacode, beheren met behulp van scripts en in de portal.

## <a name="see-also"></a>Zie ook

 [Azure Scheduler concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met Scheduler in de portal van Azure](scheduler-get-started-portal.md)

 [Abonnementen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Het maken van complexe planningen en geavanceerde terugkeerpatroon met Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API-verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Overzicht van Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)
