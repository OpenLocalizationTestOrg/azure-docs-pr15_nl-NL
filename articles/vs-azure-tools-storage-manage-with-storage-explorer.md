<properties
    pageTitle="Aan de slag met opslag Explorer (Preview) | Microsoft Azure"
    description="Azure opslag resources met opslag Explorer (Preview) beheren"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Aan de slag met opslag Explorer (Preview)

## <a name="overview"></a>Overzicht 

Microsoft Azure opslag Explorer (Preview) is een zelfstandig product-app waarmee u kunt eenvoudig werken met gegevens van Azure-opslag voor Windows, OS X en Linux. In dit artikel leert u de verschillende manieren van verbinden met en uw accounts Azure opslag beheren.

![Microsoft Azure opslag Explorer (Preview)][15]

## <a name="prerequisites"></a>Vereisten voor

- [Download en installeer opslag Explorer (preview)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Verbinding maken met een opslag-account of service

Opslag Explorer (Preview) kunt u een allerlei manieren verbinding maken met opslag-accounts. Dit geldt ook voor verbinding maken met accounts van opslag die is gekoppeld aan uw Azure-abonnementen, verbinding maken met accounts voor opslagruimte en services uit andere abonnementen Azure, gedeeld en zelfs verbinding maakt met en lokale opslag met de Emulator Azure-opslag beheren:

- [Verbinding maken met een abonnement op Azure](#connect-to-an-azure-subscription) - opslag resources die deel uitmaken van uw abonnement op Azure beheren.
- [Werken met lokale ontwikkeling opslag](#work-with-local-development-storage) - lokale opslag met de Emulator Azure-opslag beheren. 
- [Koppelen naar externe opslag](#attach-or-detach-an-external-storage-account) - opslag resources die deel uitmaken van een ander Azure abonnement met de accountnaam en de sleutel van de opslag-account beheren.
- [Bijvoegen opslag account gebruik SA's](#attach-storage-account-using-sas) - opslag resources die deel uitmaken van een ander Azure abonnement met een SA's beheren.
- [Bijvoegen mailservice gebruikt SA's](#attach-service-using-sas) - beheren van een specifieke storage-service (blob container, wachtrij of tabel) die deel uitmaken van een ander Azure abonnement met een SA's.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

> [AZURE.NOTE] Als u geen een Azure-account hebt, kunt u [registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [activeren van de voordelen van uw Visual Studio-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Selecteer in de opslagruimte Explorer (Preview) **Azure-Accountinstellingen**. 

    ![Azure Accountinstellingen][0]

1. Deelvenster aan de linkerkant wordt nu weergegeven voor alle Microsoft-accounts die u hebt aangemeld. Als u wilt verbinden met een ander account, selecteert u **een account toevoegen**en volgt u de dialoogvensters aan te melden met een Microsoft-account dat is gekoppeld aan ten minste één actief Azure-abonnement.

    ![Een account toevoegen][1]

1. Zodra u met een Microsoft-account aanmelden, wordt het linkerdeelvenster vullen met de Azure abonnementen die is gekoppeld aan dat account. Selecteer de Azure abonnementen die u wilt werken en selecteer vervolgens **toepassen**. ( **Alle abonnementen** Hiermee schakelt u het selecteren van alle of geen van de vermelde Azure abonnementen selecteren).

    ![Selecteer Azure abonnementen][3]

1. Deelvenster aan de linkerkant wordt nu weergegeven voor de opslag-accounts die zijn gekoppeld aan de geselecteerde Azure abonnementen.

    ![Geselecteerde Azure abonnementen][4]

## <a name="work-with-local-development-storage"></a>Werken met lokale ontwikkeling opslag

Opslag Explorer (Preview) kunt u in combinatie met lokale opslag gebruik van de Azure opslag Emulator. Hiermee kunt u programmacode tegen schrijven en testen van opslag zonder dat hiervoor een opslag-account dat is geïmplementeerd op Azure (Aangezien het opslag-account is door de Azure opslag Emulator wordt nagebootst).

>[AZURE.NOTE] De Emulator Azure-opslag is momenteel alleen ondersteund voor Windows. 

1. Vouw in het linkerdeelvenster van opslag Explorer (Preview) uit de **(lokaal en bijlage** > **Opslag Accounts** > **(ontwikkeling)** knooppunt.

    ![Lokale ontwikkeling knooppunt][21]

1. Als u de Azure opslag Emulator nog niet hebt geïnstalleerd, wordt u gevraagd om dit te doen via een infobalk. Als de infobalk wordt weergegeven, selecteert u **de nieuwste versie downloaden**en installeren van de emulator. 

    ![Azure opslag Emulator prompt downloaden][22]

1. Wanneer de emulator is geïnstalleerd, hebt u de mogelijkheid om te maken en werken met lokale BLOB's, wachtrijen en tabellen. Selecteer op de onderstaande koppeling om te leren werken met elk accounttype opslag:

    - [Azure blob storage resources beheren](./vs-azure-tools-storage-explorer-blobs.md)
    - Azure bestand opslag resources delen - *binnenkort beschikbaar* beheren
    - Azure wachtrij opslag resources - *binnenkort beschikbaar* beheren
    - Azure tabel opslag resources - *binnenkort beschikbaar* beheren

## <a name="attach-or-detach-an-external-storage-account"></a>Koppelen of loskoppelen van een externe opslag-account

Opslag Explorer (Preview) biedt de mogelijkheid om te koppelen aan externe opslag-accounts, zodat opslag-accounts kunnen eenvoudig worden gedeeld. In deze sectie wordt uitgelegd hoe u als bijlage toevoegen aan (en loskoppelen van) externe opslag-accounts.

### <a name="get-the-storage-account-credentials"></a>De opslag-accountreferenties ophalen

Om te delen van een externe opslag-account, moet de eigenaar van dat account eerst de referenties - accountnaam en van toetsen - ophalen voor het account en vervolgens deze informatie delen met de persoon willen koppelen aan dat account (externe). Het verkrijgen van de referenties voor de opslag-account kan worden uitgevoerd via de portal van Azure door deze stappen uit: 

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com).
1.  Selecteer **Bladeren**.
1.  **Opslag Accounts**selecteren.
1.  Selecteer het gewenste opslag-account in het blad **Opslag-Accounts** .
1.  Selecteer in het blad **Instellingen** voor het geselecteerde opslag-account, **toegangstoetsen**.

    ![Toegangsoptie toetsen][5]
    
1.  Kopieer in het blad **toegangstoetsen** de waarden **OPSLAGACCOUNTNAAM** en de **toets 1** voor gebruik wanneer u verbinding maakt met het account opslag. 

    ![Toegangstoetsen][6]

### <a name="attach-to-an-external-storage-account"></a>Als bijlage toevoegen aan een externe opslag-account
Als u wilt bijvoegen bij een externe opslag-account, moet u de naam van het account en de sleutel. Het *ophalen van de opslag-accountreferenties* gedeelte wordt uitgelegd hoe voor deze waarden van de Azure-portal. Bedenk wel dat in de portal de accountsleutel heet "belangrijke 1" dus waar de opslagruimte Explorer (Preview) wordt gevraagd om een accountsleutel, u moet invoeren (of plakken) de waarde 'belangrijke 1'. 
 
1.  Selecteer in de opslagruimte Explorer (Preview) **verbinding maken met Azure opslag**.

    ![Verbinding maken met de optie voor Azure-tabelopslag][23]

1.  Klik in het dialoogvenster **verbinding maken met Azure Storage** Geef de accountsleutel ("belangrijke 1" waarde van de Azure portal) en selecteer vervolgens **volgende**.

    ![Verbinding maken met Azure opslag-dialoogvenster][24] 

1.  In het dialoogvenster **Externe opslag bijvoegen** voert u de naam van het opslag-account in het vak **accountnaam** , geef eventuele andere gewenste instellingen en selecteer **volgende** wanneer u klaar bent. 

    ![Externe opslag dialoogvenster als bijlage toevoegen][8]

1.  Controleer de gegevens in het dialoogvenster **Verbinding overzicht** . Als u wilt dat wijzigingen aanbrengen, selecteert u **vorige** en de gewenste instellingen opnieuw in te voeren. Wanneer u klaar bent, selecteert u **verbinding maken**.

1.  Zodra u verbinding hebt, wordt het externe opslag-account weergegeven met de tekst **(externe)** toegevoegd aan de naam van het opslag-account. 

    ![Resultaat van de verbinding met een externe opslag-account][9]

### <a name="detach-from-an-external-storage-account"></a>Loskoppelen van een externe opslag-account

1.  Met de rechtermuisknop op de externe opslag-account dat u wilt loskoppelen en - selecteer in het contextmenu - **ontkoppelen**.

    ![Loskoppelen van opslagoptie][10]

1.  Wanneer het bevestigingsvenster voor het bericht wordt weergegeven, selecteert u **Ja** ter bevestiging het losmaken van het externe opslag-account.

## <a name="attach-storage-account-using-sas"></a>Opslag account SA's met als bijlage toevoegen

Een [SA's (gedeeld Access handtekening)](storage/storage-dotnet-shared-access-signature-part-1.md) kunt de beheerder van een abonnement op Azure de mogelijkheid om toegang te verlenen aan een account opslag tijdelijk zonder hun referenties Azure abonnement op te geven. 

Stel ter illustratie gebruiker a is een beheerder van een abonnement op Azure en gebruiker a wil verleent voor toegang tot een account opslag gedurende een beperkte periode met bepaalde machtigingen toestaan:

1. Gebruiker a genereert een SA's (bestaande uit de verbindingsreeks voor de opslag-account) voor een bepaalde periode en met de gewenste machtigingen.
1. Gebruiker a deelt de SA's met de persoon die toegang tot de opslag account - gebruiker b, in ons voorbeeld.  
1. Verleent gebruikt opslagruimte Explorer (Preview) om het aan het account die deel uitmaken van gebruiker a met de opgegeven SA's koppelen. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Een SA's ophalen voor het account dat u wilt delen

1.  In opslag Explorer (Preview) met de rechtermuisknop op de opslag-account dat u wilt delen en - in het contextmenu - Selecteer **Gedeeld Access-handtekening ophalen**.

    ![SA's context menuoptie ophalen][13]

1. Klik in het dialoogvenster **Gedeeld Access handtekening** Geef de periode en de machtigingen die u wilt gebruiken voor het account en selecteer **maken**.

    ![Dialoogvenster SA's ophalen][14]
 
1. Er verschijnt een tweede **Gedeeld Access handtekening** -dialoogvenster voor het weergeven van de SA's. Selecteer **kopiëren** naast de **Verbindingsreeks** naar het Klembord kopiëren. Selecteer **sluiten** om te sluiten van het dialoogvenster.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Als bijlage toevoegen aan de gedeelde-account via de SA 's

1.  Selecteer in de opslagruimte Explorer (Preview) **verbinding maken met Azure opslag**.

    ![Verbinding maken met de optie voor Azure-tabelopslag][23]

1.  Klik in het dialoogvenster **verbinding maken met Azure Storage** de verbindingsreeks opgeven en selecteer vervolgens **volgende**.

    ![Verbinding maken met Azure opslag-dialoogvenster][24] 

1.  Controleer de gegevens in het dialoogvenster **Verbinding overzicht** . Als u wilt dat wijzigingen aanbrengen, selecteert u **vorige** en de gewenste instellingen opnieuw in te voeren. Wanneer u klaar bent, selecteert u **verbinding maken**.

1.  Eenmaal is toegevoegd, wordt het opslag-account wordt weergegeven met de tekst (SA's) toegevoegd aan de naam van het account dat u hebt verstrekt.

    ![Resultaat van die zijn bijgevoegd bij een account met behulp van SA 's][17]

## <a name="attach-service-using-sas"></a>Bijvoegen service SA's gebruiken

De sectie [bijvoegen opslag account met behulp van SA's](#attach-storage-account-using-sas) ziet u hoe een beheerder Azure-abonnement kunt tijdelijke toegang verlenen tot een opslag-account door te genereren (en delen van) een SA's voor de opslag-account. Een SA's kan ook worden gegenereerd voor een specifieke services (blob container, wachtrij of tabel) binnen een opslag-account.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Een SA's voor de service die u wilt delen genereren

In deze context, kan een service blob container, wachtrij of een tabel zijn. De volgende secties wordt uitgelegd hoe u het genereren van de SA's voor de vermelde service:

- [De SA's voor een container blob ophalen](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- De SA's krijgen voor een bestandsshare - *binnenkort beschikbaar*
- De SA's ophalen voor een wachtrij - *binnenkort beschikbaar*
- De SA's ophalen voor een tabel - *binnenkort beschikbaar*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Als bijlage toevoegen aan de gedeelde account-service voor het gebruik van de SA 's

1.  Selecteer in de opslagruimte Explorer (Preview) **verbinding maken met Azure opslag**.

    ![Verbinding maken met de optie voor Azure-tabelopslag][23]

1.  Klik in het dialoogvenster **verbinding maken met Azure Storage** Geef de SAS-URI en selecteer vervolgens **volgende**.

    ![Verbinding maken met Azure opslag-dialoogvenster][24] 

1.  Controleer de gegevens in het dialoogvenster **Verbinding overzicht** . Als u wilt dat wijzigingen aanbrengen, selecteert u **vorige** en de gewenste instellingen opnieuw in te voeren. Wanneer u klaar bent, selecteert u **verbinding maken**.

1.  Eenmaal is toegevoegd, wordt de zojuist gekoppelde service wordt weergegeven onder het knooppunt **(Service SA's)** .

    ![Resultaat van koppelen aan een gedeelde service SA's gebruiken][20]

## <a name="search-for-storage-accounts"></a>Zoeken naar opslag-accounts

Als u een lange lijst opslag accounts hebt, is een snelle manier om te zoeken van een bepaalde opslag-account gebruik het zoekvak boven aan het linkerdeelvenster. 

Terwijl u in het zoekvak typt, wordt alleen de opslagruimte accounts die overeenkomen met de zoekwaarde die u maximaal hebt ingevoerd die verwijzen in het linkerdeelvenster weergegeven. De volgende schermafbeelding ziet u een voorbeeld waar ik hebt gezocht naar alle opslag accounts waarvan de naam van het opslag-account de tekst 'tarcher bevat'.

![Opslag account zoeken][11]
    
Als u wilt de zoekopdracht wissen, selecteert u de knop **x** in het zoekvak.

## <a name="next-steps"></a>Volgende stappen
- [Azure blob storage resources met opslag Explorer (Preview) beheren](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
