<properties
pageTitle="Het bijwerken van een cloudservice | Microsoft Azure"
description="Informatie over het bijwerken van cloudservices in Azure wordt aangegeven. Informatie over hoe een update op een cloudservice zodat deze zeker beschikbaar wordt uitgevoerd."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Het bijwerken van een cloudservice

## <a name="overview"></a>Overzicht
Bij 10.000 poten is een cloudservice, waaronder de rollen en de Gast-besturingssysteem bijwerken een drie stappen. Eerst moeten de binaire bestanden en de configuratiebestanden voor de nieuwe cloudservice of de versie van het besturingssysteem worden geüpload. Azure behoudt daarna berekeningscluster en netwerk bronnen voor de cloudservice op basis van de vereisten van de nieuwe versie van de cloud-service. Tot slot Azure kunt uitvoeren van een lopende upgrade als u wilt de tenant stapsgewijs bijwerken naar de nieuwe versie of de Gast-besturingssysteem behoud van uw beschikbaarheid. In dit artikel wordt beschreven hoe de details van deze laatste stap – de lopende upgrade.

## <a name="update-an-azure-service"></a>Bijwerken van een Service van Azure

Azure organiseert uw rol-sessies in logische groepen upgrade domeinen (per) genoemd. De upgrade domeinen (per) zijn logische sets met rol-processen die worden bijgewerkt als een groep.  Azure updates per cloud service één per per keer, waarmee exemplaren in andere UDs kunt doorgaan met het verkeer treden.

Het standaardaantal upgrade domeinen is 5. U kunt een ander nummer van de upgrade domeinen opgeven door het kenmerk upgradeDomainCount in van de service-definitiebestand (.csdef). Zie voor meer informatie over het kenmerk upgradeDomainCount, [WebRole Schema](https://msdn.microsoft.com/library/azure/gg557553.aspx) of [WorkerRole Schema](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Wanneer u een update in-place van een of meer functies in uw service uitvoert, bijgewerkt Azure sets van de rol exemplaren op basis van de upgrade domein waartoe ze behoren. Azure updates van de exemplaren in een bepaald upgrade domein – stoppen, bijwerken, waarmee ze ongedaan maken online – vervolgens wordt verplaatst naar het volgende domein. Azure wordt gecontroleerd door stoppen alleen de exemplaren die in het huidige domein voor de upgrade is uitgevoerd, of dat een update naar de actieve service met de minste mogelijke impact plaatsvindt. Zie [hoe de update voortgezet](#howanupgradeproceeds) verderop in dit artikel voor meer informatie.

> [AZURE.NOTE] Hoewel de termen **bijwerken** en een **upgrade** hebt iets anders zin in de context Azure, kunnen ze door elkaar worden gebruikt voor de processen en beschrijvingen van functies in dit document.

Uw service moet ten minste twee instanties van een rol voor die rol moeten worden bijgewerkt definiëren in-place zonder uitvaltijd. Als de service uit slechts één exemplaar van een rol bestaat, wordt uw service niet beschikbaar totdat de update in-place is voltooid.

Dit onderwerp behandelt de volgende informatie over updates voor Azure:

-   [Servicewijzigingen toegestaan tijdens een update](#AllowedChanges)
-   [Hoe een upgrade wordt uitgevoerd](#howanupgradeproceeds)
-   [Ongedaan maken van een update](#RollbackofanUpdate)
-   [Meerdere mutating bewerkingen in een lopend implementatie initiëren](#multiplemutatingoperations)
-   [Verdeling van rollen voor de upgrade domeinen](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Servicewijzigingen toegestaan tijdens een update
De volgende tabel ziet u de toegestane wijzigingen in een service tijdens een update:

|Wijzigingen mogen hostingprovider, services en rollen|In-place bijwerken|Gefaseerde (VIP omwisselen)|Verwijderen en opnieuw te implementeren|
|---|---|---|---|
|Versie van besturingssysteem|Ja|Ja|Ja
|.NET vertrouwensniveau|Ja|Ja|Ja|
|VM grootte<sup>1</sup>|Ja<sup>2</sup>|Ja|Ja|
|Instellingen voor lokale opslag|Alleen<sup>2</sup> vergroten|Ja|Ja|
|Toevoegen of verwijderen van rollen in een service|Ja|Ja|Ja|
|Het aantal exemplaren van een bepaalde rol|Ja|Ja|Ja|
|Getal of type eindpunten van een service|Ja<sup>2</sup>|Nee|Ja|
|Namen en waarden van de configuratie-instellingen|Ja|Ja|Ja|
|Een waarde (maar niet gebruikersnamen) configuratie-instellingen|Ja|Ja|Ja|
|Nieuwe certificaten toevoegen|Ja|Ja|Ja|
|Bestaande certificaten wijzigen|Ja|Ja|Ja|
|Nieuwe code implementeren|Ja|Ja|Ja|
<sup>1</sup> Grootte wijzigen is beperkt tot de subset van formaten beschikbaar voor de cloudservice.

<sup>2</sup> Azure SDK 1,5 of hoger vereist.

> [AZURE.WARNING] De grootte VM wijzigen, gaan verloren lokale gegevens.


De volgende items worden niet ondersteund tijdens een update:

-   De naam van een rol wijzigen. Verwijderen en voegt u de rol met de nieuwe naam.
-   Wijzigen van het domein upgraden tellen.
-   Verkleinen van de lokale resources.

Als u andere updates zijn aanbrengt in de definitie van uw service, zoals het verkleinen van lokale resource, moet u in plaats daarvan een VIP omwisselen update uitvoeren. Zie voor meer informatie [Implementatie uitwisselen](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Hoe een upgrade wordt uitgevoerd
U kunt beslissen of u wilt bijwerken alle de rollen in uw service of één rol in de service. In beide gevallen, worden alle instanties van elke functie die wordt bijgewerkt en deel uitmaakt van het eerste domein dat de upgrade gestopt, bijgewerkt en opnieuw online gebracht. Als ze weer online bent, zijn de exemplaren in het tweede upgrade domein gestopt, bijgewerkt en opnieuw online gebracht. Een cloudservice kan maximaal één upgrade tegelijk actief hebben. De upgrade wordt altijd ten opzichte van de nieuwste versie van de cloudservice uitgevoerd.

In het volgende diagram ziet u hoe de upgrade wordt uitgevoerd als u een alle van de rollen in de service upgrade:

![Upgrade van service] (media/cloud-services-update-azure-service/IC345879.png "Upgrade van service")

Dit volgende diagram ziet u hoe de update als u een slechts één functie upgrade verdergaat:

![Rol upgraden] (media/cloud-services-update-azure-service/IC345880.png "Rol upgraden")  

> [AZURE.NOTE] Bij een upgrade van een service van één exemplaar naar meerdere exemplaren uw service doorgevoerd tijdens de upgrade vanwege de manier Azure upgrades services wordt uitgevoerd. De beschikbaarheid voor service serviceovereenkomst service aansprakelijke geldt alleen voor de services die zijn geïmplementeerd met meer dan één exemplaar. De volgende lijst wordt beschreven hoe de gegevens op elke schijf wordt beïnvloed door elke Azure-service-upgrade scenario:
>
>VM opnieuw opstarten:
>
-   C: behouden
-   D: behouden
-   E-behouden
>
>Portal opnieuw opstarten:
>
-   C: behouden
-   D: behouden
-   E-verwijderd
>
>Portal Reimage:
>
-   C: behouden
-   D: verwijderd
-   E-verwijderd

>In-Place Upgrade:
>
-   C: behouden
-   D: behouden
-   E-verwijderd
>
>Knooppunt migratie:
>
-   C: verwijderd
-   D: verwijderd
-   E-verwijderd

>Houd er rekening mee dat, in de bovenstaande lijst, het e-station van de functie hoofdsite station vertegenwoordigt, en niet vastgelegde worden moet. Gebruik in plaats daarvan de omgevingsvariabele % RoleRoot % om aan te geven van de schijf.

>Als u wilt de uitvaltijd bij het bijwerken van een enkel exemplaar-service, een nieuwe service met meerdere exemplaren implementeren naar de server tijdelijk opslaan en voer een VIP uitwisselen.

Tijdens een automatische update evalueert de Controller uit Azure stof regelmatig de status van de cloudservice om te bepalen wanneer het naar de volgende per begeleiden veilig is. De evaluatie van deze status wordt uitgevoerd op basis van de per rol en acht alleen exemplaren van de nieuwste versie (dat wil zeggen exemplaren van UDs die u hebt al is toen). Dit wordt gecontroleerd of een minimum aantal exemplaren van de rol, voor elke rol een goede terminal staat hebt bereikt.

### <a name="role-instance-start-timeout"></a>Time-out voor rol exemplaar starten
De Controller stof wacht 30 minuten voor elk exemplaar van de rol aan een slag staat hebt bereikt. Als de duur time-out verstrijkt, blijft de Controller stof pakket op het volgende exemplaar van de rol.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Ongedaan maken van een update
Azure biedt flexibiliteit bij het beheren van services tijdens een update doordat u extra bewerkingen op een service, starten na de eerste update-aanvraag is geaccepteerd door de Controller uit Azure stof. Een terugdraaien kan alleen worden uitgevoerd wanneer een update (configuratie wijzigen) of upgrade is in de stand **uitgevoerd** op de implementatie. Een update of de upgrade wordt beschouwd als in uitvoering zo lang maken als er altijd ten minste één exemplaar van de service die nog niet is bijgewerkt naar de nieuwe versie. Als u wilt testen of een terugdraaien is toegestaan, controleert u de waarde van de vlag RollbackAllowed, die het resultaat van [Krijgen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) - en [Cloud Service eigenschappen](https://msdn.microsoft.com/library/azure/ee460806.aspx) bewerkingen, is ingesteld op waar.

> [AZURE.NOTE] Alleen is het handig om te bellen ongedaan maken op een update **in-place** of upgraden omdat VIP omwisselen upgrades gebruikmaakt van één exemplaar van de hele actieve van uw service vervangen door een andere.

Ongedaan maken van een update in uitvoering heeft de volgende effecten op de implementatie:

-   Alle exemplaren van de rol die nog niet had zijn bijgewerkt of bijgewerkt naar de nieuwe versie zijn niet bijgewerkt of bijgewerkt, omdat deze processen al actief zijn voor de doelversie van de service.
-   Alle exemplaren van de rol die al had zijn bijgewerkt of bijgewerkt naar de nieuwe versie van het servicepakket (\*.cspkg) bestand of de service te configureren (\*.cscfg)-bestand (of beide bestanden) worden weer op de voorafgaand aan de upgrade-versie van deze bestanden.

Dit is functioneel bepaald door de volgende functies:

-   De bewerking [Ongedaan maken bijwerken of bijwerken](https://msdn.microsoft.com/library/azure/hh403977.aspx) , die kan worden aangeroepen voor een configuratie-update (geactiveerd door bellen [Wijzigen implementatieconfiguratie](https://msdn.microsoft.com/library/azure/ee460809.aspx)) of een upgrade (geactiveerd door bellen [Implementatie upgraden](https://msdn.microsoft.com/library/azure/ee460793.aspx)) zo lang maken als er altijd ten minste één exemplaar in de service die nog niet is bijgewerkt naar de nieuwe versie.
-   Het element vergrendeld en het element RollbackAllowed die worden geretourneerd als onderdeel van de hoofdtekst van het antwoord van de [Implementatie ophalen](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service eigenschappen](https://msdn.microsoft.com/library/azure/ee460806.aspx) :
    1.  Het element vergrendeld kunt u bepalen wanneer een mutating bewerking kan worden aangeroepen voor een bepaald implementatie.
    2.  Het element RollbackAllowed kunt u bepalen wanneer de bewerking [Ongedaan maken Update of Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) kan worden aangeroepen voor een bepaald implementatie.

    Om te kunnen uitvoeren terugdraaien, er geen zowel de vergrendeld en de elementen RollbackAllowed controleren. Deze brondomein\Administrator om te bevestigen dat RollbackAllowed is ingesteld op waar. Deze elementen worden alleen geretourneerd als deze methoden worden aangeroepen met behulp van de aanvraagheader is ingesteld op "x-ms-versie: 10-01-2011 ' of een latere versie. Zie voor meer informatie over versiebeheer kopteksten, [Service Management versiebeheer](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Er zijn situaties waarin een terugdraaien van een update of upgrade wordt niet ondersteund, zijn dit als volgt:

-   Beperking van lokale bronnen - als de update de lokale bronnen voor een rol het Azure platform vergroot mag niet terugdraaien. 
-   Quotabeperkingen - hebben als het bijwerken is een schaal omlaag bewerking die u mogelijk niet meer voldoende berekeningscluster quotum om de bewerking ongedaan maken te voltooien. Elke Azure abonnement heeft een quotum is gekoppeld aan waarmee het maximum aantal die kan worden gebruikt door alle gehoste services die deel uitmaakt van abonnement. Als uw abonnement uitvoering van een terugdraaien van een bepaalde update zou plaats op de quota en die wordt niet terugdraaien ingeschakeld.
-   Raceauto voorwaarde - als de eerste update is voltooid, terugdraaien is niet mogelijk.

Een voorbeeld van wanneer het terugdraaien van een update handig kan zijn is als u de bewerking [Implementatie upgraden](https://msdn.microsoft.com/library/azure/ee460793.aspx) gebruikt in de handmatige modus aan een besturingselement voor het tarief weer waarop een belangrijke in-place upgrade uw Azure die worden gehost service wordt geïmplementeerd.

Tijdens de implementatie van de upgrade [Upgraden implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx) in handmatige modus bellen en begint begeleidt upgrade domeinen. Als op een bepaald moment controleren als u de upgrade, sommige gevallen rol in de eerste upgrade domeinen die u hebt reageert te noteren, kunt u de bewerking [Ongedaan maken bijwerken of bijwerken](https://msdn.microsoft.com/library/azure/hh403977.aspx) op de implementatie, waarin ongewijzigd blijft de exemplaren die hebt u er nog niet bijgewerkt en terugdraaien exemplaren die had zijn bijgewerkt naar de vorige servicepakket en configuratie bellen.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Meerdere mutating bewerkingen in een lopend implementatie initiëren
In sommige gevallen wilt u mogelijk meerdere gelijktijdige mutating bewerkingen voor een lopend implementatie initiëren. Zoals u mogelijk een bijwerkt van de service en, terwijl update is die wordt geïmplementeerd via uw service, die u wilt maken van sommige wijzigen, bijvoorbeeld als u wilt implementeren de update terug, een andere update toepassen of zelfs verwijderen de implementatie. Een zaak waarin dit mogelijk is als een service-upgrade bevat buggy code die ervoor zorgt dat een exemplaar van de bijgewerkte rol herhaaldelijk vastloopt. In dit geval de Controller uit Azure stof worden niet voortgang bij de toepassing waarmee de upgrade omdat een onvoldoende aantal exemplaren in de bijgewerkte domein in orde zijn. Deze status wordt genoemd een *hangen implementatie*. U kunt de implementatie doen door de update terugdraaien of een vers update over heen mislukken van de een.

Nadat de eerste aanvraag om te werken of de upgrade van de service is ontvangen door de Azure stof Controller, kunt u verdere mutating bewerkingen starten. Dat wil zeggen, hoeft u niet te wachten om door de ingebruikname tot voltooien voordat u kunt een andere mutating bewerking.

Een tweede updatebewerking starten tijdens de eerste update lopende is wordt vergelijkbaar met de bewerking ongedaan maken uitgevoerd. Als de tweede update zich in de automatisch ingeschakelde modus, wordt het eerste domein dat de upgrade bijgewerkt onmiddellijk mogelijk waardoor exemplaren van meerdere upgrade domeinen wordt offline op het hetzelfde punt in tijd.

De mutating bewerkingen zijn als volgt: [Wijzigen implementatieconfiguratie](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Upgrade implementatie](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Bijwerkstatus implementatie](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Implementatie verwijderen](https://msdn.microsoft.com/library/azure/ee460815.aspx)en [Terugdraaien bijwerken of bijwerken](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Twee bewerkingen, [Krijgen implementatie](https://msdn.microsoft.com/library/azure/ee460804.aspx) en [Cloud Service eigenschappen](https://msdn.microsoft.com/library/azure/ee460806.aspx), retourneren de vlag vergrendeld die kan worden onderzocht om te bepalen of een mutating bewerking kan worden aangeroepen voor een bepaald implementatie.

Om aan te roepen de versie van de volgende manieren waarop geeft als de vlag vergrendeld resultaat, moet u aanvraag koptekst ingesteld op "x-ms-versie: 10-01-2011 ' of een later. Zie voor meer informatie over versiebeheer kopteksten, [Service Management versiebeheer](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Verdeling van rollen voor de upgrade domeinen
Azure Hiermee stelt exemplaren van een rol gelijkmatig over een bepaald aantal upgrade domeinen die kan worden geconfigureerd als onderdeel van de service-definitiebestand (.csdef). Het maximum aantal upgrade domeinen is 20 en de standaardwaarde is 5. Zie voor meer informatie over het wijzigen van de service-definitiebestand [Azure Service definitie Schema (.csdef bestand)](cloud-services-model-and-package.md#csdef).

Als uw rol tien exemplaren heeft, al dan niet standaard bevat elk domein de upgrade bijvoorbeeld twee instanties. Als uw rol 14 exemplaren heeft, klikt u vervolgens vier van de domeinen van de upgrade drie exemplaren bevatten en een vijfde domein bevat twee.

De upgrade domeinen worden aangegeven met een op nul gebaseerde index: het eerste domein dat de upgrade heeft een ID van 0 en het tweede upgrade domein heeft een ID van 1, enzovoort.

In het volgende diagram ziet u hoe een service dan bevat twee rollen zijn verdeeld wanneer de service definieert twee upgrade domeinen. De service wordt uitgevoerd acht exemplaren van de Webrol en negen exemplaren van de rol werknemer.

![Verdeling van de Upgrade-domeinen] (media/cloud-services-update-azure-service/IC345533.png "Verdeling van de Upgrade-domeinen")

> [AZURE.NOTE] Houd er rekening mee dat Azure bepaalt hoe exemplaren voor de upgrade domeinen worden toegewezen. Niet is het mogelijk om op te geven welke exemplaren die zijn toegewezen aan welke domein.

## <a name="next-steps"></a>Volgende stappen
[Het beheren van Cloudservices](cloud-services-how-to-manage.md)  
[Het controleren van Cloudservices](cloud-services-how-to-monitor.md)  
[Het configureren van Cloudservices](cloud-services-how-to-configure.md)  
