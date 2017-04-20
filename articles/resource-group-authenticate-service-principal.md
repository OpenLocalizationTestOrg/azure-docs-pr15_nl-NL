<properties
   pageTitle="Azure-service principal maken met PowerShell | Microsoft Azure"
   description="Wordt beschreven hoe Azure PowerShell gebruiken voor het maken van een toepassing voor de Active Directory en de service principal en toegang tot bronnen via Rolgebaseerd toegangsbeheer. Deze ziet hoe u het verifiëren van toepassing met een wachtwoord of het certificaat."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Azure PowerShell gebruiken om te maken van een service principal voor toegang tot bronnen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Wanneer u een toepassing of script die vereist zijn voor toegang tot bronnen hebt, wilt hebt u waarschijnlijk niet uitvoeren dit proces met uw eigen referenties. Mogelijk hebt u verschillende machtigingen die u wilt gebruiken voor de toepassing en u niet wilt dat de toepassing om door te gaan met uw referenties als verantwoordelijkheden wijzigen. In plaats daarvan kunt u een identiteit voor de toepassing met verificatiereferenties en roltoewijzingen maken. Telkens wanneer de app wordt uitgevoerd, deze wordt geverifieerd met deze referenties. In dit onderwerp ziet u hoe u [Azure PowerShell](powershell-install-configure.md) gebruiken voor het instellen van alles wat die u nodig voor een toepassing hebt wilt uitvoeren onder een eigen referenties en -identiteit.

Met PowerShell hebt u twee opties voor het verifiëren van uw AD-toepassing:

 - wachtwoord
 - certificaat

In dit onderwerp wordt uitgelegd hoe u beide opties gebruiken in PowerShell. Als u zich wilt aanmelden bij Azure uit een programming framework (zoals Python, Ruby of Node.js), is het wachtwoordverificatie mogelijk de beste optie. Voordat u beslist of een wachtwoord of het certificaat wilt gebruiken, raadpleegt u de sectie [voorbeeldtoepassingen](#sample-applications) voor de voorbeelden in de verschillende kaders te verifiëren.

## <a name="active-directory-concepts"></a>Active Directory-concepten

In dit artikel, moet u twee objecten - de toepassing AD (Active Directory) en de service principal maken. De AD-toepassing is de algemene weergave van uw toepassing. De presentatie bevat de referenties (een toepassings-id en een wachtwoord of certificaat). De hoofdsom service is de lokale weergave van uw toepassing in een Active Directory. De presentatie bevat de roltoewijzing. In dit onderwerp is gericht op een toepassing voor de één-tenant waar de toepassing is bedoeld om uit te voeren in slechts één organisatie. Meestal gebruikt u één-tenant toepassingen voor LOB-toepassingen die worden uitgevoerd binnen uw organisatie. In een enkel-tenant-toepassing hebt u een AD-app en één service principal.

Vraagt u zich worden af - waarom heb ik nodig beide objecten? Deze benadering is handig wanneer u rekening houden met meerdere tenant-toepassingen. Meestal gebruikt u meerdere tenant toepassingen voor software-als-een-service (SaaS)-toepassingen, waar uw toepassing wordt uitgevoerd in veel verschillende abonnementen. Voor meerdere tenant-toepassingen hebt u een AD-app en meerdere service principes (één in elke Active Directory die toegang tot de app verleent). Raadpleeg [de handleiding voor ontwikkelaars naar autorisatie met de API van Azure resourcemanager](resource-manager-api-authentication.md)instellen van een toepassing voor meerdere tenant.

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt voltooien in dit onderwerp, moet u over de machtigingen in zowel uw Azure Active Directory en uw Azure-abonnement. Specifiek, moet u kunnen maken van een app in de Active Directory en de hoofdsom service toewijzen aan een rol. 

In uw Active Directory moet uw account een beheerder (zoals **Globale beheerder** of **Beheerder van de gebruiker**). Als uw account is toegewezen aan de rol van de **gebruiker** , moet u beschikken over een beheerder uw machtigingen verhogen.

Uw abonnement, moet uw account hebben `Microsoft.Authorization/*/Write` toegang is verleend tot en met de rol van de [eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) of de [gebruiker toegang tot](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) de beheerdersrol. Als uw account is toegewezen aan de rol **Inzender** , ontvangt u een fout tijdens een poging om de service-hoofdsom toewijzen aan een rol. Klik nogmaals moet de beheerder van uw abonnement u voldoende toegang verlenen.

Nu gaat u verder met een sectie voor [wachtwoord](#create-service-principal-with-password) of [certificaat](#create-service-principal-with-certificate) verificatie.

## <a name="create-service-principal-with-password"></a>Service principal maken met een wachtwoord

In deze sectie, kunt u de stappen voor het uitvoeren:

- de AD-toepassing maken met een wachtwoord
- de service-hoofdsom maken
- de rol van de lezer toewijzen aan de hoofdsom service

Als u wilt snel deze stappen hebt uitgevoerd, raadpleegt u de volgende drie cmdlets. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

In dit artikel leest u over deze stappen uitgebreider om ervoor te zorgen dat u het proces kennen.

1. Meld u aan bij uw account.

        Add-AzureRmAccount

1. Maak een nieuwe Active Directory-toepassing doordat een weergavenaam, de URI waarmee uw toepassing wordt beschreven, de URI's die uw toepassing en het wachtwoord voor de identiteit van uw toepassingen identificeren.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     De URI's voor één-tenant-toepassingen, worden niet worden gevalideerd.
     
     Als uw account heeft geen de [vereiste machtigingen](#required-permissions) op de Active Directory, wordt een foutbericht weergegeven waarin wordt aangegeven dat 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.

1. Bekijk de nieuwe application-object. 

        $app
        
     Opmerking met name de eigenschap **ApplicationId** , die nodig is voor het maken van service principes, roltoewijzingen, en bij het verkrijgen van het toegangstoken.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Maak de hoofdsom van een service voor uw toepassing door doorgeven in de toepassings-id van de Active Directory-toepassing.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Verleen de service principal machtigingen voor uw abonnement. In dit voorbeeld kunt u de service-hoofdsom toevoegen aan de functie **Reader** , die verleent leesmachtiging alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde rollen](./active-directory/role-based-access-built-in-roles.md). Geef de **ApplicationId** die u hebt gebruikt bij het maken van de toepassing voor de parameter **ServicePrincipalName** . 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht wordt weergegeven. Het bericht geeft aan uw account **heeft geen autorisatie actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid} wordt uitgevoerd**. 

Dat is alles. Uw AD-toepassing en service principal zijn ingesteld. Het volgende gedeelte ziet u hoe u aanmelden met de referentie via PowerShell. Als u de referentie gebruiken in uw toepassing code wilt, kunt u kunt gaan naar de [voorbeeld-toepassingen](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Uw referenties via PowerShell

Nu, moet u aanmelden als de toepassing bewerkingen uit te voeren.

1. Maak een **PSCredential** -object dat uw referenties met de opdracht **Get-referentie** bevat. Moet u eerst de **ApplicationId** voordat u deze opdracht dus zorg ervoor dat u hebt dat beschikbaar is om te plakken.

        $creds = Get-Credential

2. U wordt gevraagd u uw referenties invoeren. Voor de gebruikersnaam in te voeren, gebruikt u de **ApplicationId** die u hebt gebruikt bij het maken van de toepassing. Voor het wachtwoord, gebruikt u het account dat u hebt opgegeven bij het maken van het account.

     ![Voer referenties](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Wanneer u zich als een service principal aanmelden, moet u de tenant-id van de map opgeven voor de AD-app. Een tenant is een exemplaar van Active Directory. Als u slechts één abonnement hebt, kunt u het volgende gebruiken:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Als u meer dan één abonnement hebt, geeft u het abonnement waar uw Active Directory zich bevindt. Zie [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)voor meer informatie.

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Meld u aan als de service-principal door op te geven dat dit account een service principal is en door het object referenties. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     U bent nu geverifieerd als de service hoofdsom voor de Active Directory-toepassing die u hebt gemaakt.

### <a name="save-access-token-to-simplify-log-in"></a>Toegangstoken om te vereenvoudigen aanmelden opslaan

Als u wilt voorkomen dat de service principal referenties leveren telkens wanneer deze moeten aanmelden, kunt u het toegangstoken opslaan.

1. Als u wilt gebruiken het huidige toegangstoken in een latere sessie, sla het profiel.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Open het profiel en de inhoud ervan controleren. Zoals u ziet dat dit een toegangstoken bevat. 
        
2. In plaats van handmatig opnieuw logboekregistratie in, moet u gewoon het profiel geladen.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Het toegangstoken verloopt, zodat alleen voor gebruik van een opgeslagen profiel werkt zo lang maken als het token geldig is.
        
## <a name="create-service-principal-with-certificate"></a>Service principal maken met certificaat

In deze sectie, kunt u de stappen voor het uitvoeren:

- een zelfondertekend certificaat maken
- de AD-toepassing maken met het certificaat
- de service-hoofdsom maken
- de rol van de lezer toewijzen aan de hoofdsom service

Zie de volgende cmdlets uit als u wilt snel uitvoeren van deze stappen met Azure PowerShell 2.0 op Windows 10 of Windows Server 2016 Technical Preview. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

In dit artikel leest u over deze stappen uitgebreider om ervoor te zorgen dat u het proces kennen. In dit artikel leest ook hoe u taken uitvoeren als u een eerdere versie van Azure PowerShell of besturingssystemen.

### <a name="create-the-self-signed-certificate"></a>De zelfondertekend certificaat maken

De versie van PowerShell die beschikbaar zijn voor Windows 10 en Windows Server 2016 Technical Preview heeft een bijgewerkte **Nieuw SelfSignedCertificate** -cmdlet voor het genereren van een zelfondertekend certificaat. Eerdere versies van besturingssysteem heb de cmdlet New-SelfSignedCertificate maar biedt geen de parameters die u nodig hebt voor dit onderwerp. In plaats daarvan, moet u een module om te genereren van het certificaat importeren. In dit onderwerp ziet u beide methoden voor het genereren van het certificaat op basis van het besturingssysteem dat u hebt. 

- Als u **Windows 10 of Windows Server 2016 Technical Preview**hebt, voert u de volgende opdracht uit een zelfondertekend certificaat maken: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Als u **geen Windows 10 of Windows Server 2016 Technical Preview**, moet u de [zelfondertekend certificaat genereren](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) downloaden van Microsoft Script Center. De inhoud ervan te extraheren en importeer de cmdlet die u nodig hebt.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Genereer vervolgens het certificaat.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

U hebt uw certificaat en u kunt doorgaan met het maken van uw AD-app.

### <a name="create-the-active-directory-app-and-service-principal"></a>De Active Directory-app en service principal maken

1. De sleutelwaarde ophalen met het certificaat.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Meld u aan bij uw Azure-account.

        Add-AzureRmAccount

3. Maak een nieuwe Active Directory-toepassing doordat een weergavenaam, de URI waarmee uw toepassing wordt beschreven, de URI's die uw toepassing en het wachtwoord voor de identiteit van uw toepassingen identificeren.

     Als er Azure PowerShell 2.0 (augustus 2016 of hoger), gebruik de volgende cmdlet:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Als u Azure PowerShell 1.0 hebt, gebruikt u de volgende cmdlet:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    De URI's voor één-tenant-toepassingen, worden niet worden gevalideerd.
    
    Als uw account heeft geen de [vereiste machtigingen](#required-permissions) op de Active Directory, wordt een foutbericht weergegeven waarin wordt aangegeven dat 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.
        
    Bekijk de nieuwe application-object. 

        $app

    U ziet u de eigenschap **ApplicationId** , die nodig is voor het maken van service principes, roltoewijzingen, en bij het verkrijgen van access tokens.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Maak de hoofdsom van een service voor uw toepassing door doorgeven in de toepassings-id van de Active Directory-toepassing.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Verleen de service principal machtigingen voor uw abonnement. In dit voorbeeld kunt u de service-hoofdsom toevoegen aan de functie **Reader** , die verleent leesmachtiging alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde rollen](./active-directory/role-based-access-built-in-roles.md). Geef de **ApplicationId** die u hebt gebruikt bij het maken van de toepassing voor de parameter **ServicePrincipalName** .

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht wordt weergegeven. Het bericht geeft aan uw account **heeft geen autorisatie actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid} wordt uitgevoerd**.

Dat is alles. Uw AD-toepassing en service principal zijn ingesteld. Het volgende gedeelte ziet u hoe u aanmelden met de certificaat via PowerShell.

### <a name="provide-certificate-through-automated-powershell-script"></a>Geef certificaat tot en met geautomatiseerde PowerShell-script

Wanneer u zich als een service principal aanmelden, moet u de tenant-id van de map opgeven voor de AD-app. Een tenant is een exemplaar van Active Directory. Als u slechts één abonnement hebt, kunt u het volgende gebruiken:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Als u meer dan één abonnement hebt, geeft u het abonnement waar uw Active Directory zich bevindt. Zie voor meer informatie [beheren het telefoonboek van uw Azure AD](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Als u wilt verifiëren in het script, geef het account is een service principal en geef de vingerafdruk van het certificaat, de toepassings-id en tenant-id. Als u wilt uw script automatiseren, kunt u deze waarden kunt opslaan als omgevingsvariabelen en deze weer ophalen tijdens de uitvoering, of kunt u deze opnemen in uw script.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

U bent nu geverifieerd als de service hoofdsom voor de Active Directory-toepassing die u hebt gemaakt.

## <a name="sample-applications"></a>Voorbeeldtoepassingen

De volgende toepassingen van de steekproef hoe meld u aan als de hoofdsom service.

**.NET**

- [Een SSH implementeren VM met een sjabloon met .NET ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resources en resourcegroepen met .NET beheren](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Aan de slag met informatiebronnen - implementeren met Azure resourcemanager sjabloon - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Aan de slag met informatiebronnen - beheren resourcegroep - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Een SSH implementeren VM met een sjabloon in Python ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure Resource en resourcegroepen met Python beheren](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Een SSH implementeren VM met een sjabloon in Node.js ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resources en resourcegroepen met Node.js beheren](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Een SSH implementeren VM met een sjabloon in Ruby ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure Resource en resourcegroepen met Ruby beheren](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Volgende stappen
  
- Zie voor meer informatie over de integratie van een toepassing in Azure voor het beheren van resources [handleiding voor ontwikkelaars naar autorisatie met de API van Azure resourcemanager](resource-manager-api-authentication.md).
- Zie voor een gedetailleerde uitleg van toepassingen en service principes, [toepassingen en Service Principal objecten](./active-directory/active-directory-application-objects.md). 
- Zie voor meer informatie over de verificatie van Active Directory, [Verificatie-scenario's voor Azure AD](./active-directory/active-directory-authentication-scenarios.md).



