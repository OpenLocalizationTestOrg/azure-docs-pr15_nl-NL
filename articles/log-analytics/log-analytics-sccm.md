<properties
    pageTitle="Configuration Manager verbinden met Log Analytics | Microsoft Azure"
    description="In dit artikel leest de stappen voor het Configuration Manager verbinden met Log Analytics en begin met het analyseren van gegevens."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Configuration Manager verbinden met Log Analytics

U kunt verbinden System Center Configuration Manager Log Analytics in OMS naar gegevens te verzamelen apparaat synchroniseren. Dit zorgt ervoor dat gegevens uit uw Configuration Manager-implementatie beschikbaar in OMS.

Er zijn een aantal stappen Configuration Manager verbinden met OMS, dus volgt hier een beknopt overzicht van de algehele proces:

1. Klik in de beheerportal Azure Configuration Manager registreren als een webtoepassing en/of Web API-app en zorgen dat u de client-ID en geheime sleutel van de client van de registratie van Azure Active Directory hebt. Zie [de portal gebruiken om te maken van Active Directory-toepassing en service principal die toegang tot bronnen](../resource-group-create-service-principal-portal.md) voor gedetailleerde informatie over het uitvoeren van deze stap.
2. Klik in de Azure Management Portal [Configuration Manager (de geregistreerde web-app) met machtigingen voor toegang tot OMS bieden](#provide-configuration-manager-with-permissions-to-oms).
3. Ga in Configuration Manager [toevoegen van een verbinding met de Wizard OMS verbinding toevoegen](#add-an-oms-connection-to-configuration-manager).
4. Ga in Configuration Manager kunt u [de verbindingseigenschappen bijwerken](#update-oms-connection-properties) als het wachtwoord of client geheime sleutel ooit verloopt of verbroken is.
5. Met de informatie van de portal OMS, [downloadt en installeert u de Microsoft-Agent cmdlets voor controle](#download-and-install-the-agent) op de computer met de verbinding met de Configuration Manager punt site Systeemrol. De agent verzendt Configuration Manager-gegevens naar OMS.
6. In OMS, [verzamelingen van Configuration Manager importeren](#import-collections) als computergroepen.
7. In OMS, bekijkt u gegevens uit Configuration Manager als [computergroepen](log-analytics-computer-groups.md).

U kunt meer informatie over het verbinden van Configuration Manager naar OMS gegevens [uit Configuration Manager naar de Microsoft bewerkingen Management Suite](https://technet.microsoft.com/library/mt757374.aspx)te synchroniseren.



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Machtigingen Configuration Manager bieden tot OMS

De volgende procedure bevat de beheerportal Azure met machtigingen voor toegang tot OMS. Specifiek, moet u de *rol Inzender* verlenen aan gebruikers in de resourcegroep. Op zijn beurt waarmee de beheerportal Azure Configuration Manager verbinden met OMS.

>[AZURE.NOTE] Moet u machtigingen voor OMS voor Configuration Manager. Anders, ontvangt u een foutbericht weergegeven wanneer u met de configuratiewizard in Configuration Manager.


1. Open de [portal van Azure](https://portal.azure.com/) en klik op **Bladeren** > **Log Analytics Kantoorbeheersysteem** openen van het blad Log Analytics Kantoorbeheersysteem.  
2. Klik op het blad **Log Analytics Kantoorbeheersysteem** op **toevoegen** om te openen van het blad **OMS werkruimte** .  
  ![OMS blade](./media/log-analytics-sccm/sccm-azure01.png)
3. Klik op het blad **OMS werkruimte** de onderstaande informatie opgeven en klik vervolgens op **OK**.
  - **OMS-werkruimte**
  - **Abonnement**
  - **Resourcegroep**
  - **Locatie**
  - **Prijzen van laag**  
    ![OMS blade](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Het bovenstaande voorbeeld wordt een nieuwe resourcegroep. De resourcegroep wordt alleen gebruikt om te voorzien Configuration Manager machtigingen voor de werkruimte OMS in dit voorbeeld.

4. Klik op **Bladeren** > **resourcegroepen** openen van het blad **resourcegroepen** .
5. Klik op de resourcegroep die u hierboven hebt gemaakt om te openen in het blad **resourcegroepen** de &lt;groep resourcenaam&gt; instellingen blade.  
  ![resource groep instellingen blade](./media/log-analytics-sccm/sccm-azure03.png)
6. In de &lt;groep resourcenaam&gt; instellingen blade, toegangsbeheer (IAM) te openen klikt u op de &lt;groep resourcenaam&gt; gebruikers blade.  
  ![resource groep gebruikers blade](./media/log-analytics-sccm/sccm-azure04.png)  
7. In de &lt;groep resourcenaam&gt; blade van gebruikers, klikt u op **toevoegen** om te openen van het blad **toevoegen access** .
8. Klik in het blad **toevoegen access** op **Selecteer een rol**en selecteer de rol **Inzender** .  
  ![Selecteer een rol](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klik op **gebruikers toevoegen**, selecteer de gebruiker Configuration Manager, klik op **selecteren**en klik vervolgens op **OK**.  
  ![gebruikers toevoegen](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Een verbinding OMS toevoegen bij Configuration Manager

Om te voegt u een verbinding OMS, moet uw Configuration Manager-omgeving een [verbindingspunt service](https://technet.microsoft.com/library/mt627781.aspx) geconfigureerd voor de onlinemodus.

1. Selecteer in de werkruimte voor **beheer** van Configuration Manager **OMS verbindingslijn**. Hiermee opent u de **Wizard voor OMS gegevensverbinding toevoegen**. Selecteer **volgende**.

2. Klik op het scherm **algemene** Bevestig dat u de volgende acties hebt gedaan en dat u details voor elk item hebt en selecteer **volgende**.
  1. In de beheerportal Azure, hebt u Configuration Manager als een webtoepassing en/of Web API-app en dat u de [client-ID van de registratie](../active-directory/active-directory-integrating-applications.md)hebt geregistreerd.
  2. U hebt een geheime sleutel van de app voor de geregistreerde app in Azure Active Directory gemaakt in de beheerportal Azure.  
  3. U hebt de geregistreerde WebApp met de machtiging voor toegang tot OMS opgegeven in de beheerportal Azure.  
  ![Verbinding met OMS Wizard algemeen pagina](./media/log-analytics-sccm/sccm-console-general01.png)

3. Klik op het scherm **Azure Active Directory** configureren van uw verbindingsinstellingen naar OMS uw **Tenant** , de **Client-ID** en de **Client geheim sleutel** en selecteer **volgende**.  
  ![Verbinding met OMS Wizard Azure Active Directory-pagina](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Als u alle andere procedures is voltooid, klikt u vervolgens weergegeven de gegevens op het scherm **OMS verbindingsconfiguratie** automatisch op deze pagina. Informatie voor de gewenste verbindingsinstellingen moet worden weergegeven voor uw **abonnement op Azure** , **Azure resourcegroep** en **Werkruimte voor beheer Suite bewerkingen**.  
  ![Verbinding met pagina OMS Wizard OMS verbinding](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. De wizard maakt verbinding met de OMS-service met behulp van de informatie die u hebt ingevoerd. Selecteer het apparaat-siteverzamelingen die u wilt synchroniseren met OMS en klik vervolgens op **toevoegen**.  
  ![Selecteer siteverzamelingen](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Controleer of uw verbindingsinstellingen op het scherm **Summary** en selecteer **volgende**. Het scherm **voortgang** ziet u de verbindingsstatus en vervolgens moet **voltooid**.

>[AZURE.NOTE] U moet OMS verbinden met de bovenste laag-site in de hiërarchie. Als u OMS verbinden met een zelfstandig product primaire-site en vervolgens een centraal beheer-site aan uw omgeving toevoegen, hebt u wilt verwijderen en opnieuw maken van de verbinding OMS binnen de nieuwe hiërarchie.

Nadat u Configuration Manager aan OMS hebt gekoppeld, kunt u toevoegen of verwijderen van siteverzamelingen en de eigenschappen van de verbinding OMS weergeven.

## <a name="update-oms-connection-properties"></a>Verbindingseigenschappen OMS bijwerken

Als een wachtwoord of client geheime sleutel ooit verloopt of verbroken is, moet u de verbindingseigenschappen OMS handmatig bijwerken.

1. Ga in Configuration Manager Navigeer naar de **Cloud Services** en selecteer vervolgens **OMS verbindingslijn** plaatsen, opent u de pagina **Eigenschappen van de verbinding OMS** .
2. Klik op het tabblad **Azure Active Directory** om uw **Tenant**, **Cliënt-ID**- **Client geheime belangrijke verlooptijd**weer te geven op deze pagina. **Controleer of** uw **Client geheime sleutel** als dit is verlopen.


## <a name="download-and-install-the-agent"></a>Download en installeer de agent

1. Klik in de OMS portal [het installatiebestand agent uit het Kantoorbeheersysteem downloaden](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Gebruik een van de volgende methoden voor het installeren en configureren van de agent op de computer met de Configuration Manager service verbinding punt site rol:
  - [De agent met configuratie installeren](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [De agent via de opdrachtregel installeren](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [De agent DSC gebruiken in Azure automatisering installeren](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Verzamelingen importeren

Nadat u hebt een verbinding OMS bij Configuration Manager toegevoegd en de agent hebt geïnstalleerd op de computer met de verbinding met de Configuration Manager wijst u de rol van de site-systeem, wordt de volgende stap is om te importeren verzamelingen van Configuration Manager in OMS als computergroepen.

Nadat invoer is ingeschakeld, zijn de lidmaatschapsgegevens siteverzameling om 3 uur opgehaald de lidmaatschappen siteverzameling als actueel wilt houden. U kunt kiezen voor het uitschakelen van de invoer op elk gewenst moment.

1. Klik in de portal OMS klikt u op **Instellingen**.
2. Klik op het tabblad **Computergroepen** en klik op het tabblad **SCCM** .
3. Selecteer **importeren Configuration Manager siteverzameling lidmaatschappen** en klik vervolgens op **Opslaan**.  
  ![Computergroepen - SCCM tabblad](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Gegevens weergeven uit Configuration Manager

Nadat u hebt een verbinding OMS bij Configuration Manager toegevoegd en de agent op de computer met de Configuration Manager service verbinding punt site rol is geïnstalleerd, worden gegevens uit de agent wordt verzonden naar OMS. In OMS, worden uw verzamelingen Configuration Manager als [computergroepen](log-analytics-computer-groups.md)weergegeven. U kunt de groepen uit de **Configuration Manager** -pagina onder **Computergroepen** weergeven in **Instellingen**.

Nadat de verzamelingen zijn geïmporteerd, kunt u zien hoeveel computers met siteverzameling lidmaatschappen zijn gevonden. U ziet ook het aantal siteverzamelingen die zijn geïmporteerd.

![Computergroepen - SCCM tabblad](./media/log-analytics-sccm/sccm-computer-groups02.png)

Wanneer u op een klikt, zoeken wordt geopend op alle van de geïmporteerde groepen of alle computers die deel uitmaakt van elke groep. [Log zoeken](log-analytics-log-searches.md)gebruikt, kunt u uitgebreide analyse voor Configuration Manager-gegevens starten.

## <a name="next-steps"></a>Volgende stappen

- Gebruik de [Zoekfunctie van Log](log-analytics-log-searches.md) gedetailleerde informatie over uw Configuration Manager-gegevens bekijken.
