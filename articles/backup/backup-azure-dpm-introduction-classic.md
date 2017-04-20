<properties
    pageTitle="Inleiding tot Azure DPM back-up | Microsoft Azure"
    description="Een inleiding tot een back-up DPM servers met de back-up van Azure-service"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="Systeem Center Data Protection Manager, gegevens beveiliging manager, dpm back-up maken"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Back-up werkbelasting naar Azure met DPM voorbereiden

> [AZURE.SELECTOR]
- [Azure back-Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure back-Server (klassieke)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassieke)](backup-azure-dpm-introduction-classic.md)


In dit artikel bevat een inleiding over het gebruik van Microsoft Azure back-up maken om uw systeem Center Data Protection Manager (DPM)-servers en werkbelasting te beschermen. Deze leest, moet u kennen:

- De werking van Azure DPM server back-up maken
- De vereisten voor een soepele back-ervaring bereiken
- De normale fouten en hoe u omgaan met hen
- Ondersteunde scenario 's

System Center DPM een back-up van bestands-en-toepassing. Gegevens back-up gemaakt naar DPM kan worden opgeslagen op tape, op schijf of back-up gemaakt naar Azure met Microsoft Azure back-up. DPM interactie tussen en met Azure back-up als volgt:

- **DPM geïmplementeerd als een fysieke server of on-premises virtuele machine** , als DPM wordt geïmplementeerd als een fysieke server of als een on-premises implementatie Hyper-V-VM die u kunt back-up van gegevens naar een back-Azure kluis naast de schijf en tape back-up.
- **DPM geïmplementeerd als een Azure virtuele machines** , uit systeem Center 2012 R2 met Update 3, DPM kan worden geïmplementeerd als een Azure virtuele machines. Als DPM wordt geïmplementeerd als een Azure virtuele machines die u kunt back-up van gegevens naar Azure schijven toegevoegd aan de DPM Azure virtuele machine, of u de gegevensopslag staat een back-up tot een back-up van Azure-kluis.

## <a name="why-backup-your-dpm-servers"></a>Waarom een back-up van uw DPM-servers?

De zakelijke voordelen van het gebruik van Azure back-up maken voor een back-up DPM servers opnemen:

- Voor on-premises DPM implementatie, kunt u Azure back-up als alternatief voor lange implementatie op tape.
- Voor DPM implementaties in Azure kunt Azure back-up u laten opslag van de Azure schijf worden verwijderd, zodat u om uit te breiden door oudere gegevens in Azure back-up- en nieuwe gegevens op schijf opslaan.

## <a name="how-does-dpm-server-backup-work"></a>Hoe werkt de back-up van DPM server?
Als u wilt back-up van een virtuele machine, is eerst een momentopname van het punt-in-tijd van de gegevens nodig. De back-Azure-service de back-uptaak start op het geplande tijdstip en activeert de back-extensie een momentopname maken. De back-extensie coördinaten met de service in Gast VSS om te bereiken consistentie en de API blob momentopname van de Azure Storage-service activeert zodra consistentie is bereikt. Dit gebeurt als u een consistente momentopname van de schijf van de virtuele computer zonder af te sluiten.

Nadat de momentopname is genomen, wordt de gegevens door de Azure back-service overgebracht naar de back-kluis. De service zorgt voor identificeren en alleen de bouwstenen die zijn gewijzigd van de laatste back-up de opslag van de back-ups en het netwerk efficiënter overbrengen. Wanneer de gegevensoverdracht is voltooid, wordt de momentopname wordt verwijderd en wordt een herstelpunt wordt gemaakt. Dit herstelpunt kan worden bekeken in de portal van Azure klassieke.

>[AZURE.NOTE] Voor Linux virtuele machines is het alleen bestand consistente back-up mogelijk.

## <a name="prerequisites"></a>Vereisten voor
Back-up van Azure als volgt een back-up DPM gegevens voorbereiden:

1. **Een back-up-kluis maken** , een kluis maken in de back-up van Azure-console.
2. **Download kluis referenties** , In Azure back-up, uploadt u de management-certificaat dat u hebt gemaakt naar de kluis.
3. **De back-up Azure-Agent installeren en registreren van de server** , van Azure back-up en installeren van de agent op elke DPM-server en de DPM-server in de back-kluis registreren.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Vereisten (en beperkingen)

- DPM kan worden uitgevoerd als een fysieke server of een Hyper-V virtuele machine op systeem Center 2012 SP1 of systeem Center 2012 R2 is geïnstalleerd. Dit kan ook worden uitgevoerd als een Azure virtuele machines systeem Center 2012 R2 met ten minste waarop DPM 2012 R2 Update Rollup 3 of een virtuele Windows-computer in VMWare systeem Center 2012 R2 met ten minste waarop Update Rollup 5.
- Als u DPM met systeem Center 2012 SP1 uitvoert, moet u de Update Rollup 2 voor System Center Data Protection Manager SP1 installeren. Dit is vereist voordat u de back-up-Agent van Azure kunt installeren.
- De server DPM moet hebben Windows PowerShell en .net Framework 4.5 is geïnstalleerd.
- DPM kunt back-up van de meeste werkbelasting aan Azure back-up. Voor een volledige lijst met wat Zie heeft ondersteund ondersteuning voor de back-up van Azure onderstaande items.
- Gegevens die zijn opgeslagen in de back-up van Azure kan niet worden hersteld met de optie 'kopiëren naar tape'.
- U hebt een Azure-account nodig met de functie Azure back-up is ingeschakeld. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Lees meer over het [back-up van Azure prijzen](https://azure.microsoft.com/pricing/details/backup/).
- Back-up van Azure gebruikt, is de back-up-Agent van de Azure zijn geïnstalleerd op de servers die u een back wilt-up vereist. Elke server moet ten minste 10% van de grootte van de gegevens die back-up, beschikbaar zijn als gratis opslagruimte op lokale gemaakt is hebben. Een back-up 100 GB van gegevens is bijvoorbeeld minimaal van 10 GB van beschikbare ruimte in het kladgebied locatie. Terwijl de minimumwaarde is 10%, wordt 15% van gratis lokale opslagruimte moet worden gebruikt voor de locatie van de cache aanbevolen.
- Gegevens worden opgeslagen in de opslagruimte van Azure kluis. Er is geen limiet voor de hoeveelheid gegevens die u kunt back-up een back-up van Azure vault maar de grootte van een gegevensbron (bijvoorbeeld een virtuele machine of een database) 54,400 GB niet mag overschrijden.

Dit soort bestanden worden ondersteund voor back-up Azure:

- Versleutelde (volledige back-ups alleen)
- Gecomprimeerd (incrementele back-ups ondersteund)
- Sparse (incrementele back-ups ondersteund)
- Gecomprimeerd en verspreid (behandeld als Sparse)

En deze niet worden ondersteund:

- Servers op hoofdlettergevoelig bestandssystemen worden niet ondersteund.
- Vaste koppelingen (overgeslagen)
- Reparsepunten (overgeslagen)
- Versleutelde en gecomprimeerd (overgeslagen)
- Versleutelde en verspreid (overgeslagen)
- Gecomprimeerde stream
- Verspreid stream

>[AZURE.NOTE] Uit in System Center 2012 DPM met SP1, u kunt back-up maken van werkbelasting die zijn beveiligd met DPM naar Azure met Microsoft Azure Backup.
