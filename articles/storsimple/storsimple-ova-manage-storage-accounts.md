<properties 
   pageTitle="Uw account van StorSimple opslag beheren | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u kunt de pagina StorSimple Manager configureren toevoegen, bewerken, verwijderen of de beveiliging te gebruiken voor een opslag-account dat is gekoppeld aan de StorSimple virtuele matrix draaien."
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
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Gebruik van de StorSimple Manager-service voor het beheren van opslag accounts voor StorSimple virtuele matrix

## <a name="overview"></a>Overzicht

De pagina **configureren** worden in de globale serviceparameters die kunnen worden gemaakt in de service StorSimple Manager weergegeven. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en opnemen:

- Opslag-accounts 
- Access besturingselement records 

Deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** toevoegen, bewerken of verwijderen van opslagruimte accounts voor uw StorSimple virtuele matrix. De informatie in deze zelfstudie geldt alleen voor de StorSimple virtuele matrix maart 2016 GA release software uitgevoerd.

 ![Pagina configureren](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Opslag accounts bevatten de referenties die het apparaat wordt gebruikt voor toegang tot uw account opslagruimte bij uw provider cloud. Hierna ziet u referenties zoals de accountnaam van de en de primaire toegangstoets voor Microsoft Azure opslag-accounts. 

Klik op de pagina **configureren** worden alle opslag-accounts die zijn gemaakt voor de facturering abonnement in tabelvorm met de volgende gegevens weergegeven:

- **Naam** : de unieke naam die zijn toegewezen aan het account wanneer deze is gemaakt.
- **SSL ingeschakeld** – of het SSL is ingeschakeld en apparaat-naar-cloud communicatie via het beveiligde kanaal.

De meest voorkomende taken die betrekking hebben op opslag-accounts die kunnen worden uitgevoerd op de pagina **configureren** zijn:

- Een opslag-account toevoegen 
- Een account opslag bewerken 
- Een account opslagruimte verwijderen 


## <a name="types-of-storage-accounts"></a>Typen opslag-accounts

Er zijn drie soorten opslag-accounts die kunnen worden gebruikt met uw apparaat StorSimple.

- **Automatisch gegenereerde opslag accounts** , zoals de naam wordt gesuggereerd, dit type opslag account automatisch wordt gegenereerd wanneer de service voor het eerst wordt gemaakt. Zie voor meer informatie over hoe dit account opslag is gemaakt, [maken een nieuwe service](storsimple-ova-manage-service.md#create-a-service). 
- **Opslag-accounts in het serviceabonnement** – hierna ziet u de opslag van Azure-accounts die zijn gekoppeld aan hetzelfde abonnement als dat van de service. Zie voor meer informatie over hoe deze opslag accounts zijn gemaakt, [Over Azure opslag Accounts](../storage/storage-create-storage-account.md). 
- **Opslag accounts buiten het serviceabonnement** – hierna ziet u de opslag van Azure-accounts die niet zijn gekoppeld aan uw service en waarschijnlijk aanwezig waren voordat de service is gemaakt.

Elke StorSimple virtuele matrix Hiermee maakt u een container (met een voorvoegsel hcs) in het bijbehorende opslag-account. Deze container heeft de cloud-gegevens voor uw apparaat. Verwijder deze container niet door deze te openen via de opslag van Azure-service terwijl deze actie worden er gegevens verloren gaan.

## <a name="add-a-storage-account"></a>Een opslag-account toevoegen

U kunt een opslag-account toevoegen aan uw Manager StorSimple service te configureren door een unieke, beschrijvende naam en -referenties die zijn gekoppeld aan het account opslag te geven. U hebt ook de optie van het inschakelen van de secure sockets layer (SSL)-modus voor het maken van een beveiligd kanaal voor communicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaald cloud-provider. Terwijl de opslag-account wordt opgeslagen, wordt de service probeert te communiceren met de cloud-provider. De referenties en het materiaal dat toegang die u hebt verstrekt, wordt op dit moment worden geverifieerd. Een opslag-account is gemaakt alleen als de verificatie is geslaagd. Als de verificatie mislukt, wordt een foutbericht weergegeven worden weergegeven.

Resourcemanager opslag accounts in Azure-portal is gemaakt, worden ook ondersteund met StorSimple. De resourcemanager opslag-accounts wordt niet weergegeven in de vervolgkeuzelijst voor selectie, alleen de opslag accounts die zijn gemaakt in de portal van Azure klassieke wordt weergegeven. Resourcemanager opslag accounts moet worden toegevoegd met de procedure om toe te voegen een opslag-account, zoals hieronder beschreven.

De procedure voor het toevoegen van een Azure klassieke opslag-account wordt hieronder beschreven.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Een account opslag bewerken

U kunt een opslag-account gebruikt door uw apparaat kunt bewerken. Als u een opslag-account dat is momenteel in gebruik bewerkt, zijn de velden die beschikbaar zijn voor het wijzigen van de access-toets en de SSL-modus voor de opslag-account. U kunt opgeven van de nieuwe opslag-toegangstoets of wijzigen van de selectie **inschakelen SSL-modus** en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Een account opslag bewerken

1. Klik op de pagina service aantekening Selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op **configureren**.

2. Klik op **Opslag Accounts toevoegen/bewerken**.

3. Klik in het dialoogvenster **Add/Edit opslag-Accounts** :

  1. Kies in de vervolgkeuzelijst van **Opslag-Accounts**, een bestaand account die u wilt wijzigen. 
  2. Zo nodig kunt u de selectie **SSL-modus inschakelen** .
  3. U kunt kiezen om te genereren van de toegangstoetsen van uw opslag-account. Zie [de opslagruimte account toetsen genereren](storage-create-storage-account.md#manage-your-storage-access-keys)voor meer informatie. Geef de nieuwe sleutel voor opslag-account. Dit is de primaire toegangstoets voor een account Azure opslag. 
  4. Klik op het pictogram van het selectievakje ![Controleer pictogram](./media/storsimple-ova-manage-storage-accounts/checkicon.png) de instellingen op te slaan. De instellingen op de pagina **configureren** bijgewerkt. 
  5. Onderaan op de pagina, klikt u op **Opslaan** als de bijgewerkte instellingen wilt opslaan. 

     ![Een account opslag bewerken](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Een account opslagruimte verwijderen

> [AZURE.IMPORTANT] Alleen als deze niet gebruikt wordt, kunt u een account opslagruimte verwijderen. Als een opslag-account gebruikt, wordt u krijgt.

#### <a name="to-delete-a-storage-account"></a>Een account opslagruimte verwijderen

1. Op de pagina van de aantekening in de StorSimple Manager service, selecteer uw service, dubbelklik op de servicenaam van de en klik vervolgens op **configureren**.

2. Plaats in de lijst in tabelvorm opslag accounts, de muisaanwijzer op het account dat u wilt verwijderen.

3. Een verwijderpictogram (**x**) wordt weergegeven in de extreme rechterkolom voor dat account opslag. Klik op het pictogram **x** als u wilt verwijderen van de referenties.

4. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan met de verwijdering. De tabelvormige vermelding wordt bijgewerkt met de wijzigingen.

5. Onderaan op de pagina, klikt u op **Opslaan** als de bijgewerkte instellingen wilt opslaan.


## <a name="next-steps"></a>Volgende stappen

- Informatie over het [beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).
