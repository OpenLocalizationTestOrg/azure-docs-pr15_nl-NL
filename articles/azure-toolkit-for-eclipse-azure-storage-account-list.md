<properties
    pageTitle="Lijst met accounts Azure opslag"
    description="Uw accountinstellingen voor opslagruimte met behulp van de Azure-Toolkit voor Eclips beheren"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Lijst met accounts Azure opslag #

Accounts van Azure opslag inschakelen downloadlocaties moet worden gebruikt voor uw JDK, toepassingsserver en willekeurige onderdelen en voor het opslaan van status bij gebruik van de cache opslaan. Eclips onderhoudt een lijst met bekende opslag-accounts die beschikbaar voor uw projecten in uw werkruimte Eclips zijn. Als u wilt openen van het dialoogvenster **Accounts van de opslag** , die wordt gebruikt voor het beheren van die lijst, binnen Eclips, klik op **venster**, op **Voorkeuren**, **Azure**uitvouwen en klik vervolgens op **Opslag-Accounts**.

Hieronder ziet u het dialoogvenster **Accounts van opslag** .

![][ic719496]

Dit dialoogvenster u kunt ook openen vanuit een koppeling **Accounts** in dialoogvensters die gebruikmaken van opslag-accounts, zoals de volgende:

* Het tabblad **JDK** van het dialoogvenster **Serverconfiguratie** .
* Het tabblad **Server** in het dialoogvenster **Serverconfiguratie** .
* Het dialoogvenster **Component toevoegen** .
* Het eigenschappenvenster van de **cache** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Uw opslag-accounts met een publiceren instellingen-bestand importeren ##

1. In het dialoogvenster **Accounts van opslag** , klikt u op **importeren uit bestand publiceren-instellingen**.
2. (Deze stap overslaan als u een instellingenbestand publiceren al hebt opgeslagen naar uw lokale computer). Klik op **Publiceren-instellingen-bestand downloaden**in het dialoogvenster **Gegevens importeren-abonnement** . Als u uw Azure-account nog niet bent aangemeld, wordt u gevraagd aan te melden. Vervolgens wordt u gevraagd om te publiceren opslaan een Azure instellingenbestand. (U kunt de resulterende instructies weergegeven op de pagina aanmelden's - negeren ze worden verstrekt door de Azure-portal en zijn bedoeld voor Visual Studio-gebruikers.) Sla deze op uw lokale computer.
3. Nog steeds in het dialoogvenster **Importgegevens abonnement** klikt u op de knop **Bladeren** , selecteer het publiceren instellingen-bestand dat u eerder lokaal opgeslagen en klik vervolgens op **openen**.
4. Klik op **OK** om te sluiten van het dialoogvenster **Gegevens importeren-abonnement** .

## <a name="to-create-a-new-storage-account"></a>Een nieuwe opslag-account maken ##

1. In het dialoogvenster **Accounts van opslag** , klikt u op **toevoegen**.
2. Klik op **Nieuw**in het dialoogvenster **Opslag-Account toevoegen** .
3. Geef in het dialoogvenster **Nieuw Account voor opslag** waarden voor de volgende opties:
    * Opslagaccountnaam.
    * Locatie van het account opslag.
    * Beschrijving van de opslag-account.
    * Het abonnement die het opslag-account hoort.
4. Klik op **OK** om te sluiten van het dialoogvenster **Nieuwe opslag-Account** .

Het kan enkele minuten duren voor uw account opslag moet worden gemaakt. Nadat deze is gemaakt, klikt u op **OK** om te sluiten van het dialoogvenster **Opslag-Account toevoegen** en uw nieuwe opslag-account, worden toegevoegd aan de lijst met beschikbare opslagruimte accounts.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Een bestaand opslag-account toevoegen aan de lijst ##

1. Als u nog een Azure opslag-account, een volgens de stappen in het **maken van een nieuwe sectie voor de opslag-account** boven maken. (U kunt ook kunt u een nieuw account voor de opslag in de [Beheerportal van Azure][].)
2. In het dialoogvenster **Accounts van opslag** , klikt u op **toevoegen**.
3. Voer de waarden voor de **naam** en **Toegangstoets**in het dialoogvenster **Opslag-Account toevoegen** . De naam en access accountsleutel moet zijn voor een bestaande Azure opslag-account. Met de sectie **opslag** van de [Azure Management Portal][] kunt u uw opslagruimte accountnamen en toetsen. Uw **Opslag-Account toevoegen** -dialoogvenster ziet er ongeveer als volgt uit.

    ![][ic719497]

4. Klik op **OK** om het dialoogvenster **Opslag-Account toevoegen** te sluiten.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Voor het wijzigen van een opslag-account als u wilt een nieuwe access-sleutel gebruiken ##

1. In het dialoogvenster **Accounts van opslag** , klikt u op de opslag-account dat u wilt bewerken en klik vervolgens op **bewerken**.
2. In het dialoogvenster **Toegangstoets van de opslag-Account bewerken** door de waarde **Toegangstoets** te wijzigen.
3. Klik op **OK** om het dialoogvenster **Toegangstoets van de opslag-Account bewerken** te sluiten.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Een account opslagruimte verwijderen uit de lijst onderhouden in Eclips ##

1. In het dialoogvenster **Accounts van opslag** , klikt u op de opslag-account dat u wilt bewerken en klik vervolgens op **verwijderen**.
2. Klik op **OK** wanneer u wordt gevraagd om de opslag-account te verwijderen.

>[AZURE.NOTE] De opslag-account via het dialoogvenster **Accounts van opslagruimte** verwijderen alleen verwijderd uit de lijst met accounts van opslagruimte weergeven in Eclips. Deze bevat de opslag-account niet verwijderen uit uw abonnement Azure. Bovendien kan het opslag-account opnieuw zichtbaar in de lijst met nadat Eclips de details van uw abonnement formuliergebieden.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
