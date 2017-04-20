<properties
   pageTitle="Azure Beveiligingscentrum gids voor probleemoplossing | Microsoft Azure"
   description="In dit document helpt bij het oplossen van problemen in Beveiligingscentrum Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Azure Beveiligingscentrum Troubleshooting Guide
Deze handleiding is bedoeld voor IT-specialisten (IT), informatie beveiliging analisten en cloud beheerders waarvan organisaties Beveiligingscentrum Azure gebruikt en problemen met de dat Beveiligingscentrum gerelateerde problemen.

## <a name="troubleshooting-guide"></a>De gids voor probleemoplossing
Deze handleiding wordt uitgelegd hoe u het oplossen van problemen met de Beveiligingscentrum. De meeste van de troubleshooting klaar in Beveiligingscentrum vindt plaats door eerst te zoeken naar de [Controlelogboek](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) -records voor het onderdeel is mislukt. Via controlelogboeken bijhouden, kunt u het volgende bepalen:

- Welke bewerkingen zijn plaatsgevonden
- Wie de bewerking gestart
- Wanneer de bewerking is opgetreden
- De status van de bewerking
- De waarden van andere eigenschappen die u u helpen kunnen onderzoeken de bewerking

Het controlelogboek bevat alle schrijven-bewerkingen (opgeslagen, bericht, DELETE) die wordt uitgevoerd op uw bronnen, maar deze bevat geen gelezen bewerkingen (GET).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Problemen oplossen bij controle agent-installatie in Windows

Beveiligingscentrum agent cmdlets voor controle wordt gebruikt voor het verzamelen van gegevens uitvoeren. Nadat het verzamelen van gegevens is ingeschakeld en de agent juist is geïnstalleerd in de doelcomputer, moet deze processen worden uitgevoerd:

- ASMAgentLauncher.exe - Azure Agent bewaken 
- ASMMonitoringAgent.exe - extensie Azure beveiliging bewaken
- ASMSoftwareScanner.exe – Azure Scan Manager

De extensie Azure beveiliging Monitoring zoekt naar verschillende beveiliging relevante configuratie en verzamelt beveiligingslogboeken aan de van de virtuele machine. De manager gescande afbeelding wordt gebruikt als een scanner patch.

Als de installatie is uitgevoerd ziet u een vermelding die vergelijkbaar is met de onder in de controlelogboeken voor het doel VM:

![Controlelogboeken bijhouden](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Verder kunt u meer informatie over het installatieproces door te lezen de logboeken agent, vinden op *%systemdrive%\windowsazure\logs* (voorbeeld: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Als de Azure Security Center Agent problemen oplevert, moet u het doel VM opnieuw omdat er geen opdracht om te stoppen en agent te starten.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Problemen oplossen bij controle agent-installatie in Linux
Bij het oplossen van VM Agent installatie in een Linux-systeem moet u ervoor zorgen dat de extensie is gedownload naar/var/bibliotheek/waagent /. De opdracht hieronder om te controleren of als dit is geïnstalleerd, kunt u uitvoeren:

`cat /var/log/waagent.log` 

De andere logboekbestanden die u kunt bekijken voor het oplossen van doel zijn: 

- /var/log/mdsd.Err
- / var/log/azure /

In een werkend systeem ziet u een verbinding met het proces mdsd op TCP 29130. Dit is de syslog communiceren met het proces mdsd. U kunt dit gedrag valideren door de volgende opdracht uit te voeren:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Contact opnemen met Microsoft ondersteuning

Enkele problemen kunnen worden geïdentificeerd aan de hand van de richtlijnen voorwaarde in dit artikel anderen die u kunt ook vinden op de openbare Beveiligingscentrum- [Forum beschreven](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Echter als u meer problemen oplossen, kunt u een nieuwe ondersteuning nieuw vergaderverzoek met behulp van Azure-Portal, zoals hieronder openen: 

![Microsoft ondersteuning](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum configureren. Zie de volgende onderwerpen voor meer informatie over Azure Beveiligingscentrum:

- [Bewerkingen handleiding en Azure beveiliging Center plannen](security-center-planning-and-operations-guide.md) , informatie over het plannen en meer informatie over de ontwerpoverwegingen Azure Beveiligingscentrum vast te stellen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md) , informatie over het controleren van de status van uw Azure resources
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen
- [Partneroplossingen met Azure Beveiligingscentrum Monitoring](security-center-partner-solutions.md) , informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over beheercentrum voor Azure-beveiliging](security-center-faq.md) : veelgestelde vragen over het gebruik van de service zoeken
- [Azure Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) , zoeken weblogberichten over Azure beveiliging en naleving
