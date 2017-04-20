<properties
    pageTitle="Azure stapel Haalbaarheidstest implementeren | Microsoft Azure"
    description="Leer hoe u de Azure stapel Haalbaarheidstest voorbereiden en uitvoeren van de PowerShell-script om Azure stapel Haalbaarheidstest implementeren."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Azure stapel Haalbaarheidstest implementeren
Als u wilt de Azure stapel Haalbaarheidstest implementeren, moet u eerst [de implementatie-computer voorbereiden](#prepare-the-deployment-machine) en [uitvoeren van de PowerShell-script voor implementatie](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Download en Microsoft Azure stapel Haalbaarheidstest TP2 ophalen

Voordat u begint, zorg ervoor dat u ten minste 85 GB schijfruimte.

1. Het downloaden van Azure stapel Haalbaarheidstest TP2 bestaat uit een zip-bestand met de volgende 12 bestanden, totaalbedrag ~ 20 GB:
    - 1 MicrosoftAzureStackPOC.EXE
2. Controleer de gebruiksrechtovereenkomst scherm en informatie van de Wizard Self-Extractor en klik vervolgens op **volgende**.
3. Controleer het scherm van de privacyverklaring en de gegevens van de Wizard Self-Extractor en klik vervolgens op **volgende**.
4. Selecteer de bestemming voor de bestanden worden geëxtraheerd, klikt u op **volgende**.
    - De standaardinstelling is: <drive letter>:\<huidige map > \Microsoft Azure stapel Haalbaarheidstest
5. Controleer de bestemming locatie scherm en informatie van de Wizard Self-Extractor, en klik op **uitpakken** als u wilt extraheren van de CloudBuilder.vhdx (~44.5 GB) en ThirdPartyLicenses.rtf-bestanden.

> [AZURE.NOTE] Nadat u de bestanden hebt geëxtraheerd, kunt u het zip-bestand als u ruimte op de computer wilt verwijderen. Of u kunt verplaatsen het zip-bestand naar een andere locatie, dus als u wilt implementeren u hoeft niet het zip-bestanden om opnieuw te downloaden.

## <a name="prepare-the-deployment-machine"></a>De implementatie-computer voorbereiden

1. Zorg ervoor dat u kunt fysiek verbinding met de implementatie-computer maken, of toegang tot de fysieke console (zoals KVM hebben). U moet deze toegang nadat u de computer van de implementatie in stap 9 opnieuw starten.

2. Zorg ervoor dat de implementatie-computer voldoet aan de [minimale vereisten](azure-stack-deploy.md). U kunt de [Implementatie toegankelijkheidscontrole voor Azure stapel Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) gebruiken om te bevestigen van uw vereisten.

3. Meld u aan als de lokale beheerder naar uw computer Haalbaarheidstest.

4. Kopieer het bestand CloudBuilder.vhdx naar C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Als u niet de aanbevolen script gebruiken voor het voorbereiden van uw computer in Haalbaarheidstest host (stap 5 – stap 7), voert u een licentie-toets op de activeringspagina niet. Een evaluatieversie van Windows Server 2016 afbeelding is opgenomen en verlooptijd waarschuwingsberichten invoeren van een licentiesleutel veroorzaakt.

5. Op de computer Haalbaarheidstest, voer het volgende PowerShell-script om de bestanden van de ondersteuning Azure stapel TP2 te downloaden:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Dit script downloads van de Azure stapel TP2 ondersteuning-bestanden naar de map die is opgegeven met de parameter $LocalPath.
    
6. Open een verhoogde PowerShell-console en wijzigen in de map waarnaar u de bestanden hebt gekopieerd.
    - 11 MicrosoftAzureStackPOC-N.BIN (waarbij N staat voor 1-11)
7. Met de rechtermuisknop op de MicrosoftAzureStackPOC.EXE > uitvoeren als beheerder.

8. Voer het script PrepareBootFromVHD.ps1. Dit script en de zonder toezicht-bestanden zijn beschikbaar met de andere ondersteuning-scripts die samen met deze opbouwen.
    Er zijn vijf parameters voor deze PowerShell-script:
    - CloudBuilderDiskPath (vereist) – het pad naar de CloudBuilder.vhdx op de HOST.
    - (Optioneel) – DriverPath kunt u extra stuurprogramma's voor de host in de virtuele HD toevoegen.
    - (Optioneel) – ApplyUnattend Geef deze parameter schakeloptie om de configuratie van het besturingssysteem te automatiseren. Als u opgeeft, moet de gebruiker de beheerderswachtwoord om te configureren van het besturingssysteem wordt opgestart (mits de bijbehorende bestanden unattend_NoKVM.xml hiervoor) opgeven.
    Als u deze parameter niet gebruikt, wordt het bestand algemene unattend.xml zonder verdere aanpassingen gebruikt. U moet KVM voltooid aanpassing nadat deze opnieuw is opgestart.
    - Beheerderswachtwoord (optioneel) – alleen gebruikt als de parameter ApplyUnattend is ingesteld, moet ten minste zes tekens.
    - VHDLanguage (optioneel) – Hiermee geeft u de taal voor VHD, standaard ingesteld op 'en-US'.
    Het script wordt beschreven en voorbeeld hiervan bevat al is het meest gebruikt:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Als u deze exacte opdracht uitvoert, moet u de beheerderswachtwoord bij de prompt invoeren.

9. Wanneer het script voltooid is, moet u het opnieuw opstarten bevestigen. Als er andere gebruikers aangemeld, wordt deze opdracht, mislukt. Als de opdracht mislukt, voert u de volgende opdracht uit:`Restart-Computer -force` 

10. De HOST opnieuw is opgestart in het besturingssysteem van de CloudBuilder.vhdx, waar de implementatie blijft.

> [AZURE.IMPORTANT] Azure stapel vereist toegang tot Internet, rechtstreeks of via een transparante proxy. De implementatie TP2 Haalbaarheidstest ondersteunt precies één NIC voor netwerken. Als er meerdere NIC, Controleer of slechts één is ingeschakeld (en alle andere zijn uitgeschakeld) voordat u de implementatiescript uitvoeren in het volgende gedeelte.

## <a name="run-the-powershell-deployment-script"></a>De implementatie PowerShell-script uitvoeren

1. Meld u aan als de lokale beheerder naar uw computer Haalbaarheidstest. Gebruik de referenties die zijn opgegeven in de vorige stappen.

2. Open een verhoogde PowerShell-console.

3. In PowerShell, moet u deze opdracht uitvoert:`cd C:\CloudDeployment\Configuration`

4. De opdracht deploy uitvoeren:`.\InstallAzureStackPOC.ps1`

5. Klik bij de prompt **Enter het wachtwoord** een wachtwoord invoeren en bevestig dit. Dit is het wachtwoord voor de virtuele machines. Zorg ervoor dat u deze opnemen.

6. Voer de referenties voor uw Azure Active Directory-account. Deze gebruiker moet de globale beheerder in de directory-tenant.

7. De implementatie kan een paar uur, waarin het systeem automatisch opnieuw is opgestart eenmaal duren.

    > [AZURE.IMPORTANT] Als u de implementatie-voortgang bewaken wilt, moet u zich aanmelden als azurestack\AzureStackAdmin. Als u zich hebt aangemeld als beheerder van een lokale nadat de computer is verbonden met het domein kunt u de voortgang implementatie niet weergegeven. Niet opnieuw uitvoeren implementatie, in plaats daarvan Meld u aan als azurestack\AzureStackAdmin om deze te valideren dat deze wordt uitgevoerd.

    Wanneer de implementatie is geslaagd, de PowerShell-console wordt weergegeven: **voltooid: actie 'Implementatie'**.

    Als de implementatie is mislukt, kunt u proberen [opnieuw uitvoeren](azure-stack-rerun-deploy.md). U kunt [implementeren](azure-stack-redeploy.md) deze maken.

### <a name="deployment-script-examples"></a>Voorbeelden van de implementatie-script

Als uw identiteit AAD alleen is die is gekoppeld aan één AAD-map:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Als uw identiteit AAD gekoppeld aan groter is dan één AAD map is:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Als uw omgeving geen DHCP ingeschakeld, moet u de volgende aanvullende parameters naar een van de opties boven (voorbeeld gebruik verstrekt) opnemen:

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Optionele parameters InstallAzureStackPOC.ps1

| Parameter | Vereist/optioneel | Beschrijving |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Optionele | Hiermee stelt u de Azure Active Directory-gebruikersnaam en wachtwoord. Deze Azure referenties kunnen zijn een organisatie-ID of een Microsoft-Account. Als u wilt referenties van Microsoft-Account gebruikt, kunt u deze parameter in de cmdlet weglaat. Deze parameter weglaten een bericht dat de verificatie van Azure die wordt weergegeven tijdens de implementatie. Hiermee maakt u de verificatie en vernieuwen tokens die worden gebruikt tijdens de implementatie. |
| AADDirectoryTenantName | Vereist | De map tenant instellen. Gebruik deze parameter een specifieke map opgeven waar het AAD-account beschikt over machtigingen voor meerdere mappen beheren. Volledige naam van een Tenant AAD Directory in de notatie van <directoryName>. onmicrosoft.com. |
| Beheerderswachtwoord | Vereist | Hiermee stelt u de lokale beheerdersaccount en alle andere gebruikersaccounts op alle virtuele machines die worden gemaakt als onderdeel van Haalbaarheidstest-implementatie. Dit wachtwoord moet overeenkomen met de huidige lokale beheerderswachtwoord op de host. |
| AzureEnvironment | Optionele | Selecteer de Azure-omgeving waarmee u wilt deze implementatie van Azure stapel registreren. Opties voor opnemen *Openbare Azure*, *Azure - China* *Azure - Amerikaanse overheid*. |
| EnvironmentDNS | Optionele | Een DNS-server is gemaakt als onderdeel van de stapel Azure-implementatie. Als u wilt toestaan computers binnen de oplossing voor het oplossen van namen buiten het tijdstempel, vindt u uw bestaande infrastructuur DNS-server. De in tijdstempel DNS-server Hiermee stuurt u de naam van de onbekende resolutie aanvragen voor deze server. |
| NatIPv4Address | Vereist voor DHCP NAT-ondersteuning | Hiermee stelt u een statische IP-adres voor MAS-BGPNAT01. Deze parameter wordt alleen gebruiken als de DHCP een geldig IP-adres voor toegang tot Internet kan niet worden toegewezen. |
| NatIPv4DefaultGateway | Vereist voor DHCP NAT-ondersteuning | Hiermee stelt u de standaard-gateway gebruikt met het statische IP-adres voor MAS-BGPNAT01. Deze parameter wordt alleen gebruiken als de DHCP een geldig IP-adres voor toegang tot Internet kan niet worden toegewezen.  |
| NatIPv4Subnet | Vereist voor DHCP NAT-ondersteuning | IP-subnetten voorvoegsel gebruikt voor DHCP via NAT-ondersteuning. Deze parameter wordt alleen gebruiken als de DHCP een geldig IP-adres voor toegang tot Internet kan niet worden toegewezen.  |
| PublicVLan | Optionele | Hiermee stelt u de VLAN-ID. Deze parameter wordt alleen gebruiken als de host en MAS BGPNAT01 VLAN-ID voor toegang tot de fysieke netwerk (en Internet) moet configureren. Bijvoorbeeld:`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Opnieuw uitvoeren | Optionele | Deze optie uit te voeren implementatie gebruikt.  Alle vorige invoer wordt gebruikt. Opnieuw invoeren van de gegevens hebt verstrekt, wordt niet ondersteund omdat verschillende unieke waarden worden gegenereerd en die wordt gebruikt voor implementatie. |
| TimeServer | Optionele | Gebruik deze parameter als u moet een specifieke tijd-server opgeven. |

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure stapel](azure-stack-connect-azure-stack.md)
