<properties
    pageTitle="Aan de slag met Azure-toets kluis | Microsoft Azure"
    description="Met deze zelfstudie kunt u aan de slag met Azure-toets kluis maken van een harde container in Azure wordt aangegeven, opslaan en beheren van cryptografische sleutels en geheimen in Azure wordt aangegeven."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Aan de slag met Azure-toets kluis #
Azure-toets kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding  
Met deze zelfstudie kunt u aan de slag met Azure-toets kluis maken van een harde container (een kluis) in Azure wordt aangegeven, opslaan en beheren van cryptografische sleutels en geheimen in Azure wordt aangegeven. Deze begeleidt u bij het proces van het gebruik van Azure PowerShell voor het maken van een kluis met een toets of het wachtwoord dat u vervolgens met een Azure-toepassing gebruiken kunt. Deze vervolgens ziet u hoe een toepassing die toets of wachtwoord kunt gebruiken.

**Geschatte tijd in beslag:** 20 minuten

>[AZURE.NOTE]  Deze zelfstudie is niet inbegrepen voor instructies voor het schrijven van de Azure toepassing dat een van de volgende stappen bevat, namelijk hoe u een toepassing een toets of geheim gebruiken in de belangrijkste kluis Autoriseer.
>
>In deze zelfstudie wordt Azure PowerShell. Zie [deze gelijkwaardige zelfstudie](key-vault-manage-with-cli.md)voor instructies voor de opdrachtregel platforms.

Zie voor informatie over Azure-toets kluis [Wat Azure-toets kluis is?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende:

- Een abonnement op Microsoft Azure. Als u niet een hebt, kunt u zich kunt aanmelden voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).
- Azure PowerShell, **minimale versie van 1.1.0**. Azure PowerShell installeren en deze koppelen aan uw Azure-abonnement: Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Als u Azure PowerShell al hebt geïnstalleerd en de versie van de Azure PowerShell-console niet weet, typt u `(Get-Module azure -ListAvailable).Version`. Als er Azure PowerShell versie 0.9.1 tot en met 0.9.8 is geïnstalleerd, kunt u deze zelfstudie met kleine wijzigingen. Bijvoorbeeld, moet u de `Switch-AzureMode AzureResourceManager` opdrachten en parameters enkele van de Azure-toets kluis-opdrachten zijn gewijzigd. Zie voor een lijst met de toets kluis cmdlets voor versies 0.9.1 tot en met 0.9.8, [Azure toets kluis Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Een toepassing die wordt geconfigureerd voor het gebruik van de toets of het wachtwoord die u in deze zelfstudie maakt. Een toepassing voor de steekproef is beschikbaar via het [Microsoft Downloadcentrum](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Zie voor instructies voor het bijbehorende Leesmij-bestand.


Deze zelfstudie is bedoeld voor beginners Azure PowerShell, maar deze wordt ervan uitgegaan dat u de basisbegrippen, zoals modules, cmdlets en sessies kennen. Zie [aan de slag met Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)voor meer informatie.

Als u gedetailleerde help voor elke-cmdlet waarmee u in deze zelfstudie te zien, gebruikt u de cmdlet **Get-Help** .

    Get-Help <cmdlet-name> -Detailed

Typ bijvoorbeeld het volgende als u help voor de cmdlet **Login-AzureRmAccount** :

    Get-Help Login-AzureRmAccount -Detailed

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure resourcemanager in Azure PowerShell lezen:

- [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)
- [Via Azure PowerShell met bronbeheer](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Verbinding maken met uw abonnementen ##

Een Azure PowerShell-sessie starten en meld u aan bij uw Azure-account met de volgende opdracht uit:  

    Login-AzureRmAccount 

Houd er rekening mee dat als u gebruikmaakt van een specifiek exemplaar van Azure, bijvoorbeeld Azure overheid, gebruik de omgeving parameter - met deze opdracht. Bijvoorbeeld:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Voer uw gebruikersnaam in te voeren Azure-account en wachtwoord in het pop-browservenster. Azure PowerShell krijgt alle abonnementen die zijn gekoppeld aan dit account en al dan niet standaard, gebruikt de eerste fase.

Als u meerdere abonnementen hebt en geeft u aan een specifieke een voor Azure-toets kluis wilt gebruiken, typt u de volgende handelingen uit als u wilt zien van de abonnementen voor uw account:

    Get-AzureRmSubscription

Als u wilt opgeven van het abonnement te gebruiken, typ:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Zie voor meer informatie over het configureren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a id="resource"></a>Een nieuwe resourcegroep maken ##

Wanneer u resourcemanager Azure gebruikt, worden alle gerelateerde resources worden gemaakt in een resourcegroep. Maak een nieuwe resourcegroep benoemde **ContosoResourceGroup** voor deze zelfstudie wordt:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Een belangrijke kluis maken ##

Gebruik de cmdlet [New-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) om het maken van een belangrijke kluis. Deze cmdlet heeft drie verplichte parameters: een **groep resourcenaam**, een **belangrijke kluis naam**en de **geografische locatie**.

Als u de naam kluis van **ContosoKeyVault**, de naam van de resource van **ContosoResourceGroup**en de locatie van **Oost-Azië**gebruikt, typ bijvoorbeeld:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

De uitvoer van deze cmdlet ziet u de eigenschappen van de belangrijkste kluis die u zojuist hebt gemaakt. De twee belangrijkste eigenschappen zijn:

- **De naam van de kluis**: In het voorbeeld is dit **ContosoKeyVault**. Gebruikt u deze naam voor andere cmdlets toets kluis.
- **Kluis URI**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net/. Toepassingen die gebruikmaken van uw kluis tot en met de REST API moeten deze URI gebruiken.

Uw Azure-account mag zich nu alle bewerkingen uitvoeren op deze toets kluis. Nog is niemand anders.

>[AZURE.NOTE]  Als u de fout **het abonnement dat niet is geregistreerd als u wilt gebruiken naamruimte 'Microsoft.KeyVault'** wordt weergegeven wanneer u probeert te maken van uw nieuwe belangrijke kluis, uitvoeren `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` en voer de opdracht Nieuw AzureRmKeyVault. Zie de [Register-AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx)voor meer informatie.
>

## <a id="add"></a>Een toets of geheim toevoegen aan de belangrijkste kluis ##

Als u Azure-toets kluis wilt te maken van een sleutel software-beveiliging voor u, gebruik de cmdlet [Toevoegen-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) en typt u het volgende:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Echter als u een bestaande software is beveiligd sleutel hebt in een. PFX-bestand is opgeslagen op uw station C:\ in een bestand met de naam softkey.pfx die u wilt uploaden naar kluis van Azure-sleutel, typ de volgende handelingen uit om in te stellen van de variabele **securepfxpwd** om een wachtwoord van **123** voor de. PFX-bestand:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Typ het volgende als u wilt importeren de sleutel uit de. PFX-bestand, waarin de toets met software in de sleutel kluis-service beschermen kan worden gebruikt:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


U kunt nu naar deze sleutel die u hebt gemaakt of geüpload naar Azure-toets kluis, met behulp van de URI verwijzen. Gebruik van **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** voor altijd de huidige versie en het gebruik van **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** voor deze specifieke versie.  

Als u wilt weergeven de URI voor deze sleutel, typ:

    $Key.key.kid

Als u wilt een geheim toevoegen aan de kluis, die is een wachtwoord SQLPassword met de naam en de waarde van Pa$ $w0rd naar Azure-toets kluis, eerst de waarde van Pa$ $w0rd secure converteert een tekenreeks naar door te typen van de volgende handelingen uit:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Typ vervolgens het volgende:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Nu kunt u dit wachtwoord die u hebt toegevoegd aan Azure-toets kluis, met behulp van de URI raadplegen. Gebruik van **https://ContosoVault.vault.azure.net/secrets/SQLPassword** voor altijd de huidige versie en het gebruik van **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** voor deze specifieke versie.

Als u de URI voor deze geheim, typt u het:

    $secret.Id

Laten we weergeven de sleutel of geheim dat u zojuist hebt gemaakt:

- Als u wilt weergeven op uw sleutel, typ:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Uw geheime, type weergeven:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Uw belangrijkste kluis en code of geheim is nu klaar voor toepassingen gebruiken. Toepassingen u ze kunt gebruiken, moet u machtigen.  

## <a id="register"></a>Een toepassing registreren met Azure Active Directory ##

Deze stap zou meestal worden uitgevoerd door een ontwikkelaar, op een andere computer. Het is niet specifiek zijn voor Azure-toets kluis, maar is hier opgenomen voor volledigheid.


>[AZURE.IMPORTANT] U voltooit de zelfstudie, uw account, de kluis en de toepassing die u in deze stap wordt geregistreerd moeten zijn in dezelfde map Azure.

Toepassingen die gebruikmaken van een belangrijke kluis moeten worden geverifieerd met behulp van een token van Azure Active Directory. Hiervoor moet de eigenaar van de toepassing eerst de toepassing registreren in hun Azure Active Directory. De eigenaar van de toepassing wordt aan het einde van de registratie, de volgende waarden:


- Een **Toepassings-ID** (ook wel bekend als een Client-ID) en de **verificatie-toets** (ook wel bekend als het gedeelde geheim). De toepassing moet beide deze waarden met Azure Active Directory, om een token presenteren. Hoe de toepassing hiervoor is geconfigureerd, is afhankelijk van de toepassing. In het bestand app.config stelt u in de eigenaar van de toepassing deze waarden voor de toepassing van de steekproef toets kluis.

De toepassing registreren in Azure Active Directory:

1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant op **Active Directory**en selecteer vervolgens de map waarin u de toepassing wordt geregistreerd. <br> <br> **Notitie:** Moet u de dezelfde map waarin het Azure abonnement waarmee u uw sleutel kluis hebt gemaakt. Als u niet welke directory dit weet is, klikt u op **Instellingen**en noteer de naam van de map weergegeven in de laatste kolom identificeren van het abonnement waarmee u uw sleutel kluis hebt gemaakt.

3. Klik op **toepassingen**. Als u geen apps zijn toegevoegd aan uw adreslijst, ziet u deze pagina alleen de koppeling **een App toevoegen** . Klik op de koppeling of u kunt ook u kunt klikken op **toevoegen** op de balk met opdrachten.
4.  Klik in de wizard **Toepassing toevoegen** op de **Wat wilt u doen?** pagina, klikt u op **een toepassing het ontwikkelen van mijn organisatie toevoegen**.
5.  Geef een naam voor uw toepassing op de pagina **Vertel ons over het gebruik van de toepassing** en selecteer vervolgens **WEB APPLICATION en/of WEB API** (de standaardinstelling). Klik op het pictogram van de **volgende** .
6.  Geef op de pagina **Eigenschappen van de App** de **Aanmelding op URL** en de **APP-ID-URI** voor uw webtoepassing. Als uw toepassing geen deze waarden bevat, kunt u ze voor deze stap (bijvoorbeeld: u kunt opgeven http://test1.contoso.com voor beide selectievakjes). Het maakt niet uit als deze sites bestaat. Wat is het belangrijk is dat de app-ID-URI voor elke toepassing verschillende voor elke toepassing in uw adreslijst. De map gebruikt deze tekenreeks aan te geven van uw app.
7.  Klik op het pictogram **voltooid** als u wilt uw wijzigingen opslaan in de wizard.
8.  Klik op de pagina **Snel starten** op **configureren**.
9.  Ga naar het gedeelte **toetsen** , selecteert u de duur en klik vervolgens op **Opslaan**. De pagina vernieuwt en de waarde van een sleutel wordt nu weergegeven. Met deze sleutelwaarde en de waarde van de **CLIENT-ID** , moet u uw toepassing configureren. (Instructies voor deze configuratie zijn toepassingsspecifieke.)
10. Kopieer de waarde van client-ID van deze pagina, die u in de volgende stap gebruiken wilt voor machtigingen instellen voor uw kluis.

## <a id="authorize"></a>Autoriseer de toepassing op de toets of geheim gebruiken ##

Als u akkoord gaat de toepassing toegang tot de toets of geheim in de kluis, gebruikt u de cmdlet  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) .

Als de naam van uw kluis is **ContosoKeyVault** en de toepassing die u wilt machtigen een client-ID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u wilt machtigen van de toepassing u ontsleutelt en meld u aan met de toetsen in uw kluis, voert u er bijvoorbeeld het volgende:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Als u machtigen die dezelfde toepassing wilt te lezen geheimen in uw kluis, voert u het volgende:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Als u wilt gebruiken van een waardepapier HSM (hardwarebeveiligingsmodule) ##

U kunt importeren of toetsen in hardware beveiligingsmodules (HSM's) die nooit de grens HSM laat genereren voor toegevoegde assurance. De HSM's zijn FIPS 140-2 niveau 2 gevalideerd. Als deze vereiste niet voor u geldt, in dit gedeelte overslaan en Ga naar [de belangrijkste kluis verwijderen en bijbehorende toetsen en geheimen](#delete).

Als u wilt maken van deze toetsen HSM is beveiligd, moet u de [Azure toets kluis Premium servicelaag ter ondersteuning van toetsen HSM is beveiligd](https://azure.microsoft.com/pricing/free-trial/). Daarnaast Houd er rekening mee dat deze functionaliteit niet beschikbaar voor Azure China is.


Als u de belangrijkste kluis hebt gemaakt, voegt u de parameter **- SKU** :


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



U kunt software-beveiliging (zoals eerder) en HSM beveiligde sleutels toevoegen aan deze toets kluis. Als u een sleutel HSM is beveiligd, stelt u de **-bestemming** -parameter voor 'HSM':

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

U kunt de volgende opdracht uit om te importeren van een sleutel uit een. PFX-bestand op uw computer. Deze opdracht importeert de toets in HSM's in de service-toets kluis:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


De volgende opdracht wordt geïmporteerd een 'doen om uw eigen code"(BYOK)-pakket. Dit scenario kunt u uw sleutel in uw lokale HSM genereren en deze overbrengen naar HSM's in de service-toets kluis, zonder de sleutel de grens HSM verlaten:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Zie voor meer gedetailleerde informatie over het genereren van dit pakket BYOK kunt [het genereren en doorverbinden HSM beveiligde sleutels voor Azure toets kluis](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>De belangrijkste kluis en de bijbehorende sleutels en de geheimen verwijderen ##

Als u de belangrijkste kluis en de toets of geheim die deze bevat niet meer nodig hebt, kunt u de belangrijkste kluis verwijderen met behulp van de cmdlet [Verwijderen AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) :

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Of, kunt u een hele Azure resource-groep, die bestaat uit de belangrijkste kluis en andere bronnen die u hebt opgenomen in die groep verwijderen:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Andere Azure PowerShell-Cmdlets ##

Overige opdrachten die handig wellicht voor het beheren van Azure-toets kluis:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Deze opdracht krijgt een tabelvormige weergave van alle sleutels en geselecteerde eigenschappen.
- `$Keys[0]`: Deze opdracht geeft u een volledige lijst met eigenschappen voor de opgegeven sleutel
- `Get-AzureKeyVaultSecret`: Deze opdracht bevat een tabelvormige weergave van alle geheime namen en geselecteerde eigenschappen.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Voorbeeld hoe u een specifieke sleutel verwijderen.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Voorbeeld het verwijderen van een specifieke geheim.


## <a id="next"></a>Volgende stappen ##

Zie voor een opvolging zelfstudie die gebruikmaakt van Azure-toets kluis in een webtoepassing [Gebruik Azure toets kluis uit een webtoepassing](key-vault-use-from-web-application.md).

Als u wilt zien hoe uw belangrijkste kluis wordt gebruikt, Zie [Azure toets kluis vastleggen](key-vault-logging.md).

Zie voor een lijst met de meest recente Azure PowerShell-cmdlets voor Azure-toets kluis, [Azure toets kluis Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Zie [de Azure toets kluis handleiding voor ontwikkelaars](key-vault-developers-guide.md)voor programming verwijzingen.
