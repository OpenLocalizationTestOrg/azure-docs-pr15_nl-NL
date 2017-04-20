<properties
   pageTitle="Connector Release versiegeschiedenis | Microsoft Azure"
   description="In dit onderwerp ziet alle versies van de verbindingslijnen voor Forefront identiteit Manager (FIM) en Microsoft identiteit Manager (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Connector Release versiegeschiedenis
De verbindingslijnen voor Forefront identiteit Manager (FIM) en Microsoft identiteit Manager (MIM) worden regelmatig bijgewerkt.

>[AZURE.NOTE]
In dit onderwerp is alleen op FIM en MIM. Deze verbindingslijnen worden niet ondersteund op Azure AD Connect.

In dit onderwerp een lijst met alle versies van de verbindingslijnen die zijn vrijgegeven.

Verwante koppelingen:

- [Download de meest recente verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=717495)
- [Algemene LDAP-Connector](active-directory-aadconnectsync-connector-genericldap.md) naslagmateriaal
- [Algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md) naslagmateriaal
- [Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) naslagmateriaal
- [PowerShell verbindingslijn](active-directory-aadconnectsync-connector-powershell.md) naslagmateriaal
- [Lotus Domino-Connector](active-directory-aadconnectsync-connector-domino.md) naslagmateriaal

## <a name="111170"></a>1.1.117.0
Uitgebracht: 2016 maart

**Nieuwe verbindingslijn**  
Eerste versie van de [Algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md).

**Nieuwe functies:**

- Algemene LDAP-Connector:
    - Ondersteuning voor delta importeren met Isode toegevoegd.
- Connector van Web Services:
    - De csEntryChangeResult en setImportErrorCode activiteit toe te staan dat object fouten moeten worden geretourneerd terug naar de synchronisatie-engine bijgewerkt.
    - De SAP6 en SAP6User sjablonen voor het gebruik van de nieuwe functionaliteit voor het niveau fout object bijgewerkt.
- Lotus Domino-Connector:
    - Voor exporteren moet u één certifier per adresboek. U kunt nu hetzelfde wachtwoord voor alle certifiers gebruiken om het beheer te vereenvoudigen.

**Vaste problemen:**

- Algemene LDAP-Connector:
    - Voor IBM Tivoli DS, sommige kenmerken verwijzing zijn niet correct is gedetecteerd.
    - Voor openen LDAP tijdens het importeren van een delta zijn whitespaces aan het begin en einde van tekenreeksen afgekapt.
    - Een exporteren die een object tussen organisatie-eenheden/containers en op hetzelfde moment verplaatst gewijzigd voor Novell en NetIQ, het object is mislukt.
- Connector van Web Services:
    - Als de webservice meerdere eindpunten voor dezelfde binding hebt, hebt klikt u vervolgens de verbindingslijn niet correct kennismaken met deze eindpunten.
- Lotus Domino-Connector:
    - Een uitvoer van het kenmerk volledige naam met een database e-mail in werkt niet.
    - Een exporteren die zowel toegevoegd als lid uit een groep verwijderen wordt alleen de toegevoegde leden geëxporteerd.
    - Als een Document notities is ongeldig (het kenmerk isValid ingesteld op ONWAAR), is de verbindingslijn werkt niet.

## <a name="older-releases"></a>Oudere versies
Voordat u maart 2016, zijn de verbindingslijnen uitgebracht als ondersteuningsonderwerpen.

**Algemene LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 September
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, maart 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, januari 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, September 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, maart 2014

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, September 2014

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, September 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 September
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, maart 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, augustus 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, februari 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, oktober 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, augustus 2013

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
