<properties
    pageTitle="Azure DevTest Labs Veelgestelde vragen over | Microsoft Azure"
    description="Hier vindt u antwoorden op veelgestelde vragen voor Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs Veelgestelde vragen

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Algemene
- [Wat gebeurt er als mijn vraag hier niet wordt beantwoord?](#what-if-my-question-isnt-answered-here)
- [Waarom moet ik Azure DevTest Labs gebruiken?](#why-should-i-use-azure-devtest-labs) 
- [Wat betekent 'probleemloos, selfservice'?](#what-does-quotworry-free-self-servicequot-mean)
- [Hoe kan ik Azure DevTest Labs gebruiken?](#how-can-i-use-azure-devtest-labs) 
- [Hoe ben ik gefactureerd voor Azure DevTest Labs?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Beveiliging 
- [Wat zijn de verschillende niveaus in Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Hoe maak ik een rol toe te staan dat gebruikers een bepaalde taak uitvoeren?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI/CD-integratie en automatisering 
 
- [Is Azure DevTest Labs geïntegreerd met mijn CI/CD-toolchain?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuele Machines 
 
- [Waarom zie ik bepaalde VMs in het blad Azure virtuele Machines die ik Zie binnen Azure DevTest Labs niet?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Wat is het verschil tussen aangepaste afbeeldingen en formules?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Hoe maak ik meerdere VMs met dezelfde sjabloon in één keer?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Hoe verplaats ik mijn bestaande Azure VMs in mijn testomgeving Azure DevTest Labs?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Kan ik meerdere schijven aan mijn VMs koppelen?](#can-i-attach-multiple-disks-to-my-vms) 
- [Hoe ik het proces van het uploaden van bestanden VHD als u wilt maken van aangepaste afbeeldingen automatiseren?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Hoe kan ik het proces van het verwijderen van alle VMs in mijn testomgeving automatiseren?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Onderdelen 
 
- [Wat zijn de onderdelen?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Configuratie van testomgeving 
 
- [Hoe maak ik een laboratorium van een sjabloon Azure resourcemanager?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Waarom zijn mijn VMs gemaakt in verschillende resourcegroepen met willekeurige namen? Kan ik de naam van wijzigen of deze resourcegroepen wijzigen?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Hoeveel labs kan ik maken onder hetzelfde abonnement?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Hoeveel VMs kan ik per testomgeving maken?](#how-many-vms-can-i-create-per-lab)
- [Hoe kan ik een directe koppeling naar Mijn testomgeving delen?](#how-do-i-share-a-direct-link-to-my-lab)
- [Wat is een Microsoft-account?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Problemen oplossen 
 
- [Mijn onderdeel is mislukt tijdens het maken van VM. Hoe kan ik dit oplossen?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Waarom is niet mijn bestaande virtuele netwerk correct op te slaan?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?
Als uw vraag hier niet wordt weergegeven, laat het ons weten, zodat we u een antwoord te vinden.

- Stelt u een vraag in de [Disqus thread](#comments) aan het einde van deze Veelgestelde vragen en oefenen met het team van Azure Cache en andere leden van de community van dit artikel.
- Als u wilt een grotere doelgroep bereiken, stelt u een vraag op het [Azure DevTest Labs MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)en oefenen met het team van Azure DevTest Labs en andere leden van de community.
- Als u een verzoek voor een functie, moet u uw aanvragen en ideeën weer de [Azure DevTest Labs gebruiker Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs)stuurt.

### <a name="why-should-i-use-azure-devtest-labs"></a>Waarom moet ik Azure DevTest Labs gebruiken? 
Azure DevTest Labs kunt opslaan uw team tijd en geld. Ontwikkelaars kunnen hun eigen omgevingen met verschillende andere databases maken en onderdelen snel implementatie en configuratie van toepassingen gebruiken. Met aangepaste afbeeldingen en formules, kunnen virtuele machines worden opgeslagen als sjablonen en eenvoudig gereproduceerd. Daarnaast bieden labs diverse configureerbare beleidsregels waarmee testomgeving beheerders kunnen afval en beheren van een team-omgevingen. Dit beleid bevatten automatisch-afsluiten, kosten drempel, maximum VMs per gebruiker en maximumgrootte VM. Lees het [Overzicht](devtest-lab-overview.md) of bekijk de [inleidende video](/documentation/videos/videos/what-is-azure-devtest-labs)voor een uitgebreidere uitleg van Azure DevTest Labs. 

### <a name="what-does-worry-free-self-service-mean"></a>Wat betekent 'probleemloos, selfservice'?
"Zelf te kunnen probleemloos," betekent dat ontwikkelaars en testers hun eigen omgevingen zo nodig maken, en beheerders de beveiliging hebben van als u weet dat Azure DevTest Labs helpt minimaliseren verspillen en en kosten te beheren. Beheerders kunnen opgeven welke groottes VM zijn toegestaan, wordt het maximum aantal VMs, en wanneer VMs worden gestart en afsluiten. Azure DevTest Labs ook eenvoudig bewaken kosten en waarschuwingen weergeven op de hoogte blijven van hoe resources in een testomgeving worden gebruikt. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Hoe kan ik Azure DevTest Labs gebruiken? 
Azure DevTest Labs is handig als u ontwikkelaar vereisen of testomgeving en u wilt ze snel te reproduceren en/of ze beheren met kosten beleidsregels op te slaan. 

Hier volgen enkele scenario's die onze klanten gebruiken Azure DevTest Labs voor: 

- Ontwikkelaar en test omgevingen op één plaats beheren, met behulp van beleidsregels voor om te beperken kosten en aangepaste afbeeldingen om te delen via het team genereert.
- Ontwikkeling van een toepassing aangepaste afbeeldingen gebruiken om op te slaan de schijf staat gedurende de fasen ontwikkeling.
- De kosten ten opzichte van de voortgang bijhouden. 
- Maken van grootschalige testomgeving voor kwaliteit assurance te testen.
- Onderdelen en formules gebruiken om gemakkelijk configureren en te reproduceren van een toepassing op verschillende omgevingen. 
- VMs distribueren voor hackathons (samenwerkingscultuur ontwikkelaar of test werk) en klikt u vervolgens eenvoudig opheffen van provisioning ze wanneer de gebeurtenis eindigt. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hoe ben ik gefactureerd voor Azure DevTest Labs? 
Azure DevTest Labs is een gratis service, wat betekent dat gratis labs maken en configureren van het beleid, sjablonen en onderdelen is. U betaalt alleen voor de Azure resources in uw labs, zoals virtuele machines, opslag-accounts en virtuele netwerken gebruikt. Voor meer informatie over de kosten van testomgeving resources, lees meer over het [Azure DevTest Labs prijzen](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Wat zijn de verschillende niveaus in Azure DevTest Labs?  
Beveiligingstoegang wordt bepaald door [Azure Role-Based Access besturingselement RBAC ()](../active-directory/role-based-access-built-in-roles.md). Als u wilt weten over de werking van access, is het handig voor meer informatie over de verschillen tussen een machtiging, een rol en een bereik gedefinieerd door RBAC.

- **Machtiging** : een machtiging is een gedefinieerde toegang tot een bepaalde actie. Een machtigingsniveau kan bijvoorbeeld alleen toegang tot alle virtuele machines. 
- **Rol** - een rol is een set machtigingen die kunnen worden gegroepeerd en toegewezen aan een gebruiker. Bijvoorbeeld: 'abonnement eigenaar van een' toegang heeft tot alle bronnen binnen een abonnement. 
- **Scope** - bereik is een niveau in de hiërarchie van Azure resource. Een bereik kan bijvoorbeeld een resourcegroep of een enkel laboratorium of het hele abonnement. 
 
Binnen het bereik van Azure DevTest Labs, er zijn twee soorten rollen om gebruikersmachtigingen te definiëren: testomgeving eigenaar en testomgeving gebruiker.

- **De eigenaar van de testomgeving** - de eigenaar van een testomgeving heeft de toegang tot alle bronnen binnen een testomgeving. Daarom kunnen ze beleid wijzigen, lezen en schrijven eventuele VMs, het virtuele netwerk wijzigen, enzovoort. 
- **Testomgeving gebruiker** - een gebruiker testomgeving kunt bekijken alle testomgeving resources, zoals VMs, beleidsregels en virtuele netwerken, maar ze beleidsregels of een VMs gemaakt door andere gebruikers niet wijzigen. Het is ook mogelijk te maken van aangepaste rollen in Azure DevTest Labs en leert u hoe u in het artikel, [gebruikersmachtigingen verlenen voor specifieke testomgeving beleid](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Aangezien bereiken hiërarchische, wanneer een gebruiker machtigingen voor een bepaald bereik heeft, worden ze automatisch weer de desbetreffende machtigingen aan elke lagere niveaus scope wordt omsloten. Bijvoorbeeld als een gebruiker is toegewezen aan de rol van de eigenaar van het abonnement, hebben klikt u vervolgens ze toegang tot alle resources in een abonnement. Deze resources omvatten alle virtuele machines, alle virtuele netwerken en alle praktijk. Dus, neemt de eigenaar van een abonnement automatisch over de rol van de eigenaar van de testomgeving. Het tegenovergestelde is echter niet waar. De eigenaar van een testomgeving heeft toegang tot een laboratorium, dat wil een lager bereik dan het abonnement zeggen. De eigenaar van een testomgeving dus niet virtuele machines of virtuele netwerken of alle bronnen die de testomgeving buiten zien. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hoe maak ik een rol toe te staan dat gebruikers een bepaalde taak uitvoeren?
Een volledig artikel over het maken van aangepaste rollen en machtigingen toewijzen aan die rol vindt u hier. Hier volgt een voorbeeld van een script dat wordt gemaakt van de rol "DevTest Labs geavanceerde gebruiker', die daartoe is gemachtigd om te starten en stoppen alle VMs in een testomgeving:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Is Azure DevTest Labs geïntegreerd met mijn CI/CD-toolchain? 
Als u VSTS gebruikt, ziet een [toestelnummer van de Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) die kunt u uw verkooppijplijn release in Azure DevTest Labs automatiseren. Enkele van het gebruik van dit toestel zijn:

- Maken en implementeren van een VM automatisch en configureren met de meest recente versie Azure bestand kopiëren of PowerShell VSTS taken gebruiken. 
- Automatisch vastleggen de status van een VM na testen om te reproduceren een fout op de dezelfde VM om verder te onderzoeken. 
- De VM aan het einde van de pijplijn release verwijderen wanneer deze niet meer nodig is. 

De volgende blogberichten bieden instructies en informatie over het gebruik van de extensie VSTS:
 
- [Azure DevTest Labs – VSTS-extensie](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Een nieuwe VM in een bestaande AzureDevTestLab van VSTS implementeren](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Met behulp van VSTS Release Management voor doorlopende implementaties naar AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Voor andere toolchains CI/CD kunnen alle hierboven genoemde scenario's die kunnen worden bereikt tot en met de extensie van de taken VSTS op dezelfde manier worden bereikt via het implementeren van [Azure resourcemanager sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) met [Azure PowerShell-cmdlets](../resource-group-template-deploy.md) en [.NET SDK's](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). U kunt ook [REST API's voor DevTest Labs](http://aka.ms/dtlrestapis) integreren met uw toolchain.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Waarom zie ik bepaalde VMs in het blad Azure virtuele Machines die ik Zie binnen Azure DevTest Labs niet?
Wanneer een VM is gemaakt in Azure DevTest Labs, krijgen machtiging voor toegang tot die VM. U zijn kunt weergeven in het blad labs en in het blad **virtuele Machines** . Gebruikers in de rol DevTest Labs ziet alle virtuele machines gemaakt in een testomgeving via van de testomgeving **alle virtuele Machines** blade. Echter gebruikers in de rol DevTest Labs niet krijgen automatisch leestoegang voor VM-resources die anderen hebt gemaakt. Daarom worden deze VMs worden niet weergegeven in het blad **virtuele Machines** . 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Wat is het verschil tussen aangepaste afbeeldingen en formules? 
Een aangepaste afbeelding is een VHD (virtuele harde schijf), terwijl een formule is een afbeelding die u kunt configureren met aanvullende instellingen die u kunt opslaan en reproduceren. Een aangepaste afbeelding is mogelijk beter als u wilt snel verschillende omgevingen maken met dezelfde eenvoudige, onveranderlijke afbeelding. Een formule is mogelijk beter als u wilt de configuratie van uw VM met de meest recente bits, een virtueel netwerk/subnet of een bepaalde grootte reproduceren. Zie het artikel, [aangepaste afbeeldingen van vergelijking en formules in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md)voor een uitgebreidere uitleg. 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hoe maak ik meerdere VMs met dezelfde sjabloon in één keer? 
U kunt de [VSTS taken extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) of [een sjabloon Azure resourcemanager genereren](devtest-lab-add-vm-with-artifacts.md#save-arm-template) tijdens het maken van een VM en [implementeren van de sjabloon Azure resourcemanager van Windows PowerShell](../resource-group-template-deploy.md)gebruiken. 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Hoe verplaats ik mijn bestaande Azure VMs in mijn testomgeving Azure DevTest Labs? 
We een oplossing rechtstreeks VMs verplaatsen naar Azure DevTest Labs ontwerpt, maar momenteel kunt u uw bestaande VMs naar kopiëren Azure DevTest Labs als volgt: 

1. Kopieer de VHD-bestand van uw bestaande VM met deze [Windows PowerShell-script](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [De aangepaste afbeelding maken](devtest-lab-create-template.md) binnen uw testomgeving Azure DevTest Labs. 
1. Een VM maken in een testomgeving van uw aangepaste afbeelding 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kan ik meerdere schijven aan mijn VMs koppelen? 
Meerdere schijven koppelen aan VMs wordt ondersteund.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hoe ik het proces van het uploaden van bestanden VHD als u wilt maken van aangepaste afbeeldingen automatiseren? 
Er zijn twee opties:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) kan worden gebruikt om te kopiëren of VHD bestanden uploaden naar de opslag-account dat is gekoppeld aan een testomgeving.
- [Microsoft Azure opslag Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandig product-app die compatibel is met Windows, OSX en Linux.   
 
Als u de bestemming opslag-account dat is gekoppeld aan uw testomgeving zoekt, als volgt te werk:

1. Meld u aan bij de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Selecteer **Resourcegroepen** in het linkerdeelvenster. 
1. Zoek en selecteer de resourcegroep die is gekoppeld aan uw testomgeving. 
1. Klik op het blad **Overzicht** , selecteert u een van de opslag-accounts. 
1. Selecteer **BLOB's**.
1. Hiermee zoekt u de verbinding met uploads in de lijst. Als er geen voorkomt, Ga terug naar stap #4 en probeert u een ander opslag-account.
1. De **URL** als de bestemming in de opdracht AzCopy gebruiken.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hoe kan ik het proces van het verwijderen van alle VMs in mijn testomgeving automatiseren?

Naast het VMs verwijderen uit uw testomgeving in de portal van Azure, kunt u alle de VMs in uw testomgeving met een PowerShell-script verwijderen. In het volgende voorbeeld, wijzigt u gewoon de parameterwaarden onder de opmerking **waarden om te wijzigen** . U kunt ophalen de `subscriptionId`, `labResourceGroup`, en `labName` waarden uit het blad testomgeving in de portal van Azure. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Wat zijn de onderdelen? 
Onderdelen zijn aanpasbare elementen die kunnen worden gebruikt om de meest recente bits of de ontwikkelaar's naar een VM implementeren. Ze zijn gekoppeld aan uw VM tijdens het maken van met een paar eenvoudige muisklikken en zodra de VM is ingericht, de onderdelen implementeren en configureren van uw VM. Er zijn verschillende vooraf bestaande onderdelen in onze [openbare Github opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), maar u kunt ook eenvoudig [uw eigen onderdelen van de auteur](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hoe maak ik een laboratorium van een sjabloon Azure resourcemanager? 
We hebben een [Github opslagplaats van testomgeving Azure resourcemanager sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) die u als implementeren kunt opgegeven-is of als u wilt maken van aangepaste sjablonen voor uw labs wijzigen. Elk van deze sjablonen voor een koppeling waarop u klikken kunt om te implementeren van de testomgeving als heeft-staat onder uw eigen Azure-abonnement of u de sjabloon en [implementeren via PowerShell of Azure CLI](../resource-group-template-deploy.md)kunt aanpassen.
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Waarom zijn mijn VMs gemaakt in verschillende resourcegroepen met willekeurige namen? Kan ik de naam van wijzigen of deze resourcegroepen wijzigen? 
Resourcegroepen is deze manier om Azure DevTest Labs voor het beheren van de gebruikersmachtigingen en toegang tot virtuele machines gemaakt. Terwijl u de VM naar een andere resourcegroep met de naam van de gewenste verplaatsen kunt, wordt doen dus niet aanbevolen. We werken aan het verbeteren van deze ervaring zodat meer flexibiliteit.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hoeveel labs kan ik maken onder hetzelfde abonnement? 
Er is geen specifieke limiet van het aantal hand waarvan kan worden gemaakt per abonnement. De resources die worden gebruikt, zijn echter beperkt per abonnement. U kunt lezen over de [limieten en quota geheven op Azure abonnementen](../azure-subscription-service-limits.md) en [hoe u deze limieten verhogen](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Hoeveel VMs kan ik per testomgeving maken? 
Er is geen specifieke limiet van het aantal VMs die per testomgeving kan worden gemaakt. Echter ondersteunt een testomgeving momenteel alleen ongeveer 40 VMs uitgevoerd op hetzelfde moment in standard opslag en 25 VMs tegelijkertijd worden uitgevoerd in de premium-opslag. We werken momenteel aan deze limieten vergroten. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hoe kan ik een directe koppeling naar Mijn testomgeving delen?

Als u wilt delen van een directe koppeling naar uw gebruikers testomgeving kunt u de volgende procedure uitvoeren.

1. Blader naar de testomgeving in de portal van Azure.
2. Kopieer de URL testomgeving vanuit uw browser en deze delen met uw gebruikers testomgeving. 

>[AZURE.NOTE] Als uw gebruikers testomgeving externe gebruikers met een [MSA-account worden](#what-is-a-microsoft-account) en ze niet deel van uw bedrijf Active directory uitmaakt, ze mogelijk een foutbericht bij het navigeren naar de onderstaande koppeling. Als ze een foutmelding ontvangt, geef de instructie om ze op hun naam in de rechterbovenhoek van de Azure-portal en selecteer de map waar de testomgeving uit de **adreslijst** sectie van het menu bestaat.

### <a name="what-is-a-microsoft-account"></a>Wat is een Microsoft-account?

Een Microsoft-account is wat u gebruikt voor bijna alles wat die u met Microsoft-apparaten en services doen. Dit is een e-mailadres en wachtwoord waarmee u zich aanmelden bij Skype, Outlook.com, OneDrive, Windows Phone en Xbox LIVE – en betekent dit dat uw bestanden, foto's, contactpersonen en instellingen kunnen u Voer aan elk apparaat. 

>[AZURE.NOTE] Microsoft-account gebruikt om u te worden aangeroepen "Windows Live ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Mijn onderdeel is mislukt tijdens het maken van VM. Hoe kan ik dit oplossen? 
Raadpleeg het blogbericht [problemen met verbroken onderdelen in AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - geschreven door een van onze MVP's - meer informatie over het verkrijgen van logboeken over uw mislukte onderdeel. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Waarom is niet mijn bestaande virtuele netwerk correct op te slaan?  
Een mogelijkheid is dat de netwerknaam van uw virtuele perioden bevat. Als dat het geval is, kunt u proberen de perioden verwijderen of deze vervangen door streepjes en probeer het virtuele netwerk opnieuw op te slaan.
