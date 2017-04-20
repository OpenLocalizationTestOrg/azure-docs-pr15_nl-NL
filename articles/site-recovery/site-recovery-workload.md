<properties
    pageTitle="Welke werkbelasting kunt u met Azure Site herstel beveiligen?"
    description="Azure Site herstel beschermen uw werkbelasting en toepassingen door het coördineren van de replicatie, failover en herstel van on-premises implementatie virtuele machines en fysieke servers naar Azure of naar een secundaire on-premises-site"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Welke werkbelasting kunt u met Azure Site herstel beveiligen?


In dit artikel worden de werkbelasting en u met de Site herstel van Azure-service repliceren kunt-toepassingen.

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="overview"></a>Overzicht

Organisaties moeten u een zakelijke bedrijfscontinuïteit en noodgevallen herstel (BCDR) strategie werkbelasting en gegevens veilig en houden beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk.

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR. Met Site-herstel, kunt u zich realiseren toepassing herhaling implementeren in de cloud of naar een secundaire site. Of uw apps Windows zijn of Linux-gebaseerde, uitgevoerd op fysieke servers, VMware of Hyper-V, kunt u Site herstel herhaling goedkeuringen, uitvoeren noodgevallen herstel testen, en uitvoeren failovers en foutherstel.


Herstel van de site wordt geïntegreerd met het Microsoft-toepassingen, zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory. Microsoft werkt ook nauw met toonaangevende leveranciers, inclusief Oracle, SAP, IBM en rood rol. U kunt replicatie oplossingen op basis van de app-door-app kunt aanpassen.

## <a name="why-use-site-recovery-for-application-replication"></a>Waarom Site herstel voor toepassing herhaling gebruiken?

Site-herstel draagt als volgt bij aan de toepassing beveiliging en herstelbestanden:

- App-agnostic, replicatie voor alle werkbelasting op een ondersteunde computer leveren.
- In de buurt van de synchroon replicatie, met productoutput zo laag 30 seconden om te voldoen aan de behoeften van de belangrijkste zakelijke apps.
- App-consistente momentopnamen, voor één of meerdere lagen-toepassingen.
- Integratie met SQL Server AlwaysOn en verbinding met andere niveau van de webtoepassing herhaling technologieën AD replicatie, inclusief SQL AlwaysOn, Exchange Database beschikbaarheidsgroepen (DAGs) en beveiliging voor Oracle-gegevens.
- Flexibele herstel-abonnementen, die u voor het herstellen van een stapel hele toepassing met één klik inschakelen en opnemen als u wilt opnemen externe scripts en handmatige acties in het abonnement kan worden gebruikt.
- Geavanceerde netwerk beheren in het netwerk vereisten voor de app, waaronder de mogelijkheid te reserveren IP-adressen, vereenvoudigen-herstel van de Site en Azure configureren taakverdeling en integratie met Azure verkeer Manager voor lage RTO netwerk switchovers.
-  Een uitgebreide automatisering-bibliotheek waarin bevat productie gereed, toepassingsspecifieke scripts die kunnen worden gedownload en geïntegreerd met herstel abonnementen.



## <a name="workload-summary"></a>Werkbelasting samenvatting

Herstel van de site kan alle Apps op een ondersteunde computer repliceren. Daarnaast waarmee productteams te verrichten extra app / regiospecifieke worden getest.

**Werkbelasting** | **Hyper-V VMs repliceren naar een secundaire site** | **Hyper-V VMs repliceren naar Azure** | **VMware VMs repliceren naar een secundaire site** | **VMware VMs repliceren naar Azure**
---|---|---|---|---
Active Directory, DNS | Y | Y | Y | Y
Web-apps (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
SharePoint | Y | Y | Y | Y
SAP:<br/><br/>SAP-site repliceren naar Azure voor niet-cluster | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft)
Exchange (niet-DAG) | Y | Binnenkort beschikbaar | Y | Y
Extern bureaublad/VDI | Y | Y | Y | N/B
Linux (besturingssysteem en apps) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Binnenkort beschikbaar | Y | Binnenkort beschikbaar
Oracle | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft) | Y (getest door Microsoft)
Windows-bestandsserver | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Repliceren Active Directory en DNS

Een Active Directory en de DNS-infrastructuur zijn essentieel voor de meeste bedrijfs-apps. Tijdens herstel moet u beveiligen en herstellen van deze onderdelen van de infrastructuur, voordat u de werkbelasting en apps herstellen.

Herstel van de Site kunt u een volledige geautomatiseerde gaan maken voor Active Directory en DNS. Als u wilt mislukt bijvoorbeeld via SharePoint en SAP van een primaire naar een secundaire site, kunt u instellen een herstel-abonnement dat niet via Active Directory eerst en klik vervolgens op een abonnement extra app / regiospecifieke herstel mislukt via de andere apps die van Active Directory gebruikmaken.

[Meer informatie](site-recovery-active-directory.md) over het beveiligen van Active Directory en DNS.

## <a name="protect-sql-server"></a>SQL Server beveiligen

SQL Server biedt een gegevens services foundation voor gegevensservices voor veel bedrijven-apps in een on-premises implementatie-Datacenter.  Herstel van de site kan worden gebruikt samen met SQL Server HA/DR technologieën om te beveiligen meerlagige bedrijfs-apps die gebruikmaken van SQL Server. Herstel van de site bevat:

- Een eenvoudige en efficiënt noodgevallen hersteloplossing voor SQL Server. Repliceren meerdere versies en edities van SQL Server zelfstandige servers en kolomgroepen naar Azure of naar een secundaire site.  
- Integratie met SQL AlwaysOn beschikbaarheidsgroepen, voor het beheren van failover en foutherstel met Azure Site herstel herstelplannen.
- End-to-end herstel abonnementen voor de alle niveaus in een toepassing, zoals de SQL Server-databases.
- Schaalbaarheid van SQL Server voor piek wordt geladen met het herstellen van een Site door 'ontbranden"ze IaaS VM grotere in Azure.
- Eenvoudig testen van SQL Server-herstel. U kunt test failovers te analyseren gegevens en naleving controles, uitgevoerd zonder het die invloed hebben op uw productieomgeving uitvoeren.

[Meer informatie](site-recovery-sql.md) over het beveiligen van SQL server.

##<a name="protect-sharepoint"></a>Beveiligen van SharePoint

Azure Site herstel kunt beveiligen met een SharePoint-implementaties, als volgt:

- Neemt de nodig en de bijbehorende infrastructuurkosten voor een farm stand-by voor herstel. Een hele bedrijf (Web, de app en de database lagen) repliceren naar Azure of naar een secundaire site via sites worden hersteld.
- Vergemakkelijkt toepassingsimplementatie en beheer. Updates die zijn geïmplementeerd in de primaire site automatisch worden gerepliceerd, en dus beschikbaar nadat failover en herstel van een farm in een secundaire site. Verlaagt ook de complexiteit van management en de kosten die is gekoppeld aan een farm stand-by up-to-date te houden.
- Vereenvoudigd ontwikkeling van de SharePoint-toepassingen en testen door te maken van een kopie van de productie-achtige op aanvraag replica-omgeving testen en problemen oplossen.
- Vereenvoudigd overgang naar de cloud met behulp van de Site herstellen om te migreren van SharePoint-implementaties naar Azure.

[Meer informatie](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) over het beveiligen van SharePoint.


## <a name="protect-dynamics-ax"></a>Beveiligen met een Dynamics AX

Azure Site herstel helpt bij de bescherming van uw Dynamics AX ERP-oplossing door:

- Replicatie van uw hele Dynamics AX-omgeving (Web en AOS lagen, database lagen, SharePoint) tot Azure, of tot een secundaire site Orchestrating.
- Migratie van Dynamics AX-implementaties in de cloud (Azure) vereenvoudigen.
- De ontwikkeling van de Dynamics AX-toepassingen te vereenvoudigen en testen door te maken van een kopie van de productie-achtige op aanvraag, testen en problemen oplossen.

[Meer informatie](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) over het beveiligen van Dynamics AX.

## <a name="protect-rds"></a>RDS beveiligen

Remote Desktop Services (RDS) kunt virtual desktop infrastructure (VDI), op basis van een sessie voor desktops en toepassingen, zodat gebruikers kunnen de overal werken. Met Azure sites worden hersteld, kunt u het volgende doen:

- Beheerde of onbeheerde gegroepeerde virtuele bureaubladen naar een secundaire site en externe toepassingen en sessies voor een secundaire site of Azure gerepliceerd.
- Hier ziet u wat u kunt repliceren:

**RDS** | **Hyper-V VMs repliceren naar een secundaire site** | **Hyper-V VMs repliceren naar Azure** | **VMware VMs repliceren naar een secundaire site** | **VMware VMs repliceren naar Azure** | **Fysieke servers repliceren naar een secundaire site** | **Fysieke servers repliceren naar Azure**
---|---|---|---|---|---|---
**Gegroepeerde virtueel bureaublad (onbeheerd)** | Ja | Nee | Ja | Nee | Ja | Nee
**Gegroepeerde virtueel bureaublad (beheerde en zonder UPD)** | Ja | Nee | Ja | Nee | Ja | Nee
**Externe toepassingen en bureaublad-sessies (zonder UPD)** | Ja | Ja | Ja | Ja | Ja | Ja


[Meer informatie](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) over het beveiligen van RDS.


## <a name="protect-exchange"></a>Beveiligen van Exchange

Site-herstel beveiligt Exchange, als volgt:

- Herstel van de Site kunt repliceren en mislukken naar Azure of naar een secundaire site voor kleine implementaties van Exchange, zoals een enkel of zelfstandige-servers.
- Voor grotere implementaties geïntegreerd herstel van de Site met Exchange DAGS.
- Exchange-DAGs zijn de aanbevolen oplossing voor Exchange-herstel in een onderneming.  Site herstel herstel-abonnementen kunnen DAGs, om de goedkeuringen DAG failover door verschillende sites bevatten.


[Meer informatie](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) over het beveiligen van Exchange.

## <a name="protect-sap"></a>Beveiligen van SAP

Herstel van de Site gebruiken om het beveiligen van uw SAP-implementatie, als volgt:

- Beveiliging van de hele SAP-implementatie, door de implementatie van de verschillende lagen repliceren Azure of een secundaire site inschakelen.
- Vereenvoudig cloud-migratie, met behulp van de Site herstel uw SAP-implementatie migreren naar Azure.
- SAP overzichtelijker en kopieer testen, door te maken van een productie-achtige op aanvraag testen en problemen oplossen toepassingen.

[Meer informatie](http://aka.ms/asr-sap) over het beveiligen van SAP.

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden op een Site herstel-implementatie](site-recovery-best-practices.md) 
