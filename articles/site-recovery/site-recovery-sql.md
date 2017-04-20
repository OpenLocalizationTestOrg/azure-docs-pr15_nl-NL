<properties 
    pageTitle="SQL Server beveiligen met SQL Server-herstel en Azure Site herstel | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe repliceren van SQL Server Azure Site herstel van SQL Server-voorzieningen voor noodgevallen." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>SQL Server met SQL Server-herstel en Azure Site herstel beveiligen 


De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md).

 In dit artikel wordt beschreven hoe de SQL Server-back-end van een toepassing via een combinatie van SQL Server BCDR technologieën en Azure Site herstel beveiligen. Er is een goed inzicht in SQL Server noodgevallen herstel mogelijkheden (failoverclusters, AlwaysOn beschikbaarheidsgroepen, database spiegelen, meld u verzending) en van Azure sites worden hersteld, voordat u de scenario's beschreven in dit artikel implementeert.



## <a name="overview"></a>Overzicht

SQL Server veel werkbelasting gebruiken als basis. SQL Server gebruik-toepassingen zoals SharePoint, Dynamics en SAP willen implementeren gegevensservices.  SQL Server implementeren toepassingen in een aantal verschillende manieren:

- **Zelfstandige versie van SQL Server**: SQL Server en alle databases worden gehost op één computer (fysieke of een virtuele). Wanneer een gevirtualiseerde, worden host cluster wordt gebruikt voor lokale beschikbaarheid. De beschikbaarheid van geen Gast niveau wordt geïmplementeerd.
- **SQL Server Failover cluster exemplaren (altijd op FCI)**: twee of meer knooppunten van SQL server-exemplaren met gedeelde schijven zijn geconfigureerd in een Windows-failovercluster. Als een van de exemplaren cluster niet actief is, mislukt het cluster naar een ander exemplaar over SQL Server. Deze instelling wordt meestal gebruikt voor HA op een primaire-site. Deze beveiligen niet tegen mislukt of -storing in de laag gedeelde opslag. Gedeeld schijf kan worden geïmplementeerd ISCSI, glasvezelkanaal of VHDx gedeeld met.
- **SQL altijd op beschikbaarheidsgroepen**: In deze instellingen zijn twee knooppunten-instelling in een gedeelde niets cluster met SQL Server-databases die zijn geconfigureerd in een beschikbaarheidsgroep met synchroon herhaling en automatische failover.

In de Enterprise-edities biedt SQL Server ook systeemeigen noodgevallen herstel technologieën voor databases met een externe site worden hersteld. In dit artikel we gebruikmaken en integreren met deze systeemeigen SQL noodgevallen herstel technologieën: 

- SQL altijd op beschikbaarheidsgroepen voor herstel voor SQL Server 2012 of 2014 Enterprise Edition.
- SQL database spiegelen in de modus Hoog voor SQL Server Standard edition (alle versies) of voor SQL Server 2008 R2.


Herstel van de site kunt SQL Server beveiligen, zoals in de tabel is samengevat.

 |**On-premises implementatie naar on-premises implementatie** | **On-premises naar Azure** 
---|---|---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja 
**Fysieke server** | Ja | Ja


## <a name="support-and-integration"></a>Ondersteuning en integratie

Deze SQL Server-versies worden ondersteund door de scenario's in dit artikel:


- SQL Server 2014 Enterprise en standaard
- SQL Server 2012 Enterprise en Standard
- SQL Server 2008 R2 Enterprise en standaard


Herstel van de site kan worden geïntegreerd met systeemeigen SQL Server BCDR technologieën samengevat in de onderstaande tabel om aan te bieden een noodgevallen hersteloplossing.

**Functie** |**Meer informatie** | **SQL Server-versie** 
---|---|---
**AlwaysOn beschikbaarheid van de groep** | Meerdere exemplaren van de zelfstandige versie van SQL Server worden uitgevoerd in een failovercluster met meerdere knooppunten.<br/><br/>Databases kunnen worden gegroepeerd in failover groepen die kunnen worden gekopieerd (gespiegeld) op SQL Server-exemplaren zodat er geen gedeelde opslagruimte nodig is.<br/><br/>Biedt herstel tussen een primaire-site en een of meer secundaire sites. Twee knooppunten kunnen worden ingesteld in een gedeelde niets cluster met SQL Server-databases die is geconfigureerd in een beschikbaarheidsgroep met synchroon herhaling en automatische failover. | SQL Server-2014 & 2012 Enterprise edition
**Failoverclusters (AlwaysOn FCI)** | SQL Server worden gebruikt in Windows failoverclusters voor betere beschikbaarheid van SQL Server-werkbelastingen on-premises implementatie.<br/><br/>Knooppunten met exemplaren van SQL Server met gedeelde schijven zijn geconfigureerd in een failovercluster. Als een exemplaar niet actief is, is het cluster wordt overgenomen naar andere.<br/><br/>Het cluster beveiligen niet tegen mislukt of bijvoorbeeld op een gedeelde locatie. De gedeelde schijf kan worden geïmplementeerd met iSCSI, glasvezelkanaal, of VHDXs gedeeld. | Edities van SQL Server Enterprise<br/><br/>SQL Server Standard edition (beperkt tot slechts twee knooppunten)
**Spiegelen (hoge beveiliging modus) van de database** | Beschermen één database naar een enkele secundaire kopie. Beschikbaar in beide hoge beveiliging (synchroon) en krachtige (asynchroon) herhaling modi. Een failovercluster, geen vereist. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise alle edities
**Zelfstandige versie van SQL Server** | De SQL Server en database worden gehost op één server (fysieke of virtuele). Host cluster wordt gebruikt voor beschikbaarheid als de server onderdeel virtual is. Geen beschikbaarheid Gast niveau. | Enterprise of Standard edition

## <a name="deployment-recommendations"></a>Aanbevelingen voor de implementatie


In deze tabel bevat een overzicht van onze aanbevelingen voor SQL Server BCDR technologieën integreren met sites worden hersteld.

**Versie** |**Edition** | **Implementatie** | **On-premises naar on-premises** | **On-premises naar Azure** 
---|---|---|---|---
SQL Server 2014 of 2012 | Enterprise | Failover clusterexemplaar | AlwaysOn beschikbaarheid van groepen | AlwaysOn beschikbaarheid van groepen
 | Enterprise | AlwaysOn beschikbaarheidsgroepen voor maximale beschikbaarheid | AlwaysOn beschikbaarheid van groepen | AlwaysOn beschikbaarheid van groepen
 | Standaard | Failover clusterexemplaar (FCI) | Site-herstel replicatie met lokale gespiegelde | Site-herstel replicatie met lokale gespiegelde
 | Enterprise of Standard | Zelfstandige | Site-herstel herhaling | Site-herstel herhaling
SQL Server 2008 R2 | Enterprise of Standard | Failover clusterexemplaar (FCI) | Site-herstel replicatie met lokale gespiegelde | Site-herstel replicatie met lokale gespiegelde
 | Enterprise of Standard | Zelfstandige | Site-herstel herhaling | Site-herstel herhaling
SQL Server (alle versies) | Enterprise of Standard | Failover clusterexemplaar - DTC-toepassing | Site-herstel herhaling | Niet ondersteund


## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

Hier ziet u wat u nodig hebt voordat u begint:

- Een SQL Server on-premises implementatie met een ondersteunde SQL Server-versie. Meestal moet u eveneens een Active Directory voor uw SQL-server.
- De vereisten voor het scenario dat u wilt implementeren. Vereisten voor vindt u in elk artikel implementatie. Koppelingen naar deze beschikbaar in de [Site-overzicht](site-recovery-overview.md).
- Als u instellen herstel van Azure wilt, moet u het hulpprogramma [Azure virtuele machines gereedheid Assessment](http://www.microsoft.com/download/details.aspx?id=40898) uitvoeren op uw SQL Server virtuele machines om ervoor te zorgen dat zij compatibel met Azure en sites worden hersteld.


## <a name="set-up-active-directory"></a>Active Directory instellen

U moet Active Directory op de site secundaire herstel voor SQL Server worden uitgevoerd. Er zijn een paar opties:

- **Kleine onderneming**, als er een klein aantal toepassingen en één domeincontroller voor de on-premises implementatie-site en u wilt over de hele site mislukt, wordt aangeraden waarmee u een Site herstel repication kunt repliceren van de domeincontroller naar het secundaire datacenter of naar Azure.

- **Middelgrote tot grote ondernemingen**, als er een groot aantal toepassing, u een Active Directory-bos gebruikt en gewenste toepassing of werkbelasting mislukt, wordt aangeraden het instellen van een extra domeincontroller in Azure wordt aangegeven of in het secundaire datacenter. Houd er rekening mee dat als u AlwaysOn beschikbaarheidsgroepen gebruikt om te herstellen naar een externe site raden dat u nog een extra domeincontroller op de secundaire site of Azure hebt ingesteld voor de herstelde SQL Server-instantie wilt gebruiken.

De instructies in dit document wordt ervan uitgegaan dat een domeincontroller beschikbaar in de secundaire locatie is. [Lees meer](site-recovery-active-directory.md) over het beveiligen van Active Directory met sites worden hersteld.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Beveiliging integreren met SQL Server Always-On (on-premises implementatie naar Azure)


Site-herstel ondersteunt SQL AlwaysOn. Als u een SQL-beschikbaarheid van de groep hebt gemaakt met een Azure virtuele machine instellen als 'Secundaire', kunt u de Site herstel gebruiken voor het beheren van de overname van de beschikbaarheid van de groepen. 

>[AZURE.NOTE] Deze functie is momenteel in de proefversie van en beschikbaar wanneer Hyper-V hostservers in de primaire datacenter worden beheerd in VMM wolken, waarbij VMware-instelling wordt beheerd door een [Configuratieserver](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Op dit moment deze functie niet beschikbaar in de nieuwe portal van Azure is.

#### <a name="prerequisites"></a>Vereisten voor

Hier volgt wat u moet SQL AlwaysOn integreren met sites worden hersteld:

- Een lokale SQL Server (zelfstandige server of een failovercluster).
- Een of meer Azure virtuele machines met SQL Server is geïnstalleerd
- Een SQL-beschikbaarheid van de groep instellen tussen een on-premises SQL Server en SQL Server wordt uitgevoerd in Azure wordt aangegeven
- De externe PowerShell moet worden ingeschakeld op de computer van de SQL Server on-premises implementatie. De server VMM of de configuratieserver moet kunnen externe PowerShell gesprekken te voeren met de SQL Server.
- Een gebruikersaccount moet worden toegevoegd op de lokale SQL Server, in deze SQL-gebruikersgroepen met ten minste de volgende machtigingen:
    - WIJZIGEN beschikbaarheid van de groep: machtigingen [hier](https://msdn.microsoft.com/library/hh231018.aspx)en [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER DATABASE - machtigingen[hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- Een RunAs-account moet worden gemaakt op VMM Server of een account moet worden gemaakt op de configuratieserver met behulp van de CSPSConfigtool.exe voor de gebruiker die worden genoemd in de vorige stap 
- De SQL-PS-module moet worden geïnstalleerd op SQL-Servers on-premises implementatie en klik op Azure virtuele machines
- De VM-Agent moet geïnstalleerde virtuele machines waarop Azure
- NTAUTHORITY\System nodig hebt met het volgen van machtigingen op SQL Server wordt uitgevoerd op virtuele machines in Azure wordt aangegeven:
    - ALTER beschikbaarheid van de groep - machtigingen [hier](https://msdn.microsoft.com/library/hh231018.aspx)en [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER DATABASE - machtigingen [hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Stap 1: Een SQL Server toevoegen


1. Klik op **SQL toevoegen** als een nieuwe SQL Server wilt toevoegen. 

    ![SQL toevoegen](./media/site-recovery-sql/add-sql.png)

2. In **De SQL-instellingen configureren** > **naam** een beschrijvende naam te verwijzen naar de SQL-Server opgeven.
3. **In SQL Server (FQDN)** Geef de FQDN-naam van de bron SQL Server die u wilt toevoegen. De SQL-Server is geïnstalleerd op een failovercluster, geef de FQDN van het cluster en niet van een van de knooppunten.  
4. In **SQL Server-instantie** kiest u de standaardsessie of de naam van het aangepaste exemplaar opgeven.
5. Selecteer een server VMM in **Management Server** of configuratieserver geregistreerd in de Site herstel kluis. Herstel van de site gebruikmaakt van deze server Management om te communiceren met de SQL Server.
6. Geef de naam van een RunAs-account dat is gemaakt op de opgegeven VMM-server of het Account dat is gemaakt op de Configuraaaon Server in **uitvoeren als Account** . Dit account wordt gebruikt voor toegang tot de SQL-Server en moet de machtiging lezen en overname bij storing voor beschikbaarheidsgroepen op de computer met SQL Server.

    ![SQL-dialoogvenster toevoegen](./media/site-recovery-sql/add-sql-dialog.png)

Nadat u de SQL-Server hebt toegevoegd die deze wordt weergegeven in het tabblad **SQL-Servers** . 

![SQL Server-lijst](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Stap 2: Een SQL-beschikbaarheid van de groep toevoegen

1. Machine wordt toegevoegd dat de volgende stap is de beschikbaarheid van de groepen toevoegen aan sites worden hersteld nadat de SQL-Server. Daarvoor inzoomen binnen de SQL Server in de vorige stap hebt toegevoegd en klik op SQL beschikbaarheid van de groep toevoegen. 

    ![SQL-AG toevoegen](./media/site-recovery-sql/add-sqlag.png)

2. SQL beschikbaarheid van de groep kunnen worden gerepliceerd naar een of meer virtuele machines in Azure wordt aangegeven. Wanneer de sql beschikbaarheid van de groep die u moet de naam en het abonnement van de Azure virtuele machine waar u de van de beschikbaarheidsgroep worden overgenomen door herstel van de Site wilt bieden toevoegen.

    ![SQL-AG dialoogvenster toevoegen](./media/site-recovery-sql/add-sqlag-dialog.png)

3. In het bovenstaande voorbeeld beschikbaarheid van de groep BW1-AG zou worden primaire op virtuele machine SQLAGVM2 uitgevoerd binnen abonnement DevTesting2 op een failover. 

>[AZURE.NOTE] Alleen de beschikbaarheid van de groepen die primair op de SQL Server toegevoegd in stap hierboven zijn zijn beschikbaar moet worden toegevoegd aan sites worden hersteld. Als u een beschikbaarheid van de groep primaire hebt aangebracht op de SQL Server of als u meer beschikbaarheidsgroepen op de SQL-Server hebt toegevoegd nadat deze is toegevoegd, vernieuwt u deze met de beschikbaar van de optie voor vernieuwen op de SQL Server.

#### <a name="step-3-create-a-recovery-plan"></a>Stap 3: Maak een herstelplan

De volgende stap is het opzetten van een plan voor herstel met zowel virtuele machines en de van de beschikbaarheidsgroepen. Selecteer dezelfde VMM Server of configuratieserver voor die u hebt gebruikt in stap 1 als de bron- en Microsoft Azure als doel.

![Herstel planning maken](./media/site-recovery-sql/create-rp1.png)

![Herstel planning maken](./media/site-recovery-sql/create-rp2.png)

In het voorbeeld wordt de Sharepoint-toepassing bestaat uit 3 virtuele machines die een SQL-beschikbaarheid van de groep als de backend gebruiken. In dit herstelplan dat we kan zowel de van de beschikbaarheidsgroep selecteren als u ook de virtuele machine die vormen de toepassing. 

U kunt het abonnement herstel verder aanpassen door te virtuele machines verplaatsen naar verschillende failover groepen om over te volgorde u de volgorde van failover. Van de beschikbaarheidsgroep is altijd is mislukt via eerste aangezien deze zou worden gebruikt als een back-end van een toepassing. 

![Plan voor herstel aanpassen](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Stap 4: Mislukken

Verschillende failover opties zijn beschikbaar als een groep beschikbaarheid is toegevoegd aan een herstel van plan bent.

Failover | Meer informatie
--- | ---
**Geplande failover** | Geplande Failover houdt een geen gegevensverlies failover. Als u wilt bereiken van die SQL beschikbaarheid van de groep beschikbaarheid modus is eerst worden ingesteld op synchroon en klikt u vervolgens een failover wordt geactiveerd zodat de van de beschikbaarheidsgroep primair kunnen de virtuele machine opgegeven bij het toevoegen van de beschikbaarheidsgroep aan sites worden hersteld. Als u de overname hebt voltooid, worden beschikbaarheid modus is ingesteld op dezelfde waarde zoals deze was voordat de geplande overname is geactiveerd.
**Niet-geplande failover** | Niet-geplande Failover kan resulteren in verlies van gegevens. Terwijl de niet-geplande failover activeert de modus beschikbaarheid van de beschikbaarheid van de groep niet is gewijzigd en de it bestaat uit de primaire kunnen de virtuele machine opgegeven bij het toevoegen van de beschikbaarheidsgroep aan sites worden hersteld. Als niet-geplande failover voltooid is en de on-premises implementatie-server met SQL Server weer beschikbaar is, heeft replicatie omkeren worden geactiveerd op de beschikbaarheid van de groep. Houd er rekening mee dat deze actie niet beschikbaar op het abonnement herstel is en kan worden gemaakt met SQL beschikbaarheid van de groep onder tabblad SQL-Servers
**Test failover** | Test failover voor de beschikbaarheid van de SQL-groep wordt niet ondersteund. Als u Test-overname van een herstel plannen met SQL beschikbaarheid van de groep activeren, zou failover voor beschikbaarheid van de groep worden overgeslagen.


Houd rekening met deze opties failover.

Optie | Meer informatie
--- | ---
**Optie 1** | 1. een overname testen van de toepassing en het front lagen uitvoeren.<br/><br/>2. werk de toepassingslaag voor toegang tot de kopie replica in de modus alleen-lezen en uitvoeren van een alleen-lezen-toets van de toepassing.
**Optie 2** | 1. een kopie van het replica VM exemplaar van het SQL Server (met VMM klonen voor site-naar-site of back-up van Azure) maken en dit openen in een testnetwerk<br/><br/> 2. de test overname met het abonnement herstel uitvoeren.

Stap 5: Mislukt terug

Als u wilt de beschikbaarheid van de groep opnieuw primair maken op de lokale SQL Server kunt vervolgens u doen door activeert-gepland om de foutherstel op het Plan voor herstel klikken en de richting van Microsoft Azure te on-premises VMM-Server.

>[AZURE.NOTE] Nadat een niet-geplande failover omgekeerde herhaling heeft worden geactiveerd op de beschikbaarheid van de groep de replicatie hervatten. Kassa hiervoor blijft de replicatie geschorst.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Machines zonder een Server VMM of een Server Configuration beveiligen

Voor de omgevingen die niet door een Server VMM of een Server Configuration worden beheerd, kan Azure automatisering Runbooks worden gebruikt voor het configureren van een script overname van SQL beschikbaarheid van de groepen. Hieronder vindt u de stappen voor het configureren van die:

1.  Maak een lokaal bestand voor het script mislukt via een van de beschikbaarheidsgroep. Voorbeeldscript Hiermee geeft u een pad naar de beschikbaarheidsgroep op de Azure replica en overgenomen replica exemplaren. Dit script wordt uitgevoerd op de SQL Server replica VM door door te geven met de extensie aangepast script is.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Het script uploaden naar een blob in een Azure opslag-account. In dit voorbeeld gebruiken:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Maak een runbook Azure automatisering om aan te roepen de scripts op de SQL Server replica virtuele machine in Azure wordt aangegeven. Gebruik deze voorbeeldscript u dit wilt doen. [Meer informatie](site-recovery-runbook-automation.md) over het gebruik van automatisering runbooks in herstel abonnementen. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Wanneer u maakt een herstelplan voor de toepassing Voeg een "vooraf groeperen 1 opstarten" script stap toe die het runbook automatisering mislukt op beschikbaarheidsgroepen activeert.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Beveiliging integreren met SQL-AlwaysOn (on-premises implementatie naar on-premises implementatie)

Als de SQL Server beschikbaarheidsgroepen voor beschikbaarheid, of een exemplaar van de cluster failover gebruikt is, wordt u aangeraden beschikbaarheidsgroepen gebruiken op de herstel-site. Houd er rekening mee dat deze richtlijnen is bedoeld voor toepassingen die gedistribueerde transacties niet worden gebruikt.

1. [Databases configureren](https://msdn.microsoft.com/library/hh213078.aspx) in van de beschikbaarheidsgroepen.
2. Maak een nieuw virtuele netwerk op secundaire site.
3. Een site-naar-site VPN tussen het nieuwe virtuele netwerk en het primaire-site instellen.
4. Een virtuele machine maken op de site herstel en installeer SQL Server op is geïnstalleerd.
5. De bestaande AlwaysOn beschikbaarheidsgroepen naar de nieuwe SQL Server virtuele machine uitbreiden. Dit exemplaar van SQL Server als een kopie asynchroon replica configureren.
6. Een luisteraar ervan af groep beschikbaarheid maken of bijwerken van de bestaande luisteraar ervan af als u wilt opnemen van de asynchrone replica virtuele machine.
7. Zorg ervoor dat de toepassing-farm instellen met de luisteraar ervan af. Als deze ingesteld met behulp van de naam van de database-server is, werk, zodat de luisteraar ervan af zodat u niet nodig hebt om te configureren, deze na de overname kan gebruiken.

Voor toepassingen die gebruikmaken van gedistribueerde transacties we aanbeveling u [Herstel van de Site met SAN herhaling](site-recovery-vmm-san.md) of [VMWare/fysieke server naar website herhaling](site-recovery-vmware-to-vmware.md)gebruiken.

### <a name="recovery-plan-considerations"></a>Herstel abonnement overwegingen

1. In dit voorbeeld van een script aan de bibliotheek VMM primaire en secundaire-sites toevoegen.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Wanneer u maakt een herstelplan voor de toepassing Voeg een "vooraf groeperen 1 opstarten" script stap toe die het script mislukt op beschikbaarheidsgroepen activeert.



## <a name="protect-a-standalone-sql-server"></a>Beveiligen met een zelfstandig product SQL Server

In deze configuratie wordt u aangeraden dat u Site herstel herhaling gebruiken om te beveiligen van de SQL Server-computer. De exacte procedure is afhankelijk van of SQL Server is ingesteld als een virtuele machine of fysieke server is en of u wilt repliceren naar Azure of een secundair on-premises implementatie site. Instructies voor alle implementatie-scenario's in het [Overzicht Site](site-recovery-overview.md)krijgen.


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Beveiligen met een SQL Server-cluster (standaard- of de 2008 R2)

Voor een cluster met SQL Server Standard edition of SQL Server 2008 R2 wordt u aangeraden dat u Site herstel herhaling gebruiken om te beveiligen met een SQL Server.

### <a name="on-premises-to-on-premises"></a>On-premises implementatie naar on-premises implementatie

- Als van de toepassing gebruikmaakt van gedistribueerde transacties kunt dat u [Herstel van de Site met SAN herhaling](site-recovery-vmm-san.md) implementeren voor een Hyper-V-omgeving en [VMware/fysieke server VMware](site-recovery-vmware-to-vmware.md) voor VMware-omgeving.

- Voor niet-DTC-toepassingen, gebruikmaken van de bovenstaande methode het cluster herstellen als een zelfstandige server gebruikmaken van een lokale hoog DB gespiegelde.

### <a name="on-premises-to-azure"></a>On-premises naar Azure

Herstel van de site kunnen Gast clusterondersteuning biedt geen ondersteuning wanneer repliceren naar Azure. SQL Server ook kunt niet zelf een lage kosten noodgevallen hersteloplossing voor Standard edition. U wordt aangeraden u beveiligen met een zelfstandig product SQL Server cluster de SQL Server on-premises implementatie en deze herstellen in Azure wordt aangegeven.


1. Een extra zelfstandige versie van SQL Server-instantie configureren op de on-premises implementatie-site.
2. Configureer dit exemplaar moet fungeren als een gespiegelde voor de databases die u wilt beveiligen. Configureer de spiegelen in de modus Hoog.
3.  Herstel van de Site configureren op de on-premises implementatie-site op basis van de omgeving ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware/fysieke server](site-recovery-vmware-to-azure-classic.md).
4.  Site-herstel replicatie gebruiken voor de nieuwe SQL Server-instantie repliceren naar Azure. Dit is een hoge beveiliging gespiegelde kopie en zodat deze moet worden gesynchroniseerd met de primaire cluster, maar deze moet worden gerepliceerd naar Azure met Site-herstel herhaling.

De volgende afbeelding ziet u deze instelling.

![Standaard cluster](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Aandachtspunten voor de failback

Failback na een niet-geplande failover wordt een back-up van SQL vereisen en van het exemplaar gespiegelde terugzetten naar de oorspronkelijke cluster en herstellen van het gespiegelde voor SQL standaard kolomgroepen.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-best-practices.md) over het punt staat om te implementeren sites worden hersteld.










 