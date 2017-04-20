<properties
    pageTitle="Verlenging richtlijnen voor gebruikers van Office 365 en Azure Active Directory-certificaat | Microsoft Azure"
    description="In dit artikel wordt uitgelegd aan Office 365-gebruikers het oplossen van problemen met e-mailberichten die ze een melding over het vernieuwen van een certificaat."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Federatie certificaten vernieuwen voor Office 365 en Azure Active Directory

##<a name="overview"></a>Overzicht

De certificaten gebruikt door AD FS aan te melden beveiligingstokens naar Azure AD moeten overeenkomen wat in Azure AD is geconfigureerd voor succesvolle federatie tussen Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS). Een verkeerde combinaties kan leiden tot verbroken vertrouwen. Azure AD zorgt ervoor dat deze informatie wordt gesynchroniseerd wanneer u AD FS- en Web toepassingsproxy (voor extranet toegang implementeert).

Dit artikel vindt u aanvullende informatie voor het beheren van uw token ondertekenen van certificaten en houd deze synchroon met Azure AD, in de volgende gevallen:

* U niet de Web-toepassingsproxy implementeert en de metagegevens Federatie is dus niet beschikbaar in het extranet.
* U gebruikt de standaard-configuratie van AD FS voor token-ondertekening certificaten.
* U gebruikt een identiteitsprovider van derden.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>De standaardconfiguratie van AD FS voor token-ondertekening van certificaten

De token-ondertekening token decoderen certificaten zijn meestal zelfondertekende certificaten en zijn handig voor één jaar. AD FS bevat standaard een automatische verlenging proces **AutoCertificateRollover**genoemd. Als u AD FS 2.0 of hoger gebruikt, Office 365 en Azure AD automatisch bijgewerkt uw certificaat voordat het verloopt.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Melding van de verlenging van het Office 365-beheerportal of een e-mailbericht

>[AZURE.NOTE] Als u een e-mailbericht of de melding van een portal waarin u wordt gevraagd om te verlengen van uw certificaat voor Office hebt ontvangen, raadpleegt u [beheren wordt gewijzigd in token-ondertekening certificaten](#managecerts) om te controleren als u geen actie te ondernemen nodig hebt. Microsoft is op de hoogte van een mogelijke probleem dat tot meldingen voor vernieuwen van een certificaat dat wordt verzonden leiden kan, zelfs wanneer u geen actie moet worden ondernomen.

Azure AD probeert te controleren van de Federatie-metagegevens en het ondertekenen van certificaten, zoals aangegeven met deze metagegevens token bijwerken. 30 dagen vóór de verlooptijd van het ondertekenen van certificaten, token Azure AD gecontroleerd als nieuwe certificaten beschikbaar zijn als u de metagegevens Federatie polling.

* Als deze kunt succes poll uitvoeren onder de metagegevens Federatie en de nieuwe certificaten ophalen, wordt zonder e-mailmelding of waarschuwing in de Office 365-beheerportal uitgegeven aan de gebruiker.
* Als deze geen het nieuwe token ondertekenen van certificaten kunt ophalen, oplossen ofwel omdat de metagegevens Federatie niet bereikbaar is of automatische certificaat rollover niet is ingeschakeld, Azure AD per e-mail een melding en een waarschuwing in de Office 365-beheerportal.

![Office 365-portal melding](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Als u AD FS, om ervoor te zorgen bedrijfscontinuïteit gebruikt, Controleer of uw servers hebt de volgende updates zodat verificatie-problemen voor bekende problemen niet worden uitgevoerd. Deze vermindert bekende problemen zijn AD FS-proxy server voor deze verlenging en toekomstige verlenging perioden:
>
>Server 2012 R2 - [Windows Server mei 2014 rollup](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 en 2012 - [authenticatie via proxy in Windows Server 2012- of Windows 2008 R2 SP1 mislukt](http://support.microsoft.com/kb/3094446)

## Controleer als de certificaten moeten worden bijgewerkt<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Stap 1: De status AutoCertificateRollover controleren

Klik op de AD FS-server, opent u PowerShell. Controleer of de waarde AutoCertificateRollover is ingesteld op waar.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Als u AD FS 2.0 gebruikt, moet u eerst toevoegen-Pssnapin Microsoft.Adfs.Powershell uitvoeren.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Stap 2: Controleer of AD FS en Azure AD gesynchroniseerd zijn

Klik op de AD FS-server, opent u de Azure AD PowerShell-prompt en verbinding maken met Azure AD.

>[AZURE.NOTE] U kunt Azure AD PowerShell downloaden [hier](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Controleer de certificaten die is geconfigureerd in AD FS en Azure AD eigenschappen voor het opgegeven domein vertrouwen.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Als de vingerafdrukken in zowel de resultaten die overeenkomen met, is uw certificaten zijn gesynchroniseerd met Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Stap 3: Controleren of uw certificaat vervalt

Schakel in de uitvoer van Get-MsolFederationProperty of Get-AdfsCertificate voor de datum onder "Niet na." Als de datum minder dan 30 dagen afwezig is, moet u de actie uitvoeren.

| AutoCertificateRollover | Certificaten synchroon met Azure AD | Federatie metagegevens is openbaar toegankelijk | Geldigheid | Actie |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Ja | Ja | Ja | - | Geen actie nodig. Zie [verlengen token handtekeningcertificaat automatisch](#autorenew). |
| Ja | Nee  | - | Minder dan 15 dagen | Direct verlengen. Zie [verlengen token handtekeningcertificaat handmatig](#manualrenew). |
| Nee | - | - | Minder dan 30 dagen | Direct verlengen. Zie [verlengen token handtekeningcertificaat handmatig](#manualrenew). |

\[-] Maakt niet uit.

## De token handtekeningcertificaat automatisch vernieuwen (aanbevolen)<a name="autorenew"></a>

U hoeft niet alle handmatige stappen moet uitvoeren als beide van de volgende voorwaarden is voldaan:
- U hebt de toepassingsproxy Web, die uit het extranet toegang in de metagegevens Federatie inschakelen kunt geïmplementeerd.
- Gebruikt u de standaard-configuratie van AD FS (AutoCertificateRollover is ingeschakeld).

Controleer het volgende om te bevestigen dat het certificaat automatisch kan worden bijgewerkt.

**1. de AD FS-eigenschap AutoCertificateRollover moet zijn ingesteld op waar.** Hiermee wordt aangeduid dat de AD FS automatisch gegenereerd nieuwe token-ondertekening en token decoderen certificaten, voordat de oude die verloopt.

**2. de metagegevens van de AD FS-federatie is openbaar toegankelijk.** Controleer of de metagegevens van uw Federatie openbaar toegankelijk is door te gaan naar de volgende URL vanaf een computer op openbare internet (buiten het bedrijfsnetwerk):


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

waar `(your_FS_name) `wordt vervangen door de service-hostnaam van Federatie, uw organisatie, zoals fs.contoso.com gebruikt.  Als u nog om te controleren of beide van deze instellingen succes, er geen te doen.  

Voorbeeld: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## De token handtekeningcertificaat handmatig vernieuwen<a name="manualrenew"></a>

U kunt de token ondertekenen van certificaten handmatig vernieuwen. Bijvoorbeeld de volgende scenario's werken mogelijk beter voor het handmatig vernieuwen:
* Token ondertekenen van certificaten zijn niet zelfondertekende certificaten. De meest voorkomende reden hiervoor is dat de AD FS-certificaten die zijn geregistreerd bij een organisatie-certificeringsinstantie voor het beheer van uw organisatie.
* Beveiliging van het netwerk is niet toegestaan voor de metagegevens Federatie openbaar beschikbaar is.

In deze situaties telkens wanneer u het ondertekenen van certificaten, token bijwerken moet u ook bijwerken uw Office 365-domein met behulp van de PowerShell-opdracht, Update-MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Stap 1: Zorg ervoor dat AD FS nieuwe token ondertekenen van certificaten

**Niet-standaard configuratie**

Als u een niet-standaard de configuratie van AD FS (waar **AutoCertificateRollover** is ingesteld op **Onwaar**) gebruikt, gebruikt u waarschijnlijk aangepaste certificaten (niet zelfondertekend). Zie [richtlijnen voor klanten die geen gebruikmaakt van AD FS zelf-ondertekend certificaten](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)voor meer informatie over het vernieuwen van de AD FS-beveiligingstoken ondertekenen van certificaten.

**Federatie metagegevens is niet openbaar**

Aan de andere kant als **AutoCertificateRollover** is ingesteld op **waar**, maar de metagegevens van uw Federatie niet toegankelijk voor het publiek is, zorg er eerst dat nieuwe token-ondertekend certificaten zijn gegenereerd door AD FS. Bevestig er nieuwe token ondertekenen van certificaten door de volgende stappen:

1. Controleer of er verschijnt een bericht dat u bent aangemeld bij de primaire AD FS-server.
2. Controleer de huidige ondertekend certificaten in AD FS door een PowerShell-opdrachtvenster te openen en de volgende opdracht uit te voeren:

    PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

    >[AZURE.NOTE] Als u AD FS 2.0 gebruikt, moet u eerst toevoegen-Pssnapin Microsoft.Adfs.Powershell uitvoeren.

3. Bekijk de uitvoer van de opdracht aan alle certificaten weergegeven. Als u AD FS heeft een nieuw certificaat gegenereerd, ziet u twee certificaten in de uitvoer: een waarvan de waarde **IsPrimary** is **True** en de datum **niet na** is binnen 5 dagen en een waarvoor **IsPrimary** is **Onwaar** en **niet na** is over een jaar in de toekomst.

4. Als u alleen een certificaat zien en de datum **niet na** zich binnen 5 dagen, moet u een nieuw certificaat genereren.

5. Als u wilt een nieuw certificaat genereren, voer de volgende opdracht opdrachtprompt PowerShell: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Controleer of het bijwerken door de volgende opdracht opnieuw uit te voeren: PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

Twee certificaten nu moeten worden weergegeven, waaronder een datum **niet na** van ongeveer één jaar in de toekomst heeft en waarvan de waarde **IsPrimary** **Onwaar**is.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Stap 2: De nieuwe token ondertekenen van certificaten voor de Office 365-vertrouwen bijwerken

Office 365 met de nieuwe token ondertekenen van certificaten moet worden gebruikt voor het Vertrouwenscentrum, als volgt bijwerken.

1.  Open de Microsoft Azure Active Directory-Module voor Windows PowerShell.
2.  $Cred uitvoeren = Get-referentie. Wanneer deze cmdlet u om referenties vraagt, typt u uw accountreferenties cloud service-beheerder.
3.  Verbinding maken met MsolService uitvoeren – $cred referenties. Deze cmdlet maakt u verbinding met de cloudservice. Voordat u een van de aanvullende cmdlets geïnstalleerd door het hulpprogramma is voor het maken van een context die u maakt verbinding met de cloudservice vereist.
4.  Als u deze opdrachten op een computer die niet de AD FS primaire federatieserver worden uitgevoerd, voert u Set-MSOLAdfscontext-Computer <AD FS primary server>, waarbij <AD FS primary server> is de interne FQDN-naam van de primaire AD FS-server. Deze cmdlet Hiermee maakt u een context die u maakt verbinding met AD FS.
5.  Update-MSOLFederatedDomain – domeinnaam uitvoeren <domain>. Deze cmdlet instellingen van AD FS in de cloudservice worden bijgewerkt en de vertrouwensrelatie tussen de twee configureert.


>[AZURE.NOTE] Als u nodig hebt voor de ondersteuning van meerdere domeinen op het hoogste niveau, zoals contoso.com en fabrikam.com, moet u de schakeloptie **SupportMultipleDomain** gebruiken met een cmdlets. Zie [ondersteuning voor meerdere domeinen van het bovenste niveau](active-directory-aadconnect-multiple-domains.md)voor meer informatie.

## Azure AD vertrouwen herstellen met behulp van Azure AD Connect<a name="connectrenew"></a>

Als u uw AD FS-farm en Azure AD vertrouwen met behulp van Azure AD Connect hebt geconfigureerd, kunt u Azure AD Connect gebruiken om te bepalen als u niets te doen voor het ondertekenen van certificaten token nodig hebt. Als u de certificaten vernieuwen moet, kunt u Azure AD Connect kunt doen.

Zie [herstellen van het Vertrouwenscentrum](./active-directory-aadconnect-federation-management.md#repairing-the-trust)voor meer informatie.
