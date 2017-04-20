<properties
    pageTitle="Servicequota voor de en -limieten batch | Microsoft Azure"
    description="Meer informatie over standaard Azure Batch quota, grenzen en beperkingen, en over het aanvragen van quotum verhogen"
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
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Quota en -limieten voor de Batch Azure-service

Als met andere services Azure, zijn er beperkingen voor bepaalde bronnen die is gekoppeld aan de Batch-service. Veel van deze limieten zijn standaardquota toegepast door Azure op abonnement of account bevinden. In dit artikel wordt beschreven hoe deze standaardwaarden en hoe u kunt ook de quota verhogen.

Als u van plan bent om uit te voeren productie werkbelasting in Batch, moet u mogelijk een of meer van de quota boven de standaard vergroten. Als u een quotum verheffen wilt, opent u een online [klantondersteuning verzoek](#increase-a-quota) gratis.

>[AZURE.NOTE] Een quotum is de limiet van een creditcard, geen capaciteit garantie. Als u een grootschalige die u nodig hebt, neem contact op met ondersteuning voor Azure.

## <a name="subscription-quotas"></a>Quota voor abonnement
**Resource**|**Standaardlimiet**|**Maximumlimiet**
---|---|---
Batchaccounts per regio per abonnement | 1 | 50

## <a name="batch-account-quotas"></a>Quota voor batch-account
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Andere limieten
**Resource**|**Maximumlimiet**
---|---
[Gelijktijdige taken](batch-parallel-node-tasks.md) per berekeningscluster knooppunt | 4 x aantal knooppunt kernen
[Toepassingen](batch-application-packages.md) per Batch-account        | 20
Toepassingspakketten per toepassing  | 40
Grootte van de toepassing-pakket (elk)       | Ongeveer 195GB<sup>1</sup>

<sup>1</sup> azure opslaglimiet voor maximale blok blobgrootte

## <a name="view-batch-quotas"></a>Quota voor Batch weergeven

Uw Batch account quota's bekijken in de [portal van Azure][portal].

1. Selecteer van **Batchaccounts** in de portal en selecteer de Batch account waarmee die u geïnteresseerd bent.

2. Selecteer **Eigenschappen** op van het account van de Batch menu blade

3. Het blad eigenschappen worden weergegeven de **quota** momenteel wordt toegepast op de Batch-account

    ![Quota voor batch-account][account_quotas]

## <a name="increase-a-quota"></a>Een quotum vergroten

Volg de onderstaande stappen voor het aanvragen van een quotum verhogen met behulp van de [Azure portal][portal].

1. Selecteer de tegel **Help + ondersteuning** op uw portal dashboard of op het vraagteken (**?**) in de rechterbovenhoek van de portal.

2. Selecteer **nieuwe ondersteuning verzoek** > **Basisbeginselen**.

3. Klik op het blad **Basisbeginselen** :

    een. **Type probleem** > **Quotum**

    b. Selecteer uw abonnement.

    c. **Quotum type** > **Batch**

    d. **Ondersteuning voor abonnement** > **Quotum ondersteuning - opgenomen**

    Klik op **volgende**.

4. Klik op het blad **probleem** :

    een. Selecteer een **ernst** op basis van uw [bedrijfsresultaten][support_sev].

    b. Geef elke quotum die u wilt wijzigen, de naam van het Batch-account en de nieuwe limiet in **Details**.

    Klik op **volgende**.

5. Klik op het blad **contactgegevens** :

    een. Een **voorkeur contactmethode**selecteert.

    b. Controleer en voer de vereiste gegevens van de contactpersoon.

    Klik op **maken** om de aanvraag ondersteuning te verzenden.

Zodra u uw verzoek voor ondersteuning hebt verzonden, Azure er wordt contact met u. Houd er rekening mee dat het voltooien van de aanvraag maximaal 2 werkdagen kan duren.

## <a name="related-topics"></a>Verwante onderwerpen

* [Maak een Batch Azure-account met behulp van de Azure portal](batch-account-create-portal.md)

* [Overzicht van de functie Azure Batch](batch-api-basics.md)

* [Azure-abonnement en limieten van de service, quota en beperkingen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
