<properties
    pageTitle="Probleemoplossing van Microsoft Azure stapel | Microsoft Azure"
    description="Azure stapel voor probleemoplossing."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure stapel problemen oplossen

Dit document bevat algemene informatie over probleemoplossing voor Azure-Stack.

Zorg ervoor dat uw implementatieomgeving voldoet aan alle [vereisten](azure-stack-deploy.md) en [voorbereidingen](azure-stack-run-powershell-script.md) voordat u installeert voor een optimale ervaring. 

De aanbevelingen voor het oplossen van problemen die worden beschreven in deze sectie zijn afgeleid van diverse bronnen en mogelijk of uw probleem mogelijk niet is verholpen. Als er een probleem niet beschreven, zorg er dan voor dat het [Azure stapel MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) voor meer informatie en ondersteuning voor verdere controleren.

Voorbeelden van code worden geleverd huidige staat en verwachte resultaten kunnen niet worden gegarandeerd. In deze sectie zijn zowel veelgebruikte bewerkingen en updates zoals verbeteringen in het product worden geïmplementeerd.

  

## <a name="known-issues"></a>Bekende problemen

 - Mogelijk ziet u de volgende niet beëindigd fouten tijdens de implementatie, die zijn van invloed op implementatie success:
     - "De term 'C:\WinRM\Start-Logging.ps1' wordt niet herkend"
     - "Roepen EceAction: kan niet worden geïndexeerd in een null matrix" 
     - "InvokeEceAction: kan geen binding maken van de argumenten met parameter 'Bericht' omdat dit een lege tekenreeks is."
 - Ziet u mogelijk de implementatie mislukt bij stap 60.61.93 met een fout "Toepassing met id URI wordt niet gevonden". Dit probleem is vanwege de manier waarop toepassingen zijn geregistreerd in Azure Active Directory.  Als u deze fout ontvangt, gaat u verder met [de installatiescript opnieuw uitvoeren](azure-stack-rerun-deploy.md) van stap 60.61.93 totdat implementatie voltooid is.
 - U ziet dat de resource **Beschikbaarheid instellen** in de Marketplace onder de categorie **VirtualMachine-ARM weergegeven** – deze weergave alleen een cosmetische probleem is.
 - Bij het maken van een nieuwe virtuele machine in de portal, in de stap koppeling de optie **Basisbeginselen** standaard de opslagoptie SSD.  Deze instelling moet worden gewijzigd naar harde schijf of in de stap koppeling de **grootte** van VM implementatie, worden er geen VM formaten beschikbaar zijn om te selecteren en gaat u verder implementatie. 
 - Ziet u AzureRM PowerShell modules zijn niet meer standaard geïnstalleerd op de VM MAS-CON01 (in TP1 dit is de naam ClientVM). Dit probleem is inherent, omdat er een andere methode voor het [installeren van deze modules en verbinding maken](azure-stack-connect-powershell.md).  
 - U ziet dat de provider van de resource **Microsoft.Insights** niet automatisch is geregistreerd voor de tenant-abonnementen. Als u zien wilt-gegevens bewaken voor een VM geïmplementeerd als een tenant, voer de volgende opdracht uit PowerShell (als u het [installeren en verbinding te maken](azure-stack-connect-powershell.md) als een tenant): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Ziet u de functionaliteit in de portal voor de resourcegroepen exporteren, maar geen tekst weergegeven en is beschikbaar voor exporteren is.      
 - U kunt een implementatie van opslag resources groter is dan beschikbaar quotum starten.  Deze installatie mislukt en de resources account wordt geschorst.  Er zijn twee remediation opties beschikbaar:
     - Service-beheerder kan het quotum, vergroten, hoewel wijzigingen niet onmiddellijk van kracht en vaak duurt maximaal een uur doorgeven.
     - Service-beheerder kunt u een invoegtoepassing-abonnement maken met extra target die de tenant vervolgens aan het abonnement toevoegen kunt.
 - Wanneer u met de portal VMs maken op Azure stapel omgevingen met identiteit in 'Azure - China', kunt u ziet geen VM formaten beschikbaar zijn om te selecteren in de stap van de **grootte** van VM implementatie en niet kunt doorgaan met implementatie.
 - U ziet mogelijk een fout bij implementatie in de portal wanneer de VM heeft daadwerkelijk geïmplementeerd.
 - Wanneer u een abonnement, aanbieding of abonnement verwijdert, is het mogelijk dat er niet VMs worden verwijderd.
 - Hier ziet u het selectievakje extensies VM in de marketplace.
 - U kunt een VM uit een opgeslagen VM afbeelding niet implementeren.
 - Tenants ziet mogelijk services die niet zijn opgenomen in hun abonnement.  Als tenants implementeren van deze resources wilt, krijgen ze een fout.  Voorbeeld: De Tenant-abonnement bevat alleen de opslagruimte resources.  Optie voor het maken van andere bronnen zoals VMs zien tenant.  In dit scenario als een tenant probeert te implementeren van een VM, krijgen ze een bericht waarin wordt aangegeven dat de VM kan niet worden gemaakt. 
 - Tijdens de installatie van TP2, moet u de host OS in de opgegeven waarop u de stapel Azure setup-script uitvoeren of u mogelijk een foutbericht vermelding Windows messaging VHD binnenkort verloopt niet geactiveerd.


## <a name="deployment"></a>Implementatie

### <a name="deployment-failure"></a>Fout bij implementatie
Als er een fout tijdens de installatie optreden, wordt het installatieprogramma van Azure stapel kunt u doorgaan met een mislukte installatie volgens de [implementatiestappen opnieuw uit te voeren](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Aan het einde van de implementatie, de PowerShell-sessie nog is geopend en worden niet weergegeven in een uitvoer

Dit probleem is waarschijnlijk alleen het resultaat van het standaardgedrag van een PowerShell-opdrachtvenster wanneer deze is geselecteerd. De implementatie Haalbaarheidstest daadwerkelijk is voltooid, maar het script is onderbroken bij het selecteren van het venster. U kunt controleren of dat dit het geval is door te zoeken naar het woord "select" in de titelbalk van het opdrachtvenster.  Druk op ESC als u wilt deze selectie opheffen en het voltooiingsbericht moet worden weergegeven na het.

## <a name="templates"></a>Sjablonen

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure sjabloon won't implementeren naar Azure-Stack

Zorg ervoor dat:

- De sjabloon moet een Microsoft Azure-service die al is beschikbaar of in de proefversie van Azure gestapelde gebruiken.
- De API's gebruikt voor een specifieke bron worden ondersteund door het lokale exemplaar van de Azure stapel en dat u hebt samengesteld een geldige locatie ("lokale" in Azure stapel Technical Preview (TP) 2, tegenover de "Oost VS" of "Zuid-India" in Azure wordt aangegeven).
- U controleren [in dit artikel](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) over de cmdlets Test-AzureRmResourceGroupDeployment, waarin kleine verschillen tussen de syntaxis van de Azure resourcemanager onderschept.

U kunt ook de Azure stapel sjablonen die al zijn opgegeven in de [bibliotheek GitHub](http://aka.ms/AzureStackGitHub/) gebruiken om u aan de slag te helpen.

## <a name="virtual-machines"></a>Virtuele machines

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Nadat de host van mijn Microsoft Azure stapel Haalbaarheidstest is gestart, zijn alle mijn tenants VMs van Hyper-V Manager is verdwenen en keert u terug automatisch na een stapje wachten?

Als het systeem van de opslagsubsysteem en RPs nodig om te bepalen de consistentie terugkomt. De tijd die nodig is, is afhankelijk van de hardware en specificaties die wordt gebruikt, maar kan het enige tijd na het opnieuw opstarten van de host voor tenant VMs terugkomen en herkend zijn.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ik sommige virtuele machines hebt verwijderd, maar nog steeds de VHD-bestanden op schijf. Dit probleem naar verwachting?

Ja, is dit gedrag verwacht. Deze methode is ontworpen omdat:

- Wanneer u een VM verwijdert, worden VHD's worden niet verwijderd. Schijven zijn afzonderlijke resources in de resourcegroep.
- Wanneer een opslag-account wordt verwijderd, de verwijdering is zichtbaar direct via Azure resourcemanager (portal, PowerShell), maar de schijven die komen er zijn nog steeds opgeslagen totdat opschonen wordt uitgevoerd.

Als u 'beginregel' VHD's ziet, is het belangrijk te weten als ze deel uitmaken van de map voor een opslag-account dat is verwijderd. Als de opslagruimte-account niet is verwijderd, is het normale dat ze zijn er nog steeds.

U kunt meer informatie over het configureren van de drempelwaarde voor bewaren in de [opslagruimte accounts beheren](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Installatiestappen
De volgende informatie over de installatiestappen Azure stapel misschien handig voor het oplossen van problemen:  

| Index | Naam | Beschrijving|
| ----- | ----- | -----|
|0.11 | (DEP) Valideer de fysieke Machines | De hardware en configuratie van het besturingssysteem op de fysieke knooppunten valideren. |
| 0,12 | (DEP) Fysieke Machines netwerken voor Haalbaarheidstest configureren | Virtuele netwerk schakelopties en interfaces configureren. |
| 0.14 | (DEP) Domein implementeren | Active Directory op VM implementeren. |
| 0,15 | (DEP) Configureer de domein-server | Domeinserver configureren met beveiligingsgroepen enzovoort. |
| 0,16 | (DEP) Fysieke Machine configureren | Netwerken, join-domein en lokale beheerders setup configureren. |
| 0.18 | (STO) Opslag Cluster configureren | Opslag cluster maken, maakt u een opslag van toepassingen en -bestand met de server. |
| 0.19 | CPI) Setup stof infrastructuur | Stel de vereisten voor stof-implementatie. |
| 0.21 | (NETTO) BGP en NAT instellen | BGP en NAT - alleen nodig voor één knooppunt is geïnstalleerd. |
| 0.22 | (NETTO) NAT en tijdserver configureren | De tijdserver gesynchroniseerd en configureert NAT-vermeldingen. |
| 40.41 | CPI) Gast VMs maken | Het beheer VMs maken. |
| 40.42 | (FBI) PowerShell JEA instellen | PowerShell JEA voor alle rollen instellen. |
| 40.43 | (FBI) Azure stapel certificeringsinstantie instellen | Azure stapel certificeringsinstantie is geïnstalleerd. |
| 40.44 | (FBI) Azure stapel certificeringsinstantie configureren | Hiermee configureert u Azure stapel certificeringsinstantie. |
| 40.45 | (NETTO) AC op VMs instellen | AC is geïnstalleerd op de VMs Gast |
| 40.46 | (NETTO) AC configureren op VMs | AC configureren op de VMs Gast |
| 40.47 | (NETTO) Gast VMs configureren | Configureer het beheer VMs met AC ACL's. |
| 60.61.81 | (FBI) Azure stapel bellen configuratieservices - FabricRing vereiste implementeren | Hiermee maakt u VIP's voor FabricRing |
| 60.61.82 | (FBI) Implementeren van Azure stapel bellen configuratieservices - stof bellen Cluster implementeren | Installeert en configureert Azure stapel stof bellen Cluster. |
| 60.61.83 | (FBI) Beheerder uitbreidingen voor Resource providers implementeren | Installatie van beheerder uitbreidingen voor de resource providers |
| 60.61.84 | (ACS) Azure-consistente opslag in knooppuntniveau instellen. | Installeert en configureert Azure-consistente opslagruimte in knooppuntniveau. |
| 60.61.85 | (ACS) Azure-consistente opslag in clusterniveau instellen. | Installeert en configureert Azure-consistente opslagruimte in clusterniveau. |
| 60.61.86 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor InfraServiceController |
| 60.61.87 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor CPI |
| 60.61.88 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor ASAppGateway |
| 60.61.89 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor Controller-opslag |
| 60.61.90 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor HealthMonitoring |
| 60.61.91 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor ECE |
| 60.61.92 | (FBI) Azure stapel bellen Controller configuratieservices - vereiste implementeren | Vereisten voor PMM |
| 60.61.93 | (Katal) AzureStack Service principes maken | Azure Graph-toepassingen en Service principes in AAD maken. |
| 60.61.94 | (NETTO) Setup GW VMs | GW installeert op de VMs Gast. |
| 60.61.95 | (NETTO) GW VMs configureren | Hiermee configureert GW voor de VMs Gast. |
| 60.61.96 | (NETTO) IDN op hosts implementeren | IDN op infrastructuur hosts implementeren |
| 60.61.97 | (NETTO) IDN configureren | IDN rol configureren |
| 60.61.98 | (FBI) Setup WSUS VMs | WSUS-server is geïnstalleerd op de VMs Gast. |
| 60.61.99 | (FBI) WSUS VMs configureren | Hiermee configureert WSUS-server voor de VMs Gast. |
| 60.61.100 | (FBI) Azure SQL-VMs instellen | Azure SQL server is geïnstalleerd op de VMs Gast |
| 60.61.101 | (Katal) Vereisten voor VMs is de installatie. | Bepaalt de vereisten voor Microsoft Azure stapel op de VMs Gast. |
| 60.61.102 | (Katal) Setup is VMs | Microsoft Azure stapel installeert op de VMs Gast. |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.121 | (FBI) Resource-providers en Controllers implementeren | Resource-providers en Controllers is geïnstalleerd |
| 60.120.122 | (FBI) Configuratie van domeincontroller | Hiermee configureert u Controllers |
| 60.120.123 | (Katal) WAS configureren VMs | Hiermee configureert Microsoft Azure stapel voor de VMs Gast. |
| 60.120.124 | (Katal) Azure stapel AAD configuratie. | Azure stapel configureert met Azure AD. |
| 60.120.125 | (Katal) ADFS installeren | Installeert Active Directory Federation Services (ADFS) |
| 60.120.126 | (Katal) ADFS/Graph installeren | Azure stapel Graph is geïnstalleerd |
| 60.120.127 | (Katal) ADFS configureren | Hiermee configureert u Active Directory Federation Services (ADFS) |
| 60.140.141 | (FBI) SRP configureren | Hiermee configureert u Storage Resource Provider |
| 60.140.142 | (ACS) Azure-consistente opslag configureren. | Azure-consistente opslag configureert. |
| 60.140.143 | (FBI) Opslag-Accounts maken | Alle opslag-accounts moet worden gebruikt door andere providers maken. |
| 60.140.144 | (FBI) Gebruik registreren voor SRP | Gebruik registreren voor opslagprovider. |
| 60.140.145 | CPI) Gemaakte VMs, Hosts en Cluster migreren naar CPI | Objecten van het gemaakte VMs, Hosts en Cluster migreert naar CPI |
| 60.140.146 | (FBI) Windows Defender configureren | Windows Defender configureert |
| 60.160.161 | (MA) Agent cmdlets voor controle configureren | Configureert Agent bewaken |
| 60.160.162 | (FBI) Vereiste NRP | Installaties NRP vereisten |
| 60.160.163 | (FBI) NRP-implementatie | Installaties NRP  |
| 60.160.164 | (FBI) NRP configuratie | Configureert NRP |
| 60.160.165 | (FBI) CAPPL vereiste | Installaties CAPPL vereisten |
| 60.160.166 | (FBI) CAPPL-implementatie | Installaties CAPPL |
| 60.160.167 | (FBI) CAPPL configuratie | Hiermee configureert u CAPPL |
| 60.160.168 | (FBI) Vereiste FRP | Installaties FRP vereisten |
| 60.160.169 | (FBI) FRP-implementatie | Installaties FRP  |
| 60.160.170 | (FBI) FRP configuratie | Configureert FRP |
| 60.160.174 | (FBI) Vereiste URP | Installaties URP vereisten |
| 60.160.175 | (FBI) URP-implementatie | Installaties URP  |
| 60.160.176 | (FBI) URP configuratie | Configureert URP |
| 60.160.171 | (FBI) Vereiste HRP | Installaties HRP vereisten |
| 60.160.172 | (FBI) HRP-implementatie | Installaties HRP  |
| 60.160.173 | (FBI) HRP configuratie | Configureert HRP |
| 60.160.177 | (KV) Vereiste KeyVault | Installaties KeyVault vereisten |
| 60.160.178 | (KV) KeyVault-implementatie | Installaties KeyVault  |
| 60.160.179 | (KV) KeyVault configuratie | Configureert KeyVault | 
| 60.190.191 | (FBI) Galerie configureren | Galerie configureren |
| 60.190.192 | (FBI) Bellen configuratieservices configureren | Stof Ring-Services configureren |
| 60.221 | (FBI) Setup-Console VMs | Klik op de VMs Gast installaties Console-server. |
| 60.222 | (FBI) Setup-Console VMs | DVM inhoud verplaatsen naar de Console VM. |
| 251 | Voorbereiden voor toekomstige host opnieuw is opgestart | Beleid van de opnieuw instellen |


## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen](azure-stack-FAQ.md)
