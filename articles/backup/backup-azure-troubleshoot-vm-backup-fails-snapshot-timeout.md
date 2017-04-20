<properties
   pageTitle="Azure VM back-up mislukt: kan niet communiceren met de VM-agent voor momentopname status - momentopname VM subtaak time-out | Microsoft Azure"
   description="Symptomen oorzaken en oplossingen voor Azure VM back-fouten die betrekking hebben op kunnen niet communiceren met de VM-agent voor momentopname status. Momentopname VM subtaak time-out fout"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure VM back-up mislukt: kan niet communiceren met de VM-agent voor momentopname status - momentopname VM subtaak timed-out

## <a name="summary"></a>Overzicht

Na het registreren en een virtuele Machine (VM) voor Azure back-ups plannen, start de back-Azure-service de back-uptaak op het geplande tijdstip door het communiceren met de back-extensie in VM naar een punt-in-time momentopname. Er zijn omstandigheden die verhinderen de momentopname wordt dat kunnen geactiveerd die leidt naar een back-up is mislukt. Dit artikel bevat stappen voor probleemoplossing voor problemen met Azure VM back-fouten die betrekking hebben op momentopname time-fout.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptoom

Microsoft Azure back-up voor infrastructuur als een service (IaaS) VM mislukt en geeft als resultaat het volgende foutbericht weergegeven in de details van de taak-fout in de [portal van Azure](https://portal.azure.com/):

**Kan niet communiceren met de VM-agent voor momentopname status - momentopname VM subtaak timed-out.**

## <a name="cause"></a>Oorzaak
Er zijn vier meest voorkomende oorzaken van deze fout:

- De VM heeft geen toegang tot Internet.
- De Microsoft Azure VM-agent is geïnstalleerd in de VM is verouderd (voor Linux VMs).
- De back-extensie mislukt om te werken of te laden.
- De status momentopnamen kan niet worden opgehaald of de momentopnamen kunnen niet worden gemaakt.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Oorzaak 1: De VM heeft geen toegang tot Internet
Per de vereiste implementatie de VM heeft geen toegang tot Internet of heeft beperkingen op hun plaats staan die toegang tot de Azure infrastructuur voorkomen.

De back-extensie is connectiviteit vereist met de Azure openbare IP-adressen correct. Dit is omdat deze opdrachten naar een eindpunt Azure Storage (HTTP URL verzendt) voor het beheren van de momentopnamen van VM. Als de extensie geen toegang tot openbare Internet heeft, wordt de back-up uiteindelijk mislukt.

### <a name="solution"></a>Oplossing
Gebruik één van de volgende methoden om het probleem te verhelpen in dit scenario:

- IP-bereiken gebruikt "witte" lijst het Azure datacenter
- Maak een pad voor HTTP-verkeer

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Naar "witte" lijst de Azure datacenter IP-bereiken gebruikt

1. De [lijst met Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) om te worden whitelisted verkrijgen.
2. Blokkering van het IP-adressen met behulp van de cmdlet New-NetRoute opheffen. Deze cmdlet uitvoeren in de VM Azure in een verhoogde PowerShell-venster (als Administrator uitvoeren).
3. Regels toevoegen aan het netwerk beveiliging groep (NSG) als er een toe te staan dat toegang tot het IP-adressen.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Om een pad voor HTTP-verkeer te maken

1. Als u netwerkbeperkingen op hun plaats staan (bijvoorbeeld een NSG) hebt, moet u een HTTP-proxy-server om het verkeer te routeren implementeren.
2. Als u een beveiligingsgroep netwerk (NSG) hebt, kunt u regels voor het toestaan van toegang met Internet uit de HTTP-proxy toevoegen.

Informatie over het [instellen van een HTTP-proxy VM back-ups](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Oorzaak 2: De Microsoft Azure VM-agent is geïnstalleerd in de VM is verouderd (voor Linux VMs)

### <a name="solution"></a>Oplossing
Meest agent gerelateerde of extensie-gerelateerde fouten voor Linux VMs worden veroorzaakt door problemen die betrekking hebben op een oude VM-agent. Normaal gesproken zijn de eerste stappen voor het oplossen van dit probleem in de volgende:

1. [Installeer de meest recente Azure VM-agent](https://github.com/Azure/WALinuxAgent).
2. Zorg ervoor dat de Azure-agent wordt uitgevoerd op de VM. Hiervoor voert u de volgende opdracht uit:```ps -e```

    Als dit proces wordt niet uitgevoerd, kunt u de volgende opdrachten gebruiken om deze opnieuw te starten.

    Voor Ubuntu:```service walinuxagent start```

    Voor andere onderzoeken: '''service-waagent starten
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
