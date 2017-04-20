<properties
    pageTitle="Azure AD Connect: Aan de slag met express instellingen | Microsoft Azure"
    description="Informatie over het downloaden, installeren en voer de installatiewizard voor Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Aan de slag met Azure AD Connect met express-instellingen
Azure AD Connect **Expresinstellingen** wordt gebruikt als er een enkele verzameling topologie en [Wachtwoordsynchronisatie](../active-directory-aadconnectsync-implement-password-synchronization.md) voor verificatie. **Expresinstellingen** is de standaardoptie en wordt gebruikt voor het meest worden geïmplementeerd scenario. U bent slechts een paar korte muisklikken ergens anders om uit te breiden uw on-premises adreslijst in de cloud.

Voordat u begint met het installeren van Azure AD Connect, zorg [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) downloaden en voltooid de oude vereiste stappen in [Azure AD Connect: Hardware en vereisten voor](../active-directory-aadconnect-prerequisites.md).

Als express-instellingen niet overeenkomen met uw topologie, raadpleegt u [Verwante documentatie](#related-documentation) voor andere scenario's.

## <a name="express-installation-of-azure-ad-connect"></a>Installatie van Azure AD Connect Express
Hier ziet u deze stappen in de actie in de sectie [video's](#videos) .

1. Meld u aan als beheerder op de server die u Azure AD Connect wilt op installeren. U moet dit doen op de server die u wilt worden de synchronisatieserver.
2. Ga naar en dubbelklik op **AzureADConnect.msi**.
3. Klik op het scherm Welkom schakelt u het ermee akkoord met de licentievoorwaarden en klikt u op **Doorgaan**.  
4. Klik op het scherm instellingen Express op **express instellingen gebruiken**.  
![Welkom bij Azure AD verbinding maken](./media/active-directory-aadconnect-get-started-express/express.png)
5. Voer de gebruikersnaam en wachtwoord van een globale beheerder voor uw Azure AD op de verbinding maken met Azure AD-scherm. Klik op **volgende**.  
![Verbinding maken met Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) als u een foutbericht weergegeven en problemen met verbinding hebt, raadpleegt u [problemen oplossen met de verbinding oplossen](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Op de verbinding wilt maken naar AD DS-scherm, voert u de gebruikersnaam en wachtwoord voor een enterprise-beheerdersaccount. U kunt het domeinonderdeel in NetBios of FQDN-naam, dat wil zeggen FABRIKAM\administrator of fabrikam.com\administrator invoeren. Klik op **volgende**.  
![Verbinding maken met AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. De pagina [**Azure AD aanmeldingsproblemen configuratie**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) wordt alleen weergegeven als u niet [controleren of uw domeinen](../active-directory-add-domain.md) in de [vereisten](../active-directory-aadconnect-prerequisites.md)is voltooid.
![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Als u deze pagina wordt weergegeven, controleert u elk domein dat is gemarkeerd **Niet toegevoegd** en wordt **Niet gecontroleerd**. Zorg ervoor dat deze domeinen die u gebruikt in Azure AD zijn geverifieerd. Klik op het symbool vernieuwen wanneer u uw domeinen hebt gecontroleerd.
8. Klik op het wilt configureren scherm, klikt u op **installeren**.
    - U kunt ook op de gereed voor het configureren van de pagina, kunt u bijvoorbeeld het selectievakje **het code synchronisatie starten zodra de configuratie is voltooid** deselecteren. Desgewenst kunt u aanvullende instellingen, zoals [filteren](../active-directory-aadconnectsync-configure-filtering.md), moet u dit selectievakje bijvoorbeeld deselecteren. Als u deze optie bijvoorbeeld deselecteren, wordt de wizard configureert synchronisatie maar verlaat de planner uitgeschakeld. Deze wordt niet uitgevoerd totdat u deze handmatig door [de installatiewizard opnieuw inschakelen](../active-directory-aadconnectsync-installation-wizard.md).
    - Als u Exchange in uw lokale Active Directory hebt, hebt u ook een optie voor het inschakelen van [**Exchange hybride implementatie**](https://technet.microsoft.com/library/jj200581.aspx). Schakel deze optie als u van plan bent de Exchange-postvakken zowel in de cloud en on-premises op hetzelfde moment.
![Azure AD Connect configureren](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Wanneer de installatie is voltooid, klikt u op **Afsluiten**.
10. Nadat de installatie is voltooid, afmelden en opnieuw aanmelden voordat u synchronisatie servicebeheer of synchronisatie regel Editor gebruiken.

## <a name="videos"></a>Video 's

Voor een video over het gebruik van de aangepaste installatie, raadpleegt u:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Volgende stappen
Nu dat u Azure AD Connect geïnstalleerd hebt kunt u [controleren of de installatie en licenties toewijzen](../active-directory-aadconnect-whats-next.md).

Meer informatie over deze functies, die zijn ingeschakeld met de installatie: [Automatische upgrade](../active-directory-aadconnect-feature-automatic-upgrade.md), [voorkomen dat per ongeluk wordt verwijderd](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)en [Azure AD verbinding maken](../active-directory-aadconnect-health-sync.md).

Meer informatie over deze algemene onderwerpen: [scheduler en hoe u synchronisatie activeren](../active-directory-aadconnectsync-feature-scheduler.md).

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Gerelateerde documentatie

Onderwerp |  
--------- | ---------
Azure AD Connect-overzicht | [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](../active-directory-aadconnect.md)
Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Upgrade van DirSync | [Een upgrade uitvoert vanuit Azure AD-synchronisatie (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Accounts die worden gebruikt voor de installatie | [Meer informatie over Azure AD Connect-accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md)
