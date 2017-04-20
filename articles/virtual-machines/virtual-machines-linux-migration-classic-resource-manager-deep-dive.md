<properties
    pageTitle="Technische uitgebreide kennismaking op platform ondersteund migratie van klassiek naar Azure resourcemanager | Microsoft Azure"
    description="In dit artikel bevat een technische uitgebreide Kennismaking voor platform-ondersteunde migratie van resources uit klassieke naar Azure resourcemanager"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="kasing"/>

# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>Technische uitgebreide kennismaking op platform ondersteund migratie van klassiek naar Azure resourcemanager

In dit artikel doen we een technische uitgebreide kennismaking op migratie op een niveau van de resource en functie uitgelegd hoe het platform resources uit het implementatiemodel klassieke aan het implementatiemodel van Azure resourcemanager migreert u. Lees voor meer informatie de service aankondiging-artikel: [Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager](virtual-machines-windows-migration-classic-resource-manager.md).

## <a name="detailed-guidance-on-migration-at-a-resource-and-feature-level"></a>Gedetailleerde richtlijnen voor migratie op een niveau van de resource en functie

U vindt het klassieke en resourcemanager weergaven van de resources in de volgende tabel. Functies en resources die niet wordt vermeld in deze tabel worden niet ondersteund op dit moment.

| Klassieke weergave| Resourcemanager weergave | Gedetailleerde notities |
|--------------------------------------------------------|-------------------------------------------------|----------------------------------------------------------|
| De naam van de cloud-service                                     | DNS-naam                                        | Tijdens de migratie we een nieuwe resourcegroep voor elke cloudservice maken met het patroon dat naming `<cloudservicename>-migrated`. Deze resourcegroep bevat alle bronnen. De naam van de cloud-service, wordt een DNS-naam die is gekoppeld aan het openbare IP-adres.                                                                                                                                                                                                    |   
| VM                                        | VM                                 | VM-specifieke eigenschappen worden gemigreerd ongewijzigd. Houd er rekening mee dat bepaalde gegevens osProfile, zoals de naam van de computer, niet in het implementatiemodel klassieke opgeslagen wordt en leeg na de migratie blijft.                                                                                                                                                                                                                                                                |   
| Schijf resources die zijn bijgevoegd bij VM                          | Impliciete schijven die zijn bijgevoegd bij VM                   | Schijven worden niet als op het hoogste niveau resources in het implementatiemodel resourcemanager gebaseerd. Ze worden gemigreerd als impliciete schijven onder de VM. Op dit moment kan ondersteunen we alleen schijven die zijn gekoppeld aan een VM. Resourcemanager VMs om in te schakelen migratie, kunt nu klassieke opslag accounts gebruiken. Hierdoor wordt de schijven eenvoudig aan het model resourcemanager zonder de updates worden gemigreerd. |   
| VM extensies                                          | VM extensies                                   | Resource extensies, met uitzondering van XML-extensies, worden gemigreerd van het implementatiemodel klassieke.                                                                                                                                                                                                                                                                                                                                                                        |   
| VM certificaten                           | Certificaten in Azure belangrijke kluis                 | Als een cloudservice service certificaten bevat, wordt het platform Hiermee maakt u een nieuwe Azure belangrijke kluis per cloudservice en de certificaten overgezet naar de belangrijkste kluis. De VMs wordt bijgewerkt als u wilt verwijzen naar de certificaten van de belangrijkste kluis.                                                                                                                                                                                                                               |   
| WinRM configuratie                                    | WinRM configuratie onder osProfile             | Windows Extern beheer configuratie wordt verplaatst ongewijzigd als onderdeel van de migratie.                                                                                                                                                                                                                                                                                                                                                                                            |   
| Eigenschap beschikbaarheid instellen                              | Beschikbaarheid van de set resource                       | Specificatie van de beschikbaarheid van de set is een eigenschap op de VM in het implementatiemodel klassieke. Beschikbaarheid sets worden een op het hoogste niveau resource als onderdeel van de migratie. Opmerking dat wordt de volgende configuratie niet ondersteund: meerdere beschikbaarheid sets per cloudservice binnenkomt, of stelt u een of meer beschikbaarheid samen met VMs die niet in andere beschikbaarheid instellen in een cloudservice zijn.                                                                                |   
| Netwerkconfiguratie op een VM                          | Primaire netwerkinterface                       | Netwerkconfiguratie op een VM wordt weergegeven als de primaire netwerk interface resource na de migratie. Voor VMs die niet in een virtueel netwerk, wordt het interne IP-adres wijzigt tijdens de migratie.                                                                                                                                                                                                                                                            |  
| Meerdere netwerkinterfaces op een VM                    | Netwerkinterfaces                              | Als een VM had meerdere netwerkinterfaces gekoppeld, wordt elke netwerkinterface een op het hoogste niveau resource als onderdeel van de migratie in het implementatiemodel resourcemanager, samen met alle eigenschappen.                                                                                                                                                                                                                                                            |   
| Taakverdeling eindpunt instellen                             | De belasting voor documentconversies                                   | In het implementatiemodel klassieke toegewezen het platform een impliciete taakverdeling voor elke cloudservice. Tijdens de migratie, maak een nieuwe resource van taakverdeling en wordt de set-taakverdeling eindpunt taakverdeling regels.                                                                                                                                                                                                                                     |   
| Binnenkomende NAT-regels                                      | Binnenkomende NAT-regels                               | Invoer eindpunten gedefinieerd op de VM zijn omzetten in inkomende netwerk Access vertaling regels onder de taakverdeling tijdens de migratie.                                                                                                                                                                                                                                                                                                                                           |   
| VIP-adres                              | Openbare IP-adres met DNS-naam                 | Het virtuele IP-adres verandert in een openbare IP-adres en is gekoppeld aan de taakverdeling.                                                                                                                                                                                                                                                                                                                                                        |   
| Virtuele netwerk                                        | Virtuele netwerk                                 | Het virtuele netwerk wordt gemigreerd, met alle eigenschappen, aan het implementatiemodel resourcemanager. Een nieuwe resourcegroep is gemaakt met de naam `-migrated`. Opmerking de [niet-ondersteunde configuraties](virtual-machines-windows-migration-classic-resource-manager.md).                                                                                                                                                                                                     |
| Gereserveerde IP-adressen                                           | Openbare IP-adres met statische toewijzingsmethode  | Gereserveerde IP-adressen die is gekoppeld aan de taakverdeling worden gemigreerd, samen met de migratie van de cloudservice of de virtuele machine. Niet-gekoppelde gereserveerde IP-migratie wordt niet ondersteund op dit moment.                                                                                                                                                                                                                                                           |   
| Openbare IP-adres per VM                               | Openbare IP-adres met de methode voor dynamische toewijzing | Het openbare IP-adres dat is gekoppeld aan de VM is geconverteerd als een openbare IP-adres resource, met de toewijzingsmethode is ingesteld op statische.                                                                                                                                                                                                                                                                                                                                   |   
| NSGs                                | NSGs                         | Netwerk beveiligingsgroepen die is gekoppeld aan een subnet worden gekopieerd als onderdeel van de migratie aan het implementatiemodel resourcemanager. Houd er rekening mee dat het NSG in het implementatiemodel klassieke niet tijdens de migratie is verwijderd. De bewerkingen management-vlak voor de NSG worden echter worden geblokkeerd wanneer de migratie is uitgevoerd.                                                                                                                                                                             |   
| DNS-servers                                            | DNS-servers                                     | DNS-servers die is gekoppeld aan een virtueel netwerk of de VM worden gemigreerd als onderdeel van de bijbehorende resource-migratie, samen met alle eigenschappen.                                                                                                                                                                                                                                                                                                                    |   
| UDRs                                    | UDRs                             | De gebruiker gedefinieerde routes die is gekoppeld aan een subnet worden gekopieerd als onderdeel van de migratie aan het implementatiemodel resourcemanager. Houd er rekening mee dat het UDR in het implementatiemodel klassieke niet tijdens de migratie is verwijderd. De bewerkingen management-vlak voor de UDR worden echter geblokkeerd wanneer de migratie uitgevoerd is.                                                                                                                                                                             |   
| IP-eigenschap voor netwerkconfiguratie van een VM doorschakelen | IP-eigenschap op de NIC doorschakelen               | De eigenschap van een VM doorschakelen IP wordt geconverteerd naar een eigenschap op de netwerkinterface tijdens de migratie.                                                                                                                                                                                                                                                                                                                                                           |   
| De belasting voor documentconversies met meerdere IP-adressen                        | De belasting voor documentconversies met meerdere openbare IP-resources | Elke openbare IP die is gekoppeld aan de taakverdeling is geconverteerd naar een openbare IP-resource en dat is gekoppeld aan de taakverdeling na de migratie.                                                                                                                                                                                                                                                                                                       |   
| Interne DNS-namen op de VM                           | Interne DNS-namen op de NIC                    | Tijdens de migratie zijn de interne DNS-achtervoegsels voor de VM gemigreerd naar een alleen-lezen eigenschap met de naam 'InternalDomainNameSuffix' op de NIC Het achtervoegsel blijft ongewijzigd na de migratie en VM resolutie moet blijven bewerken als voorheen.                                                                                                                                                                                                           |   

## <a name="illustration-of-a-simple-migration-walkthrough"></a>Illustratie van eenvoudige migratie Stapsgewijze instructies

In het volgende voorbeeld schermafbeeldingen ziet u de weergave van een cloudservice met een VM (niet in een virtueel netwerk) na de voorbereidende fase.

![Schermafbeelding die de klassieke weergave is als u na voorbereiden](./media/virtual-machines-windows-migration-classic-resource-manager/classic-migration-prepare-portal.png)
![schermafbeelding die de weergave resourcemanager is na voorbereiden](./media/virtual-machines-windows-migration-classic-resource-manager/resourcemanager-migration-prepare-portal.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u de migratie van klassieke IaaS resources aan resourcemanager kennen, kunt u beginnen met het migreren van resources.

- [PowerShell gebruiken om te migreren naar Azure resourcemanager IaaS resources van klassiek](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [CLI gebruiken om te migreren naar Azure resourcemanager IaaS resources van klassiek](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Een klassieke virtuele machine naar Azure resourcemanager klonen met behulp van de community PowerShell-scripts](virtual-machines-windows-migration-scripts.md)