<properties 
   pageTitle="Uw account van StorSimple opslag beheren | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u kunt de pagina StorSimple Manager configureren toevoegen, bewerken, verwijderen of de beveiliging te gebruiken voor een account opslag draaien."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Gebruik van de StorSimple Manager-service voor het beheren van uw account opslag

## <a name="overview"></a>Overzicht

De pagina **configureren** bevat alle serviceparameters globale die kunnen worden gemaakt in de service StorSimple Manager. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en opnemen:

- Opslag-accounts 
- Bandbreedte sjablonen 
- Access besturingselement records 

Deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** toevoegen, bewerken, of opslag accounts verwijderen of de beveiliging te gebruiken voor een account opslag draaien.

 ![Pagina configureren](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Opslag accounts bevatten de referenties die het apparaat wordt gebruikt voor toegang tot uw account opslagruimte bij uw provider cloud. Hierna ziet u referenties zoals de accountnaam van de en de primaire toegangstoets voor Microsoft Azure opslag-accounts. 

Klik op de pagina **configureren** worden alle opslag-accounts die zijn gemaakt voor de facturering abonnement in tabelvorm met de volgende gegevens weergegeven:

- **Naam** : de unieke naam die zijn toegewezen aan het account wanneer deze is gemaakt.
- **SSL ingeschakeld** – of het SSL is ingeschakeld en apparaat-naar-cloud communicatie via het beveiligde kanaal.
- **Wordt gebruikt voor** – het aantal volume met de opslag-account.

De meest voorkomende taken die betrekking hebben op opslag-accounts die kunnen worden uitgevoerd op de pagina **configureren** zijn:

- Een opslag-account toevoegen 
- Een account opslag bewerken 
- Een account opslagruimte verwijderen 
- Belangrijke draaiing van opslag-accounts 

## <a name="types-of-storage-accounts"></a>Typen opslag-accounts

Er zijn drie soorten opslag-accounts die kunnen worden gebruikt met uw apparaat StorSimple.

- **Automatisch gegenereerde opslag accounts** , zoals de naam wordt gesuggereerd, dit type opslag account automatisch wordt gegenereerd wanneer de service voor het eerst wordt gemaakt. Zie meer informatie over hoe dit account opslag is gemaakt, [stap 1: Maak een nieuwe service](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) in [Deploy uw on-premises implementatie StorSimple-apparaat](storsimple-deployment-walkthrough.md). 
- **Opslag-accounts in het serviceabonnement** – hierna ziet u de opslag van Azure-accounts die zijn gekoppeld aan hetzelfde abonnement als dat van de service. Zie voor meer informatie over hoe deze opslag accounts zijn gemaakt, [Over Azure opslag Accounts](../storage/storage-create-storage-account.md). 
- **Opslag accounts buiten het serviceabonnement** – hierna ziet u de opslag van Azure-accounts die niet zijn gekoppeld aan uw service en waarschijnlijk aanwezig waren voordat de service is gemaakt.

## <a name="add-a-storage-account"></a>Een opslag-account toevoegen

U kunt een opslag-account toevoegen door een unieke, beschrijvende naam en -referenties die zijn gekoppeld aan het account opslagruimte (met de opgegeven cloud-serviceprovider) te geven. U hebt ook de optie van het inschakelen van de secure sockets layer (SSL)-modus voor het maken van een beveiligd kanaal voor communicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaald cloud-provider. Let op, echter nadat een opslag-account is gemaakt, kunt u de cloud-serviceprovider niet wijzigen.

Terwijl de opslag-account wordt opgeslagen, wordt de service probeert te communiceren met de cloud-provider. De referenties en het materiaal dat toegang die u hebt verstrekt, wordt op dit moment worden geverifieerd. Een opslag-account is gemaakt alleen als de verificatie is geslaagd. Als de verificatie mislukt, wordt een foutbericht weergegeven worden weergegeven.

Resourcemanager opslag accounts in Azure-portal is gemaakt, worden ook ondersteund met StorSimple. De resourcemanager opslag-accounts wordt niet weergegeven in de vervolgkeuzelijst voor selectie wanneer u probeert te maken van een container volume, alleen de opslag-accounts die zijn gemaakt in de portal van Azure klassieke wordt weergegeven. Resourcemanager opslag accounts moet worden toegevoegd met de procedure een opslag toevoegen hieronder beschreven.

> [AZURE.NOTE] De procedure voor het toevoegen van een account opslag verschilt op basis van de StorSimple software-versie die u gebruikt. Zorg ervoor dat de juiste procedure voor uw versie StorSimple.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Een account opslag bewerken

U kunt een opslag-account dat wordt gebruikt door een container volume bewerken. Als u een opslag-account dat is momenteel in gebruik bewerken, is het enige veld die beschikbaar zijn voor het wijzigen van de access-toets voor de opslag-account. U kunt de nieuwe opslag-toegangstoets opgeven en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Een account opslag bewerken

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op **configureren**.

2. Klik op **Opslag Accounts toevoegen/bewerken**.

3. Klik in het dialoogvenster **Add/Edit opslag-Accounts** :

  1. Kies in de vervolgkeuzelijst van **Opslag-Accounts**, een bestaand account die u wilt wijzigen. Het kan ook hierbij de opslag-accounts die automatisch zijn gegenereerd wanneer de service voor het eerst is gemaakt.
  2. Zo nodig kunt u de selectie **SSL-modus inschakelen** .
  3. U kunt kiezen voor het omwisselen van uw toegangstoetsen voor opslag-account. Zie [toets draaiing van opslag accounts](#key-rotation-of-storage-accounts) voor meer informatie over het uitvoeren van belangrijke draaiing.
  4. Klik op het pictogram van het selectievakje ![Controleer pictogram](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) de instellingen op te slaan. De instellingen op de pagina **configureren** bijgewerkt. Klik op **Opslaan** als de bijgewerkte instellingen wilt opslaan.

     ![Een account opslag bewerken](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Een account opslagruimte verwijderen

> [AZURE.IMPORTANT] Alleen als deze niet wordt gebruikt door een container volume, kunt u een account opslagruimte verwijderen. Als een opslag-account wordt gebruikt door een container volume, verwijdert u eerst de container volume en verwijder vervolgens het bijbehorende opslag-account.

#### <a name="to-delete-a-storage-account"></a>Een account opslagruimte verwijderen

1. Op de pagina van de aantekening in de StorSimple Manager service, selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op **configureren**.

2. Plaats in de lijst in tabelvorm opslag accounts, de muisaanwijzer op het account dat u wilt verwijderen.

3. Een verwijderpictogram (**x**) wordt weergegeven in de extreme rechterkolom voor dat account opslag. Klik op het pictogram **x** als u wilt verwijderen van de referenties.

4. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan met de verwijdering. De tabelvormige vermelding wordt bijgewerkt met de wijzigingen.

## <a name="key-rotation-of-storage-accounts"></a>Belangrijke draaiing van opslag-accounts

Vanwege de beveiliging is belangrijke draaiing vaak een vereiste in datacenters. 

> [AZURE.NOTE] De volgende belangrijke draaiing-informatie en de procedure draaiing toepassen op alleen Microsoft Azure opslag-accounts. Als u een andere cloud-serviceprovider gebruikt, kunt u opslagruimte account toetsen tot en met dashboard van de provider beheren.
 
Elke Microsoft Azure-abonnement kunt een of meer bijbehorende opslag-accounts hebben geïnstalleerd. De toegang tot deze accounts wordt bepaald door de toetsen abonnement en toegang voor elk account opslag. 

Wanneer u een account opslag maakt, genereert Microsoft Azure twee 512 bits opslag toegangstoetsen die worden gebruikt voor verificatie als de opslagruimte-account wordt geopend. Twee opslag toegangstoetsen ondervindt, kunt u de toetsen met uw storage-service niet onderbroken of toegang tot deze service genereren. De toets die momenteel in gebruik is de *primaire* sleutel en de back-toets wordt de *secundaire* sleutel genoemd. Een van deze twee sleutels moet worden opgegeven als uw apparaat Microsoft Azure StorSimple toegang heeft tot uw provider van de cloud-opslag.

## <a name="what-is-key-rotation"></a>Wat is de belangrijkste draaiing?

Toepassingen gebruiken meestal slechts één van de toetsen voor toegang tot uw gegevens. Na een bepaalde periode, kunt u uw toepassingen overzet over het gebruik van de tweede sleutel hebben. Nadat u hebt uw toepassingen naar de secundaire sleutel bent veranderd, kunt u de eerste toets intrekken en klikt u vervolgens een nieuwe sleutel genereren. Met de twee toetsen op deze manier kunt uw toepassingen toegang tot de gegevens zonder eventuele downtime.

De opslagruimte account sleutels zijn altijd opgeslagen in de service in gecodeerde vorm. Deze kunnen echter worden hersteld via de StorSimple Manager-service. De service kan onmiddellijk de primaire en secundaire sleutel voor alle accounts in de opslagruimte hetzelfde abonnement, inclusief accounts hebt gemaakt in de Storage-service, evenals de standaard-opslag accounts gegenereerd wanneer de StorSimple Manager-service is voor het eerst wordt gemaakt. De service StorSimple Manager wordt altijd deze toetsen te krijgen van de Azure klassieke portal en sla deze op een versleutelde wijze.

## <a name="rotation-workflow"></a>Draaiing werkstroom

De beheerder van een Microsoft Azure kunt genereren of de sleutel primaire of secundaire wijzigen door het rechtstreeks toegang krijgen tot het account opslag (via de Microsoft Azure Storage-service). De service StorSimple Manager ziet deze wijziging niet automatisch.

Om te informeren de StorSimple Manager-service van de wijziging, nodig hebt u voor toegang tot de service StorSimple Manager, de opslag-account opent, en synchroniseer de toets met het primaire of secundaire (afhankelijk van welke monitor is gewijzigd). De service vervolgens krijgt de meest recente toets, versleutelt de toetsen en verzendt de gecodeerde sleutel naar het apparaat.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Voor het synchroniseren van sleutels voor opslag-accounts in hetzelfde abonnement als de service (alleen Azure)

1. Klik op de pagina **Services** op het tabblad **configureren** .

2. Klik op **Opslag Accounts toevoegen/bewerken**.

3. Ga als volgt te werk in het dialoogvenster:

  1. Selecteer het account opslagruimte met de toets die u wilt synchroniseren. De opslagruimte account sleutels zijn versleuteld wanneer deze worden weergegeven.
  2. In de service StorSimple Manager moet u de sleutel die eerder is gewijzigd in de Microsoft Azure Storage-service wordt bijgewerkt. Als de primaire toegangstoets is gewijzigd (geregenereerde), klikt u op **primaire sleutel synchroniseren**. Als de secundaire sleutel is gewijzigd, klikt u op **secundaire sleutel synchroniseren**.

    ![toetsen synchroniseren](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Voor het synchroniseren van sleutels voor opslag-accounts buiten het serviceabonnement

1. Klik op de pagina **Services** op het tabblad **configureren** .

2. Klik op **Opslag Accounts toevoegen/bewerken**.

3. Ga als volgt te werk in het dialoogvenster:

  1. Selecteer het account opslagruimte met de toegangstoets die u wilt bijwerken.
  2. U moet de toegangstoets opslag in de service StorSimple Manager bijwerken. In dit geval ziet u de toegangstoets opslag. Voer de nieuwe sleutel in het vak van de y **Toegangstoets voor opslag-Account**. 
  3. Sla uw wijzigingen op. Uw opslag access accountsleutel moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [StorSimple beveiliging](storsimple-security.md).
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
