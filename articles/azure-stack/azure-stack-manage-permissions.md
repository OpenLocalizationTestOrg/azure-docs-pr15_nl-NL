<properties
    pageTitle="Machtigingen beheren voor resources per gebruiker Azure gestapelde (service-beheerder en tenant) | Microsoft Azure"
    description="Als een service-beheerder of de tenant, informatie over het beheren van machtigingen aan resources per gebruiker."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Gebruikersmachtigingen beheren

Een gebruiker in Azure stapel kan reader, eigenaar of een inzender voor elk exemplaar van een abonnement, resourcegroep of service zijn. Gebruiker A mogelijk bijvoorbeeld lezer machtigingen om abonnement 1, maar de machtigingen van de eigenaar tot en met 7 VM hebt.

-   Lezer: Gebruiker kunt Alles weergeven, maar geen wijzigingen kunt aanbrengen.

-   Inzender: Gebruiker, kunt alles behalve toegang tot bronnen beheren.

-   Eigenaar: Gebruiker, kunt alles, ook toegang tot bronnen beheren.


## <a name="set-access-permissions-for-a-user"></a>Machtigingen instellen voor een gebruiker

1.  Aanmelden met een account die eigenaarsmachtigingen heeft voor de resource die u wilt beheren.

2.  Klik op het **Access** -pictogram in het blad voor de resource, ![](media/azure-stack-manage-permissions/image1.png).

3.  Klik in het blad **gebruikers** , op **rollen**.

4.  Klik op **toevoegen** om machtigingen voor de gebruiker in het blad **rollen** .

## <a name="next-steps"></a>Volgende stappen

[Een stapel Azure-tenant toevoegen](azure-stack-add-new-user-aad.md)
