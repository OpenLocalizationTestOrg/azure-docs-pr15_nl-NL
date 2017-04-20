<properties
   pageTitle="Het toevoegen van een volgende generatie firewall in Beveiligingscentrum Azure | Microsoft Azure"
   description="In dit document ziet u hoe u de aanbevelingen Azure Beveiligingscentrum **toevoegen een volgende generatie Firewall** en **Route traffice tot en met alleen NGFW**implementeren."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Een volgende generatie Firewall in Beveiligingscentrum Azure toevoegen

Azure Beveiligingscentrum mogelijk raadzaam een volgende generatie firewall (NGFW) uit een Microsoft-partner toe te voegen om te kunnen vergroten uw beveiligingsinstellingen. In dit document begeleidt u door een voorbeeld van hoe u dit doet.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **toevoegen een volgende generatie-Firewall**.
![Een volgende generatie Firewall toevoegen][1]

2. Selecteer in het blad **toevoegen een volgende generatie Firewall** een eindpunt.
![Selecteer een eindpunt][2]

3. Een tweede **toevoegen een volgende generatie Firewall** blade wordt geopend. U kunt een bestaande oplossing gebruiken als deze functie beschikbaar of u een nieuwe record kunt maken. In dit voorbeeld zijn er geen bestaande oplossingen beschikbaar zodat we een nieuwe NGFW wilt maken.
![Nieuwe volgende generatie Firewall maken][3]

4. Als u wilt een nieuwe NGFW maken, selecteert u een oplossing in de lijst met geïntegreerde partners. In dit voorbeeld wordt we **Selectievakje punt**selecteren.
![Selecteer volgende generatie Firewall-oplossing][4]

5. Het **Selectievakje punt** blad geopend zodat u informatie over de partneroplossing. Selecteer **maken** in het blad informatie.
![Firewall informatie blade][5]

6. Hiermee opent u het blad **VM maken** . Hier kunt u informatie nodig is om te draaien van een virtuele machine (VM) die de NGFW kan worden uitgevoerd. Volg de stappen en geef de vereiste NGFW-gegevens. Selecteer OK om toe te passen.
![VM om uit te voeren NGFW maken][6]

## <a name="route-traffic-through-ngfw-only"></a>Route verkeer via NGFW alleen

Ga terug naar het blad **aanbevelingen** . Een nieuwe vermelding is gegenereerd nadat u een NGFW via Beveiligingscentrum toegevoegd **Route-verkeer via NGFW alleen**genoemd. Deze aanbeveling wordt gemaakt alleen als u uw NGFW tot en met Beveiligingscentrum hebt geïnstalleerd. Als u de eindpunten internetverbinding hebt, wordt er Beveiligingscentrum aangeraden netwerk beveiligingsgroep regels die afdwingen van binnenkomende verkeer naar uw VM via uw NGFW te configureren.

1. Selecteer in de **aanbevelingen blade**, **Route-verkeer via NGFW alleen**.
![Route verkeer via NGFW alleen][7]

2. Hiermee wordt het **verkeer via NGFW alleen routeren** waarin VMs die u kunt verkeer naar routeert blad geopend. Selecteer een VM in de lijst.
![Selecteer een VM][8]

3. Een blade voor de geselecteerde VM wordt geopend, met gerelateerde regels voor binnenkomende verbindingen. Een beschrijving biedt u meer informatie over de volgende stappen. Selecteer **bewerken van regels voor binnenkomende verbindingen** om te beginnen met het bewerken van een binnenkomende regel. De verwachting is dat **bron** niet is ingesteld op **alle** voor de eindpunten van de internetgerichte die zijn gekoppeld aan de NGFW. Zie meer informatie over de eigenschappen van de binnenkomende regel [NSG regels](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Regels voor het beperken van toegang configureren][9]
![binnenkomende regel bewerken][10]

## <a name="see-also"></a>Zie ook

In dit document hebt u geleerd hoe u het implementeren van de aanbeveling Beveiligingscentrum "Toevoegen een volgende generatie-Firewall." Zie de volgende onderwerpen voor meer informatie over NGFWs en de oplossing Check Point-partner:

- [Allernieuwste Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Punt vSEC controleren](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --zoeken blog over Azure beveiliging en naleving in berichten.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
