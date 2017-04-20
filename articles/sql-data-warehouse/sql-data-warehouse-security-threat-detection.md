<properties
   pageTitle="Aan de slag met SQL Data Warehouse bedreiging detectie"
   description="Hoe u aan de slag met bedreiging detectie"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Aan de slag met bedreiging detectie

> [AZURE.SELECTOR]
- [Controle](sql-data-warehouse-auditing-overview.md)
- [Bedreiging detectie](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Overzicht

Bedreiging detectie vastgesteld activiteiten met een afwijkende database waarin wordt aangegeven dat mogelijke beveiligingsrisico's met de database. Bedreiging detectie is in de Preview-versie en wordt ondersteund voor SQL Data Warehouse.

Bedreiging detectie biedt een nieuwe laag voor waardepapier, waarmee klanten detecteren en reageren op mogelijke threats schiet door te geven beveiligingsmeldingen over afwijkende activiteiten. Gebruikers kunnen de verdachte gebeurtenissen die werken met [Azure SQL Data Warehouse controle](sql-data-warehouse-auditing-overview.md) om te bepalen als deze afkomstig zijn van een poging toegang krijgen tot, haar heeft of misbruik van gegevens in het datawarehouse verkennen.
Bedreiging detectie kunt u eenvoudig naar adres mogelijke risico's voor de datawarehouse zonder dat u een expert waardepapier of geavanceerde beveiligingsinstellingen bewaken beheren.

Bijvoorbeeld vastgesteld bedreiging detectie bepaalde activiteiten met een afwijkende database waarin wordt aangegeven dat mogelijke SQL webweergave pogingen. SQL-webweergave is een van de algemene Web application beveiligingskwesties op Internet, gebruikt voor aanvallen gegevensgestuurde toepassingen. Hackers profiteren van de toepassing problemen om toe te voegen schadelijke SQL-instructies in de toepassingsvelden voor gegevensinvoer voor inbreuk op of het wijzigen van gegevens in de database.


## <a name="set-up-threat-detection-for-your-database"></a>Bedreiging detectie voor uw database instellen

1. Start de Azure-Portal op [https://portal.azure.com](https://portal.azure.com).

2. Ga naar het blad configuratie van de SQL Data Warehouse die u wilt controleren. Selecteer in het blad instellingen **controleren & bedreiging detectie**.

    ![Navigatiedeelvenster][1]

3. Schakel in het blad van de configuratie **controle & bedreiging detectie** **op** controle, waarin de instellingen van de detectie bedreiging wordt weergegeven.

    ![Navigatiedeelvenster][2]

4. Schakel **op** bedreiging detectie.

5. De lijst met e-mailberichten die beveiligingsmeldingen na detectie van afwijkende data warehouse activiteiten ontvangt configureren.

6. Klik op **Opslaan** in het blad **controle & bedreiging detectie** configuratie op te slaan de nieuwe of bijgewerkte controle en gevaar detectie beleid.

    ![Navigatiedeelvenster][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Afwijkende data warehouse activiteiten na detectie van een verdachte gebeurtenis verkennen

1. U ontvangt per e-mail een melding na detectie van activiteiten met een afwijkende database. <br/>
Het e-mailbericht, vindt u informatie over de verdachte gebeurtenis met inbegrip van de aard van de afwijkende activiteiten, de databasenaam van de, de servernaam van de en de tijd van de gebeurtenis. Bovendien, vindt u informatie over mogelijke oorzaken en aanbevolen acties onderzoeken en verklein de mogelijke bedreiging met de database.<br/>

    ![Navigatiedeelvenster][4]

2. Klik op de koppeling **Azure SQL-controle logboek** , waarmee u de klassieke Azure-Portal starten en de relevante records controle rond de tijd van de verdachte gebeurtenis weergeven in het e-mailbericht.

    ![Navigatiedeelvenster][5]

3. Klik op de controlerecords voor meer meer informatie over de databaseactiviteiten verdachte zoals SQL-instructie is mislukt reden en client IP.

    ![Navigatiedeelvenster][6]

4. Klik in het blad Records controle op **openen in Excel** als u wilt openen, een vooraf geconfigureerde excel-sjabloon als u wilt importeren en grondigere analyse van het controlelogboek rond de tijd van de verdachte gebeurtenis uitvoeren.<br/>
**Notitie:** In Excel 2010 of hoger, is Power Query en de instelling **Snel combineren** vereist

    ![Navigatiedeelvenster][7]

5. Als u wilt configureren de instelling **Snel combineren** - het linttabblad **POWER QUERY** , selecteer **Opties** om het dialoogvenster opties weer te geven. Selecteer de sectie over Beheerdersprivacy en de tweede optie - "De privacyniveaus negeren en mogelijk de prestaties verbeteren" kiest:

    ![Navigatiedeelvenster][8]

6. Als u wilt laden SQL controlelogboeken bijhouden, zorg ervoor dat de parameters in de instellingen op tabblad correct zijn ingesteld, en selecteer vervolgens het lint 'Gegevens' en klikt u op de knop Alles vernieuwen.

    ![Navigatiedeelvenster][9]

7. De resultaten worden weergegeven in het blad **SQL controlelogboeken bijhouden** waarmee u kunt het uitvoeren van grondigere analyse van de afwijkende activiteiten die zijn gevonden en het effect van de beveiligingsgebeurtenis in uw toepassing.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
