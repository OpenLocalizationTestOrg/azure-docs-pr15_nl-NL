<properties
    pageTitle="De belangrijkste kluis tenant-ID wijzigen nadat een abonnement hebt verplaatst | Microsoft Azure"
    description="Meer informatie over het overschakelen van de tenant-ID voor een belangrijke kluis nadat een abonnement wordt verplaatst naar een andere tenant"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Een belangrijke kluis tenant-ID wijzigen nadat een abonnement verplaatsen
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>V: Mijn abonnement is verplaatst van tenant A tenant b te drukken. Hoe het wijzigen van de tenant-ID voor mijn bestaande belangrijke kluis en ik juiste ACL's instellen voor principes in B-tenant?

Wanneer u een nieuwe belangrijke kluis in een abonnement maakt, wordt deze automatisch gekoppeld aan de Azure Active Directory-tenant-ID voor dit abonnement. Alle gegevens van access beleid zijn ook gekoppeld aan deze tenant-ID. Wanneer u naar uw Azure-abonnement van tenant A tenant B, zijn uw bestaande belangrijke kluizen niet toegankelijk door de principes (gebruikers en toepassingen) in de tenant b te drukken. U lost dit probleem op door moet u:

- Wijzigen van de tenant-ID die is gekoppeld aan alle bestaande belangrijke kluizen in dit abonnement tenant b te drukken.
- Verwijder alle bestaande access beleid posten.
- Voeg nieuwe items van de access-beleid die zijn gekoppeld aan de tenant b te drukken.

Bijvoorbeeld, als er belangrijke kluis 'myvault' in een abonnement dat is verplaatst van tenant A tenant B, als volgt van het wijzigen van de tenant-ID voor deze toets kluis en oude clienttoegangsbeleid verwijderen.

<pre>
$vaultResourceId = (get-AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() Set-AzureRmResource - ResourceId $vaultResourceId-eigenschappen $vault. Eigenschappen
</pre>

Omdat deze kluis in de tenant A voordat de verplaatsen, de oorspronkelijke waarde van **$vault is. Properties.TenantId** tenant A, terwijl **(Get-AzureRmContext) is. Tenant.TenantId** is tenant b te drukken.

Nu uw kluis gekoppeld aan de juiste tenant-ID is en oude access beleid posten worden verwijderd, instellen nieuwe toegang beleid posten met [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Volgende stappen

Als u vragen over Azure-toets kluis hebt, bezoekt u de- [Forums van Azure toets kluis](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
