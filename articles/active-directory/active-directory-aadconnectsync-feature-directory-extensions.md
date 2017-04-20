<properties
   pageTitle="Synchronisatie van Azure AD Connect: Directory extensies | Microsoft Azure"
   description="In dit onderwerp worden de functie directory extensies in Azure AD Connect."
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
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Synchronisatie van Azure AD Connect: Directory extensies
Directory-extensies kunt u het schema uitbreiden in Azure AD met uw eigen kenmerken vanuit lokale Active Directory. Deze functie kunt u LOB-apps die door andere kenmerken die u gaat u verder met het beheren van on-premises implementatie maken. Deze kenmerken kunnen worden gebruikt door [Azure AD Graph directory extensies](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) of [Microsoft Graph](https://graph.microsoft.io/). Hier ziet u de kenmerken die beschikbaar met [Azure AD Graph Verkenner](https://graphexplorer.cloudapp.net) en [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectievelijk.

Op dit moment gebruikt geen Office 365-werkbelasting deze kenmerken.

U configureren welke extra kenmerken die u wilt synchroniseren in het pad aangepaste instellingen in de installatiewizard.
![Schema extensie Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) de installatie ziet u de volgende kenmerken, die geldig kandidaten zijn:

- Gebruikers en groepen objecttypen
- Kenmerken met één waarde: tekenreeks, Booleaans, Integer, binair getal
- Met meerdere waarden kenmerken: tekenreeks, een binair getal

De lijst met kenmerken wordt van de cache gemaakt tijdens de installatie van Azure AD Connect gelezen. Als u de Active Directory-schema met extra kenmerken hebt verlengd, het [schema moet worden vernieuwd](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) voordat u deze nieuwe kenmerken zijn zichtbaar.

Een object kan maximaal 100 directory extensies kenmerken hebben. De maximale lengte is 250 tekens. Als een kenmerkwaarde langer is, wordt de waarde afgekapt door de synchronisatie-engine.

Tijdens de installatie van Azure AD Connect is een toepassing geregistreerd waar deze kenmerken zijn beschikbaar. Hier ziet u deze toepassing in de portal van Azure.  
![Schema extensie-App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Deze kenmerken zijn nu beschikbaar via de Graph:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

De kenmerken worden voorafgegaan door extensie\_{AppClientId}\_. De AppClientId heeft hetzelfde resultaat voor alle kenmerken in uw adreslijst Azure AD.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
