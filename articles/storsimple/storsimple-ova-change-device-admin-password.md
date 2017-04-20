<properties 
   pageTitle="Wijzigen van de beheerderswachtwoord StorSimple virtueel apparaat | Microsoft Azure"
   description="Beschreven hoe u het gebruik van de Azure klassieke portal of het web StorSimple virtuele matrix UI om te wijzigen van het apparaat beheerderswachtwoord."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Het virtuele matrix StorSimple apparaat beheerderswachtwoord wijzigen

## <a name="overview"></a>Overzicht

Wanneer u de Windows PowerShell-interface voor toegang tot het StorSimple virtuele apparaat gebruikt, moet u een apparaat beheerderswachtwoord invoeren. Wanneer het apparaat StorSimple is eerst deze is ingericht en gestart, is het standaardwachtwoord *Wachtwoord1*. Het standaardwachtwoord verloopt voor de beveiliging van uw gegevens, de eerste keer is dat u zich aanmelden en u moet dit wachtwoord wijzigen.

U kunt ook op het lokale web UI of de Azure klassieke portal gebruiken om te wijzigen van het apparaat administrator-wachtwoord op elk gewenst moment nadat het apparaat dat is geïmplementeerd in uw productieomgeving. Elk van deze procedures wordt in dit artikel beschreven.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Gebruik de Azure klassieke portal het wachtwoord wijzigen

De volgende stappen als u wilt wijzigen van het apparaat beheerderswachtwoord via de portal van Azure klassieke uitvoeren.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Het apparaat beheerderswachtwoord via de portal van Azure klassieke wijzigen

1. Klik in de portal klikt u op **apparaten** > **configuratie** voor uw apparaat.

2. Schuif omlaag naar de sectie **Apparaat beheerderswachtwoord** . Een wachtwoord in dat van 8 tot 15 tekens bevat bevatten. Het wachtwoord moet een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens.

3. Bevestig het wachtwoord.

4. Klik op **Opslaan** onder aan de pagina.

Het wachtwoord van het apparaat-beheerder moet nu worden bijgewerkt. U kunt dit gewijzigde wachtwoord gebruiken voor toegang tot het apparaat lokaal.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Gebruik het web StorSimple virtuele matrix UI het wachtwoord wijzigen

De volgende stappen als u wilt wijzigen van het apparaat beheerderswachtwoord via het lokale web UI uitvoeren.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Het apparaat beheerderswachtwoord via het lokale web UI wijzigen

1. Klik in het lokale web UI, op **onderhoud** > **wachtwoord wijzigen** voor uw apparaat.

    ![Wachtwoord1 wijzigen](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Voer in het **huidige wachtwoord**.

3. Een **Nieuw wachtwoord**opgeven. Het wachtwoord moet ten minste 8 tekens lang zijn. 3 van 4 van de volgende lijst moet bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.

    Houd er rekening mee dat uw wachtwoord mogen niet hetzelfde als de laatste 24 wachtwoorden.

3. Het wachtwoord te bevestigen opnieuw invoert.

    ![Wachtwoord2 wijzigen](./media/storsimple-ova-change-device-admin-password/image41.png)

4. Onderaan op de pagina, klikt u op **toepassen**. Het nieuwe wachtwoord worden vervolgens toegepast. Als het wijzigen van het wachtwoord is geslaagd, ziet u het volgende foutbericht weergegeven.

    ![Wachtwoordfout](./media/storsimple-ova-change-device-admin-password/image42.png)

    Nadat u het wachtwoord is bijgewerkt, wordt u gewaarschuwd. U kunt dit gewijzigde wachtwoord vervolgens gebruiken voor toegang tot het apparaat lokaal.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).
