<properties
    pageTitle="Wat moet u doen in het geval dat een Azure service-onderbreking van invloed op Azure virtuele machines | Microsoft Azure"
    description="Lees wat u moet doen in het geval dat een Azure service-onderbreking van invloed op Azure virtuele machines."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Wat moet u doen in het geval dat een Azure service-onderbreking van invloed op Azure virtuele machines

Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd beschikbaar zijn voor u zijn wanneer u deze nodig hebt. Hiermee dwingt u af buiten onze wil soms van invloed zijn op ons op een manier die ongeplande serviceonderbrekingen veroorzaken.

Microsoft biedt een Service Level (Agreement) voor de services als een resourcebeperkingen voor beschikbaarheid en connectiviteit. De SLA voor afzonderlijke Azure services kan worden gevonden op [Azure-serviceovereenkomsten niveau](https://azure.microsoft.com/support/legal/sla/).

Azure heeft al veel ingebouwde platform-functies die ondersteuning bieden voor maximaal beschikbare toepassingen. Lees voor meer informatie over deze services, [herstel en beschikbaarheid van Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

In dit artikel behandelt een waar herstellen van fouten, als er een storing vanwege de belangrijkste natuurlijke noodgevallen of algemene serviceonderbreking optreedt in een hele regio. Hierna ziet u niet vaak voorkomen exemplaren, maar u moet voorbereiden voor de mogelijkheid dat er een storing van een hele regio is. Als u een hele regio ervaringen een service-onderbreking, kunnen de lokaal overbodige kopieën van uw gegevens zou tijdelijk niet beschikbaar. Als u geografische herhaling hebt ingeschakeld, worden drie extra exemplaren van de opslag van Azure BLOB's en tabellen worden opgeslagen in een andere regio. In het geval van een volledige regionale storing of een noodgevallen waarin de primaire regio kan niet worden hersteld, remaps Azure alle DNS-vermeldingen naar het gebied geografische gerepliceerd.

>[AZURE.NOTE]Let erop dat u geen eventuele controle over dit proces hebt en dit alleen voor regio hele serviceonderbrekingen gebeurt. Reden, moet u ook zijn afhankelijk van andere toepassingsspecifieke back-strategieën om te bereiken van het hoogste niveau van de beschikbaarheid van. Zie de sectie op [gegevens strategieën voor het herstel](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery)voor meer informatie.

Als u deze niet vaak voorkomen exemplaren afhandelen, bieden wij de volgende richtlijnen voor Azure virtuele machines in het geval van een service-onderbreking van het gehele gebied waar uw toepassing Azure virtuele machines wordt geïmplementeerd.

##<a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op een herstel
In dit geval is geen actie ondernemen vereist. U weet dat we naar eer en geweten werken als u wilt herstellen beschikbare service. De huidige status van service kunt u zien op onze [Dashboard voor servicestatus van Azure-Service](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Dit is de beste optie als u geen Azure sites worden hersteld, VM back-up, leestoegang geografische-redundante opslag of geografische-redundante opslag vóór de verstoring hebt ingesteld. Als u hebt ingesteld geografische-redundante opslag of leestoegang geografische-redundante opslag voor het account opslag waar uw VM virtuele vaste schijven (VHD's) worden opgeslagen, kunt u zoeken als u wilt herstellen van de afbeelding VHD en probeert voor het inrichten van een nieuwe VM hieruit. Dit is niet een optie voorkeur omdat er geen garanties van synchronisatie van gegevens in, tenzij Azure VM back-up of herstel van Azure Site worden gebruikt. Dus is deze optie niet gegarandeerd.

Voor klanten die directe toegang tot virtuele machines, zijn de volgende twee opties beschikbaar.  

>[AZURE.NOTE]Let erop dat beide van de volgende opties de mogelijkheid van gegevensverlies hebben.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Optie 2: Een VM terugzetten uit een back-up
Voor klanten die een VM back-up hebt geconfigureerd, kunt u de VM terugzetten vanaf de back-up- en herstelbestanden punt.

Als u een nieuwe VM herstellen uit back-up van Azure, raadpleegt u [virtuele machines in Azure herstellen](../backup/backup-azure-restore-vms.md).

Als u van plan bent voor uw back-infrastructuur van Azure virtuele machines, raadpleegt u [Plan de back-infrastructuur van uw VM in Azure wordt aangegeven](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Optie 3: Een failover initiëren met behulp van Azure sites worden hersteld
Als u Azure Site herstel voor gebruik met uw risico Azure virtuele machines hebt geconfigureerd, kunt u uw VMs terugzetten vanaf hun replica's. Deze replica's kunnen zich bevinden op Azure of on-premises. In dit geval kunt u een nieuwe VM vanuit de bestaande replica. Als u uw VMs herstellen uit een replica Azure sites worden hersteld, raadpleegt u [IaaS van migreren Azure virtuele machines tussen Azure regio's met Azure sites worden hersteld](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Hoewel het besturingssysteem van Azure virtuele machines en gegevensschijven moeten worden gerepliceerd naar een secundaire VHD als ze zich in een geografische-redundante opslag of geografische-redundante opslag leestoegang account, wordt elke VHD onafhankelijk gerepliceerd. Dit niveau van herhaling garandeert consistentie niet over de gerepliceerde VHD's. Als uw toepassing en/of databases die gebruikmaken van deze gegevensschijven hebt afhankelijkheden van elkaar, is niet gegarandeerd dat alle VHD's als één momentopname worden gerepliceerd. Er is ook niet gegarandeerd dat de replica VHD uit geografische-redundante opslag of leestoegang geografische-redundante opslag in een consistente momentopname van de toepassing resulteren zal te starten de VM.

##<a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van een herstel en beschikbaarheid strategie, [herstel en beschikbaarheid van Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Zie [Azure tolerantie technische richtlijnen](../resiliency/resiliency-technical-guidance.md)voor het ontwikkelen van een gedetailleerde technische begrip van de mogelijkheden van een cloud-platform.

Als u wilt weten hoe u een back-up VMs, raadpleegt u [een Back-up Azure virtuele machines](../backup/backup-azure-vms.md).

Leer hoe u herstel van Azure-Site gebruiken om goedkeuringen en te automatiseren bescherming van uw fysieke (en virtuele) Windows en Linux machines die worden uitgevoerd op VMWare en Hyper-V VMs, raadpleegt u [Azure sites worden hersteld](https://azure.microsoft.com/documentation/learning-paths/site-recovery/).

Als u de instructies zijn niet wissen of als u wilt dat Microsoft naar de bewerkingen uitvoeren op uw naam, neemt u contact op met [Ondersteuning van de klant](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
