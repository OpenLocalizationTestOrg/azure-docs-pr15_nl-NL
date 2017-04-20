<properties
    pageTitle="Azure Site herstel ondersteuningsmatrix | Microsoft Azure"
    description="Bevat een overzicht van de ondersteunde besturingssystemen en de onderdelen voor herstel van Azure-Site"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure Site herstel ondersteuningsmatrix

In dit artikel bevat een overzicht van ondersteunde besturingssystemen en de onderdelen voor Azure Site herstel implementaties. Een lijst met ondersteunde onderdelen en vereisten is beschikbaar voor elk implementatiescenario in elk het bijbehorende implementatie-artikel en dit document bevat een overzicht van deze.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Ondersteunde besturingssystemen voor servers virtualization


**Bron** | **Doel** | **Host OS**
---|---|---|--- 
**Hyper-V hosts (zonder VMM)** | Azure | Windows Server 2012 R2 met meest recente updates
**Hyper-V hosts (met VMM)** | Azure | Windows Server 2012 R2 met meest recente updates
**Hyper-V hosts (met VMM)** | Secundaire VMM-site | Ten minste Windows-Server 2012 met meest recente updates
**VMware hosts/vCenter** | Azure | vCenter 5.5 of 6.0 (ondersteuning voor alleen 5,5 functies) <br/><br/> vSphere 6.0, 5.5 of 5.1 met de meest recente updates
**VMware hosts/vCenter** | Secundaire VMware-site | vCenter 5.5 of 6.0 (ondersteuning voor alleen 5,5 functies) <br/><br/> vSphere 6.0, 5.5 of 5.1 met de meest recente updates

## <a name="supported-requirements-for-replicated-machines"></a>Ondersteunde vereisten voor gerepliceerde machines

**Bron** | **Wat wordt gerepliceerd** | **Doel** | **Host OS**
---|---|---|--- 
**Hyper-V VMs** | Een werkbelasting | Azure | Een gast OS [worden ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs moeten voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (met VMM)** | Een werkbelasting | Azure | Een gast OS [worden ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs moeten voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (met VMM)** | Een werkbelasting | Secundaire VMM-site | Een gast OS [worden ondersteund door Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMware VMs** | Een werkbelasting uitvoeren op Windows VM | Azure | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met bij minimaal SP1<br/><br/> VMs moeten voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Een werkbelasting waarop Linux VM | Azure | Rode rol Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Opslag vereist: File system (EXT3, ETX4, ReiserFS, XFS); Paden software-apparaat toewijzing (paden)); Volume manager: (LVM2). Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.<br/><br/> VMs moeten voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Een werkbelasting uitvoeren op Windows VM | Secundaire VMware-site | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met bij minimaal SP1
**VMware VMs** | Een werkbelasting waarop Linux VM | Secundaire VMware-site | Rode rol Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Opslag vereist: File system (EXT3, ETX4, ReiserFS, XFS); Paden software-apparaat toewijzing (paden)); Volume manager: (LVM2). Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.
**Fysieke servers** | Een werkbelasting met Windows | Azure | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 met bij minimaal SP1
**Fysieke servers** | Een werkbelasting waarop Linux | Azure | Rode rol Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Opslag vereist: File system (EXT3, ETX4, ReiserFS, XFS); Paden software-apparaat toewijzing (paden)); Volume manager: (LVM2). Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.
**Fysieke servers** | Een werkbelasting met Windows | Secundaire site | 64-bits Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 met bij minimaal SP1
**Fysieke servers** | Een werkbelasting waarop Linux | Secundaire site | Rode rol Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Opslag vereist: File system (EXT3, ETX4, ReiserFS, XFS); Paden software-apparaat toewijzing (paden)); Volume manager: (LVM2). Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.


## <a name="provider-versions"></a>Provider versies

**Naam** | **Beschrijving** | **Meest recente versie** | **Ondersteuning** | **Meer informatie**
---|---|---|---| ---
**Azure Site herstel-Provider** | Coördinaten communicatie tussen servers van de on-premises implementatie en Azure/secundaire-site <br/><br/> Geïnstalleerd op de on-premises implementatie VMM servers of Hyper-V servers als er geen VMM-server | 5.1.1700 (beschikbaar vanaf de portal) | [Nieuwste functies en correcties](https://support.microsoft.com/kb/3155002)
**Azure Site herstel Unified Setup (VMware Azure)** | Coördinaten communicatie tussen on-premises implementatie VMware servers en Azure <br/><br/> Geïnstalleerd op on-premises implementatie VMware servers | 9.3.4246.1 (beschikbaar vanaf de portal) | [Nieuwste functies en correcties](https://support.microsoft.com/kb/3155002)
**Mobiliteit-service** | Coördinaten herhaling tussen on-premises implementatie VMware servers/fysieke servers en Azure/secundaire-site | NB (beschikbaar vanaf de portal) | Op elke VMware VM of fysieke server die u wilt repliceren geïnstalleerd. **Agent van Microsoft Azure herstel Services (MARS)** | Coördinaten replicatie tussen Hyper-V VMs en Azure<br/><br/> Geïnstalleerd op on-premises implementatie Hyper-V servers (met of zonder een VMM-server) | 2.0.8689.0 (beschikbaar vanaf de portal) | Deze agent wordt gebruikt door Azure Site herstel en Azure back-up). [Meer informatie] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor implementatie](site-recovery-best-practices.md)

