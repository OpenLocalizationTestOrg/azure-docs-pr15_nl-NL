<properties
   pageTitle="Een verhelpen OS problemen in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **verhelpen OS problemen**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Een OS problemen in Azure Beveiligingscentrum verhelpen

Azure Beveiligingscentrum analyseert dagelijks uw besturingssysteem VM (VM) (OS) voor configuraties die kunnen de VM toegankelijk maken voor aanvallen en wordt aanbevolen configuratiewijzigingen van deze oplost. Raadpleeg de [lijst met regels aanbevolen configuratie](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)voor meer informatie over de specifieke configuraties wordt gecontroleerd. Beveiligingscentrum wordt aanbevolen dat u problemen oplost wanneer de configuratie van het besturingssysteem van uw VM niet overeenkomen met de aanbevolen configuratieregels.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **verhelpen OS problemen**.
![Een OS problemen verhelpen][1]

    Het blad **problemen verhelpen OS** wordt geopend en uw VMs met OS configuraties die niet overeenkomen met de aanbevolen configuratieregels bevat.  Voor elke VM, is het blad wordt geïdentificeerd:

   - **Regels is mislukt** --het aantal regels die van de VM OS configuratie is mislukt.
   - **Laatste SCANNEN tijd** --de datum en tijd dat Beveiligingscentrum laatst configuratie van het besturingssysteem van de VM gescand.
   - **De staat** --de huidige status van het probleem:

        - Open: De beveiligingsprobleem niet is beantwoord nog
        - Voortgang van uw: De beveiligingsprobleem momenteel wordt toegepast, geen actie is vereist door u
        - Opgelost: De beveiligingsprobleem is al opgenomen (wanneer het probleem opgelost is, het fragment grijs weergegeven)
  - **ERNST** --alle problemen zijn ingesteld op een ernst van laag, wat betekent dat een beveiligingsprobleem moet worden behandeld, maar geen onmiddellijke aandacht vereist.

Selecteer een VM. Een blade voor die VM geopend en de regels die niet zijn weergegeven.
   ![Van configuratieregels die niet zijn][2]

Selecteer een regel. In dit voorbeeld kunt Selecteer **wachtwoorden moeten voldoen aan vereisten voor complexiteit**. Een blade wordt geopend met een beschrijving van de regel is mislukt en de invloed. Controleer de details en houd rekening met hoe besturingssysteemconfiguraties wordt toegepast.
  ![Beschrijving voor de regel is mislukt][3]

  Beveiligingscentrum wordt algemene configuratie inventarisatie (CCE) gebruikt om unieke id's voor configuratieregels. De volgende gegevens worden weergegeven op deze blade:

  - NAAM--Naam van de regel
  - ERNST--CCE ernst waarde van kritieke, belangrijke of waarschuwing
  - CCIED--CCE unieke id voor de regel
  - Beschrijving--Beschrijving van regel
  - BEVEILIGINGSPROBLEEM--Uitleg van beveiligingsprobleem of risico als regel niet toegepast wordt.
  - IMPACT--Bedrijfsresultaten wanneer regel wordt toegepast
  - VERWACHTE waarde--Waarde verwacht wanneer Beveiligingscentrum uw configuratie VM OS ten opzichte van de regel analyseert
  - REGEL-bewerking--Regel bewerking gebruikt door Beveiligingscentrum tijdens een analyse van uw configuratie VM OS ten opzichte van de regel
  - WERKELIJKE waarde--Waarde geretourneerd na een analyse van de configuratie VM OS ten opzichte van de regel
  - RESULTAAT van de evaluatie –-resultaat van analyse: doorgeeft, is mislukt


## <a name="see-also"></a>Zie ook

In dit artikel beschreven wat u het implementeren van de aanbeveling Beveiligingscentrum "Remediate OS problemen." U kunt de set configuratieregels bekijken [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Beveiligingscentrum wordt CCE (algemene configuratie opsomming) gebruikt om unieke id's voor configuratieregels. Ga naar de site [CCE](http://cce.mitre.org) voor meer informatie.

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --zoeken blog over Azure beveiliging en naleving in berichten.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
