<properties
   pageTitle="Aan de slag maken van een interne taakverdeling in resourcemanager met behulp van de Azure portal | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling in resourcemanager met behulp van de Azure portal"
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
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Een interne taakverdeling maken in de portal van Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Een interne taakverdeling met behulp van Azure portal maken aan de slag

Gebruik de volgende stappen uit om te maken van een interne taakverdeling van de Azure-Portal.

1. Open een browser, Ga naar de [Azure-portal](http://portal.azure.com)en meld u aan met uw Azure-account.
2. Klik in de bovenste linkerzijde van het scherm, op **Nieuw** > **netwerkproblemen** > **taakverdeling**.
3. Voer in het blad **de belasting voor documentconversies maken** een **naam** voor uw taakverdeling.
4. Klik onder **kleurenschema**, klikt u op **interne**.
5. Klik op **virtuele netwerk**en selecteer vervolgens het virtuele netwerk waar u wilt maken van de taakverdeling.

    >[AZURE.NOTE] Als u het virtuele netwerk dat u wilt gebruiken niet ziet, controleert u de **locatie** u gebruikt voor de taakverdeling en overeenkomstig gewijzigd.

6. Klik op **Subnet**, en selecteer vervolgens het subnet waar u de taakverdeling maken.
7. Klik onder **IP-adrestoewijzing**, **dynamische** of **statische**, afhankelijk van of u wilt dat het IP-adres voor de verdeling van de belasting vast (statisch) of niet.

    >[AZURE.NOTE] Als u een statische IP-adres gebruiken selecteert, wordt er een adres voor de taakverdeling opgeeft.

8. Onder **resourcegroep** Geef de naam van een nieuwe resourcegroep voor de verdeling van de belasting, of klik op **bestaande selecteren** en selecteer een bestaande resourcegroep.
9. Klik op **maken**.

## <a name="configure-load-balancing-rules"></a>Regels van taakverdeling configureren

Navigeer naar de resource laden de verdeling van het configureren na het maken van de verdeling van laden.
Moet u eerst een back-enddatabase adres-toepassingen en configureren een test voordat u een regel van taakverdeling configureert.

### <a name="step-1-configure-a-back-end-pool"></a>Stap 1: Een back-enddatabase van toepassingen configureren

1. Klik op **Bladeren**in de portal Azure > **netwerktaakverdelers**, en klik vervolgens op de verdeling van de belasting die u hierboven hebt gemaakt.
2. Klik in het blad **Instellingen** op **back-end-toepassingen**.
3. Klik in het blad **Backend-mailadres van toepassingen** op **toevoegen**.
4. In het blad **toevoegen backend-toepassingen** , voert u een **naam** voor de backend-toepassingen en klik vervolgens op **OK**.

### <a name="step-2-configure-a-probe"></a>Stap 2: Een test configureren

1. Klik op **Bladeren**in de portal Azure > **netwerktaakverdelers**, en klik vervolgens op de verdeling van de belasting die u hierboven hebt gemaakt.
2. Klik in het blad **Instellingen** op **controleert**.
3. Klik in het blad **controleert** op **toevoegen**.
4. Voer in het blad **test toevoegen** een **naam** voor de test.
5. Selecteer onder **Protocol**, **HTTP** (voor websites) of **TCP** (voor andere toepassingen TCP gebaseerd).
6. Selecteer bij **poort**de poort gebruiken bij het openen van de test te opgeven.
7. Geef onder **pad** (voor HTTP sondes), het pad wilt gebruiken als een test.
8. Geef onder **Interval** hoe vaak u de toepassing te zoeken.
9. Geef het aantal pogingen moeten mislukt voordat de backend virtuele machine is gemarkeerd als beschadigd onder **beschadigd drempelwaarde**.
10. Klik op **OK** als u wilt maken van de test.

### <a name="step-3-configure-load-balancing-rules"></a>Stap 3: Regels van taakverdeling configureren

1. Klik op **Bladeren**in de portal Azure > **netwerktaakverdelers**, en klik vervolgens op de verdeling van de belasting die u hierboven hebt gemaakt.
2. Klik in het blad **Instellingen** op **regels van taakverdeling**.
3. Klik in het blad **taakverdeling regels** op **toevoegen**.
4. Voer in het blad **toevoegen laden taakverdeling regel** een **naam** voor de regel.
5. Selecteer onder **Protocol**, **HTTP** (voor websites) of **TCP** (voor andere toepassingen TCP gebaseerd).
6. Geef dat de poort-clients verbinding maakt met in de taakverdeling onder **poort**.
7. Geef onder **Backend poort**de poort moet worden gebruikt in de groep backend (meestal de poort van de verdeling van laden en de backend-poort zijn hetzelfde).
8. Selecteer onder **Backend-toepassingen**, de backend-toepassingen die u hierboven hebt gemaakt.
9. Selecteer onder **sessie permanente**, hoe u sessies om te behouden.
10. Geef onder **inactiviteit (minuten)**, inactiviteit.
11. Onder **Zwevende IP (directe server afzender)**, klikt u op **uitgeschakeld** of **ingeschakeld**.
12. Klik op **OK**.

## <a name="next-steps"></a>Volgende stappen

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)