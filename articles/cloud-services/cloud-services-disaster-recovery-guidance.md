<properties
    pageTitle="Wat moet u doen in het geval van een Azure service-onderbreking invloed Azure-Cloudservices | Microsoft Azure"
    description="Lees wat u moet doen in het geval van een Azure service-onderbreking invloed Azure-Cloudservices."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Wat moet u doen in het geval van een Azure service-onderbreking invloed Cloudservices van Azure

Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd beschikbaar zijn voor u zijn wanneer u deze nodig hebt. Hiermee dwingt u af buiten onze wil soms van invloed zijn op ons op een manier die ongeplande serviceonderbrekingen veroorzaken.

Microsoft biedt een Service Level (Agreement) voor de services als een resourcebeperkingen voor beschikbaarheid en connectiviteit. De SLA voor afzonderlijke Azure services kan worden gevonden op [Azure-serviceovereenkomsten niveau](https://azure.microsoft.com/support/legal/sla/).

Azure heeft al veel ingebouwde platform-functies die ondersteuning bieden voor maximaal beschikbare toepassingen. Lees voor meer informatie over deze services, [herstel en beschikbaarheid van Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

In dit artikel behandelt een waar herstellen van fouten, als er een storing vanwege de belangrijkste natuurlijke noodgevallen of algemene serviceonderbreking optreedt in een hele regio. Hierna ziet u niet vaak voorkomen exemplaren, maar u moet voorbereiden voor de mogelijkheid dat er een storing van een hele regio is. Als u een hele regio ervaringen een service-onderbreking, kunnen de lokaal overbodige kopieën van uw gegevens zou tijdelijk niet beschikbaar. Als u geografische herhaling hebt ingeschakeld, worden drie extra exemplaren van de opslag van Azure BLOB's en tabellen worden opgeslagen in een andere regio. In het geval van een volledige regionale storing of een noodgevallen waarin de primaire regio kan niet worden hersteld, remaps Azure alle DNS-vermeldingen naar het gebied geografische gerepliceerd.

>[AZURE.NOTE]Let erop dat u geen eventuele controle over dit proces hebt en dit alleen voor datacenter hele serviceonderbrekingen gebeurt. Reden, moet u ook zijn afhankelijk van andere toepassingsspecifieke back-strategieën om te bereiken van het hoogste niveau van de beschikbaarheid van. Zie het gedeelte over [gegevens strategieën voor het herstel](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR)voor meer informatie. Als u kunnen van invloed zijn op uw eigen failover wilt, wilt u mogelijk Houd rekening met het gebruik van [leestoegang geografische-redundante opslag (AB-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), maakt een alleen-lezen kopie van uw gegevens in een andere regio.

Als u deze niet vaak voorkomen exemplaren afhandelen, bieden we de volgende richtlijnen voor Azure virtuele machines (VMs) in het geval van een service-onderbreking van het gehele gebied waar uw Azure VM-toepassing wordt geïmplementeerd.

##<a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op een herstel
In dit geval is geen actie ondernemen vereist. U weet dat Azure teams naar eer en geweten werkt als u wilt herstellen beschikbare service. De huidige status van service kunt u zien op onze [Dashboard voor servicestatus van Azure-Service](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Dit is de beste optie als een klant herstel van Azure-Site niet ingesteld of een secundaire implementatie in een andere regio heeft.

Voor klanten die directe toegang tot hun geïmplementeerd cloudservices, zijn de volgende opties beschikbaar.

>[AZURE.NOTE]Let erop dat deze opties de mogelijkheid van gegevensverlies hebben.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Optie 2: De configuratie van uw cloud-service naar een nieuwe gebied opnieuw te implementeren

Als u uw oorspronkelijke code hebt, kunt u gewoon alleen de toepassing, gekoppeld configuratie en bijbehorende hulpbronnen naar een nieuwe cloudservice in een nieuwe regio implementeren.  

Zie voor meer informatie over het maken en implementeren van een cloud-servicetoepassing [maken en implementeren van een cloudservice](./cloud-services-how-to-create-deploy-portal.md).

Afhankelijk van uw toepassing-gegevensbronnen moet u mogelijk de herstelprocedures voor uw toepassingsgegevensbron controleren.
  * Zie voor gegevensbronnen Azure Storage [Azure Storage herhaling](../storage/storage-redundancy.md#read-access-geo-redundant-storage) om te controleren of de opties die beschikbaar zijn op basis van het model Kies herhaling voor uw toepassing.
  * Lees voor bronnen van de SQL-Database, [Overzicht: Cloud bedrijven herstel bedrijfscontinuïteit en een database met SQL-Database](../sql-database/sql-database-business-continuity.md) om te controleren of de opties die beschikbaar zijn op basis van het door u gekozen herhaling model voor uw toepassing.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Optie 3: Een back-implementatie via Azure verkeer beheer gebruiken
Deze optie wordt ervan uitgegaan dat u al uw toepassing-oplossing met regionale herstel in gedachten hebt ontworpen. U kunt deze optie als u al een secundaire cloud services-toepassingsimplementatie die wordt uitgevoerd in een andere regio en verbonden via een verkeer manager kanaal. In dit geval de status van de secundaire implementatie controleren. Als het goed is, kunt u verkeer omleiden naar deze via Azure verkeer Manager. Met deze strategie, kunt u profiteren van de methode verkeer en failover volgorde configuraties in Azure verkeer Manager. Lees [hoe u het verkeer Manager-instellingen configureren](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings)voor meer informatie.

![Azure-Cloudservices te verdelen over regio's met Azure verkeer Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van een herstel en beschikbaarheid strategie, [herstel en beschikbaarheid van Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Zie [Azure tolerantie technische richtlijnen](../resiliency/resiliency-technical-guidance.md)voor het ontwikkelen van een gedetailleerde technische begrip van de mogelijkheden van een cloud-platform.

Neem contact op met [De klantenondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)als zijn de instructies niet uit te schakelen, of als u wilt dat Microsoft naar de bewerkingen uitvoeren op uw naam.
