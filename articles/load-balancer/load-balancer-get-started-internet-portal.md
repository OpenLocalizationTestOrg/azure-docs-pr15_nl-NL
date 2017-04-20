<properties
   pageTitle="Maken van een taakverdeling internetgerichte in resourcemanager met behulp van de Azure portal | Microsoft Azure"
   description="Informatie over het maken van een verdeling van de belasting internetgerichte in resourcemanager met behulp van de Azure portal"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Een internetgerichte taakverdeling met behulp van de Azure portal maken

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel worden de resourcemanager implementatiemodel. U kunt ook [informatie over het maken van een internetgerichte taakverdeling klassieke implementatie gebruiken](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Dit behandelt de volgorde van individuele taken worden uitgevoerd moet voor het maken van een taakverdeling en uitleg in detail de werkzaamheden hiertoe het doel.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Wat is vereist voor het maken van een verdeling van de belasting internetgerichte?

U moet maken en configureren van de volgende objecten als u wilt een taakverdeling implementeren.

- Front IP-configuratie - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.

- Groep met back-enddatabase adressen - bevat netwerkinterfaces (NIC's) voor de virtuele machines moet ontvangen netwerkverkeer van de taakverdeling.

- Regels van taakverdeling - een openbare poort op de taakverdeling toewijzen aan poort in de groep back-enddatabase adressen regels bevat.

- Binnenkomende verbindingen NAT - een openbare poort op de taakverdeling toewijzen aan een poort voor een specifieke virtuele machine in de groep back-enddatabase adressen regels bevat.

- Controleert of-status sondes gebruikt om te controleren van de beschikbaarheid van virtuele machines exemplaren in de groep back-enddatabase adressen bevat.

U gaat meer informatie over het laden de verdeling van onderdelen met Azure resourcemanager [Azure resourcemanager ondersteuning voor taakverdeling](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Een taakverdeling in Azure-portal instellen

> [AZURE.IMPORTANT] In dit voorbeeld wordt ervan uitgegaan dat u hebt een virtueel netwerk **myVNet**genoemd. Raadpleeg [virtuele netwerk maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) u dit wilt doen. Deze ook ervan uitgaande dat er is een subnet binnen **myVNet** genoemd **Kg Subnet worden** en twee VMs genaamd **web1** en **web2** respectievelijk binnen dezelfde beschikbaarheid set **myAvailSet** in **myVNet**genoemd. Raadpleeg [deze koppeling](../virtual-machines/virtual-machines-windows-hero-tutorial.md) VMs maken.


1. Navigeren in een browser naar de Azure-portal: [http://portal.azure.com](http://portal.azure.com) en meld u aan met uw Azure-account.

2. Selecteer **Nieuw**op de bovenste linkerkant van het scherm > **netwerkproblemen** > **taakverdeling.**

3. Typ een naam voor uw taakverdeling in het blad **maken de belasting voor documentconversies** . Hier wordt genoemd **myLoadBalancer**.

4. Klik onder **Type**selecteert u **openbare**.

5. Maak een nieuwe openbare IP-adres genoemd **myPublicIP**onder **openbare IP-adres**.

6. Selecteer onder resourcegroep, **myRG**. Selecteer vervolgens de juiste **locatie**en klik vervolgens op **OK**. Verdeling van de belasting wordt wordt geopend om te implementeren en een paar minuten te voltooien op implementatie.

![Resourcegroep van taakverdeling bijwerken](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Een back-enddatabase adresgroep maken

1. Nadat uw taakverdeling heeft geïmplementeerd, selecteert u deze uit binnen uw resources. Schakel onder instellingen Backend-toepassingen. Typ een naam voor uw back-end-toepassingen. Klik op de knop **toevoegen** naar de bovenkant van het blad dat wordt weergegeven.

2. Klik op **een virtuele machine toevoegen** in het blad **toevoegen backend-toepassingen** .  Selecteer **een set beschikbaarheid kiezen** onder **beschikbaarheid instellen** en selecteer **myAvailSet**. Vervolgens selecteert u **de virtuele machines Kies** onder de sectie virtuele Machines in het blad en klik op **web1** en **web2**, de twee VMs voor taakverdeling gemaakt. Zorg ervoor dat beide blauwe vinkjes links hebben, zoals wordt weergegeven in de onderstaande afbeelding. Klik vervolgens op **selecteren** in die blade gevolgd door OK in het blad **Kies virtuele machines** en vervolgens op **OK** in het blad **backend-groep toevoegen** .

    ![Toevoegen aan de groep backend adressen- ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Controleer of de meldingen vervolgkeuzelijst heeft een update met betrekking tot de toepassingen laden verdeling backend naast het bijwerken van de netwerkinterface voor zowel het VMs **web1** en de **web2**op te slaan.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Een test, kg regel en NAT regels maken

1. Maak een systeemstatus-test.

    Schakel onder instellingen van uw taakverdeling sondes. Klik vervolgens op **toevoegen** aan de bovenkant van het blad bevinden.

    Er zijn twee manieren voor het configureren van een test: HTTP of TCP. In dit voorbeeld ziet u HTTP, maar TCP kan worden geconfigureerd in een soortgelijke manier.
    De benodigde informatie bijwerkt. Zoals vermeld, wordt **myLoadBalancer** balance '-verkeer op poort 80 geladen. Het geselecteerde pad is HealthProbe.aspx, Interval is 15 seconden en beschadigd drempelwaarde is 2. Wanneer u klaar bent, klikt u op **OK** om te maken van de test.

    De muisaanwijzer op de 'i' pictogram voor meer informatie over deze afzonderlijke configuraties en hoe ze kunnen worden gewijzigd om geschikt zijn voor uw vereisten.

    ![Een test toevoegen](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Maak een regel van de verdeling van laden.

    Klik op regels in de sectie instellingen van uw taakverdeling van taakverdeling. Klik in het nieuwe blad, op **toevoegen**. De regel een naam. Hier is HTTP. Kies de frontend poort en Backend poort. Hier is 80 gekozen voor beide. Kies **kg backend** als uw back-end-groep en de eerder gemaakte **HealthProbe** als de test. Andere configuraties kunnen worden ingesteld op basis van uw vereisten. Klik vervolgens op OK als u wilt opslaan van de regel van taakverdeling.

    ![Een regel van taakverdeling toevoegen](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Binnenkomende NAT-regels maken

    Klik op regels voor binnenkomende verbindingen NAT onder de sectie instellingen van uw taakverdeling. Klik in het nieuwe blad die, klikt u op **toevoegen**. Geef een naam op uw binnenkomende NAT-regel. Hier wordt genoemd **inboundNATrule1**. De bestemming moet het openbare IP eerder hebt gemaakt. Selecteer Custom onder Service en selecteer het protocol dat u wilt gebruiken. Hier is TCP geselecteerd. Voer de poort 3441, en de doelpoort, in dit geval 3389. Klik vervolgens op OK om op te slaan met deze regel.

    Nadat de eerste regel is gemaakt, herhaalt u deze stap voor het tweede binnenkomende NAT regel inboundNATrule2 aangeroepen vanuit poort 3442 doel poort 3389.

    ![Een binnenkomende NAT-regel toevoegen](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Een taakverdeling verwijderen

Als een taakverdeling verwijderen, selecteert u de verdeling van de belasting die u wilt verwijderen. Klik in het blad *Taakverdeling* op **verwijderen** zich aan de bovenkant van het blad. Selecteer vervolgens **Ja** wanneer u wordt gevraagd.

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-cli.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
