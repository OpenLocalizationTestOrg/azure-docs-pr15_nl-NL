<properties 
   pageTitle="Wijzig uw wachtwoorden StorSimple | Microsoft Azure" 
   description="Wordt beschreven hoe de StorSimple Manager-service gebruiken om uw beheerder StorSimple momentopname Manager en apparaat wachtwoorden wijzigen." 
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
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Gebruik van de service StorSimple Manager StorSimple wachtwoorden wijzigen

## <a name="overview"></a>Overzicht 

De Azure klassieke portal pagina **configureren** bevat alle apparaatparameters die u kunt opnieuw configureren op een apparaat StorSimple dat wordt beheerd door een StorSimple Manager-service. Deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** een apparaatbeheerder van uw of StorSimple momentopname Manager-wachtwoord wijzigen.

## <a name="change-the-device-administrator-password"></a>Het wachtwoord van de beheerder van apparaat wijzigen

Wanneer u Windows PowerShell-interface voor toegang tot het StorSimple apparaat gebruikt, moet u een apparaat beheerderswachtwoord invoeren. Wanneer het eerste StorSimple-apparaat is geregistreerd bij een service, is het standaardwachtwoord voor deze interface *Wachtwoord1*. Voor de beveiliging van uw gegevens moet u dit wachtwoord aan het einde van het registratieproces wijzigen. U kunt niet van het registratieproces afsluiten zonder te dit wachtwoord wijzigen. Zie voor meer informatie [stap 3: configureren en het apparaat via Windows PowerShell registreren voor StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord die eerst is ingesteld met de Windows PowerShell-interface tijdens de registratie kan vervolgens worden gewijzigd via de portal van Azure klassieke. De volgende stappen als u wilt wijzigen van het apparaat beheerderswachtwoord uitvoeren.

#### <a name="to-change-the-device-administrator-password"></a>Het apparaat administrator-wachtwoord wijzigen

1. Klik in de klassieke-portal op **apparaten** > **configureren** voor uw apparaat.

2. Schuif omlaag naar de sectie **Apparaat beheerderswachtwoord** . Een wachtwoord in dat van 8 tot 15 tekens bevat bevatten. Het wachtwoord moet een combinatie van 3 of meer van hoofdletters, kleine letters, cijfers en speciale tekens.

3. Bevestig het wachtwoord.

4. Klik op **Opslaan** onder aan de pagina.

Het wachtwoord van het apparaat-beheerder moet nu worden bijgewerkt. U kunt dit gewijzigde wachtwoord gebruiken voor toegang tot de Windows PowerShell-interface.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Het wachtwoord StorSimple momentopname Manager wijzigen

StorSimple momentopname Manager-software bevindt zich op uw Windows-host en kunnen beheerders voor het beheren van back-ups van uw apparaat StorSimple in de vorm van lokale en cloud momentopnamen.

Tijdens het configureren van een apparaat in StorSimple momentopname Manager, wordt u gevraagd het IP-adres en wachtwoord om te verifiëren van uw apparaat opslagruimte op te geven. Dit wachtwoord wordt eerst geconfigureerd via de Windows PowerShell-interface. Zie voor meer informatie [stap 3: configureren en het apparaat via Windows PowerShell registreren voor StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Het wachtwoord die eerst is ingesteld met de Windows PowerShell-interface tijdens de registratie kan vervolgens worden gewijzigd via het klassieke portal. De volgende stappen als u wilt wijzigen van het wachtwoord StorSimple momentopname Manager uitvoeren.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Het wachtwoord StorSimple momentopname Manager wijzigen

1. Klik in de klassieke-portal op **apparaten** > **configureren** voor uw apparaat.

2. Schuif omlaag naar de sectie **StorSimple momentopname Manager** . Voer een wachtwoord 14 of 15 tekens. Zorg ervoor dat het wachtwoord een combinatie van 3 of meer van hoofdletters, kleine letters, cijfers en speciale tekens bevat.

3. Bevestig het wachtwoord.

4. Klik op **Opslaan** onder aan de pagina.

Het wachtwoord StorSimple momentopname Manager moet nu worden bijgewerkt.
 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [StorSimple beveiliging](storsimple-security.md).

- Meer informatie over het [wijzigen van de apparaatconfiguratie van uw](storsimple-modify-device-config.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
