<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Hierdoor wordt een vergelijking van functies en informatie over het ontwikkelen van toepassingen voor de overheid Azure."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure overheid voor controle en beheer

In dit artikel worden de cmdlets voor controle en beheer services variaties en aandachtspunten voor de overheid Azure-omgeving.

## <a name="automation"></a>Automatisering

Automatisering is in het algemeen beschikbaar zijn in Azure overheid.

### <a name="variations"></a>Variaties

De volgende automatisering-functies zijn niet momenteel beschikbaar in Azure overheid.

+ Het maken van een Service principe referentie voor verificatie

Zie [automatisering openbare documentatie](../automation/automation-intro.md)voor meer informatie.

## <a name="log-analytics"></a>Log Analytics

Log Analytics vindt u in het algemeen in Azure overheid.

### <a name="variations"></a>Variaties

De volgende Log Analytics-functies en oplossingen zijn niet momenteel beschikbaar in Azure overheid.

+ Oplossingen die in het voorbeeld in Microsoft Azure, waaronder:
  - Controle-oplossing
  - Afhankelijkheid Monitoring oplossing
  - Office 365-oplossing
  - Windows 10 upgraden Analytics-oplossing
  - Oplossing inzichten
  - Azure toegang Analytics-oplossing
  - Azure automatisering Analytics-oplossing
  - Toets kluis Analytics-oplossing
+ Oplossingen en functies die opgevolgd updates voor on-premises implementatie-software moeten, waaronder:
  - Integratie met System Center Operations Manager 2016 (eerdere versies van Operations Manager worden ondersteund)
  - Computers groepen van System Center Configuration Manager
  - Surface Hub-oplossing
+ Functies die in de proefversie van in openbare Azure wordt aangegeven zijn, waaronder:
  - Exporteren van gegevens voor Power BI
+ Azure maatstaven en Azure diagnostische gegevens
+ Bewerkingen Management Suite mobiele toepassingen

De URL's voor Log Analytics zijn Azure overheid:

| Azure openbare | Azure overheid | Notities |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.us | Meld u Analytics-portal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Gegevens verzamelen API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent communicatie - [firewallinstellingen configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent communicatie - [firewallinstellingen configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent communicatie - [firewallinstellingen configureren](../log-analytics/log-analytics-proxy-firewall.md) |


De volgende Log Analytics-functies werken anders in Azure overheid:

+ De Windows-Agent moet worden gedownload van de [Log Analytics-portal](https://oms.microsoft.us) voor de overheid Azure.
+ Als u wilt uw systeem Center Operations Manager management server verbinden met Log analyses, die u wilt downloaden en bijgewerkte management packs importeren.
  1. Download en sla de [management packs bijgewerkt](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Pak het bestand dat u hebt gedownload.
  3. Importeer de packs management in Operations Manager. Zie [het importeren van een Management Pack voor Operations Manager](http://technet.microsoft.com/library/hh212691.aspx) op de Microsoft TechNet-website voor informatie over het importeren van een management pack vanaf een schijf.
  4. Volg de stappen in [Verbinding maken met Operations Manager naar Log Analytics](../log-analytics/log-analytics-om-agents.md)verbinding Operations Manager Log Analytics wordt.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

+ Kan ik gegevens migreren van Log Analytics in Microsoft Azure naar Azure overheid?
  - Nee. Het kan niet verplaatsen van gegevens of uw werkruimte van Microsoft Azure naar Azure overheid.
+ Kan ik overschakelen tussen Microsoft Azure en Azure overheid werkruimten van de portal bewerkingen Management Suite Log Analytics?
  - Nee. De portals voor Microsoft Azure en Azure overheid afzonderlijk zijn en geen inhoud delen.

Zie [openbare Log Analytics-documentatie](../log-analytics/log-analytics-overview.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Aanvullende informatie en updates, abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
