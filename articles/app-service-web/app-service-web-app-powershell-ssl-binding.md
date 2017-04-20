<properties
    pageTitle="SSL-certificaten binding via PowerShell"
    description="Leer hoe u SSL-certificaten verbinden met uw web-app via PowerShell."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Service SSL-certificaat Binding via PowerShell #

Met de versie van Microsoft Azure PowerShell versie 1.1.0 is een nieuwe cmdlet toegevoegd die krijgt de gebruiker de mogelijkheid bestaand of nieuw SSL-certificaten koppelen aan een bestaande Web-App.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Meer informatie over het gebruik van Azure resourcemanager op basis van Azure PowerShell controleren cmdlets voor het beheren van uw Web-Apps [resourcemanager Azure op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Uploaden en een nieuwe SSL-certificaat binden ##

Scenario: De gebruiker wil graag een SSL-certificaat koppelen aan een van de Webapps.

De naam van de resource-groep met de web-app, de naam van de web-app, het pad certificaat .pfx-bestand op de computer van de gebruiker, het wachtwoord voor het certificaat en de aangepaste hostname u weet, kunnen we de volgende PowerShell-opdracht gebruiken om te die SSL-binding maken:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Houd er rekening mee dat voordat u een SSL-binding toevoegt aan een web-app, moet er een hostnaam (aangepaste domein) al geconfigureerd. Als de hostnaam niet is geconfigureerd en vervolgens u een fout krijgt 'hostname' bestaat niet tijdens het uitvoeren van nieuw-AzureRmWebAppSSLBinding. U kunt een hostname rechtstreeks vanuit de portal of via Azure PowerShell toevoegen. Het volgende PowerShell-fragment kan verwijzen naar de hostnaam voordat u nieuw-AzureRmWebAppSSLBinding configureren.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Het is belangrijk om te begrijpen dat de cmdlet Set-AzureRmWebApp de hostnamen voor de web-app overschrijft. Het codefragment van de bovenstaande PowerShell is dus toevoegen aan de bestaande lijst met de hostnamen voor de web-app.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Uploaden en binden van een bestaande SSL-certificaat ##

Scenario: De gebruiker wil graag een eerder geüploade SSL-certificaat koppelen aan een van de Webapps.

De lijst met certificaten al hebt geüpload naar een bepaalde resourcegroep met behulp van de volgende opdracht uit de Help

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Houd er rekening mee dat de certificaten die lokaal op een specifieke locatie en een resourcegroep, dat de gebruiker moet opnieuw het certificaat uploaden als de geconfigureerde WebApp in een andere locatie wordt weergegeven en resource groep andere die die van het benodigde certificaat 

De naam van de resource-groep met de web-app, de naam van de web-app vingerafdruk van het certificaat en de aangepaste hostname u weet, kunnen we de volgende PowerShell-opdracht gebruiken om te die SSL-binding maken:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Verwijderen van een bestaande SSL-binding  ##

Scenario: De gebruiker wilt verwijderen van een bestaande SSL-binding.

De naam van de resource-groep met de web-app, de naam van de web-app en de aangepaste hostname u weet, kunnen we de volgende PowerShell-opdracht gebruiken om te die SSL-binding verwijderen:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Houd er rekening mee dat als de verwijderde SSL-binding de laatste binding met behulp van dat certificaat in dat locatie, al dan niet standaard het certificaat wordt verwijderd is, als de gebruiker bewaren van het certificaat dat hij de optie DeleteCertificate kunt gebruiken wilt om het certificaat

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Verwijzingen ###
- [Azure resourcemanager op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Inleiding tot het App-omgeving](app-service-app-service-environment-intro.md)
- [Via Azure PowerShell met Azure resourcemanager](../powershell-azure-resource-manager.md)
