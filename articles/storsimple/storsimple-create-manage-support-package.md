<properties
   pageTitle="Een StorSimple ondersteuning-pakket maken | Microsoft Azure"
   description="Informatie over het maken en bewerken van een ondersteuningspakket met voor uw apparaat StorSimple ontsleutelen."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Maken en beheren van een pakket StorSimple-ondersteuning

## <a name="overview"></a>Overzicht

Een StorSimple ondersteuning-pakket is een eenvoudig te gebruiken om die alle relevante logboeken Microsoft Support helpen verzamelt bij het oplossen van problemen StorSimple apparaat. De logboeken aan de verzameld zijn versleuteld en gecomprimeerd.

Deze zelfstudie bevat stapsgewijze instructies voor het maken en beheren van het ondersteuningspakket.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Maken en uploaden van een ondersteuningspakket in de klassieke Azure-portal

U kunt maken en een support-pakket uploaden naar de website van Microsoft Support via de pagina **onderhoud** van de service in de portal van Azure klassieke.

> [AZURE.NOTE] De upload is vereist een sleutel ondersteuning. Uw engineeren ondersteuning dient dit voor u in een e-mailbericht.

Een pakket met ondersteuning voor versleuteld en gecomprimeerde (.cab-bestand) is gemaakt en geüpload naar de site ondersteuning. Vervolgens kan de engineeren ondersteuning dit pakket opgehaald van de site ondersteuning voor het oplossen van het probleem.

De volgende stappen uitvoeren in de klassieke portal een ondersteuningspakket maken.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Een ondersteuningspakket maken in de klassieke Azure-portal

1. Selecteer **apparaten** > **onderhoud**.

2. Selecteer in de sectie **ondersteunen pakket** **maken en uploaden support-pakket**.

3. Ga als volgt te werk in het dialoogvenster **support-pakket maken en uploaden** :

    ![Ondersteuningspakket maken](./media/storsimple-create-manage-support-package/IC740923.png)

    - Geef in het tekstvak **Ondersteuning sleutel** de sleutel. Uw Microsoft-ondersteuning engineeren moet deze sleutel naar u verzenden in een e-mail.

    - Schakel het selectievakje toestemming geven om automatisch de support-pakket uploaden naar de website van Microsoft Support.

    - Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Handmatig een ondersteuningspakket maken

In sommige gevallen moet u handmatig het ondersteuningspakket via Windows PowerShell voor StorSimple maken. Bijvoorbeeld:

- Als u verwijderen vertrouwelijke informatie uit uw logboekbestanden wilt voordat u delen met Microsoft Support.

- Als u problemen bij het uploaden van het pakket vanwege verbindingsproblemen met ondervindt.

U kunt uw handmatig gegenereerde support-pakket met Microsoft Support delen via e-mail. Voer de volgende stappen uit om te maken van een ondersteuningspakket in Windows PowerShell voor StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Maken van een ondersteuningspakket in Windows PowerShell voor StorSimple

1. Als u wilt een Windows PowerShell-sessie starten als een beheerder op de externe computer die wordt gebruikt om te verbinden met uw StorSimple-apparaat, voert u de volgende opdracht uit:

    `Start PowerShell`

2. In de Windows PowerShell-sessie, verbinding met de SSAdmin Console van uw apparaat:

    - Voer bij de opdrachtprompt:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. Voer uw beheerderswachtwoord apparaat in het dialoogvenster dat wordt geopend. Het standaardwachtwoord luidt als volgt:

        `Password1`

        ![PowerShell referentie-dialoogvenster](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Selecteer **OK**.
    1. Voer bij de opdrachtprompt:

        `Enter-PSSession $MS`

3. Voer de gewenste opdracht in de sessie dat wordt geopend.

    - Voor gedeelde netwerken die beveiligd met een wachtwoord zijn, invoeren:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        U wordt gevraagd om een wachtwoord, een pad naar de gedeelde netwerkmap en een wachtwoordzin versleuteling (omdat het ondersteuningspakket is versleuteld). Een ondersteuningspakket is vervolgens in de opgegeven map gemaakt.

    - Voor aandelen die niet beveiligd met een wachtwoord zijn, hoeft u niet de `-Credential` parameter. Voer de volgende in:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Het ondersteuningspakket wordt gemaakt voor besturing voor beide in de opgegeven gedeelde netwerkmap. Dit is een versleutelde, gecomprimeerde bestand die kan worden verzonden naar Microsoft Support voor probleemoplossing. Zie voor meer informatie [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>De Export-HcsSupportPackage cmdlet-parameters
U kunt de volgende parameters gebruiken met de cmdlet exporteren-HcsSupportPackage.

| Parameter            | Vereist/optioneel | Beschrijving                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Vereist          | Gebruiken om de locatie van de gedeelde netwerkmap waarin het ondersteuningspakket is geplaatst.                                                                 |
| `-EncryptionPassphrase` | Vereist          | Gebruiken om een wachtwoordzin waarmee het ondersteuningspakket versleutelen.                                                                                                        |
| `-Credential`           | Optionele          | Gebruik access referenties voor de gedeelde netwerkmap op te geven.                                                                                        |
| `-Force`                | Optionele          | Gebruik om de versleuteling wachtwoordzin bevestigingsstap overslaan.                                                                                                                |
| `-PackageTag`           | Optionele          | Wordt gebruikt om op te geven van een map onder *pad* waarin het ondersteuningspakket is geplaatst. De standaardinstelling is [apparaatnaam van het]-[huidige datum en time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Optionele          | Geef als **Cluster** (standaard) om te maken van een ondersteuningspakket met voor beide besturing voor. Als u maken van een pakket alleen voor de huidige controller wilt, geeft u **Controller**. |


## <a name="edit-a-support-package"></a>Een ondersteuningspakket bewerken

Nadat u een support-pakket hebt gegenereerd, moet u mogelijk het pakket om te verwijderen van vertrouwelijke informatie bewerken. Dit kunt volumenamen, apparaat IP-adressen en back-up van de namen van de logboekbestanden opnemen.

> [AZURE.IMPORTANT] U kunt alleen voor het ondersteuningspakket dat is gegenereerd via Windows PowerShell voor StorSimple bewerken. U kunt een pakket dat is gemaakt in de portal van Azure klassieke met StorSimple Manager-service niet bewerken.

Als u wilt bewerken een ondersteuningspakket voordat u deze op de website Microsoft Support uploadt, eerst ontsleutelen het ondersteuningspakket, de bestanden bewerken en klikt u vervolgens opnieuw versleutelen. De volgende stappen uitvoeren.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Een ondersteuningspakket in Windows PowerShell voor StorSimple bewerken

1. Genereer een ondersteuningspakket zoals hierboven, in [een ondersteuningspakket in Windows PowerShell voor StorSimple maken](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Download het script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op uw client.

3. Importeer de Windows PowerShell-module. Geef het pad naar de lokale map waarin u het script hebt gedownload. Als u wilt importeren de module, invoeren:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Alle bestanden zijn *.aes* -bestanden die worden gecomprimeerd en versleuteld. Als u wilt het uit en ontsleutelen van bestanden, invoeren:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Houd er rekening mee dat de werkelijke bestandsextensies nu worden weergegeven voor alle bestanden.

    ![Voor ondersteuningspakket bewerken](./media/storsimple-create-manage-support-package/IC750706.png)

5. Wanneer u wordt gevraagd of u voor de wachtwoordzin versleuteling, voert u de wachtwoordzin die u gebruikt wanneer het ondersteuningspakket is gemaakt.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Blader naar de map waarin de logboekbestanden. Omdat de logboekbestanden worden nu gecomprimeerd en ontsleuteld, wordt deze oorspronkelijke bestandsextensies hebben. Deze bestanden wijzigt om het verwijderen van een klant-specifiek-gegevens, zoals het van volumenamen en apparaat IP-adressen, en sla de bestanden.

7. Sluit de bestanden om deze te comprimeren met gzip en ze te versleutelen met AES-256. Dit is voor de snelheid en beveiliging in het ondersteuningspakket overbrengen via een netwerk. Als u wilt comprimeren en coderen van bestanden, voert u de volgende handelingen uit:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Voor ondersteuningspakket bewerken](./media/storsimple-create-manage-support-package/IC750707.png)

8. Wanneer u wordt gevraagd, bieden u een wachtwoordzin versleuteling voor het ondersteuningspakket gewijzigde.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Noteer de nieuwe wachtwoordzin, zodat u deze met Microsoft Support delen kunt wanneer aangevraagd.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Voorbeeld: Bewerken van bestanden in een ondersteuningspakket op een wachtwoord is beveiligd delen

Het volgende voorbeeld wordt getoond hoe ontsleutelen, bewerken en opnieuw coderen een ondersteuningspakket.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van de ondersteuningspakketten en apparaat logboeken problemen met de implementatie van uw apparaat](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
