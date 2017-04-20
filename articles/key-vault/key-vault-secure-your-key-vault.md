<properties
    pageTitle="Uw belangrijkste kluis Secure | Microsoft Azure"
    description="Toegangsmachtigingen voor belangrijke kluis voor het beheren van kluizen en toetsen en geheimen beheren. Verificatie en machtiging model voor belangrijke kluis en hoe u uw sleutel kluis beveiligt"
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
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Uw belangrijkste kluis Secure

Azure-toets kluis is een cloudservice die versleuteling toetsen en geheimen (zoals certificaten, verbindingstekenreeksen met de, wachtwoorden) voor uw cloud-toepassingen beveiligt. Aangezien deze gegevens gevoelige zijn en bedrijven kritieke, die u beveiligen, toegang tot uw belangrijkste kluizen wilt zodat alleen gemachtigde toepassingen en gebruikers hebben toegang tot belangrijke kluis. In dit artikel bevat een overzicht van belangrijke kluis access model, wordt uitgelegd verificatie en autorisatie en wordt uitgelegd hoe u toegang tot belangrijke kluis voor uw cloud-toepassingen met een voorbeeld beveiligt.

## <a name="overview"></a>Overzicht

Toegang tot een belangrijke kluis wordt beheerd via twee afzonderlijke interfaces: vlak van management en gegevens vlak. Voor beide vlakken is BEGINLETTERS verificatie en machtiging vereist voordat een beller (een gebruiker of een toepassing) kunt toegang krijgen tot belangrijke kluis. Verificatie tot stand brengt de identiteit van de beller, terwijl autorisatie bepaalt welke bewerkingen door de beller is toegestaan om uit te voeren.

Gebruik Azure Active Directory voor verificatie zowel vlak van management en gegevens vlak. Vlak van management gebruikt Rolgebaseerd toegangsbeheer (RBAC) voor verificatie, terwijl de gegevens vlak met belangrijke kluis-beleid.

Hier volgt een beknopt overzicht van de onderwerpen:

[Verificatie met behulp van Azure Active Directory](#authentication-using-azure-active-direcrory) - deze sectie wordt uitgelegd hoe een beller met Azure Active Directory voor toegang tot een belangrijke kluis via vlak van management en gegevens vlak wordt geverifieerd. 

[Vlak van management en gegevens vlak](#management-plane-and-data-plane) - vlak van Management en gegevens vlak zijn twee access vlakken gebruikt voor toegang tot uw belangrijkste kluis. Elke vlak toegang ondersteunt specifieke bewerkingen. In deze sectie worden de eindpunten van access, bewerkingen ondersteund en toegang tot besturingselement methode gebruikt door elke vlak. 

[Toegangsbeheer voor management vlak](#management-plane-access-control) - In dit gedeelte bespreken we toestaan van toegang tot management vlak bewerkingen Rolgebaseerd toegangsbeheer gebruikt.

[Gegevens beheren in access vlak](#data-plane-access-control) - deze sectie wordt beschreven hoe belangrijke kluis-beleid gebruiken om te bepalen de toegang tot het vlak van gegevens.

[Voorbeeld](#example) : in dit voorbeeld wordt beschreven hoe voor het instellen van toegangsbeheer voor uw sleutel kluis toe te staan dat drie verschillende teams (beveiligingsteam, ontwikkelaars/operatoren en revisoren) specifieke taken uitvoeren om te ontwikkelen, beheren en controleren van een toepassing in Azure wordt aangegeven.


## <a name="authentication-using-azure-active-directory"></a>Verificatie met behulp van Azure Active Directory

Wanneer u een belangrijke kluis in een Azure-abonnement maakt, wordt deze automatisch gekoppeld aan het abonnement van Azure Active Directory-tenant. Alle bellers (gebruikers en toepassingen) moeten worden geregistreerd in deze tenant voor toegang tot deze key kluis. Een toepassing of een gebruiker moet worden geverifieerd bij Azure Active Directory voor toegang tot belangrijke kluis. Dit geldt voor beide management vlak en gegevens vlak-toegang. In beide gevallen kunt een toepassing belangrijke kluis op twee manieren openen:

-  **gebruiker + app access** - meestal is dit voor toepassingen die toegang hebben tot belangrijke kluis namens een aangemelde gebruiker. Azure PowerShell, Azure-Portal ziet voorbeelden van dit type toegang. Er zijn twee manieren om toegang te verlenen aan gebruikers: kunt u toegang verlenen aan gebruikers, zodat ze toegang heeft tot belangrijke kluis vanuit elke toepassing en de andere manier een gebruikerstoegang geven tot belangrijke kluis alleen wanneer ze een specifieke toepassing is (genoemd samengestelde identiteit) gebruiken. 
-  **app - lezentoegang** - voor-toepassingen die daemon services wordt uitgevoerd, achtergrond taken enzovoort. De identiteit van de toepassing toegang heeft tot de belangrijkste kluis.

In beide soorten toepassingen, wordt met de toepassing verifieert met Azure Active Directory met een van de [ondersteunde verificatiemethoden](../active-directory/active-directory-authentication-scenarios.md) en een token krijgt. Verificatiemethode die wordt gebruikt, is afhankelijk van het toepassingstype. De toepassing wordt vervolgens deze token wordt gebruikt en wordt REST API-aanvraag verzonden naar belangrijke kluis. Voor het geval de toegang tot het vlak van management worden de aanvragen gerouteerd via Azure resourcemanager eindpunt. Bij het openen van gegevens vlak, de toepassingen spreekt rechtstreeks naar een toets kluis eindpunt. Zie meer informatie op de [hele verificatie stroom](../active-directory/active-directory-protocols-oauth-code.md). 

De naam van de resource waarvoor de toepassing vraagt om een token verschilt afhankelijk van of de toepassing toegang heeft tot vlak van management of gegevens vlak. De naam van de resource is dus op management vlak of gegevens vlak de eindpunten beschreven in de tabel verderop, afhankelijk van de Azure-omgeving.

Met één enkele om te verifiëren bij zowel management en gegevens vlak heeft een eigen voordelen:

- Organisaties kunnen toegang tot alle belangrijke kluizen in hun organisatie centraal beheren
- Als een gebruiker verlaat, verliest ze direct toegang tot alle belangrijke kluizen in de organisatie
- Organisaties kunnen verificatie via de opties in Azure Active Directory (bijvoorbeeld inschakelen meervoudige verificatie voor extra beveiliging) aanpassen

## <a name="management-plane-and-data-plane"></a>Vlak van Management en gegevens vlak

Azure-toets kluis is een Azure service verkrijgbaar via Azure resourcemanager implementatiemodel. Als u een belangrijke kluis hebt gemaakt, krijgt u een virtuele container in die u kunt andere objecten zoals toetsen, geheimen en certificaten te maken. Vervolgens kunt u uw sleutel kluis specifieke bewerkingen uitvoeren met vlak van management en gegevens vlak openen. Management vlak interface wordt gebruikt voor het beheren van uw belangrijkste kluis zelf, zoals maken, verwijderen, key kluis kenmerken bijwerken en -beleid voor gegevens vlak instellen. Gegevens vlak interface wordt gebruikt voor het toevoegen, verwijderen, wijzigen en gebruik de toetsen, geheimen en certificaten die zijn opgeslagen in uw belangrijkste kluis.

De management vlak en gegevens vlak-interfaces zijn toegankelijk via verschillende eindpunten (Zie tabel). De tweede kolom in de tabel worden de DNS-namen voor deze eindpunten in verschillende Azure omgevingen. De derde kolom worden de bewerkingen die u vanuit elke vlak access uitvoeren kunt. Elke vlak access heeft ook een eigen toegangsbeveiliging: voor management vlak toegangsbeheer is ingesteld met Azure Resource Manager Role-Based Access besturingselement RBAC (), terwijl u voor gegevens vlak toegangsbeheer is ingesteld met belangrijke kluis-beleid.

| Access vlak | Access-eindpunten | Bewerkingen | Access besturingselement om|
|--------------|------------------|--------------------|--------|
| Vlak van Management|**Globale:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure Amerikaanse overheid:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Duitsland:**<br> Management.microsoftazure.de:443 | Belangrijke kluis maken/gelezen/bijwerken/verwijderen <br> Access-beleid voor belangrijke kluis instellen<br>Labels voor instellen voor belangrijke kluis | Azure Resource Manager Rolgebaseerd toegangsbeheer RBAC) |
| Gegevens vlak | **Globale:**<br> &lt;kluis-naam&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;kluis-naam&gt;. vault.azure.cn:443<br><br> **Azure Amerikaanse overheid:**<br> &lt;kluis-naam&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluis-naam&gt;. vault.microsoftazure.de:443 | Voor sleutels: Ontsleutelen, versleutelen, UnwrapKey, WrapKey, verifiëren, meld u aan, krijgen, lijst, bijwerken, maken, importeren, verwijderen, back-up herstellen<br><br> Voor geheimen: U, lijst, instellen, verwijderen | Belangrijke kluis-beleid|

De management vlak en gegevens vlak toegang tot besturingselementen voor werken onafhankelijk. Als u toegang verlenen een toepassing als u wilt gebruiken in een belangrijke kluis wilt, bijvoorbeeld u hoeft slechts eenmaal om belangrijke kluis clienttoegangsbeleid met vlak-toegangsmachtigingen voor gegevens te verlenen en geen management vlak toegang nodig is voor deze toepassing. En omgekeerd, als u een gebruiker kan kluis-eigenschappen en labels lezen, maar niet hebben geen toegang hebben tot de toetsen, geheimen of certificaten wilt, kunt u deze gebruiker verlenen 'gelezen' toegang tot een RBAC en geen toegang tot gegevens vlak is vereist.

## <a name="management-plane-access-control"></a>Toegangsbeheer voor Management vlak

Het vlak van management bestaat uit de bewerkingen die betrekking hebben op de toets kluis zelf. U kunt bijvoorbeeld maken of verwijderen van een belangrijke kluis. U kunt een lijst met kluizen openen in een abonnement. U kunt de eigenschappen van de belangrijkste kluis (zoals SKU, labels) ophalen en stelt belangrijke kluis clienttoegangsbeleid waarmee wordt bepaald van de gebruikers en toepassingen die toegang heeft tot toetsen en geheimen in de belangrijkste kluis. Management vlak wordt gebruikgemaakt van RBAC. Zie de volledige lijst met belangrijke kluis bewerkingen die kunnen worden uitgevoerd via vlak van management in de tabel in de voorgaande sectie. 

### <a name="role-based-access-control-rbac"></a>Rolgebaseerd toegangsbeheer RBAC)
Elke Azure abonnement heeft een Azure Active Directory. Gebruikers, groepen en toepassingen van deze map kunnen beheren van resources in het Azure abonnement met het implementatiemodel van Azure resourcemanager toegang is verleend. Dit soort toegangsbeheer wordt Rolgebaseerd toegangsbeheer (RBAC) genoemd. Als u wilt deze toegang beheren, kunt u de [Azure-portal](https://portal.azure.com/), de [Hulpmiddelen voor Azure CLI](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)of de [Azure resourcemanager REST API's](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Met het model Azure resourcemanager maakt u uw sleutel kluis in een resource groep en de toegang tot het vlak van management van deze toets kluis met behulp van Azure Active Directory. U kunt bijvoorbeeld gebruikers of een groep mogelijkheid voor het beheren van belangrijke kluizen in een bepaalde resourcegroep verlenen.

U kunt toegang verlenen aan gebruikers, groepen en toepassingen op een bepaald bereik door het juiste RBAC rollen toewijzen. Om toegang te verlenen aan een gebruiker voor het beheren van belangrijke kluizen zou u bijvoorbeeld een vooraf gedefinieerde rol 'key kluis Inzender' toewijzen aan deze gebruiker op een bepaald bereik. Het bereik zou in dit geval zijn een abonnement, een resourcegroep of alleen een specifieke belangrijke kluis. Een rol toegewezen aan het abonnement geldt voor alle resourcegroepen en bronnen in die abonnement. Een toegewezen resource groepeerniveau rol geldt voor alle resources in die resourcegroep. Een rol toegewezen voor een specifieke resource geldt alleen voor die resource. Er zijn verschillende vooraf gedefinieerde rollen (Zie [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md)), en als de vooraf gedefinieerde rollen kunnen niet aan uw wensen voldoet kunt u ook uw eigen rollen definiëren.

>[AZURE.IMPORTANT] Houd er rekening mee dat als een gebruiker heeft Inzender machtigingen (RBAC) om een belangrijke kluis management vlak, ze kan verlenen zichzelf toegang tot gegevens vlak, door de instelling belangrijke kluis-beleid controleert de toegang tot gegevens vlak. Het verdient daarom hecht bepalen wie 'Inzender' toegang heeft tot uw belangrijkste kluizen om ervoor te zorgen alleen geautoriseerde personen kunnen openen en beheren van uw belangrijkste kluizen, sleutels, geheimen en certificaten.

## <a name="data-plane-access-control"></a>Toegangsbeheer voor gegevens vlak

Het belangrijkste kluis gegevens vlak bestaat uit de bewerkingen die betrekking hebben op de objecten in een belangrijke kluis, zoals sleutels, geheimen en certificaten.  Deze groep omvat toets bewerkingen, zoals maken, importeren, update, lijst, back-up en herstellen toetsaanslagen cryptografische bewerkingen zoals aanmelding, verifiëren versleutelen, ontsleutelen, laten teruglopen, en pak en labels en andere kenmerken voor sleutels instellen. Op dezelfde manier voor geheimen gezocht, krijgen, instellen, lijst, verwijderen.

Gegevens vlak toegang wordt verleend door in te stellen-beleid voor een belangrijke kluis. Een gebruiker, groep of een toepassing moet gemachtigd Inzender (RBAC) voor management vlak voor een belangrijke kluis moeten kunnen-beleid voor die belangrijke kluis instellen. Een gebruiker, groep of toepassing kan toegang worden verleend voor het uitvoeren van specifieke bewerkingen voor toetsen of geheimen binnen een belangrijke kluis. belangrijke kluis ondersteuning van maximaal 16 access beleid vermeldingen voor een belangrijke kluis. Maak een beveiligingsgroep Azure Active Directory en gebruikers toevoegen aan de groep gegevens vlak om toegang te verlenen aan meerdere gebruikers aan een belangrijke kluis.

### <a name="key-vault-access-policies"></a>belangrijke kluis Clienttoegangsbeleid

belangrijke kluis clienttoegangsbeleid verlenen machtigingen voor sleutels, geheimen en certificaten afzonderlijk. U kunt bijvoorbeeld een gebruikerstoegang tot alleen sleutels, maar geen machtigingen voor geheimen geven. Machtigingen voor toegang tot toetsen of geheimen of certificaten zijn echter op het niveau van de kluis. Met andere woorden, ondersteunt key kluis-beleid geen niveau objectmachtigingen. U kunt [Azure-portal](https://portal.azure.com/), de [Hulpmiddelen voor Azure CLI](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)of de [belangrijkste kluis Management REST API's](https://msdn.microsoft.com/library/azure/mt620024.aspx) gebruiken om in te stellen-beleid voor een belangrijke kluis.

>[AZURE.IMPORTANT] Houd er rekening mee dat belangrijke kluis access beleid van toepassing op het niveau van de kluis zijn. Bijvoorbeeld wanneer een gebruiker is gemachtigd maken en verwijderen van toetsen, zij deze in kunt bewerkingen uitvoeren op alle sleutels die belangrijke kluis.

## <a name="example"></a>Voorbeeld

Stel dat u een toepassing die gebruikmaakt van een certificaat voor SSL, Azure opslag voor het opslaan van gegevens, en wordt een RSA 2048-bits-sleutel voor aanmelding bewerkingen ontwikkelt. Stel dat u dat deze toepassing wordt uitgevoerd in een VM (of een VM schaal ingesteld). U kunt een belangrijke kluis gebruiken voor de opslag van alle de toepassing geheimen en belangrijke kluis gebruiken voor de bootstrap certificaat dat wordt gebruikt door de toepassing om te verifiëren met Azure Active Directory.

Zo is, volgt hier een overzicht van alle sleutels en geheimen moeten worden opgeslagen in een belangrijke kluis.

- **SSL-certificaat** - gebruikt voor SSL
- **Storage Key** - die wordt gebruikt voor toegang tot opslag-account
- **RSA 2048-bits sleutel** - gebruikt voor aanmelding bewerkingen
- **Bootstrap certificaat** - gebruikt om te verifiëren met Azure Active Directory, toegang kunt krijgen tot belangrijke kluis ophalen van de opslag-toets en gebruiken van de RSA-toets voor de ondertekening.

Nu we voldoen aan de personen die zijn beheren, implementeert en controle van deze toepassing. In dit voorbeeld gebruiken we drie rollen.

- **Beveiligingsteam** - dit zijn meestal IT-afdeling van de 'kantoor van de BBF (directeur beveiliging)' of equivalente, die verantwoordelijk is voor de juiste bewaren geheimen zoals SSL-certificaten, RSA-sleutels gebruikt voor het ondertekenen, verbindingstekenreeksen met de voor databases, opslag account toetsen.
- **Ontwikkelaars/operatoren** - hierna ziet u de mensen die deze toepassing te ontwikkelen en deze vervolgens implementeert in Azure wordt aangegeven. Meestal ze geen deel uitmaken van het beveiligingsteam en daarom ze mogen geen toegang tot alle gevoelige gegevens, zoals SSL-certificaten, RSA-sleutels, maar de toepassing die u toegang tot die nodig hebt.
- **Revisoren** - maar gewoonlijk is dit een andere set personen, geïsoleerd uit de ontwikkelaars en algemene IT-afdeling. Hun verantwoordelijkheid is om te controleren BEGINLETTERS gebruik en onderhoud van certificaten, toetsen, enzovoort en naleving van gegevens beveiligingsstandaarden. 

Is er één meer functie die zich buiten de reikwijdte van deze toepassing, maar gebruik relevante hier te vermelden, en dat is het abonnement (of de resourcegroep) beheerder. De beheerder van het abonnement bepaalt eerste toegangsmachtigingen voor het beveiligingsteam. Hier wordt ervan uitgegaan dat de beheerder van het abonnement heeft toegang tot het security team aan een resourcegroep in welke alle resources die nodig zijn voor deze toepassing wonen.

Nu laten we eens kijken welke acties worden uitgevoerd elke rol in de context van deze toepassing.

- **Beveiligingsteam**
    - Belangrijke kluizen maken
    - Hiermee schakelt u op de toets kluis logboekregistratie
    - Toetsen/geheimen toevoegen
    - Back-up van sleutels voor herstel maken
    - Beleid voor belangrijke kluis toegang instellen om machtigingen te verlenen aan gebruikers en toepassingen specifieke bewerkingen uit te voeren
    - Regelmatig album toetsen/geheimen
- **Ontwikkelaars/operatoren**
    - Verwijzingen naar bootstrap en SSL-certificaten (vingerafdrukken), opslag-sleutel (geheim URI) en de toets (toets URI) van beveiligingsteam ondertekening
    - Ontwikkelen en implementeren van de toepassing die toegang heeft tot toetsen en geheimen via programmacode
- **Revisoren**
    - Gebruikslogboeken bekijken om te bevestigen sleutel/geheim gebruikt en wat de naleving van gegevens beveiliging normen

Nu laten we eens kijken welke machtigingen voor belangrijke kluis nodig zijn voor elke rol (en de toepassing) aan hen toegewezen taken uitvoeren. 

| Gebruikersfunctie    | Machtigingen voor het vlak van Management | Machtigingen voor het vlak van gegevens |
|--------------|------------------------------|------------------------|
|Beveiligingsteam|belangrijke kluis Inzender|Toetsen: back-up maken, maken, verwijderen, krijgen, importeren, lijst, herstellen <br> Geheimen: alle|
|Ontwikkelaars/Operator| belangrijke kluis machtiging implementeren, zodat de VMs ze implementeren geheimen kunnen ophalen uit de belangrijkste kluis | Geen |
|Revisoren| Geen | Toetsen: lijst<br>Geheimen: lijst|
|Toepassing| Geen | Toetsen: aanmelden<br>Geheimen: ophalen |

>[AZURE.NOTE] Revisoren nodig machtiging voor toetsen en geheimen lijst, zodat ze kenmerken voor toetsen en geheimen die niet worden weergegeven in de logboeken, zoals tags, kunnen controleren activering en de vervaldatum.

Naast de machtigingen voor belangrijke kluis moeten alle drie rollen ook toegang tot andere bronnen. Als u bijvoorbeeld mogelijk te implementeren VMs (of Web-Apps enz.) Ontwikkelaars/operatoren moet ook 'Inzender' toegang tot deze resourcetypen. Revisoren moeten alleen toegang tot het opslag-account waar de logboeken aan de belangrijkste kluis worden opgeslagen.

Aangezien de nadruk in dit artikel is de beveiliging van toegang tot uw belangrijkste kluis, we alleen illustreren de relevante delen die betrekking hebben op dit onderwerp en gaat u verder details over certificaten implementeert, toegang krijgen tot toetsen en geheimen via programmacode enz. Deze gegevens zijn gedekt ergens anders. Certificaten die zijn opgeslagen in belangrijke kluis naar VMs implementeert vindt u in een [blog publiceren](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/), en er [steekproef code](https://www.microsoft.com/download/details.aspx?id=45343) beschikbaar die ziet u hoe u gebruik bootstrap certificaat om te verifiëren bij Azure AD toegang kunt krijgen tot belangrijke kluis.

Grootste deel van de toegangsmachtigingen kan worden verleend met behulp van Azure portal, maar als u wilt gedetailleerde machtigingen verlenen moet u wellicht gebruiken Azure PowerShell (of Azure CLI) om het gewenste resultaat. 

Het volgende PowerShell-fragmenten wordt ervan uitgegaan dat:

- De beheerder van de Azure Active Directory heeft gemaakt-beveiligingsgroepen die voor de drie rollen, namelijk Contoso Security Team, Contoso App Devops, Contoso App Auditors. De beheerder heeft gebruikers ook toegevoegd aan de groepen waartoe ze behoren.

- **ContosoAppRG** is de resourcegroep waarin alle resources zijn opgeslagen. **contosologstorage** is waar de logboeken worden opgeslagen. 

- Belangrijke kluis **ContosoKeyVault** en opslagruimte account gebruikt voor belangrijke kluis logboeken **contosologstorage** moeten op dezelfde locatie Azure


De beheerder van het abonnement toegewezen eerst 'belangrijke kluis Inzender' en ' Access Gebruikersbeheerder ' rollen voor de beveiliging-teamsite. Hiermee kunt het security team toegang tot andere resources beheren en beheren van belangrijke kluizen in de resourcegroep ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Het volgende script ziet u hoe het security team een belangrijke kluis, setup logboekregistratie en machtigingen instellen voor de andere rollen en de toepassing kunt maken. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

De aangepaste rol die zijn gedefinieerd, kan alleen toegewezen aan het abonnement waarop de resourcegroep ContosoAppRG wordt gemaakt. Als dezelfde aangepaste rollen wordt gebruikt voor andere projecten in andere abonnementen, kan het bereik meer abonnementen toegevoegd hebt.

De aangepaste roltoewijzing voor ontwikkelaars/operatoren voor de machtiging "implementeren/actie" is beperkt tot de resourcegroep. Deze manier alleen de VMs die zijn gemaakt in de resourcegroep 'ContosoAppRG' krijgt de geheimen (SSL-certificaat en bootstrap certificaat). Een VMs dat een lid van ontwikkelaar/ops team in andere resourcegroep maakt worden niet kunnen bereiken van deze geheimen, zelfs als ze de geheime URI's wist.

In dit voorbeeld ziet u een eenvoudige scenario. Praktijk scenario's is mogelijk meer complexe en mogelijk moet u machtigingen voor uw sleutel kluis op basis van uw behoeften aanpassen. In ons voorbeeld we Stel dat beveiligingsteam, vindt u in de sleutel en geheime verwijzingen (URI's en -vingerafdrukken) dat ontwikkelaars/operatoren team moet in hun toepassingen verwijzen. Daarom hoeven niet ontwikkelaars/operatoren eventuele gegevens vlak om toegang te verlenen. Bedenk ook, dat in dit voorbeeld bevat informatie over het beveiligen van uw belangrijkste kluis. Vergelijkbare daarbij rekening te beveiligen van [uw VMs](https://azure.microsoft.com/services/virtual-machines/security/), [opslag-accounts](../storage/storage-security-guide.md) en andere Azure bronnen.

>[AZURE.NOTE] Opmerking: Dit voorbeeld ziet u hoe belangrijke kluis access worden vergrendeld in productie. De ontwikkelaars moeten hun eigen-abonnement hebt of resourcegroup waarvoor ze volledige machtigingen hebben om hun kluizen, VMs en opslag-account te beheren waar ze de toepassing ontwikkelen.


## <a name="resources"></a>Resources

-   [Azure Active Directory Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md)

    In dit artikel wordt uitgelegd het toegangsbeheer op basis van Azure Active Directory-rol en hoe deze werkt.

-   [RBAC: Ingebouwd in rollen](../active-directory/role-based-access-built-in-roles.md)

    Dit artikel wordt uitgelegd alle ingebouwde functies die beschikbaar zijn in RBAC.

-   [Wat zijn resourcemanager implementatie en klassieke implementatie](../resource-manager-deployment-model.md)

    In dit artikel wordt uitgelegd de resourcemanager implementatie en klassieke implementatiemodellen en wordt uitgelegd van de voordelen van het gebruik van de groepen resourcemanager- en resourcekalenders

-    [Rolgebaseerd toegangsbeheer met Azure PowerShell beheren](../active-directory/role-based-access-control-manage-access-powershell.md)

     In dit artikel wordt uitgelegd hoe u Rolgebaseerd toegangsbeheer met Azure PowerShell beheren

-   [Rolgebaseerd toegangsbeheer met de REST API beheren](../active-directory/role-based-access-control-manage-access-rest.md)

    In dit artikel leest hoe u met de REST API RBAC beheren.

-   [Rolgebaseerd toegangsbeheer voor Microsoft Azure uit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dit is een koppeling naar een video op kanaal 9 uit de 2015 MS Ignite vergadering. In deze sessie, ze computers nader bekeken management en de rapportagemogelijkheden in Azure openen en te verkennen aanbevelingen rond het beveiligen van toegang tot Azure abonnementen die gebruikmaken van Azure Active Directory.

-   [Autoriseer toegang tot webtoepassingen met OAuth 2.0 en Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)

    In dit artikel worden voltooid OAuth 2.0-mailstroom voor het verifiëren met Azure Active Directory.

-   [belangrijke kluis Management REST API 's](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Dit document is de verwijzing voor de REST API's voor het beheren van uw belangrijkste kluis via programmacode, inclusief het instellen van belangrijke kluis-beleid.

-   [belangrijke kluis REST API 's](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Koppeling naar belangrijkste kluis REST API-documentatie.

-   [Belangrijke toegangsbeheer](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Koppeling naar documentatie bij toets access control verwijzing.

-   [Geheim toegangsbeheer](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Koppeling naar documentatie bij toets access control verwijzing.

-   [Instellen](https://msdn.microsoft.com/library/mt603625.aspx) en [Verwijder](https://msdn.microsoft.com/library/mt619427.aspx) belangrijke kluis-beleid via PowerShell

    Koppelingen naar documentatie voor PowerShell-cmdlets voor het beheren van belangrijke kluis-beleid.

## <a name="next-steps"></a>Volgende stappen

Zie [Aan de slag met Azure belangrijke kluis](key-vault-get-started.md)voor een ophalen gestart zelfstudie voor een beheerder.

Zie [Azure belangrijke kluis logboekregistratie](key-vault-logging.md)voor meer informatie over het gebruik van logboekregistratie voor belangrijke kluis.

Zie voor meer informatie over het gebruik van toetsen en geheimen met Azure belangrijke kluis [over sleutels en geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Als u vragen over belangrijke kluis hebt, bezoekt u de [Azure belangrijke kluis Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
