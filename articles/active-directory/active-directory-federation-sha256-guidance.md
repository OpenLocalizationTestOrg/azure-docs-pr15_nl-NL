<properties
    pageTitle="Wijziging handtekening hashalgoritme voor Office 365 beantwoorden partijen vertrouwen | Microsoft Azure"
    description="Deze pagina bevat richtlijnen voor het wijzigen van SHA algoritme voor Federatie vertrouwen met Office 365"
    keywords="SHA1, SHA256, O365, Federatie, aadconnect, adfs, ad fs, sha wijzigen, Federatie vertrouwen, te vertrouwen partijen vertrouwen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Handtekening hashalgoritme wijzigen voor Office 365 beantwoorden van derden vertrouwen

## <a name="overview"></a>Overzicht

Azure Active Directory Federation Services (AD FS) ondertekent eigen tokens met Microsoft Azure Active Directory om ervoor te zorgen dat ze kunnen niet worden gewijzigd. Deze handtekening kan worden gebaseerd op SHA1 of SHA256. Azure Active Directory ondersteunt nu tokens die zijn aangemeld met een algoritme SHA256 en het is raadzaam de algoritme van de token-ondertekening naar SHA256 instellen voor het hoogste niveau van beveiliging. In dit artikel worden de stappen beschreven die nodig zijn om de algoritme van de token-ondertekening in te stellen de veiliger SHA256 niveau.

## <a name="change-the-token-signing-algorithm"></a>Wijzigen van het algoritme token-ondertekening

Nadat u de handtekeningalgoritme van de met een van de onderstaande twee processen hebt ingesteld, wordt ondertekend met AD FS de tokens voor Office 365 te vertrouwen partijen vertrouwen met SHA256. U hoeft te maken van extra configuratiewijzigingen aanbrengt en deze wijziging heeft geen invloed op de mogelijkheid voor toegang tot Office 365 of andere Azure AD-toepassingen.

### <a name="ad-fs-management-console"></a>AD FS-beheerconsole

1. Open de AD FS-beheerconsole op de primaire AD FS-server.
2. Vouw het knooppunt AD FS uit en klik op **Partijen vertrouwensrelaties te vertrouwen**.
3. Met de rechtermuisknop op uw Office 365/Azure gebruikmakende partij vertrouwen en selecteer **Eigenschappen**.
4. Selecteer het tabblad **Geavanceerd** en selecteer het beveiligde-algoritme SHA256.
5. Klik op **OK**.

![SHA256 ondertekend algoritme--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell-cmdlets

1. Op een AD FS-server, opent u PowerShell onder beheerdersbevoegdheden.
2. Stel het beveiligde-algoritme met behulp van de cmdlet **Set-AdfsRelyingPartyTrust** .

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Ook lezen

* [Office 365 vertrouwen met Azure AD Connect herstellen](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
