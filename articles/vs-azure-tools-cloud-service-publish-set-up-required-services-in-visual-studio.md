<properties
   pageTitle="Voorbereiden om te publiceren of een Azure-toepassing van Visual Studio implementeren | Microsoft Azure"
   description="Informatie over de procedures om de cloud en opslagruimte Accountservices instellen en configureren van uw Azure-toepassing."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>De voorbereiding publiceren of een Azure-toepassing van Visual Studio implementeren

## <a name="overview"></a>Overzicht

Voordat u een cloud-service project publiceert kunt, moet u de volgende services instellen:

- Een **cloudservice** uw rollen uitvoeren in de Azure-omgeving

- Een **account opslagruimte** biedt toegang tot de services Blob, wachtrij en tabel.

Gebruik van de volgende procedures deze services instellen en configureren van uw toepassing


## <a name="create-a-cloud-service"></a>Een cloudservice maken

Als u een cloudservice wilt Azure publiceren, moet u eerst een cloudservice, die wordt uitgevoerd van uw rollen in de Azure-omgeving maken. U kunt een cloudservice maken in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), zoals beschreven in de sectie **naar een cloudservice met behulp van de Azure klassieke portal maken**, verderop in dit onderwerp. U kunt ook een cloudservice in Visual Studio maken met behulp van de wizard publiceren.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Een cloudservice maken met behulp van Visual Studio

1. Open het snelmenu voor het Azure project en kies **publiceren**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Als u dit nog niet hebt aangemeld, meld u aan met uw gebruikersnaam en wachtwoord voor de Microsoft-account of organisatieaccount dat is gekoppeld aan uw Azure-abonnement.

1. Kies de knop **volgende** om naar de pagina **Instellingen** te gaan.

    ![Publicatie van algemene instellingen van de Wizard](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Klik in de lijst **Cloudservices** door **Nieuw**te selecteren. Het dialoogvenster **Azure-Services maken** wordt weergegeven.

1. Voer de naam van uw cloudservice. De naam deel uitmaakt van de URL van uw service en daarom moet uniek zijn. De naam is niet hoofdlettergevoelig.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Een cloudservice maken met behulp van de Azure klassieke portal

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkId=253103) op de website van Microsoft.

1. (optioneel) Als u wilt weergeven in een lijst met cloudservices die u al hebt gemaakt, kies de koppeling Cloudservices aan de linkerkant van de pagina.

1. Kies de **+** pictogram in de linkerbenedenhoek linksboven en kies vervolgens **Cloudservice** in het menu dat wordt weergegeven. Een ander scherm met twee opties voor **Snelle maken** en **Aangepaste maken**, wordt weergegeven. Als u **Snelle maken kiest**, kunt u een cloudservice maken door alleen te geven van de URL en de regio waar deze fysiek ondergebracht. Als u **Aangepaste maken**, kunt u direct een cloudservice publiceren door het opgeven van een pakket (.cspkg-bestand), een configuratiebestand (.cscfg) en een certificaat. Aangepaste maken is niet vereist als u publiceren van uw cloudservice wilt met behulp van de opdracht **publiceren** in een Azure-project. De opdracht **publiceren** is beschikbaar op het snelmenu voor een Azure-project.

1. Kies **Snelle maken** uw cloudservice later te publiceren met behulp van Visual Studio.

1. Geef een naam voor uw cloudservice. De complete URL wordt weergegeven naast de naam.

1. Kies de regio waar de meeste gebruikers zich bevinden in de lijst.

1. Kies onder aan het venster en de koppeling **Cloudservice maken** .

## <a name="create-a-storage-account"></a>Een opslag-account maken

Een account opslagruimte biedt toegang tot de services Blob, wachtrij en tabel. U kunt een opslag-account maken met behulp van Visual Studio of de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Een opslag-account maken met behulp van Visual Studio

1. In **Solution Explorer**, opent u het snelmenu voor het knooppunt **opslag** , en kies vervolgens **Opslag-Account maken**.

    ![Een nieuw Azure opslag-account maken](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Selecteer of Voer de volgende gegevens voor de nieuwe opslag-account in het dialoogvenster **Opslag-Account maken** .
    - Het Azure abonnement waarnaar u wilt het opslag-account toevoegen.
    - De naam die u wilt gebruiken voor de nieuwe opslag-account.
    - De regio of affiniteit groep (zoals West Amerikaans of Oost-Azië).
    - Het type herhaling die u wilt gebruiken voor de opslag-account, zoals geografische-redundante.

1. Wanneer u klaar bent, kiest u **maken**. Het nieuwe opslag-account wordt weergegeven in de lijst **opslagruimte** in **Server Explorer**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Een opslag-account maken met behulp van de Azure klassieke portal

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkId=253103) op de website van Microsoft.

1. (Optioneel) Als wilt bekijken uw accounts opslag, kiest u de koppeling **opslagruimte** in het deelvenster aan de linkerkant van de pagina.

1. Kies in de linkerbenedenhoek van de pagina, de **+** pictogram.

1. In het menu dat wordt weergegeven, kiest u **opslagruimte**en kiest u **Snelle maken**.

1. Geef een naam die zijn ingevoegd om een unieke url voor de opslag-account.

1. Geef uw cloudservice een naam. De complete URL wordt weergegeven naast de naam.

1. Kies in de lijst met rayons, een gebied waar de meeste gebruikers zich bevinden.

1. Opgeven of u wilt inschakelen geografische-herhaling. Als u geografische herhaling inschakelt, wordt uw gegevens worden opgeslagen in meerdere fysieke locaties om te voorkomen dat verlies. Deze functie kunt u opslagruimte meer dure, maar kunt u de kosten verkleinen doordat geografische locatie wanneer u het account opslagruimte in plaats van de functie later toevoegen. Zie [geografische-replicatie](http://go.microsoft.com/fwlink/?LinkId=253108)voor meer informatie.

1. Onder aan het venster en kies de koppeling **Opslag-Account maken** .

Nadat u uw opslagruimte-account hebt gemaakt, ziet u de URL's die u gebruiken kunt voor toegang tot bronnen in elk van de Azure storage-services, en ook de primaire en secundaire toegangstoetsen voor uw account. U deze toetsen gebruiken om te verifiëren aanvragen ten opzichte van de storage-services.

>[AZURE.NOTE] De secundaire toegangstoets biedt dezelfde toegang tot uw account opslag als primaire toegangstoets en is gegenereerd als een back-up moet uw primaire toegangstoets worden geknoeid. Bovendien is het aanbevolen dat u opnieuw uw toegangstoetsen regelmatig te genereren. Een instelling van de tekenreeks verbinding met de secundaire toets terwijl u de primaire sleutel genereren en vervolgens u wijzigen kunt, zodat de geregenereerde primaire sleutel gebruiken terwijl u de secundaire sleutel genereren, kunt u wijzigen.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Uw app als u wilt gebruiken services van het opslag-account configureren

Een functie die toegang heeft tot storage-services als u wilt gebruiken van de Azure storage-services die u hebt gemaakt, moet u configureren. Hiervoor kunt u meerdere configuraties voor uw Azure project. Standaard worden twee gemaakt in uw Azure project. Met behulp van meerdere configuraties, kunt u gebruikmaken van dezelfde verbindingsreeks in code, maar hebben een andere waarde voor een verbindingsreeks in elke service te configureren. U kunt bijvoorbeeld een service te configureren uit te voeren en fouten opsporen in uw toepassing lokaal via de emulator Azure opslag en een andere service te configureren voor het publiceren van uw toepassing Azure gebruiken. Zie voor meer informatie over configuraties [Configureren van uw Azure Project gebruik van meerdere configuraties](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Uw toepassing services vindt u de opslag-account configureren

1. Open uw Azure-oplossing in Visual Studio. Open het snelmenu voor elke rol in uw Azure project die toegang heeft tot de services opslagruimte in Solution Explorer en kies **Eigenschappen**. Een pagina met de naam van de rol wordt weergegeven in de Visual Studio-editor. De pagina bevat de velden voor het tabblad **configuratie** .

1. Kies in de eigenschappenvensters voor de rol, **Instellingen**.

1. Kies de naam van de configuratie van de service die u wilt bewerken in de lijst **Service te configureren** . Als u wijzigingen aanbrengen op alle van de serviceconfiguraties voor deze rol wilt, kunt u **Alle configuraties**kiezen.  Zie het gedeelte **Tekenreeksen met de verbinding beheren voor opslag-Accounts** in het onderwerp [configureren de rollen voor een Azure-Cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)voor meer informatie over het bijwerken van configuraties.

1. Als u wilt wijzigen van een tekenreeks verbindingsinstellingen, kiest u de **…** knop naast de instelling. Het dialoogvenster **Opslag verbindingsreeks maken** wordt weergegeven.

1. Kies onder **Verbinden via**, de optie **uw abonnement** .

1. Klik in de lijst **abonnement** kiest u uw abonnement. Als de lijst met abonnementen niet de fase die u wilt opnemen, kiest u de koppeling **Publish Settings downloaden** .

1. Kies uw opslagaccountnaam in de lijst **naam van het Account** . Azure extra wordt opslag-accountreferenties automatisch opgehaald met behulp van het bestand .publishsettings. De referenties van uw opslag-account als handmatig wilt opgeven, kies de optie **handmatig ingevoerd referenties** en gaat u verder met deze procedure. U kunt uw opslagaccountnaam en de primaire sleutel krijgen van de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885). Als u niet wilt opgeven van uw opslag accountinstellingen handmatig, kies de knop **OK** om het dialoogvenster te sluiten.

1. Kies de koppeling van de referenties **Enter opslag-account** .

1. Voer de naam van uw account opslagruimte in het vak **naam van het Account** .

    >[AZURE.NOTE] Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)en kies vervolgens de knop **opslag** . De portal bevat een overzicht van opslag-accounts. Als u een account kiest, wordt een pagina voor deze wordt geopend. U kunt de naam van het account opslag kopiëren van deze pagina. Als u een eerdere versie van de klassieke portal gebruikt, is de naam van uw account opslag wordt weergegeven in de weergave voor de **Opslag-Accounts** . Als u wilt kopiëren van deze naam, markeert u deze in het venster **Eigenschappen** van deze weergave en kies vervolgens de toetsen Ctrl + C. Plak de naam in Visual Studio, kiest u het tekstvak **naam van het Account** en kies vervolgens de toetsen Ctrl + V.

1. In het vak **accountsleutel** uw primaire sleutel, typ of kopieer en plak deze in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).
    Deze toets kopiëren:

    1. Onderaan op de pagina voor het juiste opslag-account, klikt u op de knop **Sleutels beheren** .

    1. Klik op de pagina **Toegang tot het toetsen beheren** selecteert u de tekst van de primaire toegangstoets en kiest u de toetsen Ctrl + C.

    1. Plak de toets in het vak **accountsleutel** in hulpmiddelen voor Azure.

    1. U moet een van de volgende opties om te bepalen hoe de service wordt toegang tot het account opslagruimte selecteren:
        - **Gebruik HTTP**. Dit is de optie standaard. Bijvoorbeeld `http://<account name>.blob.core.windows.net`.
        - **Gebruik HTTPS** voor een beveiligde verbinding. Bijvoorbeeld `https://<accountname>.blob.core.windows.net`.
        - **Aangepaste eindpunten opgeven** voor elk van de drie services. U kunt deze eindpunten naar het veld voor de specifieke service typt.

        >[AZURE.NOTE] Als u aangepaste eindpunten maakt, kunt u een complexere verbindingsreeks. Wanneer u deze tekenreeksindeling gebruikt, kunt u opslagruimte service eindpunten die een aangepaste domeinnaam die u hebt geregistreerd voor uw account opslagruimte met de service Blob bevatten. Ook kunt u alleen toegang tot blob resources in een container één tot en met een handtekening gedeelde toegang verlenen. Zie voor meer informatie over het maken van aangepaste eindpunten [Verbindingsreeks van de Azure-opslag configureren](storage-configure-connection-string.md).

1. Klik op **OK** om deze verbinding tekenreeks wijzigingen op te slaan en kies vervolgens de knop **Opslaan** op de werkbalk. Nadat u deze wijzigingen hebt opgeslagen, kunt u de waarde van deze verbindingsreeks in code opvragen met behulp van [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Wanneer u de toepassing Azure publiceert, kiest u de configuratie van de service met het account Azure opslag voor de verbindingsreeks. Nadat uw toepassing wordt gepubliceerd, controleert u of of de toepassing werkt zoals verwacht ten opzichte van de Azure storage-services

## <a name="next-steps"></a>Volgende stappen

Zie meer informatie over het publiceren van apps op Azure van Visual Studio, [publiceren van een Cloudservice met de hulpmiddelen Azure](vs-azure-tools-publishing-a-cloud-service.md).
