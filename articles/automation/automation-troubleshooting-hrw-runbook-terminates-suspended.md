<properties
   pageTitle="Hybride Runbook werknemer: Een taak runbook eindigt met de status geschorst | Microsoft Azure"
   description="Symptomen oorzaken en oplossingen voor hybride Runbook werknemer taak beë fout."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybride Runbook werknemer: Een taak runbook eindigt met de status geschorst

## <a name="summary"></a>Overzicht

Uw runbook is geschorst kort na het uitvoeren van deze drie keer. Er zijn situaties waarin het runbook wordt voltooid mogelijk onderbreken en de gerelateerde foutbericht is niet inbegrepen bij eventueel aanvullende informatie die aangeeft dat u waarom. In dit artikel vindt stappen voor probleemoplossing voor met betrekking tot fouten bij de uitvoering van de hybride Runbook werknemer runbook.

Als uw probleem Azure niet wordt beantwoord in dit artikel, bezoekt u de Azure-forums op [MSDN en de stapel overloop](https://azure.microsoft.com/support/forums/). U kunt het probleem posten op deze forums of [ @AzureSupport op Twitter](https://twitter.com/AzureSupport). U kunt ook een verzoek om Azure ondersteuning indienen door te selecteren op de site [Azure ondersteunen](https://azure.microsoft.com/support/options/) **ondersteuning** .

## <a name="symptom"></a>Symptoom

Runbook uitvoering mislukt en wordt de fout als resultaat gegeven, "de taak actie 'Activeren' kan niet worden uitgevoerd, omdat het proces onverwacht gestopt. De actie voor de taak is 3 keer geprobeerd."


## <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken voor de fout: 

  1. De werknemer hybride zich achter een proxy of de firewall
  2. De computer die de werknemer hybride wordt uitgevoerd op is kleiner is dan de minimale hardware- [vereisten](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. De runbooks verifiëren niet met lokale bronnen


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Oorzaak 1: Hybride Runbook werknemer is achter een firewall of proxyserver

De computer die de hybride Runbook werknemer wordt uitgevoerd op bevindt zich achter een firewall of proxyserver-server en uitgaande netwerktoegang niet toegestaan of geconfigureerd.

### <a name="solution"></a>Oplossing

Controleer of de computer uitgaande toegang heeft tot *. cloudapp.net op poort 443, 9354 en 30000-30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Oorzaak 2: Computer heeft minder dan minimale hardwarevereisten

Computers met de hybride Runbook werknemer moeten voldoen aan de minimale hardwarevereisten voordat u deze voor het hosten van deze functie aanwijzen. Anders de computer afhankelijk van het Resourcegebruik van andere achtergrondprocessen en een conflict veroorzaakt door runbooks tijdens de uitvoering, worden meer dan wordt worden gebruikt en ertoe leiden dat runbook taak vertragingen of -outs. 

### <a name="solution"></a>Oplossing 

Controleer eerst de computer ingesteld om uit te voeren van de functie Hybride Runbook werknemer voldoet aan de minimale hardwarevereisten.  Als dat zo is, controleert u processor en geheugen gebruik om te bepalen van een relatie tussen de prestaties van hybride Runbook werknemer processen en Windows.  Als er geheugen of CPU druk, mogelijk dat u een upgrade of extra processors toevoegen of geheugen als u de resource knelpunt te verhelpen van de fout wilt vergroten. U kunt ook een ander berekeningscluster resource selecteren die de minimale vereisten en schaal kunt ondersteund wanneer werkbelasting vraag geven dat een grotere nodig is.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Oorzaak 3: Runbooks niet verifiëren met lokale bronnen

### <a name="solution"></a>Oplossing

Controleer het gebeurtenislogboek van **Microsoft-SMA** voor een bijbehorende gebeurtenis met beschrijving *Win32 proces is afgesloten met code [4294967295]*.  De oorzaak van deze fout wordt u dit nog niet hebt geconfigureerd verificatie in uw runbooks of de referenties uitvoeren als voor de hybride werknemersgroep opgegeven.  Raadpleeg [Runbook machtigingen](automation-hybrid-runbook-worker.md#runbook-permissions) om te bevestigen dat u correct verificatie voor uw runbooks hebt geconfigureerd.  


 

