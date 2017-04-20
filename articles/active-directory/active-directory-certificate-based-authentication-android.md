<properties 
    pageTitle="Aan de slag met verificatie via clientcertificaat die zijn gebaseerd op android-apparaat | Microsoft Azure" 
    description="Meer informatie over het configureren van verificatie via clientcertificaat gebaseerd in oplossingen met Android-apparaten" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>Aan de slag met verificatie via clientcertificaat die zijn gebaseerd op Android - Public Preview  

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android-apparaat](active-directory-certificate-based-authentication-android.md)


In dit onderwerp ziet u hoe u configureren en gebruiken van gebaseerde verificatie via clientcertificaat (CBA) op een Android-apparaat voor gebruikers van tenants in Office 365 Enterprise, bedrijven en Education-abonnementen. 

CBA kunt u kunnen worden geverifieerd door Azure Active Directory met een clientcertificaat op een Android- of iOS-apparaat wanneer u verbinding maakt van uw Exchange online-account: 

- Mobiele Office-toepassingen zoals Microsoft Outlook en Microsoft Word   
- Exchange ActiveSync (EAS)-clients 

Configuratie van deze functie hoeven invoeren van een combinatie van gebruikersnaam en wachtwoord in bepaalde e-mail- en Microsoft Office-toepassingen op uw mobiele apparaat. 
 

## <a name="supported-scenarios-and-requirements"></a>Ondersteunde scenario's en vereisten  



### <a name="general-requirements"></a>Algemene vereisten 


De volgende taken is vereist voor alle scenario's in dit onderwerp:  

- Toegang tot certificaat authority(s) certificaten van de client.  

- De certificaten authority(s) moet worden geconfigureerd in Azure Active Directory. Hier vindt u gedetailleerde stappen voor het voltooien van de configuratie in de sectie [Aan de slag](#getting-started) .  

- De basiscertificeringsinstantie en een tussenliggende certificeringsinstanties moeten worden geconfigureerd in Azure Active Directory.  

- Elke certificeringsinstantie moet een certificaat intrekkingsreferenties lijst (CRL) die kan worden verwezen via de URL van een internetverbinding hebt.  

- Dit certificaat moet worden uitgegeven voor clientverificatie.  


- Voor alleen voor Exchange ActiveSync-clients moet dit certificaat geschikt e-mailadres van de gebruiker in Exchange online in de naam van de hoofdsom of de waarde RFC822 naam van het veld alternatieve naam voor onderwerp. De waarde RFC822 kaarten Azure Active Directory met het kenmerk proxyadres in de adreslijst.  



### <a name="office-mobile-applications-support"></a>Ondersteuning voor mobiele toepassingen van Office 

| Apps                      | Ondersteuning      |
| ---                       | ---          |
| Word / Excel / PowerPoint | ![Selectievakje][1]  |
| OneNote                   | Binnenkort beschikbaar  |
| OneDrive                  | ![Selectievakje][1]  |
| Outlook                   | ![Selectievakje][1]  |
| Yammer                    | ![Selectievakje][1]  |
| Skype voor bedrijven        | ![Selectievakje][1]  |


### <a name="requirements"></a>Vereisten  

De apparaatversie van het OS moet Android 5.0 (Lollipop) en hoger. 

Een federatieserver moet worden geconfigureerd.  


Voor Azure Active Directory een clientcertificaat intrekken, moet de ADFS-token de volgende claims hebben:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Het seriële getal van het clientcertificaat) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(De tekenreeks voor de uitgever van de clientcertificaat) 

Azure Active Directory wordt deze claims op de token vernieuwen toegevoegd als ze beschikbaar in de ADFS-token (of een andere SAML-token zijn). Wanneer het token vernieuwen worden gevalideerd moet, wordt deze informatie wordt gebruikt om te controleren van het intrekken. 

Als een goede gewoonte, moet u de ADFS-fout-pagina's met instructies voor het ophalen van het certificaat van een gebruiker bijwerken. 

Zie [de AD FS-aanmelding's aanpassen](https://technet.microsoft.com/library/dn280950.aspx)voor meer informatie.  



### <a name="exchange-activesync-clients-support"></a>Ondersteuning voor Exchange ActiveSync-clients 


Bepaalde Exchange ActiveSync-toepassingen op Android 5.0 (Lollipop) of hoger worden ondersteund. Neem contact op met de ontwikkelaar van uw toepassingen om te bepalen als deze functie wordt ondersteund door uw e-mailtoepassing. 



## <a name="getting-started"></a>Aan de slag 


Als u wilt beginnen, moet u de certificeringsinstanties configureren in Azure Active Directory. Voor elke certificeringsinstantie upload het volgende: 

- De openbare gedeelte van het certificaat, in *.cer* -indeling 

- De internetverbinding URL's waarin de certificaatintrekkingslijsten () zijn opgeslagen
 

Hieronder ziet u het schema voor een certificeringsinstantie: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Als u wilt de gegevens uploaden, kunt u de Azure AD-module via Windows PowerShell.  
Hieronder vindt u voorbeelden voor het toevoegen, verwijderen of wijzigen van een certificeringsinstantie. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Het configureren van uw Azure AD-tenant voor certificaat op gebaseerde verificatie 

1. Start Windows PowerShell met beheerdersbevoegdheden. 

2. Installeer de Azure AD-module. U moet installeren versie [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) of hoger.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Verbinding maken met uw doeltenant: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Een nieuwe certificeringsinstantie toevoegen

1. Verschillende eigenschappen van de certificeringsinstantie instellen en deze toevoegen aan Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Krijg de certificeringsinstanties: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>De lijst certificeringsinstanties ophalen

De certificeringsinstanties die momenteel zijn opgeslagen in Azure Active Directory voor uw tenant ophalen: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Verwijderen van een certificeringsinstantie

1.  De certificeringsinstanties ophalen: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Het certificaat voor de certificeringsinstantie verwijderen: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying een certificeringsinstantie 

1.  De certificeringsinstanties ophalen: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Eigenschappen van op de certificeringsinstantie wijzigen: 

        $c[0].AuthorityType=1 

3. De **certificeringsinstantie**instellen: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Mobiele Office-toepassingen testen  

Verificatie via clientcertificaat op uw mobiele Office-toepassing testen: 

1.  Op uw testapparaat, installeert u een mobiele Office-toepassing (bijvoorbeeld OneDrive) vanuit de Google Play Store.

2.  Controleer of het certificaat van de gebruiker aan uw testapparaat is ingericht. 

3.  Start de toepassing. 

4.  Voer uw gebruikersnaam en kies vervolgens het gebruikerscertificaat die u wilt gebruiken. 

U moet zijn aangemeld. 





## <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync-clienttoepassingen testen

Voor toegang tot Exchange ActiveSync via verificatie via clientcertificaat die zijn gebaseerd, moet een EAS-profiel met het clientcertificaat beschikbaar gemaakt voor toepassingen. Het EAS-profiel moet de volgende gegevens bevatten:

- Het certificaat moet worden gebruikt voor verificatie 

- Het eindpunt EAS moet outlook.office365.com (als deze functie is momenteel alleen in de Exchange online meerdere tenant-omgeving ondersteund)

Een EAS-profiel kan worden geconfigureerd en klik op het apparaat tot en met het gebruik van een MDM zoals Intune of door te handmatig plaatsen van het certificaat in het EAS-profiel op het apparaat is geplaatst.  


### <a name="testing-eas-client-applications-on-android"></a>EAS-clienttoepassingen op android-apparaat testen 

Als u wilt testen verificatie via clientcertificaat met een toepassing op Android 5.0 (Lollipop) of hoger, voert u de volgende stappen uit:  

1. Een EAS-profiel configureren in de toepassing die voldoet aan de bovenstaande voorwaarden.  


2. Wanneer het profiel correct is geconfigureerd, open de toepassing verschijnt en controleer dat e-mail wordt gesynchroniseerd. 




## <a name="revocation"></a>Intrekken

Als u wilt intrekken een clientcertificaat, Azure Active Directory ophaalt van de certificaatintrekkingslijsten (CRL) van de URL's die zijn geüpload, als onderdeel van informatie over certificeringsinstantie en deze in de cache opgeslagen. De laatste publiceren tijdstempel (**Ingangsdatum** eigenschap) in de CRL wordt gebruikt om te zorgen dat de CRL nog geldig is. De CRL wordt regelmatig om in te trekken van toegang tot certificaten die deel van de lijst uitmaken verwezen.

Als een meer direct intrekkingsreferenties is vereist (bijvoorbeeld als een gebruiker een apparaat verliest), kan het Autorisatietoken van de gebruiker worden ongeldig. Als u wilt het Autorisatietoken ongeldig, stelt u het veld **StsRefreshTokenValidFrom** voor deze bepaalde gebruiker via Windows PowerShell. U moet het veld **StsRefreshTokenValidFrom** voor elke gebruiker die u wilt intrekken toegang voor bijwerken.
 
Om ervoor te zorgen dat de intrekking zich blijft voordoen, stelt u de **Datum** van de CRL op een datum na de waarde instellen door **StsRefreshTokenValidFrom** en controleer of het certificaat betrokken bevindt zich in de CRL.
 
Het proces voor het bijwerken van en daardoor de Autorisatietoken door in te stellen van het veld **StsRefreshTokenValidFrom** een overzicht van de volgende stappen uit. 

1. Verbinding maken met beheerdersreferenties met de service MSOL: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  De huidige StsRefreshTokensValidFrom-waarde voor een gebruiker ophalen: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Een nieuwe StsRefreshTokensValidFrom-waarde voor de gebruiker gelijk is aan de huidige tijdstempel configureren: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


De datum die u instelt, moet in de toekomst zijn. Als de datum in de toekomst, worden de eigenschap **StsRefreshTokensValidFrom** is niet ingesteld. Als de datum in de toekomst is, wordt **StsRefreshTokensValidFrom** is ingesteld op de huidige tijd (niet de datum aangegeven met de opdracht Set-MsolUser). 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png