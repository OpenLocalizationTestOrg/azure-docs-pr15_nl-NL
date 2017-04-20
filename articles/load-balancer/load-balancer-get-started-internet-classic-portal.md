
<properties
   pageTitle="Aan de slag voor het maken van een internetverbinding van taakverdeling in klassieke implementatiemodel met behulp van de Azure klassieke portal | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding van taakverdeling in klassieke implementatiemodel met behulp van de Azure klassieke portal"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Maken van een internetverbinding van taakverdeling (klassieke) in de portal van Azure klassieke aan de slag

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [informatie over het maken van taakverdeling met Azure Resource Manager van een internetverbinding](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Een taakverdeling internetgerichte voor virtuele machines instellen

Als u wilt laden balance '-netwerkverkeer van Internet in de virtuele machines van een cloudservice, moet u een set verdeeld. Deze procedure wordt ervan uitgegaan dat u de virtuele machines al hebt gemaakt en dat ze afkomstig alle binnen de dezelfde cloudservice zijn.

**Voor het configureren van een set taakverdeling voor virtuele machines**

1. Klik in de klassieke portal Azure **virtuele Machines**op en klik vervolgens op de naam van een virtuele machine in de set verdeeld.

2. Klik op de **eindpunten**en klik vervolgens op **toevoegen**.

3. Klik op de pagina **toevoegen een eindpunt met een virtuele machine** op de pijl-rechts.

4. Klik op de pagina **de details van het eindpunt opgeven** :

    * In het vak **naam**een naam voor het eindpunt Typ of Selecteer de naam in de lijst met vooraf gedefinieerde eindpunten voor algemene protocollen.
    * Selecteer in het vak **Protocol**het protocol dat is vereist door het type eindpunt, TCP of UDP, indien nodig.
    * Typ in het **openbare poort en privé poort**, de poortnummers die u de virtuele machines gebruik, wilt indien nodig. U kunt de privé poorten en firewallregels op de virtuele machine verkeer op een manier die geschikt is voor uw toepassing omleiden. De privé poort is hetzelfde als de openbare poort. Bijvoorbeeld voor een eindpunt voor web (HTTP)-verkeer is toegestaan, kunt u aan de openbare en persoonlijke poort poort 80 toewijzen.

5. Selecteer **een groep taakverdeling maken**en klik vervolgens op de pijl-rechts.

6. Klik op de pagina **configureren de set taakverdeling** Typ een naam voor het instellen van taakverdeling en vervolgens de waarden voor test gedrag van taakverdeling Azure toewijzen. De taakverdeling gebruikt sondes om te bepalen als de virtuele machines in de set taakverdeling beschikbaar zijn voor het ontvangen van binnenkomende verkeer.

7. Klik op het vinkje om te maken van het eindpunt verdeeld. Ziet u **Ja** in de kolom **naam van taakverdeling instellen** van de pagina voor de virtuele machine- **eindpunten** .

8. Klik in de portal klikt u op **virtuele Machines**, klik op de naam van een extra virtuele machine in de set verdeeld, klikt u op **eindpunten**en klik vervolgens op **toevoegen**.

9. Klik op de pagina **toevoegen een eindpunt met een virtuele machine** op **eindpunt toevoegen aan een set verdeeld**, selecteert u de naam van de set taakverdeling en klik vervolgens op de pijl-rechts.

10. Klik op de pagina **Geef de details van het eindpunt** Typ een naam voor het eindpunt en klik vervolgens op het vinkje.

Herhaal stap 8-10 voor de extra virtuele machines in de set verdeeld.



## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

