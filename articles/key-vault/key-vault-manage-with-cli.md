<properties
    pageTitle="Toets kluis met CLI beheren | Microsoft Azure"
    description="Deze zelfstudie algemene taken in sleutel kluis automatiseren met behulp van de CLI gebruiken"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Toets kluis met CLI beheren #
Azure-toets kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding  
Met deze zelfstudie kunt u aan de slag met Azure-toets kluis maken van een harde container (een kluis) in Azure wordt aangegeven, opslaan en beheren van cryptografische sleutels en geheimen in Azure wordt aangegeven. Deze begeleidt u bij het proces van het gebruik van Azure van een Interface voor de opdrachtregel platforms voor het maken van een kluis met een toets of het wachtwoord dat u vervolgens met een Azure-toepassing gebruiken kunt. Deze vervolgens ziet u hoe een toepassing kunt gebruikt u deze toets of wachtwoord.

**Geschatte tijd in beslag:** 20 minuten

>[AZURE.NOTE]  Deze zelfstudie is niet inbegrepen voor instructies over het schrijven van de Azure toepassing dat een van de volgende stappen bevat, die ziet u hoe u een toepassing een toets of geheim gebruiken in de belangrijkste kluis Autoriseer.
>
>U configureren niet op dit moment Azure-toets kluis in de portal van Azure. Gebruik in plaats daarvan deze instructies platforms opdrachtregel-Interface. Of Azure voor instructies over PowerShell, raadpleegt u [deze gelijkwaardige zelfstudie](key-vault-get-started.md).

Zie voor informatie over Azure-toets kluis [Wat Azure-toets kluis is?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten voor
Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende:

- Een abonnement op Microsoft Azure. Als u niet een hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial).
- Opdrachtregel versie 0.9.1 of hoger. Als u wilt de nieuwste versie installeren en verbinden met uw Azure-abonnement, Zie [installeren en configureren van de opdrachtregel Azure-platforms](../xplat-cli-install.md).
- Een toepassing die wordt geconfigureerd voor het gebruik van de toets of het wachtwoord die u in deze zelfstudie maakt. Een toepassing voor de steekproef is beschikbaar via het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=45343). Zie voor instructies voor het bijbehorende Leesmij-bestand.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Met Azure platforms opdrachtregel-Interface-informatie opvragen

Deze zelfstudie wordt ervan uitgegaan dat u bekend bent met een interface voor de opdrachtregel (Terminal, opdrachtprompt we vaker doen)

De--help of -h parameter kan worden gebruikt om hulp voor specifieke opdrachten weergeven. U kunt ook de azure help [opdracht], [opties] indeling kan ook worden gebruikt om terug te keren dezelfde gegevens. De volgende opdrachten alle retourneren bijvoorbeeld dezelfde gegevens:

    azure account set --help

    azure account set -h

    azure help account set

Raadpleeg bij twijfel over de parameters die nodig zijn voor een opdracht de help gebruiken--help, -h of azure help [opdracht].

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure resourcemanager in Azure platforms opdrachtregel lezen:

- [Het installeren en configureren van Azure platforms Command Line Interface](../xplat-cli-install.md)
- [Azure platforms opdrachtregel-Interface met Azure Resource Manager gebruiken](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen

Gebruik de volgende opdracht uit te melden met een organisatieaccount:

    azure login -u username -p password

of als u zich wilt aanmelden door te typen interactief

    azure login

>[AZURE.NOTE]  De methode login werkt alleen met organisatieaccount. Een organisatieaccount is een gebruiker dat wordt beheerd door uw organisatie en gedefinieerd in de Azure Active Directory-tenant van uw organisatie.


Als u niet een organisatie-account hoeft en een Microsoft-account aanmelden bij uw Azure-abonnement gebruikt, kunt u eenvoudig een met de volgende stappen maken.

1.  Meld u aan de aanmelding aan het [Azure Management Portal](https://manage.windowsazure.com/)en klik op Active Directory.
2.  Als er geen map bestaat, schakelt u het telefoonboek van uw maken en geef de gevraagde gegevens.
3.  Selecteer de map en een nieuwe gebruiker toevoegen. Deze nieuwe gebruiker is een organisatie-account. Tijdens het maken van de gebruiker, wordt u hebt verstrekt met zowel een e-mailadres voor de gebruiker en een tijdelijk wachtwoord. Bewaar deze gegevens zoals deze wordt gebruikt in een andere stap.
4.  Selecteer instellingen en selecteer vervolgens beheerders in de portal. Selecteer toevoegen en de nieuwe gebruiker toevoegen als de beheerder van een collega. Hierdoor wordt de organisatie-account om uw Azure abonnement te beheren.
5.  Ten slotte zich afmeldt bij de Azure-portal en meld u weer aan met de nieuwe organisatie-account. Als dit de eerste keer aanmelden met dit account, wordt u gevraagd het wachtwoord wijzigen.

Zie voor meer informatie over het gebruik van een organisatieaccount met Microsoft Azure, [registreren voor Microsoft Azure als een organisatie](../active-directory/sign-up-organization.md).

Als u meerdere abonnementen hebt en geeft u aan een specifieke een voor Azure-toets kluis wilt gebruiken, typt u de volgende handelingen uit als u wilt zien van de abonnementen voor uw account:

    azure account list

Als u wilt opgeven van het abonnement te gebruiken, typ:

    azure account set <subscription name>

Zie [hoe u de installatie en Azure platforms opdrachtregel-Interface configureren](../xplat-cli-install.md)voor meer informatie over het configureren van Azure van een Interface voor de opdrachtregel platforms.


## <a name="switch-to-using-azure-resource-manager"></a>Overschakelen naar resourcemanager van Azure

De toets kluis Azure Resource Manager is vereist, dus typt u de volgende handelingen uit om te schakelen naar Azure resourcemanager modus:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken

Wanneer u met Azure resourcemanager, worden alle gerelateerde resources worden gemaakt in een resourcegroep. We gaan een nieuwe resourcegroep 'ContosoResourceGroup' maken voor deze zelfstudie.

    azure group create 'ContosoResourceGroup' 'East Asia'

De eerste parameter is de naam van de resource-groep en de tweede parameter is de locatie. Gebruik de opdracht voor locatie, `azure location list` om te bepalen hoe een alternatieve locatie met de sjabloon in dit voorbeeld opgeven. Als u meer informatie nodig hebt, typt u het:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>De provider van de resource toets kluis registreren
Controleer of dat die toets kluis resource-provider is geregistreerd in uw abonnement:

`azure provider register Microsoft.KeyVault`

Dit moet slechts één keer per abonnement worden uitgevoerd.


## <a name="create-a-key-vault"></a>Een belangrijke kluis maken

Gebruik de `azure keyvault create` opdracht om te maken van een belangrijke kluis. Dit script heeft drie verplichte parameters: de naam van een resource-groep, de naam van een belangrijke kluis en de geografische locatie.

Als u de naam kluis van ContosoKeyVault, de naam van de resource van ContosoResourceGroup en de locatie van Oost-Azië gebruikt, typ bijvoorbeeld:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

De uitvoer van deze opdracht ziet u de eigenschappen van de belangrijkste kluis die u zojuist hebt gemaakt. De twee belangrijkste eigenschappen zijn:

- **Naam**: In het voorbeeld is dit ContosoKeyVault. Gebruikt u deze naam voor andere cmdlets toets kluis.
- **vaultUri**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net. Toepassingen die gebruikmaken van uw kluis tot en met de REST API moeten deze URI gebruiken.

Uw Azure-account mag zich nu alle bewerkingen uitvoeren op deze toets kluis. Nog is niemand anders.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Een toets of geheim toevoegen aan de belangrijkste kluis

Als u Azure-toets kluis een sleutel software-beveiliging voor u te maken wilt, gebruikt u de `azure key create` opdracht en typ de volgende handelingen uit:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Echter als u een bestaande sleutel in een .pem-bestand dat is opgeslagen als een lokaal bestand in een bestand met de naam softkey.pem die u wilt uploaden naar Azure-toets kluis hebt, typt u de volgende handelingen uit als u wilt importeren de sleutel uit de. PEM-bestand, dat de toets met software in de sleutel kluis-service beschermen kan worden gebruikt:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

U kunt nu naar de sleutel die u hebt gemaakt of geüpload naar Azure-toets kluis, met behulp van de URI verwijzen. Gebruik van **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** voor altijd de huidige versie en het gebruik van **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** voor deze specifieke versie.

Als u wilt een geheim toevoegen aan de kluis, die een wachtwoord met de naam SQLPassword en die de waarde van Pa$ $w0rd naar Azure-toets kluis, typt u de volgende handelingen uit:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Nu kunt u dit wachtwoord die u hebt toegevoegd aan Azure-toets kluis, met behulp van de URI raadplegen. Gebruik van **https://ContosoVault.vault.azure.net/secrets/SQLPassword** voor altijd de huidige versie en het gebruik van **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** voor deze specifieke versie.

Laten we weergeven de sleutel of geheim dat u zojuist hebt gemaakt:

- Als u wilt weergeven op uw sleutel, typ:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Uw geheime, type weergeven:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Een toepassing registreren met Azure Active Directory

Deze stap zou meestal worden uitgevoerd door een ontwikkelaar, op een andere computer. Het is niet specifiek zijn voor Azure-toets kluis maar dat is opgenomen, volledigheid.


>[AZURE.IMPORTANT] U voltooit de zelfstudie, uw account, de kluis en de toepassing die u in deze stap wordt geregistreerd moeten zijn in dezelfde map Azure.

Toepassingen die gebruikmaken van een belangrijke kluis moeten worden geverifieerd met behulp van een token van Azure Active Directory. Hiervoor moet de eigenaar van de toepassing eerst de toepassing registreren in hun Azure Active Directory. De eigenaar van de toepassing wordt aan het einde van de registratie, de volgende waarden:


- Een **Toepassings-ID** (ook wel bekend als een Client-ID) en de **verificatie-toets** (ook wel bekend als het gedeelde geheim). De toepassing moet van deze waarden met Azure Active Directory, om een token presenteren. Hoe de toepassing hiervoor is geconfigureerd, is afhankelijk van de toepassing. In het bestand app.config stelt u in de eigenaar van de toepassing deze waarden voor de toepassing van de steekproef toets kluis.



De toepassing registreren in Azure Active Directory:

1. Meld u aan bij de portal van Azure.
2. Aan de linkerkant op **Active Directory**en selecteer vervolgens de map waarin u de toepassing wordt geregistreerd. <br> <br> Opmerking: U moet de dezelfde map waarin het Azure abonnement waarmee u uw sleutel kluis gemaakt selecteren. Als u niet welke directory dit weet is, klikt u op **Instellingen**en noteer de naam van de map weergegeven in de laatste kolom identificeren van het abonnement waarmee u uw sleutel kluis hebt gemaakt.

3. Klik op **toepassingen**. Als u geen apps zijn toegevoegd aan uw adreslijst, wordt alleen de koppeling **een App toevoegen** door deze pagina weergegeven. Klik op de koppeling of u kunt ook klikken op de **toevoegen** op de balk met opdrachten.
4.  Klik in de wizard **Toepassing toevoegen** op de **Wat wilt u doen?** pagina, klikt u op **een toepassing het ontwikkelen van mijn organisatie toevoegen**.
5.  Klik op de pagina **Vertel ons over het gebruik van de toepassing** Geef een naam voor uw toepassing en selecteer **WEB APPLICATION en/of WEB API** (de standaardinstelling). Klik op het pictogram van de volgende.
6.  Geef op de pagina **Eigenschappen van de App** de **Aanmelding op URL** en de **APP-ID-URI** voor uw webtoepassing. Als uw toepassing geen deze waarden bevat, kunt u ze voor deze stap (bijvoorbeeld: u kunt opgeven http://test1.contoso.com voor beide selectievakjes). Het maakt niet uit als deze sites bestaat; Wat is het belangrijk is dat de app-ID-URI voor elke toepassing verschillende voor elke toepassing in uw adreslijst. De map gebruikt deze tekenreeks aan te geven van uw app.
7.  Klik op het volledige pictogram om uw wijzigingen opslaan in de wizard.
8.  Klik op de pagina snel starten op **configureren**.
9.  Ga naar het gedeelte **toetsen** , selecteert u de duur en klik vervolgens op **Opslaan**. De pagina vernieuwt en de waarde van een sleutel wordt nu weergegeven. Met deze sleutelwaarde en de waarde van de **CLIENT-ID** , moet u uw toepassing configureren. (Instructies voor deze configuratie worden toepassingsspecifieke.)
10. Kopieer de waarde van client-ID van deze pagina, die u in de volgende stap gebruiken wilt voor machtigingen instellen voor uw kluis.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autoriseer de toepassing op de toets of geheim gebruiken

Als u akkoord gaat de toepassing toegang tot de toets of geheim in de kluis, gebruikt u de `azure keyvault set-policy` opdracht.

Bijvoorbeeld als de naam van uw kluis ContosoKeyVault is en de toepassing die u wilt machtigen een client-ID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u machtigen van de toepassing wilt u ontsleutelt en meld u aan met de toetsen in uw kluis, voer het volgende:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Als u op de opdrachtprompt van Windows wordt uitgevoerd, moet u enkele aanhalingstekens vervangen door de dubbele aanhalingstekens is geplaatst en ook druk op ESC om de interne dubbele aanhalingstekens is geplaatst. Bijvoorbeeld: "[\"ontsleutelen\",\"Aanmeldingsadres\"]".

Als u machtigen die dezelfde toepassing wilt te lezen geheimen in uw kluis, voert u het volgende:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u wilt gebruiken van een waardepapier HSM (hardwarebeveiligingsmodule) ##

U kunt importeren of toetsen in hardware beveiligingsmodules (HSM's) die nooit de grens HSM laat genereren voor toegevoegde assurance. De HSM's zijn FIPS 140-2 niveau 2 gevalideerd. Als deze vereiste niet voor u geldt, in dit gedeelte overslaan en Ga naar [de belangrijkste kluis verwijderen en bijbehorende toetsen en geheimen](#delete-the-key-vault-and-associated-keys-and-secrets).

Als u wilt maken van deze toetsen HSM is beveiligd, moet u een abonnement kluis die ondersteuning biedt voor HSM beveiligde sleutels hebben.

Als u de keyvault hebt gemaakt, voegt u de parameter 'sku':

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

U kunt software-beveiliging (zoals eerder) en HSM beveiligde sleutels toevoegen aan deze kluis. Als u wilt maken van een sleutel HSM is beveiligd, stelt u de parameter bestemming naar 'HSM':

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

U kunt de volgende opdracht uit een sleutel importeren uit een bestand .pem op uw computer. Deze opdracht importeert de toets in HSM's in de service-toets kluis:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

De volgende opdracht wordt geïmporteerd een 'doen om uw eigen code"(BYOK)-pakket. Hiermee kunt u uw sleutel in uw lokale HSM genereren en deze overbrengen naar HSM's in de service-toets kluis, zonder de sleutel de grens HSM verlaten:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Voor meer gedetailleerde informatie over hoe u dit pakket BYOK genereren, Zie [hoe u met de pijltoetsen HSM-Protected met Azure toets kluis](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>De belangrijkste kluis en de bijbehorende sleutels en de geheimen verwijderen

Als u de belangrijkste kluis en de toets of geheim die deze bevat niet meer nodig hebt, kunt u de belangrijkste kluis verwijderen met behulp van de opdracht azure keyvault verwijderen:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Of, kunt u een hele Azure resource-groep, die bestaat uit de belangrijkste kluis en andere bronnen die u hebt opgenomen in die groep verwijderen:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Overige Commands Azure platforms opdrachtregel-Interface

Overige commands dat u mogelijk handig voor het beheren van Azure-toets kluis.

Deze opdracht bevat een tabelvormige weergave van alle sleutels en geselecteerde eigenschappen:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Deze opdracht geeft u een volledige lijst met eigenschappen voor de opgegeven sleutel:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Deze opdracht bevat een tabelvormige weergave van alle geheime namen en geselecteerde eigenschappen:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Hier volgt een voorbeeld van hoe u een specifieke sleutel verwijdert:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Hier volgt een voorbeeld van het verwijderen van een specifieke geheim:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Volgende stappen

Zie [de Azure toets kluis handleiding voor ontwikkelaars](key-vault-developers-guide.md)voor programming verwijzingen.
