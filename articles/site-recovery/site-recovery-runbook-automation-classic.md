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


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Azure automatisering runbooks toevoegen aan herstel abonnementen - klassiek


Deze zelfstudie wordt beschreven hoe herstel van Azure-Site wordt geïntegreerd met Azure automatisering te leveren uitbreiden op herstel-abonnementen. Herstel-abonnementen kunnen goedkeuringen door herstel van uw virtuele machines beveiligd met behulp van Azure Site herstel voor replicatie op secundaire cloud zowel replicatie Azure scenario's. Ze ook helpen bij het maken van de **consistente nauwkeurige**herstel, **herhaald**en **geautomatiseerde**. Als u via uw virtuele machines naar Azure mislukken, wordt integratie met Azure automatisering breidt de herstel-abonnementen en geeft u de mogelijkheid runbooks, zodat krachtige automatiseringstaken uitvoeren.

Als u niet gehoord over Azure automatisering nog, [hier](https://azure.microsoft.com/services/automation/) registreren en hun voorbeeldscripts downloaden [hier](https://azure.microsoft.com/documentation/scripts/). Lees meer over het [Herstellen van Azure-Site](https://azure.microsoft.com/services/site-recovery/) en hoe herstel naar Azure herstel abonnementen met goedkeuringen [hier](https://azure.microsoft.com/blog/?p=166264).

In deze korte zelfstudie gaan we hoe u met Azure automatisering runbooks herstel abonnementen integreren kunt. We eenvoudige taken die eerder tussenkomst vereist automatiseren en lees hoe u een herstel van meerdere stap converteren naar een herstelactie met één klik. We gaan ook bekijken hoe u een eenvoudig script oplossen kunt als deze mis gaat.

## <a name="protect-the-application-to-azure"></a>De toepassing Azure beveiligen

Laat ons beginnen met een eenvoudige toepassing die bestaat uit twee virtuele machines. We hebben hier een toepassing HRweb van Fabrikam. Fabrikam-HRweb-frontend en Fabrikam-Hrweb-backend zijn de twee virtuele machines beveiligde naar Azure met Azure sites worden hersteld. Als u wilt beveiligen in de virtuele machines met Azure sites worden hersteld, de onderstaande stappen uit te voeren.

1.  Beveiliging voor uw virtuele machines inschakelen.

2.  Zorg ervoor dat de virtuele machines eerste replicatie is voltooid en zijn repliceren.

3.  Wacht totdat de eerste replicatie is voltooid en de status herhaling beveiligde staat.

![](media/site-recovery-runbook-automation/01.png)
---------------------

In deze zelfstudie gaan we een herstelplan voor de toepassing Fabrikam HRweb foutherstel voor de toepassing Azure maken. En vervolgens deze met een runbook dat een eindpunt op de mislukte maken wilt integreert via Azure virtuele machines moet fungeren van webpagina's op poort 80.

Eerst een abonnement herstel van de toepassing maken.

## <a name="create-the-recovery-plan"></a>Het abonnement herstel maken

Als u wilt herstellen van de toepassing Azure, moet u een abonnement herstel maken.
Gebruik een herstel-abonnement dat kunt u de volgorde van herstel van de virtuele machines. De virtuele machine in de groep 1 geplaatst wordt herstellen en eerste starten en vervolgens de virtuele machine in de groep 2 volgt.

Maak een Plan voor herstel die lijkt op onder.

![](media/site-recovery-runbook-automation/12.png)

Meer informatie lezen over de abonnementen herstel, documentatie [hier](https://msdn.microsoft.com/library/azure/dn788799.aspx "hier").

Vervolgens de benodigde onderdelen in Azure automatisering maken.

## <a name="create-the-automation-account-and-its-assets"></a>Het account dat automatisering en haar activa maken

Moet u een account Azure automatisering runbooks maken. Als u een account niet al hebt, gaat u naar Azure automatisering tabblad aangeduid met ![](media/site-recovery-runbook-automation/02.png)en een nieuw account maken.

1.  Geef een naam om aan te geven met het account.

2.  Geef een geografisch gebied waar u wilt plaatsen van het account.

Het wordt aanbevolen om het account in hetzelfde gebied, als de ASR kluis.

![](media/site-recovery-runbook-automation/03.png)

Maak vervolgens de volgende elementen in het Account.

### <a name="add-a-subscription-name-as-asset"></a>De naam van een abonnement toevoegen als actief

1.  Een nieuwe instelling toevoegen ![](media/site-recovery-runbook-automation/04.png) in het Azure automatisering activa en selecteer aan![](media/site-recovery-runbook-automation/05.png)

2.  Selecteer het type variabele als **tekenreeks**

3.  Variabelennaam opgeven als **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Geef de naam van uw werkelijke Azure-abonnement als de waarde van de variabele.

    ![](media/site-recovery-runbook-automation/07_1.png)

U kunt de naam van uw abonnement op de instellingenpagina van uw account in de portal van Azure identificeren.

### <a name="add-an-azure-login-credential-as-asset"></a>Een referentie Azure login toevoegen als actief

Azure automatisering Azure PowerShell verbinding maakt met het abonnement en wordt toegepast op de onderdelen er. Hiervoor moet u verifiëren met uw Microsoft-account of een werk- of schoolaccount.
U kunt de accountreferenties opslaan in een activum veilig worden gebruikt door het runbook.

1.  Een nieuwe instelling toevoegen ![](media/site-recovery-runbook-automation/04.png) in het Azure automatisering activa en selecteer![](media/site-recovery-runbook-automation/09.png)

2.  Selecteer het type referentie als **Windows PowerShell referentie**

3.  De naam opgeven als **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Geef de gebruikersnaam en wachtwoord voor het aanmelden met.

Beide deze instellingen zijn nu beschikbaar in uw activa.

![](media/site-recovery-runbook-automation/11.png)

Meer informatie over het verbinding maken met uw abonnement via PowerShell wordt uitgedrukt [hier](../powershell-install-configure.md).

U maakt vervolgens een runbook in Azure automatisering die een eindpunt voor de front virtuele machine na failover kunt toevoegen.

## <a name="azure-automation-context"></a>Azure automatisering context

Een variabele context geeft ASR aan het runbook om te helpen u deterministic scripts schrijven. Een kan voeren aan dat de namen van de Cloudservice en de virtuele Machine zijn overzichtelijk, maar er gebeurt dat dit is niet altijd de hoofdletters/kleine letters als gevolg van bepaalde scenario's zoals het waar de naam van de naam van de VM gevolg van niet-ondersteunde tekens in Azure mogelijk zijn gewijzigd. Deze informatie wordt daarom doorgegeven aan het abonnement ASR herstel als onderdeel van de *context*.

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


Omgaan met de VmMap Key in de context kunt u ook gaat u naar de pagina van de eigenschappen VM in ASR en kijkt u naar de eigenschap VM-GUID.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Auteur een runbook automatisering

Maak nu het runbook om poort 80 op de front virtuele machine te openen.

1.  Een nieuwe runbook maken in het account Azure automatisering met de naam **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Ga naar de weergave van de auteur van het runbook en voer de tekst conceptmodus.

3.  Geef eerst de variabele wilt gebruiken als de context van herstel plannen

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Volgende verbinding maken met het abonnement met de naam van de referentie- en -abonnement

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Houd er rekening mee dat u het Azure activa – **AzureCredential** en **AzureSubscriptionName** hier gebruikt.

5.  Nu de eindpuntdetails en de GUID van de virtuele machine waarvoor u wilt laten zien het eindpunt opgeven. In dit geval de front virtuele machine.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Hiermee geeft u het protocol Azure eindpunt, lokale poort op de VM en de toegewezen openbare poort. Deze variabelen zijn vereist door de Azure-opdrachten die eindpunten aan VMs toevoegen parameters. De VMGUID bevat de GUID van de virtuele machine die u worden uitgevoerd moet op.

6.  Het script wordt nu extraheren van de context voor de opgegeven VM GUID en een eindpunt op de virtuele machine waarnaar wordt verwezen door deze te maken.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Wanneer dit voltooid is, klik op publiceren ![](media/site-recovery-runbook-automation/20.png) toe te staan dat het script kan worden gecontroleerd voor uitvoering.

Het volledige script worden hieronder gegeven voor uw verwijzing

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Het script toevoegen aan het abonnement herstel

Zodra het script klaar is, kunt u deze toevoegen aan het herstelproces-abonnement dat u eerder hebt gemaakt.

1.  Kies in het herstelproces is dat u hebt gemaakt, een script na 2 van de groep toevoegen. ![](media/site-recovery-runbook-automation/15.png)

2.  Geef een scriptnaam. Dit is alleen een beschrijvende naam voor dit script voor waarin binnen het herstelproces-abonnement.

3.  Selecteer in de overname aan Azure script – de accountnaam van Azure automatisering.

4.  Selecteer in de Runbooks Azure het runbook die u hebt samengesteld.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Scripts op primaire

Wanneer u een failover naar Azure uitvoert, kunt u ook uitvoeren van scripts op primaire kiezen. Deze scripts worden uitgevoerd op de server VMM tijdens een overname.
Scripts op primaire zijn alleen beschikbaar zijn alleen voor oude afsluiten en afsluiten fasen posten. Dit is omdat de primaire site worden gewoonlijk niet beschikbaar wanneer een noodgevallen biedt.
Tijdens een een niet-geplande overname, alleen als u ervoor voor primaire sitebewerkingen kiezen, wordt geprobeerd de primaire-scripts worden uitgevoerd. Als ze niet bereikbaar zijn of time-out voor de overname blijft de virtuele machines herstellen.
Scripts op primaire zijn niet beschikbaar voor Sites VMware/fysieke/Hyper-v zonder VMM beveiligde naar Azure - terwijl u failover naar Azure.
Echter wanneer u failback van Azure naar on-premises scripts op primaire (Runbooks) kan worden gebruikt voor alle doelen behalve VMware.

## <a name="test-the-recovery-plan"></a>Het abonnement herstel testen

Zodra u hebt het runbook toegevoegd aan het abonnement waarop u kunt een test failover- en in actie zien. Het verdient altijd om uit te voeren een failover testen om uw toepassing en het abonnement herstellen om ervoor te zorgen dat er geen fouten te testen.

1.  Selecteer het herstelproces is abonnement en een test failover.

2.  Tijdens de uitvoering van het abonnement, kunt u zien of het runbook is uitgevoerd, of niet via de status ervan.

    ![](media/site-recovery-runbook-automation/17.png)

3.  U kunt ook de status van de uitvoering gedetailleerde runbook bekijken op de pagina Azure automatisering taken voor het runbook.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Nadat de overname is voltooid, met uitzondering van het runbook execution resultaat, kunt u zien of de uitvoering gelukt is of niet door de pagina Azure virtuele machines en bekijken via de eindpunten.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Voorbeeldscripts

Hoewel we doorlopen taak van het toevoegen van een eindpunt aan een Azure virtuele machines in deze zelfstudie automatiseren een vaak gebruikt, kunt u een aantal andere krachtige automatiseringstaken met Azure automatisering doen. Microsoft en de community Azure automatisering bieden steekproef runbooks waarmee u kunt aan de slag voor het maken van uw eigen oplossingen en hulpprogramma runbooks, waarin u kunt gebruiken als bouwstenen voor grote automatiseringstaken. Gebruik deze vanuit de galerie en maken van krachtige één muisklik herstelplannen voor uw toepassingen met Azure sites worden hersteld.

## <a name="additional-resources"></a>Aanvullende informatie

[Overzicht van de Azure automatisering] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Overzicht van de Azure automatisering")

[Azure automatisering voorbeeldscripts] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Azure automatisering voorbeeldscripts")
