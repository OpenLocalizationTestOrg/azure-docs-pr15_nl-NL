<properties
    pageTitle="Aan de slag door de Database inschakelen uit te voeren voor uitrekken Wizard | Microsoft Azure"
    description="Leer hoe u een database configureren voor uitrekken Database met behulp van de Database inschakelen voor uitrekken Wizard."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Aan de slag door de Database inschakelen voor uitrekken Wizard uit te voeren

Voer de Database inschakelen voor uitrekken Wizard voor meer informatie over het configureren van een database voor uitrekken Database.  In dit onderwerp worden de gegevens die u hebt om in te voeren en de opties die u moet maken in de wizard.

Zie voor meer informatie over uitrekken Database, [Uitrekken Database](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Als u de Database uitrekken uitschakelt, moet u later onthouden dat uitrekken Database uitschakelen voor een tabel of voor een database niet het externe object verwijdert. Als u verwijderen van de externe tabel of de externe database wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De externe objecten blijven Azure kosten totdat u deze handmatig verwijderen. 

## <a name="launch-the-wizard"></a>Start de wizard

1.  Selecteer de database die u wilt inschakelen uitrekken in SQL Server Management Studio in Object Explorer.

2.  Rechts\-klikt u op en selecteert u die **taken**, en selecteer vervolgens **Uitrekken**, en selecteer vervolgens **inschakelen** om de wizard te starten.

## <a name="Intro"></a>Inleiding
Bekijk het doel van de wizard en de vereisten.

De belangrijke vereisten onder andere het volgende:

-   U moet een beheerder zijn om instellingen van de database te wijzigen.
-   U hebt een Microsoft Azure-abonnement hebben.
-   Uw SQL-Server heeft moeten kunnen communiceren met de externe Azure server.

![Introductiepagina van de wizard uitrekken][StretchWizardImage1]

## <a name="Tables"></a>Selecteer tabellen
Selecteer de tabellen die u wilt inschakelen voor uitrekken.

Tabellen met een groot aantal rijen weergegeven boven aan de gesorteerde lijst. Voordat u de lijst van tabellen in de Wizard wordt weergegeven, wordt deze voor de gegevenstypen die momenteel niet worden ondersteund door uitrekken Database geanalyseerd.

![Selecteer de pagina tabellen van de wizard uitrekken][StretchWizardImage2]

|Kolom|Beschrijving|
|----------|---------------|
|(geen titel)|Schakel het selectievakje in deze kolom om de geselecteerde tabel inschakelen voor uitrekken.|
|**Naam**|Hiermee geeft u de naam van de kolom in de tabel.|
|(geen titel)|Een symbool in deze kolom mogelijk een waarschuwing vertegenwoordigen die hoort niet\'t voorkomen dat u de geselecteerde tabel inschakelen voor uitrekken. Deze mogelijk ook een probleem die voorkomen dat u de geselecteerde tabel inschakelen voor uitrekken vertegenwoordigen \- bijvoorbeeld omdat in de tabel wordt een niet-ondersteunde gegevenstype gebruikt. Plaats de muisaanwijzer op het symbool voor meer informatie in de knopinfo wordt weergegeven. Zie [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md)voor meer informatie.|
|**Uitgerekt**|Geeft aan of de tabel al is ingeschakeld voor uitrekken.|
|**Migreren**|U kunt een hele tabel (**Volledige tabel**) migreren of kunt u een filter op een bestaande kolom in de tabel. Als u een ander filter-functie gebruiken om te selecteren van rijen om te migreren wilt, voert u de instructie ALTER TABLE om op te geven van de filterfunctie nadat u de wizard afsluit. Voor meer informatie over de filterfunctie, Zie [selecteert u rijen wilt migreren met behulp van een filterfunctie](sql-server-stretch-database-predicate-function.md). Zie voor meer informatie over het toepassen van de functie [Uitrekken-Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) of [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Rijen**|Hiermee geeft u het aantal rijen in de tabel.|
|**Grootte (KB)**|Hiermee geeft de grootte van de tabel in KB.|

## <a name="Filter"></a>Geef desgewenst een rij-filter

Als u opgeven van de functie van een filter om rijen wilt te migreren te selecteren, het volgende doen op de pagina **Selecteer tabellen** .

1.  Klik in de lijst **Selecteer de tabellen die u wilt uitrekken** , klikt u op de **Hele tabel** in de rij voor de tabel. Het dialoogvenster **Selecteer rijen worden uitgerekt** wordt geopend.

    ![Een filterfunctie definiëren][StretchWizardImage2a]

2.  Selecteer in het dialoogvenster **Selecteer rijen te rekken** , **Kiest u rijen**.

3.  Geef een naam voor de filterfunctie in het **veld Naam**.

4.  Voor de **Where** -component, kiest u een kolom uit de tabel, kiest u een operator en een waarde opgeven.

5. Klik op **controleren** als u wilt testen van de functie. Als de functie resultaten uit de tabel retourneert - dat wil zeggen rapporten als er rijen om te migreren die voldoen aan de voorwaarde - de test **slagen**.

    >   [AZURE.NOTE] Het tekstvak waarin u kunt de filterquery is alleen-lezen. U kunt de query in het tekstvak niet bewerken.

6.  Klik op gereed om terug te keren naar de pagina **Selecteer tabellen** .

Alleen wanneer u klaar bent met de wizard, wordt met de filterfunctie gemaakt in SQL Server. U kunt tot die tijd terugkeren naar de pagina **Selecteer tabellen** om te wijzigen of de naam van de filterfunctie wijzigen.

![Selecteer de pagina tabellen na het definiëren van een filterfunctie][StretchWizardImage2b]

Als u een ander type van de filterfunctie gebruiken om te selecteren van rijen om te migreren wilt, de volgende dingen doen.  

-   De wizard afsluiten en de instructie ALTER TABLE uitrekken inschakelen voor de tabel en geef een filterfunctie uitvoeren. Zie voor meer informatie [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md).  

-   Voer de instructie ALTER TABLE om op te geven van een filterfunctie nadat u de wizard afsluit. Zie [een filterfunctie na het uitvoeren van de Wizard toevoegen](sql-server-stretch-database-predicate-function.md#addafterwiz)voor de vereiste stappen.

## <a name="Configure"></a>Azure-implementatie configureren

1.  Meld u aan bij Microsoft Azure met een Microsoft-account.

    ![Meld u aan bij Azure - wizard uitrekken Database][StretchWizardImage3]

2.  Selecteer het bestaande Azure abonnement voor uitrekken Database wilt gebruiken.

3.  Selecteer een Azure regio.
    -   Als u een nieuwe server maakt, wordt de server in dit gebied gemaakt.  
    -   Als u bestaande servers in het geselecteerde gebied hebt, bevat de wizard deze wanneer u **bestaande server**kiezen.

    Als u wilt minimaliseren latentie, kiest u het Azure regio waarin uw SQL-Server zich bevindt. Zie voor meer informatie over regio's, [Azure regio's](https://azure.microsoft.com/regions/).

4.  Opgeven of u wilt gebruiken van een bestaande server of maak een nieuwe Azure server.

    Als de Active Directory op de SQL Server wordt gekoppeld aan Azure Active Directory, kunt u desgewenst een federatieve serviceaccount voor SQL Server gebruiken om te communiceren met de externe Azure server. Zie voor meer informatie over de vereisten voor deze optie [ALTER DATABASE-opties instellen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Nieuwe server maken**

        1.  Maak een aanmelden en een wachtwoord voor de serverbeheerder.

        2.  Gebruik desgewenst een federatieve serviceaccount voor SQL Server om te communiceren met de externe Azure server.

        ![Nieuwe Azure server - wizard uitrekken Database maken][StretchWizardImage4]

    -   **Bestaande server**

        1.  Selecteer de bestaande Azure server.

        2.  De verificatiemethode selecteren.

            -   Als u **SQL Server-verificatie**selecteert, is de beheerder login en -wachtwoord opgeven.

            -   Selecteer **Geïntegreerde verificatie van Active Directory** een federatieve serviceaccount voor SQL Server gebruiken om te communiceren met de externe Azure server. Als de geselecteerde server is niet geïntegreerd met Azure Active Directory, wordt deze optie niet wordt weergegeven.

        ![Selecteer bestaande Azure server - wizard uitrekken Database][StretchWizardImage5]

## <a name="Credentials"></a>Veilige referenties
U hebt een database basispagina gebruiken voor het beveiligen van de referenties die uitrekken Database verbinding maakt met de externe database.  

Als er al een database outmodel sleutel bestaat, voert u het wachtwoord voor deze.  

![Pagina van de referenties van de wizard uitrekken beveiligen][StretchWizardImage6b]

Als de database geen een bestaande basispagina sleutel heeft, voert u een sterk wachtwoord als een sleutel van de basispagina database wilt maken.  

![Pagina van de referenties van de wizard uitrekken beveiligen][StretchWizardImage6]

Zie [OUTMODEL sleutel maken (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) en [Database outmodel sleutel maken](https://msdn.microsoft.com/library/aa337551.aspx)voor meer informatie over de database outmodel-toets. Zie voor meer informatie over de referenties die de wizard maakt, [Maken DATABASE beperkt referentie (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Selecteer IP-adres
Gebruik de subnet IP-adresbereiken (aanbevolen) of het openbare IP-adres van de SQL-Server, een firewallregel maken op Azure waarmee SQL Server communiceren met de externe Azure server.

Informeer de Azure server toe te staan dat binnenkomende gegevens, query's en beheertaken uit te voeren via SQL Server door de firewall Azure of meer IP-adressen die u op deze pagina opgeeft. De wizard wordt niets in de instellingen van de firewall op de SQL Server niet gewijzigd.

![Selecteer de pagina van de IP-adres van de wizard uitrekken][StretchWizardImage7]

## <a name="Summary"></a>Overzicht
Controleer de waarden die u hebt ingevoerd en de opties die u hebt geselecteerd in de wizard en de geschatte kosten op Azure. Selecteer vervolgens **Voltooien** om in te schakelen uitrekken.

![De pagina overzicht van de wizard uitrekken][StretchWizardImage8]

## <a name="Results"></a>Resultaten
Bekijk de resultaten.

Als u wilt controleren van de status van de gegevensmigratie, raadpleegt u [controleren en problemen met gegevensmigratie (uitrekken-Database)](sql-server-stretch-database-monitor.md).

![Pagina met zoekresultaten van de wizard uitrekken][StretchWizardImage9]

## <a name="KnownIssues"></a>Problemen met de wizard
**De wizard uitrekken Database is mislukt.**
Als uitrekken Database nog niet op het serverniveau van de is ingeschakeld en u de beheerdersmachtigingen voor deze inschakelen voor de wizard zonder het systeem uitvoeren, mislukt de wizard. De systeembeheerder uitrekken Database inschakelen voor de lokale server-instantie vragen en voer de wizard opnieuw. Zie voor meer informatie [vereiste: machtiging uitrekken Database inschakelen voor de server](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Volgende stappen
Extra tabellen inschakelen voor uitrekken Database. Gegevensmigratie controleren en beheren van uitrekken\-ingeschakeld databases en tabellen.

-   [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) voor het inschakelen van extra tabellen.

-   [Controleren en problemen met gegevensmigratie](sql-server-stretch-database-monitor.md) om de status van de gegevensmigratie weer te geven.

-   [Onderbreken en hervatten uitrekken Database](sql-server-stretch-database-pause.md)

-   [Beheren en problemen met uitrekken Database](sql-server-stretch-database-manage.md)

-   [Back-up uitrekken ingeschakelde databases](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Zie ook

[Uitrekken Database inschakelen voor een database](sql-server-stretch-database-enable-database.md)

[Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
