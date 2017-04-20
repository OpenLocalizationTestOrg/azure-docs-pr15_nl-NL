<properties
   pageTitle="Azure-Beveiligingscentrum en Azure SQL Database-service | Microsoft Azure"
   description="In dit artikel leest hoe Beveiligingscentrum kunt beveiligen van uw databases in Azure SQL-Database."
   services="sql-database"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure-Beveiligingscentrum en Azure SQL Database-service

[Azure Beveiligingscentrum](https://azure.microsoft.com/documentation/services/security-center/) kunt u voorkomen dat, detecteren en reageren op threats. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

In dit artikel leest hoe Beveiligingscentrum kunt beveiligen van uw databases in Azure SQL-Database.

## <a name="why-use-security-center"></a>Waarom Beveiligingscentrum gebruiken?

Beveiligingscentrum kunt u gegevens in SQL-Database doordat inzicht in de beveiliging van al uw servers en databases. Met Beveiligingscentrum, kunt u het volgende doen:

- Beleid voor controle en SQL-Database versleuteling definiëren.
- De beveiliging van de SQL-Database resources over alle abonnementen bewaken.
- Snel kunt herkennen en beveiligingsproblemen verhelpen.
- Meldingen van [Azure SQL-Database bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md)integreren.

Naast het beveiligen van uw resources SQL-Database, biedt Beveiligingscentrum ook beveiliging voor controle en beheer voor Azure virtuele machines, Cloudservices, App-Services, virtuele netwerken en meer. Meer informatie over Beveiligingscentrum [hier](security-center-intro.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt beginnen met Beveiligingscentrum, moet u een abonnement op Microsoft Azure hebben. De gratis laag van Beveiligingscentrum is aan uw abonnement ingeschakeld. Zie [Beveiliging Center prijzen](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie over de gratis en standaard lagen van Beveiligingscentrum.

Beveiligingscentrum ondersteunt Rolgebaseerd-toegang. Zie voor meer informatie over Rolgebaseerd toegangsbeheer (RBAC) in Azure wordt aangegeven, [Toegangsbeheer op basis van Azure Active Directory-rol](../active-directory/role-based-access-control-configure.md). Veelgestelde vragen over de beveiliging-beheercentrum wordt aandacht besteed aan [hoe de machtigingen in Beveiligingscentrum worden afgehandeld](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Access-Beveiligingscentrum

U opent Beveiligingscentrum vanuit de [Azure-portal](https://azure.microsoft.com/features/azure-portal/). [Meld u aan bij de portal](https://portal.azure.com/) en selecteer de **optie Beveiligingscentrum**.

![De optie Beveiligingscentrum][1]

Hiermee opent u het blad **Beveiligingscentrum** .
![Beveiligingscentrum blade][2]

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen

Een beveiligingsbeleid definieert de reeks besturingselementen die worden aanbevolen voor resources binnen de opgegeven abonnement of de resourcegroep. In Beveiligingscentrum definieert u beleidsregels voor uw abonnementen of resourcegroepen op basis van de behoeften van de beveiliging van uw bedrijf en het type van toepassingen of gevoeligheid van de gegevens in elk abonnement.

U kunt een beleid om weer te geven aanbevelingen voor het controleren van de SQL- en SQL transparante gegevens-versleuteling (TDE) instellen.

- Wanneer u **SQL controle en bedreiging detectie**inschakelt, wordt Beveiligingscentrum aanbevolen dat controle van access met Azure-Database worden ingeschakeld voor naleving, geavanceerde detectie en onderzoek doeleinden.
- Als u **SQL transparante gegevens-versleuteling**, Beveiligingscentrum aanbevolen inschakelt worden die versleuteling in rust ingeschakeld voor uw Azure SQL-Database, gekoppeld back-ups, en transactie-logboekbestanden.

Als u een beveiligingsbeleid, selecteert u de tegel **beleid** op het blad Beveiligingscentrum. Selecteer het abonnement waarop u wilt het beveiligingsbeleid inschakelen op het blad **beveiligingsbeleid** . Selecteer **beleid voor voorkoming** en **Schakel de beveiligingsaanbevelingen die u wilt gebruiken op dit abonnement** .
![Beveiligingsbeleid][3]

Zie [beveiligingsbeleid voor apparaten instellen](security-center-policies.md)voor meer informatie.

## <a name="manage-security-recommendation"></a>Beveiliging aanbeveling beheren

Beveiligingscentrum analyseert regelmatig de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden mogelijke beveiligingsrisico's aangegeven, wordt deze aanbevelingen gemaakt. De aanbevelingen helpt u bij het proces van het configureren van de benodigde besturingselementen.

Nadat u een beveiligingsbeleid hebt ingesteld, analyseert Beveiligingscentrum de beveiligingsstatus van uw resources identificeren mogelijke problemen. De aanbevelingen worden weergegeven in een tabel waarin elke regel één bepaalde aanbeveling vertegenwoordigt. De volgende tabel als een overzicht gebruiken om u te helpen u meer informatie over de beschikbare aanbevelingen voor Azure SQL-Database en wat elke aanbeveling betekent als u deze hebt toegepast. Een aanbeveling selecteren Hiermee, gaat u naar een artikel waarin wordt uitgelegd hoe u het implementeren van de aanbeveling in Beveiligingscentrum.

| Aanbeveling | Beschrijving |
| ----- | ----- |
| [Controle- en bedreiging detectie op SQL-servers inschakelen](security-center-enable-auditing-on-sql-servers.md) | Wordt aanbevolen dat u voor SQL-Database servers controle en bedreiging detectie inschakelen. (Alleen voor SQL-Database-service. Zijn niet opgenomen Microsoft SQL Server waarop uw virtuele machines.) |
| [Controle- en bedreiging detectie op SQL-databases inschakelen](security-center-enable-auditing-on-sql-databases.md) | Wordt aanbevolen dat u voor SQL-Database databases controle en bedreiging detectie inschakelen. (Alleen voor SQL-Database-service. Zijn niet opgenomen Microsoft SQL Server waarop uw virtuele machines.) |
| [Transparante gegevenscodering inschakelen](security-center-enable-transparent-data-encryption.md) | Aanbevolen versleuteling voor SQL-databases in te schakelen. (Alleen SQL-Database-service). |

Als u wilt zien aanbevelingen voor uw Azure resources, selecteert u de tegel **aanbevelingen** op het blad Beveiligingscentrum. Selecteer op het blad **aanbevelingen** een aanbeveling details kunnen zien. In dit voorbeeld Selecteer we **controle inschakelen en bedreiging detectie op SQL-servers**.

![Aanbevelingen][4]

Zoals hieronder wordt weergegeven, ziet u Beveiligingscentrum de SQL-servers waar controle en bedreiging detectie zijn niet is ingeschakeld. Nadat u controle, kunt u de instellingen voor bedreiging detectie en e-mailinstellingen om te ontvangen beveiligingsmeldingen kunt configureren. Bedreiging detectie wordt u gewaarschuwd wanneer wordt vastgesteld afwijkende databaseactiviteiten die mogelijke beveiligingsrisico's met de database aangeven. De waarschuwingen worden weergegeven in het dashboard Beveiligingscentrum.
![Controle- en bedreiging detectie][5]

Volg de stappen in de [aan de slag met SQL Database bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md) kunt inschakelen en configureren van bedreiging detectie en voor het configureren van de lijst met e-mailberichten die beveiligingsmeldingen na detectie van afwijkende activiteiten ontvangt.

Zie voor meer informatie over aanbevelingen, [aanbevelingen voor de beveiliging beheren](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Monitor beveiliging systeemstatus

Nadat u [beveiligingsbeleid voor apparaten](security-center-policies.md) voor resources van een abonnement hebt ingeschakeld, wordt Beveiligingscentrum de beveiliging van uw resources identificeren potentiële problemen analyseren.  U kunt de beveiligingsstatus van uw resources weergeven in de tegel **Resource beveiliging status** . Wanneer u **gegevens** in de tegel **Resource beveiliging systeemstatus** klikt, wordt het blad **Gegevensbronnen** geopend met SQL-aanbevelingen voor problemen zoals controle en transparante gegevensversleuteling niet is ingeschakeld. Er wordt ook aanbevelingen voor de algemene status van de database.
![Status van resource-beveiliging][6]

Meer informatie raadpleegt u [beveiliging statuscontrole](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Beheren en reageren op beveiligingsmeldingen

Beveiligingscentrum automatisch worden verzameld, worden geanalyseerd en integreert logboekgegevens van [Azure SQL bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md), evenals andere Azure bronnen, naar reële threats detecteren en onrechte te verkleinen. Een lijst met prioriteit beveiligingsmeldingen wordt weergegeven in Beveiligingscentrum samen met de gegevens die u snel onderzoeken het probleem en aanbevelingen voor hoe moet u een een aanval te verhelpen.

Als u wilt zien waarschuwingen, selecteert u de tegel **beveiligingsmeldingen** op het blad Beveiligingscentrum. Selecteer op het blad **beveiligingsmeldingen** een waarschuwing voor meer informatie over de gebeurtenissen die de waarschuwing en wat, geactiveerd als een, stappen u moet uitvoeren om te verhelpen van een aanval. Selecteer in dit voorbeeld laten we **mogelijke SQL webweergave**.
![Beveiligingsmeldingen][7]

Zoals hieronder wordt weergegeven, Beveiligingscentrum vindt u aanvullende details die inzicht in wat de melding, de doel-resource, indien van toepassing geactiveerd bieden de bron-IP-adres en aanbevelingen over het moet worden hersteld.
![Mogelijke SQL webweergave][8]

Zie voor meer informatie, [beheren en beantwoorden van beveiligingsmeldingen](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Volgende stappen

- [Veelgestelde vragen over beveiliging-beheercentrum](security-center-faq.md) : veelgestelde vragen over het gebruik van de service zoeken.
- [Handleiding voor het plannen en bewerkingen van Beveiligingscentrum](security-center-planning-and-operations-guide.md) - opvolgen een reeks stappen en taken kunt optimaliseren van het gebruik van Beveiligingscentrum gebaseerd op de beveiligingsvereisten die zijn en de cloud-beheermodel van uw organisatie.
- [Gegevens van de beveiliging-beheercentrum](security-center-data-security.md) – leert u hoe Beveiligingscentrum worden verzameld en gegevens over uw Azure resources, zoals configuratiegegevens, metagegevens, gebeurtenislogboeken, vastlopen bestanden en meer verwerkt.
- Voor de [verwerking van-incidenten](security-center-incident.md) - informatie over het gebruik van de waarschuwing mogelijkheid beveiliging in Beveiligingscentrum helpt u bij het afhandelen van-incidenten.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
