<properties
   pageTitle="Beperken van toegang tot en met internetgerichte eindpunten in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **beperken toegang via Internet omlaag eindpunt**."
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

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Beperken van toegang tot en met internetgerichte eindpunten in Azure Beveiligingscentrum

Azure Beveiligingscentrum wordt aangeraden dat u toegang tot en met internetgerichte eindpunten beperken als een van uw netwerk-beveiligingsgroepen (NSGs) heeft een of meer regels voor binnenkomende verbindingen die toegang van 'een' IP-adres toestaan. Toegang tot de 'een' openen, kan indringers voor toegang tot uw resources inschakelen. Beveiligingscentrum wordt aangeraden dat u deze binnenkomende regels voor het beperken van toegang aan bron IP-adressen die wel behoefte aan access bewerken.

Deze aanbeveling wordt gegenereerd voor elke poort niet op het web met 'een' als bron.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie. Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in de **aanbevelingen blade**, **beperkt toegang via Internet omlaag eindpunt**.
![Toegang via Internet die tegenover elkaar liggen eindpunt beperken][1]

2. Hiermee opent u het blad **beperken toegang via Internet omlaag eindpunt**. Deze blade lijsten de virtuele machines (VMs) met regels voor binnenkomende verbindingen die een potentieel beveiligingsprobleem maakt. Selecteer een VM.
![Selecteer een VM][2]

3. Het blad **NSG** geeft informatie, gerelateerde regels voor binnenkomende verbindingen en de bijbehorende VM netwerk beveiligingsgroep. Selecteer **bewerken van regels voor binnenkomende verbindingen** om te beginnen met het bewerken van een binnenkomende regel.
![Beveiligingsgroep netwerk blade][3]

4. Selecteer op het blad **inkomende beveiligingsregels** de binnenkomende regel bewerken. Selecteer in dit voorbeeld laten we **AllowWeb**.
![Regels voor binnenkomende][4]

  Opmerking u kunt ook **standaardregels** om te zien van de set standaard voorschriften door alle NSGs selecteren. De standaardregels kunnen niet worden verwijderd, maar, omdat deze een lagere prioriteit zijn toegewezen, kunnen ze worden overschreven door de regels die u maakt. Meer informatie over [standaardregels voor het](../virtual-network/virtual-networks-nsg.md#default-rules).
![Standaardregels][5]

5. Klik op het blad **AllowWeb** door de eigenschappen van de binnenkomende regel te bewerken, zodat de **bron** een IP-adres of blok met IP-adressen is. Zie meer informatie over de eigenschappen van de binnenkomende regel [NSG regels](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Binnenkomende regel bewerken][6]

## <a name="see-also"></a>Zie ook

In dit artikel beschreven wat u het implementeren van de aanbeveling Beveiligingscentrum "Beperken toegang via Internet omlaag eindpunt." Zie de volgende onderwerpen voor meer informatie over het inschakelen van NSGs en regels:

- [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Het beheren van NSGs met behulp van de Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md)--meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren](security-center-recommendations.md)--Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md)--informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingsmeldingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md)--meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md)--zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/)--het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
