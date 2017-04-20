<properties
    pageTitle="Het installeren en configureren van Azure PowerShell"
    description="Informatie over het installeren en configureren van Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Het installeren en configureren van Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Wat is Azure PowerShell?
Azure PowerShell is een verzameling modules waarmee cmdlets uit om het beheren van Azure met Windows PowerShell. U kunt de cmdlets gebruiken voor het maken, te testen, te implementeren en beheren van oplossingen en services die via het Azure platform is bezorgd. In de meeste gevallen kan de cmdlets worden gebruikt voor de taken die dezelfde als de Azure-Portal, zoals maken en configureren van cloudservices, virtuele machines virtuele netwerken en Webapps.

## <a name="how-versioning-works"></a>Hoe versiebeheer werkt

Azure PowerShell gebruikt Semantic versiebeheer, wat dat als betekent versie A > B-versie en klik vervolgens versie A heeft de meest recente API's. Ook, betekent dit dat er wijzigingen in het primaire versie gemiddelde een recente in een of meer cmdlets wijzigen.  Zo is, bijvoorbeeld versie 1.7.0 is om een recente wijziging in de 1.x-versie van Azure PowerShell.

Zie voor meer informatie over de procedures Semantic versiebeheer in Azure PowerShell, de semantische versiebeheer specificatie op: http://semver.org
 
Als u de meest recente API's, moet u versie 2.x. Maar als u geschreven scripts hebt ten opzichte van versie 1.x en u niet wilt absorbeert de recente wijzigingen in versie 2.x die worden beschreven in de 2.x [Releaseopmerkingen](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), klik u 1.7.0 moet installeren.

De versie niet overeenkomt kan resulteren als de nieuwste versie van de module profiel is geïnstalleerd en een eerdere versie van een module die afhankelijk van deze is vervolgens is geladen. De eenvoudigste manier om op te lossen dit is om te installeren vanuit de meest recente msi. De MSI worden automatisch oudere versies van de modules.
 
###<a name="installing-module-versions-side-by-side"></a>Installatie van versies naast elkaar module

2.1.0 (en versie 1.2.6 voor AzureStack) zijn van de eerste moduleversies zijn geïnstalleerd en naast elkaar gebruikt. Omdat Azure PowerShell binaire modules gebruikt, moet u een nieuw PowerShell-venster openen en gebruiken van **Import-Module** voor het importeren van een specifieke versie van de cmdlets AzureRM:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Versies voordat 2.1.0 (behalve 1.2.6) niet werken goed naast elkaar met andere versies van Azure PowerShell-module. Bij het laden van een eerdere versie van het gebruik van een opdracht zoals de bovenstaande Azure PowerShell-modules, niet-compatibele versies van de module **AzureRM.Profile** wordt geladen, waardoor de cmdlets gevraagd u aan te melden wanneer u een cmdlet, hebt uitgevoerd, zelfs nadat u zich hebt aangemeld.

De eenvoudigste manier om op te lossen dat dit is de meest recente Azure PowerShell installeren vanaf de WebPI feed of MSI – Hiermee eerdere versies van de modules geïnstalleerd in de galerie. 

Houd er rekening mee dat zowel Azure als AzureRM modules afhankelijkheden elkaar gemeen hebben, dus als u beide modules, gebruikt bij het bijwerken van een, moet u beide bijwerken. Eerdere versies van de Azure module hebben hetzelfde probleem met elkaar-module voor het laden van eerdere versies van de module AzureRM hebben.

<a id="Install"></a>
## <a name="step-1-install"></a>Stap 1: installeren

Hier volgen de twee manieren waarop u Azure PowerShell kunt installeren. U kunt deze van de PowerShell-galerie of de WebPI installeren.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Azure PowerShell installeren vanuit de galerie met PowerShell

De beste manier is PowerShell galerie te gebruiken. Moet u de module PowerShellGet via de PowerShell-galerie. Dit is hier beschikbaar: [PowerShellGallery.com](https://www.powershellgallery.com/)

Azure PowerShell 1.3.0 installeren of hoger in de PowerShell-galerie met een verhoogde bevoegdheid om Windows PowerShell of filter geïntegreerde uitvoeren van scripts-omgeving (wissen) met de volgende opdrachten:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Meer informatie over deze opdrachten

- **Installatie-Module AzureRM** installeert een rollup-module voor de resourcemanager Azure-cmdlets. Afhankelijk van de module AzureRM 
- het bereik van een bepaalde versie voor elke resourcemanager Azure-module. Het versiebereik opgenomen zorgt ervoor dat er geen wijzigingen van de module recente opgenomen worden kunnen tijdens de installatie van AzureRM modules met dezelfde primaire versie. Wanneer u de module AzureRM installeert, wordt elke module Azure resourcemanager die eerder niet is geïnstalleerd worden gedownload en geïnstalleerd in de galerie PowerShell. Zie voor meer informatie over de semantische versiebeheer die worden gebruikt door Azure PowerShell modules, [semver.org](http://semver.org). 
- **Installatie-Module Azure** -installaties de Azure-module. Deze module is de module Servicebeheer van Azure PowerShell 0.9.x. Dit heeft geen grote wijzigingen en verwisselbaar voor de vorige versie van de Azure-module.

###<a name="installing-azure-powershell-from-webpi"></a>Installatie van Azure PowerShell vanaf WebPI

Installatie van Azure PowerShell 1,0 en groter vanaf WebPI is dezelfde zoals deze voor 0.9.x was. [Azure PowerShell](http://aka.ms/webpi-azps) downloaden en de installatie te starten. Als er Azure PowerShell 0.9.x is geïnstalleerd, versie 0.9.x als onderdeel van de upgrade wordt verwijderd. Als u vanuit de galerie PowerShell Azure PowerShell modules hebt geïnstalleerd, wordt de modules voordat installatie tot een consistente Azure PowerShell-omgeving automatisch verwijderd door het installatieprogramma.

> [AZURE.NOTE] Als u eerder Azure modules vanuit de galerie met PowerShell hebt geïnstalleerd, kan het installatieprogramma van deze worden automatisch verwijderd. Hiermee voorkomt u verwarring over welke moduleversies u hebt geïnstalleerd en waar ze zich bevinden. Galerie met PowerShell modules installeert normaal in **%ProgramFiles%\WindowsPowerShell\Modules**. Daarentegen de Azure modules in *kan worden geïnstalleerd door het installatieprogramma van WebPI *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Als u een fout is opgetreden tijdens de installatie kunt u handmatig de Azure verwijderen* mappen in uw map **%ProgramFiles%\WindowsPowerShell\Modules** en probeer de installatie opnieuw.

Zodra de installatie is voltooid, uw ```$env:PSModulePath``` instelling van de lijsten met de Azure PowerShell-cmdlets moet bevatten.

> [AZURE.NOTE] Er is een bekend probleem met PowerShell **$env: PSModulePath** die kunnen optreden tijdens de installatie van WebPI. Als uw computer opnieuw worden gestart vanwege systeemupdates of andere installaties moet, kan dit leiden tot updates **$env: PSModulePath** niet opnemen het pad waar Azure PowerShell is geïnstalleerd. Als dit gebeurt, ziet u mogelijk een 'cmdlet niet herkend'-bericht tijdens een poging om Azure PowerShell-cmdlets gebruiken na de installatie of upgrade. Start het systeem moet als dit gebeurt, het probleem opgelost.

Als u een als volgt te werk bericht wanneer u probeert te laden of cmdlets uitvoeren:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Dit kan worden gecorrigeerd door opnieuw starten van de computer of de cmdlets van C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ importeren als volgt te werk (waarbij XXXX staat voor de versie van PowerShell is geïnstalleerd:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Stap 2: starten
U kunt de cmdlets uitvoeren vanuit het standaard Windows PowerShell-console of vanuit filter geïntegreerde uitvoeren van scripts-omgeving (wissen).
De methode die u gebruikt een van beide console openen, is afhankelijk van de versie van Windows die u uitvoert:

- Op een computer met ten minste Windows 8 of Windows Server 2012, kunt u de ingebouwde zoekfunctie. Beginnen met typen power vanuit **het startscherm** . Hiermee wordt een bereik lijst met apps met Windows PowerShell. Als de console wilt openen, klikt u op een van beide app. (Als u wilt de app vastmaken aan **het startscherm** , met de rechtermuisknop op het pictogram.)

- Gebruik het **menu Start**op een computer met een versie eerder dan Windows 8 of Windows Server 2012. In het menu **Start** , **Alle**programma's, Bureau- **accessoires**, klik op de **Windows PowerShell** -map en klik vervolgens op **Windows PowerShell**.

U kunt ook de **Windows PowerShell wissen** om het menu-items en sneltoetsen gebruiken om te kunnen ook veel van de taken die u in de Windows PowerShell-console uitvoeren wilt uitgevoerd uitvoeren. Als u wilt gebruiken het wissen, in de Windows PowerShell-console Cmd.exe of in het vak **uitvoeren** , typ, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Opdrachten om u te helpen aan de slag

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Stap 3: verbinding maken
De cmdlets nodig uw abonnement hebt, zodat ze uw services kunnen beheren. Als u er nog geen hebt, kunt u een Azure-abonnement kopen. Zie voor instructies voor het [kopen van Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Type **Login-AzureRmAccount**

2. Typ het e-mailadres en wachtwoord in voor uw account. Azure wordt geverifieerd door de referentiegegevens opslaat en sluit het venster.

--OF--

Meld u aan bij uw werk- of schoolaccount:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Als u meer dan één tenant dat is gekoppeld aan uw organisatie-account hebt, moet u de parameter TenantId opgeven:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Deze niet-interactieve aanmeldingsgegevens methode werkt alleen met een werk- of schoolaccount. Een werk- of schoolaccount is een gebruiker dat wordt beheerd door uw werk of school en gedefinieerd in het exemplaar van de Azure Active Directory voor uw werk of school. Als u niet een werk- of schoolaccount-account hoeft en een Microsoft-account aanmelden bij uw Azure-abonnement gebruikt, kunt u eenvoudig een met de volgende stappen maken.

> 1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com), en klik op **Active Directory**.

> 2. Als er geen map bestaat, selecteert u **uw adreslijst maken** en geef de gevraagde gegevens.

> 3. Selecteer de map en een nieuwe gebruiker toevoegen. Deze nieuwe gebruiker kunt aanmelden met een werk- of schoolaccount. Tijdens het maken van de gebruiker, wordt u hebt verstrekt met zowel een e-mailadres voor de gebruiker en een tijdelijk wachtwoord. Sla deze informatie, zoals deze wordt gebruikt in stap 5 hieronder.

> 4. Selecteer **Instellingen** en selecteer **beheerders**van de Azure klassieke portal. Selecteer **toevoegen**en de nieuwe gebruiker toevoegen als de beheerder van een collega. Hiermee kunt het account voor werk- of schoolaccount voor het beheren van uw Azure-abonnement.

> 5. Tot slot zich afmeldt bij de Azure klassieke portal en meld u weer aan het gebruik van het werk of schoolaccount. Als dit de eerste keer aanmelden met dit account, wordt u gevraagd het wachtwoord wijzigen.

> Voor meer informatie over zich registreert voor Microsoft Azure met een account voor werk of school, raadpleegt u [zich aanmeldt voor Microsoft Azure als een organisatie](./active-directory/sign-up-organization.md).

> Zie voor meer informatie over verificatie en abonnementen management in Azure wordt aangegeven, [Accounts beheren, abonnementen, en beheerdersrollen](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Het account en abonnement details weergeven

U kunt meerdere accounts en abonnementen beschikbaar voor gebruik door Azure PowerShell hebben. U kunt meerdere accounts toevoegen door meer dan één keer **Toevoegen-AzureRmAccount** uit te voeren.

Als u de beschikbare Azure rekeningen, typt u **Get-AzureAccount**.

Als u uw Azure-abonnementen, typt u **Get-AzureRmSubscription**.

##<a id="Help"></a>Help opvragen##

Deze resources bieden hulp voor specifieke cmdlets:


-   Vanuit de console kunt u het ingebouwde Help-systeem. De cmdlet **Get-Help** biedt toegang tot dit systeem. 

- Probeer deze populaire forums voor hulp bij het van de community:

 - [Azure op MSDN-forum]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Meer informatie


Raadpleeg de volgende bronnen voor meer informatie over het gebruik van de cmdlets:

Zie [Windows PowerShell gebruiken](http://go.microsoft.com/fwlink/p/?LinkId=321939)voor eenvoudige instructies over het gebruik van Windows PowerShell.

Zie [Azure Cmdlet-naslaginformatie](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx)voor naslaginformatie over de cmdlets.

Zie voor voorbeeldscripts en instructies voor het uitvoeren van scripts gebruiken voor het beheren van Azure Leer hoe u het [Script Center](http://go.microsoft.com/fwlink/p/?LinkId=321940).

