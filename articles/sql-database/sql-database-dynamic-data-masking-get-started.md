<properties
   pageTitle="Aan de slag met SQL-Database dynamische gegevens Masking (Azure Portal)"
   description="Hoe u aan de slag met SQL-Database dynamische gegevens Masking in de Portal van Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Aan de slag met SQL-Database dynamische gegevens Masking (Azure Portal)

> [AZURE.SELECTOR]
- [Dynamische gegevens Masking - Azure klassieke Portal](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Overzicht

SQL-Database dynamische gegevens-Masking, beperkt gevoelige gegevens weergeven door deze masking aan onbevoegde gebruikers. Dynamische gegevens masking wordt ondersteund voor de V12-versie van Azure SQL-Database.

Dynamische gegevens masking helpt voorkomen dat onbevoegde toegang doordat klanten wilt aanwijzen hoeveel van de gevoelige gegevens om weer te geven met minimale gevolgen voor de toepassingslaag gevoelige gegevens. Dit is een beleid gebaseerde beveiliging-functie dat de gevoelige gegevens in het resultaat van een query via aangewezen databasevelden, verbergt terwijl de gegevens in de database niet worden gewijzigd.

Bijvoorbeeld een medewerker van de service aan een callcenter mogelijk bellers identificeren door verschillende cijfers van hun sofi-nummer of creditcardnummer, maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice. Een regel masking kan worden gedefinieerd dat alle maskers, maar de laatste vier cijfers met een bepaalde sofi-nummer of creditcardnummer in het resultaat van een query instellen. Een ander voorbeeld kan invoermasker van de juiste gegevens worden gedefinieerd ter bescherming van persoonsgegevens (PII) gegevens, zodat een ontwikkelaar productieomgevingen voor het oplossen van problemen getal van de regelgeving kunt zoeken.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL-Database dynamische gegevens-Masking basisbeginselen

U instellen kunt een dynamische gegevens beleid in de Portal Azure masking door de bewerking dynamische gegevens Masking selecteren in uw SQL-Database configuratie blade of instellingen blade.


### <a name="dynamic-data-masking-permissions"></a>Dynamische gegevens masking machtigingen

Dynamische gegevens masking kan worden geconfigureerd door de beheerder Azure-Database, server-beheerder of beveiliging directeur rollen.

### <a name="dynamic-data-masking-policy"></a>Dynamische gegevens masking beleid

* **SQL-gebruikers die zijn uitgesloten van masking** - een reeks SQL-gebruikers of AAD-identiteiten die zonder masker gegevens in de resultaten van de SQL-query krijgt. Houd er rekening mee dat gebruikers met beheerdersbevoegdheden altijd masking uitgesloten wordt en de oorspronkelijke gegevens zonder een invoermasker ziet.

* **Regels masking** - een set regels die definiëren de desbetreffende velden om te worden gemaskeerde en de masking-functie die wordt gebruikt. De desbetreffende velden kunnen worden gedefinieerd met een schema databasenaam, de naam van de tabel en de kolomnaam.

* **Functies masking** - een reeks methoden voor het weergeven van gegevens voor verschillende scenario's.

| Functie masking | Logica masking |
|----------|---------------|
| **Standaard**  |**Volledige masking op basis van de gegevenstypen van de desbetreffende velden**<br/><br/>• Gebruik XXXX of minder x'en als de grootte van het veld minder dan 4 tekens voor de gegevenstypen tekenreeks (nchar, ntext, nvarchar is).<br/>• Gebruik de waarde nul voor numerieke gegevenstypen (bigint, bits, decimalen, int, geld, numerieke, DateTime, smallmoney, tinyint, toegestane achterstand, reëel).<br/>• Gebruik 01-01-1900 voor de gegevenstypen voor datum/tijd (datum, datetime2, datetime, datetimeoffset, smalldatetime, tijd).<br/>• Voor SQL-variant, de standaardwaarde van het huidige type wordt gebruikt.<br/>• Voor het document XML <masked/> wordt gebruikt.<br/>• Gebruik een lege waarde voor de speciale gegevenstypen (tijdstempel van een tabel, activiteitknooppunt, GUID, binair getal, afbeelding, varbinary ruimte typen).
| **Creditcard** |**Methode die de laatste vier cijfers van de desbetreffende velden beschrijft masking** en een constant tekenreeks wordt als een voorvoegsel in de vorm van een creditcard.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Sofi-nummer** |**Methode die de laatste vier cijfers van de desbetreffende velden beschrijft masking** en een constant tekenreeks wordt als een voorvoegsel in de vorm van een Amerikaans sofi-nummer.<br/><br/>LENGTE-XX-1234 |
| **E-mail** | **Methode die worden weergegeven de eerste letter en Hiermee vervangt u het domein met XXX.com masking** met een constante tekenreeks voorvoegsel in de vorm van een e-mailadres.<br/><br/>aXX@XXXX.com |
| **Willekeurig getal** | De **methode die een willekeurig getal genereert masking** op basis van de geselecteerde grenzen en de werkelijke gegevenstypen. Als de aangewezen grenzen gelijk zijn, klikt u vervolgens zijn de functie masking een constant getal.<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Aangepaste tekst** | **Methode die de eerste en laatste tekens beschrijft masking** en voegt u een aangepaste opvulling-tekenreeks in het midden. Als de oorspronkelijke tekenreeks korter dan de weergegeven voor- en achtervoegsel is, worden alleen de tekenreeks opvulling worden gebruikt. <br/>voorvoegsel [opvulling] achtervoegsel<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Aanbevolen velden aan het masker

Bepaalde velden uit de database wordt de DDM aanbevelingen engine gemarkeerd als vertrouwelijke velden, die mogelijk goede kandidaten voor masking. In het blad dynamische gegevens Masking in de portal ziet u de aanbevolen kolommen voor de database. U hoeft te is op **Masker toevoegen** voor een of meer kolommen en klik vervolgens op **Opslaan** te klikken, een masker voor deze velden toepassen.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dynamische gegevens masking voor de database met behulp van de Azure-Portal instellen

1. Start de Azure-Portal op [https://portal.azure.com](https://portal.azure.com).

2. Ga naar het blad instellingen van de database die u vertrouwelijke gegevens bevat.

3. Klik op de tegel voor **Dynamische gegevens Masking** die Hiermee start u het blad met **Dynamische gegevens Masking** configuratie.

    * U kunt ook Schuif omlaag naar de sectie **bewerkingen** en klik op **Dynamische gegevens Masking**.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. In het blad configuratie **Dynamische gegevens Masking** ziet u mogelijk enkele databasekolommen die de engine aanbevelingen voor masking is gemarkeerd. Als u wilt de aanbevelingen accepteren, klikt u op **Masker toevoegen** voor een of meer kolommen en een invoermasker wordt gemaakt op basis van het standaardtype voor deze kolom. U kunt de functie masking wijzigen door te klikken op de regel masking en bewerken van de masking veldindeling naar een andere indeling van uw keuze. Zorg ervoor dat klikt u op **Opslaan** als uw instellingen wilt opslaan.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Klik om toe te voegen een masker voor een kolom in uw database, boven aan het blad configuratie **Dynamische gegevens Masking** **Masker toevoegen** als u wilt openen van het blad van de configuratie **Masking regel toevoegen**

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Selecteer **het **Schema**, **tabel** - en het veld dat is opgegeven die wordt worden gemaskeerde definiëren** .

7. Kies een **Veldindeling Masking** in de lijst met gevoelige gegevens masking categorieën.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. Klik op **Opslaan** in de regel blade als u wilt bijwerken van de set regels in de dynamische gegevens masking beleid masking masking gegevens.

9. Typ de SQL-gebruikers of AAD-identiteiten die moeten worden uitgesloten van masking en toegang hebt tot de niet-gemaskeerde vertrouwelijke gegevens. Dit moet een puntkomma's gescheiden lijst met gebruikers. Houd er rekening mee dat gebruikers met beheerdersbevoegdheden altijd toegang tot de oorspronkelijke zonder masker gegevens hebben.

    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Als u wilt voorkomen dat de toepassingslaag kunt gevoelige gegevens voor gebruikers van toepassingen bevoegdheden weergeven, voegt u de SQL-gebruiker of AAD identiteit die de toepassing wordt gebruikt om query's in de database. Het wordt ten zeerste aanbevolen dat deze lijst een minimum aantal gebruikers met bevoegdheden te minimaliseren ten weergeven van de gevoelige gegevens bevatten.

10. Klik op **Opslaan** in de configuratie blade om op te slaan van het beleid voor de nieuwe of bijgewerkte masking masking gegevens.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dynamische gegevens voor uw database via Powershell-cmdlets masking instellen

Zie [cmdlets voor Azure SQL-Database](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dynamische gegevens masking voor uw database via REST API instellen

Zie [Operations voor Azure SQL-Databases](https://msdn.microsoft.com/library/dn505719.aspx).
