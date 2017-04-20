<properties
   pageTitle="StorSimple virtuele matrix - bepaling in Hyper-V implementeren"
   description="Deze tweede zelfstudie in de virtuele matrix StorSimple implementatie heeft betrekking op een virtueel apparaat in Hyper-V inrichten."
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
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Implementeren StorSimple virtuele matrix - inrichten van een virtuele matrix in Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Overzicht

Deze zelfstudie inrichten is van toepassing op Microsoft Azure StorSimple virtuele matrices (ook wel bekend als StorSimple on-premises implementatie virtuele apparaten of StorSimple virtuele apparaten) actieve maart 2016 beschikbaarheid (GA) van de algemene release. Deze zelfstudie wordt beschreven hoe een virtuele matrix StorSimple op een hostsysteem Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 inrichten. In dit artikel is van toepassing op de implementatie van StorSimple virtuele matrices in Azure klassieke portal, evenals Microsoft Azure overheid Cloud.

U moet beheerdersbevoegdheden inrichten en een virtueel apparaat configureren. De instelling inrichten en eerste kan ongeveer 10 minuten duren om te voltooien.


## <a name="provisioning-prerequisites"></a>Vereisten voor inrichting

Hier vindt u de vereisten voor het inrichten van een virtueel apparaat op een hostsysteem Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2.

### <a name="for-the-storsimple-manager-service"></a>Voor de service StorSimple Manager

Controleer het volgende voordat u begint:

-   U kunt de stappen in [de portal voor StorSimple virtuele matrix voorbereiden](storsimple-ova-deploy1-portal-prep.md)hebt voltooid.

-   U kunt de afbeelding virtueel apparaat voor Hyper-V hebt gedownload van de Azure-portal. Zie voor meer informatie [stap 3: de afbeelding virtueel apparaat downloaden](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] De software uitvoeren op de StorSimple virtuele matrix kan alleen worden gebruikt in combinatie met de Storsimple Manager-service.

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele StorSimple-apparaat

Controleer het volgende voordat u een virtueel apparaat implementeert:

-   U toegang hebt tot een hostsysteem met Hyper-V of hoger voor Windows Server 2008 R2 dat kan worden gebruikt voor een voorziening een apparaat.

-   Het hostsysteem kan opslagquota van de volgende bronnen voor het inrichten van uw virtuele apparaat:

    -   Minimaal 4 cores.

    -   Ten minste 8 GB RAM.

    -   Gebruikersinterface van één netwerk.

    -   Een 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Bekijk de netwerken vereisten voor het implementeren van een virtueel apparaat StorSimple en correct configureren van het datacenter-netwerk voordat u begint. Zie [StorSimple virtuele matrix netwerken vereisten](storsimple-ova-system-requirements.md#networking-requirements)voor meer informatie.

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichting

Als u wilt inrichten en verbinding maken met een virtueel apparaat, moet u de volgende stappen uitvoeren:

1.  Zorg ervoor dat het hostsysteem onvoldoende bronnen om te voldoen aan de minimale virtueel apparaat.

2.  Een virtueel apparaat in uw hypervisor inrichten.

3.  Het virtuele apparaat starten en u het IP-adres.

Elk van deze stappen wordt uitgelegd in de volgende secties.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Stap 1: Zorg ervoor dat het hostsysteem voldoet aan minimum aantal virtueel apparaat

Als u wilt een virtueel apparaat hebt gemaakt, wordt u nodig hebt:

-   De functie Hyper-V is geïnstalleerd op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 SP1.

-   Microsoft Hyper-V Manager op een Microsoft Windows-client die is gekoppeld aan de host.

U moet ervoor zorgen dat de onderliggende hardware (hostsysteem) waarop u het virtuele apparaat maakt mogen reserveren van de volgende bronnen voor uw virtuele apparaat is:

- Minimaal 4 cores.
- Ten minste 8 GB RAM.
- Gebruikersinterface van één netwerk.
- Een 500 GB virtuele schijf voor systeemgegevens.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Stap 2: Een virtueel apparaat in hypervisor inrichten

De volgende stappen voor het inrichten van een apparaat in uw hypervisor uitvoeren.

#### <a name="to-provision-a-virtual-device"></a>Voor het inrichten van een virtueel apparaat

1.  Op uw Windows Server-host, de afbeelding virtueel apparaat naar een lokaal station te kopiëren. Dit is de afbeelding die u hebt gedownload via de portal van Azure (schijf of VHDX). Noteer de locatie waar u de afbeelding hebt gekopieerd, zoals u dit later in de procedure gebruikt.

2.  Open **Serverbeheer**. In de rechterbovenhoek, klik op **Extra** en selecteer **Hyper-V-beheer**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Als u Windows Server 2008 R2 uitvoert, opent u de Manager Hyper-V. In Serverbeheer, klikt u op **rollen > Hyper-V > Hyper-V Manager**.

1.  In **Hyper-V-beheer**in het deelvenster bereik met de rechtermuisknop op uw systeem knooppunt om het contextmenu te openen en klik vervolgens op **Nieuw** > **VM**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Klik op **volgende**op de pagina **voordat u begint** van de Wizard Nieuwe virtuele Machine.

1.  Geef een **naam** voor uw virtuele apparaat op de pagina **naam opgeven en de locatie** . Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Klik op de pagina **opgeven generatie** Kies het type apparaat afbeelding en klik op **volgende**. Deze pagina wordt niet weergegeven als u Windows Server 2008 R2 gebruikt.

    * Kies **generatie 2** als u de afbeelding van een .vhdx of hoger voor Windows Server 2012 hebt gedownload.
    * Kies **generatie 1** als u de afbeelding van een VHD voor Windows Server 2008 R2 of hoger hebt gedownload.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Klik op de pagina **toewijzen geheugen** ten minste een **opstarten geheugen** van opgegeven **8192 MB**, niet dynamisch geheugen inschakelen en klik vervolgens op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Klik op de pagina **netwerk configureren** de virtuele schakeloptie gebruikt die is verbonden met Internet en klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Klik op de pagina **verbinding maken met virtuele harde schijf** Kies **een bestaande virtuele harde schijf**, geef de locatie van de afbeelding virtueel apparaat (.vhdx of VHD) en klik vervolgens op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Bekijk het **Overzicht** en klik op **Voltooien** om te maken van de virtuele machine. Maar niet gehandhaafd nog - moet u ook enkele cores CPU en een tweede station toevoegen. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Om te voldoen aan de minimale vereisten, moet u 4 cores. Als u wilt toevoegen van virtuele processors, met uw hostsysteem is geselecteerd in het venster **Hyper-V-beheer** in het rechterdeelvenster onder de lijst met **virtuele Machines**, zoek de virtuele machine die u zojuist hebt gemaakt. Selecteer en met de rechtermuisknop op de computernaam van de en selecteer **Instellingen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Klik op de pagina **Instellingen** in het linkerdeelvenster op **Processor**. Stel in het rechterdeelvenster, **aantal virtuele processors** aan 4 (of meer). Klik op **toepassen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Om te voldoen aan de minimale vereisten, moet u ook een 500 GB virtuele gegevensschijf toevoegen. Op de pagina **Instellingen** :

    1.  Selecteer in het linkerdeelvenster **SCSI-Controller**.
    2.  In het rechterdeelvenster, selecteer **Harde schijf,** en klik op **toevoegen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Klik op de pagina **harde schijf** , selecteer de optie **virtuele harde schijf** en klikt u op **Nieuw**. Hiermee start u de **Wizard Nieuwe virtuele hardeschijf**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Klik op **volgende**op de pagina **voordat u begint** van de Wizard Nieuwe virtuele hardeschijf.

1.  Accepteer de standaardwaarde van **VHDX** opmaak op de **pagina Kies Schijfopruiming opmaken**. Klik op **volgende**. Als u werkt met het Windows Server 2012 R2 of Windows Server 2008 R2, kunt u dit venster niet zien.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  Klik op de **pagina Kies Schijfopruiming Type**virtuele harde schijftype als **dynamisch gegevensniveaus uitvouwen** (aanbevolen) instellen. Als u een **vaste grootte** schijf kiest, werkt ook, maar u moet mogelijk wachten lang duren. Het is raadzaam dat u niet de optie **Differencing** gebruikt. Klik op **volgende**. Houd er rekening mee dat het standaardapparaat op Windows Server 2012 R2 en Windows Server 2012 **dynamisch gegevensniveaus uitvouwen** is. De standaardinstelling is in Windows Server 2008 R2, **vaste grootte**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Geef een **naam** , evenals de **locatie** (u kunt bladeren naar een) voor de gegevensschijf op de pagina **naam en locatie opgeven** . Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Selecteer de optie **een nieuwe lege virtuele vaste-schijf maken** op de pagina **Schijf configureren** en de grootte te geven als **500 GB** (of meer). 500 GB is de minimumvereiste, kunt u altijd een grotere schijf inrichten. Houd er rekening mee dat u geen uitvouwen of verkleinen van de schijf eenmaal deze is ingericht. Raadpleeg de sectie van de formaatgrepen in het document [Aanbevolen procedures](storsimple-ova-best-practices.md#configuration-best-practices) voor meer informatie over de grootte van schijf inrichten. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Controleer de details van de gegevensschijf virtuele op de pagina **Summary** en als voldaan, klikt u op **Voltooien** om te maken van de schijf. De wizard wordt gesloten en een virtuele harde schijf, worden toegevoegd aan uw computer.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  U gaat terug naar de pagina **Instellingen** . Klik op **OK** om de pagina **Instellingen** sluiten en terugkeren naar Hyper-V Manager-venster.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Stap 3: Het virtuele apparaat starten en de IP ophalen

De volgende stappen om te starten van uw virtuele apparaat en verbinden met dit uitvoeren.

#### <a name="to-start-the-virtual-device"></a>Het virtuele apparaat starten

1.  Start het virtuele apparaat.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Nadat het apparaat wordt uitgevoerd, selecteer het apparaat, klik met de rechtermuisknop en selecteert u **verbinding maken**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Mogelijk moet u wacht 5-10 minuten totdat het apparaat gereed. Een bericht wordt weergegeven op de console om de voortgang te geven. Nadat het apparaat klaar is, gaat u naar **actie**. Druk op `Ctrl + Alt + Delete` aan te melden bij het virtuele apparaat. De standaardgebruiker is *StorSimpleAdmin* en het standaardwachtwoord *Wachtwoord1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Veiligheidsoverwegingen verloopt het wachtwoord van het apparaat-beheerder in het eerste logboek op. U wordt gevraagd het wachtwoord wijzigen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Typ het wachtwoord dat ten minste 8 tekens bevat. Het wachtwoord moet voldoen aan ten minste 3 uit de volgende vereisten voor 4: hoofdletters, kleine letters, cijfers en speciale tekens. Het wachtwoord te bevestigen opnieuw invoert. U wordt geïnformeerd dat het wachtwoord heeft gewijzigd.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Nadat u het wachtwoord is gewijzigd, kan het virtuele apparaat, start opnieuw. Wacht totdat het apparaat te starten.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    De Windows PowerShell-console van het apparaat worden, samen met een voortgangsbalk weergegeven.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Stap 6-8 zijn alleen van toepassing wanneer u in een omgeving niet DHCP-starten. Als u zich in een omgeving DHCP, sla deze stappen en Ga naar stap 9. Als u van uw apparaat in niet DHCP-omgeving opgestart, ziet u het volgende scherm.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Nu moet u voor het configureren van het netwerk.

1.  Gebruik de `Get-HcsIpAddress` opdracht voor een overzicht van het netwerkinterfaces ingeschakeld op uw virtuele apparaat. Als uw apparaat een interface één netwerk is ingeschakeld heeft, wordt de standaardnaam die is toegewezen aan deze interface is `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Gebruik de `Set-HcsIpAddress` cmdlet voor het configureren van het netwerk. Hieronder vindt u een voorbeeld:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Nadat de installatie voltooid is en het apparaat is opgestart omhoog, ziet u de tekst van de banner apparaat. Noteer het IP-adres en de URL die wordt weergegeven in de bannertekst voor het beheren van het apparaat. U kunt dit IP-adres verbinding maken met het web UI van uw virtueel apparaat en de lokale installatie en registratie te voltooien.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Optioneel) Deze stap alleen uitvoeren als u uw apparaat in de Cloud overheid implementeert. U kunt nu de Verenigde Staten federale Information Processing Standard (FIPS)-modus op uw apparaat. De standaard FIPS 140 definieert cryptografische algoritmen goedgekeurd voor gebruik door ons federale overheid computersystemen voor het beveiligen van vertrouwelijke gegevens.
    1. Als u wilt de FIPS-modus inschakelen, moet u de volgende cmdlet uitvoeren:

        `Enter-HcsFIPSMode`

    2. Het apparaat opnieuw opstart nadat u de modus FIPS hebt ingeschakeld, zodat de cryptografische validatie pas van kracht.

        > [AZURE.NOTE] U kunt inschakelen of uitschakelen van FIPS-modus op uw apparaat. Variërende van het apparaat tussen FIPS en niet-FIPS-modus wordt niet ondersteund.

Als uw apparaat niet aan de van de minimale configuratievereisten voldoet, ziet u een fout in de bannertekst (Zie hieronder). U moet de apparaatconfiguratie wijzigen, zodat er voldoende resources om te voldoen aan de minimale vereisten. U kunt vervolgens opnieuw opstart, en u kunt verbinding maken met het apparaat. Zie de van de minimale configuratievereisten in [stap 1: Zorg ervoor dat het hostsysteem aan de minimale virtueel apparaat voldoet](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Als u een andere fout tijdens de eerste configuratie via het lokale web UI betrokken, raadpleegt u de volgende werkstromen aan [uw StorSimple virtuele matrix beheren met de lokale web UI](storsimple-ova-web-ui-admin.md).

-   Diagnostische tests uitvoeren om op te [lossen web UI-instelling](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Genereren log Inpakken en weergave logboekbestanden](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![videopictogram](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Video beschikbaar**

Bekijk de video om te zien hoe u een virtuele matrix StorSimple in Hyper-V kunt inrichten.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

-   [Uw StorSimple virtuele matrix als een bestandsserver instellen](storsimple-ova-deploy3-fs-setup.md)

-   [Uw StorSimple virtuele matrix instellen als een iSCSI-server](storsimple-ova-deploy3-iscsi-setup.md)
