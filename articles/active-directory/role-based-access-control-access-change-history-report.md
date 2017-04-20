<properties
    pageTitle="Maken van een access-rapport wijzigen geschiedenis | Microsoft Azure"
    description="Een rapport waarin alle wijzigingen in de toegang tot uw Azure abonnementen met toegangsbeheer op basis van rollen de afgelopen 90 dagen genereren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Een access-rapport wijzigen geschiedenis maken

Elk gewenst moment iemand verleent of trekt toegang binnen uw abonnementen u de wijzigingen geregistreerd in Azure gebeurtenissen. U kunt rapporten van access wijzigen Geschiedenis om te zien van alle wijzigingen voor de afgelopen 90 dagen maken.

## <a name="create-a-report-with-azure-powershell"></a>Een rapport maken met Azure PowerShell
Als u wilt een access-rapport wijzigen geschiedenis in PowerShell maken, gebruikt u de `Get-AzureRMAuthorizationChangeLog` opdracht. Meer informatie over deze cmdlet zijn beschikbaar in de [Galerie met PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Wanneer u deze opdracht belt, kunt u opgeven welke eigenschap van de toewijzingen die u vermelden, inclusief het volgende wilt:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| **Actie** | Opgeven of toegang is verleend of ingetrokken |
| **Beller** | De eigenaar bent verantwoordelijk voor het wijzigen van access |
| **Datum** | De datum en tijd waarop toegang is gewijzigd |
| **DirectoryName** | De map Azure Active Directory |
| **PrincipalName** | De naam van de gebruiker, groep of toepassing |
| **PrincipalType** | Opgeven of de toewijzing is voor een gebruiker, groep of toepassing |
| **RoleId** | De GUID van de rol die is verleend of ingetrokken |
| **Functienaam** | De rol die is verleend of ingetrokken |
| **Scope-naam** | De naam van het abonnement, resourcegroep of resource |
| **ScopeType** | Opgeven of de toewijzing is aan het abonnement, resourcegroep of resource-bereik |
| **SubscriptionId** | De GUID van de Azure-abonnement |
| **SubscriptionName** | De naam van de Azure abonnement |

In dit voorbeeldopdracht worden alle wijzigingen in access in het abonnement voor de afgelopen zeven dagen:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - schermafbeelding](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Een rapport maken met Azure CLI
Als u wilt maken van een access-geschiedenisrapport van wijzigen in de opdrachtregel Azure (CLI), gebruikt u de `azure role assignment changelog list` opdracht.

## <a name="export-to-a-spreadsheet"></a>Exporteren naar een werkblad
Als u wilt het rapport opslaat of de gegevens bewerken, exporteert u de access-wijzigingen in een CSV-bestand. Vervolgens kunt u het rapport bekijken in een werkblad voor revisie.

![Changelog weergegeven als een werkblad - schermafbeelding](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Zie ook
- Aan de slag met [Toegangsbeheer Azure Role-Based](role-based-access-control-configure.md)
- Werken met [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md)
