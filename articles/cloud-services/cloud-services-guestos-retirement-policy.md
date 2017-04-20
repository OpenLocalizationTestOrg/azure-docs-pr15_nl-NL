<properties 
   pageTitle="Mogelijkheden voor ondersteuning en buitengebruikstelling beleid gids voor Azure Gast OS | Microsoft Azure" 
   description="Hier vindt u informatie over wat Microsoft ondersteuning met betrekking tot aan het Azure Gast OS door Cloud Services gebruikt." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Gast OS-ondersteuning en buitengebruikstelling van beleid
De gegevens op deze pagina betrekking op het besturingssysteem van Azure gast ([Gast OS](cloud-services-guestos-update-matrix.md)) voor de werknemer Cloudservices en web rollen (PaaS). Dit geldt niet voor virtuele Machines (IaaS). 

Microsoft heeft een gepubliceerde [ondersteuningsbeleid voor het besturingssysteem Gast](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). De pagina die u aan het lezen zijn nu wordt beschreven hoe het beleid wordt geïmplementeerd.

Het beleid is 

1. Microsoft biedt ondersteuning voor **ten minste de meest recente twee gezinnen van het besturingssysteem Gast**. Wanneer een huishouden is teruggetrokken, hebben klanten 12 maanden na de datum officiële pensioen bijwerken naar een nieuwere ondersteunde Gast OS familie.
2. Microsoft biedt ondersteuning voor de **ten minste de nieuwste twee versies van de ondersteunde Gast OS gezinnen**. 
3. Microsoft biedt ondersteuning voor de bij **minimaal de nieuwste twee versies van de Azure-SDK**. Wanneer u een versie van de SDK wordt beëindigd en moeten klanten met 12 maanden na de datum officiële pensioen bijwerken naar een nieuwere versie. 

Soms meer dan twee gezinnen of releases mogelijk worden ondersteund. Officiële Gast OS ondersteuningsinformatie wordt weergegeven op de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Als een gast OS familie of de versie is buiten gebruik gesteld 


Een nieuwe Gast OS **familie** is geïntroduceerd ergens na de release van een nieuwe officiële versie van het besturingssysteem Windows Server. Wanneer u een nieuwe Gast OS familie wordt toegevoegd, wordt de oudste Gast OS familie door Microsoft intrekken. 

Nieuwe Gast OS **versies** worden geïntroduceerd over elke maand kunt u de meest recente updates voor MSRC. Vanwege de normale maandelijkse updates is een versie Gast OS normaal uitgeschakelde 60 dagen na de release. Hiermee ten minste twee Gast OS versies voor iedere familie beschikbaar maken voor gebruik. 

### <a name="process-during-a-guest-os-family-retirement"></a>Proces tijdens een gast OS family buitengebruikstelling 


Zodra de buitengebruikstelling wordt aangekondigd, hebben klanten een overgangsperiode 12 maanden '' voordat de oudere familie officieel van service wordt verwijderd. Deze overgangstijd kan worden verlengd naar keuze van Microsoft. Updates worden geboekt op het [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).

Een proces geleidelijke buitengebruikstelling begint 6 maanden in de overgangsperiode. Tijdens deze periode:

1. Microsoft melding klanten van de buitengebruikstelling. 
2. De nieuwere versie van de Azure-SDK de teruggetrokken Gast OS familie niet wordt ondersteund.
3. Nieuwe implementaties en nieuwe distributies van Cloudservices is niet toegestaan op de teruggetrokken familie

Microsoft blijft introduceren nieuwe Gast OS versie bevat de meest recente updates MSRC tot de laatste dag van de overgangsperiode, bekend als de 'vervaldatum'. Op dat moment wordt het eventuele Cloudservices nog actief onder de SLA Azure worden ondersteund. Microsoft heeft de keuze upgrade afdwingen, verwijderen of stoppen van deze services na deze datum.



### <a name="process-during-a-guest-os-version-retirement"></a>Proces tijdens een buitengebruikstelling Gast OS versie 
Als klanten ingesteld hun OS Gast automatisch moeten worden bijgewerkt, worden ze nooit zelf hoeft te bang omgaan met gast OS versies. Ze altijd gebruikt de meest recente versie van de Gast OS.

Gast OS versies worden elke maand uitgebracht. Vanwege de snelheid van de normale versies heeft elke versie een vaste levensduur.

Bij 60 dagen in de levensduur van een versie is het "*uitgeschakeld*". "Uitgeschakeld" betekent dat de versie is verwijderd uit de Azure klassieke portal. Ook kan niet meer worden ingesteld in het configuratiebestand CSCFG. Bestaande implementaties's actief blijven, maar nieuwe implementaties en code- en configuratieservices updates met bestaande installaties is niet toegestaan. 

Op een later tijdstip, de Gast OS versie '*verloopt*' en eventuele installaties zijn nog actief die versie dwingen bijgewerkt en instellen op automatisch in de toekomst bijwerken de OS Gast. Verlooptijd is gedaan in batches zodat de periode tussen deactivering naar verlooptijd kan variëren. 

Deze perioden kunnen plaatsvinden langer tijdens de keuze van Microsoft om het te vergemakkelijken klant overgangen. Wijzigingen worden worden meegedeeld de [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).



### <a name="notifications-during-retirement"></a>Meldingen tijdens buitengebruikstelling 

* **Family buitengebruikstelling** <br>Microsoft gebruikt blogberichten en Azure klassieke melding bij de portal. Klanten die nog steeds een teruggetrokken Gast OS familie krijgt via rechtstreekse communicatie (e-mail, portal berichten, telefoongesprek) met toegewezen servicebeheerders. Alle wijzigingen worden in deze pagina wordt geplaatst en de RSS-feed aan het begin van deze pagina wordt vermeld. 


* **Versie buitengebruikstelling** <br>Alle wijzigingen worden in deze pagina wordt geplaatst en de RSS-feed weergegeven aan het begin van deze pagina, inclusief de release, uitgeschakeld en de vervaldatum. Beheerders van de services, e-mailberichten ontvangt als ze implementaties uitvoeren op een uitgeschakelde Gast OS versie of familie hebben. De timing van deze e-mailberichten kan variëren. Ze zijn in het algemeen ten minste een maand vóór deactivering, hoewel dit tijdsinstellingen niet een officiële SLA is. 


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Hoe kan ik in het risico van de gevolgen van het migratie?**

U moet de nieuwste Gast OS familie gebruiken voor het ontwerpen van uw Cloudservices. 

1. Begin met het plannen van de migratie naar een nieuwere familie vroeg. 
2. Tijdelijke test implementaties instellen voor het testen van uw Cloudservice die wordt uitgevoerd op de nieuwe familie. 
3. Uw gast OS-versie instellen op **automatisch** (Versie_besturing = * in het bestand [.cscfg](cloud-services-model-and-package.md#cscfg) ) zodat de migratie naar nieuwe Gast OS versies automatisch plaatsvindt.

**Wat gebeurt er als mijn webtoepassing vereist voor betere integratie met het besturingssysteem?**

Als uw web-toepassingsarchitectuur grondigere afhankelijkheid van het onderliggende besturingssysteem vereist, ondersteund gebruik platform mogelijkheden zoals [opstarttaken](cloud-services-startup-tasks.md) of andere uitbreiden regelingen die mogelijk in de toekomst bestaan. U kunt ook kunt u ook [Azure virtuele Machines](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – infrastructuur als een Service), waar u verantwoordelijk bent voor het onderhouden van het onderliggende besturingssysteem gebruiken.
 
## <a name="next-steps"></a>Volgende stappen
Bekijk de meest recente [Gast OS loslaat](cloud-services-guestos-update-matrix.md).
