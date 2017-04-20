<properties
    pageTitle="Voorbeelden van Azure Active Directory-aanmelding activiteit rapport API | Microsoft Azure"
    description="Hoe u aan de slag met de API in de rapportage Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Voorbeelden van Azure Active Directory-aanmelding activiteit rapport API

In dit onderwerp maakt deel uit van een verzameling onderwerpen over de Azure Active Directory API rapportage.  
Azure AD-rapportage biedt u een API waarmee u toegang tot aanmeldingsproblemen activiteitsgegevens met code of gerelateerde hulpprogramma's.  
Het bereik van dit onderwerp is bedoeld als u met code van de steekproef voor de **aanmeldingsproblemen activiteit API**.

Zie:

- [Controlelogboeken bijhouden](active-directory-reporting-azure-portal.md#audit-logs) voor meer informatie
- [Aan de slag met de API in de rapportage Azure Active Directory](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage API.

Voor vragen, problemen of feedback, neemt u contact [AAD rapportage Help](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Vereisten voor
Voordat u de voorbeelden in dit onderwerp kunt, moet u de [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md)voltooien.  


##<a name="powershell-script"></a>PowerShell-script

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Het script
Als u klaar bent met het script bewerken, worden uitgevoerd en controleer of dat de verwachte gegevens uit als u de controlelogboeken rapport Logboeken wordt geretourneerd.

Het script retourneert uitvoer van het rapport aanmelden in de indeling van JSON. Er worden ook gemaakt een `SigninActivities.json` bestand met hetzelfde resultaat op. U kunt experimenteren met het aanpassen van het script om terug te keren gegevens uit andere rapporten en commentaar de uitvoerindelingen die u niet nodig hebt.



## <a name="next-steps"></a>Volgende stappen

- Wilt u aanpassen in de voorbeelden in dit onderwerp? Bekijk de [Azure Active Directory-aanmelding activiteit API verwijzing](active-directory-reporting-api-sign-in-activity-reference.md). 

- Als u zien van een volledig overzicht wilt van het gebruik van de Azure Active Directory API rapportage, raadpleegt u [aan de slag met Azure Active Directory API rapportage](active-directory-reporting-api-getting-started.md).

- Als u meer informatie wilt over het rapporteren van Azure Active Directory, raadpleegt u de [Azure Active Directory rapportage Guide](active-directory-reporting-guide.md).  