<properties 
   pageTitle="Syslog berichten in Log Analytics | Microsoft Azure"
   description="Syslog is een gebeurtenis logboekregistratie-protocol die geldt voor Linux.   In dit artikel wordt beschreven hoe verzameling berichten klikt Syslog ter Log analyses en details van de records die ze in de bibliotheek OMS maken configureren."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>De gegevensbronnen Syslog in Log Analytics

Syslog is een gebeurtenis logboekregistratie-protocol die geldt voor Linux.  Toepassingen worden verzonden berichten die kunnen worden opgeslagen op de lokale computer of afgeleverd in een Syslog verzamelen.  Wanneer de OMS-Agent voor Linux is geïnstalleerd, configureert deze de lokale Syslog daemon doorsturen van berichten naar de agent.  Het bericht stuurt de agent naar Log Analytics waar een overeenkomende record in de bibliotheek OMS wordt gemaakt.  

> [AZURE.NOTE]Log Analytics ondersteunt verzameling berichten klikt door rsyslog of syslog-ng verzonden. De standaard syslog daemon op versie 5 van Red rol Enterprise Linux, CentOS en Oracle Linux versie (sysklog) wordt niet ondersteund voor syslog gebeurtenis verzameling. Als u wilt syslog gegevens verzamelen van deze versie van deze onderzoeken, moet de [rsyslog daemon](http://rsyslog.com) geïnstalleerd en geconfigureerd voor het vervangen van sysklog.

![Syslog siteverzameling](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Syslog configureren
De OMS-Agent voor Linux verzamelt alleen gebeurtenissen met de faciliteiten en severities die zijn opgegeven in de configuratie.  U kunt Syslog configureren via de portal van de OMS of met het beheren van configuratiebestanden op uw Linux agenten.


### <a name="configure-syslog-in-the-oms-portal"></a>Syslog in de portal OMS configureren

Syslog in het [menu van de gegevens in Log Analytics-instellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.  Deze configuratie wordt bezorgd in het configuratiebestand op elke agent Linux.

U kunt een nieuwe faciliteit toevoegen door te typen in de naam en te klikken op **+**.  Voor elke faciliteit, worden alleen berichten met de geselecteerde severities worden verzameld.  Controleer de severities voor de bepaalde opslagruimte die u wilt verzamelen.  U kunt geen eventuele aanvullende criteria om berichten te filteren opgeven.

![Syslog configureren](media/log-analytics-data-sources-syslog/configure.png)


Standaard worden alle configuratiewijzigingen worden automatisch verplaatst naar alle agenten.  Als u Syslog handmatig configureren op elke Linux-agent wilt, schakelt u het vak *toepassen onder configuratie voor mijn computers Linux*.


### <a name="configure-syslog-on-linux-agent"></a>Syslog configureren op Linux-agent

Wanneer de die wordt [OMS-agent is geïnstalleerd op een Linux-client](log-analytics-linux-agents.md), wordt er een standaard syslog configuratiebestand die bepaalt welke faciliteit en ernst van de berichten die zijn verzameld.  U kunt dit bestand als u wilt wijzigen van de configuratie wijzigen.  Het configuratiebestand verschilt afhankelijk van de Syslog daemon die de client is geïnstalleerd.

> [AZURE.NOTE] Als u de configuratie syslog bewerkt, kunt u de daemon syslog totdat de wijzigingen van kracht moet opnieuw.

#### <a name="rsyslog"></a>rsyslog

Het configuratiebestand voor rsyslog bevindt zich op **/etc/rsyslog.d/95-omsagent.conf**.  De Standaardinhoud ervan worden hieronder weergegeven.  Dit verzamelt syslog verzonden berichten van de lokale agent voor alle voorzieningen met een niveau van de waarschuwing of hoger.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

U kunt een faciliteit verwijderen door te verwijderen van de sectie van het configuratiebestand.  U kunt de severities die zijn verzameld voor een bepaalde opslagruimte doordat de vermelding van dat faciliteit beperken.  Bijvoorbeeld als u wilt beperken van de functie van de gebruiker aan berichten met een ernst van fout of hoger wijzigt u deze regel van het configuratiebestand als volgt uit:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng

Het configuratiebestand voor rsyslog is locatie op **/etc/syslog-ng/syslog-ng.conf**.  De Standaardinhoud ervan worden hieronder weergegeven.  Dit verzamelt syslog verzonden berichten van de lokale agent voor alle faciliteiten en alle severities.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

U kunt een faciliteit verwijderen door te verwijderen van de sectie van het configuratiebestand.  U kunt de severities die zijn verzameld voor een bepaalde opslagruimte door ze te verwijderen uit de lijst beperken.  U beperkt de gebruiker faciliteit alleen waarschuwingen en kritieke berichten, wijzigt u bijvoorbeeld dat gedeelte van het configuratiebestand als volgt uit:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>De poort Syslog wijzigen

De OMS-agent luistert voor Syslog berichten op de lokale client op poort 25224.  U kunt deze poort wijzigen door de volgende sectie toevoegen aan het Kantoorbeheersysteem agent configuratiebestand **/etc/opt/microsoft/omsagent/conf/omsagent.conf**zich bevindt.  25224 in het fragment **poort** vervangen door het poortnummer in dat u wilt.  Houd er rekening mee dat u ook moet voor het wijzigen van het configuratiebestand voor de daemon Syslog berichten naar deze poort te verzenden.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Gegevens verzamelen

De OMS-agent luistert voor Syslog berichten op de lokale client op poort 25224. Het configuratiebestand voor de daemon Syslog stuurt Syslog verzonden berichten van toepassing op deze poort waar ze worden verzameld door Log Analytics.


## <a name="syslog-record-properties"></a>Syslog recordeigenschappen

Syslog records een soort **Syslog** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Computer | Computer waarop de gebeurtenis is verzameld uit. |
| Faciliteit | Hiermee definieert u het deel van het systeem dat het bericht gegenereerd. |
| HostIP | IP-adres van het systeem dat het bericht verstuurt.  |
| HostName | Naam van het systeem dat het bericht verstuurt. |
| Foutcode | Het prioriteitsniveau van de gebeurtenis. |
| SyslogMessage | Tekst van het bericht. |
| Proces-id | ID van het proces dat het bericht gegenereerd. |
| EventTime | Datum en tijd waarop de gebeurtenis is gegenereerd.



## <a name="log-queries-with-syslog-records"></a>Log query's met Syslog records

De volgende tabel bevat verschillende voorbeelden van log-query's die Syslog records ophalen.

| Query | Beschrijving |
|:--|:--|
| Typ = Syslog | Alle Syslogs. |
| Typ = Syslog SeverityLevel = fout | Alle records Syslog met ernst van fout. |
| Typ = Syslog & #124; eenheid count() door Computer | Tellen van Syslog-records op de computer. |
| Typ = Syslog & #124; count() door faciliteit meten | Tellen van Syslog-records op faciliteit. |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren. 
- [Aangepaste velden](log-analytics-custom-fields.md) gebruiken om te parseren van gegevens uit syslog records in afzonderlijke velden.
- [Agenten Linux configureren](log-analytics-linux-agents.md) voor het verzamelen van andere typen gegevens. 
