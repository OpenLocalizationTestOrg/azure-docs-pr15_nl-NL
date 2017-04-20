<properties
    pageTitle="Aan de slag met de gegevenscatalogus | Microsoft Azure"
    description="End-to-end zelfstudie presenteren de scenario's en de mogelijkheden van Azure-gegevenscatalogus."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Aan de slag met Azure-gegevenscatalogus
Azure gegevenscatalogus is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise gegevens activa fungeert. Zie [Wat Azure-gegevenscatalogus is](data-catalog-what-is-data-catalog.md)voor een gedetailleerd overzicht.

Deze zelfstudie kunt u aan de slag met Azure-gegevenscatalogus. U kunt de volgende procedures uitvoeren in deze zelfstudie:

| Procedure | Beschrijving |
| :--- | :---------- |
| [Gegevenscatalogus inrichten](#provision-data-catalog) | In deze procedure inrichten of instellen van Azure-gegevenscatalogus. U doen deze stap alleen als de catalogus voordat niet is ingesteld. U kunt slechts één gegevenscatalogus per organisatie (Microsoft Azure Active Directory-domein) hebben, zelfs als er meerdere abonnementen die is gekoppeld aan uw Azure-account. |
| [Register gegevens activa](#register-data-assets) | In deze procedure kunt u gegevens activa van de voorbeelddatabase AdventureWorks2014 registreren met de gegevenscatalogus. Registratie is het proces van belangrijke structurele metagegevens, zoals namen, typen en locaties uitpakken uit de gegevensbron en metagegevens kopiëren naar de catalogus. De gegevensbron en gegevens activa blijven waar ze zijn, maar de metagegevens wordt gebruikt door de catalogus zodat u ze gemakkelijker makkelijker kan worden gevonden en te begrijpen. |
| [Kennismaken met gegevens activa](#discover-data-assets) | In deze procedure kunt u de portal Azure-gegevenscatalogus gebruiken om te ontdekken gegevens activa die zijn geregistreerd in de vorige stap. Nadat een gegevensbron met de gegevenscatalogus Azure is geregistreerd, wordt de bijbehorende metagegevens zodat gebruikers kunnen gemakkelijk de benodigde gegevens zoeken door de service geïndexeerd. |
| [Gegevens activa van aantekeningen voorzien](#annotate-data-assets) | In deze procedure kunt u aantekeningen (toegangsgegevens voor de beschrijvingen, labels, documentatie of experts) opgeeft voor de activa gegevens. Deze informatie is een aanvulling op de metagegevens geëxtraheerd uit de gegevensbron en om de gegevensbron begrijpelijker aan meer personen. |
| [Verbinding maken met gegevens activa](#connect-to-data-assets) | U opent in deze procedure gegevens activa in geïntegreerde clienthulpprogramma's (zoals Excel en SQL Server Data Tools) en een niet-geïntegreerde hulpmiddel (SQL Server Management Studio). |
| [Gegevens activa beheren](#manage-data-assets) | In deze procedure kunt instellen u beveiliging voor de activa van uw gegevens. Gegevenscatalogus wordt niet gebruikers toegang geven tot de gegevens zelf. Toegang tot gegevens Hiermee stelt u de eigenaar van de gegevensbron. <br/><br/> U kunt gegevensbronnen en weergave de **metagegevens** die zijn gerelateerd aan de bronnen die zijn geregistreerd in de catalogus ontdekken gegevenscatalogus ziet. Er zijn situaties, echter waar gegevensbronnen weergegeven alleen bepaalde gebruikers of groepen worden moeten. Voor deze scenario's, kunt u gegevenscatalogus eigenaar van de activa geregistreerde gegevens binnen de catalogus en beheert de zichtbaarheid van de activa dat u eigenaar bent. |
| [Elementen van de gegevens verwijderen](#remove-data-assets) | In deze procedure leert u hoe gegevens activa verwijdert uit de gegevenscatalogus. |  

## <a name="tutorial-prerequisites"></a>Zelfstudievideo vereisten

### <a name="azure-subscription"></a>Azure-abonnement
Als u wilt de gegevenscatalogus Azure hebt ingesteld, moet u de eigenaar of de mede-eigenaar van een abonnement op Azure.

Azure abonnementen waarmee u toegang tot cloud serviceresources zoals Azure-gegevenscatalogus kunt organiseren. Ze ook help u hoe de weergave Resourcegebruik wordt aangegeven bepalen, gefactureerd en dat is betaald. Elk abonnement kan verschillende facturering en betalingsgegevens instellingen, hebben, zodat u kunt verschillende-abonnementen en andere abonnementen door afdeling, project, regionale office en dergelijke hebben. Elke cloudservice behoort tot een abonnement, en moet u beschikken over een abonnement vóór het instellen van Azure-gegevenscatalogus. Zie [accounts beheren, abonnementen, en beheerderrollen](../active-directory/active-directory-how-subscriptions-associated-directory.md)meer informatie.

Als u niet een abonnement hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

### <a name="azure-active-directory"></a>Azure Active Directory
Als u wilt de gegevenscatalogus Azure hebt ingesteld, moet u zich aangemeld met een gebruikersaccount Azure Active Directory (Azure AD). U moet de eigenaar of de mede-eigenaar van een abonnement op Azure.  

Azure AD biedt een eenvoudige manier om uw bedrijf voor het beheren van identiteit en open, zowel in de cloud en on-premises implementatie. U kunt één werk- of schoolaccount te melden bij een webtoepassing cloud of on-premises implementatie. Azure AD Azure gegevenscatalogus gebruikt om te verifiëren aanmelden. Zie [Wat Azure Active Directory is](../active-directory/active-directory-whatis.md)meer informatie.

### <a name="azure-active-directory-policy-configuration"></a>Configuratie van Azure Active Directory-beleid

U kunt een situatie kan optreden wanneer u zich kunt aanmelden bij de portal Azure-gegevenscatalogus, maar wanneer u probeert te melden bij het hulpprogramma voor de registratie gegevens, er een foutbericht wordt weergegeven dat u niet aanmelden. Deze fout kan optreden wanneer u zich in het netwerk van het bedrijf of wanneer u verbinding van buiten het bedrijfsnetwerk maakt.

De registratie-hulpmiddel maakt gebruik van *formulierverificatie* voor het valideren van gebruiker aanmeldingen ten opzichte van de Azure Active Directory. Voor een succesvolle aanmelden, moet de beheerder van de Azure Active Directory formulierverificatie in het *beleid van de globale verificatie*inschakelen.

Het beleid globale verificatie kunt u de verificatie voor afzonderlijk intranet en extranet verbindingen, zoals wordt weergegeven in de volgende afbeelding. Aanmeldingsproblemen oplossen kunnen optreden als formulierverificatie is niet ingeschakeld voor het netwerk van waaruit u verbinding maakt.

 ![Azure Active Directory-beleid voor globale verificatie](./media/data-catalog-prerequisites/global-auth-policy.png)

Zie [beleidsregels voor verificatie configureren](https://technet.microsoft.com/library/dn486781.aspx)voor meer informatie.

## <a name="provision-data-catalog"></a>Gegevenscatalogus inrichten
U kunt slechts één gegevenscatalogus per organisatie (Azure Active Directory-domein) inrichten. Daarom als een catalogus al door de eigenaar of de mede-eigenaar van een abonnement op Azure die deel uitmaakt van dit Azure Active Directory-domein gemaakt heeft, is niet mogelijk opnieuw moet maken een catalogus zelfs als u meerdere Azure abonnementen hebt. Als u wilt testen of een gegevenscatalogus is gemaakt door een gebruiker in uw Azure Active Directory-domein, gaat u naar de [startpagina van de catalogus van Azure-gegevens](http://azuredatacatalog.com) en controleer of of u de catalogus zien. Als een catalogus al voor u is gemaakt, gaat u verder met de volgende procedure en Ga naar het volgende gedeelte.    

1. Ga naar de [pagina van de service gegevenscatalogus](https://azure.microsoft.com/services/data-catalog) en klik **aan de slag**.

    ![Azure gegevenscatalogus--marketing aantekening toevoegen](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Zich aanmelden met een gebruikersaccount dat de eigenaar of de mede-eigenaar van een abonnement op Azure. U ziet de volgende pagina na aanmelden.

    ![Azure gegevenscatalogus--gegevenscatalogus inrichten](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Geef een **naam** voor de catalogus met gegevens, het **abonnement** dat u wilt gebruiken en de **locatie** voor de catalogus.
4. Vouw van **prijzen** en selecteer een Azure-gegevenscatalogus **edition** (gratis of standaard).
    ![Azure gegevenscatalogus--select-editie](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Vouw **Gebruikers catalogus** en klik op **toevoegen** als gebruikers voor de catalogus met gegevens wilt toevoegen. U wordt automatisch toegevoegd aan deze groep.
    ![Azure gegevenscatalogus--gebruikers](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Vouw **Beheerders van de catalogus** en klik op **toevoegen** om extra beheerders van de gegevenscatalogus. U wordt automatisch toegevoegd aan deze groep.
    ![Azure gegevenscatalogus--beheerders](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klik op **Catalogus maken** als u wilt maken van de gegevenscatalogus voor uw organisatie. U kunt de introductiepagina van de gegevenscatalogus ziet nadat deze is gemaakt.
    ![Azure gegevenscatalogus--gemaakt](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Een gegevenscatalogus vinden in de portal van Azure
1. Klik op een tabblad dat afzonderlijk in de webbrowser of in een afzonderlijk webbrowservenster, Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan met het account waarmee u gebruikt om te maken van de catalogus met gegevens in de vorige stap.
2. Selecteer **Bladeren** en klik op **Gegevenscatalogus**.

    ![Azure gegevenscatalogus--Blader Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) ziet u de gegevenscatalogus die u hebt gemaakt.

    ![Azure gegevenscatalogus--weergave catalogus in lijst](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Klik op de catalogus die u hebt gemaakt. U ziet het blad **Gegevenscatalogus** in de portal.

    ![Azure gegevenscatalogus--blade in de portal ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. U kunt de eigenschappen van de catalogus met gegevens weergeven en bijgewerkt om ze. Bijvoorbeeld, klik op **prijzen laag** en de editie wijzigen.

    ![Azure gegevenscatalogus--prijzen van laag](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works-voorbeelddatabase
In deze zelfstudie u gegevens activa (tabellen) van de voorbeelddatabase AdventureWorks2014 hebt geregistreerd voor de SQL Server-Database Engine, maar u kunt elke ondersteunde gegevensbron gebruiken als u wilt werken met gegevens die vertrouwde en relevant zijn voor uw rol. Zie [ondersteunde gegevensbronnen](data-catalog-dsr.md)voor een lijst met ondersteunde gegevensbronnen.

### <a name="install-the-adventure-works-2014-oltp-database"></a>Installeer de Adventure Works 2014 OLTP-database
De database Adventure Works ondersteunt standaard online verwerking van transacties-scenario's voor een factitief fietsenfabrikant (Adventure Works Cycle), waaronder-producten, verkoop, en inkoop. In deze zelfstudie kunt u gegevens over producten in de gegevenscatalogus Azure registreren.

De voorbeelddatabase Adventure Works installeren:

1. Download [Adventure Works 2014 volledige Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) op CodePlex.
2. Als u met het herstellen van de database op uw computer, volgt u de instructies in [een Database terugzetten met behulp van SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx)of door deze stappen uit:
    1. Open SQL Server Management Studio en verbinding maken met de SQL Server-Database Engine.
    2. Met de rechtermuisknop op **Databases** en klik op **Database herstellen**.
    3. Klik onder **Database terugzetten**, klikt u op de optie **apparaat** voor **bron** en klik op **Bladeren**.
    4. Klik onder **Selecteer back-apparaten**, klikt u op **toevoegen**.
    5. Ga naar de map waar u het bestand **AdventureWorks2014.bak** , schakelt u het bestand en klik op **OK** om het dialoogvenster **Zoeken back-upbestand** te sluiten.
    6. Klik op **OK** om het dialoogvenster **Selecteer back-apparaten** te sluiten.    
    7. Klik op **OK** om het dialoogvenster **Database terugzetten** te sluiten.

U kunt nu gegevens activa van de voorbeelddatabase Adventure Works registreren met behulp van Azure-gegevenscatalogus.

## <a name="register-data-assets"></a>Register gegevens activa

In deze oefening kunt u het hulpmiddel registratie activa van de gegevens uit de Adventure Works-database met de catalogus registreren. Registratie is het proces van belangrijke structurele metagegevens, zoals namen, typen en locaties uitpakken uit de gegevensbron en de activa bevat en dat metagegevens te kopiëren naar de catalogus. De gegevensbron en gegevens activa blijven waar ze zijn, maar de metagegevens wordt gebruikt door de catalogus zodat u ze gemakkelijker makkelijker kan worden gevonden en te begrijpen.

### <a name="register-a-data-source"></a>Een gegevensbron registreert

1.  Ga naar de [startpagina van de catalogus van Azure-gegevens](https://azuredatacatalog.com) en klik op **Gegevens publiceren**.

    ![Azure gegevens catalogus--gegevens publiceren knop](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Klik op **Starten toepassing** als u wilt downloaden, installeren, en het registratiehulpprogramma uitvoeren op uw computer.

    ![Azure gegevenscatalogus--knop starten](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Klik op **aanmelden** en voert u uw referenties op **de welkomstpagina** .    

    ![Azure gegevenscatalogus--welkomstpagina](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Klik op **SQL Server** en het **volgende**op de pagina **Microsoft Azure-gegevenscatalogus** .

    ![Azure gegevenscatalogus--gegevensbronnen](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Voer in de eigenschappen van de SQL Server-verbinding voor **AdventureWorks2014** (Zie het volgende voorbeeld) en klik op **verbinding maken**.

    ![Azure gegevenscatalogus--SQL Server-verbindingsinstellingen](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  De metagegevens van uw gegevens activa registreren. In dit voorbeeld registreren u **Productie/Product** objecten uit de naamruimte AdventureWorks productie:

    1. Klik in de **Hiërarchie van de Server** **AdventureWorks2014** uitvouwen en **productie**op.
    2. **Product**, **ProductCategory** **ProductDescription**en **ProductPhoto** selecteren met Ctrl + klikken.
    3. Klik op de **geselecteerde pijl verplaatsen** (**>**). Deze actie overgezet alle geselecteerde objecten naar de lijst met **objecten worden geregistreerd** .

        ![Azure gegevenscatalogus zelfstudie--bladeren en objecten selecteren](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Selecteer **een voorbeeld opnemen** om op te nemen een momentopname voorbeeld van de gegevens. De momentopname maximaal 20 records uit elke tabel bevat en dit hebt gekopieerd in de catalogus.
    5. Selecteer **Gegevens-profiel opnemen** om op te nemen een momentopname van de object-statistieken voor het profiel gegevens (bijvoorbeeld: minimum, maximum aantal en gemiddelde waarden voor een kolom, het aantal rijen).
    6. Voer de **adventure works, maal**in het veld **tags toevoegen** . Deze actie worden opgeteld labels zoeken voor de activa van deze gegevens. Markeringen zijn een uitstekende manier om gebruikers een geregistreerde gegevensbron zoeken te helpen.
    7. Geef de naam van een **expert** op deze gegevens (optioneel).

        ![Azure gegevenscatalogus zelfstudie--objecten worden geregistreerd](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Klik op **REGISTREREN**. Azure gegevenscatalogus registreert uw geselecteerde objecten. In deze oefening zijn de geselecteerde objecten uit Adventure Works geregistreerd. Het hulpmiddel registratie metagegevens haalt uit de gegevens van activa en kopieert u die gegevens naar de gegevenscatalogus van Azure-service. De gegevens blijft waar deze zich momenteel bevindt, en blijft onder het beheer van de beheerders en beleid van het huidige systeem.

        ![Azure gegevenscatalogus--geregistreerde objecten](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Als u wilt uw geregistreerde gegevensbronobjecten wordt weergegeven, klikt u op **Weergave-Portal**. Bevestig dat u alle vier tabellen en de database in de rasterweergave ziet in de portal Azure-gegevenscatalogus.

        ![Objecten in de gegevenscatalogus van Azure-portal ](media/data-catalog-get-started/data-catalog-view-portal.png)


In deze oefening u objecten uit de voorbeelddatabase Adventure Works geregistreerd, zodat deze worden eenvoudig door gebruikers in uw organisatie gevonden kunnen. In de volgende oefening leert u hoe u kennismaken met geregistreerde gegevens activa.

## <a name="discover-data-assets"></a>Kennismaken met gegevens activa
Discovery in Azure-gegevenscatalogus gebruikt twee primaire regelingen: zoeken en filteren.

Zoeken is bedoeld als intuïtieve en krachtige. Standaard worden zoektermen vergeleken met een eigenschap in de catalogus, met inbegrip van de gebruiker opgegeven aantekeningen.

Filteren is ontworpen als aanvulling op zoeken. U kunt specifieke kenmerken zoals experts, gegevensbrontype objecttype en labels overeenkomende gegevens activa weergeven en zoekresultaten op overeenkomende activa beperken selecteren.

Met behulp van een combinatie van zoeken en filteren, kunt u snel de gegevensbronnen die zijn geregistreerd met de gegevenscatalogus Azure te vinden van de activa van de gegevens die u nodig hebt gaan.

In deze oefening kunt u de portal Azure-gegevenscatalogus gebruiken om te ontdekken gegevens activa die u in de vorige oefening hebt geregistreerd. Zie [Naslaginformatie over zoeken in gegevenscatalogus-syntaxis](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor meer informatie over de syntaxis van de zoekopdracht.

Hier volgen enkele voorbeelden om vast te stellen gegevens activa in de catalogus.  

### <a name="discover-data-assets-with-basic-search"></a>Kennismaken met gegevens activa met een basiszoekopdracht
Eenvoudige zoeken kunt u zoeken in een catalogus met behulp van een of meer zoektermen. Resultaten zijn elementen die overeenkomen met op een eigenschap met een of meer van de opgegeven voorwaarden.

1. Klik op **Start** in de portal Azure-gegevenscatalogus. Als u de webbrowser hebt gesloten, gaat u naar de [startpagina van Azure-gegevenscatalogus](https://www.azuredatacatalog.com).
2. Voer in het zoekvak `cycles` en druk op **ENTER**.

    ![Azure gegevenscatalogus--basistekst zoeken](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Bevestig dat u alle vier tabellen en de database (AdventureWorks2014) in de resultaten zien. U kunt schakelen tussen **rasterweergave** en de **Lijstweergave** door te klikken op knoppen op de werkbalk, zoals wordt weergegeven in de volgende afbeelding. Zoals u ziet dat de zoekwoord is gemarkeerd in de lijst met zoekresultaten omdat de optie **markeren** **ingeschakeld is**. U kunt ook het aantal **resultaten per pagina** in de lijst met zoekresultaten opgeven.

    ![Azure gegevenscatalogus--basistekst zoekresultaten](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    Het deelvenster **zoekopdrachten** is aan de linkerkant en het deelvenster **Eigenschappen** is aan de rechterkant. U kunt in het deelvenster **zoekopdrachten** zoekcriteria wijzigen en resultaten filteren. Het deelvenster **Eigenschappen** geeft eigenschappen van een geselecteerd object in het raster of de lijst.

4. Klik op **Product** in de lijst met zoekresultaten. Klik op de **Preview-versie**, **kolommen**, **Gegevens profiel**en **documentatie** tabbladen of klik op de pijl om uit te vouwen het onderste deelvenster.  

    ![Azure gegevenscatalogus--onderste deelvenster](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    Op het tabblad **voorbeeld** ziet u een voorbeeld van de gegevens in de tabel **Product** .  
5. Klik op het tabblad **kolommen** voor meer informatie over kolommen (zoals **naam** en het **gegevenstype**) in de activa gegevens.
6. Klik op het tabblad **Gegevens profiel** als u wilt zien van het profiel van gegevens (bijvoorbeeld: aantal rijen, de grootte van de gegevens of de minimumwaarde in een kolom) in de activa gegevens.
7. De resultaten filteren met behulp van **Filters** aan de linkerkant. Bijvoorbeeld, klikt u op **tabel** voor het **Objecttype**en u ziet alleen de vier tabellen, niet de database.

    ![Azure gegevenscatalogus--zoekresultaten filteren](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Kennismaken met gegevens activa met een bereik eigenschap instellen
Een bereik eigenschap instellen, zodat u achterhaalt gegevens activa waar de zoekterm wordt vergeleken met de opgegeven eigenschap.

1. Wis de filter **tabel** onder **Objecttype** in **Filters**.  
2. Voer in het zoekvak `tags:cycles` en druk op **ENTER**. Zie [Zoeken in gegevenscatalogus syntaxis van de verwijzing](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor alle eigenschappen die u gebruiken kunt voor het zoeken van de gegevenscatalogus.
3. Bevestig dat u alle vier tabellen en de database (AdventureWorks2014) in de resultaten zien.  

    ![Gegevenscatalogus--eigenschap een bereik van zoekresultaten instellen](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>De zoekopdracht opslaan als
1. Voer een naam voor het zoeken in het deelvenster **zoekopdrachten** in de sectie **Huidige zoeken** en klik op **Opslaan**.

    ![Azure gegevenscatalogus--opslaan zoeken](media/data-catalog-get-started/data-catalog-save-search.png)
2. Bevestig dat de opgeslagen zoekactie onder **Zoekopdrachten opgeslagen weergegeven**.

    ![Azure gegevenscatalogus--opgeslagen zoekopdrachten](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Selecteer een van de acties die u op het opgeslagen zoeken (**de naam van wijzigen**, **verwijderen**, **Opslaan als standaard** zoeken uitvoeren kunt).

    ![Azure gegevenscatalogus--opgeslagen zoekopties](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Booleaans operatoren
U kunt uitbreiden of beperken van uw zoekopdracht met Booleaans operatoren.

1. Voer in het zoekvak `tags:cycles AND objectType:table`, en druk op **ENTER**.
2. Bevestig dat u alleen tabellen (niet de database) in de zoekresultaten ziet.  

    ![Azure gegevenscatalogus--Booleaanse operator in zoeken](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Groeperen met haakjes
Groeperen en tussen haakjes weergegeven, kunt u onderdelen van de query om te bereiken logische moeten worden geïsoleerd, met name samen met Booleaans operatoren groeperen.

1. Voer in het zoekvak `name:product AND (tags:cycles AND objectType:table)` en druk op **ENTER**.
2. Bevestig dat u alleen de tabel **Product** in de lijst met zoekresultaten ziet.

    ![Azure gegevenscatalogus--groeperen zoeken](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Vergelijkingsoperatoren
Met vergelijkingsoperatoren, kunt u vergelijkingen dan gelijke voor eigenschappen de gegevenstypen Numeriek en datum.

1. Voer in het zoekvak `lastRegisteredTime:>"06/09/2016"`.
2. Wis de filter **tabel** onder **Objecttype**.
3. Druk op **ENTER**.
4. Bevestig dat u ziet de tabellen **Product**, **ProductCategory** **ProductDescription**en **ProductPhoto** en de AdventureWorks2014-database die u in de zoekresultaten hebt geregistreerd.

    ![Azure gegevenscatalogus--vergelijkingsresultaten zoeken](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Lees [hoe u gegevens activa ontdekken](data-catalog-how-to-discover.md) voor gedetailleerde informatie over gegevens activa en [referentie voor kql zoeken in gegevenscatalogus](https://msdn.microsoft.com/library/azure/mt267594.aspx) ontdekken voor de syntaxis van de zoekopdracht.

## <a name="annotate-data-assets"></a>Gegevens activa van aantekeningen voorzien
In deze oefening gebruikt u de gegevenscatalogus van Azure-portal om aantekeningen te (toegangsgegevens voor de beschrijvingen, labels of experts toevoegen) gegevens activa die u eerder hebt geregistreerd in de catalogus. De aantekeningen aanvulling en de structurele metagegevens opgehaald uit de gegevensbron tijdens de registratie verbeteren en zorgt ervoor dat de gegevens activa veel gemakkelijker om te ontdekken en begrijpen.

In deze oefening u aantekeningen toevoegen aan een enkele gegevens activa (ProductPhoto). U toevoegen een beschrijvende naam en beschrijving aan de activa van de gegevens ProductPhoto.  

1.  Ga naar de [startpagina van de catalogus van Azure-gegevens](https://www.azuredatacatalog.com) en zoek met `tags:cycles` om de gegevens activa die u hebt geregistreerd.  
2. Klik op **ProductPhoto** in lijst met zoekresultaten.  
3. Voer **productafbeeldingen** voor **Beschrijvende naam** en **foto's Product voor marketingactiviteit materialen** voor de **Beschrijving**.

    ![Azure gegevenscatalogus--ProductPhoto beschrijving](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    De **Beschrijving** helpt anderen detecteren en waarom begrijpen en het gebruik van de activa van de geselecteerde gegevens. U kunt ook meer tags toevoegen en weergeven van kolommen. Nu kunt u proberen zoeken en filteren als u wilt ontdekken gegevens activa met behulp van de beschrijvende metagegevens die u hebt toegevoegd aan de catalogus.

U kunt ook de volgende handelingen uit op deze pagina doen:

- Voeg experts voor de activa van de gegevens. Klik op **toevoegen** in het gebied **Experts** .
- Tags toevoegen op het niveau van de gegevensset. Klik op **toevoegen** in het gebied **Tags** . Een tag kan zijn de tag van een gebruiker of woordenlijst. De standaard-editie van gegevenscatalogus bevat een woordenlijst voor bedrijven waarmee beheerders van de catalogus met een centrale zakelijke taxonomie definiëren. Catalogus-gebruikers kunnen vervolgens aantekeningen maken bij gegevens activa met termen uit de woordenlijst. Zie voor meer informatie, [het instellen van de woordenlijst voor bedrijven voor geregeld labeling](data-catalog-how-to-business-glossary.md)
- Tags toevoegen op het kolomniveau van de. Klik op **toevoegen** onder **labels** voor de kolom die u wilt van aantekeningen voorzien.
- Een beschrijving toevoegen op het kolomniveau van de. **Omschrijving** invoeren voor een kolom. U kunt ook de beschrijving van metagegevens opgehaald uit de gegevensbron weergeven.
- Voeg **toegangsaanvragen** informatie die gebruikers wordt getoond hoe u toegang tot de gegevens van activa.

    ![Azure gegevenscatalogus--tags, beschrijvingen toevoegen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Kies het tabblad **documentatie** en geef de documentatie van de activa gegevens. Met de gegevenscatalogus Azure documentatie, kunt u uw gegevenscatalogus als inhoud opslagplaats voltooid sprekersnotities van de activa van uw gegevens maken.

    ![Azure gegevenscatalogus--documentatie tabblad](media/data-catalog-get-started/data-catalog-documentation.png)


U kunt ook een aantekening toevoegen aan meerdere gegevens activa. U kunt bijvoorbeeld, selecteert u alle gegevens activa die u geregistreerd en een expert voor hen opgeven.

![Azure gegevenscatalogus--aantekeningen toevoegen aan meerdere gegevens activa](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure gegevenscatalogus ondersteunt een aanpak rest-bron voor aantekeningen. Elke gebruiker gegevenscatalogus kunt labels (gebruiker of woordenlijst), beschrijvingen en andere metagegevens, toevoegen, zodat elke gebruiker met een perspectief op een activum gegevens en het gebruik ervan dat perspectief genomen en beschikbaar zijn voor andere gebruikers hebben kan.

Lees [hoe u gegevens activa van aantekeningen voorzien](data-catalog-how-to-annotate.md) voor gedetailleerde informatie over gegevens activa aantekeningen te maken.

## <a name="connect-to-data-assets"></a>Verbinding maken met gegevens activa
In deze oefening kunt u gegevens activa in een hulpmiddel geïntegreerde-client (Excel) en een niet-geïntegreerde hulpmiddel (SQL Server Management Studio) met behulp van informatie over de databaseverbinding openen.

> [AZURE.NOTE] Het is belangrijk te onthouden Azure-gegevenscatalogus niet hebt u toegang tot de werkelijke gegevensbron, deze gewoon gemakkelijker kunt ontdekken en begrijpt. Wanneer u verbinding met een gegevensbron maakt, wordt de clienttoepassing die u kiest uw Windows-referenties gebruikt of vraagt u om referenties zo nodig. Als u niet eerder toegang is verleend tot de gegevensbron, moet u toegang is verleend voordat u verbinding kunt maken.

### <a name="connect-to-a-data-asset-from-excel"></a>Verbinding maken met een activum gegevens uit Excel

1. Selecteer **Product** in de lijst met zoekresultaten. Klik op **Openen In** op de werkbalk en klik op **Excel**.

    ![Azure gegevenscatalogus--verbinding maken met gegevens van activa](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klik op **openen** in het pop-upvenster voor downloaden. Deze varieert mogelijk naar gelang de browser.

    ![Azure gegevenscatalogus--gedownloade Excel-verbindingsbestand](media/data-catalog-get-started/data-catalog-download-open.png)
3. Klik op **inschakelen**in het venster **Beveiligingskennisgeving van Microsoft Excel beschreven** .

    ![Azure gegevenscatalogus--Excel beveiliging pop](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. De standaardinstellingen behouden in het dialoogvenster **Gegevens importeren** en klik op **OK**.

    ![Azure gegevenscatalogus--gegevens van Excel importeren](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Bekijk de gegevensbron in Excel.

    ![Azure gegevenscatalogus--producttabel in Excel](media/data-catalog-get-started/data-catalog-connect2.png)

In deze oefening verbonden u met gegevens activa ontdekt met behulp van Azure-gegevenscatalogus. Met de gegevenscatalogus van Azure-portal, kunt u verbinding maakt via de clienttoepassingen geïntegreerd in het menu **openen in** . U kunt ook verbinding maakt met een toepassing die u met behulp van de verbindingsgegevens locatie opgenomen in de metagegevens van activa kiezen. U kunt bijvoorbeeld SQL Server Management Studio verbinding maken met de database AdventureWorks2014 voor toegang tot de gegevens in de activa van de gegevens in deze zelfstudie geregistreerd.

1. Open **SQL Server Management Studio**.
2. Voer in het dialoogvenster **verbinding maken met de Server** de servernaam vanuit het deelvenster **Eigenschappen** in de gegevenscatalogus van Azure-portal.
3. Gebruik de juiste verificatie en referenties voor toegang tot de gegevens van activa. Als u geen toegang hebt, gebruikt u informatie in het veld **Toegang vragen** om dit te krijgen.

    ![Azure gegevenscatalogus--toegangsaanvragen](media/data-catalog-get-started/data-catalog-request-access.png)

Klik op **Weergave verbindingstekenreeksen** om te bekijken en ADF.NET, ODBC en OLEDB verbindingstekenreeksen naar het Klembord voor gebruik in uw toepassing kopiëren.

## <a name="manage-data-assets"></a>Gegevens activa beheren
In deze stap ziet u hoe u beveiliging instellen voor uw gegevens activa. Gegevenscatalogus wordt niet gebruikers toegang geven tot de gegevens zelf. Toegang tot gegevens Hiermee stelt u de eigenaar van de gegevensbron.

U kunt gegevenscatalogus gebruiken om te ontdekken gegevensbronnen en weer te geven van de metagegevens die betrekking hebben op de bronnen die zijn geregistreerd in de catalogus. Er zijn situaties, echter waar gegevensbronnen alleen weergegeven aan specifieke gebruikers of groepen worden moeten. Voor deze scenario's, kunt u gegevenscatalogus eigenaar van de activa geregistreerde gegevens in de catalogus en aan een besturingselement voor vervolgens de zichtbaarheid van de activa u de eigenaar bent.

> [AZURE.NOTE] De beheermogelijkheden die worden beschreven in deze oefening zijn beschikbaar alleen in de standaard-editie van Azure gegevenscatalogus, niet in de gratis versie.
In de catalogus van Azure-gegevens, kunt u eigenaar worden van gegevens activa, collega eigenaren toevoegen aan gegevens activa en de zichtbaarheid van gegevens activa.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Eigenaar worden van gegevens activa en zichtbaarheid beperken

1. Ga naar de [startpagina van Azure-gegevenscatalogus](https://www.azuredatacatalog.com). Voer in het tekstvak **Zoeken** `tags:cycles` en druk op **ENTER**.
2. Klik op een item in de lijst met resultaten en klikt u op **Eigenaar** op de werkbalk.
3. In de sectie **beheer** van het deelvenster **Eigenschappen** , klikt u op **Eigenaar**.

    ![Azure gegevenscatalogus--eigenaar](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Als u wilt beperken zichtbaarheid, kies **eigenaren en deze gebruikers** in de sectie **zichtbaarheid** en klikt u op **toevoegen**. Voer de e-mailadressen van gebruikers in het tekstvak en druk op **ENTER**.

    ![Azure gegevenscatalogus--beperken van toegang.](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Elementen van de gegevens verwijderen

In deze oefening gebruikt u de portal Azure-gegevenscatalogus Gegevensvoorbeeld van geregistreerde gegevens activa intrekken en gegevens activa verwijderen uit de catalogus.

In de catalogus van Azure-gegevens, kunt u afzonderlijke activa of meerdere activa verwijderen.

1. Ga naar de [startpagina van Azure-gegevenscatalogus](https://www.azuredatacatalog.com).
2. Voer in het tekstvak **Zoeken** `tags:cycles` en klikt u op **ENTER**.
3. Selecteer een item in de lijst met resultaten en klik op **verwijderen** op de werkbalk zoals wordt weergegeven in de volgende afbeelding:

    ![Azure gegevenscatalogus--raster-item verwijderen](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Als u de lijstweergave gebruikt, is het selectievakje links van het item, zoals wordt weergegeven in de volgende afbeelding:

    ![Azure gegevenscatalogus--lijstitem verwijderen](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    U kunt ook meerdere gegevens-elementen selecteren en verwijdert u deze zoals wordt weergegeven in de volgende afbeelding:

    ![Azure gegevenscatalogus--meerdere gegevens activa verwijderen](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Het standaardgedrag van de catalogus is toe te staan dat iedere gebruiker voor het registreren van een gegevensbron en toestaan dat alle gebruikers verwijderen van de activa van alle gegevens die is geregistreerd. De beheermogelijkheden opgenomen in de standaard-editie van Azure gegevenscatalogus bieden extra opties voor de eigenaar van activa, beperken die activa, kunnen detecteren en beperken wie activa kunt verwijderen.


## <a name="summary"></a>Overzicht

In deze zelfstudie verkend u essentiële mogelijkheden van de gegevenscatalogus Azure, inclusief registreren, aantekeningen, ontdekken en enterprise gegevens activa te beheren. Nu u de zelfstudie hebt uitgevoerd, is het tijd aan de slag. U kunt vandaag gaan door het registreren van de gegevensbronnen die u en uw team afhankelijk en uitnodigen collega's aan de catalogus gebruiken.

## <a name="references"></a>Verwijzingen

- [Het registreren van gegevens activa](data-catalog-how-to-register.md)
- [Hoe u gegevens activa ontdekken](data-catalog-how-to-discover.md)
- [Hoe u gegevens activa van aantekeningen voorzien](data-catalog-how-to-annotate.md)
- [Het vastleggen van gegevens activa](data-catalog-how-to-documentation.md)
- [Verbinding maken met gegevens activa](data-catalog-how-to-connect.md)
- [Het beheren van gegevens activa](data-catalog-how-to-manage.md)
