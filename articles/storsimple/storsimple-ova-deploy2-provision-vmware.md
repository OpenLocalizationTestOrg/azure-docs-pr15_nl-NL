<properties
   pageTitle="StorSimple virtuele matrix - bepaling in VMware implementeren"
   description="Deze tweede zelfstudie in de virtuele matrix StorSimple implementatie reeks heeft betrekking op een virtueel apparaat in VMware inrichting."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Implementeren StorSimple virtuele matrix - een virtuele matrix in VMware inrichten

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Overzicht 
Deze inrichten zelfstudie geldt voor StorSimple virtuele matrices (ook wel bekend als StorSimple on-premises implementatie virtuele apparaten of StorSimple virtuele apparaten) actieve maart 2016 beschikbaarheid (GA) van de algemene release. Deze zelfstudie wordt beschreven hoe provision en verbinding maken met een virtuele matrix StorSimple op een hostsysteem VMware ESXi 5.5 en hoger. In dit artikel is van toepassing op de implementatie van StorSimple virtuele matrices in Azure klassieke portal, evenals Microsoft Azure overheid Cloud.

U moet beheerdersbevoegdheden inrichten en verbinding maken met een virtueel apparaat. De instelling inrichten en eerste kan ongeveer 10 minuten duren om te voltooien.


## <a name="provisioning-prerequisites"></a>Vereisten voor inrichting

Hier vindt u de vereisten voor het inrichten van een virtueel apparaat op een hostsysteem VMware ESXi 5.5 en hoger.

### <a name="for-the-storsimple-manager-service"></a>Voor de service StorSimple Manager

Controleer het volgende voordat u begint:

-   U kunt de stappen in [de portal voor StorSimple virtuele matrix voorbereiden](storsimple-ova-deploy1-portal-prep.md)hebt voltooid.

-   U kunt de afbeelding virtueel apparaat voor VMware hebt gedownload van de Azure-portal. Zie voor meer informatie [stap 3: de afbeelding virtueel apparaat downloaden](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele StorSimple-apparaat 

Controleer het volgende voordat u een virtueel apparaat implementeert:

-   U toegang hebt tot een hostsysteem met Hyper-V (2008 R2 of hoger) die is een voorziening kon u een apparaat.

-   Het hostsysteem kan opslagquota van de volgende bronnen voor het inrichten van uw virtuele apparaat:

    -   Minimaal 4 cores.

    -   Ten minste 8 GB RAM.

    -   Gebruikersinterface van één netwerk.

    -   Een 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-network-in-datacenter"></a>Voor het netwerk in datacenter 

Controleer het volgende voordat u begint:

-   U hebt de netwerken vereisten voor het implementeren van een virtueel apparaat StorSimple beoordeeld en het netwerk van het datacenter aan de hand van de vereisten geconfigureerd. Zie [systeemvereisten voor StorSimple virtuele matrix](storsimple-ova-system-requirements.md)voor meer informatie.

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichting 

Als u wilt inrichten en verbinding maken met een virtueel apparaat, moet u de volgende stappen uitvoeren:

1.  Zorg ervoor dat het hostsysteem onvoldoende bronnen om te voldoen aan de minimale virtueel apparaat.

2.  Een virtueel apparaat in uw hypervisor inrichten.

3.  Het virtuele apparaat starten en u het IP-adres.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Stap 1: Zorgen hostsysteem minimale virtueel apparaat voldoet

Als u wilt een virtueel apparaat hebt gemaakt, wordt u nodig hebt:

-   Toegang tot een hostsysteem met VMware ESXi Server 5.5 en hierboven.

-   VMware vSphere client op uw systeem voor het beheren van de ESXi-host.

    -   Minimaal 4 cores.

    -   Ten minste 8 GB RAM.

    -   Netwerkinterface van één is verbonden met het netwerk dat het verkeer met Internet. De minimale internetbandbreedte moet 5 Mbps toe te staan dat voor optimale werking van het apparaat.

    -   Een 500 GB virtuele schijf voor gegevens.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Stap 2: Een virtueel apparaat in hypervisor inrichten

De volgende stappen voor het inrichten van een virtueel apparaat in uw hypervisor uitvoeren.

1.  Kopieer de afbeelding virtueel apparaat op uw systeem. Dit is de afbeelding die u hebt gedownload via de portal van Azure klassieke. 
    1.  Zorg ervoor dat dit is de meest recente afbeeldingsbestand dat u hebt gedownload. Als u de afbeelding eerder hebt gedownload, downloadt u deze opnieuw om ervoor te zorgen dat u de meest recente afbeelding. De meest recente afbeelding heeft twee bestanden (in plaats van één).
    2.  Noteer de locatie waar u de afbeelding hebt gekopieerd, zoals u dit later in de procedure gebruikt.

2.  Meld u aan bij het gebruik van de client vSphere ESXi-server. U moet beheerdersbevoegdheden een virtuele machine maken.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  Selecteer in de vSphere-client, in de sectie voorraad in het linkerdeelvenster de ESXi-Server.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  U wordt eerst de VMDK uploaden naar de server ESXi. Ga naar het tabblad **configuratie** in het rechterdeelvenster. Klik onder **Hardware**Selecteer **opslag**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  Selecteer in het rechterdeelvenster, onder **Datastores**, de gegevensopslag waarnaar u wilt uploaden van de VMDK. De gegevensopslag moet er voldoende ruimte voor de schijven OS en gegevens.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Klik met de rechtermuisknop en selecteer **Bladeren gegevensopslag**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Een **Gegevensopslag** browservenster wordt weergegeven.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Klik in de werkbalk op ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) pictogram om een nieuwe map maken. Geef de mapnaam en noteer deze. U wordt de naam van deze map later gebruiken bij het maken van een virtuele machine (aanbevolen aanbevolen). Klik op **OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  De nieuwe map wordt weergegeven in het linkerdeelvenster van de **Browser voor gegevensopslag**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Klik op het pictogram Upload ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) en selecteert u **Bestand uploaden**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  U moet nu bladeren en wijst u de VMDK-bestanden die u hebt gedownload. Er zijn twee bestanden. Selecteer een bestand uploaden.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Klik op **openen**. Hiermee start u nu het uploaden van het bestand VMDK voor de opgegeven gegevensopslag. Het kan enkele minuten duren voor het bestand te uploaden.


1.  Wanneer de upload voltooid is, ziet u het bestand in de gegevensopslag in de map die u hebt gemaakt. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    U moet nu de tweede VMDK-bestand uploaden naar de dezelfde gegevensopslag.

1.  Ga terug naar het venster van de client vSphere. Met ESXi-server is geselecteerd, met de rechtermuisknop op en selecteer **nieuwe virtuele Machine**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Een **nieuwe virtuele Machine maken** -venster wordt weergegeven. Selecteer de optie **aangepast** op de pagina **configuratie** . Klik op **volgende**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Geef de naam van uw virtuele machine op de pagina **naam en locatie** . Deze naam moet overeenkomen met de mapnaam (aanbevolen aanbevolen) die u eerder in stap 8 hebt opgegeven.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Selecteer een gegevensopslag die u gebruiken wilt voor het inrichten van uw VM op de pagina **opslag** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Selecteer op de pagina **VM versie** **VM versie: 8**. Houd er rekening mee dat versies 8 tot en met 11 worden allemaal ondersteund.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Selecteer het **besturingssysteem Gast** als **Windows**op de pagina **Gast-besturingssysteem** . Selecteer in de vervolgkeuzelijst voor **versie**, **Microsoft Windows Server 2012 (64-bits)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Klik op de pagina **CPU's** past u het **aantal virtuele Sockets** en het **aantal kernen per virtuele socket** zodat het **totale aantal kernen** is 4 (of meer). Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Geef op de pagina **geheugen** 8 GB (of meer) RAM. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Geef het aantal de netwerkinterfaces op de pagina **netwerk** . De minimumvereiste is één netwerkinterface.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Klik op de pagina **SCSI-Controller** accepteer de standaardwaarde **LSI logica SA's controller**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Kies **een bestaande virtuele schijf**op de pagina **Selecteer een schijf** . Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Klik op de pagina **Bestaande upschijf selecteren** onder **Het bestandspad schijf**, klikt u op **Bladeren**. Hiermee wordt een dialoogvenster **Bladeren Datastores** geopend. Navigeer naar de locatie waar u de VMDK hebt geüpload. Nu ziet u slechts één bestand in de gegevensopslag zoals de twee bestanden die u in eerste instantie hebt geüpload zijn samengevoegd. Selecteer het bestand en klik op **OK**. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Accepteer de standaardwaarde en klik op **volgende**op de pagina **Geavanceerde opties** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Bekijk alle instellingen die is gekoppeld aan de nieuwe virtuele machine op de pagina **Gereed om te voltooien** . Controleer **de instellingen VM voordat deze was voltooid bewerken**. Klik op **Doorgaan**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Zoek op de pagina **Eigenschappen van virtuele Machines** in het tabblad **Hardware** het apparaat. Selecteer **nieuwe harde schijf**. Klik op **toevoegen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Hiermee opent u het venster **Hardware toevoegen** . Klik op de pagina **Type apparaat** onder **Kies het type apparaat dat u wilt toevoegen**, selecteer **harde schijf** en klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Kies op de pagina **Selecteer een schijf** **maken een nieuwe virtuele schijf**. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Klik op de pagina **maken een schijf** door de **Schijfgrootte** te wijzigen naar 500 GB (of meer). Selecteer onder **Schijf inrichting**, **Dunne inrichten**. Klik op **volgende**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Accepteer de standaardwaarde op de pagina **Geavanceerde opties** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Bekijk de schijfopties op de pagina **Gereed om te voltooien** . Klik op **Voltooien**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Nu gaat u terug naar de pagina eigenschappen van virtuele Machine. Een nieuwe vaste schijf wordt toegevoegd aan uw virtuele computer. Klik op **Voltooien**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Met uw VM geselecteerd in het rechterdeelvenster, Ga naar het tabblad **Samenvatting** . Controleer de instellingen voor uw VM.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Uw VM is nu ingericht. De volgende stap is het IP-adres en power op deze computer.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Stap 3: Het virtuele apparaat starten en de IP ophalen

De volgende stappen om te starten van uw virtuele apparaat en verbinden met dit uitvoeren.

#### <a name="to-start-the-virtual-device"></a>Het virtuele apparaat starten

1.  Start het virtuele apparaat. Klik in de vSphere Configuration Manager in het linkerdeelvenster, selecteer uw apparaat en met de rechtermuisknop om het contextmenu weergeven. Selecteer **Power** en selecteer vervolgens **Power op**. Dit moet power op uw virtuele machine. U kunt de status weergeven in het deelvenster onder **Recente taken** van de client vSphere.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  De configuratietaken duurt een paar minuten om te voltooien. Nadat het apparaat wordt uitgevoerd, gaat u naar het tabblad **Console** . Ctrl + Alt + Delete aan te melden bij het apparaat verzenden. U kunt ook, wijst u de cursor in het consolevenster en druk op Ctrl + Alt + Insert. De standaardgebruiker is *StorSimpleAdmin* en het standaardwachtwoord *Wachtwoord1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Veiligheidsoverwegingen verloopt het wachtwoord van het apparaat-beheerder in het eerste logboek op. U wordt gevraagd het wachtwoord wijzigen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Typ het wachtwoord dat ten minste 8 tekens bevat. Het wachtwoord moet bevatten 3 van 4 van deze vereisten is voldaan: hoofdletters, kleine letters, cijfers en speciale tekens. Het wachtwoord te bevestigen opnieuw invoert. U wordt geïnformeerd dat het wachtwoord heeft gewijzigd.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Nadat u het wachtwoord is gewijzigd, kan het virtuele apparaat start opnieuw op. Wacht totdat het opnieuw opstarten om te voltooien. De Windows PowerShell-console van het apparaat kan worden weergegeven, samen met een voortgangsbalk.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Stap 6-8 zijn alleen van toepassing wanneer u in een omgeving niet DHCP-starten. Als u zich in een omgeving DHCP, sla deze stappen en Ga naar stap 9. Als u van uw apparaat in niet DHCP-omgeving opgestart, ziet u het volgende scherm. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Nu moet u voor het configureren van het netwerk.

1.  Gebruik de `Get-HcsIpAddress` opdracht voor een overzicht van het netwerkinterfaces ingeschakeld op uw virtuele apparaat. Als uw apparaat een interface één netwerk is ingeschakeld heeft, wordt de standaardnaam die is toegewezen aan deze interface is `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Gebruik de `Set-HcsIpAddress` cmdlet voor het configureren van het netwerk. Hieronder vindt u een voorbeeld:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Nadat de installatie voltooid is en het apparaat is opgestart omhoog, ziet u de tekst van de banner apparaat. Noteer het IP-adres en de URL die wordt weergegeven in de bannertekst voor het beheren van het apparaat. U kunt dit IP-adres verbinding maken met het web UI van uw virtueel apparaat en de lokale installatie en registratie te voltooien.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Optioneel) Deze stap alleen uitvoeren als u uw apparaat in de Cloud overheid implementeert. U kunt nu de Verenigde Staten federale Information Processing Standard (FIPS)-modus op uw apparaat. De standaard FIPS 140 definieert cryptografische algoritmen goedgekeurd voor gebruik door ons federale overheid computersystemen voor het beveiligen van vertrouwelijke gegevens.
    1. Als u wilt de FIPS-modus inschakelen, moet u de volgende cmdlet uitvoeren:
        
        `Enter-HcsFIPSMode`

    2. Het apparaat opnieuw opstart nadat u de modus FIPS hebt ingeschakeld, zodat de cryptografische validatie pas van kracht.

        > [AZURE.NOTE] U kunt inschakelen of uitschakelen van FIPS-modus op uw apparaat. Variërende van het apparaat tussen FIPS en niet-FIPS-modus wordt niet ondersteund.


Als uw apparaat niet aan de van de minimale configuratievereisten voldoet, ziet u een fout in de bannertekst (Zie hieronder). U moet de apparaatconfiguratie wijzigen, zodat er voldoende resources om te voldoen aan de minimale vereisten. U kunt vervolgens opnieuw opstart, en u kunt verbinding maken met het apparaat. Zie de van de minimale configuratievereisten in [stap 1: Zorg ervoor dat het hostsysteem aan de minimale virtueel apparaat voldoet](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Als u een andere fout tijdens de eerste configuratie via het lokale web UI betrokken, raadpleegt u de volgende werkstromen aan [uw StorSimple virtuele matrix beheren met de lokale web UI](storsimple-ova-web-ui-admin.md).

-   Diagnostische tests uitvoeren om op te [lossen web UI-instelling](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Genereren log Inpakken en weergave logboekbestanden](storsimple-ova-web-ui-admin.md#generate-a-log-package)..

## <a name="next-steps"></a>Volgende stappen

-   [Uw StorSimple virtuele matrix als een bestandsserver instellen](storsimple-ova-deploy3-fs-setup.md)

-   [Uw StorSimple virtuele matrix instellen als een iSCSI-server](storsimple-ova-deploy3-iscsi-setup.md)

