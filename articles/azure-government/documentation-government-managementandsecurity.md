<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Dit vindt u een vergelijking van functies en informatie over het ontwikkelen van toepassingen voor de overheid van Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure overheid Management en beveiliging

## <a name="automation"></a>Automatisering

Automatisering is in het algemeen beschikbaar zijn in Azure overheid.

### <a name="variations"></a>Variaties

De volgende automatisering-functies zijn niet momenteel beschikbaar in Azure overheid.

+ Het maken van een Service principe referentie voor verificatie

Zie [automatisering openbare documentatie](../automation/automation-intro.md)voor meer informatie.


##  <a name="key-vault"></a>Belangrijke kluis
Zie voor meer informatie over deze service en hoe u dit gebruikt, de <a href="https://azure.microsoft.com/documentation/services/key-vault">openbare documentatie Azure-toets kluis.</a>
### <a name="data-considerations"></a>Aandachtspunten voor de gegevens
De volgende informatie geeft u de grens Azure overheid voor Azure-toets kluis:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens worden versleuteld met een sleutel Azure-toets kluis kan Regulated/beheerde gegevens bevatten. | Azure-toets kluis metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Deze metagegevens bevat alle configuratiegegevens ingevoerd bij het maken en onderhouden van uw sleutel kluis.  Voer geen Regulated/gebruikersgegevens in de volgende velden: groep resourcenamen, sleutel kluis namen, de naam van abonnement |

Toets kluis is in het algemeen beschikbaar zijn in Azure overheid. Zoals in openbare is het geen extensie, dus toets kluis alleen beschikbaar via PowerShell en CLI is.
## <a name="log-analytics"></a>Log Analytics
Log Analytics vindt u in het algemeen in Azure overheid. 

### <a name="variations"></a>Variaties

De volgende Log Analytics-functies en oplossingen zijn niet momenteel beschikbaar in Azure overheid. Deze lijst wordt bijgewerkt wanneer de status van functies / oplossingen die zijn gewijzigd.

+ Oplossingen die in de proefversie van in openbare Azure wordt aangegeven zijn, waaronder:
  - Controle-oplossing
  - Controle van toepassingen afhankelijkheid
  - Office 365-oplossing
  - Windows 10 upgraden Analytics-oplossing
  - Toepassing inzichten
  - Azure toegang Analytics-oplossing
  - Azure automatisering Analytics
  - Belangrijke kluis Analytics
+ Oplossingen en functies die opgevolgd updates voor on-premises implementatie-software moeten, inclusief
  - Integratie met System Center Operations Manager 2016 (eerdere versies van Operations Manager worden ondersteund)
  - Computers groepen van System Center Configuration Manager
  - Surface Hub-oplossing
+ Functies die in de proefversie van in openbare Azure wordt aangegeven zijn, inclusief
  - Exporteren van gegevens naar PowerBI
+ Azure maatstaven en Azure diagnostische gegevens
+ OMS mobiele toepassingen

De URL's voor Log Analytics zijn Azure overheid:

| Azure openbare | Azure overheid | Notities |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.us | Meld u Analytics-portal |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Gegevens verzamelen API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent communicatie - [firewallinstellingen configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent communicatie - [firewallinstellingen configureren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent communicatie - [firewallinstellingen configureren](../log-analytics/log-analytics-proxy-firewall.md) |


De volgende Log Analytics-functies hebben verschillende gedrag in Azure overheid:

+ De Windows-agent moet worden gedownload van de [Log Analytics-portal](https://oms.microsoft.us) voor de overheid Azure.
+ Als u wilt uw systeem Center Operations Manager management server verbinden met Log analyses, die u wilt downloaden en bijgewerkte Management Packs importeren.
  1. Download en sla de [management packs bijgewerkt](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Pak het bestand dat u hebt gedownload
  3. Importeer de packs management in Operations Manager. Zie het onderwerp van [het importeren van een Management Pack voor Operations Manager](http://technet.microsoft.com/library/hh212691.aspx) op de Microsoft TechNet-website voor informatie over het importeren van een management pack vanaf een schijf.
  4. Operations Manager Log Analytics verbinding wordt u de stappen in [Verbinding maken met Operations Manager naar Log Analytics](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Veelgestelde vragen

+ Kan ik gegevens migreren van Log Analytics in openbare Azure-Azure overheid?
  - Nee. Het kan niet verplaatsen van gegevens of uw werkruimte van openbare Azure-Azure overheid.
+ Kan ik overschakelen tussen openbare Azure en Azure overheid werkruimten van de portal OMS Log Analytics?
  - Nee. De portals voor openbare Azure en Azure overheid afzonderlijk zijn en geen inhoud delen. 

Zie [openbare Log Analytics-documentatie](../log-analytics/log-analytics-overview.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Aanvullende informatie en updates, abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
 
