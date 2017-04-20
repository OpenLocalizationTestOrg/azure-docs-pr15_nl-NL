<properties
    pageTitle="Azure AD Connect: Poorten | Microsoft Azure"
    description="Deze pagina is een pagina met technische naslaginformatie voor poorten die nodig zijn te worden gebruikt voor Azure AD Connect"
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
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hybride identiteit vereist poorten en protocollen
Het volgende document is een technische verwijzing informatie te verstrekken over de vereiste poorten en protocollen die vereist zijn voor de uitvoering van een hybride identiteit oplossing. Gebruik van de volgende afbeelding en verwijzen naar de bijbehorende tabel.

![Wat is Azure AD Connect](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabel 1: Azure AD verbinding en On-premises AD
In deze tabel worden de poorten en protocollen die vereist voor de communicatie tussen de Azure AD Connect-server zijn en on-premises AD.

Protocol | Poorten | Beschrijving
--------- | --------- |---------
DNS|53 (TCP/UDP)| DNS-lookups op de bestemming bos.
Kerberos|88 (TCP/UDP)| Kerberos-verificatie de AD-bos.
MS-RPC |135 (TCP/UDP)| Wordt gebruikt tijdens de eerste configuratie van de wizard Azure AD Connect wanneer deze gekoppeld aan de AD-bos.
LDAP|389 (TCP/UDP)| Wordt gebruikt voor het importeren van gegevens uit Active Directory. Gegevens worden gecodeerd met Kerberos-teken & stempel.
LDAP/SSL|636 (TCP/UDP)| Wordt gebruikt voor het importeren van gegevens uit Active Directory. De gegevensoverdracht is ondertekend en versleuteld. Alleen gebruikt als u SSL gebruikt.
RPC |49152 - 65535 (willekeurig hoge RPC Port)(TCP/UDP)| Tijdens de eerste configuratie van Azure AD Connect gebruikt wanneer wordt deze gekoppeld aan de AD-forests. Zie [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)en [KB224196](https://support.microsoft.com/kb/224196) voor meer informatie.

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabel 2: Azure AD verbinding en Azure AD
Deze tabel worden de poorten en protocollen die vereist voor de communicatie tussen de Azure AD Connect-server en Azure AD zijn beschreven.

Protocol |Poorten |Beschrijving
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Wordt gebruikt voor het downloaden van CRL's (certificaatintrekkingslijsten) om te controleren of SSL-certificaten.
HTTPS|443(TCP/UDP)| Gebruikt voor synchronisatie met Azure AD.

Voor een lijst met URL's en IP-adressen die u wilt openen in uw firewall, raadpleegt u [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Tabel 3: Azure AD verbinding en Federatie Servers/WAP
In deze tabel worden de poorten en protocollen die vereist voor de communicatie tussen de Azure AD Connect-server en Federatie/WAP-servers zijn.  

Protocol |Poorten |Beschrijving
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Wordt gebruikt voor het downloaden van CRL's (certificaatintrekkingslijsten) om te controleren of SSL-certificaten.
HTTPS|443(TCP/UDP)| Gebruikt voor synchronisatie met Azure AD.
WinRM|5985| WinRM luisteraar ervan af

## <a name="table-4---wap-and-federation-servers"></a>Tabel 4 - WAP en federatieservers
In deze tabel worden de poorten en protocollen die vereist voor de communicatie tussen de federatieservers en WAP-servers zijn.

Protocol |Poorten |Beschrijving
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Wordt gebruikt voor verificatie.

## <a name="table-5---wap-and-users"></a>Tabel 5: WAP en gebruikers
In deze tabel worden de poorten en protocollen die vereist voor de communicatie tussen gebruikers en de WAP-servers zijn.

Protocol |Poorten |Beschrijving
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Wordt gebruikt voor verificatie van apparaat.
TCP|49443 (TCP)| Wordt gebruikt voor verificatie via clientcertificaat.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabel 6a & 6 ter - status verbinding maken met Azure AD-agent voor (AD FS-synchronisatie) en Azure AD
De volgende tabellen beschrijven de eindpunten, poorten en protocollen die vereist voor de communicatie tussen Azure AD verbinding systeemstatus kunt vinden en Azure AD zijn

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabel 6a - poorten en protocollen voor status verbinding maken met Azure AD-agent voor (AD FS-synchronisatie) en Azure AD
In deze tabel worden de volgende uitgaande poorten en protocollen die vereist voor de communicatie tussen de Azure AD verbinding systeemstatus kunt vinden en Azure AD zijn.  

Protocol |Poorten  |Beschrijving
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Uitgaande
Azure-Service Bus|5671 (TCP/UDP)| Uitgaande

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6 ter - eindpunten voor status verbinding maken met Azure AD-agent voor (AD FS-synchronisatie) en Azure AD
Zie [het gedeelte vereisten voor de systeemstatus verbinding maken met Azure AD-agent](active-directory-aadconnect-health-agent-install.md#requirements)voor een lijst met eindpunten.
