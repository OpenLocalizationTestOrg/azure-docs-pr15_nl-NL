<properties
   pageTitle="Update 1.2 installeren op uw apparaat StorSimple | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u StorSimple 8000 reeks Update 1.2 installeert op uw apparaat van de reeks StorSimple 8000."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Update 1.2 installeren op uw apparaat StorSimple

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u Update 1.2 installeert op een apparaat StorSimple waarop een softwareversie vóór Update 1 wordt uitgevoerd. De zelfstudie behandelt ook de extra stappen die is vereist voor het bijwerken wanneer een gateway is geconfigureerd op een netwerkinterface dan gegevens 0 van het apparaat StorSimple.

Update 1.2 bevat apparaat software-updates, LSI stuurprogramma updates en schijf firmware. De software en LSI stuurprogramma updates zijn ononderbroken updates en via de portal van Azure klassieke kunnen worden toegepast. De schijf firmware-updates zijn storend updates en kunnen alleen worden toegepast via de Windows PowerShell-interface van het apparaat.

Afhankelijk van welke versie van uw apparaat wordt uitgevoerd, kunt u bepalen als Update 1.2 wordt toegepast. U kunt de softwareversie van uw apparaat controleren door te gaan naar de sectie **snel aan te duiden** van uw apparaat **Dashboard**.

</br>

| Als softwareversie...   | Wat gebeurt er in de portal?                              |
|---------------------------------|--------------------------------------------------------------|
| Release - GA                    | Als u de releaseversie (GA) gebruikt, worden niet toegepast op deze update. Neem [contact op met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als u wilt bijwerken van uw apparaat.|
| 0,1 bijwerken                      | Portal geldt Update 1.2.                                |
| 0,2 bijwerken                      | Portal geldt Update 1.2.                                |
| 0,3 bijwerken                      | Portal geldt Update 1.2.                                |
| Update 1                        | Deze update is niet beschikbaar.                           |
| 1.1 bijwerken                      | Deze update is niet beschikbaar.                           |

</br>

> [AZURE.IMPORTANT]

> -  Er kan geen Update 1.2 onmiddellijk omdat we een gefaseerd uitrol van de updates doen. Zoeken naar updates in een paar dagen weer als deze Update beschikbaar komen binnenkort.
> - Deze update bevat een handmatige en automatische oude controles om te bepalen de status van het apparaat met hardware provincie en de netwerkconnectiviteit. Deze oude controles uitgevoerd alleen als u de updates vanaf de portal van Azure klassieke toepassen.
> - Het is raadzaam dat u de software en stuurprogramma's via de portal van Azure klassieke installeren. U moet alleen Ga naar de Windows PowerShell-interface van het apparaat (voor het installeren van updates) als het selectievakje oude update gateway is mislukt in de portal. De updates duurt 5-10 uur te installeren (inclusief de Windows-Updates). De updates van de modus onderhoud moeten worden geïnstalleerd via de Windows PowerShell-interface van het apparaat. Zoals onderhoud modus updates storend updates zijn, resulteert dit in een tijd voor uw apparaat.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Update 1.2 installeren via de portal van Azure klassieke

De volgende stappen als u wilt bijwerken van uw apparaat naar [1.2 bijwerken](storsimple-update1-release-notes.md)uitvoeren. Gebruik deze procedure alleen als er een gateway op gegevens 0 netwerkinterface op uw apparaat is geconfigureerd.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Controleer of uw apparaat **StorSimple 8000 reeks Update 1.2 (6.3.9600.17584)**wordt uitgevoerd. **Laatst bijgewerkt datum** moet ook worden gewijzigd. U ziet ook dat onderhoud modus updates beschikbaar zijn (dit bericht mogelijk verder moet worden weergegeven voor tot 24 uur nadat u de updates hebt geïnstalleerd).

    Onderhoud modus updates zijn storend updates die resulteren in apparaat downtime en kunnen alleen worden toegepast via de Windows PowerShell-interface van uw apparaat.

    ![Pagina Onderhoud] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Pagina Onderhoud")

13. Download de updates van de modus onderhoud met behulp van de stappen die worden vermeld in [hotfixes downloaden]( #to-download-hotfixes) zoeken en downloaden KB3063416, welke installaties schijf firmware-updates (de andere updates moeten al worden geïnstalleerd door nu).

13. Volg de stappen in [installeren en controleer of onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) de updates van de modus onderhoud te installeren.

14. Klik in de klassieke Azure portal navigeren naar de pagina **onderhoud** en onder aan de pagina, klikt u op **Updates scannen** als u wilt controleren op een Windows-Updates en klik vervolgens op **Updates installeren**. U bent klaar nadat alle updates zijn geïnstalleerd.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Update 1.2 installeren op een apparaat met een gateway is geconfigureerd voor een niet-gegevens 0 netwerkinterface

U moet deze procedure alleen gebruiken als u het selectievakje gateway niet bij het installeren van de updates via de portal van Azure klassieke. Het selectievakje mislukt als u een gateway toegewezen aan een niet-gegevens 0 netwerkinterface hebt en uw apparaat een softwareversie vóór Update 1 wordt uitgevoerd. Als uw apparaat heeft geen een gateway op een niet-gegevens 0 netwerkinterface, kunt u het apparaat rechtstreeks vanuit de portal van Azure klassieke bijwerken. Zie [installatie 1.2 via de portal van Azure klassieke bijwerken](#install-update-1.2-via-the-azure-classic-portal).

De softwareversies die kunnen worden bijgewerkt via deze methode worden updates Update 0,1, 0,2, en 0,3.


> [AZURE.IMPORTANT]
>
> - Als uw apparaat wordt uitgevoerd Release (GA) versie, neem contact op met [Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om u te helpen met de update.
> - Deze procedure moet slechts één keer worden uitgevoerd als u wilt bijwerken 1.2 toepassen. U kunt de portal van Azure klassieke latere updates toepassen.

Als uw apparaat 1 oude Update-software wordt uitgevoerd en er een gateway instellen voor een netwerkinterface dan gegevens 0, kunt u de Update 1.2 toepassen op de volgende twee manieren:

- **Optie 1**: de update downloaden en toepassen met behulp van de `Start-HcsHotfix` cmdlet uit de Windows PowerShell-interface van het apparaat. Dit is de aanbevolen methode. **Gebruik deze methode geen Update 1.2 toepassen als uw apparaat Update 1.0 of Update 1.1 wordt uitgevoerd.**

- **Optie 2**: de gatewayconfiguratie verwijderen en installeer de update rechtstreeks vanuit de Azure klassieke portal.


Gedetailleerde instructies voor elk van deze beschikbaar in de volgende secties.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Optie 1: Gebruik Windows PowerShell voor StorSimple Update 1.2 als hotfix toepassen

U moet deze procedure alleen gebruiken als u de Update 0,1, 0,2, 0,3 en als het selectievakje van de gateway is mislukt bij het installeren van updates van de Azure klassieke portal worden uitgevoerd. Als u software Release (GA) gebruikt, kunt u [Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als u wilt bijwerken van uw apparaat.

Als u wilt installeren Update 1.2 als hotfix, moet u downloaden en installeren van de volgende hotfixes:

| Volgorde  | KB        | Beschrijving             | Type update  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Software-update         |  Normale     |
| 2      | KB3043005 | LSI SA's controller bijwerken |  Normale     |
| 3      | KB3063416 | Schijf firmware           | Onderhoud  |

Voordat u deze procedure de update toepassen, zorg ervoor dat:

- Beide apparaatcontrollers zijn online.

De volgende stappen om toe te passen Update 1.2 uitvoeren. **De updates kunnen ongeveer 2 uur duren om uit te voeren (ongeveer 30 minuten voor software, 30 minuten voor stuurprogramma, 45 minuten voor Schijfopruiming firmware).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Optie 2: De Azure klassieke-portal gebruiken om toe te passen Update 1.2 na het verwijderen van de gatewayconfiguratie

Deze procedure geldt alleen voor StorSimple-apparaten die een softwareversie vóór Update 1 of beschikken over een gateway instellen op een netwerkinterface dan gegevens 0. U moet de gateway-instelling voordat u de update wissen.

De update mogelijk enkele uren duren. Als uw hosts in verschillende subnetten zijn, verwijderen van de gatewayconfiguratie op de iSCSI-interfaces kan leiden tot downtime. Het is raadzaam dat u gegevens 0 voor iSCSI-verkeer om te verkleinen van de downtime configureren.

De volgende stappen om te schakelen van de netwerkinterface met de gateway en pas de update uitvoeren.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Update 1.2 release](storsimple-update1-release-notes.md).
