<properties
    pageTitle="Synchronisatie van Azure AD Connect: het beheren van de Azure AD-serviceaccount | Microsoft Azure"
    description="In dit onderwerp documenten het herstellen van de Azure AD-serviceaccount."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, hoe u het wachtwoord voor het synchroniseren van Azure AD Connect Connector service-account opnieuw instellen"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Synchronisatie van Azure AD Connect: het beheren van de Azure AD-serviceaccount
De serviceaccount gebruikt door de Azure AD-Connector moet zijn gratis service. Als u de referenties opnieuw instellen moet, klikt u vervolgens in dit onderwerp is bedoeld voor u. Bijvoorbeeld als een globale beheerder per ongeluk heeft het wachtwoord opnieuw instellen voor de serviceaccount via PowerShell.

## <a name="reset-the-credentials"></a>De referenties opnieuw instellen
Als de serviceaccount die is gedefinieerd op de verbindingslijn van Azure AD geen contact met Azure AD door verificatieproblemen, kunt u het wachtwoord kunt herstellen.

1. Meld u aan bij de server Azure AD Connect-synchronisatie en PowerShell starten.
2. Uitvoeren `Add-ADSyncAADServiceAccount`.  
![PowerShell-cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD globale beheerdersreferenties opgeven.

Deze cmdlet Hiermee stelt u het wachtwoord voor de serviceaccount opnieuw en in Azure AD en in de synchronisatie-engine bijwerken.

## <a name="known-issues-these-steps-can-solve"></a>Bekende problemen die kunnen worden opgelost door deze stappen
In dit gedeelte is een lijst met fouten gemeld door klanten die zijn opgelost door een referenties opnieuw instellen op de Azure AD-serviceaccount.

-----------
Gebeurtenis 6900  
De server is een onverwachte fout opgetreden tijdens het verwerken van de melding van een wachtwoord wijzigen:  
AADSTS70002: Fout valideren referenties. AADSTS50054: Oude wachtwoord wordt gebruikt voor verificatie.

----------
Gebeurtenis 659  
Fout bij het ophalen van de configuratie van wachtwoord beleid synchroniseren. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fout valideren referenties. AADSTS50054: Oude wachtwoord wordt gebruikt voor verificatie.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
