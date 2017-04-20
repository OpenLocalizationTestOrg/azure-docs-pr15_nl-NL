<properties
    pageTitle="Maak een account Azure Batch | Microsoft Azure"
    description="Informatie over het maken van een Batch Azure-account in de portal van Azure grootschalige parallelle werkbelasting uitvoeren in de cloud"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Maak een Batch Azure-account met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Azure-portal](batch-account-create-portal.md)
- [Batch Management .NET](batch-management-dotnet.md)

Informatie over het maken van een Batch Azure-account in de [portal van Azure][azure_portal], en waar vind ik belangrijke eigenschappen van account, zoals de toegangstoetsen en URL's-account. Wordt ook Batch prijzen en het koppelen van een Azure Storage-account bij uw account Batch, zodat u [toepassingspakketten](batch-application-packages.md) en [Project en uitvoer persistent gebruiken kunt](batch-task-output.md)besproken.

## <a name="create-a-batch-account"></a>Een Batch-account maken

1. Meld u aan bij de [portal van Azure][azure_portal].

2. Klik op **nieuwe** > **berekenen** > **Batch-Service**.

    ![De batch op Marketplace][marketplace_portal]

3. Het blad **Nieuw Batch-Account** wordt weergegeven. Zie items *een* tot en met *e* onder voor beschrijvingen van elk element blade.

    ![Een Batch-account maken][account_portal]

    een. **Accountnaam**: een unieke naam voor uw account Batch. Deze naam moet uniek zijn binnen de Azure regio die het account is gemaakt (Zie onderstaande *locatie* ). Er mogelijk alleen kleine letters, cijfers, bevatten en moet 3-24 tekens lang zijn.

    b. **Abonnement**: een abonnement waarin u de Batch-account kunt maken. Als u slechts één abonnement hebt, wordt het al dan niet standaard geselecteerd.

    c. **Resourcegroep**: een bestaande resources groeperen voor uw nieuwe Batch-account of (optioneel) een nieuwe record maken.

    d. **Locatie**: een Azure regio waarin u de Batch-account kunt maken. Alleen de regio's die worden ondersteund door uw abonnement en de resourcegroep worden als opties weergegeven.

    e. **Opslag-Account** (optioneel): een **algemene** opslag-account u (koppeling) koppelen aan uw nieuwe Batch-account. Zie [gekoppelde Azure Storage account](#linked-azure-storage-account) hieronder voor meer informatie.

4. Klik op **maken** om het account te maken.

  De portal geeft aan dat het **implementeren** van het account is voltooid, een melding **implementaties is voltooid** wordt weergegeven in *meldingen*.

## <a name="view-batch-account-properties"></a>Eigenschappen van account Batch weergeven

Zodra het account is gemaakt, kunt u de **Batch account blade** voor toegang tot de instellingen en eigenschappen te openen. U kunt alle accountinstellingen en eigenschappen openen via het linkermenu van het blad Batch-account.

![Batch account blade in Azure-portal][account_blade]

* **Batch account URL**: toepassingen die u met de [Batch ontwikkeling API's maakt](batch-technical-overview.md#batch-development-apis) de URL van een account voor het beheren van resources en uitvoeren van taken in het account nodig. De URL van een Batch-account heeft de volgende indeling:

    `https://<account_name>.<region>.batch.azure.com`

![URL van de batch-account in de portal][account_url]

* **Toegangstoetsen**: uw toepassingen ook toegangstoets nodig hebt bij het werken met resources in uw account Batch. Als u wilt weergeven of van uw account van de Batch toegangstoetsen genereren, voert u `keys` in **het linkermenu zoekvak op het blad Batch-account,** en selecteer vervolgens **toetsen**.

    ![Batch account toetsen in Azure-portal][account_keys]

## <a name="pricing"></a>Prijzen

Batchaccounts worden aangeboden alleen in een 'gratis laag,' wat betekent dat er worden niet in rekening gebracht voor de Batch account zelf. U wordt geheven voor de onderliggende Azure berekeningscluster resources die uw oplossingen Batch gebruiken en voor de resources die door andere services wordt gebruikt wanneer uw werkbelasting uitvoert. Bijvoorbeeld, wordt geheven voor de knooppunten berekeningscluster in uw toepassingen en voor de gegevens die u opslaat in Azure opslag als invoer of uitvoer voor uw taken. Op dezelfde manier als u de functie [toepassingspakketten](batch-application-packages.md) van Batch gebruikt, wordt geheven voor de opslag van Azure-resources die worden gebruikt voor het opslaan van uw toepassingspakketten. Zie [Batch prijzen] [ batch_pricing] voor meer informatie.

## <a name="linked-azure-storage-account"></a>Gekoppelde opslag van Azure-account

Zoals eerder is vermeld, kunt u een **algemene** opslag-account (optioneel) koppelen aan uw account Batch. De functie [toepassingspakketten](batch-application-packages.md) van Batch gebruikt blobopslag in een gekoppelde algemene opslag-account, de [Batch bestand conventies .NET](batch-task-output.md) -bibliotheek. Deze optioneel functies helpen u bij het distribueren van de toepassingen die uw batchtaken uitvoeren en persistent maken van de gegevens die ze produceren.

Momenteel ondersteunt *alleen* het accounttype voor **algemene** opslag batch volgens de beschrijving in stap 5, [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account), van [over Azure opslag-accounts](../storage/storage-create-storage-account.md). Wanneer u een opslag van Azure-account aan uw account Batch koppelen, moet ervoor koppeling *alleen* een **algemene** opslag-account.

![Een "Algemene" opslag-account maken][storage_account]

Het is raadzaam dat u een account opslag voor exclusief gebruik door uw account Batch maken.

>[AZURE.WARNING] Wees voorzichtig wanneer de toegangstoetsen van een gekoppelde opslag-account opnieuw te genereren. Slechts één opslag accountsleutel genereren en klik op **Synchroniseren toetsen** op het gekoppelde opslag account blad. Wacht vijf minuten toe te staan dat de toetsen om doorgegeven aan de knooppunten berekenen in uw groepen, vervolgens genereren en de andere toets synchroniseren indien nodig. Als u beide toetsen tegelijkertijd, uw knooppunten berekeningscluster niet mogelijk om te synchroniseren van een sleutel en ze geen toegang meer aan het account opslag.

  ![Opslag account toetsen genereren][4]

## <a name="batch-service-quotas-and-limits"></a>Quota voor batch-service en -limieten

Zorg ervoor dat met uw Azure abonnement en andere Azure-services, bepaalde [quota en limieten](batch-quota-limit.md) van toepassing op Batchaccounts. Huidige quota voor een account Batch worden weergegeven in de portal in de **Eigenschappen**van het account.

![Quota voor batch-account in Azure-portal][quotas]

Houd er rekening mee deze quota als u ontwerpen en schaalbaarheid van uw werkbelasting Batch zijn. Bijvoorbeeld: als uw groep is niet kunnen het doel aantal berekeningscluster knooppunten die u hebt opgegeven bereiken, u mogelijk hebt bereikt de core quotumlimiet voor uw account Batch.

Bedenk ook dat u niet tot één Batch account voor uw Azure-abonnement beperkt worden. U kunt meerdere Batch werkbelasting uitvoeren in één Batch account of distributie van uw werkbelasting tussen Batchaccounts in hetzelfde abonnement, maar in verschillende Azure regio's.

Veel van deze quota kunnen worden verhoogd gewoon met een verzoek om gratis product die in de portal van Azure verzonden. Zie [quota en beperkingen voor de Batch Azure-service](batch-quota-limit.md) voor meer informatie over het aanvragen van een quota toeneemt.

## <a name="other-batch-account-management-options"></a>Het hulpprogramma voor het beheer van andere Batch-Accountopties

Naast de Azure-portal, kunt u ook maken en beheren van Batchaccounts met de volgende items:

* [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Volgende stappen

* Zie [overzicht van de functie Azure Batch](batch-api-basics.md) voor meer informatie over de Batch service concepten en functies. Het artikel wordt beschreven hoe de primaire Batch bronnen zoals groepen, berekeningscluster knooppunten, taken en taken en krijgt u een overzicht van de service-functies die wel grootschalige berekeningscluster werkbelasting kan worden uitgevoerd.

* Leer de basisprincipes van het ontwikkelen van een Batch-toepassingen met de [Batch .NET-client-bibliotheek](batch-dotnet-get-started.md). Het [inleidende artikel](batch-dotnet-get-started.md) begeleidt u bij een werken-toepassing die u gebruikt de Batch om een werkbelasting uitvoeren op meerdere berekeningscluster knooppunten en bevat met behulp van Azure-opslag voor werkbelasting bestand tijdelijke en ophalen.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Opslag account toetsen genereren"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
