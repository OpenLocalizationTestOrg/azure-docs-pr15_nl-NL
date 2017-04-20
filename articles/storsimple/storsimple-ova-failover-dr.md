<properties
   pageTitle="Noodgevallen herstel en apparaat failover voor uw virtuele StorSimple-matrix"
   description="Meer informatie over het foutherstel voor uw StorSimple virtuele matrix."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Noodgevallen herstel en apparaat failover voor uw virtuele StorSimple-matrix


## <a name="overview"></a>Overzicht

In dit artikel worden de herstel voor uw Microsoft Azure StorSimple virtuele matrix (ook wel bekend als het StorSimple on-premises implementatie virtuele apparaat), met inbegrip van de vereiste mislukt boven aan een ander virtuele apparaat een storing gedetailleerde stappen. Een failover kunt u uw gegevens uit een *bron* -apparaat in het datacenter migreren naar *een ander apparaat zich bevindt in dezelfde of een andere geografische locatie* . De overname van het apparaat is voor het hele apparaat. Tijdens een overname verandert de cloud-gegevens voor het bronapparaat eigendom met die van het doelapparaat.

Apparaat failover is ingedeeld via de functie voor noodgevallen herstel (DR) en vanaf de pagina **apparaten** wordt gestart. Deze pagina registreert alle de StorSimple apparaten die zijn verbonden met uw service StorSimple Manager. Voor elk apparaat, worden de beschrijvende naam, status, ingerichte- en maximumwaarden capaciteit, type en model weergegeven.

![](./media/storsimple-ova-failover-dr/image15.png)

In dit artikel is alleen van toepassing op StorSimple virtuele matrices. Als u wilt niet via een 8000 reeks-apparaat, gaat u naar [Failover en herstel van uw apparaat StorSimple](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Wat is herstel?

In een herstellen (DR) van fouten, het primaire apparaat uitvalt. In dit geval kunt u de cloud-gegevens die zijn gekoppeld aan het apparaat naar een ander apparaat door het primaire apparaat als de *bron* en geven van een ander apparaat als *doel*verplaatsen. Dit proces wordt *failover*genoemd. Tijdens een overname, worden alle de hoeveelheden of de gedeelde gegevens uit de Bronapparaat eigendom wijzigen en worden overgedragen naar het doelapparaat. Geen filter van de gegevens is toegestaan.

DR wordt gezien als het herstellen van een volledige apparaat gebruikt de heatmap kaartgebaseerde – trapsgewijs schakelen en bijhouden. Een warmtekaart is gedefinieerd door het toewijzen van een waarde heatmap met de gegevens die zijn gebaseerd op lezen en schrijven patronen. Deze heatmap wijs vervolgens lagen het laagste heatmap gegevens delen in de cloud eerst terwijl het delen van de gegevens hoge temperaturen (meest gebruikt) in de lokale laag. Tijdens een DR, wordt de heatmap wilt herstellen en de gegevens vanuit de cloud rehydrate gebruikt. Het apparaat dat alle volumes/aandelen in de laatste recente back-up ophaalt (zoals bepaald intern) en voert een herstellen uit die back-up. Het hele DR-proces is door het apparaat dat ingedeeld.


## <a name="prerequisites-for-device-failover"></a>Vereisten voor apparaat failover


### <a name="prerequisites"></a>Vereisten voor

Voor elk apparaat failover, moeten de volgende vereisten worden voldaan:

- De Bronapparaat moet worden in een **gedeactiveerd** staat.

- Het doelapparaat moet weergegeven als **actief** in de portal van Azure klassieke. Moet u voor het inrichten van een virtueel doel-apparaat van de capaciteit dezelfde of hoger. Vervolgens moet u het lokale web UI gebruiken om te configureren en het virtuele apparaat goed hebt geregistreerd.

    > [AZURE.IMPORTANT] Probeer niet het geregistreerde virtuele apparaat via de service configureren door te klikken op **voltooid Apparaatinstelling**. Geen apparaat te configureren, moet worden uitgevoerd door de service.

- Het apparaat van de bronsite en doelsites moet hetzelfde type. U kunt alleen niet via een virtueel apparaat is geconfigureerd als een bestandsserver naar een ander bestandsserver. Dit geldt voor een iSCSI-server.

- Voor een bestandsserver DR, wordt u aangeraden het doelapparaat te koppelen aan hetzelfde domein als die van de bron zodat de sharemachtigingen automatisch opgelost worden. Alleen de overname op een apparaat in hetzelfde domein wordt ondersteund in deze release.

### <a name="other-considerations"></a>Andere relevante informatie

- Wij raden u aan dat u alle volumes of waarden voor aandelen op het bronapparaat offline nemen.

- Als dit een geplande overname is, raadzaam dat u een back-up van het apparaat en ga vervolgens verder met de overname te minimaliseren ten verlies van gegevens. Als dit een niet-geplande overname is, wordt de meest recente back-up gebruikt het apparaat te herstellen.

- De beschikbare apparaten voor DR zijn apparaten waarvoor de dezelfde of een grotere capaciteit vergeleken met het bronapparaat. De apparaten die zijn verbonden met uw service, maar niet voldoen aan de criteria voldoende ruimte is alleen beschikbaar als doelapparaten.

### <a name="dr-prechecks"></a>DR prechecks

Voordat de DR begint, worden prechecks worden uitgevoerd op het apparaat. Deze controles ervoor te zorgen dat er geen fouten plaatsvinden zal wanneer DR begint. De prechecks opnemen:

- Het account opslag valideren

- De cloud-connectiviteit met Azure controleren

- Beschikbare ruimte op het apparaat controleren

- Controleren of een iSCSI-server Bronapparaat geldige ACR namen heeft, IQN (niet meer dan 220 tekens lang), en CHAP wachtwoord (12 tot 16 tekens lang) die zijn gekoppeld aan de delen

Als een van de bovenstaande prechecks mislukt, kunt u de DR niet voortzetten. U moet deze problemen oplossen en probeer DR.

Nadat de DR is voltooid, wordt het eigendom van de cloud-gegevens op het bronapparaat overgebracht naar het doelapparaat. Het bronapparaat is vervolgens niet meer beschikbaar in de portal. Toegang tot alle volumes/aandelen op het bronapparaat is geblokkeerd en het doelapparaat wordt geactiveerd.

> [AZURE.IMPORTANT]
> 
> Hoewel het apparaat niet langer beschikbaar is, verbruikt de virtuele machine die u deze is ingericht op het hostsysteem nog steeds bronnen. Zodra de DR is voltooid is, kunt u deze virtuele machine verwijderen uit uw hostsysteem.

## <a name="fail-over-to-a-virtual-array"></a>Storing overgenomen door een virtueel matrix

Het is raadzaam dat er een andere StorSimple virtuele matrix deze is ingericht, geconfigureerd via het lokale web UI en geregistreerd bij de StorSimple Manager-service voordat u deze procedure wordt uitgevoerd.


> [AZURE.IMPORTANT]
> 
> - U bent niet mag worden uitgevoerd vanaf een apparaat van de reeks StorSimple 8000 naar een virtuele 1200-apparaat.
> - U kunt niet via vanaf een virtueel apparaat geïmplementeerd in voor de overheid-portal met een virtueel apparaat in Azure klassieke portal van federale Information Processing Standard (FIPS) ingeschakeld. Het omgekeerde geldt ook.

De volgende stappen als u wilt herstellen van het apparaat aan een virtuele apparaat StorSimple uitvoeren.

1. Volumes/aandelen offline nemen op de host. Raadpleeg de instructies op de host van de besturingssysteem-specifieke naar de volumes/aandelen offline te zetten. Als niet al offline, moet u alle volumes/aandelen offline nemen op het apparaat door te gaan naar **apparaten > waarden voor aandelen** (of **apparaat > Volumes**). Selecteer een delen/volume en klik op **offline halen** vanaf de onderkant van de pagina. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**. Herhaal deze procedure voor alle de waarden voor aandelen/hoeveelheden op het apparaat.

2. Selecteer het bronapparaat voor failover op de pagina **apparaten** en klik op **deactiveren**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. U wordt gevraagd om bevestiging. Apparaat deactiveren is een permanente proces dat niet ongedaan worden gemaakt. U wordt ook herinnerd moet worden ondernomen waarden voor aandelen/begint offline de host.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Na bevestiging, wordt het deactiveren gestart. Nadat de deactiveren is voltooid, wordt u gewaarschuwd.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Klik op de pagina **apparaten** worden de status van het apparaat wordt nu gewijzigd in **gedeactiveerd**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Selecteer het apparaat dat gedeactiveerd en onderaan op de pagina, klikt u op **Failover**.

6. Ga als volgt te werk in de wizard van de pagina bevestig failover dat wordt geopend:

    1. In de vervolgkeuzelijst van beschikbare apparaten, kiest u een **apparaat.** Alleen de apparaten die voldoende capaciteit worden weergegeven in de vervolgkeuzelijst worden weergegeven.

    2. Bekijk de details die is gekoppeld aan het bronapparaat zoals de apparaatnaam van het, totale capaciteit en de namen van de waarden voor aandelen die via mislukt.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Controleer **ik mee eens dat niet ongedaan maken failover en zodra de overname is voltooid, het bronapparaat wordt verwijderd**.

8. Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-failover-dr/image1.png).


9. Een taak failover wordt gestart en u ontvangt een melding. Klik op **taak weergeven** om de overname de te houden.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. **Taken** op de pagina ziet u een taak failover is gemaakt voor het bronapparaat. Deze taak kunt u de DR-prechecks uitvoeren.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Nadat de DR-prechecks geslaagd is, wordt de taak failover brengen herstellen taken voor elk delen/volume die zich op uw Bronapparaat gang.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Nadat de overname is voltooid, gaat u naar de pagina **apparaten** .

    een. Selecteer het StorSimple virtuele apparaat dat is gebruikt als het apparaat voor het failoverproces.

    b. Ga naar pagina van de **waarden voor aandelen** (of **Volumes** als iSCSI-server). Alle aandelen (volumes) van het oude apparaat moeten nu worden vermeld.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Video beschikbaar**

Deze video laat zien hoe u kunt niet via een virtueel StorSimple on-premises implementatie-apparaat aan een ander virtuele apparaat.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Zakelijke bedrijfscontinuïteit herstel (BCDR)

Een zakelijke bedrijfscontinuïteit herstellen (BCDR) van fouten wordt weergegeven wanneer het hele Azure datacenter uitvalt. Dit invloed kan zijn op uw StorSimple Manager-service en de bijbehorende StorSimple-apparaten.

Als er StorSimple apparaten die zijn geregistreerd voordat een noodgevallen zijn aangebracht, klikt u vervolgens deze StorSimple-apparaten moet mogelijk worden verwijderd. Na het noodgevallen, kunt u opnieuw maken en configureren van deze apparaten.

## <a name="errors-during-dr"></a>Fouten tijdens DR

**Cloud connectivity storing tijdens DR**

Als de cloud-connectiviteit wordt onderbroken nadat DR is gestart en voordat het herstellen van het apparaat voltooid is, wordt de DR, mislukt en wordt u krijgt. Het apparaat dat is gebruikt voor DR is wordt gemarkeerd als *onbruikbaar.* Hetzelfde apparaat kan niet worden vervolgens worden gebruikt voor toekomstige DRs.

**Geen compatibele apparaten**

Als de beschikbare apparaten nog niet voldoende ruimte is, ziet u een fout aan het effect dat er geen compatibele apparaten.

**Vooraf fouten controleren**

Als een van de prechecks niet is voldaan, ziet u controle vooraf fouten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple virtuele matrix via het lokale web UI](storsimple-ova-web-ui-admin.md).
