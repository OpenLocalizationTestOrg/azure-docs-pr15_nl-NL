<properties
    pageTitle="Azure Active Directory-PowerShell-cmdlets van preview voor groepsbeheer in Azure AD | Microsoft Azure"
    description="Deze pagina bevat PowerShell voorbeelden om u te helpen bij het beheren van uw groepen in Azure Active Directory"
    keywords="Azure AD, Azure Active Directory, PowerShell, groepen, groepsbeheer"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory preview-cmdlets om uw groep te beheren

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure klassieke portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Het volgende document, vindt u voorbeelden van PowerShell gebruiken voor het beheren van uw groepen in Azure Active Directory (Azure AD).  Ook vindt u informatie over het instellen met de Azure AD PowerShell preview-module. Eerst moet u [downloaden van de Azure AD PowerShell-module](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Installatie van de Azure AD PowerShell-module

Als u wilt de AzureAD PowerShell preview-module hebt geïnstalleerd, kunt u de volgende opdrachten gebruiken:

    PS C:\Windows\system32> install-module azureadpreview

Om te bevestigen dat de module preview is geïnstalleerd, kunt u de volgende opdracht gebruiken:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

U kunt nu starten met de cmdlets in de module. Raadpleeg de [online documentatie](https://msdn.microsoft.com/library/azure/mt757216.aspx)voor een volledige beschrijving van de cmdlets in de module AzureAD Preview.

## <a name="connecting-to-the-directory"></a>Verbinding maken met de adreslijst
Voordat u kunt groepen met Azure AD PowerShell preview-cmdlets beheren, moet u uw PowerShell-sessie koppelen aan de map die u wilt beheren. Gebruik hiervoor de volgende opdracht uit:

    PS C:\Windows\system32> Connect-AzureAD -Force

De cmdlet wordt u gevraagd om de referenties die u gebruiken wilt voor toegang tot het telefoonboek van uw. In dit voorbeeld gebruiken we karen@drumkit.onmicrosoft.com voor toegang tot de adreslijst demonstratie. De cmdlet retourneert een bevestiging om weer te geven dat de sessie met succes is verbonden met uw adreslijst:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

U kunt nu gaan de AzureAD preview-cmdlets gebruiken om groepen in uw adreslijst te beheren.

## <a name="retrieving-groups"></a>Groepen ophalen
U kunt de cmdlet Get-AzureADGroups gebruiken om bestaande groepen zoeken in uw adreslijst. Als u wilt ophalen alle groepen in de adreslijst, gebruikt u de cmdlet zonder parameters:

    PS C:\Windows\system32> get-azureadgroup

De cmdlet retourneert alle groepen in de verbonden adreslijst.

U kunt de parameter - object-id gebruiken om op te halen van een specifieke groep waarvoor u een van de groep object-id opgeven:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

De cmdlet retourneert nu de groep waarvan object-id overeenkomt met de waarde van de parameter die u hebt ingevoerd:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

U kunt zoeken naar een specifieke groep met de - filterparameter. Deze parameter wordt een ODATA-filter-component en geeft als resultaat alle groepen die overeenkomen met het filter, zoals in het volgende voorbeeld:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Houd er rekening mee dat het AzureAD PowerShell-cmdlets de OData-query standaard implementeren, meer informatie vindt u [hier](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Maken van groepen
Als u wilt een nieuwe groep maken in uw adreslijst, gebruikt u de cmdlet New-AzureADGroup. Deze cmdlet Hiermee maakt u een nieuwe beveiligingsgroep 'Marketing' genoemd:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Groepen bijwerken
Als u een bestaande groep bijwerken, gebruikt u de cmdlet Set-AzureADGroup. In dit voorbeeld wilt wordt de eigenschap weergavenaam van de groep 'Intune Administrators.' wijzigen Eerst we de groep met de cmdlet Get-AzureADGroup bent zoeken en filteren met het kenmerk om:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

We bent de eigenschap Beschrijving vervolgens wordt gewijzigd in de nieuwe waarde 'Intune apparaat beheerders':

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nu als we de groep opnieuw vinden, dat de eigenschap Description wordt bijgewerkt met de nieuwe waarde ziet:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Groepen verwijderen
Als u wilt verwijderen groepen uit uw adreslijst, gebruikt u de cmdlet verwijderen-AzureADGroup als volgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Leden van groepen beheren
Als u nieuwe leden toevoegen aan een groep moet, gebruikt u de cmdlet toevoegen-AzureADGroupMember. Deze opdracht voegt een lid aan de beheerdersgroep van Intune die we in het vorige voorbeeld hebt gebruikt:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de id van de groep die we wilt toevoegen van een lid en de RefObjectId - id van de gebruiker wilt toevoegen als lid aan de groep is.

Als u de bestaande leden van een groep, gebruikt u de cmdlet Get-AzureADGroupMember, zoals in dit voorbeeld:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Als u wilt het lid dat we eerder hebt toegevoegd aan de groep verwijderen, gebruikt u de cmdlet verwijderen-AzureADGroupMember, zoals hier wordt weergegeven:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Als u wilt controleren of de groep membership(s) van een gebruiker, gebruikt u de cmdlet Selecteer-AzureADGroupIdsUserIsMemberOf. Deze cmdlet neemt als de parameters id van de gebruiker waarvan u het groepslidmaatschap controleren en een lijst met groepen waarvoor u de lidmaatschappen controleren. De lijst met groepen te worden gegeven in de vorm van een complex variabele van het type 'Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck', zodat we eerst een variabele met dat type maken moet:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Wij bieden vervolgens waarden voor de groupIds in het kenmerk "GroupIds" van deze variabele complex te checken:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nu, als u wilt controleren het groepslidmaatschap van een gebruiker met object-id 72cd4bbd-2594-40a2-935c-016f3cfeeeea ten opzichte van de groepen in $g, dat we moet gebruiken:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


De waarde die het resultaat is een lijst met groepen waarin deze gebruiker lid is. U kunt ook deze methode als u wilt controleren, contactpersonen, groepen of Service principes lidmaatschap voor een bepaalde lijst met groepen, selecteer-AzureADGroupIdsContactIsMemberOf, selecteer-AzureADGroupIdsGroupIsMemberOf of selecteer AzureADGroupIdsServicePrincipalIsMemberOf toepassen

## <a name="managing-owners-of-groups"></a>Eigenaren van groepen beheren
Als u wilt eigenaren toevoegen aan een groep, gebruikt u de cmdlet toevoegen-AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de id van de groep die we wilt toevoegen van een eigenaar en de RefObjectId - id van de gebruiker wilt toevoegen als een eigenaar van de groep is.

Als u wilt ophalen de eigenaren van een groep, gebruikt u de Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

De cmdlet retourneert de lijst met eigenaren voor de opgegeven groep:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Als u een eigenaar uit een groep verwijderen wilt, gebruikt u verwijderen AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Volgende stappen

U vindt meer Azure Active Directory PowerShell-documentatie bij [Azure Active Directory-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
