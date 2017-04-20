<properties 
   pageTitle="Beveiligen van gegevens die zijn opgeslagen in de gegevensopslag Lake Azure | Microsoft Azure" 
   description="Meer informatie over het beveiligen van gegevens in Azure Lake gegevensopslag met groepen en toegangsbeheerlijsten" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Gegevens die zijn opgeslagen in de gegevensopslag Lake Azure beveiligen

Beveiligen van gegevens in de gegevensopslag Lake Azure is een benadering van de drie stappen.

1. Beginnen met het maken van beveiligingsgroepen in Azure Active Directory (AAD). Deze beveiligingsgroepen worden gebruikt voor het implementeren van Rolgebaseerd toegangsbeheer (RBAC) in Azure-Portal. Zie [Toegangsbeheer op basis van rollen in Microsoft Azure](../active-directory/role-based-access-control-configure.md)voor meer informatie.

2. De beveiligingsgroepen AAD toewijzen aan het account Azure Lake gegevensopslag. Hiermee bepaalt u toegang aan het account voor gegevensopslag Lake uit de bewerkingen voor portal en beheer van de portal of API's.

3. De beveiligingsgroepen AAD als access toegangsbeheerlijsten (ACL's) in het bestandssysteem Lake gegevensopslag toewijzen.

4. Bovendien kunt u ook een IP-adresbereiken instellen voor clients die toegang heeft tot de gegevens in Lake gegevensopslag.

In dit artikel bevat instructies over het gebruik van de Azure portal de bovenstaande taken uitvoeren. Zie voor uitgebreide informatie over hoe Lake gegevensopslag beveiliging op het niveau van de account en gegevens implementeert, [beveiliging in Azure Lake gegevensopslag](data-lake-store-security-overview.md). Zie [Overzicht van toegangsbeheer in Lake gegevensopslag](data-lake-store-access-control.md)voor uitvoerige informatie over hoe ACL's worden geïmplementeerd in Azure Lake gegevensopslag.

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- **Een Azure Lake gegevensopslag-account**. Zie voor instructies over hoe u een account maakt, [aan de slag met Azure Lake gegevensopslag](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Beveiligingsgroepen maken in Azure Active Directory

Zie voor instructies over het maken van AAD beveiligingsgroepen en hoe u gebruikers toevoegt aan de groep, [beveiligingsgroepen beheren in Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Toewijzen aan gebruikers of beveiligingsgroepen Azure Lake gegevensopslag accounts

Wanneer u gebruikers of beveiligingsgroepen op aan Azure Lake gegevensopslag accounts toewijst, kunt u toegang tot de bewerkingen management op het gebruik van de Azure-portal en Azure resourcemanager-API's account beheren. 

1. Open een Azure Lake gegevensopslag-account. In het linkerdeelvenster, klikt u op **Bladeren**, klikt u op **Meer gegevensopslag**en klik vervolgens op het blad Lake gegevensopslag op de naam van het account waaraan u wilt toewijzen van een groep gebruikers of beveiligingsgroepen.

2. In uw account Lake gegevensopslag blade, klikt u op **Instellingen**. Klik op het blad **Instellingen** op **gebruikers**.

    ![Beveiligingsgroep met Azure Lake gegevensopslag account toewijzen] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Beveiligingsgroep met Azure Lake gegevensopslag account toewijzen")

3. Het blad **gebruiker** al dan niet standaard bevat **abonnement** beheerdersgroep als eigenaar. 

    ![Gebruikers toevoegen en rollen] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Gebruikers toevoegen en rollen")
 
    Er zijn twee manieren een groep toevoegen en relevante rollen toewijzen.

    * Een gebruiker/groep toevoegen aan het account en vervolgens toewijzen een rol, of
    * Voeg een rol en vervolgens gebruikers/groepen toewijzen aan rol.

    In dit gedeelte kijken we naar de eerste methode, een groep toe te voegen en klik vervolgens rollen toewijzen. U kunt dezelfde stappen om te selecteert u eerst een rol en vervolgens groepen toewijzen aan die rol uitvoeren.
    
4. Klik in het blad **gebruikers** op **toevoegen** om te openen van het blad **toevoegen access** . Klik in het blad **toevoegen access** op **Selecteer een rol**en selecteer een rol voor de gebruiker/groep.

     ![Een rol voor de gebruiker toevoegen] (./media/data-lake-store-secure-data/adl.add.user.1.png "Een rol voor de gebruiker toevoegen")

    De **eigenaar** en de rol **Inzender** bieden toegang tot een aantal functies voor beheer op het account van de lake gegevens. Voor gebruikers die wordt werken met gegevens in het meer gegevens, kunt u deze toevoegen aan de rol van de **lezer **. Het bereik van deze rollen is beperkt tot de management bewerkingen die betrekking hebben op het account Azure Lake gegevensopslag.

    Voor gegevens definiëren afzonderlijke machtigingen bestandssysteem bewerkingen wat de gebruikers kunnen doen. Daarom een gebruiker met een lezersrol kan alleen bekijken beheerinstellingen die is gekoppeld aan het account, maar kunt mogelijk gegevens lezen en schrijven op basis van de toegewezen bestandssysteemmachtigingen. Machtigingen voor gegevensopslag Lake bestandssysteem worden beschreven bij het [toewijzen van de beveiligingsgroep als ACL's naar het bestandssysteem Azure Lake gegevensopslag](#filepermissions).



5. Klik in het blad **toevoegen access** op **gebruikers toevoegen** om te openen van het blad **gebruikers toevoegen** . In dit blade, zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Als u een groot aantal groepen om over te zoeken op hebt, gebruikt u het tekstvak aan de bovenkant om te filteren op de naam van de groep. Klik op **selecteren**.

    ![Een beveiligingsgroep toevoegen] (./media/data-lake-store-secure-data/adl.add.user.2.png "Een beveiligingsgroep toevoegen")

    Als u toevoegen van een groep/de gebruiker die niet wordt vermeld wilt, kunt u ze kunt uitnodigen door het pictogram **uitnodigen** en geven van het e-mailadres voor de gebruiker/groep.

6. Klik op **OK**. Hier ziet u de beveiligingsgroep die is toegevoegd, zoals hieronder wordt weergegeven.

    ![Beveiligingsgroep toegevoegd] (./media/data-lake-store-secure-data/adl.add.user.3.png "Beveiligingsgroep toegevoegd")

7. De gebruiker/groep heeft nu toegang tot het account Azure Lake gegevensopslag. Als u toegang verlenen aan specifieke gebruikers wilt, kunt u deze toevoegen aan de beveiligingsgroep. Op dezelfde manier als u toegang voor een gebruiker intrekken wilt, kunt u deze uit de beveiligingsgroep. U kunt ook meerdere beveiligingsgroepen toewijzen aan een account. 

## <a name="filepermissions"></a>Gebruikers of beveiligingsgroep als ACL's toewijzen aan het bestandssysteem Azure Lake gegevensopslag

Gebruikers/beveiligingsgroepen aan het bestandssysteem Azure gegevens Lake toewijst, moet u toegangsbeheer voor de gegevens die zijn opgeslagen in de gegevensopslag Lake Azure instellen.

1. Klik in uw account Lake gegevensopslag blade, op **Data Explorer**.

    ![Mappen maken in Lake gegevensopslag-account] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Mappen maken in gegevens Lake-account")

2. In het blad **Data Explorer** op het bestand of map waarvoor u wilt de ACL configureren en klik vervolgens op **toegang**. ACL toewijzen aan een bestand, klik u op **toegang** van het blad **PQ-update** .

    ![ACL's instellen op gegevens Lake bestandssysteem] (./media/data-lake-store-secure-data/adl.acl.1.png "ACL's instellen op gegevens Lake bestandssysteem")

3. Het **Access** -blad bevat de standaard toegang en aangepaste access al is toegewezen in de hoofdmap. Klik op het pictogram **toevoegen** om toe te voegen ACL's aangepast niveau.

    ![Lijst met standaardkleuren of aangepaste access] (./media/data-lake-store-secure-data/adl.acl.2.png "Lijst met standaardkleuren of aangepaste access")

    * De toegang UNIX-stijl **standaard toegang tot** is waar u lezen, schrijven, uitvoeren (rwx) naar drie afzonderlijke gebruikersklassen: eigenaar, groep en anderen.
    * **Aangepaste access** overeenkomt met de POSIX ACL's waarmee u kunt machtigingen instellen voor specifieke benoemde gebruikers of groepen, en niet alleen van het bestand eigenaar of groep. 
    
    Zie [HDFS ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)voor meer informatie. Zie voor meer informatie over hoe ACL's worden geïmplementeerd in Lake gegevensopslag [Toegangsbeheer in Lake gegevensopslag](data-lake-store-access-control.md).

4. Klik op het pictogram **toevoegen** om het openen van het blad **Aangepaste Access toevoegen** . In deze blade, klikt u op **gebruiker of groep selecteren**en klik vervolgens in de **gebruiker of groep selecteren** blade, zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Als u een groot aantal groepen om over te zoeken op hebt, gebruikt u het tekstvak aan de bovenkant om te filteren op de naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **selecteren**.

    ![Een groep toevoegen] (./media/data-lake-store-secure-data/adl.acl.3.png "Een groep toevoegen")

5. Klik op **Machtigingen selecteren**, selecteert u de machtigingen en of u de machtigingen toewijzen standaard ACL wilt, toegang heeft tot ACL of beide. Klik op **OK**.

    ![Machtigingen toewijzen aan een groep] (./media/data-lake-store-secure-data/adl.acl.4.png "Machtigingen toewijzen aan een groep")

    Zie voor meer informatie over machtigingen in Lake gegevensopslag en standaard/Access ACL's [Toegangsbeheer in Lake gegevensopslag](data-lake-store-access-control.md).


6. Klik op **OK**in het blad **Aangepaste Access toevoegen** . De toegevoegde groep met de bijbehorende machtigingen worden nu worden vermeld in het **Access** -blad.

    ![Machtigingen toewijzen aan een groep] (./media/data-lake-store-secure-data/adl.acl.5.png "Machtigingen toewijzen aan een groep")

    > [AZURE.IMPORTANT] U kunt alleen 9 vermeldingen onder **Aangepast toegang**hebben in de huidige versie. Als u wilt meer dan 9 gebruikers toevoegen, moet u beveiligingsgroepen kan maken gebruikers toevoegen aan beveiligingsgroepen, voegt u zorgen voor toegang tot deze beveiligingsgroepen voor het account voor gegevensopslag Lake.

7. Indien nodig, kunt u ook de toegangsmachtigingen wijzigen nadat u de groep hebt toegevoegd. Schakel het selectievakje in voor elk machtigingstype (lezen, schrijven, uitvoeren) op basis van of u wilt verwijderen of deze machtiging toewijzen aan de beveiligingsgroep of uit. Klik op **Opslaan** de wijzigingen op te slaan of op **negeren** om de wijzigingen ongedaan te maken.

## <a name="set-ip-address-range-for-data-access"></a>IP-adresbereiken voor toegang tot gegevens instellen

Azure Lake gegevensopslag kunt u verdere vergrendelen toegang tot uw gegevensopslag op netwerkniveau. U kunt firewall, Geef een IP-adres, of een IP-adresbereiken voor uw vertrouwde clients definiëren. Zodra ingeschakeld, wordt alleen-clients die door het IP-in gedefinieerde bereik adressen kunnen koppelen aan de store.

![Firewallinstellingen en IP-toegang] (./media/data-lake-store-secure-data/firewall-ip-access.png "Firewallinstellingen en IP-adres")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Beveiligingsgroepen voor een Azure Lake gegevensopslag-account verwijderen

Wanneer u beveiligingsgroepen uit de gegevensopslag Lake Azure-accounts verwijderen, wijzigt u alleen toegang in de management bewerkingen in het account via de Portal van Azure en Azure resourcemanager-API's.

1. In uw account Lake gegevensopslag blade, klikt u op **Instellingen**. Klik op het blad **Instellingen** op **gebruikers**.

    ![Beveiligingsgroep met Azure gegevens Lake account toewijzen] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Beveiligingsgroep met Azure gegevens Lake account toewijzen")

2. Klik in het blad **gebruikers** op de beveiligingsgroep die u wilt verwijderen.

    ![Beveiligingsgroep verwijderen] (./media/data-lake-store-secure-data/adl.add.user.3.png "Beveiligingsgroep verwijderen")

3. Klik in het blad voor de beveiligingsgroep, op **verwijderen**.

    ![Beveiligingsgroep verwijderd] (./media/data-lake-store-secure-data/adl.remove.group.png "Beveiligingsgroep verwijderd")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Beveiligingsgroep ACL's verwijderen uit het bestandssysteem Azure Lake gegevensopslag

Wanneer u beveiligingsgroepen ACL's uit de gegevensopslag Lake Azure-bestandssysteem verwijderen, moet u toegang tot de gegevens in de Lake gegevensopslag wijzigen.

1. Klik in uw account Lake gegevensopslag blade, op **Data Explorer**.

    ![Mappen maken in gegevens Lake-account] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Mappen maken in gegevens Lake-account")

2. In het blad **Data Explorer** op het bestand of map waarvoor u wilt de ACL verwijderen en klik vervolgens in uw account bladeserver, op het pictogram van **Access** . Als u wilt verwijderen ACL voor een bestand, klik u op **toegang** van het blad **PQ-update** .

    ![ACL's instellen op gegevens Lake bestandssysteem] (./media/data-lake-store-secure-data/adl.acl.1.png "ACL's instellen op gegevens Lake bestandssysteem")

3. Klik in het blad **Access** , klikt u in de sectie met **Aangepaste Access** op de beveiligingsgroep die u wilt verwijderen. In het blad **Aangepaste Access** , klikt u op **verwijderen** en klik vervolgens op **OK**.

    ![Machtigingen toewijzen aan een groep] (./media/data-lake-store-secure-data/adl.remove.acl.png "Machtigingen toewijzen aan een groep")


## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md)
- [Gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag](data-lake-store-copy-data-azure-storage-blob.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Aan de slag met Lake gegevensopslag via PowerShell](data-lake-store-get-started-powershell.md)
- [Aan de slag met Lake gegevensopslag met .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Diagnostische logboeken toegang voor gegevensopslag Lake](data-lake-store-diagnostic-logs.md)
