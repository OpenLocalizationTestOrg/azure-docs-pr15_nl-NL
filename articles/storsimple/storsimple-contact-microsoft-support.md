<properties 
   pageTitle="Contact opnemen met Microsoft ondersteuning | Microsoft Azure"
   description="Informatie over het maken van een verzoek voor ondersteuning en een ondersteuningssessie met op uw apparaat StorSimple starten."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Contact opnemen met Microsoft ondersteuning

Als u problemen met uw Microsoft Azure StorSimple-oplossing ondervindt, kunt u een serviceaanvraag voor technische ondersteuning. In een online-sessie met de ondersteuning-engineeren moet u mogelijk ook een ondersteuningssessie met op uw apparaat StorSimple starten. In dit artikel begeleidt u bij:

- Het maken van een verzoek voor ondersteuning.
- Hoe u een ondersteuningssessie voor in de Windows PowerShell-interface van uw apparaat StorSimple starten.

Bekijk de [StorSimple 8000 reeks ondersteuning serviceovereenkomsten en gegevens](https://msdn.microsoft.com/library/mt433077.aspx) voordat u een verzoek voor ondersteuning van maakt.

## <a name="create-a-support-request"></a>Een verzoek voor ondersteuning van maken

De volgende stappen als u wilt maken van een verzoek voor ondersteuning uitvoeren:

#### <a name="to-create-a-support-request"></a>Een verzoek voor ondersteuning van maken

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com/)in de rechterbovenhoek, klikt u op de naam van uw account en klik op **Contact opnemen met Microsoft ondersteuning**.

    ![Ondersteuning van de contactpersoon MS via ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. U wordt omgeleid naar de nieuwe Azure-portal (portal.azure.com). Klik op de tegel **nieuwe aanvraag ondersteuning** .

    ![Ondersteuning van de contactpersoon MS via de nieuwe portal](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Aan de rechterkant van het scherm, wordt het deelvenster **nieuwe ondersteuning verzoek** weergegeven. 

    ![Deelvenster van de aanvraag nieuwe ondersteuning](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. Klik in het dialoogvenster **Basisbeginselen** Vul in het volgende:                                
    1. Selecteer de vervolgkeuzelijst **type probleem** **technische**.
    2. Selecteer een **abonnement** in de vervolgkeuzelijst.
    3. Selecteer in de vervolgkeuzelijst **Service** **StorSimple**. 
    4. Selecteer een **plan voor ondersteuning** in de vervolgkeuzelijst. Moet u eerst een abonnement betaalde ondersteuning voor technische ondersteuning inschakelen.

4. Klik op **volgende**. Het dialoogvenster **probleem** wordt weergegeven.

    ![Deelvenster van de aanvraag nieuwe ondersteuning](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. Klik in het dialoogvenster **probleem** Vul in het volgende:

    1.  Selecteer **een prioriteitsniveau** in de vervolgkeuzelijst.
    2.  Selecteer een **type probleem** in de vervolgkeuzelijst.
    3.  Selecteer een **categorie** in de vervolgkeuzelijst. 
    4.  Beschrijving van het probleem in het vak **Details** .
    5.  Geven aan de datum, tijd en tijdzone die met de meest recente exemplaar van het probleem overeenkomt in het vak **periode** .
    6.  Klik onder **bestand uploaden**, klikt u op het mappictogram om naar uw support-pakket.
    7.  Schakel het selectievakje **diagnostische informatie delen** .

6. Klik op **volgende**. Het dialoogvenster **contactgegevens** wordt weergegeven.

    ![Deelvenster van de aanvraag nieuwe ondersteuning](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Voer uw contactgegevens in en selecteert u een contactmethode (telefonisch of via e-mail). 

8. Schakel het selectievakje **opslaan wijzigingen in contactpersonen voor aanvragen voor toekomstige ondersteuning** .

9. Klik op **maken**.

Nadat u hebt uw aanvraag ingediend, een medewerker van de technische wordt contact met u zo snel mogelijk om verder te gaan met uw aanvraag kunt invullen.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Een ondersteuningssessie voor in Windows PowerShell voor StorSimple starten

Om op te lossen eventuele problemen die bij het apparaat StorSimple optreden mogelijk, moet u oefenen met het team van Microsoft Support. Microsoft Support mogelijk moet u een ondersteuningssessie voor aan te melden bij uw apparaat gebruiken. 

Voer de volgende stappen uit om een ondersteuningssessie te starten:

#### <a name="to-start-a-support-session"></a>Een ondersteuningssessie te starten

1. Toegang tot het apparaat rechtstreeks met behulp van de seriële console of via een telnetsessie vanuit een externe computer. Hiervoor de stappen in [Gebruik stopverf verbinding maken met de seriële console apparaat](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)uit te voeren.

2. In de sessie dat wordt geopend, drukt u op de **Enter** -toets om een opdrachtprompt.

3. Selecteer in het menu seriële console optie 1, **aanmelden met de volledige toegang**.

4. Typ het volgende wachtwoord bij de prompt: 

    `Password1`

5. Typ bij de prompt de volgende opdracht uit:

    `Enable-HcsSupportAccess`

6. Een versleutelde tekenreeks wordt weergegeven voor u. Kopieer deze tekenreeks in een teksteditor zoals Kladblok.

7. Sla deze tekenreeks en deze in een e-mailbericht verzenden naar Microsoft Support. 

> [AZURE.IMPORTANT] U kunt ondersteuning toegang uitschakelen door te voeren `Disable-HcsSupportAccess`. Het apparaat StorSimple wordt ook proberen te uitschakelen ondersteuning toegang 8 uur nadat de sessie is gestart. Dit is een goede gewoonte om uw referenties in StorSimple apparaat wijzigen nadat een ondersteuningssessie is gestart.
