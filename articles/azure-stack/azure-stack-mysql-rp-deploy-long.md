<properties
    pageTitle="De provider van de resource MySQL op Azure Stack implementeren | Microsoft Azure"
    description="Gedetailleerde stappen voor het implementeren van de Provider van de Resource MySQL naar Azure stapel."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>De Provider van de Resource MySQL op Azure-Stack voor gebruik met WebApps implementeren

> [AZURE.NOTE] De volgende informatie is alleen van toepassing op Azure stapel TP1 implementaties.

Gebruik dit artikel voor volgt u de gedetailleerde stappen voor het instellen van de Provider van de Resource MySQL op de stapel Azure aankoopbewijs haalbaarheidstest zodat u [met MySQL-databases](azure-stack-mysql-rp-deploy-short.md) op Azure-Stack beginnen kunt, waaronder MySQL gebruiken als de backend voor WordPress sites die zijn gemaakt met [Azure Web Apps](azure-stack-webapps-deploy.md).

## <a name="set-up-steps-before-you-deploy"></a>Stappen hebt ingesteld voordat u implementeren

Voordat u de resource-provider implementeert, moet u naar:

- Hebt u de afbeelding van een standaard-Windows Server met .NET 3.5
- Verbeterde beveiliging van Internet Explorer (IE) uitschakelen
- Installeer de meest recente versie van Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Maken van een afbeelding van Windows Server, zoals .NET 3.5

Als u de stapel Azure-bits na 2/23/2016 gedownload, omdat de standaardafbeelding voor de basis Windows Server 2012 R2 .NET 3.5 framework in deze downloaden en hoger bevat, kunt u deze stap overslaan.

Als u vóór 2/23/2016 hebt gedownload, moet u een Windows Server 2012 R2 Datacenter VHD met .NET 3.5-afbeelding maken en instellen als de standaardafbeelding in de bibliotheek van de afbeelding Platform is.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Uitschakelen IE enhanced cookies beveiligings- en inschakelen

Als u wilt implementeren een resource-provider, uitvoeren u de filter geïntegreerde uitvoeren van scripts-omgeving (wissen) als een beheerder, dus moet u toestaan dat cookies en JavaScript in het Internet Explorer-profiel dat u gebruikt voor zowel de beheerder als de gebruiker aanmeldingen aanmelden bij Azure Active Directory.

**Als u wilt uitschakelen IE verbeterde beveiliging:**

1. Meld u aan bij de computer Azure stapel bewijs-van-concept (haalbaarheidstest) als een AzureStack/beheerder en open vervolgens Serverbeheer.

2. **Verbeterde beveiliging van IE** voor zowel beheerders en gebruikers uitschakelen.

3. Meld u aan bij de **ClientVM.AzureStack.local** virtuele machine als een beheerder en open vervolgens Serverbeheer.

4. **Verbeterde beveiliging van IE** voor zowel beheerders en gebruikers uitschakelen.

**Cookies inschakelen:**

1. Klik op het startscherm van Windows, klikt u op **alle apps**op **Windows Bureau-accessoires**, met de rechtermuisknop op **Internet Explorer**, wijs **meer**en selecteer **Als administrator uitvoeren**.

2. Als u wordt gevraagd, controleren **Gebruik aanbevolen beveiliging**en klik vervolgens op **OK**.

3. Klik in Internet Explorer op de **Extra (tandwielpictogram)** &gt; **Internetopties** &gt; tabblad **Privacy** .

4. Klik op **Geavanceerd**, zorg ervoor dat beide knoppen **accepteren** zijn geselecteerd, klikt u op **OK**en klik vervolgens nogmaals op **OK** .

5. Sluit Internet Explorer en start opnieuw PowerShell ISE als beheerder.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Een Azure stapel compatibele versie van Azure PowerShell installeren

1. Verwijder eventuele bestaande Azure PowerShell van uw VM-Client.

2. Meld u aan bij de computer Azure stapel Haalbaarheidstest als AzureStack/beheerder.

3. Met extern bureaublad, meld u aan bij de **ClientVM.AzureStack.local** virtuele machine als beheerder.

4. Open het Configuratiescherm, klikt u op **een programma verwijderen** &gt; klikt u op **Azure PowerShell** &gt; Klik op **verwijderen**.

5. [Download de meest recente Azure PowerShell die ondersteuning biedt voor Azure stapel](http://aka.ms/azstackpsh) en installeer deze.

    Nadat u PowerShell hebt geïnstalleerd, kunt u deze controle PowerShell-script om ervoor te zorgen dat u verbinding met uw Azure stapel-exemplaar maken kunt (een webpagina login moet worden weergegeven) kunt uitvoeren.

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>De resource provider implementatie PowerShell bootstrap

1. Het externe bureaublad Azure stapel Haalbaarheidstest verbinden met clientVm.AzureStack.Local en meld u aan als azurestack\\azurestackuser.

2. [Download de MySQL-RP binaire bestanden](http://aka.ms/masmysqlrp) archiveren en pak het D:\\MySQLRP.

3. D: uitvoeren\\MySQLRP\\Bootstrap.cmd-bestand als een beheerder (azurestack\administrator).

    Hiermee opent het bestand Bootstrap.ps1 in PowerShell ISE.

4. Wanneer de windows PowerShell ISE is voltooid laden, klikt u op de knop "afspelen" of druk op F5.

    Twee belangrijkste tabbladen wordt geladen, elk met alle scripts en bestanden die u moet uw Provider van de Resource MySQL implementeren.

## <a name="prepare-prerequisites"></a>Vereisten voor voorbereiden

Klik op het tabblad **Vereisten voorbereiden** om:

- Vereiste certificaten maken
- MySQL binaire bestanden downloaden naar uw Azure-Stack
- Onderdelen uploaden naar een opslagruimte op Azure-Stack
- Galerie items publiceren

### <a name="create-the-required-certificates"></a>De vereiste certificaten maken
Dit script **Nieuw SslCert.ps1** voegt u de \_. AzureStack.local.pfx SSL-certificaat naar de D:\\MySQLRP\\vereisten\\BlobStorage\\Container map. Het certificaat beveiligt communicatie tussen de provider van de resource en het lokale exemplaar van de Azure Resource Manager.

1. Klik op het tabblad **Nieuw SslCert.ps1** in het primaire tabblad **Voorbereiden vereisten** en worden uitgevoerd.

2. In het dialoogvenster dat wordt weergegeven, typt u een wachtwoord PFX persoonlijke sleutel en **noteert u dit wachtwoord**te beschermen. U moet deze later.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>MySQL binaire bestanden downloaden naar uw Azure-Stack

1. Selecteer het tabblad **Downloaden-MySqlServer.ps1** en worden uitgevoerd.
2. Wanneer u wordt gevraagd, klikt u op **Ja** in het bevestigingsvenster accepteer de gebruiksrechtovereenkomst.

    Deze opdracht toegevoegd twee zip-bestanden naar de map D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Alle onderdelen uploaden naar een opslagruimte op Azure-Stack

1. Klik op het tabblad **Upload-Microsoft.MySql-RP.ps1** en worden uitgevoerd.

2. Typ in het dialoogvenster Windows PowerShell referentie verzoek om de referenties van Azure stapel service-beheerder.

3. Wanneer u hierom wordt gevraagd voor de Azure Active Directory-Tenant-ID, typt u de volledig gekwalificeerde domeinnaam van de Azure Active Directory-tenant: bijvoorbeeld microsoftazurestack.onmicrosoft.com.

    Een pop-upvenster wordt gevraagd om referenties.

    > [AZURE.TIP] Als het pop-upvenster niet wordt weergegeven, u die dit nog niet hebt hetzij IE uitgeschakeld verbeterde beveiliging JavaScript inschakelen op deze computer- en of u dit nog niet hebt geaccepteerd cookies in Internet Explorer. Zie [stappen voordat u implementeert instellen](#set-up-steps-before-you-deploy).

4. Typ uw beheerdersreferenties Azure stapel Service en klik vervolgens op **Aanmelden**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Galerie met items voor het maken van de resource later publiceren

Selecteer het tabblad **Publiceren-GalleryPackages.ps1** en worden uitgevoerd. Dit script worden opgeteld twee marketplace-items van de portal van de stapel Haalbaarheidstest van Azure marketplace die u gebruiken kunt om te implementeren database resources als marketplace-items.

## <a name="deploy-the-mysql-resource-provider-vm"></a>De Resource MySQL-Provider VM implementeren

Nu dat u hebt de haalbaarheidstest Azure stapel met de benodigde certificaten en marketplace items voorbereid, kunt u een Resource Provider voor SQL Server implementeren. Klik op het tabblad **implementeren MySQL-provider** naar:

   - Verstrek waarden in een JSON-bestand dat de implementatieproces verwijst naar
   - De resource-provider implementeren
   - De lokale DNS-records bijwerken
   - De SQL Server Resource Provider Adapter registreren

### <a name="provide-values-in-the-json-file"></a>Verstrek waarden in de JSON-bestand

Klik op **Microsoft.MySqlprovider.Parameters.JSON**. Dit bestand heeft parameters dat de sjabloon Azure resourcemanager moet behoren implementeren naar Azure stapel.

1. Vul de **lege** parameters in de JSON-bestand:

    - Zorg ervoor dat u de **adminusername** en **beheerderswachtwoord** voor MySQL Resource Provider VM opgeeft.

    - Zorg ervoor dat u het wachtwoord voor de parameter weer **SetupPfxPassword** die u een genoteerd in de stap [voorbereiden prequisites hebt](#prepare-prerequisites) opgeven.

    - Zorg ervoor dat u **basicAuthUserName** en **basicAuthPassword** parameters opgeeft. **Noteer deze waarden.** U moet ze later wilt registreren van de resource-provider.

2. Klik op **Opslaan**.

### <a name="deploy-the-resource-provider"></a>De resource-provider implementeren

1. Klik op het tabblad **Deploy-Microsoft.Mysql-provider.PS1** en het script uitvoeren.
2. Typ de naam van de tenant in Azure Active Directory wanneer u wordt gevraagd.
3. In het pop-upvenster indienen uw beheerdersreferenties voor stapel Azure-service.

De volledige implementatie duurt tussen 15 en 45 minuten op sommige ten zeerste gebruikte Azure stapel POCs.

### <a name="update-the-local-dns"></a>De lokale DNS-records bijwerken

1. Klik op het tabblad van de **Register-Microsoft.MySQL-fqdn.ps1** en het script uitvoeren.
2. Wanneer u wordt gevraagd voor Azure Active Directory-Tenant-ID, de volledig gekwalificeerde domeinnaam van de Azure Active Directory-tenant input: bijvoorbeeld **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>De Resource-Provider voor SQL-RP registreren

1. Klik op het tabblad van de **Register-Microsoft.My-provider.ps1** en het script uitvoeren.

2. Wanneer u hierom wordt gevraagd om referenties, gebruikt u wat u hebt genoteerd als de parameters **basicAuthUserName** en **basicAuthPassword** .

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de implementatie met behulp van de Azure stapel-Portal

1. Meld u af bij de ClientVM en opnieuw aanmelden als **AzureStack\User**.

2. Klik op het bureaublad, op **Azure stapel Haalbaarheidstest Portal** en meld u aan bij de portal als de service-beheerder.

3. Controleer of dat de implementatie is voltooid. Klik op **Bladeren** &gt; **Resourcegroepen**, klikt u op de resourcegroep die u hebt gebruikt (standaard is **MySQLRP**), en zorg ervoor dat het essentials deel van het blad (bovenste helft) **implementatie is voltooid lezen**.


4. Controleer of dat de registratie is voltooid. Klik op **Bladeren** &gt; **Resource providers**en zoek naar **MySQL lokaal**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Uw eerste MySQL-database als u wilt testen, uw implementatie maken

1. Meld u aan bij de portal Azure stapel Haalbaarheidstest als service-beheerder.

2. Klik op de **+** knop &gt; **aangepaste** &gt; **MySQL-Server en Databases**.

3. Vul het formulier met informatie over de database in.

    **Noteer de "servernaam" u invoert.** De tekenreeks verbindingen voor de database bevat "naam van de server" als onderdeel van de gebruikersnaam in te voeren: bijvoorbeeld ** "user@ <ServerName>"**. U moet een gebruikersnaam in deze indeling invoer wanneer u verbinding met de database maakt: bijvoorbeeld wanneer u een MySQL-website met de provider van de resource Azure-website implementeren


## <a name="next-steps"></a>Volgende stappen

Probeer andere [PaaS services](azure-stack-tools-paas-services.md) , zoals de [resource-provider voor SQL Server](azure-stack-sql-rp-deploy-short.md) en de [Web Apps resource provider](azure-stack-webapps-deploy.md).
