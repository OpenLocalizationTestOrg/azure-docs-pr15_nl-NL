<properties 
   pageTitle="Servicedashboard in de StorSimple Manager-- virtuele matrix | Microsoft Azure"
   description="Een beschrijving van het servicedashboard van de StorSimple Manager en wordt uitgelegd hoe u deze gebruiken om te controleren van de status van uw StorSimple virtuele matrix."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Het servicedashboard van de StorSimple Manager gebruiken voor de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

De dashboardpagina StorSimple Manager-service biedt een overzichtsweergave van de StorSimple virtuele matrices (ook wel bekend als StorSimple on-premises implementatie virtuele apparaten of virtuele apparaten) die verbonden zijn met de service StorSimple Manager, markeren die een systeembeheerder aandacht meer nodig heeft. Deze zelfstudie maakt u kennis met de dashboardpagina, worden de Dashboardinhoud en de functie uitgelegd en de taken die u van deze pagina uitvoeren kunt wordt beschreven.

![Service-dashboard](./media/storsimple-ova-service-dashboard/dashboard1.png)

Het servicedashboard van de StorSimple Manager bevat de volgende gegevens:

- Klik in **het grafiekgebied boven aan de pagina,** kunt u de doelstellingen van de relevante bekijken voor uw virtuele apparaten. U kunt de primaire opslag gebruikt op alle virtuele apparaten, evenals de cloudopslag gedurende een periode worden gebruikt door virtuele apparaten weergeven. Gebruik de besturingselementen in de rechterbovenhoek van het diagram opgeven relatief of absoluut gebruik en wilt zien van een 1 week, 1 maand, 3-maand of 1 jaar tijdschaal. Gebruik het besturingselement voor vernieuwen ![vernieuwen-besturingselementen](./media/storsimple-ova-service-dashboard/refresh-control.png) vernieuwen van de grafiek.

- Het **Gebruik overzicht** ziet u de primaire opslag dat wordt deze is ingericht en verbruikt door alle virtuele apparaten ten opzichte van de totale opslagcapaciteit beschikbaar op alle virtuele apparaten. **Provisioned** verwijst naar de hoeveelheid opslagruimte die is voorbereid en toegewezen voor gebruik en **Max. capaciteit** ziet u de maximale capaciteit van alle virtuele apparaten **gebruikt** verwijst naar gebruik van de waarden voor aandelen of volumes zoals door de initiators die met de virtuele apparaten verbonden zijn bekeken.

- Het gebied **waarschuwingen** biedt een momentopname van de actieve waarschuwingen over alle virtuele apparaten, gegroepeerd op waarschuwing ernst. Het prioriteitsniveau klikken, opent de **waarschuwingen** -pagina, beperkt als u wilt deze meldingen weergeven. Klik op de pagina **waarschuwingen** kunt u klikken op een afzonderlijke waarschuwing om extra informatie over deze melding, inclusief eventuele aanbevolen acties weer te geven. Als het probleem is opgelost, kunt u ook de melding wissen.

- Het gebied **taken** bevat een momentopname van recente taken op alle virtuele apparaten die zijn verbonden met uw service. Er zijn koppelingen die u gebruiken kunt om te zoeken op taken die momenteel in uitvoering en die is geslaagd of mislukt in de afgelopen 24 uur. 

- Het gebied **snel aan te duiden** aan de rechterkant van de pagina bevat nuttige informatie, zoals de status van service totale aantal virtuele apparaten die zijn verbonden met de service, de locatie van de service en de details van het abonnement dat is gekoppeld aan de service. Er is ook een koppeling naar het logboek bewerkingen. Klik op de koppeling om een lijst met alle servicebewerkingen voor voltooide StorSimple Manager. 

U kunt de dashboardpagina StorSimple Manager-service gebruiken om de volgende taken uitvoeren:

- De sleutel voor de registratie krijgen.
- De logboeken aan de bewerking weergeven.

## <a name="get-the-service-registration-key"></a>De sleutel voor de registratie ophalen

De registersleutel voor de service wordt gebruikt om u te registreren van een virtueel StorSimple-apparaat met de service StorSimple Manager, zodat het apparaat wordt weergegeven in de portal van Azure klassieke voor verdere management acties. De sleutel is gemaakt op de eerste virtueel apparaat en gedeeld met de resterende virtuele apparaten. 

**Registratiegegevens sleutel** (onderaan op de pagina) te klikken, opent het dialoogvenster **Service registratiegegevens sleutel** , waar kunt u de huidige service registratie sleutel naar het Klembord kopiëren of genereren van de sleutel voor de registratie.

![registratie-toets](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Opnieuw genereren van de sleutel heeft geen invloed op eerder geregistreerde virtuele apparaten: dit van invloed is op alleen de virtuele apparaten die zijn geregistreerd met de service nadat de sleutel wordt opnieuw gegenereerd.

Ga voor meer informatie over het aanvragen van de service registratie-toets om [de service registratie-toets](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>De logboeken bewerkingen weergeven

U kunt de logboeken aan de bewerking weergeven door te klikken op de koppeling van bewerking logboeken beschikbaar in het deelvenster **snelle weergave** van het dashboard. Hiermee gaat u naar de services beheerpagina, waar u kunt filteren en raadpleegt u de logboeken aan de specifieke uw StorSimple Manager-service.

![Meld u bewerkingen](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Volgende stappen

Informatie over het [gebruik van de lokale web gebruikersinterface voor het beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).