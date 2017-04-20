<properties
   pageTitle="Azure automatisering runbooks toevoegen aan abonnementen herstel | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe herstel van Azure-Site kunt u nu herstel abonnementen met Azure automatisering complexe taken uit te voeren tijdens het herstellen naar Azure uitbreiden"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure automatisering runbooks toevoegen aan herstel-abonnementen


Deze zelfstudie wordt beschreven hoe herstel van Azure-Site wordt geïntegreerd met Azure automatisering te leveren uitbreiden op herstel-abonnementen. Herstel-abonnementen kunnen goedkeuringen door herstel van uw virtuele machines beveiligd met behulp van Azure Site herstel voor replicatie op secundaire cloud zowel replicatie Azure scenario's. Ze ook helpen bij het maken van de **consistente nauwkeurige**herstel, **herhaald**en **geautomatiseerde**. Als u via uw virtuele machines naar Azure mislukken, wordt integratie met Azure automatisering breidt de herstel-abonnementen en geeft u de mogelijkheid runbooks, zodat krachtige automatiseringstaken uitvoeren.

Als u niet gehoord over Azure automatisering nog, [hier](https://azure.microsoft.com/services/automation/) registreren en hun voorbeeldscripts downloaden [hier](https://azure.microsoft.com/documentation/scripts/). Lees meer over het [Herstellen van Azure-Site](https://azure.microsoft.com/services/site-recovery/) en hoe herstel naar Azure herstel abonnementen met goedkeuringen [hier](https://azure.microsoft.com/blog/?p=166264).

In deze zelfstudie gaan we hoe u met Azure automatisering runbooks herstel abonnementen integreren kunt. We eenvoudige taken die eerder tussenkomst vereist automatiseren en lees hoe u een herstel meerdere stappen converteren naar een herstelactie met één klik. We gaan ook bekijken hoe u een eenvoudig script oplossen kunt als deze mis gaat.

## <a name="customize-the-recovery-plan"></a>Het abonnement herstel aanpassen

1. Laat ons eerst operning het blad resource van het abonnement herstel. Het abonnement herstel heeft twee virtuele machines toegevoegd voor herstel, kunt u zien. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Klik op de knop aanpassen om te beginnen met het toevoegen van een runbook. Hiermee opent u het abonnement herstel blade aanpassen.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Klik met de rechtermuisknop op de groep start 1 en selecteer een 'bericht actie toevoegen' toevoegen.

4. Selecteren tot het kiezen van een script in het nieuwe blad.

5. Het script 'Hallo allemaal' een naam.

6. Kies de naam van een automatisering-Account. Dit is de automatisering Azure-account. Houd er rekening mee dat dit account in een Azure Geografie worden kan, maar moet zich in hetzelfde abonnement als de Site herstel kluis.

7. Selecteer een runbook in het automatisering-Account. Dit is het script die wordt uitgevoerd tijdens de uitvoering van het abonnement herstel nadat het herstelproces is van de eerste groep.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Selecteer OK om op te slaan het script. Hiermee wordt het script toegevoegd aan de groep bericht acties van groep 1: beginnen.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Belangrijke punten van het toevoegen van een script

1. U kunt Klik met de rechtermuisknop op het script en kies 'delete stap' of 'bijwerken script'.

2. Een script op de Azure terwijl failover kunt uitvoeren vanuit On-premises naar Azure en kan worden uitgevoerd op Azure als een script op de primaire voordat de afgesloten, tijdens failback van Azure naar on-premises implementatie.

3. Wanneer een script wordt uitgevoerd, worden deze een herstel abonnement context gevoegd.

Hieronder volgt een voorbeeld van hoe de context variabele eruit ziet.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


De onderstaande tabel bevat de naam en beschrijving voor elke variabele in de context.

**De naam van de variabele** | **Beschrijving**
---|---
RecoveryPlanName | De naam van abonnement wordt uitgevoerd. Helpt u op basis van naam met hetzelfde script
FailoverType | Hiermee geeft u of de overname is testen, of niet gepland.
FailoverDirection | Geef aan of herstel naar primaire of secundaire
Groeps-id | De groep getal in het abonnement herstel bepalen wanneer het abonnement wordt uitgevoerd
VmMap | Matrix van de virtuele machines in de groep
VMMap-toets | Unieke sleutel (GUID) voor elke VM. Dit is hetzelfde als de VMM-ID van de virtuele machine indien van toepassing.
Functienaam | Naam van de Azure VM die worden hersteld
CloudServiceName | Azure Cloudservice naam waaronder de virtuele machine is gemaakt.
CloudServiceName (in resourcemanager implementatiemodel) | Azure resourcegroep naam waaronder de virtuele machine is gemaakt.


## <a name="using-complex-variables-per-recovery-plan"></a>Complex variabelen per plan voor herstel gebruiken

Soms kan vereist een runbook voor meer informatie dan alleen de RecoveryPlanContext. Er is geen eerste-klas methode voor het doorgeven van een parameter aan een runbook. Echter als u wilt gebruiken, hetzelfde script via meerdere herstel-abonnementen u kunt u de variabele herstel plannen Context 'RecoveryPlanName' gebruiken en de onder technieken voor het gebruik van een variabele van Azure automatisering complex in een runbook. Het volgende voorbeeld ziet u hoe u kunt drie verschillende complex variabele activa maken en gebruiken in het runbook op basis van de naam van het abonnement herstel.

Houd rekening met dat u wilt gebruiken 3 aanvullende parameters in een runbook. Laat ons coderen in een formulier JSON {"Var1": "testautomation", "Var2": "Ongeplande", "Var3": "PrimaryToSecondary"}

[AA complex variabele](../automation/automation-variables.md#variable-types Complex variable) gebruiken om een nieuwe automatisering activum te maken.
Naam van de variabele als <RecoveryPlanName>- parameters.
U kunt de verwijzing hier gebruiken om te maken van een [complex variabele](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

Een naam geven voor verschillende herstel abonnementen voor de variabele als

1. recoveryPlanName1 >-parameters
2. recoveryPlanName2 >-parameters
3. recoveryPlanName3 >-parameters

Nu in het script verwijzen naar de parameters als

1. De naam van de RP krijgen van de $rpname = $Recoveryplancontext variabele
2. Ophalen van de activa van $paramValue = "$($rpname)-parameters"
3. Gebruik dit als een variabele complex voor het plan herstellen door te bellen Get-AzureAutomationVariable [-AutomationAccountName] <String> -$paramValue een naam.

Als voorbeeld, als u de complex variabele/parameter voor het abonnement waarop SharepointApp herstel, maak een Azure automatisering complex variabele met de naam 'SharepointApp-parameters'.

In het abonnement herstel gebruiken door het extraheren van de variabele uit het actief met de instructie Get-AzureAutomationVariable [-AutomationAccountName] <String> [-naam] $paramValue. [Overzicht van dit voor meer informatie](https://msdn.microsoft.com/library/dn913772.aspx)

Deze manier hetzelfde script kan worden gebruikt voor verschillende herstel-abonnement door de specifieke complex-variabele van abonnement in de activa opslaan.

## <a name="sample-scripts"></a>Voorbeeldscripts

Voor een bibliotheek van scripts die u rechtstreeks in uw account automatisering importeren kunt, raadpleegt u [de Kristian Nese OMS opslagplaats voor scripts](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Het script hier is een resourcemanager Azure-sjabloon die al implementeren de onder scripts

* NSG

Het runbook NSG wordt openbare IP-adressen voor elke VM binnen het Plan voor herstel toewijzen en hun virtuele netwerkadapters wilt toevoegen aan een netwerk-beveiligingsgroep die wordt standaard communicatie toestaan

* PublicIP

Het openbare IP-runbook wordt openbare IP-adressen toewijzen aan elke VM binnen het Plan voor herstel. Toegang tot de toepassingen en machines hangt af van de instellingen van de firewall binnen elke gast


* CustomScript

Het runbook CustomScript wordt openbare IP-adressen toewijzen aan elke VM binnen het Plan voor herstel en extensie aangepast script dat klikt, wordt het script dat u naar tijdens de implementatie van de sjabloon verwijzen installeren

* NSGwithCustomScript

De NSGwithCustomScript runbook openbare IP-adressen wordt toegewezen aan elke VM binnen het herstelproces is Plan installeren een aangepast script met de extensie en verbinden met de virtuele netwerkadapters een NSG toestaan standaard binnenkomende en uitgaande communicatie voor externe toegang

## <a name="additional-resources"></a>Aanvullende informatie

[Azure Automatiseringsservice uitvoeren als Account](../automation/automation-sec-configure-azure-runas-account.md)

[Overzicht van de Azure automatisering] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Overzicht van de Azure automatisering")

[Azure automatisering voorbeeldscripts] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Azure automatisering voorbeeldscripts")
