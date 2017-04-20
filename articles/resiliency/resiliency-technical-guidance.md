<properties
   pageTitle="Tolerantie technische richtlijnen index | Microsoft Azure"
   description="Index van technische artikelen op lidmaatschap en ontwerpen robuuste, ten zeerste beschikbaar, fouttolerantie toepassingen, evenals planning voor herstel en noodgevallen een optimale bedrijfscontinuïteit"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance"></a>Technische ondersteuning van Azure tolerantie

##<a name="introduction"></a>Inleiding

Beschikbaarheid en vereisten voor noodgevallen herstel de vergadering hebt u twee soorten knowledge nodig:

- Gedetailleerde technische lidmaatschap van een cloud-platform mogelijkheden
- Kennis beschikken om te correct ontwerpen een gedistribueerde service

Deze reeks artikelen behandelt de voormalige: de mogelijkheden en beperkingen van de Azure platform met betrekking tot tolerantie (ook wel bedrijfscontinuïteit genoemd). Als u geïnteresseerd in de laatste bent, raadpleegt u de reeks artikel is gericht op [herstel en beschikbaarheid van Azure-toepassingen](https://aka.ms/drtechguide). Hoewel deze reeks artikel raakt op architectuur en een ontwerp patronen, maar dat is niet de focus verplaatsen van de reeks. Voor Ontwerphulp, kunt u het materiaal dat in de sectie [Extra bronnen](#additional-resources) raadplegen.

De informatie is ingedeeld in de volgende artikelen:

- [Herstel van lokale fouten](resiliency-technical-guidance-recovery-local-failures.md).
Fysieke hardware (bijvoorbeeld stations, servers en netwerkapparaten) kan mislukken. Resources kunnen worden leeg wanneer laden bereikt. In dit artikel worden de mogelijkheden die Azure biedt voor het behoud van beschikbaarheid deze voorwaarden wordt voldaan.

- [Herstel van een Azure regio hele service-onderbreking](resiliency-technical-guidance-recovery-loss-azure-region.md).
Algemene fouten zijn niet vaak voorkomen, maar ze zijn theorie. Hele regio's kunnen worden geïsoleerd vanwege netwerkstoringen of ze kunnen fysiek beschadigd na natuurlijke systeemfouten. In dit artikel wordt uitgelegd hoe u met Azure toepassingen die geografisch diverse regio's beslaan maken.

- [Herstel vanuit on-premises naar Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
De cloud wijzigt aanzienlijk de kosten van herstel, zodat organisaties Azure tot stand brengen van een tweede site voor herstel gebruiken. U kunt dit doen bij een deel van de kosten van bouwen en onderhouden van een secundaire datacenter. In dit artikel wordt uitgelegd dat de mogelijkheden die Azure biedt voor het uitbreiden van een datacenter van de on-premises implementatie in de cloud.

- [Herstel van beschadiging of per ongeluk wordt verwijderd](resiliency-technical-guidance-recovery-data-corruption.md).
Toepassingen kunnen bugs bij te werken die beschadigde gegevens bevatten. Operatoren kunnen onjuist belangrijke gegevens verwijderen. In dit artikel wordt uitgelegd wat Azure biedt voor back-ups van gegevens en het terugzetten naar een vorig punt deze tijd.

##<a name="additional-resources"></a>Aanvullende informatie

- [Problemen oplossen en beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
In dit artikel is een gedetailleerd overzicht van beschikbaarheid en herstel. Deze bedekt de uitdaging van handmatige replicatie verwijzing en transacties gegevens. De uiteindelijke secties bevatten samenvattingen van verschillende soorten noodgevallen herstel topologieën die Azure regio's voor het hoogste niveau van de beschikbaarheid van's beslaan.

- [Controlelijst voor hoge beschikbaarheid](resiliency-high-availability-checklist.md).
In dit artikel is een lijst met functies, services, en ontwerpen waarmee u kunt de tolerantie en de beschikbaarheid van uw toepassing vergroten.

- [Richtlijnen voor tolerantie van Microsoft Azure-Services](resiliency-service-guidance-index.md).
In dit artikel is een index van Azure services en koppelingen naar zowel noodgevallen herstel instructies en richtlijnen.

- [Overzicht: Cloud bedrijven herstel bedrijfscontinuïteit en een database met SQL-Database](../sql-database/sql-database-business-continuity.md).
In dit artikel biedt Azure SQL Database-technieken voor beschikbaarheid. Deze hoofdzakelijk op back-up en herstellen strategieën gecentreerd. Wanneer u Azure SQL-Database in de cloudservice gebruikt, raadpleegt u dit artikel en de gerelateerde bronnen.

- [Beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
In dit artikel wordt beschreven hoe de van de beschikbaarheidopties die u verkennen kunt wanneer u infrastructuur als een service (IaaS) voor het hosten van uw databaseservices gebruikt. Deze worden besproken AlwaysOn beschikbaarheidsgroepen, spiegelen, logboekbestanden en back-up/herstellen. Verschillende zelfstudies hoe deze technieken gebruiken.

- [Aanbevolen procedures voor het ontwerpen van grootschalige services op Azure-Cloudservices](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
In dit artikel bevat informatie over het ontwikkelen van zeer scalable cloud architecturen. Veel van de technieken die u gebruikmaken van de schaalbaarheid te verbeteren ook verbeteren beschikbaarheid. Als uw toepassing kan geen onder verbeterde laden schalen, wordt het schaalbaarheid van gegevens ook een beschikbaarheidsprobleem.

- [Back-up en herstellen voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Dit artikel bevat technische informatie over hoe u een back-up en herstellen van Microsoft SQL Server op Azure virtuele Machines wordt uitgevoerd.

- [Failsafe: richtlijnen voor het robuuste cloud architecturen](https://channel9.msdn.com/Series/FailSafe).
In dit artikel bevat richtlijnen voor het samenstellen van robuuste cloud architecturen, richtlijnen voor de uitvoering van deze architecturen op Microsoft-technologieën en recepten voor de uitvoering van deze architecturen voor specifieke scenario's.

- [Technische casestudy: cloud-technologieën gebruiken om te verbeteren herstel](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Deze casestudy ziet u hoe Microsoft IT Azure gebruikt voor het verbeteren van herstel.

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks die zijn gericht op technische richtlijnen voor het Azure tolerantie. Als u geïnteresseerd bent in andere artikelen in deze reeks lezen, kunt u beginnen met [herstel van lokale fouten](resiliency-technical-guidance-recovery-local-failures.md).
